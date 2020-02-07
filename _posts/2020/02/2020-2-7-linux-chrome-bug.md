---
layout: post
title: "Linux/ubuntu:Chrome报错解决： error while loading shared libraries: libnss3.so  libXss.so.1 libasound.so.2"
categories: [行走的问题解决机]
description: "Linux/ubuntu:Chrome报错解决： error while loading shared libraries: libnss3.so  libXss.so.1 libasound.so.2"
keywords: linux
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

下午在用nodejs在linux上操作`puppeteer/chromium/chrome`时报错如下：

```sh
-> # node search.js 
count is 1
(node:15360) UnhandledPromiseRejectionWarning: Error: Failed to launch the browser process!
/root/youtube/search/node_modules/puppeteer/.local-chromium/linux-722234/chrome-linux/chrome: error while loading shared libraries: libnss3.so: cannot open shared object file: No such file or directory


TROUBLESHOOTING: https://github.com/puppeteer/puppeteer/blob/master/docs/troubleshooting.md

    at onClose (/root/youtube/search/node_modules/puppeteer/lib/Launcher.js:750:14)
    at Interface.<anonymous> (/root/youtube/search/node_modules/puppeteer/lib/Launcher.js:739:50)
    at Interface.emit (events.js:228:7)
    at Interface.close (readline.js:402:8)
    at Socket.onend (readline.js:180:10)
    at Socket.emit (events.js:228:7)
    at endReadableNT (_stream_readable.js:1185:12)
    at processTicksAndRejections (internal/process/task_queues.js:81:21)
(node:15360) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .catch(). (rejection id: 1)
(node:15360) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.
```

解决方法如下：
```sh
apt install libnss3-dev
```

{% raw %}
***          
{% endraw %}

又遇到报错：
```sh
-> # node search.js         
count is 1
(node:15488) UnhandledPromiseRejectionWarning: Error: Failed to launch the browser process!
/root/youtube/search/node_modules/puppeteer/.local-chromium/linux-722234/chrome-linux/chrome: error while loading shared libraries: libXss.so.1: cannot open shared object file: No such file or directory


TROUBLESHOOTING: https://github.com/puppeteer/puppeteer/blob/master/docs/troubleshooting.md

    at onClose (/root/youtube/search/node_modules/puppeteer/lib/Launcher.js:750:14)
    at Interface.<anonymous> (/root/youtube/search/node_modules/puppeteer/lib/Launcher.js:739:50)
    at Interface.emit (events.js:228:7)
    at Interface.close (readline.js:402:8)
    at Socket.onend (readline.js:180:10)
    at Socket.emit (events.js:228:7)
    at endReadableNT (_stream_readable.js:1185:12)
    at processTicksAndRejections (internal/process/task_queues.js:81:21)
(node:15488) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .catch(). (rejection id: 1)
(node:15488) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.

```

解决方法如下：
```sh
apt-get install libxss1
```

{% raw %}
***          
{% endraw %}
又遇到报错：
```sh
-> # node search.js         
count is 1
(node:15601) UnhandledPromiseRejectionWarning: Error: Failed to launch the browser process!
/root/youtube/search/node_modules/puppeteer/.local-chromium/linux-722234/chrome-linux/chrome: error while loading shared libraries: libasound.so.2: cannot open shared object file: No such file or directory


TROUBLESHOOTING: https://github.com/puppeteer/puppeteer/blob/master/docs/troubleshooting.md

    at onClose (/root/youtube/search/node_modules/puppeteer/lib/Launcher.js:750:14)
    at Interface.<anonymous> (/root/youtube/search/node_modules/puppeteer/lib/Launcher.js:739:50)
    at Interface.emit (events.js:228:7)
    at Interface.close (readline.js:402:8)
    at Socket.onend (readline.js:180:10)
    at Socket.emit (events.js:228:7)
    at endReadableNT (_stream_readable.js:1185:12)
    at processTicksAndRejections (internal/process/task_queues.js:81:21)
(node:15601) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .catch(). (rejection id: 1)
(node:15601) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.

```

解决方法如下：
```sh
apt-get install libasound2
```



