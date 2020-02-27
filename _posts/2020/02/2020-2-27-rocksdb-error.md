---
layout: post
title: "RocksDB 静态编译报错解决：rocksdb::UncompressBlockContentsForCompressionType"
categories: [行走的问题解决机]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

下午编译安装`RocksDB`后，在使用的时候报错如下：

```sh
--> % g++ -std=c++11 -o rocksdbtest test.cpp ./third/rocksdb/librocksdb.a   -lpthread
./third/rocksdb/librocksdb.a(format.o): In function `rocksdb::UncompressBlockContentsForCompressionType(char const*, unsigned long, rocksdb::BlockContents*, unsigned int, rocksdb::Slice const&, rocksdb::CompressionType, rocksdb::ImmutableCFOptions const&)':
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:783: undefined reference to `ZSTD_createDCtx'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:784: undefined reference to `ZSTD_decompress_usingDict'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:787: undefined reference to `ZSTD_freeDCtx'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:176: undefined reference to `snappy::GetUncompressedLength(char const*, unsigned long, unsigned long*)'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:319: undefined reference to `inflateInit2_'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:327: undefined reference to `inflateSetDictionary'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:345: undefined reference to `inflate'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:379: undefined reference to `inflateEnd'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:617: undefined reference to `LZ4_createStreamDecode'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:622: undefined reference to `LZ4_decompress_safe_continue'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:625: undefined reference to `LZ4_freeStreamDecode'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:470: undefined reference to `BZ2_bzDecompressInit'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:485: undefined reference to `BZ2_bzDecompress'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:517: undefined reference to `BZ2_bzDecompressEnd'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:617: undefined reference to `LZ4_createStreamDecode'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:622: undefined reference to `LZ4_decompress_safe_continue'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:625: undefined reference to `LZ4_freeStreamDecode'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:509: undefined reference to `BZ2_bzDecompressEnd'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:185: undefined reference to `snappy::RawUncompress(char const*, unsigned long, char*)'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:371: undefined reference to `inflateEnd'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:619: undefined reference to `LZ4_setStreamDecode'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:619: undefined reference to `LZ4_setStreamDecode'
./third/rocksdb/librocksdb.a(options_helper.o): In function `rocksdb::GetSupportedCompressions()':
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:88: undefined reference to `ZSTD_versionNumber'
./third/rocksdb/librocksdb.a(column_family.o): In function `rocksdb::CheckCompressionSupported(rocksdb::ColumnFamilyOptions const&)':
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:88: undefined reference to `ZSTD_versionNumber'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:88: undefined reference to `ZSTD_versionNumber'
./third/rocksdb/librocksdb.a(db_impl.o): In function `rocksdb::DBImpl::DBImpl(rocksdb::DBOptions const&, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&)':
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:88: undefined reference to `ZSTD_versionNumber'
./third/rocksdb/librocksdb.a(block_based_table_builder.o): In function `rocksdb::CompressBlock(rocksdb::Slice const&, rocksdb::CompressionOptions const&, rocksdb::CompressionType*, unsigned int, rocksdb::Slice const&, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >*)':
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:245: undefined reference to `deflateInit2_'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:253: undefined reference to `deflateSetDictionary'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:271: undefined reference to `deflate'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:257: undefined reference to `deflateEnd'
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
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:280: undefined reference to `deflateEnd'
/home/zhang/paxosstore/certain/third/rocksdb/./util/compression.h:559: undefined reference to `LZ4_loadDict'
collect2: error: ld returned 1 exit status

```
里面主要的报错是`librocksdb.a(format.o): In function rocksdb::UncompressBlockContentsForCompressionType(char const*, unsigned long, rocksdb::BlockContents*, unsigned int, rocksdb::Slice const&, rocksdb::CompressionType, rocksdb::ImmutableCFOptions const&)':
`


这个报错让人很烦，我的编译命令是：
```sh
g++ -std=c++11 -o rocksdbtest test.cpp ./third/rocksdb/librocksdb.a   -lpthread
```
解决方法是把缺失的库链接上：
```sh
g++ -std=c++11 -o rocksdbtest test.cpp ./third/rocksdb/librocksdb.a -lpthread -lsnappy  -lz -lbz2 -lzstd /usr/lib/x86_64-linux-gnu/liblz4.a
```
另一个解决方法是不使用静态链接库`librocksdb.a`，使用动态库：

```sh
g++ -std=c++11 -o rocksdbtest test.cpp -lrocksdb  -lpthread
```


