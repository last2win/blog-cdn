---
layout: post
title: "nodejs报错解决： EBUSY: resource busy or locked 和 npm ERR! errno -4082"
categories: [行走的问题解决机]
description: "nodejs报错解决： EBUSY: resource busy or locked 和 npm ERR! errno -4082"
keywords: nodejs
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

早上在用nodejs安装包的时候报错如下：
```sh
C:\Users\Documents\GitHub\youtube-upload-multi-videos>npm install puppeteer-extra puppeteer-extra-plugin-stealth


npm ERR! code EBUSY
npm ERR! syscall rmdir
npm ERR! path C:\Users\Documents\GitHub\youtube-upload-multi-videos\node_modules\puppeteer
npm ERR! errno -4082
npm ERR! EBUSY: resource busy or locked, rmdir 'C:\Users\peter\Documents\GitHub\youtube-upload-multi-videos\node_modules\puppeteer'

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\AppData\Roaming\npm-cache\_logs\2020-02-17T01_38_46_184Z-debug.log
```
里面提到报错`npm ERR! errno -4082`和`npm ERR! EBUSY: resource busy or locked, rmdir `

显示的错误很明显了，就是无法删除目录。

网上有很多人遇到了这个报错：[npm Error : EBUSY resource busy or locked · Issue #13461 · npm/npm](https://github.com/npm/npm/issues/13461)            

一个解决方法是先卸载，再安装：
```sh
npm cache clean --force
```

另一个解决方法是关闭杀毒软件，有可能是杀毒软件造成的问题。

如果还不行，可以尝试手动删除目录`node_modules`，删除后可以正常安装。

无法删除文件夹的话，查看哪个程序占用了文件。

windows系统可以使用`LockHunter`查看锁住文件的进程，然后到任务管理器中进行关闭。

实在不行，可以重启电脑尝试解决。