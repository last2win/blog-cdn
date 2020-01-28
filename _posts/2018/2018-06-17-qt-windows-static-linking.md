---
layout: post
title: Qt5.10.1在Windows平台下进行静态编译
categories: [Windows]
description: "花费了一下午的时间进行QT的静态编译 QT,windows,static linking vs2017"
keywords: QT,windows,static linking
---
Qt静态编译的最大好处就是可以直接产生能够不依靠多余的dll运行的exe文件，
而且exe文件会比动态编译的要小上很多。

# 1.下载源码
静态编译的第一步是下载QT的源码，根据你要编译的版本选择QT的版本进行下载。
我要编译5.10.1版本，所以我在http://download.qt.io/archive/qt/5.10/5.10.1/single/
上下载了http://download.qt.io/archive/qt/5.10/5.10.1/single/qt-everywhere-src-5.10.1.zip
压缩包大小为638M，解压后有1.9G

# 2.准备工作
解压后，找到源码里的qtbase\mkspecs\common\msvc-desktop.conf这个文件，
然后把  
```C++
QMAKE_CFLAGS_RELEASE    = $$QMAKE_CFLAGS_OPTIMIZE -MD
QMAKE_CFLAGS_RELEASE_WITH_DEBUGINFO += $$QMAKE_CFLAGS_OPTIMIZE -MD -Zi
QMAKE_CFLAGS_DEBUG      = -Zi -MDd
```
修改为  
```C++
QMAKE_CFLAGS_RELEASE    = $$QMAKE_CFLAGS_OPTIMIZE -MT
QMAKE_CFLAGS_RELEASE_WITH_DEBUGINFO += $$QMAKE_CFLAGS_OPTIMIZE -MT -Zi
QMAKE_CFLAGS_DEBUG      = -Zi -MTd
```
然后在cmd里执行命令：
其中的2个目录要更换，第一个要更换为你的源码的位置，第二个目录要更换为要生成的static的目录位置
C:\Qt\Qt5.10.1\5.10.1\qt-everywhere-src-5.10.1\configure -confirm-license -opensource -platform win32-g++ -debug-and-release -static -static-runtime -force-debug-info -prefix "C:\Qt\Qt5.10.1\5.10.1\mingw53_32_static" -qt-sqlite -qt-pcre -qt-zlib -qt-libpng -qt-libjpeg -opengl desktop -qt-freetype -nomake tools -nomake tests -no-compile-examples -nomake examples
等待大约几分钟，命令执行完成。
# 3.编译
运行命令：mingw32-make -j4
其中的4指的是4线程编译，改大会更快，改小会电脑不卡。
我的CPU是i7-6700HQ,花了大约2个小时编译完成。
最后运行命令： mingw32-make install
大约花费
# 4.配置

## 参考链接
*   [VS2015 一键编译 QT5.10.1 X64位 静态库 MT](https://blog.csdn.net/dbyoung/article/details/79793746)
*   [Qt5.8 在windows下mingw静态编译](http://www.cnblogs.com/ike_li/p/6860089.html)