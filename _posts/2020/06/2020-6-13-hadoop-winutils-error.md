---
layout: post
title: "Windows下Hadoop报错解决： java.io.FileNotFoundException: Could not locate Hadoop executable: winutils.exe"
categories: [行走的问题解决机]
description: "下载winutils解决报错"
---


此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

最近在Windows 10上使用Hadoop时遇到了报错：

```sh
java.lang.RuntimeException: java.io.FileNotFoundException: Could not locate Hadoop executable: D:\hadoop\bin\winutils.exe -see https://wiki.apache.org/hadoop/WindowsProblems
        at org.apache.hadoop.util.Shell.getWinUtilsPath(Shell.java:722)
        at org.apache.hadoop.util.Shell.getGetPermissionCommand(Shell.java:245)
        at org.apache.hadoop.fs.RawLocalFileSystem$DeprecatedRawLocalFileStatus.loadPermissionInfo(RawLocalFileSystem.java:693)
        at org.apache.hadoop.fs.RawLocalFileSystem$DeprecatedRawLocalFileStatus.getPermission(RawLocalFileSystem.java:668)
        at org.apache.hadoop.util.DiskChecker.mkdirsWithExistsAndPermissionCheck(DiskChecker.java:233)
        at org.apache.hadoop.util.DiskChecker.checkDirInternal(DiskChecker.java:141)
        at org.apache.hadoop.util.DiskChecker.checkDir(DiskChecker.java:116)
        at org.apache.hadoop.hdfs.server.datanode.StorageLocation.check(StorageLocation.java:128)
        at org.apache.hadoop.hdfs.server.datanode.StorageLocation.check(StorageLocation.java:44)
        at org.apache.hadoop.hdfs.server.datanode.checker.ThrottledAsyncChecker$1.call(ThrottledAsyncChecker.java:142)
        at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
        at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
        at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
        at java.base/java.lang.Thread.run(Thread.java:834)
Caused by: java.io.FileNotFoundException: Could not locate Hadoop executable: D:\hadoop\bin\winutils.exe -see https://wiki.apache.org/hadoop/WindowsProblems
        at org.apache.hadoop.util.Shell.getQualifiedBinInner(Shell.java:604)
        at org.apache.hadoop.util.Shell.getQualifiedBin(Shell.java:578)
        at org.apache.hadoop.util.Shell.<clinit>(Shell.java:675)
        at org.apache.hadoop.util.StringUtils.<clinit>(StringUtils.java:78)
        at org.apache.hadoop.hdfs.server.datanode.DataNode.secureMain(DataNode.java:2803)
        at org.apache.hadoop.hdfs.server.datanode.DataNode.main(DataNode.java:2828)
20/06/13 18:32:35 ERROR datanode.DataNode: Exception in secureMain
org.apache.hadoop.util.DiskChecker$DiskErrorException: Too many failed volumes - current valid volumes: 0, volumes configured: 1, volumes failed: 1, volume failures tolerated: 0
        at org.apache.hadoop.hdfs.server.datanode.checker.StorageLocationChecker.check(StorageLocationChecker.java:228)
        at org.apache.hadoop.hdfs.server.datanode.DataNode.makeInstance(DataNode.java:2703)
        at org.apache.hadoop.hdfs.server.datanode.DataNode.instantiateDataNode(DataNode.java:2613)
        at org.apache.hadoop.hdfs.server.datanode.DataNode.createDataNode(DataNode.java:2660)
        at org.apache.hadoop.hdfs.server.datanode.DataNode.secureMain(DataNode.java:2804)
        at org.apache.hadoop.hdfs.server.datanode.DataNode.main(DataNode.java:2828)
20/06/13 18:32:35 INFO util.ExitUtil: Exiting with status 1: org.apache.hadoop.util.DiskChecker$DiskErrorException: Too many failed volumes - current valid volumes: 0, volumes configured: 1, volumes failed: 1, volume failures tolerated: 0

20/06/13 18:32:36 ERROR namenode.NameNode: Failed to start namenode.
java.lang.UnsatisfiedLinkError: 'boolean org.apache.hadoop.io.nativeio.NativeIO$Windows.access0(java.lang.String, int)'
        at org.apache.hadoop.io.nativeio.NativeIO$Windows.access0(Native Method)
        at org.apache.hadoop.io.nativeio.NativeIO$Windows.access(NativeIO.java:606)
        at org.apache.hadoop.fs.FileUtil.canWrite(FileUtil.java:1017)
        at org.apache.hadoop.hdfs.server.common.Storage$StorageDirectory.analyzeStorage(Storage.java:558)
        at org.apache.hadoop.hdfs.server.common.Storage$StorageDirectory.analyzeStorage(Storage.java:518)
        at org.apache.hadoop.hdfs.server.namenode.FSImage.recoverStorageDirs(FSImage.java:383)
        at org.apache.hadoop.hdfs.server.namenode.FSImage.recoverTransitionRead(FSImage.java:239)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.loadFSImage(FSNamesystem.java:1063)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.loadFromDisk(FSNamesystem.java:685)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.loadNamesystem(NameNode.java:674)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.initialize(NameNode.java:736)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.<init>(NameNode.java:961)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.<init>(NameNode.java:940)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.createNameNode(NameNode.java:1714)
        at org.apache.hadoop.hdfs.server.namenode.NameNode.main(NameNode.java:1782)
20/06/13 18:32:36 INFO util.ExitUtil: Exiting with status 1: java.lang.UnsatisfiedLinkError: 'boolean org.apache.hadoop.io.nativeio.NativeIO$Windows.access0(java.lang.String, int)'

20/06/13 18:45:20 INFO service.AbstractService: Service org.apache.hadoop.yarn.server.nodemanager.LocalDirsHandlerService failed in state INITED; cause: java.lang.RuntimeException: java.io.FileNotFoundException: Could not locate Hadoop executable: D:\hadoop\bin\winutils.exe -see https://wiki.apache.org/hadoop/WindowsProblems
java.lang.RuntimeException: java.io.FileNotFoundException: Could not locate Hadoop executable: D:\hadoop\bin\winutils.exe -see https://wiki.apache.org/hadoop/WindowsProblems
        at org.apache.hadoop.util.Shell.getWinUtilsPath(Shell.java:722)
        at org.apache.hadoop.util.Shell.getSetPermissionCommand(Shell.java:256)
        at org.apache.hadoop.util.Shell.getSetPermissionCommand(Shell.java:273)
        at org.apache.hadoop.fs.RawLocalFileSystem.setPermission(RawLocalFileSystem.java:767)
        at org.apache.hadoop.fs.RawLocalFileSystem.mkOneDirWithMode(RawLocalFileSystem.java:511)
        at org.apache.hadoop.fs.RawLocalFileSystem.mkdirsWithOptionalPermission(RawLocalFileSystem.java:551)
        at org.apache.hadoop.fs.RawLocalFileSystem.mkdirs(RawLocalFileSystem.java:529)
        at org.apache.hadoop.fs.FileSystem.primitiveMkdir(FileSystem.java:1228)
        at org.apache.hadoop.fs.DelegateToFileSystem.mkdir(DelegateToFileSystem.java:183)
        at org.apache.hadoop.fs.FilterFs.mkdir(FilterFs.java:212)
        at org.apache.hadoop.fs.FileContext$4.next(FileContext.java:747)
        at org.apache.hadoop.fs.FileContext$4.next(FileContext.java:743)
        at org.apache.hadoop.fs.FSLinkResolver.resolve(FSLinkResolver.java:90)
        at org.apache.hadoop.fs.FileContext.mkdir(FileContext.java:743)
        at org.apache.hadoop.yarn.server.nodemanager.DirectoryCollection.createDir(DirectoryCollection.java:553)
        at org.apache.hadoop.yarn.server.nodemanager.DirectoryCollection.createNonExistentDirs(DirectoryCollection.java:343)
        at org.apache.hadoop.yarn.server.nodemanager.LocalDirsHandlerService.serviceInit(LocalDirsHandlerService.java:214)
        at org.apache.hadoop.service.AbstractService.init(AbstractService.java:164)
        at org.apache.hadoop.service.CompositeService.serviceInit(CompositeService.java:108)
        at org.apache.hadoop.yarn.server.nodemanager.NodeHealthCheckerService.serviceInit(NodeHealthCheckerService.java:59)
        at org.apache.hadoop.service.AbstractService.init(AbstractService.java:164)
        at org.apache.hadoop.service.CompositeService.serviceInit(CompositeService.java:108)
        at org.apache.hadoop.yarn.server.nodemanager.NodeManager.serviceInit(NodeManager.java:468)
        at org.apache.hadoop.service.AbstractService.init(AbstractService.java:164)
        at org.apache.hadoop.yarn.server.nodemanager.NodeManager.initAndStartNodeManager(NodeManager.java:878)
        at org.apache.hadoop.yarn.server.nodemanager.NodeManager.main(NodeManager.java:946)
Caused by: java.io.FileNotFoundException: Could not locate Hadoop executable: D:\hadoop\bin\winutils.exe -see https://wiki.apache.org/hadoop/WindowsProblems
        at org.apache.hadoop.util.Shell.getQualifiedBinInner(Shell.java:604)
        at org.apache.hadoop.util.Shell.getQualifiedBin(Shell.java:578)
        at org.apache.hadoop.util.Shell.<clinit>(Shell.java:675)
        at org.apache.hadoop.util.StringUtils.<clinit>(StringUtils.java:78)
        at org.apache.hadoop.yarn.server.nodemanager.NodeManager.main(NodeManager.java:941)
20/06/13 18:45:20 INFO service.AbstractService: Service org.apache.hadoop.yarn.server.nodemanager.NodeHealthCheckerService failed in state INITED; cause: java.lang.RuntimeException: java.io.FileNotFoundException: Could not locate Hadoop executable: D:\hadoop\bin\winutils.exe -see https://wiki.apache.org/hadoop/WindowsProblems
java.lang.RuntimeException: java.io.FileNotFoundException: Could not locate Hadoop executable: D:\hadoop\bin\winutils.exe -see https://wiki.apache.org/hadoop/WindowsProblems
        at org.apache.hadoop.util.Shell.getWinUtilsPath(Shell.java:722)
        at org.apache.hadoop.util.Shell.getSetPermissionCommand(Shell.java:256)
        at org.apache.hadoop.util.Shell.getSetPermissionCommand(Shell.java:273)
        at org.apache.hadoop.fs.RawLocalFileSystem.setPermission(RawLocalFileSystem.java:767)
        at org.apache.hadoop.fs.RawLocalFileSystem.mkOneDirWithMode(RawLocalFileSystem.java:511)
        at org.apache.hadoop.fs.RawLocalFileSystem.mkdirsWithOptionalPermission(RawLocalFileSystem.java:551)
        at org.apache.hadoop.fs.RawLocalFileSystem.mkdirs(RawLocalFileSystem.java:529)
        at org.apache.hadoop.fs.FileSystem.primitiveMkdir(FileSystem.java:1228)
        at org.apache.hadoop.fs.DelegateToFileSystem.mkdir(DelegateToFileSystem.java:183)
        at org.apache.hadoop.fs.FilterFs.mkdir(FilterFs.java:212)
        at org.apache.hadoop.fs.FileContext$4.next(FileContext.java:747)
        at org.apache.hadoop.fs.FileContext$4.next(FileContext.java:743)
        at org.apache.hadoop.fs.FSLinkResolver.resolve(FSLinkResolver.java:90)
        at org.apache.hadoop.fs.FileContext.mkdir(FileContext.java:743)
        at org.apache.hadoop.yarn.server.nodemanager.DirectoryCollection.createDir(DirectoryCollection.java:553)
        at org.apache.hadoop.yarn.server.nodemanager.DirectoryCollection.createNonExistentDirs(DirectoryCollection.java:343)
        at org.apache.hadoop.yarn.server.nodemanager.LocalDirsHandlerService.serviceInit(LocalDirsHandlerService.java:214)
        at org.apache.hadoop.service.AbstractService.init(AbstractService.java:164)
        at org.apache.hadoop.service.CompositeService.serviceInit(CompositeService.java:108)
        at org.apache.hadoop.yarn.server.nodemanager.NodeHealthCheckerService.serviceInit(NodeHealthCheckerService.java:59)
        at org.apache.hadoop.service.AbstractService.init(AbstractService.java:164)
        at org.apache.hadoop.service.CompositeService.serviceInit(CompositeService.java:108)
        at org.apache.hadoop.yarn.server.nodemanager.NodeManager.serviceInit(NodeManager.java:468)
        at org.apache.hadoop.service.AbstractService.init(AbstractService.java:164)
        at org.apache.hadoop.yarn.server.nodemanager.NodeManager.initAndStartNodeManager(NodeManager.java:878)
        at org.apache.hadoop.yarn.server.nodemanager.NodeManager.main(NodeManager.java:946)
Caused by: java.io.FileNotFoundException: Could not locate Hadoop executable: D:\hadoop\bin\winutils.exe -see https://wiki.apache.org/hadoop/WindowsProblems
        at org.apache.hadoop.util.Shell.getQualifiedBinInner(Shell.java:604)
        at org.apache.hadoop.util.Shell.getQualifiedBin(Shell.java:578)
        at org.apache.hadoop.util.Shell.<clinit>(Shell.java:675)
        at org.apache.hadoop.util.StringUtils.<clinit>(StringUtils.java:78)
        at org.apache.hadoop.yarn.server.nodemanager.NodeManager.main(NodeManager.java:941)
20/06/13 18:45:20 INFO service.AbstractService: Service NodeManager failed in state INITED; cause: java.lang.RuntimeException: java.io.FileNotFoundException: Could not locate Hadoop executable: D:\hadoop\bin\winutils.exe -see https://wiki.apache.org/hadoop/WindowsProblems
java.lang.RuntimeException: java.io.FileNotFoundException: Could not locate Hadoop executable: D:\hadoop\bin\winutils.exe -see https://wiki.apache.org/hadoop/WindowsProblems
        at org.apache.hadoop.util.Shell.getWinUtilsPath(Shell.java:722)
        at org.apache.hadoop.util.Shell.getSetPermissionCommand(Shell.java:256)
        at org.apache.hadoop.util.Shell.getSetPermissionCommand(Shell.java:273)
        at org.apache.hadoop.fs.RawLocalFileSystem.setPermission(RawLocalFileSystem.java:767)
        at org.apache.hadoop.fs.RawLocalFileSystem.mkOneDirWithMode(RawLocalFileSystem.java:511)
        at org.apache.hadoop.fs.RawLocalFileSystem.mkdirsWithOptionalPermission(RawLocalFileSystem.java:551)
        at org.apache.hadoop.fs.RawLocalFileSystem.mkdirs(RawLocalFileSystem.java:529)
        at org.apache.hadoop.fs.FileSystem.primitiveMkdir(FileSystem.java:1228)
        at org.apache.hadoop.fs.DelegateToFileSystem.mkdir(DelegateToFileSystem.java:183)
        at org.apache.hadoop.fs.FilterFs.mkdir(FilterFs.java:212)
        at org.apache.hadoop.fs.FileContext$4.next(FileContext.java:747)
        at org.apache.hadoop.fs.FileContext$4.next(FileContext.java:743)
        at org.apache.hadoop.fs.FSLinkResolver.resolve(FSLinkResolver.java:90)
        at org.apache.hadoop.fs.FileContext.mkdir(FileContext.java:743)
        at org.apache.hadoop.yarn.server.nodemanager.DirectoryCollection.createDir(DirectoryCollection.java:553)
        at org.apache.hadoop.yarn.server.nodemanager.DirectoryCollection.createNonExistentDirs(DirectoryCollection.java:343)
        at org.apache.hadoop.yarn.server.nodemanager.LocalDirsHandlerService.serviceInit(LocalDirsHandlerService.java:214)
        at org.apache.hadoop.service.AbstractService.init(AbstractService.java:164)
        at org.apache.hadoop.service.CompositeService.serviceInit(CompositeService.java:108)
        at org.apache.hadoop.yarn.server.nodemanager.NodeHealthCheckerService.serviceInit(NodeHealthCheckerService.java:59)
        at org.apache.hadoop.service.AbstractService.init(AbstractService.java:164)
        at org.apache.hadoop.service.CompositeService.serviceInit(CompositeService.java:108)
        at org.apache.hadoop.yarn.server.nodemanager.NodeManager.serviceInit(NodeManager.java:468)
        at org.apache.hadoop.service.AbstractService.init(AbstractService.java:164)
        at org.apache.hadoop.yarn.server.nodemanager.NodeManager.initAndStartNodeManager(NodeManager.java:878)
        at org.apache.hadoop.yarn.server.nodemanager.NodeManager.main(NodeManager.java:946)
Caused by: java.io.FileNotFoundException: Could not locate Hadoop executable: D:\hadoop\bin\winutils.exe -see https://wiki.apache.org/hadoop/WindowsProblems
        at org.apache.hadoop.util.Shell.getQualifiedBinInner(Shell.java:604)
        at org.apache.hadoop.util.Shell.getQualifiedBin(Shell.java:578)
        at org.apache.hadoop.util.Shell.<clinit>(Shell.java:675)
        at org.apache.hadoop.util.StringUtils.<clinit>(StringUtils.java:78)
        at org.apache.hadoop.yarn.server.nodemanager.NodeManager.main(NodeManager.java:941)
20/06/13 18:45:20 INFO impl.MetricsSystemImpl: Stopping NodeManager metrics system...
20/06/13 18:45:20 INFO impl.MetricsSystemImpl: NodeManager metrics system stopped.
20/06/13 18:45:20 INFO impl.MetricsSystemImpl: NodeManager metrics system shutdown complete.
20/06/13 18:45:20 ERROR nodemanager.NodeManager: Error starting NodeManager
java.lang.RuntimeException: java.io.FileNotFoundException: Could not locate Hadoop executable: D:\hadoop\bin\winutils.exe -see https://wiki.apache.org/hadoop/WindowsProblems
        at org.apache.hadoop.util.Shell.getWinUtilsPath(Shell.java:722)
        at org.apache.hadoop.util.Shell.getSetPermissionCommand(Shell.java:256)
        at org.apache.hadoop.util.Shell.getSetPermissionCommand(Shell.java:273)
        at org.apache.hadoop.fs.RawLocalFileSystem.setPermission(RawLocalFileSystem.java:767)
        at org.apache.hadoop.fs.RawLocalFileSystem.mkOneDirWithMode(RawLocalFileSystem.java:511)
        at org.apache.hadoop.fs.RawLocalFileSystem.mkdirsWithOptionalPermission(RawLocalFileSystem.java:551)
        at org.apache.hadoop.fs.RawLocalFileSystem.mkdirs(RawLocalFileSystem.java:529)
        at org.apache.hadoop.fs.FileSystem.primitiveMkdir(FileSystem.java:1228)
        at org.apache.hadoop.fs.DelegateToFileSystem.mkdir(DelegateToFileSystem.java:183)
        at org.apache.hadoop.fs.FilterFs.mkdir(FilterFs.java:212)
        at org.apache.hadoop.fs.FileContext$4.next(FileContext.java:747)
        at org.apache.hadoop.fs.FileContext$4.next(FileContext.java:743)
        at org.apache.hadoop.fs.FSLinkResolver.resolve(FSLinkResolver.java:90)
        at org.apache.hadoop.fs.FileContext.mkdir(FileContext.java:743)
        at org.apache.hadoop.yarn.server.nodemanager.DirectoryCollection.createDir(DirectoryCollection.java:553)
        at org.apache.hadoop.yarn.server.nodemanager.DirectoryCollection.createNonExistentDirs(DirectoryCollection.java:343)
        at org.apache.hadoop.yarn.server.nodemanager.LocalDirsHandlerService.serviceInit(LocalDirsHandlerService.java:214)
        at org.apache.hadoop.service.AbstractService.init(AbstractService.java:164)
        at org.apache.hadoop.service.CompositeService.serviceInit(CompositeService.java:108)
        at org.apache.hadoop.yarn.server.nodemanager.NodeHealthCheckerService.serviceInit(NodeHealthCheckerService.java:59)
        at org.apache.hadoop.service.AbstractService.init(AbstractService.java:164)
        at org.apache.hadoop.service.CompositeService.serviceInit(CompositeService.java:108)
        at org.apache.hadoop.yarn.server.nodemanager.NodeManager.serviceInit(NodeManager.java:468)
        at org.apache.hadoop.service.AbstractService.init(AbstractService.java:164)
        at org.apache.hadoop.yarn.server.nodemanager.NodeManager.initAndStartNodeManager(NodeManager.java:878)
        at org.apache.hadoop.yarn.server.nodemanager.NodeManager.main(NodeManager.java:946)
Caused by: java.io.FileNotFoundException: Could not locate Hadoop executable: D:\hadoop\bin\winutils.exe -see https://wiki.apache.org/hadoop/WindowsProblems
        at org.apache.hadoop.util.Shell.getQualifiedBinInner(Shell.java:604)
        at org.apache.hadoop.util.Shell.getQualifiedBin(Shell.java:578)
        at org.apache.hadoop.util.Shell.<clinit>(Shell.java:675)
        at org.apache.hadoop.util.StringUtils.<clinit>(StringUtils.java:78)
        at org.apache.hadoop.yarn.server.nodemanager.NodeManager.main(NodeManager.java:941)
```

遇到这个报错的原因是Hadoop在Windows上不能直接运行，需要额外下载`winutils`。



官方不直接提供`winutils`，第三方GitHub上有已经编译完成的：[cdarlint/winutils: winutils.exe hadoop.dll and hdfs.dll binaries for hadoop windows](https://github.com/cdarlint/winutils)

下载后找到自己版本Hadoop的`winutils`，并把文件夹中的内容放置到`hadoop/bin`下。

如果Hadoop版本较新，没有第三方编译好的`winutils`，需要自己手动编译，比较繁琐，不推荐。

拷贝文件后，程序可以正常运行。

