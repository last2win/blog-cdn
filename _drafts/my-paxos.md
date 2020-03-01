---
layout: post
title: "我的paxos分布式数据库"
categories: [cate1, cate2]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

我的paxos基于腾讯的开源库`PaxosStore`，参考这篇文章：[paxos企业级框架：PaxosStore 编译与报错解决 — zhang0peter的博客](https://zhang0peter.com/2020/02/28/paxos-PaxosStore-phxpaxos/)

推荐使用 linux系统，尤其推荐 ubuntu/debian

编译前的准备工作，安装有用的库：
```sh
apt install zlib1g-dev libssl-dev  libpcap-dev libelf-dev libicu-dev libreadline-dev libtool  libsysfs-dev libgtest-dev \
libsqlite3-dev libbz2-dev libreadline-dev zlib1g-dev liblzma-dev liblzo2-dev   \
libsnappy-dev  liblz4-dev libzstd-dev libgflags-dev  libprotoc-dev \
```
安装`grpc`，`protobuf`和`rocksdb`，毕竟编译安装太慢了：
```sh
apt install libgrpc++-dev protobuf-compiler-grpc  librocksdb-dev libprotobuf-dev 
```















