---
layout: post
title: "paxos企业级框架：PaxosStore 编译与报错解决"
categories: [分布式]
description: "PaxosStore 的编译-grpc 静态编译/static link-运行 PaxosStore"
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

# phxpaxos与PaxosStore
说道分布式算法，一定会提及paxos，而paxos的框架轮子并没有多少。

在GitHub上的paxos实现，最有名的2个是腾讯开源的`PaxosStore`和`phxpaxos`，这2个库都在微信用过。

其中`phxpaxos`这个库2年没更新了，`PaxosStore`这个库2019年还更新过，因此我选择`PaxosStore`。

PaxosStore 项目地址：[Tencent/paxosstore: PaxosStore has been deployed in WeChat production for more than two years, providing storage services for the core businesses of WeChat backend. Now PaxosStore is running on thousands of machines, and is able to afford billions of peak TPS.](https://github.com/Tencent/paxosstore)
# PaxosStore 的编译
下载：
```sh
git clone https://github.com/Tencent/paxosstore.git
cd paxosstore
git submodule update --init --recursive
```
把需要的第三方库都下载好后，总大小大约1G，其中主要占空间的是`grpc`

编译前的准备工作，安装有用的库：
```sh
apt install zlib1g-dev libssl-dev  libpcap-dev libelf-dev libicu-dev libreadline-dev libtool  libsysfs-dev \
libsqlite3-dev libbz2-dev libreadline-dev zlib1g-dev liblzma-dev liblzo2-dev   \
libsnappy-dev  liblz4-dev libzstd-dev libgflags-dev  libprotoc-dev
```

## PaxosStore-certain 编译
进入目录`certain`:
```sh
cd certain
```

### grpc 静态编译/static link
如果直接进行编译，在编译过程中会报很多`grpc`相关的错误，因此首先自己把grpc编译好。

grpc静态编译：
```sh
cd third/grpc
export CXXFLAGS='-Wno-expansion-to-defined -Wno-implicit-fallthrough'
make REQUIRE_CUSTOM_LIBRARIES_opt=1 static
```

### 编译
然后在`network/IOChannel.cpp`添加头文件：
```c
#include<sys/uio.h>
```
修改`Makefile`，把第40行修改为如下：
```sh
DLIBS = -pthread -ldl -lsnappy  -lz -lbz2 -lzstd /usr/lib/x86_64-linux-gnu/liblz4.a
```
注意最后的`liblz4.a`要跟你机器上的文件位置一样。

编译：
```sh
export CFLAGS='-Wno-implicit-fallthrough'
./build.sh example
```
编译成功！！

### 运行 PaxosStore-certain
在本地运行3个服务，从而形成分布式
```sh
mkdir /tmp/certain
./card_srv -c example/example.conf -p /tmp/certain -i 0 &
./card_srv -c example/example.conf -p /tmp/certain -i 1 &
./card_srv -c example/example.conf -p /tmp/certain -i 2 &
```
测试：
```sh
-> % ./card_tool -X 127.0.0.1:50050 -o Insert -i 12358 -n rock -u 20170001 -b 200
Done
-> % ./card_tool -X 127.0.0.1:50050 -o Select -i 12358
user_name=rock user_id=20170001 balance=200
Done 
-> % ./card_tool -X 127.0.0.1:50050 -o Update -i 12358 -d 10
balance=210
Done 
-> % ./card_tool -X 127.0.0.1:50050 -o Delete -i 12358
Done
-> % ./card_tool -X 127.0.0.1:50050 -o Select -i 12358      
Failure with error: code(8001) msg(card not exist)
```
运行成功！！

## PaxosStore-paxoskv 编译

```sh
git submodule update --init
cd paxoskv/
```

### 编译安装 leveldb
该项目需要`leveldb`，直接编译太麻烦，我选择安装`leveldb`
```sh
apt install libleveldb-dev
```

### 编译 paxoskv
修改文件`paxoskv/cutils/cqueue.h`，添加头文件：`#include <functional>`

```
mkdir build
cd build
export CXXFLAGS='-std=c++11'
export CFLAGS='-std=c++11'
cmake ..
make 
```

测试：
```sh
./example/membase_paxoskv
```


# 后记

如果没有静态编译，而采用了脚本中的`grpc`编译，会报错如下：
```sh
./third/grpc/bins/opt/grpc_cpp_plugin: program not found or is not executable
--grpc_out: protoc-gen-grpc: Plugin failed with status code 1.
Makefile:95: recipe for target 'example/example.grpc.pb.cc' failed
make: *** [example/example.grpc.pb.cc] Error 1
make: *** Waiting for unfinished jobs....
In file included from example/Client.cpp:1:0:
./example/Client.h:6:10: fatal error: example.grpc.pb.h: No such file or directory
 #include "example.grpc.pb.h"
          ^~~~~~~~~~~~~~~~~~~
compilation terminated.
Makefile:104: recipe for target 'example/Client.o' failed
make: *** [example/Client.o] Error 1
rm src/Certain.pb.cc
```


如果没有加上链接参数`-lsnappy  -lz -lbz2 -lzstd /usr/lib/x86_64-linux-gnu/liblz4.a`，编译过程中会报错如下：

```sh
g++ -static-libgcc -static-libstdc++ -std=c++11 example/example.pb.o example/example.grpc.pb.o example/Client.o example/Coding.o example/DBImpl.o example/PLogImpl.o example/CertainUserImpl.o example/UserWorker.o example/ServiceImpl.o example/UUIDGenerator.o example/TemporaryTable.o example/CoHashLock.o example/BenchTool.o libcertain.a -o bench_tool ./third/protobuf/src/.libs/libprotobuf.a ./third/libco/lib/libcolib.a ./third/rocksdb/librocksdb.a ./third/grpc/libs/opt/libares.a ./third/grpc/libs/opt/libboringssl.a ./third/grpc/libs/opt/libgpr.a ./third/grpc/libs/opt/libgrpc.a ./third/grpc/libs/opt/libgrpc++.a ./third/grpc/libs/opt/libgrpc++_core_stats.a ./third/grpc/libs/opt/libgrpc++_cronet.a ./third/grpc/libs/opt/libgrpc_cronet.a ./third/grpc/libs/opt/libgrpc++_error_details.a ./third/grpc/libs/opt/libgrpc_plugin_support.a ./third/grpc/libs/opt/libgrpc_unsecure.a ./third/grpc/libs/opt/libgrpc++_unsecure.a ./third/grpc/libs/opt/libz.a -Wl,--no-as-needed ./third/grpc/libs/opt/libgrpc++_reflection.a -Wl,--as-needed -pthread -ldl


/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:746: undefined reference to `ZSTD_compressBound'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:750: undefined reference to `ZSTD_createCCtx'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:751: undefined reference to `ZSTD_compress_usingDict'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:754: undefined reference to `ZSTD_freeCCtx'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:163: undefined reference to `snappy::MaxCompressedLength(unsigned long)'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:165: undefined reference to `snappy::RawCompress(char const*, unsigned long, char*, unsigned long*)'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:415: undefined reference to `BZ2_bzCompressInit'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:429: undefined reference to `BZ2_bzCompress'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:438: undefined reference to `BZ2_bzCompressEnd'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:552: undefined reference to `LZ4_compressBound'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:557: undefined reference to `LZ4_createStream'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:563: undefined reference to `LZ4_compress_fast_continue'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:571: undefined reference to `LZ4_freeStream'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:671: undefined reference to `LZ4_compressBound'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:676: undefined reference to `LZ4_createStreamHC'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:677: undefined reference to `LZ4_resetStreamHC'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:681: undefined reference to `LZ4_loadDictHC'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:685: undefined reference to `LZ4_compress_HC_continue'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:693: undefined reference to `LZ4_freeStreamHC'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:438: undefined reference to `BZ2_bzCompressEnd'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:559: undefined reference to `LZ4_loadDict'
collect2: error: ld returned 1 exit status
Makefile:142: recipe for target 'db_tool' failed
make: *** [db_tool] Error 1
```

如果不安装leveldb，会报错：
```sh
-- LevelDB: LEVELDB_INCLUDE_DIR-NOTFOUND LEVELDB_LIBRARY-NOTFOUND
CMake Error: The following variables are used in this project, but they are set to NOTFOUND.
Please set them or make sure they are set and tested correctly in the CMake files:
LEVELDB_INCLUDE_DIR (ADVANCED)
   used as include directory in directory /home/zhang/paxosstore/paxoskv
   used as include directory in directory /home/zhang/paxosstore/paxoskv
   used as include directory in directory /home/zhang/paxosstore/paxoskv
   used as include directory in directory /home/zhang/paxosstore/paxoskv
   used as include directory in directory /home/zhang/paxosstore/paxoskv
   used as include directory in directory /home/zhang/paxosstore/paxoskv

```
如果不添加头文件，会报错如下：
```sh
In file included from /home/zhang/paxosstore/paxoskv/memkv/memloader.cc:25:0:
/home/zhang/paxosstore/paxoskv/cutils/cqueue.h:181:14: error: ‘std::function’ has not been declared
         std::function<bool(const std::unique_ptr<EntryType>&)> pred) {
              ^~~~~~~~
/home/zhang/paxosstore/paxoskv/cutils/cqueue.h:181:22: error: expected ‘,’ or ‘...’ before ‘<’ token
         std::function<bool(const std::unique_ptr<EntryType>&)> pred) {
                      ^
memkv/CMakeFiles/paxoskv_memkv.dir/build.make:254: recipe for target 'memkv/CMakeFiles/paxoskv_memkv.dir/memloader.cc.o' failed
make[2]: *** [memkv/CMakeFiles/paxoskv_memkv.dir/memloader.cc.o] Error 1
CMakeFiles/Makefile2:1223: recipe for target 'memkv/CMakeFiles/paxoskv_memkv.dir/all' failed
make[1]: *** [memkv/CMakeFiles/paxoskv_memkv.dir/all] Error 2
Makefile:129: recipe for target 'all' failed
make: *** [all] Error 2
```



参考：

- 我之前写的博客：[一次失败的尝试：paxosstore示例编译_网络_zhang0peter的博客-CSDN博客](https://zhang0peter.blog.csdn.net/article/details/97614728)

- [Build static library of gRPC  Nan Xiao's Blog](https://nanxiao.me/en/build-static-library-of-grpc/)








