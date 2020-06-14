---
layout: post
title: "win10安装和使用Hadoop 3.2"
categories: [大数据]
description: "安装Java-配置ssh-编辑配置文件-启动Hadoop单机伪集群-查看集群状态"
permalink: /windows-install-hadoop/
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}


我之前已经写了一篇博客：[Ubutnu 20.04 安装和使用单机版hadoop 3.2](https://zhang0peter.com/ubuntu-20.04-install-hadoop/)

但对于Windows用户来说，直接在Windows上安装和使用Hadoop会更加方便，毕竟Hadoop是Java写的，拥有跨平台的能力。

## 安装Java

Hadoop基于Java，需要先安装Java。Java 8是推荐版本，Java 11也可以使用。

我选择的是Java 11.

```sh
PS C:\Users\peter> java -version
openjdk version "11.0.7" 2020-04-14
OpenJDK Runtime Environment AdoptOpenJDK (build 11.0.7+10)
OpenJDK 64-Bit Server VM AdoptOpenJDK (build 11.0.7+10, mixed mode)
```

## 下载Hadoop


Hadoop 官网：[Apache  Hadoop releases list](http://hadoop.apache.org/releases.html)

**注意：最新版本 3.2.1在Windows下存在bug，无法正常使用**

当前最新版本为2.9.2，下载二进制包：

```sh
wget https://downloads.apache.org/hadoop/common/hadoop-2.9.2/hadoop-2.9.2.tar.gz
tar xvzf hadoop-*.tar.gz
mv hadoop-2.9.2 hadoop
```

## 下载 winutils

Hadoop在Windows上不能直接运行，需要额外下载`winutils`

官方不直接提供`winutils`，第三方GitHub上有已经编译完成的：[cdarlint/winutils: winutils.exe hadoop.dll and hdfs.dll binaries for hadoop windows](https://github.com/cdarlint/winutils)

下载后找到自己版本Hadoop的`winutils`，并把文件夹中的内容放置到`hadoop/bin`下。

如果Hadoop版本较新，没有第三方编译好的`winutils`，需要自己手动编译，比较繁琐，不推荐。

## 配置Hadoop

### 编辑环境变量


设置`JAVA_HOME `环境变量和其他变量，编辑文件`/hadoop/etc/hadoop/hadoop-env.cmd `

**windowsd的cmd不允许设置变量路径带有空格，所以Java的安装目录需要不带空格**

```sh
set JAVA_HOME=D:\AdoptOpenJDK\jdk-11.0.7.10-hotspot
set HADOOP_PREFIX=D:\hadoop
set HADOOP_CONF_DIR=%HADOOP_PREFIX%\etc\hadoop
set YARN_CONF_DIR=%HADOOP_CONF_DIR%
set PATH=%PATH%;%HADOOP_PREFIX%\bin
```

### 编辑配置文件

设置配置文件`hadoop/etc/hadoop/core-site.xml`：

```xml
<configuration>
<property>
  <name>fs.default.name</name>
    <value>hdfs://localhost:9000</value>
</property>
</configuration>
```

**********
配置文件`hadoop/etc/hadoop/hdfs-site.xml`：

**修改路径为你的目录**

```xml
<configuration>
<property>
 <name>dfs.replication</name>
 <value>1</value>
</property>

<property>
  <name>dfs.name.dir</name>
    <value>/D:/hadoop/hadoop/hadoopdata/hdfs/namenode</value>
</property>

<property>
  <name>dfs.data.dir</name>
    <value>/D:/hadoop/hadoopdata/hdfs/datanode</value>
</property>
</configuration>
```

***************
修改配置文件名`hadoop/etc/hadoop/mapred-site.xml.template`到`mapred-site.xml`：


```xml
<configuration>
 <property>
  <name>mapreduce.framework.name</name>
   <value>yarn</value>
 </property>
</configuration>
```
********

配置文件`hadoop/etc/hadoop/yarn-site.xml`：


```xml
<configuration>
 <property>
  <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
 </property>
</configuration>
```

**********



## 启动Hadoop单机伪集群

### 格式化 namenode 
```sh
hadoop/bin/hadoop.cmd namenode -format
```
```js
20/06/13 14:51:19 INFO namenode.NNStorageRetentionManager: Going to retain 1 images with txid >= 0
20/06/13 14:51:19 INFO namenode.FSImage: FSImageSaver clean checkpoint: txid = 0 when meet shutdown.
20/06/13 14:51:19 INFO namenode.NameNode: SHUTDOWN_MSG:
```

### 启动服务
```sh
hadoop/sbin/start-all.cmd
```

随后会跳出4个弹窗，每个弹窗应该都没有报错，正常运行。

## 查看集群状态

访问页面`http://localhost:50070/`

![图片]({% link /images/2020/2020-6-13-1.png %})

访问页面：`http://localhost:8088/`

![图片]({% link /images/2020/2020-6-13-2.png %})

