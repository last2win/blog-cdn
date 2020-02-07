---
layout: post
title: "nodejs报错解决：Error: Can only perform operation while paused. - undefined"
categories: [行走的问题解决机]
description: "nodejs报错解决：Error: Can only perform operation while paused. - undefined"
keywords: nodejs
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

下午在用nodejs进行调试时遇到报错：
```js
debug> n
Thrown:
Error: Can only perform operation while paused. - undefined
    at _pending.<computed> (internal/deps/node-inspect/lib/internal/inspect_client.js:243:27)
    at Client._handleChunk (internal/deps/node-inspect/lib/internal/inspect_client.js:213:11)
    at Socket.emit (events.js:223:5)
    at Socket.EventEmitter.emit (domain.js:475:20)
    at addChunk (_stream_readable.js:309:12)
    at readableAddChunk (_stream_readable.js:290:11)
    at Socket.Readable.push (_stream_readable.js:224:10)
    at TCP.onStreamRead (internal/stream_base_commons.js:181:23) {
  code: -32000
}
break in search.js:34

```
这个报错是因为程序还在等待：`await sleep(1000);`，这时候自然不能运行下一步。


