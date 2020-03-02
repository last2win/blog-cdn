---
layout: post
title: "grpc 报错解决：undefined symbol: _ZN9grpc_impl25InsecureServerCredentialsEv"
categories: [行走的问题解决机]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}


早上在用 grpc的时候遇到了报错：

```sh
-> # ./greeter_server 
./greeter_server: symbol lookup error: ./greeter_server: undefined symbol: _ZN9grpc_impl25InsecureServerCredentialsEv
```
```sh
-> # ldd -r greeter_server
	linux-vdso.so.1 (0x00007ffda25e3000)
	libprotobuf.so.22 => /usr/local/lib/libprotobuf.so.22 (0x00007f91fc0be000)
	libgrpc++.so.1 => /usr/local/lib/libgrpc++.so.1 (0x00007f91fbe6c000)
	libgpr.so.9 => /usr/local/lib/libgpr.so.9 (0x00007f91fbc59000)
	libgrpc++_reflection.so.1 => /usr/local/lib/libgrpc++_reflection.so.1 (0x00007f91fb8a9000)
	libstdc++.so.6 => /usr/lib/x86_64-linux-gnu/libstdc++.so.6 (0x00007f91fb520000)
	libgcc_s.so.1 => /lib/x86_64-linux-gnu/libgcc_s.so.1 (0x00007f91fb308000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f91faf17000)
	/lib64/ld-linux-x86-64.so.2 (0x00007f91fc79c000)
	libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007f91facf8000)
	libgrpc.so.5 => /usr/local/lib/libgrpc.so.5 (0x00007f91fa8b4000)
	librt.so.1 => /lib/x86_64-linux-gnu/librt.so.1 (0x00007f91fa6ac000)
	libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007f91fa30e000)
undefined symbol: _ZN9grpc_impl25InsecureServerCredentialsEv	(./greeter_server)
undefined symbol: _ZN9grpc_impl13ServerBuilder15RegisterServiceEPN4grpc7ServiceE	(./greeter_server)
undefined symbol: _ZN9grpc_impl13ServerBuilder13BuildAndStartEv	(./greeter_server)
undefined symbol: _ZN9grpc_impl13ServerBuilderC1Ev	(./greeter_server)
undefined symbol: _ZN9grpc_impl13ServerBuilderD1Ev	(./greeter_server)
undefined symbol: _ZN9grpc_impl13ServerBuilder16AddListeningPortERKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEESt10shared_ptrINS_17ServerCredentialsEEPi	(./greeter_server)
```
里面提到了报错：`undefined symbol: _ZN9grpc_impl25InsecureServerCredentialsEv`


我的解决方法是重装系统，重新编译。

正常的grpc编译后的结果如下：

```sh
root@node1:/home/ubuntu/grpc/examples/cpp/helloworld# ldd -r greeter_server
	linux-vdso.so.1 (0x00007fffb65f1000)
	libprotobuf.so.22 => /usr/local/lib/libprotobuf.so.22 (0x00007fee2ba23000)
	libgrpc++.so.1 => /usr/local/lib/libgrpc++.so.1 (0x00007fee2b77b000)
	libgpr.so.9 => /usr/local/lib/libgpr.so.9 (0x00007fee2b568000)
	libgrpc++_reflection.so.1 => /usr/local/lib/libgrpc++_reflection.so.1 (0x00007fee2b31a000)
	libstdc++.so.6 => /usr/lib/x86_64-linux-gnu/libstdc++.so.6 (0x00007fee2af91000)
	libgcc_s.so.1 => /lib/x86_64-linux-gnu/libgcc_s.so.1 (0x00007fee2ad79000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fee2a988000)
	/lib64/ld-linux-x86-64.so.2 (0x00007fee2c101000)
	libgrpc.so.9 => /usr/local/lib/libgrpc.so.9 (0x00007fee2a438000)
	librt.so.1 => /lib/x86_64-linux-gnu/librt.so.1 (0x00007fee2a230000)
	libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007fee2a011000)
	libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007fee29c73000)
```


{% raw %}
***          
{% endraw %}

关于`grpc`与`protobuf`的安装和使用，参考这篇博客：[grpc与protobuf最佳实践](http://zhang0peter.com/grpc-and-protobuf/)

