---
layout: post
title: "用snap在ubuntu上构建 Microk8s，使用kubectl，部署应用"
categories: [k8s]
description: "用snap在ubuntu上构建 Microk8s"
keywords: snap, k8s, ubuntu, Microk8s
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

最近在学习k8s，也就是`Kubernetes`的使用。

在登录ubuntu的终端时出现了`Microk8s`的广告：
```sh
 * Overheard at KubeCon: "microk8s.status just blew my mind".

     https://microk8s.io/docs/commands#microk8s.status
```

于是我就想试试`Microk8s`

GitHub项目：[ubuntu/microk8s: MicroK8s is a small, fast, single-package Kubernetes for developers, IoT and edge.](https://github.com/ubuntu/microk8s)

官网：[MicroK8s - Fast, Light, Upstream Developer Kubernetes](https://microk8s.io/)

## 下载与安装，配置PATH
安装过程参考的官方文档：[Quick start | MicroK8s](https://microk8s.io/docs/)

截止`2020-1-26`，`MicroK8s`最新的版本是`1.17`，需要使用`snap`进行安装，`snap`是原生安装在ubuntu上的，直接运行：
```sh
-> % sudo snap install microk8s --classic
Download snap "microk8s" (1107) from channel "stable"                                                                    0%     1B/s ages!
```
可以看到下载进度很慢，需要一个世纪，但`snap`没有国内的镜像，谷歌搜索`snap ubuntu mirror china`发现并没有镜像。

之前我写过一篇文章：[新一代包管理器：snap介绍和使用：安装，代理，禁用](https://blog.csdn.net/zhangpeterx/article/details/95892242)

解决方法有2个：

1.使用代理进行下载
2.使用`snap download`下载安装包，然后传递文件到需要的机器。

我采用的是下载下来进行安装：
```sh
-> % sudo snap download microk8s          
Fetching snap "microk8s"
Fetching assertions for "microk8s"
Install the snap with:
   snap ack microk8s_1107.assert
   snap install microk8s_1107.snap
```
```sh
-> % sudo snap ack microk8s_1107.assert 
-> % sudo snap install microk8s_1107.snap --classic

Warning: /snap/bin was not found in your $PATH. If you've not restarted your session since you
         installed snapd, try doing that. Please see https://forum.snapcraft.io/t/9469 for more
         details.

microk8s v1.17.0 from Canonical✓ installed
```
安装完成，还需要添加路径，不然会报错：
```sh
root@ubuntu:/home/ubuntu# microk8s.status --wait-ready
Command 'microk8s.status' is available in '/snap/bin/microk8s.status'
The command could not be located because '/snap/bin' is not included in the PATH environment variable.
```
```sh
echo "export PATH=$PATH:/snap/bin" >> ~/.bashrc #bash
source ~/.bashrc
```
```sh
echo "export PATH=$PATH:/snap/bin" >> ~/.zshrc #zsh
source ~/.zshrc
```
## 添加用户组
为了不在使用时需要root用户，建议添加用户组：
```sh
-> % sudo usermod -a -G microk8s $USER
-> % su - $USER
```
##  检查状态并使用kubectl
检查服务状态：
```sh
-> # microk8s.status --wait-ready
microk8s is running
addons:
cilium: disabled
dashboard: disabled
dns: disabled
fluentd: disabled
gpu: disabled
helm: disabled
ingress: disabled
istio: disabled
jaeger: disabled
juju: disabled
knative: disabled
kubeflow: disabled
linkerd: disabled
metallb: disabled
metrics-server: disabled
prometheus: disabled
rbac: disabled
registry: disabled
storage: disabled
```
检查`kubectl`状态：
```sh
-> # microk8s.kubectl get nodes
NAME     STATUS   ROLES    AGE    VERSION
ubuntu   Ready    <none>   114m   v1.17.0
-> # microk8s.kubectl get services
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.152.183.1   <none>        443/TCP   114m
```
## 部署应用
```sh
-> # microk8s.kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
deployment.apps/kubernetes-bootcamp created
-> # microk8s.kubectl get pods
NAME                                   READY   STATUS              RESTARTS   AGE
kubernetes-bootcamp-69fbc6f4cf-ktpcm   0/1     ContainerCreating   0          38s 
-> # microk8s.kubectl get pods
NAME                                   READY   STATUS    RESTARTS   AGE
kubernetes-bootcamp-69fbc6f4cf-ktpcm   1/1     Running   0          3m10s
```
部署过程大约需要几分钟，需要注意的是下载需要网络。

## 停止和启动服务：
```sh
-> # microk8s.stop
Stopped.
-> # microk8s.start
Started.
Enabling pod scheduling
node/ubuntu already uncordoned
```

更多内容，未完待续。