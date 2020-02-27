---
layout: post
title: "IDA Pro-IDAPython 安装和使用"
categories: [安全]
description: "IDA Pro-IDAPython 安装和使用"
keywords: IDAPython
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

最近想用`IDAPython`来做软件的逆向，需要先安装`IDA Pro 7.0`,推荐到吾爱破解论坛下载：[爱盘 - 最新的在线破解工具包](https://down.52pojie.cn/Tools/Disassemblers/)

安装好`IDA`后，Python的库已经安装好了，需要手动安装`Python 2.7`

1.从 http://www.python.org/ 安装 Python 2.7

2.将安装的python这个文件夹，复制到IDA 的安装目录下（默认为C:\Program Files\IDA）

3.`IDA Pro 7.0`自带了`IDAPython`的库，重启`IDA`后随便打开一个exe，终端中会显示如下：

```sh
---------------------------------------------------------------------------------------
Python 2.7.17 (v2.7.17:c2f86d86e6, Oct 19 2019, 21:01:17) [MSC v.1500 64 bit (AMD64)] 
IDAPython v1.7.0 final (serial 0) (c) The IDAPython Team <idapython@googlegroups.com>
---------------------------------------------------------------------------------------
```

