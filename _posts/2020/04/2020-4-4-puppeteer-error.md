---
layout: post
title: "puppeteer安装报错解决：Error: Cannot find module 'node_modules/puppeteer/install.js'"
categories: [行走的问题解决机]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

晚上用Node安装`puppeteer`时发生了报错：

```sh
-> # npm i puppeteer                                        

> puppeteer@2.1.1 install /root/youtube-spider/node_modules/puppeteer
> node install.js

internal/modules/cjs/loader.js:985
  throw err;
  ^

Error: Cannot find module '/root/youtube-spider/node_modules/puppeteer/install.js'
    at Function.Module._resolveFilename (internal/modules/cjs/loader.js:982:15)
    at Function.Module._load (internal/modules/cjs/loader.js:864:27)
    at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.js:74:12)
    at internal/main/run_main_module.js:18:47 {
  code: 'MODULE_NOT_FOUND',
  requireStack: []
}
npm WARN youtube-spider@1.0.0 No repository field.
npm WARN youtube-spider@1.0.0 No license field.

npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! puppeteer@2.1.1 install: `node install.js`
npm ERR! Exit status 1
npm ERR! 
npm ERR! Failed at the puppeteer@2.1.1 install script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2020-04-04T11_25_11_882Z-debug.log

```
```js
1159 verbose stack Error: puppeteer@2.1.1 install: `node install.js`
1159 verbose stack Exit status 1
1159 verbose stack     at EventEmitter.<anonymous> (/usr/lib/node_modules/npm/node_modules/npm-lifecycle/index.js:332:16)
1159 verbose stack     at EventEmitter.emit (events.js:311:20)
1159 verbose stack     at ChildProcess.<anonymous> (/usr/lib/node_modules/npm/node_modules/npm-lifecycle/lib/spawn.js:55:14)
1159 verbose stack     at ChildProcess.emit (events.js:311:20)
1159 verbose stack     at maybeClose (internal/child_process.js:1021:16)
1159 verbose stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:286:5
```
我起初怀疑是版本问题，尝试安装全局的库仍然失败：
```sh
-> # npm install puppeteer -g      

> puppeteer@2.1.1 install /usr/lib/node_modules/puppeteer
> node install.js

ERROR: Failed to download Chromium r722234! Set "PUPPETEER_SKIP_CHROMIUM_DOWNLOAD" env variable to skip download.
{ Error: EACCES: permission denied, mkdir '/usr/lib/node_modules/puppeteer/.local-chromium'
  -- ASYNC --
    at BrowserFetcher.<anonymous> (/usr/lib/node_modules/puppeteer/lib/helper.js:111:15)
    at Object.<anonymous> (/usr/lib/node_modules/puppeteer/install.js:66:16)
    at Module._compile (internal/modules/cjs/loader.js:778:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:789:10)
    at Module.load (internal/modules/cjs/loader.js:653:32)
    at tryModuleLoad (internal/modules/cjs/loader.js:593:12)
    at Function.Module._load (internal/modules/cjs/loader.js:585:3)
    at Function.Module.runMain (internal/modules/cjs/loader.js:831:12)
    at startup (internal/bootstrap/node.js:283:19)
    at bootstrapNodeJSCore (internal/bootstrap/node.js:623:3)
  errno: -13,
  code: 'EACCES',
  syscall: 'mkdir',
  path: '/usr/lib/node_modules/puppeteer/.local-chromium' }
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! puppeteer@2.1.1 install: `node install.js`
npm ERR! Exit status 1
npm ERR! 
npm ERR! Failed at the puppeteer@2.1.1 install script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2020-04-04T11_48_23_856Z-debug.log
```

然后我发现是权限的问题：
```sh
sudo npm install puppeteer
```
使用`sudo`就成功安装了。。。。