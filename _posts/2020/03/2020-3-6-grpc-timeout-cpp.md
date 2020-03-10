---
layout: post
title: "grpc C++ timeout 超时设置"
categories: [rpc]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

如果 `grpc`的客户端是阻塞式请求，那么默认是没有超时设置，会一直等待：

```c
grpc::ClientContext oCtx;
oStatus = poStub->Echo(&oCtx, echoRequest, &echoResponse);
```

官方文档：[gRPC and Deadlines](https://grpc.io/blog/deadlines/)

设置超时：

```c
unsigned int client_connection_timeout = 5;

ClientContext context;

// Set timeout for API
std::chrono::system_clock::time_point deadline =
    std::chrono::system_clock::now() + std::chrono::seconds(client_connection_timeout);

context.set_deadline(deadline);
```


{% raw %}
***          
{% endraw %}

关于`grpc`与`protobuf`的安装和使用，参考这篇博客：[grpc与protobuf最佳实践](http://zhang0peter.com/grpc-and-protobuf/)
