---
layout: post
title: "win10安装和使用Hadoop 3.2"
categories: [Hadoop]
description: ""
permalink: /windows-install-hadoop/
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}


## 安装Java

Hadoop基于Java，需要先安装Java。Java 8是推荐版本，Java 11也可以使用。

```sh
apt update &&apt upgrade
apt install openjdk-11-jdk
```
## 创建专门用户并配置ssh

为Hadoop专门创建用户：
```sh
sudo useradd -m hadoop -s /bin/bash
sudo passwd hadoop
sudo adduser hadoop sudo
```

因为hadoop需要使用ssh,因此推荐使用密钥免认证ssh:
```sh
su - hadoop 
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```
## 下载Hadoop

**不要切换用户，继续用新创建的hadoop用户**


Hadoop 官网：[Apache  Hadoop releases list](http://hadoop.apache.org/releases.html)

当前最新版本为3.2.1，下载二进制包：

```sh
wget https://downloads.apache.org/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz
tar xvzf hadoop-*.tar.gz
mv hadoop-3.2.1 hadoop
```

## 配置Hadoop

### 编辑环境变量

修改`~/.bashrc`，最后添加：
```js
export HADOOP_HOME=/home/hadoop/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
```
生效：
```sh
source ~/.bashrc
```
设置`JAVA_HOME `环境变量：
```sh
nano ~/hadoop/etc/hadoop/hadoop-env.sh 
```
```sh
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```

### 设置配置文件

设置配置文件：
```sh
cd $HADOOP_HOME/etc/hadoop
nano core-site.xml
```
```xml
<configuration>
<property>
  <name>fs.default.name</name>
    <value>hdfs://localhost:9000</value>
</property>
</configuration>
```

**********

```js
nano hdfs-site.xml
```
```xml
<configuration>
<property>
 <name>dfs.replication</name>
 <value>1</value>
</property>

<property>
  <name>dfs.name.dir</name>
    <value>file:///home/hadoop/hadoopdata/hdfs/namenode</value>
</property>

<property>
  <name>dfs.data.dir</name>
    <value>file:///home/hadoop/hadoopdata/hdfs/datanode</value>
</property>
</configuration>
```

***************

```js
nano mapred-site.xml
```
```xml
<configuration>
 <property>
  <name>mapreduce.framework.name</name>
   <value>yarn</value>
 </property>
</configuration>
```
********
```js
nano yarn-site.xml
```
```xml
<configuration>
 <property>
  <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
 </property>
</configuration>
```

**********

### 格式化 namenode 
```sh
cd ~
hdfs namenode -format
```

## 启动Hadoop单机伪集群

```sh
cd $HADOOP_HOME/sbin
./start-dfs.sh 
./start-yarn.sh 
```

## 查看集群状态
```sh
curl 127.0.0.1:9870
```
```html
<!--
   Licensed to the Apache Software Foundation (ASF) under one or more
   contributor license agreements.  See the NOTICE file distributed with
   this work for additional information regarding copyright ownership.
   The ASF licenses this file to You under the Apache License, Version 2.0
   (the "License"); you may not use this file except in compliance with
   the License.  You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
-->
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="REFRESH" content="0;url=dfshealth.html" />
<title>Hadoop Administration</title>
</head>
</html>

```




