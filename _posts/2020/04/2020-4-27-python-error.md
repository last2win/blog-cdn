---
layout: post
title: "Python报错解决：Process finished with exit code -1073741571 (0xC00000FD)和RecursionError: maximum recursion depth exceeded in comparison"
categories: [行走的问题解决机]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

早上写代码时遇到了报错：
```sh
  [Previous line repeated 2974 more times]
    trade_flag = ((now_price - avg) / avg) > LONG_RATE
RecursionError: maximum recursion depth exceeded in comparison
```
```sh
Process finished with exit code -1073741571 (0xC00000FD)
```
根据网上的资料，这个报错是因为函数的递归太深了。

我查看了代码，确实有一段代码写的有问题，递归太深了。

把递归改为迭代就可以了。