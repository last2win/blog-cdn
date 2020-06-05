---
layout: post
title: "k8s免安装-使用kubectl部署Pod, Deployment, LoadBalancer"
categories: [k8s]
description: "通过digitalocean实现k8s免安装，使用kubectl部署pod, Deployment, LoadBalancer"
keywords: k8s, kubernetes, 
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}



如果你想要从零开始搭建自己的k8s集群参考我的这篇博客，预计花费时间为1天：[从零开始在ubuntu上安装和使用k8s集群及报错解决](https://zhang0peter.com/2020/01/30/k8s-install-and-use-and-fix-bug/)

自己搭建k8s集群的难点之一是需要3台ubuntu虚拟机，要求电脑至少10G内存：操作系统4G内存，3台虚拟机需要6G内存。

另一个难度是对初学者来说，搭建太复杂了。

如果你不想手动搭建集群，只想体验和使用kubernetes集群，推荐使用`digitalocean`的`kubernetes`集群服务，自动搭建，无需安装。

`digitalocean`的`kubernetes`集群提供3台ubuntu虚拟机(node)，每台1核CPU，2G内存，共30\$一个月，体验一天只要1\$。

**通过我的链接在`digitalocean`注册的新用户，可以获得100美元的2个月使用权，相当于前2个月免费用：[DigitalOcean – sign up](https://m.do.co/c/cd843946e47a)**

创建kubernetes集群后，DO会提醒你使用`kubectl`或者`doctl`操作集群，我推荐`kubectl`这个通用工具。


在本地linux上安装`kubectl`，通过 kubectl 操作 k8s 集群。

```sh
echo "deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo gpg --keyserver keyserver.ubuntu.com --recv-keys BA07F4FB #对安装包进行签名
sudo gpg --export --armor BA07F4FB | sudo apt-key add -
sudo apt-get update
sudo apt install kubectl
```
安装完成后下载yaml配置文件到目录`~/.kube`，然后运行：
```sh
-> # cd ~/.kube && mv k8s-zhang0peter-kubeconfig.yaml config
-> # kubectl get nodes                                          
NAME                  STATUS   ROLES    AGE    VERSION
pool-7wa24lnka-v3sf   Ready    <none>   11m    v1.16.2
pool-7wa24lnka-v3sq   Ready    <none>   11m    v1.16.2
pool-7wa24lnka-v3sy   Ready    <none>   8m8s   v1.16.2
```
可以看到集群的状态是`Ready`

部署前先创建命名空间，防止污染：
```sh
-> # kubectl create namespace flask-test
namespace/flask-test created 
-> # kubectl get namespace
NAME              STATUS   AGE
default           Active   169m
flask             Active   30m
flask-test        Active   65s
kube-node-lease   Active   169m
kube-public       Active   169m
kube-system       Active   169m

```
## 部署单个 pod
编辑`flask-pod.yaml`文件如下：
```yaml
apiVersion: v1
kind: Pod
metadata:
        name: flask-pod
        labels:
           app: flask-helloworld
spec:
        containers:
        - name:  flask
          image: registry.cn-hangzhou.aliyuncs.com/zhang0peter/flask:v0
          ports:
          - containerPort: 5000
```
部署应用：
```sh
-> # kubectl apply -f flask-pod.yaml -n flask-test
pod/flask-pod created
-> # kubectl get pod -n flask-test
NAME        READY   STATUS    RESTARTS   AGE
flask-pod   1/1     Running   0          12s

```
转发端口并访问：
```sh
-> # kubectl port-forward pods/flask-pod 5000:5000 -n flask-test
Forwarding from 127.0.0.1:5000 -> 5000
Handling connection for 5000

-> %  curl http://127.0.0.1:5000
hello world!
```
删除 Pod:
```sh
-> # kubectl delete pod flask-pod -n flask-test
pod "flask-pod" deleted
```
## 部署 Deployment
编写`flask-deployment.yaml`文件
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: flask-dep
    labels:
        app: flask-helloworld
spec:
    replicas: 2
    selector:
        matchLabels:
          app: flask-helloworld
    template:
        metadata:
          labels:
            app: flask-helloworld
        spec:
            containers:
            - name:  flask
              image: 'registry.cn-hangzhou.aliyuncs.com/zhang0peter/flask:v0'
              ports:
              - containerPort: 5000
```

部署 Deployment：
```sh
-> # kubectl apply -f flask-deployment.yaml -n flask-test
deployment.apps/flask-dep created
-> # kubectl get deploy -n flask-test
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
flask-dep   2/2     2            2           12s
-> # kubectl get pod -n flask-test
NAME                         READY   STATUS    RESTARTS   AGE
flask-dep-56bcc4b6c5-44gkv   1/1     Running   0          25s
flask-dep-56bcc4b6c5-kkkvl   1/1     Running   0          25s
```
转发端口并访问：
```sh
-> # kubectl port-forward deployment/flask-dep 5000:5000 -n flask-test
Forwarding from 127.0.0.1:5000 -> 5000
Handling connection for 5000

-> %  curl http://127.0.0.1:5000
hello world!
```
不要删除Deployment，后面还要用。

## 部署负载均衡应用
编写`flask-service.yaml`：
```yaml
apiVersion: v1
kind: Service
metadata:
  name: flask-svc
  labels:
    app: flask-helloworld
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 5000
    protocol: TCP
  selector:
    app: flask-helloworld
```

部署 LoadBalancer 负载均衡：
```sh
-> # kubectl apply -f flask-service.yaml -n flask-test
service/flask-svc created
-> # kubectl get service -n flask-test
NAME        TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
flask-svc   LoadBalancer   10.245.139.6   <pending>     80:32349/TCP   9s
```
等待约5分钟，负载均衡实现，对外暴露端口:
```sh
-> # kubectl get service -n flask-test   
NAME        TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE
flask-svc   LoadBalancer   10.245.139.6   139.59.194.75   80:32349/TCP   4m23s
-> # curl 139.59.194.75
hello world!            
```

删除 service
```sh
-> # kubectl delete service flask-svc -n flask-test   
service "flask-svc" deleted
```




部署结束。

参考：

- [A DigitalOcean Workshop: Get Started with Containers and Kubernetes - YouTube](https://www.youtube.com/watch?v=7WOgYfZgSf0&feature=youtu.be)   
- [Getting Started with Containers and Kubernetes: A DigitalOcean Workshop Kit  DigitalOcean](https://www.digitalocean.com/community/meetup_kits/getting-started-with-containers-and-kubernetes-a-digitalocean-workshop-kit)

