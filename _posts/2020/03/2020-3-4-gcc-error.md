---
layout: post
title: "gcc/g++ 报错解决： multiple definition of first defined here"
categories: [行走的问题解决机]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

下午在用 gcc/g++ 编译 c/c++ 代码时遇到了报错：
```sh
paxos-kv/DBImpl.o: In function `KeyHitEntityID(unsigned long, rocksdb::Slice const&)':
/home/zhang/paxosstore/./paxos-kv/Coding.h:277: multiple definition of `KeyHitEntityID(unsigned long, rocksdb::Slice const&)'
paxos-kv/PLogImpl.o:/home/zhang/paxosstore/./paxos-kv/Coding.h:277: first defined here
paxos-kv/ServiceImpl.o: In function `__gnu_cxx::new_allocator<std::_Rb_tree_node<std::pair<std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const, std::pair<KVStatus, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > > > > >::~new_allocator()':
/home/zhang/paxosstore/./paxos-kv/Coding.h:277: multiple definition of `KeyHitEntityID(unsigned long, rocksdb::Slice const&)'
paxos-kv/PLogImpl.o:/home/zhang/paxosstore/./paxos-kv/Coding.h:277: first defined here
paxos-kv/Server.o: In function `KeyHitEntityID(unsigned long, rocksdb::Slice const&)':
/home/zhang/paxosstore/./paxos-kv/Coding.h:277: multiple definition of `KeyHitEntityID(unsigned long, rocksdb::Slice const&)'
paxos-kv/PLogImpl.o:/home/zhang/paxosstore/./paxos-kv/Coding.h:277: first defined here
paxos-kv/DBImpl.o: In function `clsDBImpl::GetEntityMeta(unsigned long, unsigned long&, unsigned int&)':
/home/zhang/paxosstore/paxos-kv/DBImpl.cpp:92: undefined reference to `clsDBImpl::GetEntityMeta(unsigned long, unsigned long&, unsigned int&, rocksdb::Snapshot const*)'
paxos-kv/ServiceImpl.o: In function `clsServiceImpl::GetDBEntityMeta(grpc::ServerContext&, kv_rpc::GetDBRequest const&, kv_rpc::GetDBResponse&)':
/home/zhang/paxosstore/paxos-kv/ServiceImpl.cpp:97: undefined reference to `clsDBImpl::GetEntityMeta(unsigned long, unsigned long&, unsigned int&, rocksdb::Snapshot const*)'
collect2: error: ld returned 1 exit status
Makefile:154: recipe for target 'db_server' failed
make: *** [db_server] Error 1
rm certain/src/Certain.pb.cc
```
报错的重点是`first defined here In function `和`multiple definition of`

测试示例代码如下：

`util.h` 
```c
#pragma once
#include <stdio.h>
void print(){
    printf("test");
}
```
`a.c`
```c
#include "util.h"
void a(){
    print();
}
```
`main.c`
```c
#include "util.h"
#include "a.c"
int main(void){
    a();
}
```

编译：
```sh
gcc -c a.c -o a.out
gcc -c main.c -o main.out
gcc main.out a.out -o main
```
报错如下：
```sh
-> % gcc main.out a.out -o main
a.out: In function `print':
a.c:(.text+0x0): multiple definition of `print'
main.out:main.c:(.text+0x0): first defined here
a.out: In function `a':
a.c:(.text+0x18): multiple definition of `a'
main.out:main.c:(.text+0x18): first defined here
collect2: error: ld returned 1 exit status
```

报错的原因是虽然头文件指定只编译一次，但每次编译时，都编译了一次，最后链接的时候自然就报错了。

解决方法一：头文件只声明，函数主体内容放到`.c/.cpp`中

解决方法二：头文件的函数加上关键字`inline`，直接把函数编译进去，自然不会冲突。