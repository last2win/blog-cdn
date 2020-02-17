---
layout: post
title: "自动登录谷歌账号失败解决：fail to auto login Google Account in Chrome "
categories: ["node.js"]
description: "fail to auto login Google Account in Chrome 自动登录谷歌账号失败解决"
keywords: node
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

早上在用`puppeteer`操作`chrome`浏览器，想要登录谷歌账号，报错：

> 此浏览器或应用可能不安全。           
> 请尝试使用其他浏览器。如果您使用的是受支持的浏览器，则可以刷新屏幕，然后重新尝试登录。

> This browser or app may not be secure. Learn more           
> Try using a different browser. If you’re already using a supported browser, you can refresh your screen and try again to sign in.

谷歌检测到在自动化操作浏览器时会不允许登录谷歌账号，GitHub上有人讨论：["Couldn't sign you in" Google account login fails in headless mode in Google Cloud Functions · Issue #4871 · puppeteer/puppeteer](https://github.com/puppeteer/puppeteer/issues/4871)

我尝试修改`user-agent`，无效。

我也尝试使用`puppeteer-extra`和`puppeteer-extra-plugin-stealth`，也无效。

无论是有头(non-headless)还是无头(headless)，都无法解决问题。

最后的解决方法就是手动登录Google账号，`puppeteer`安装的chrome默认目录：
`node_modules/puppeteer/.local-chromium/win64-706915/chrome-win/chrome.exe`

手动打开chrome，然后登录谷歌账号，问题解决。

后记：在自动化操作chrome时，需要指定chrome的`userDataDir`目录，不然本地没有cache，就要重新登录。

打开`chrome://version`，查看`Profile Path`:`C:\Users\peter\AppData\Local\Chromium\User Data\Default`, 删除 `Default`， 剩下的路径是 user_Data_Dir: `C:\Users\peter\AppData\Local\Chromium\User Data`

参考：[User Data Directory](https://chromium.googlesource.com/chromium/src/+/master/docs/user_data_dir.md)








