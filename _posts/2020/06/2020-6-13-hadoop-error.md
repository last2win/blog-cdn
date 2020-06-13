---
layout: post
title: "Windows下Hadoop格式化namenode报错解决： ERROR namenode.NameNode: Failed to start namenode."
categories: [行走的问题解决机]
description: "需要编辑配置文件 hdfs-site.xml"
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

最近在Windows 10上使用Hadoop时遇到了报错：


```sh
2020-06-12 23:31:43,159 ERROR namenode.NameNode: Failed to start namenode.
java.lang.IllegalArgumentException: No class configured for D
        at org.apache.hadoop.hdfs.server.namenode.FSEditLog.getJournalClass(FSEditLog.java:1792)
        at org.apache.hadoop.hdfs.server.namenode.FSEditLog.createJournal(FSEditLog.java:1808)
        at org.apache.hadoop.hdfs.server.namenode.FSEditLog.initJournals(FSEditLog.java:296)
        at org.apache.hadoop.hdfs.server.namenode.FSEditLog.initJournalsForWrite(FSEditLog.java:261)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.format(NameNode.java:1185)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.createNameNode(NameNode.java:1649)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.main(NameNode.java:1759)
2020-06-12 23:31:43,161 INFO util.ExitUtil: Exiting with status 1: java.lang.IllegalArgumentException: No class configured for D
```

出现这些报错的原因是`hdfs-site.xml`的配置文件路径有问题，我原先的配置文件：

```xml
<property>
  <name>dfs.name.dir</name>
    <value>D:/hadoop/hadoop/hadoopdata/hdfs/namenode</value>
</property>

<property>
  <name>dfs.data.dir</name>
    <value>D:/hadoop/hadoopdata/hdfs/datanode</value>
</property>
```

这里的路径写法应该更改：

```xml
<property>
  <name>dfs.name.dir</name>
    <value>/D:/hadoop/hadoop/hadoopdata/hdfs/namenode</value>
</property>

<property>
  <name>dfs.data.dir</name>
    <value>/D:/hadoop/hadoopdata/hdfs/datanode</value>
</property>
```

然后遇到了另一个报错：

```sh
2020-06-13 07:35:36,534 ERROR namenode.NameNode: Failed to start namenode.
java.lang.UnsupportedOperationException
        at java.base/java.nio.file.Files.setPosixFilePermissions(Files.java:2078)
        at org.apache.hadoop.hdfs.server.common.Storage$StorageDirectory.clearDirectory(Storage.java:452)
        at org.apache.hadoop.hdfs.server.namenode.NNStorage.format(NNStorage.java:591)
        at org.apache.hadoop.hdfs.server.namenode.NNStorage.format(NNStorage.java:613)
        at org.apache.hadoop.hdfs.server.namenode.FSImage.format(FSImage.java:188)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.format(NameNode.java:1206)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.createNameNode(NameNode.java:1649)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.main(NameNode.java:1759)
2020-06-13 07:35:36,560 INFO util.ExitUtil: Exiting with status 1: java.lang.UnsupportedOperationException
2020-06-13 07:35:36,599 INFO namenode.NameNode: SHUTDOWN_MSG:
```

在网上找的各种解决方法都不靠谱，然后我看到了这个回答：[java - Failed to start NameNode - Stack Overflow](https://stackoverflow.com/questions/58461455/failed-to-start-namenode)

里面提到`Hadoop 3.2.1` 版本有这个问题，换成旧的`Hadoop 2.10.0`版本就没有问题了，无语了。










