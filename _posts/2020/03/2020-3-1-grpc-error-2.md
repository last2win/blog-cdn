---
layout: post
title: "grpc与protobuf 报错解决：Perhaps you should add the directory containing `protobuf.pc' to the PKG_CONFIG_PATH environment variable"
categories: [行走的问题解决机]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

下午在使用`grpc`的时候遇到了报错：
```sh
-> # make
g++ -std=c++11 `pkg-config --cflags protobuf grpc`  -c -o helloworld.pb.o helloworld.pb.cc
Package protobuf was not found in the pkg-config search path.
Perhaps you should add the directory containing `protobuf.pc'
to the PKG_CONFIG_PATH environment variable
No package 'protobuf' found
In file included from helloworld.pb.cc:5:0:
helloworld.pb.h:9:10: fatal error: google/protobuf/stubs/common.h: No such file or directory
 #include <google/protobuf/stubs/common.h>
          ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
compilation terminated.
<builtin>: recipe for target 'helloworld.pb.o' failed
make: *** [helloworld.pb.o] Error 1
```
里面提到了报错`Perhaps you should add the directory containing protobuf.pc' to the PKG_CONFIG_PATH environment variable`和`fatal error: google/protobuf/stubs/common.h: No such file or directory`

这是因为我是直接下载了二进制的`protobuf`，没有编译安装，缺少头文件。

其实`protobuf`和`grpc`的安装都比较麻烦，如果不嫌弃库的版本太老，推荐直接安装使用：

```sh
apt install libgrpc++-dev protobuf-compiler-grpc libprotobuf-dev 
```
问题解决。

{% raw %}
***          
{% endraw %}

关于`grpc`与`protobuf`的安装和使用，参考这篇博客：[grpc与protobuf最佳实践](http://zhang0peter.com/grpc-and-protobuf/)