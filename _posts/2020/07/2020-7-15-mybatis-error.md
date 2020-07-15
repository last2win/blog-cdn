---
layout: post
title: "mybatis报错解决：org.apache.ibatis.binding.BindingException: Invalid bound statement (not found)
"
categories: [行走的问题解决机]
description: "将Mapper.xml所在的resources目录标记为Resources root"
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}





晚上在使用spring-mybatis时遇到了问题。

访问控制器，获得的回复如下：
```json
{"success":false,"errorCode":4000000,"errorMsg":"System Error","result":null}
```

代码中的报错如下：
```java
org.apache.ibatis.binding.BindingException: Invalid bound statement (not found): com.service.spring.template.repository.mybatis.mapper.PaymentOrderMapper.selectByPrimaryKey
	at org.apache.ibatis.binding.MapperMethod$SqlCommand.<init>(MapperMethod.java:196) 
```

这个报错的原因是Mapper interface和Mapper xml的定义对应不上。

先做如下检查：

1、检查xml文件所在的package名称是否和interface对应的package名称一一对应。

2、检查xml文件的namespace是否和xml文件的package名称一一对应。


但如果你的mapper文件是使用mybatis generator生成的，遇到上面两个问题的可能性很小。

*****

如果你是用IDEA，那么可以进行如下尝试：

鼠标右键`Mapper.xml`所在的`resources`目录，选择`Mark Directory as`，标记为`Resources root`，然后惊奇的发现之前的文件夹结构变了，这时候重新移动文件位置，问题就解决了！！！

这是因为IDEA的文件夹结构有问题。

如果还没解决你的问题，欢迎看一下别人的博客。







{% raw %}
***          
{% endraw %}


完整报错如下：
```java
org.apache.ibatis.binding.BindingException: Invalid bound statement (not found): com.service.spring.template.repository.mybatis.mapper.PaymentOrderMapper.selectByPrimaryKey
	at org.apache.ibatis.binding.MapperMethod$SqlCommand.<init>(MapperMethod.java:196) 
	at org.apache.ibatis.binding.MapperMethod.<init>(MapperMethod.java:44) 
	at org.apache.ibatis.binding.MapperProxy.cachedMapperMethod(MapperProxy.java:59) 
	at org.apache.ibatis.binding.MapperProxy.invoke(MapperProxy.java:52) 
	at com.sun.proxy.$Proxy103.selectByPrimaryKey(Unknown Source) 
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) 
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) 
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) 
	at java.lang.reflect.Method.invoke(Method.java:498) 
	at com.dianping.zebra.dao.AsyncMapperProxy.invoke(AsyncMapperProxy.java:68) 
	at com.sun.proxy.$Proxy103.selectByPrimaryKey(Unknown Source) 
	at com.service.spring.template.api.PayService.getPayment(PayService.java:19) 
	at com.service.spring.template.api.controller.PayController.getUserInfo(PayController.java:27) 
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) 
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) 
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) 
	at java.lang.reflect.Method.invoke(Method.java:498) 
	at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:205) 
	at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:133) 
	at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:97) 
	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:854) 
	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:765) 
	at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:85) 
	at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:967) 
	at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:901) 
	at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:970) 
	at org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:861) 
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:634) 
	at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:846) 
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:741) 
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:231) 
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) 
	at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:52) 
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) 
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) 
	at org.springframework.boot.web.filter.ApplicationContextHeaderFilter.doFilterInternal(ApplicationContextHeaderFilter.java:54) 
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) 
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) 
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) 
	at com.service.spring.template.common.sign.CacheBodyFilter.doFilter(CacheBodyFilter.java:29) 
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) 
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) 
	at com.dianping.cat.servlet.CatFilter.logTransaction(CatFilter.java:273) 
	at com.dianping.cat.servlet.CatFilter.doFilter(CatFilter.java:92) 
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) 
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) 
	at org.springframework.boot.actuate.trace.WebRequestTraceFilter.doFilterInternal(WebRequestTraceFilter.java:111) 
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) 
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) 
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) 
	at org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:99) 
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) 
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) 
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) 
	at org.springframework.web.filter.HttpPutFormContentFilter.doFilterInternal(HttpPutFormContentFilter.java:109) 
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) 
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) 
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) 
	at org.springframework.web.filter.HiddenHttpMethodFilter.doFilterInternal(HiddenHttpMethodFilter.java:93) 
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) 
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) 
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) 
	at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:197) 
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) 
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) 
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) 
	at org.springframework.boot.actuate.autoconfigure.MetricsFilter.doFilterInternal(MetricsFilter.java:100) 
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) 
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) 
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) 
	at com.pdd.iris.spring.filter.IrisWebFilter.doFilter(IrisWebFilter.java:43) 
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) 
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) 
	at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:199) 
	at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:96) 
	at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:493) 
	at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:137) 
	at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:81) 
	at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:87) 
	at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:343) 
	at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:798) 
	at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:66) 
	at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:808) 
	at org.apache.tomcat.util.net.Nio2Endpoint$SocketProcessor.doRun(Nio2Endpoint.java:1775) 
	at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49) 
	at org.apache.tomcat.util.net.AbstractEndpoint.processSocket(AbstractEndpoint.java:1082) 
	at org.apache.tomcat.util.net.Nio2Endpoint$Nio2SocketWrapper$2.completed(Nio2Endpoint.java:569) 
	at org.apache.tomcat.util.net.Nio2Endpoint$Nio2SocketWrapper$2.completed(Nio2Endpoint.java:547) 
	at sun.nio.ch.Invoker.invokeUnchecked(Invoker.java:126) 
	at sun.nio.ch.Invoker$2.run(Invoker.java:218) 
	at sun.nio.ch.AsynchronousChannelGroupImpl$1.run(AsynchronousChannelGroupImpl.java:112) 
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149) 
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624) 
	at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61) 
	at java.lang.Thread.run(Thread.java:748) 

```







