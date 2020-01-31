---
layout: post
title: "从零开始在ubuntu上安装和使用k8s集群及报错解决"
categories: [k8s]
description: "安装docker,配置更新源，安装kubeadm，配置flannel网络，配置集群"
keywords: kubernetes, ubuntu
toc: true
---




此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}
这几天在学习K8S的安装和使用，在此记录一下

此文参考了视频教程：[两小时Kubernetes(K8S)从懵圈到熟练——大型分布式集群环境捷径部署搭建_哔哩哔哩 (゜-゜)つロ 干杯~-bilibili](https://www.bilibili.com/video/av57580105)

报错解决在文章最后

## 安装docker
先安装docker:
```sh
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository "deb https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
apt update && apt install docker-ce
docker run hello-world
```
## 安装kubernetes
docker成功运行后配置k8s的更新源，推荐阿里云:
```sh
echo "deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo gpg --keyserver keyserver.ubuntu.com --recv-keys BA07F4FB #对安装包进行签名
sudo gpg --export --armor BA07F4FB | sudo apt-key add -
sudo apt-get update
```
关闭虚拟内存
```sh
sudo swapoff -a #暂时关闭
nano /etc/fstab #永久关闭，注释掉swap那一行，推荐永久关闭
```
安装最新版的k8s：
```sh
apt-get install kubelet kubeadm kubectl kubernetes-cni
```
其中`kubeadm`用于初始化环境，`kubectl`用于操作`kubelet`。
设置开机启动：
```sh
sudo systemctl enable kubelet && systemctl start kubelet
```

查看`kubectl`版本：
```sh
root@ubuntu:/home/ubuntu# kubectl version
Client Version: version.Info{Major:"1", Minor:"17", GitVersion:"v1.17.2", GitCommit:"59603c6e503c87169aea6106f57b9f242f64df89", GitTreeState:"clean", BuildDate:"2020-01-18T23:30:10Z", GoVersion:"go1.13.5", Compiler:"gc", Platform:"linux/amd64"}
The connection to the server localhost:8080 was refused - did you specify the right host or port?
```
## 配置k8s集群
刚刚已经装好一台虚拟机的k8s，现在要配置2台额外的虚拟机，总共3台，形成k8s集群。

推荐的做法是直接使用`vmware`自带的克隆功能，这样可以免去重装的烦恼。

共3台机器，分别为 master, node1, node2.

### 配置虚拟机网络



在`/etc/hostname`中配置主节点为master,node1为 node1,node2为 node2

配置每台机器的`/etc/netplan/50-cloud-init.yaml`，把DHCP的IP改为固定IP：
```sh
network:
    ethernets:
        ens33:
            addresses: [192.168.32.132/24]
            dhcp4: false
            gateway4: 192.168.32.2
            nameservers:
                addresses: [192.168.32.2]
            optional: true
    version: 2
```
修改`/etc/hosts`
```sh
192.168.32.132 master
192.168.32.133 node1
192.168.32.134 node2
```
重启机器后能互相ping表示配置成功：
```sh
ubuntu@node2:~$ ping master
PING master (192.168.32.132) 56(84) bytes of data.
64 bytes from master (192.168.32.132): icmp_seq=1 ttl=64 time=0.837 ms
64 bytes from master (192.168.32.132): icmp_seq=2 ttl=64 time=0.358 ms
```
### 配置Master节点的k8s，并使用 kubeadm 拉取镜像
使用`kubeadm init `进行初始化操作：
```sh
#修改IP地址为master节点的IP地址并配置pod地址
kubeadm init \
--apiserver-advertise-address=192.168.32.132 \
--image-repository registry.aliyuncs.com/google_containers  \
--pod-network-cidr=10.244.0.0/16 
```
```sh
root@master:/home/ubuntu# kubeadm init \
> --apiserver-advertise-address=192.168.32.132 \
> --image-repository registry.aliyuncs.com/google_containers  \
> --pod-network-cidr=10.244.0.0/16 
W0131 07:58:41.470780    4096 version.go:101] could not fetch a Kubernetes version from the internet: unable to get URL "https://dl.k8s.io/release/stable-1.txt": Get https://dl.k8s.io/release/stable-1.txt: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
W0131 07:58:41.470831    4096 version.go:102] falling back to the local client version: v1.17.2
W0131 07:58:41.470908    4096 validation.go:28] Cannot validate kube-proxy config - no validator is available
W0131 07:58:41.470912    4096 validation.go:28] Cannot validate kubelet config - no validator is available
[init] Using Kubernetes version: v1.17.2
[preflight] Running pre-flight checks
	[WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
........................
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.32.132:6443 --token uf5mqk.bssr36md2y6b7w7g \
    --discovery-token-ca-cert-hash sha256:fa6e8c828a4480baf8dba2331bcaad4d30ae593024e0a56258cf22fdde3f897a
```

```sh
ubuntu@master:~/k8s$   mkdir -p $HOME/.kube
ubuntu@master:~/k8s$   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
ubuntu@master:~/k8s$   sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
创建系统服务并启动
```shell
# 启动kubelet 设置为开机自启动
sudo systemctl enable kubelet
# 启动k8s服务程序
sudo systemctl start kubelet
```
查看启动状况：
```sh
ubuntu@master:~/k8s$ kubectl get nodes
NAME     STATUS     ROLES    AGE     VERSION
master   NotReady   master   7m53s   v1.17.2
ubuntu@master:~/k8s$ kubectl get cs
NAME                 STATUS    MESSAGE             ERROR
controller-manager   Healthy   ok                  
scheduler            Healthy   ok                  
etcd-0               Healthy   {"health":"true"}  
```
现在只有一个master节点。
### 配置内部通信 flannel 网络(master和node都要配)
先配置内部通信 flannel 网络：
```sh
wget https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
确保kubeadm.conf中的podsubnet的地址和kube-flannel.yml中的网络配置一样

加载配置文件：
```sh
ubuntu@master:~/k8s$ kubectl apply -f kube-flannel.yml 
podsecuritypolicy.policy/psp.flannel.unprivileged created
clusterrole.rbac.authorization.k8s.io/flannel configured
clusterrolebinding.rbac.authorization.k8s.io/flannel unchanged
serviceaccount/flannel unchanged
configmap/kube-flannel-cfg configured
daemonset.apps/kube-flannel-ds-amd64 created
daemonset.apps/kube-flannel-ds-arm64 created
daemonset.apps/kube-flannel-ds-arm created
daemonset.apps/kube-flannel-ds-ppc64le created
daemonset.apps/kube-flannel-ds-s390x created
```
状态变为ready:
```sh
ubuntu@master:~/k8s$ kubectl get nodes
NAME     STATUS   ROLES    AGE   VERSION
master   Ready    master   39m   v1.17.2
```
如果没变为ready应该是镜像下载失败，手动下载，镜像版本由当前flannel版本决定。
```sh
docker pull quay-mirror.qiniu.com/coreos/flannel:v0.11.0-amd64
docker tag quay-mirror.qiniu.com/coreos/flannel:v0.11.0-amd64 quay.io/coreos/flannel:v0.11.0-amd64
```
### 配置 node节点
```sh
sudo systemctl enable kubelet
sudo systemctl start kubelet
```
拷贝配置文件到每个node:
```sh
scp /etc/kubernetes/admin.conf ubuntu@node1:/home/ubuntu/
scp /etc/kubernetes/admin.conf ubuntu@node2:/home/ubuntu/
```
配置并加入节点，加入中的哈希值是之前配置时生成的。
```sh
mkdir -p $HOME/.kube
sudo cp -i $HOME/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
kubeadm join 192.168.32.132:6443 --token uf5mqk.bssr36md2y6b7w7g \
    --discovery-token-ca-cert-hash sha256:fa6e8c828a4480baf8dba2331bcaad4d30ae593024e0a56258cf22fdde3f897a
```
查看node是否已经加入到k8s集群中(需要等一段时间才能ready):
```sh
ubuntu@master:~$ kubectl get nodes
NAME     STATUS     ROLES    AGE     VERSION
master   Ready      master   5h8m    v1.17.2
node1    Ready      <none>   3h21m   v1.17.2
node2    Ready      <none>   3h20m   v1.17.2
```
出现报错参考后面的报错解决。

## 部署应用

编写配置文件`mysql-rc.yaml`：
```yaml
apiVersion: v1
kind: ReplicationController                           
metadata:
  name: mysql                                          
spec:
  replicas: 1                                          #Pod副本的期待数量
  selector:
    app: mysql                                         #符合目标的Pod拥有此标签
  template:                                            #根据此模板创建Pod的副本（实例）
    metadata:
      labels:
        app: mysql                                     #Pod副本拥有的标签，对应RC的Selector
    spec:
      containers:                                      #Pod内容器的定义部分
      - name: mysql                                    #容器的名称
        image: hub.c.163.com/library/mysql              #容器对应的Docker image
        ports: 
        - containerPort: 3306                          #容器应用监听的端口号
        env:                                           #注入容器内的环境变量
        - name: MYSQL_ROOT_PASSWORD 
          value: "123456"
```
加载文件到集群中，等待几分钟等待docker下载完成。
```sh
ubuntu@master:~/k8s$ kubectl create -f mysql-rc.yaml
replicationcontroller/mysql created
ubuntu@master:~/k8s$ kubectl get pods
NAME          READY   STATUS              RESTARTS   AGE
mysql-chv9n   0/1     ContainerCreating   0          29s
ubuntu@master:~/k8s$ kubectl get pods 
NAME          READY   STATUS    RESTARTS   AGE
mysql-chv9n   1/1     Running   0          5m56s
```

集群创建完毕。



参考：
- [kubernetes安装（国内环境） - 知乎](https://zhuanlan.zhihu.com/p/46341911)
- [两小时Kubernetes(K8S)从懵圈到熟练——大型分布式集群环境捷径部署搭建_哔哩哔哩 (゜-゜)つロ 干杯~-bilibili](https://www.bilibili.com/video/av57580105)
- [Install and Set Up kubectl - Kubernetes](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## 报错解决

之前参考视频配置的时候报错如下：
```sh
ubuntu@master:~/k8s$ kubeadm config images pull --config ./kubeadm.conf
W0130 01:11:49.990838   11959 validation.go:28] Cannot validate kube-proxy config - no validator is available
W0130 01:11:49.991229   11959 validation.go:28] Cannot validate kubelet config - no validator is available
failed to pull image "registry.cn-beijing.aliyuncs.com/imcto/kube-apiserver:v1.17.0": output: Error response from daemon: manifest for registry.cn-beijing.aliyuncs.com/imcto/kube-apiserver:v1.17.0 not found: manifest unknown: manifest unknown
, error: exit status 1
To see the stack trace of this error execute with --v=5 or higher
```
于是我就换了一种安装的方法，具体操作见文章

如果不关闭swap虚拟内存，会报错：
```sh
ubuntu@master:~/k8s$ sudo kubeadm init --config ./kubeadm.conf
[sudo] password for ubuntu: 
W0130 01:32:14.915442   16070 validation.go:28] Cannot validate kube-proxy config - no validator is available
W0130 01:32:14.915742   16070 validation.go:28] Cannot validate kubelet config - no validator is available
[init] Using Kubernetes version: v1.17.0
[preflight] Running pre-flight checks
	[WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/
error execution phase preflight: [preflight] Some fatal errors occurred:
	[ERROR Swap]: running with swap on is not supported. Please disable swap
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher
```
如果使用旧版本的`kube-flannel.yml `会报错，需要下载最新版本：
```sh
ubuntu@master:~/k8s$ kubectl apply -f kube-flannel.yml 
clusterrole.rbac.authorization.k8s.io/flannel created
clusterrolebinding.rbac.authorization.k8s.io/flannel created
serviceaccount/flannel created
configmap/kube-flannel-cfg created
unable to recognize "kube-flannel.yml": no matches for kind "DaemonSet" in version "extensions/v1beta1"
unable to recognize "kube-flannel.yml": no matches for kind "DaemonSet" in version "extensions/v1beta1"
unable to recognize "kube-flannel.yml": no matches for kind "DaemonSet" in version "extensions/v1beta1"
unable to recognize "kube-flannel.yml": no matches for kind "DaemonSet" in version "extensions/v1beta1"
unable to recognize "kube-flannel.yml": no matches for kind "DaemonSet" in version "extensions/v1beta1"

```
如果运行程序的用户不对，会报错：
```sh
root@node2:/home/ubuntu# kubectl get nodes
The connection to the server localhost:8080 was refused - did you specify the right host or port?
ubuntu@node2:~$ kubectl get nodes
NAME     STATUS     ROLES    AGE    VERSION
master   Ready      master   147m   v1.17.2
node1    NotReady   <none>   40m    v1.17.2
node2    NotReady   <none>   39m    v1.17.2
```

### node节点一直NotReady，报错 cni.go:237] Unable to update cni config: no networks found in /etc/cni/net.d

如果k8s的node节点一直是`NotReady`状态，那么需要查看日志：
```sh
ubuntu@node1:~/.kube$ journalctl -f -u kubelet
-- Logs begin at Tue 2020-01-28 11:02:32 UTC. --
Jan 30 04:25:10 node1 kubelet[1893]: W0130 04:25:10.042232    1893 cni.go:237] Unable to update cni config: no networks found in /etc/cni/net.d
Jan 30 04:25:11 node1 kubelet[1893]: E0130 04:25:11.637588    1893 remote_runtime.go:105] RunPodSandbox from runtime service failed: rpc error: code = Unknown desc = failed pulling image "k8s.gcr.io/pause:3.1": Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
Jan 30 04:25:11 node1 kubelet[1893]: E0130 04:25:11.637625    1893 kuberuntime_sandbox.go:68] CreatePodSandbox for pod "kube-proxy-pk22k_kube-system(ad0d231e-e5a5-421d-944d-7f860d1119fa)" failed: rpc error: code = Unknown desc = failed pulling image "k8s.gcr.io/pause:3.1": Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
Jan 30 04:25:11 node1 kubelet[1893]: E0130 04:25:11.637685    1893 kuberuntime_manager.go:729] createPodSandbox for pod "kube-proxy-pk22k_kube-system(ad0d231e-e5a5-421d-944d-7f860d1119fa)" failed: rpc error: code = Unknown desc = failed pulling image "k8s.gcr.io/pause:3.1": Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
Jan 30 04:25:11 node1 kubelet[1893]: E0130 04:25:11.637737    1893 pod_workers.go:191] Error syncing pod ad0d231e-e5a5-421d-944d-7f860d1119fa ("kube-proxy-pk22k_kube-system(ad0d231e-e5a5-421d-944d-7f860d1119fa)"), skipping: failed to "CreatePodSandbox" for "kube-proxy-pk22k_kube-system(ad0d231e-e5a5-421d-944d-7f860d1119fa)" with CreatePodSandboxError: "CreatePodSandbox for pod \"kube-proxy-pk22k_kube-system(ad0d231e-e5a5-421d-944d-7f860d1119fa)\" failed: rpc error: code = Unknown desc = failed pulling image \"k8s.gcr.io/pause:3.1\": Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)"
Jan 30 04:25:12 node1 kubelet[1893]: E0130 04:25:12.608103    1893 aws_credentials.go:77] while getting AWS credentials NoCredentialProviders: no valid providers in chain. Deprecated.
Jan 30 04:25:12 node1 kubelet[1893]:         For verbose messaging see aws.Config.CredentialsChainVerboseErrors
Jan 30 04:25:13 node1 kubelet[1893]: E0130 04:25:13.662938    1893 kubelet.go:2183] Container runtime network not ready: NetworkReady=false reason:NetworkPluginNotReady message:docker: network plugin is not ready: cni config uninitialized
Jan 30 04:25:15 node1 kubelet[1893]: W0130 04:25:15.043972    1893 cni.go:237] Unable to update cni config: no networks found in /etc/cni/net.d
Jan 30 04:25:18 node1 kubelet[1893]: E0130 04:25:18.671967    1893 kubelet.go:2183] Container runtime network not ready: NetworkReady=false reason:NetworkPluginNotReady message:docker: network plugin is not ready: cni config uninitialized
```
看报错就可以解决问题了。

这里显示的报错是镜像没下载，那么就手动下载。


随后又报错如下：
```sh
ubuntu@node1:~$ journalctl -f -u kubelet
-- Logs begin at Tue 2020-01-28 11:02:32 UTC. --
Jan 30 04:32:26 node1 kubelet[1893]: E0130 04:32:26.252152    1893 pod_workers.go:191] Error syncing pod 9e1020f5-06a0-469b-8340-adff61fb2f56 ("kube-flannel-ds-amd64-rcvjv_kube-system(9e1020f5-06a0-469b-8340-adff61fb2f56)"), skipping: failed to "StartContainer" for "install-cni" with ImagePullBackOff: "Back-off pulling image \"quay.io/coreos/flannel:v0.11.0-amd64\""
Jan 30 04:32:30 node1 kubelet[1893]: E0130 04:32:30.115061    1893 kubelet.go:2183] Container runtime network not ready: NetworkReady=false reason:NetworkPluginNotReady message:docker: network plugin is not ready: cni config uninitialized
Jan 30 04:32:30 node1 kubelet[1893]: W0130 04:32:30.149915    1893 cni.go:237] Unable to update cni config: no networks found in /etc/cni/net.d
Jan 30 04:32:35 node1 kubelet[1893]: E0130 04:32:35.125483    1893 kubelet.go:2183] Container runtime network not ready: NetworkReady=false reason:NetworkPluginNotReady message:docker: network plugin is not ready: cni config uninitialized
Jan 30 04:32:35 node1 kubelet[1893]: W0130 04:32:35.150265    1893 cni.go:237] Unable to update cni config: no networks found in /etc/cni/net.d
Jan 30 04:32:39 node1 kubelet[1893]: E0130 04:32:39.251675    1893 pod_workers.go:191] Error syncing pod 9e1020f5-06a0-469b-8340-adff61fb2f56 ("kube-flannel-ds-amd64-rcvjv_kube-system(9e1020f5-06a0-469b-8340-adff61fb2f56)"), skipping: failed to "StartContainer" for "install-cni" with ImagePullBackOff: "Back-off pulling image \"quay.io/coreos/flannel:v0.11.0-amd64\""
Jan 30 04:32:40 node1 kubelet[1893]: E0130 04:32:40.134950    1893 kubelet.go:2183] Container runtime network not ready: NetworkReady=false reason:NetworkPluginNotReady message:docker: network plugin is not ready: cni config uninitialized
Jan 30 04:32:40 node1 kubelet[1893]: W0130 04:32:40.151451    1893 cni.go:237] Unable to update cni config: no networks found in /etc/cni/net.d
Jan 30 04:32:45 node1 kubelet[1893]: E0130 04:32:45.145834    1893 kubelet.go:2183] Container runtime network not ready: NetworkReady=false reason:NetworkPluginNotReady message:docker: network plugin is not ready: cni config uninitialized
Jan 30 04:32:45 node1 kubelet[1893]: W0130 04:32:45.151693    1893 cni.go:237] Unable to update cni config: no networks found in /etc/cni/net.d

```
报错`Unable to update cni config: no networks found in /etc/cni/net.d`这个报错我也看不出来哪里有问题。

随后查看更具体的情况：
```sh
ubuntu@master:~$ kubectl get pods -n kube-system -o wide
NAME                             READY   STATUS     RESTARTS   AGE     IP               NODE     NOMINATED NODE   READINESS GATES
kube-flannel-ds-amd64-gtlwv      1/1     Running    4          4h23m   192.168.32.132   master   <none>           <none>
kube-flannel-ds-amd64-m78z2      0/1     Init:0/1   0          3h13m   192.168.32.134   node2    <none>           <none>
kube-flannel-ds-amd64-rcvjv      1/1     Running    1          3h13m   192.168.32.133   node1    <none>           <none>
ubuntu@master:~$ kubectl --namespace kube-system logs kube-flannel-ds-amd64-m78z2
Error from server (BadRequest): container "kube-flannel" in pod "kube-flannel-ds-amd64-m78z2" is waiting to start: PodInitializing
ubuntu@master:~$ kubectl describe pod kube-flannel-ds-amd64-m78z2 --namespace=kube-system
Name:         kube-flannel-ds-amd64-m78z2
Namespace:    kube-system
............................
Events:
  Type     Reason                  Age                     From               Message
  ----     ------                  ----                    ----               -------
  Normal   Scheduled               <unknown>               default-scheduler  Successfully assigned kube-system/kube-flannel-ds-amd64-m78z2 to node2
  Warning  FailedCreatePodSandBox  3h17m (x22 over 3h27m)  kubelet, node2     Failed to create pod sandbox: rpc error: code = Unknown desc = failed pulling image "k8s.gcr.io/pause:3.1": Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
  Warning  FailedCreatePodSandBox  139m (x63 over 169m)    kubelet, node2     Failed to create pod sandbox: rpc error: code = Unknown desc = failed pulling image "k8s.gcr.io/pause:3.1": Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
  Warning  Failed                  23m (x3 over 26m)       kubelet, node2     Failed to pull image "quay.io/coreos/flannel:v0.11.0-amd64": rpc error: code = Unknown desc = context canceled
  Warning  Failed                  23m (x3 over 26m)       kubelet, node2     Error: ErrImagePull
  Normal   BackOff                 23m (x5 over 26m)       kubelet, node2     Back-off pulling image "quay.io/coreos/flannel:v0.11.0-amd64"
  Warning  Failed                  23m (x5 over 26m)       kubelet, node2     Error: ImagePullBackOff
  Normal   Pulling                 22m (x4 over 30m)       kubelet, node2     Pulling image "quay.io/coreos/flannel:v0.11.0-amd64"
  Normal   SandboxChanged          19m                     kubelet, node2     Pod sandbox changed, it will be killed and re-created.
  Normal   Pulling                 18m                     kubelet, node2     Pulling image "quay.io/coreos/flannel:v0.11.0-amd64"
  Normal   Pulling                 10m                     kubelet, node2     Pulling image "quay.io/coreos/flannel:v0.11.0-amd64"
  Normal   Pulling                 3m4s                    kubelet, node2     Pulling image "quay.io/coreos/flannel:v0.11.0-amd64"
```
可以看出问题在于无法拉镜像：`Pulling image "quay.io/coreos/flannel:v0.11.0-amd64"`，手动拉：
```sh
docker pull quay-mirror.qiniu.com/coreos/flannel:v0.11.0-amd64
docker tag quay-mirror.qiniu.com/coreos/flannel:v0.11.0-amd64 quay.io/coreos/flannel:v0.11.0-amd64
```
问题解决，参考：[k8s 部署问题解决 - 简书](https://www.jianshu.com/p/f53650a85131)



### 拉取k8s需要的镜像
由于官方镜像地址被墙，所以我们需要首先获取所需镜像以及它们的版本。然后从国内镜像站获取。
```sh
ubuntu@master:~/k8s$ kubeadm config images list --config kubeadm.conf
W0130 01:31:26.536909   15911 validation.go:28] Cannot validate kube-proxy config - no validator is available
W0130 01:31:26.536973   15911 validation.go:28] Cannot validate kubelet config - no validator is available
k8s.gcr.io/kube-apiserver:v1.17.0
k8s.gcr.io/kube-controller-manager:v1.17.0
k8s.gcr.io/kube-scheduler:v1.17.0
k8s.gcr.io/kube-proxy:v1.17.0
k8s.gcr.io/pause:3.1
k8s.gcr.io/etcd:3.4.3-0
k8s.gcr.io/coredns:1.6.5
```
```sh
#下载全部当前版本的k8s所关联的镜像
images=(  # 下面的镜像应该去除"k8s.gcr.io/"的前缀，版本换成上面获取到的版本
kube-apiserver:v1.17.0
kube-controller-manager:v1.17.0
kube-scheduler:v1.17.0
kube-proxy:v1.17.0
pause:3.1
etcd:3.4.3-0
coredns:1.6.5
)

for imageName in ${images[@]} ; do
    docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/$imageName
    docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/$imageName k8s.gcr.io/$imageName
    docker rmi registry.cn-hangzhou.aliyuncs.com/google_containers/$imageName
done
```