---
layout: post
title: "grpc 报错解决：/usr/local/lib/libgrpc.so: undefined reference to `OPENSSL_free'
"
categories: [行走的问题解决机]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

早上在用 grpc 的时候遇到了报错：

```sh
-> % make                         
g++ helloworld.pb.o helloworld.grpc.pb.o greeter_client.o -L/usr/local/lib `pkg-config --libs protobuf grpc++` -pthread -Wl,--no-as-needed -lgrpc++_reflection -Wl,--as-needed -ldl -o greeter_client
/usr/local/lib/libgrpc.so: undefined reference to `OPENSSL_free'
/usr/local/lib/libgrpc.so: undefined reference to `BIO_pending'
/usr/local/lib/libgrpc.so: undefined reference to `sk_pop_free_ex'
/usr/local/lib/libgrpc.so: undefined reference to `SSL_CTX_add_extra_chain_cert'
/usr/local/lib/libgrpc.so: undefined reference to `OpenSSL_add_all_algorithms'
/usr/local/lib/libgrpc.so: undefined reference to `EVP_DigestVerifyUpdate'
/usr/local/lib/libgrpc.so: undefined reference to `SSL_CTX_set_session_cache_mode'
/usr/local/lib/libgrpc.so: undefined reference to `SSL_CTX_set_tlsext_ticket_keys'
/usr/local/lib/libgrpc.so: undefined reference to `SSL_set_tlsext_host_name'
/usr/local/lib/libgrpc.so: undefined reference to `sk_new_null'
/usr/local/lib/libgrpc.so: undefined reference to `EVP_DigestSignUpdate'
/usr/local/lib/libgrpc.so: undefined reference to `SSL_CTX_get_ex_new_index'
/usr/local/lib/libgrpc.so: undefined reference to `SSL_CTX_set_tmp_ecdh'
/usr/local/lib/libgrpc.so: undefined reference to `SSL_CTX_set_tlsext_servername_arg'
/usr/local/lib/libgrpc.so: undefined reference to `SSL_load_error_strings'
/usr/local/lib/libgrpc.so: undefined reference to `SSL_library_init'
/usr/local/lib/libgrpc.so: undefined reference to `SSL_CTX_set_tlsext_servername_callback'
/usr/local/lib/libgrpc.so: undefined reference to `EVP_MD_CTX_destroy'
/usr/local/lib/libgrpc.so: undefined reference to `BIO_get_mem_data'
/usr/local/lib/libgrpc.so: undefined reference to `sk_num'
/usr/local/lib/libgrpc.so: undefined reference to `BIO_get_mem_ptr'
/usr/local/lib/libgrpc.so: undefined reference to `sk_value'
/usr/local/lib/libgrpc.so: undefined reference to `EVP_MD_CTX_create'
/usr/local/lib/libgrpc.so: undefined reference to `sk_push'
/usr/local/lib/libgrpc.so: undefined reference to `BIO_should_retry'
collect2: error: ld returned 1 exit status
Makefile:44: recipe for target 'greeter_client' failed
make: *** [greeter_client] Error 1
```

这个报错网上有人遇到过，但找不到好的解决方法。

有人说是openssl的问题：[undefined reference in libgrpc.so.0 when trying to build helloword in examples/cpp · Issue #6665 · grpc/grpc](https://github.com/grpc/grpc/issues/6665)

有人说安装`libssl1.0-dev`可以解决问题：
```sh
apt install libssl1.0-dev
```
我尝试了，不行。

我解决不了这个报错，只能把`grpc`卸载了，重装。

