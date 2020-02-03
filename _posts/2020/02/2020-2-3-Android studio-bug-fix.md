---
layout: post
title: "Android studio 报错解决：Error:SSL peer shut down incorrectly"
categories: [行走的问题解决机]
description: "Android studio 报错解决：Error:SSL peer shut down incorrectly"
keywords: Android studio
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}
很久没用 Android studio 了，刚刚在打开一个项目的biuld过程中报错：

```sh
SSL peer shut down incorrectly
```
具体的报错内容放文章结尾处。

这个报错很多人遇到，网上的一个解决方法是把配置文件`gradle-wrapper.properties`中的`distributionUrl`参数从`https://services.gradle.org/distributions/`换成`http://`。

但这个解决方法已经不行了，因为`gradle`把服务部署在`cloudflare`上，如果使用http访问会报错：
```js
Error 1020 Ray ID: 55f3e39ffd4aa2ca • 2020-02-03 10:58:20 UTC
Access denied
What happened?
This website is using a security service to protect itself from online attacks.
```

解决方法就是到官网手动下载压缩包：[Gradle Distributions](https://services.gradle.org/distributions/)，解压到目录`%USERPROFILE%/.gradle/wrapper/dists`

解压后重启Android studio即可。

另一个解决方法是把URL直接改为本地已经下载的gradle。




具体的报错内容：
```js
org.gradle.api.UncheckedIOException: Failed to capture snapshot of input files for task ':app:mergeDebugResources' property 'aapt2FromMaven' during up-to-date check.
	at org.gradle.api.internal.changedetection.state.CacheBackedTaskHistoryRepository.snapshotTaskFiles(CacheBackedTaskHistoryRepository.java:331)
	at org.gradle.api.internal.changedetection.state.CacheBackedTaskHistoryRepository.createExecution(CacheBackedTaskHistoryRepository.java:151)
	at org.gradle.api.internal.changedetection.state.CacheBackedTaskHistoryRepository.access$100(CacheBackedTaskHistoryRepository.java:61)
	at org.gradle.api.internal.changedetection.state.CacheBackedTaskHistoryRepository$1.getCurrentExecution(CacheBackedTaskHistoryRepository.java:111)
	at org.gradle.api.internal.changedetection.changes.DefaultTaskArtifactStateRepository$TaskArtifactStateImpl.getStates(DefaultTaskArtifactStateRepository.java:208)
	at org.gradle.api.internal.changedetection.changes.DefaultTaskArtifactStateRepository$TaskArtifactStateImpl.isUpToDate(DefaultTaskArtifactStateRepository.java:93)
	at org.gradle.api.internal.tasks.execution.SkipUpToDateTaskExecuter.execute(SkipUpToDateTaskExecuter.java:50)
	at org.gradle.api.internal.tasks.execution.ResolveTaskOutputCachingStateExecuter.execute(ResolveTaskOutputCachingStateExecuter.java:54)
	at org.gradle.api.internal.tasks.execution.ValidatingTaskExecuter.execute(ValidatingTaskExecuter.java:59)
	at org.gradle.api.internal.tasks.execution.SkipEmptySourceFilesTaskExecuter.execute(SkipEmptySourceFilesTaskExecuter.java:101)
	at org.gradle.api.internal.tasks.execution.FinalizeInputFilePropertiesTaskExecuter.execute(FinalizeInputFilePropertiesTaskExecuter.java:44)
	at org.gradle.api.internal.tasks.execution.CleanupStaleOutputsExecuter.execute(CleanupStaleOutputsExecuter.java:91)
	at org.gradle.api.internal.tasks.execution.ResolveTaskArtifactStateTaskExecuter.execute(ResolveTaskArtifactStateTaskExecuter.java:62)
	at org.gradle.api.internal.tasks.execution.SkipTaskWithNoActionsExecuter.execute(SkipTaskWithNoActionsExecuter.java:59)
	at org.gradle.api.internal.tasks.execution.SkipOnlyIfTaskExecuter.execute(SkipOnlyIfTaskExecuter.java:54)
	at org.gradle.api.internal.tasks.execution.ExecuteAtMostOnceTaskExecuter.execute(ExecuteAtMostOnceTaskExecuter.java:43)
	at org.gradle.api.internal.tasks.execution.CatchExceptionTaskExecuter.execute(CatchExceptionTaskExecuter.java:34)
	at org.gradle.execution.taskgraph.DefaultTaskGraphExecuter$EventFiringTaskWorker$1.run(DefaultTaskGraphExecuter.java:256)
	at org.gradle.internal.progress.DefaultBuildOperationExecutor$RunnableBuildOperationWorker.execute(DefaultBuildOperationExecutor.java:336)
	at org.gradle.internal.progress.DefaultBuildOperationExecutor$RunnableBuildOperationWorker.execute(DefaultBuildOperationExecutor.java:328)
	at org.gradle.internal.progress.DefaultBuildOperationExecutor.execute(DefaultBuildOperationExecutor.java:199)
	at org.gradle.internal.progress.DefaultBuildOperationExecutor.run(DefaultBuildOperationExecutor.java:110)
	at org.gradle.execution.taskgraph.DefaultTaskGraphExecuter$EventFiringTaskWorker.execute(DefaultTaskGraphExecuter.java:249)
	at org.gradle.execution.taskgraph.DefaultTaskGraphExecuter$EventFiringTaskWorker.execute(DefaultTaskGraphExecuter.java:238)
	at org.gradle.execution.taskgraph.DefaultTaskPlanExecutor$TaskExecutorWorker.processTask(DefaultTaskPlanExecutor.java:123)
	at org.gradle.execution.taskgraph.DefaultTaskPlanExecutor$TaskExecutorWorker.access$200(DefaultTaskPlanExecutor.java:79)
	at org.gradle.execution.taskgraph.DefaultTaskPlanExecutor$TaskExecutorWorker$1.execute(DefaultTaskPlanExecutor.java:104)
	at org.gradle.execution.taskgraph.DefaultTaskPlanExecutor$TaskExecutorWorker$1.execute(DefaultTaskPlanExecutor.java:98)
	at org.gradle.execution.taskgraph.DefaultTaskExecutionPlan.execute(DefaultTaskExecutionPlan.java:663)
	at org.gradle.execution.taskgraph.DefaultTaskExecutionPlan.executeWithTask(DefaultTaskExecutionPlan.java:597)
	at org.gradle.execution.taskgraph.DefaultTaskPlanExecutor$TaskExecutorWorker.run(DefaultTaskPlanExecutor.java:98)
	at org.gradle.internal.concurrent.ExecutorPolicy$CatchAndRecordFailures.onExecute(ExecutorPolicy.java:63)
	at org.gradle.internal.concurrent.ManagedExecutorImpl$1.run(ManagedExecutorImpl.java:46)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at org.gradle.internal.concurrent.ThreadFactoryImpl$ManagedThreadRunnable.run(ThreadFactoryImpl.java:55)
	at java.lang.Thread.run(Thread.java:748)
Caused by: org.gradle.api.internal.artifacts.ivyservice.DefaultLenientConfiguration$ArtifactResolveException: Could not resolve all files for configuration ':app:_internal_aapt2_binary'.
	at org.gradle.api.internal.artifacts.configurations.DefaultConfiguration.rethrowFailure(DefaultConfiguration.java:944)
	at org.gradle.api.internal.artifacts.configurations.DefaultConfiguration.access$1600(DefaultConfiguration.java:120)
	at org.gradle.api.internal.artifacts.configurations.DefaultConfiguration$ConfigurationFileCollection.getFiles(DefaultConfiguration.java:918)
	at org.gradle.api.internal.file.AbstractFileCollection.iterator(AbstractFileCollection.java:68)
	at org.gradle.api.internal.changedetection.state.AbstractFileCollectionSnapshotter$FileCollectionVisitorImpl.visitCollection(AbstractFileCollectionSnapshotter.java:72)
	at org.gradle.api.internal.file.AbstractFileCollection.visitRootElements(AbstractFileCollection.java:234)
	at org.gradle.api.internal.file.CompositeFileCollection.visitRootElements(CompositeFileCollection.java:185)
	at org.gradle.api.internal.changedetection.state.AbstractFileCollectionSnapshotter.snapshot(AbstractFileCollectionSnapshotter.java:55)
	at org.gradle.api.internal.changedetection.state.DefaultGenericFileCollectionSnapshotter.snapshot(DefaultGenericFileCollectionSnapshotter.java:38)
	at org.gradle.api.internal.changedetection.state.CacheBackedTaskHistoryRepository.snapshotTaskFiles(CacheBackedTaskHistoryRepository.java:329)
	... 36 more
Caused by: org.gradle.internal.resolve.ModuleVersionResolveException: Could not resolve com.android.tools.build:aapt2:3.2.0-4818971.
Required by:
    project :app
	at org.gradle.api.internal.artifacts.ivyservice.ivyresolve.RepositoryChainComponentMetaDataResolver.resolveModule(RepositoryChainComponentMetaDataResolver.java:103)
	at org.gradle.api.internal.artifacts.ivyservice.ivyresolve.RepositoryChainComponentMetaDataResolver.resolve(RepositoryChainComponentMetaDataResolver.java:63)
	at org.gradle.api.internal.artifacts.ivyservice.resolveengine.ComponentResolversChain$ComponentMetaDataResolverChain.resolve(ComponentResolversChain.java:93)
	at org.gradle.api.internal.artifacts.ivyservice.clientmodule.ClientModuleResolver.resolve(ClientModuleResolver.java:60)
	at org.gradle.api.internal.artifacts.ivyservice.resolveengine.graph.builder.ComponentState.resolve(ComponentState.java:163)
	at org.gradle.api.internal.artifacts.ivyservice.resolveengine.graph.builder.ComponentState.getMetaData(ComponentState.java:174)
	at org.gradle.api.internal.artifacts.ivyservice.resolveengine.graph.builder.EdgeState.calculateTargetConfigurations(EdgeState.java:137)
	at org.gradle.api.internal.artifacts.ivyservice.resolveengine.graph.builder.EdgeState.attachToTargetConfigurations(EdgeState.java:108)
	at org.gradle.api.internal.artifacts.ivyservice.resolveengine.graph.builder.DependencyGraphBuilder.attachToTargetRevisionsSerially(DependencyGraphBuilder.java:236)
	at org.gradle.api.internal.artifacts.ivyservice.resolveengine.graph.builder.DependencyGraphBuilder.resolveEdges(DependencyGraphBuilder.java:226)
	at org.gradle.api.internal.artifacts.ivyservice.resolveengine.graph.builder.DependencyGraphBuilder.traverseGraph(DependencyGraphBuilder.java:140)
	at org.gradle.api.internal.artifacts.ivyservice.resolveengine.graph.builder.DependencyGraphBuilder.resolve(DependencyGraphBuilder.java:111)
	at org.gradle.api.internal.artifacts.ivyservice.resolveengine.DefaultArtifactDependencyResolver.resolve(DefaultArtifactDependencyResolver.java:92)
	at org.gradle.api.internal.artifacts.ivyservice.DefaultConfigurationResolver.resolveGraph(DefaultConfigurationResolver.java:146)
	at org.gradle.api.internal.artifacts.ivyservice.ShortCircuitEmptyConfigurationResolver.resolveGraph(ShortCircuitEmptyConfigurationResolver.java:73)
	at org.gradle.api.internal.artifacts.ivyservice.ErrorHandlingConfigurationResolver.resolveGraph(ErrorHandlingConfigurationResolver.java:66)
	at org.gradle.api.internal.artifacts.configurations.DefaultConfiguration$4.run(DefaultConfiguration.java:494)
	at org.gradle.internal.progress.DefaultBuildOperationExecutor$RunnableBuildOperationWorker.execute(DefaultBuildOperationExecutor.java:336)
	at org.gradle.internal.progress.DefaultBuildOperationExecutor$RunnableBuildOperationWorker.execute(DefaultBuildOperationExecutor.java:328)
	at org.gradle.internal.progress.DefaultBuildOperationExecutor.execute(DefaultBuildOperationExecutor.java:199)
	at org.gradle.internal.progress.DefaultBuildOperationExecutor.run(DefaultBuildOperationExecutor.java:110)
	at org.gradle.api.internal.artifacts.configurations.DefaultConfiguration.resolveGraphIfRequired(DefaultConfiguration.java:485)
	at org.gradle.api.internal.artifacts.configurations.DefaultConfiguration.resolveToStateOrLater(DefaultConfiguration.java:470)
	at org.gradle.api.internal.artifacts.configurations.DefaultConfiguration.access$1700(DefaultConfiguration.java:120)
	at org.gradle.api.internal.artifacts.configurations.DefaultConfiguration$ConfigurationFileCollection.getSelectedArtifacts(DefaultConfiguration.java:927)
	at org.gradle.api.internal.artifacts.configurations.DefaultConfiguration$ConfigurationFileCollection.getFiles(DefaultConfiguration.java:915)
	... 43 more
Caused by: org.gradle.internal.resolve.ModuleVersionResolveException: Could not resolve com.android.tools.build:aapt2:3.2.0-4818971.
	at org.gradle.api.internal.artifacts.ivyservice.ivyresolve.ErrorHandlingModuleComponentRepository$ErrorHandlingModuleComponentRepositoryAccess.resolveComponentMetaData(ErrorHandlingModuleComponentRepository.java:139)
	at org.gradle.api.internal.artifacts.ivyservice.ivyresolve.ComponentMetaDataResolveState.process(ComponentMetaDataResolveState.java:66)
	at org.gradle.api.internal.artifacts.ivyservice.ivyresolve.ComponentMetaDataResolveState.resolve(ComponentMetaDataResolveState.java:58)
	at org.gradle.api.internal.artifacts.ivyservice.ivyresolve.RepositoryChainComponentMetaDataResolver.findBestMatch(RepositoryChainComponentMetaDataResolver.java:138)
	at org.gradle.api.internal.artifacts.ivyservice.ivyresolve.RepositoryChainComponentMetaDataResolver.findBestMatch(RepositoryChainComponentMetaDataResolver.java:119)
	at org.gradle.api.internal.artifacts.ivyservice.ivyresolve.RepositoryChainComponentMetaDataResolver.resolveModule(RepositoryChainComponentMetaDataResolver.java:92)
	... 68 more
Caused by: org.gradle.api.resources.ResourceException: Could not get resource 'https://dl.google.com/dl/android/maven2/com/android/tools/build/aapt2/3.2.0-4818971/aapt2-3.2.0-4818971.pom'.
	at org.gradle.internal.resource.ResourceExceptions.failure(ResourceExceptions.java:74)
	at org.gradle.internal.resource.ResourceExceptions.getFailed(ResourceExceptions.java:57)
	at org.gradle.internal.resource.transfer.DefaultCacheAwareExternalResourceAccessor.copyToCache(DefaultCacheAwareExternalResourceAccessor.java:201)
	at org.gradle.internal.resource.transfer.DefaultCacheAwareExternalResourceAccessor.access$300(DefaultCacheAwareExternalResourceAccessor.java:55)
	at org.gradle.internal.resource.transfer.DefaultCacheAwareExternalResourceAccessor$1.create(DefaultCacheAwareExternalResourceAccessor.java:90)
	at org.gradle.internal.resource.transfer.DefaultCacheAwareExternalResourceAccessor$1.create(DefaultCacheAwareExternalResourceAccessor.java:82)
	at org.gradle.cache.internal.ProducerGuard$AdaptiveProducerGuard.guardByKey(ProducerGuard.java:97)
	at org.gradle.internal.resource.transfer.DefaultCacheAwareExternalResourceAccessor.getResource(DefaultCacheAwareExternalResourceAccessor.java:82)
	at org.gradle.api.internal.artifacts.repositories.resolver.DefaultExternalResourceArtifactResolver.downloadByCoords(DefaultExternalResourceArtifactResolver.java:138)
	at org.gradle.api.internal.artifacts.repositories.resolver.DefaultExternalResourceArtifactResolver.downloadStaticResource(DefaultExternalResourceArtifactResolver.java:98)
	at org.gradle.api.internal.artifacts.repositories.resolver.DefaultExternalResourceArtifactResolver.resolveArtifact(DefaultExternalResourceArtifactResolver.java:65)
	at org.gradle.api.internal.artifacts.repositories.metadata.AbstractRepositoryMetadataSource.parseMetaDataFromArtifact(AbstractRepositoryMetadataSource.java:69)
	at org.gradle.api.internal.artifacts.repositories.metadata.AbstractRepositoryMetadataSource.create(AbstractRepositoryMetadataSource.java:59)
	at org.gradle.api.internal.artifacts.repositories.resolver.ExternalResourceResolver.resolveStaticDependency(ExternalResourceResolver.java:199)
	at org.gradle.api.internal.artifacts.repositories.resolver.MavenResolver.doResolveComponentMetaData(MavenResolver.java:117)
	at org.gradle.api.internal.artifacts.repositories.resolver.ExternalResourceResolver$RemoteRepositoryAccess.resolveComponentMetaData(ExternalResourceResolver.java:400)
	at org.gradle.api.internal.artifacts.ivyservice.ivyresolve.CachingModuleComponentRepository$ResolveAndCacheRepositoryAccess.resolveComponentMetaData(CachingModuleComponentRepository.java:381)
	at org.gradle.api.internal.artifacts.ivyservice.ivyresolve.ErrorHandlingModuleComponentRepository$ErrorHandlingModuleComponentRepositoryAccess.resolveComponentMetaData(ErrorHandlingModuleComponentRepository.java:136)
	... 73 more
Caused by: org.gradle.internal.resource.transport.http.HttpRequestException: Could not GET 'https://dl.google.com/dl/android/maven2/com/android/tools/build/aapt2/3.2.0-4818971/aapt2-3.2.0-4818971.pom'.
	at org.gradle.internal.resource.transport.http.HttpClientHelper.performRequest(HttpClientHelper.java:96)
	at org.gradle.internal.resource.transport.http.HttpClientHelper.performRawGet(HttpClientHelper.java:80)
	at org.gradle.internal.resource.transport.http.HttpClientHelper.performGet(HttpClientHelper.java:84)
	at org.gradle.internal.resource.transport.http.HttpResourceAccessor.openResource(HttpResourceAccessor.java:43)
	at org.gradle.internal.resource.transport.http.HttpResourceAccessor.openResource(HttpResourceAccessor.java:29)
	at org.gradle.internal.resource.transfer.DefaultExternalResourceConnector.openResource(DefaultExternalResourceConnector.java:56)
	at org.gradle.internal.resource.transfer.ProgressLoggingExternalResourceAccessor.openResource(ProgressLoggingExternalResourceAccessor.java:36)
	at org.gradle.internal.resource.transfer.AccessorBackedExternalResource.withContentIfPresent(AccessorBackedExternalResource.java:130)
	at org.gradle.internal.resource.BuildOperationFiringExternalResourceDecorator$11.call(BuildOperationFiringExternalResourceDecorator.java:237)
	at org.gradle.internal.resource.BuildOperationFiringExternalResourceDecorator$11.call(BuildOperationFiringExternalResourceDecorator.java:229)
	at org.gradle.internal.progress.DefaultBuildOperationExecutor$CallableBuildOperationWorker.execute(DefaultBuildOperationExecutor.java:350)
	at org.gradle.internal.progress.DefaultBuildOperationExecutor$CallableBuildOperationWorker.execute(DefaultBuildOperationExecutor.java:340)
	at org.gradle.internal.progress.DefaultBuildOperationExecutor.execute(DefaultBuildOperationExecutor.java:199)
	at org.gradle.internal.progress.DefaultBuildOperationExecutor.call(DefaultBuildOperationExecutor.java:120)
	at org.gradle.internal.resource.BuildOperationFiringExternalResourceDecorator.withContentIfPresent(BuildOperationFiringExternalResourceDecorator.java:229)
	at org.gradle.internal.resource.transfer.DefaultCacheAwareExternalResourceAccessor.copyToCache(DefaultCacheAwareExternalResourceAccessor.java:199)
	... 88 more
Caused by: javax.net.ssl.SSLHandshakeException: Remote host closed connection during handshake
	at sun.security.ssl.SSLSocketImpl.readRecord(SSLSocketImpl.java:994)
	at sun.security.ssl.SSLSocketImpl.performInitialHandshake(SSLSocketImpl.java:1367)
	at sun.security.ssl.SSLSocketImpl.startHandshake(SSLSocketImpl.java:1395)
	at sun.security.ssl.SSLSocketImpl.startHandshake(SSLSocketImpl.java:1379)
	at org.apache.http.conn.ssl.SSLConnectionSocketFactory.createLayeredSocket(SSLConnectionSocketFactory.java:396)
	at org.apache.http.impl.conn.DefaultHttpClientConnectionOperator.upgrade(DefaultHttpClientConnectionOperator.java:193)
	at org.apache.http.impl.conn.PoolingHttpClientConnectionManager.upgrade(PoolingHttpClientConnectionManager.java:389)
	at org.apache.http.impl.execchain.MainClientExec.establishRoute(MainClientExec.java:416)
	at org.apache.http.impl.execchain.MainClientExec.execute(MainClientExec.java:237)
	at org.apache.http.impl.execchain.ProtocolExec.execute(ProtocolExec.java:185)
	at org.apache.http.impl.execchain.RetryExec.execute(RetryExec.java:89)
	at org.apache.http.impl.execchain.RedirectExec.execute(RedirectExec.java:111)
	at org.apache.http.impl.client.InternalHttpClient.doExecute(InternalHttpClient.java:185)
	at org.apache.http.impl.client.CloseableHttpClient.execute(CloseableHttpClient.java:83)
	at org.gradle.internal.resource.transport.http.HttpClientHelper.performHttpRequest(HttpClientHelper.java:148)
	at org.gradle.internal.resource.transport.http.HttpClientHelper.performHttpRequest(HttpClientHelper.java:126)
	at org.gradle.internal.resource.transport.http.HttpClientHelper.executeGetOrHead(HttpClientHelper.java:103)
	at org.gradle.internal.resource.transport.http.HttpClientHelper.performRequest(HttpClientHelper.java:94)
	... 103 more
Caused by: java.io.EOFException: SSL peer shut down incorrectly
	at sun.security.ssl.InputRecord.read(InputRecord.java:505)
	at sun.security.ssl.SSLSocketImpl.readRecord(SSLSocketImpl.java:975)
	... 120 more


```

