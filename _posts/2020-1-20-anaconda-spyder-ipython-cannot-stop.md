---
layout: post
title: "anaconda-spyder-ipython终端控制台无法停止问题解决"
categories: [行走的问题解决机]
description: "anaconda-spyder-ipython终端控制台无法停止问题解决"
keywords: Python, anaconda, spyder, ipython
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

自从我使用anaconda的spyder作为PythonD的IDE开始，经常会出现一种情况，那就是调试代码或者运行程序的时候，点击终止按钮`stop debugging`无法停止程序，这让我很苦恼。

后来我发现想要强行终止程序，可以直接关闭控制台`console 1/A`，新的控制台自然是全新的开始。

然后今天早上在调试我写的爬虫程序时又出现了这个问题，我突然就发现了问题所在。

按终止按钮无法暂停程序的原因是**程序捕获了CTRL+C发送的终止信号，并继续运行**

这个错误经常在我写的爬虫程序中出现，因为爬虫程序中存在许多`try...except...`，所以有时候程序就不会终止，接着运行。

所以检查代码中的`try...except...`也许就能找到无法终止程序的原因。