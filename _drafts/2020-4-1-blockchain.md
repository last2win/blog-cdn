---
layout: post
title: "区块链发币"
categories: [区块链]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

**警告：发代币进行ICO集资是违法行为！！**

大约在2016年时就看到了文章说只要花10元，就可以在区块链上发属于自己的虚拟货币。当时一直没实践，后来就一直没动静。

最近因为比特币在几年3月13号因为美股股灾跌到低位￥27,000每个，于是想起了自己在区块链上发币的这个操作。

现在打算实际演练一下。

一般来说发币是发在以太坊上，因为以太坊可以执行智能合约。而以太坊的运行开销是ETH，所以使用ETH作为币的初始定价。

根据我在网上看到的许多文章，都推荐使用Remix和MetaMask(可能大家的文章都是抄来抄去的)。

`MetaMask`钱包：[MetaMask Download](https://metamask.io/download.html)

不推荐使用以太坊本地钱包，要外网：[Releases · ethereum/mist](https://github.com/ethereum/mist/releases)






以太坊已经有代币的标准`ERC-20`:[EIP 20: ERC-20 Token Standard](https://eips.ethereum.org/EIPS/eip-20)

开源实现：[openzeppelin-contracts/ERC20.sol  OpenZeppelin/openzeppelin-contracts](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol)



`solidity`在线IDE:[Remix - Ethereum IDE](https://remix.ethereum.org/)



```java

```







**警告：发代币进行ICO集资是违法行为！！**









{% raw %}
***          
{% endraw %}

如果在没有外网的情况下使用mist，会报错如下：
```json
[2020-03-28T21:41:00.692] [INFO] (ui: mist) - Meteor starting up...
[2020-03-28T21:41:00.695] [INFO] (ui: mist) - Initialize Mist Interface
[2020-03-28T21:41:00.745] [INFO] (ui: popupWindow) - Meteor starting up...
[2020-03-28T21:41:08.150] [WARN] EthereumNodeRemote - Error from ws:  Error: Unexpected server response: 403
    at ClientRequest.req.on (C:\Program Files\Mist\resources\app.asar\node_modules\ws\lib\websocket.js:542:5)
    at emitOne (events.js:115:13)
    at ClientRequest.emit (events.js:210:7)
    at HTTPParser.parserOnIncomingClient [as onIncoming] (_http_client.js:565:21)
    at HTTPParser.parserOnHeadersComplete (_http_common.js:116:23)
    at TLSSocket.socketOnData (_http_client.js:454:20)
    at emitOne (events.js:115:13)
    at TLSSocket.emit (events.js:210:7)
    at addChunk (_stream_readable.js:252:12)
    at readableAddChunk (_stream_readable.js:239:11)
    at TLSSocket.Readable.push (_stream_readable.js:197:10)
    at TLSWrap.onread (net.js:589:20)
[2020-03-28T21:41:08.151] [WARN] EthereumNodeRemote - Remote WebSocket connection closed (code: 1006). Reopening connection...
[2020-03-28T21:42:55.576] [INFO] Windows - Create popup window: updateAvailable
[2020-03-28T21:42:55.577] [INFO] Windows - Create secondary window: updateAvailable, owner: notset
[2020-03-28T21:42:59.105] [INFO] (ui: popupWindow) - Meteor starting up...
[2020-03-28T21:42:59.206] [INFO] updateChecker - Check for update...
[2020-03-28T21:43:00.189] [INFO] updateChecker - App is up-to-date.
[2020-03-28T21:43:00.190] [ERROR] updateChecker - TypeError: Error processing argument at index 1, conversion failure from #<Object>
    at WebContents.send (C:\Program Files\Mist\resources\electron.asar\browser\api\web-contents.js:97:15)
    at Window.send (C:\Program Files\Mist\resources\app.asar\modules\windows.js:243:27)
    at check.then.update (C:\Program Files\Mist\resources\app.asar\modules\updateChecker.js:84:11)
    at <anonymous>
    at process._tickCallback (internal/process/next_tick.js:188:7)
[2020-03-28T21:43:14.048] [INFO] Windows - All primary windows closed/invisible, so quitting app...
[2020-03-28T21:43:14.049] [INFO] main - Defer quitting until sockets and node are shut down
[2020-03-28T21:43:14.049] [INFO] Sockets - Destroy all sockets
[2020-03-28T21:43:14.049] [INFO] Sockets/2 - Disconnecting...
[2020-03-28T21:43:14.051] [INFO] Sockets/3 - Disconnecting...
[2020-03-28T21:43:14.052] [INFO] Sockets/node-ipc - Disconnecting...
[2020-03-28T21:43:14.052] [INFO] main - Defer quitting until sockets and node are shut down
[2020-03-28T21:43:14.053] [INFO] Sockets - Destroy all sockets
[2020-03-28T21:43:14.059] [ERROR] main - Error shutting down sockets
[2020-03-28T21:43:14.561] [INFO] main - About to quit...
[2020-03-28T21:43:14.587] [INFO] Windows - All primary windows closed/invisible, so quitting app...
[2020-03-28T21:43:19.474] [INFO] Settings - Running in production mode: true
[2020-03-28T21:43:19.525] [INFO] EthereumNode - undefined null 'light'
[2020-03-28T21:43:19.527] [INFO] EthereumNode - Defaults loaded: geth main light
[2020-03-28T21:43:19.651] [INFO] main - Starting in Mist mode
[2020-03-28T21:43:19.725] [INFO] Db - Loading db: C:\Users\peter\AppData\Roaming\Mist\mist.lokidb
[2020-03-28T21:43:19.745] [INFO] Windows - Creating commonly-used windows
[2020-03-28T21:43:19.746] [INFO] Windows - Create secondary window: loading, owner: notset
[2020-03-28T21:43:19.926] [INFO] updateChecker - Check for update...
[2020-03-28T21:43:26.147] [INFO] Sockets/node-ipc - Connect to {"path":"\\\\.\\pipe\\geth.ipc"}
[2020-03-28T21:43:27.371] [INFO] Windows - Create primary window: main, owner: notset
[2020-03-28T21:43:27.421] [INFO] ClientBinaryManager - Initializing...
[2020-03-28T21:43:27.422] [INFO] ClientBinaryManager - Checking for new client binaries config from: https://raw.githubusercontent.com/ethereum/mist/master/clientBinaries.json
[2020-03-28T21:43:27.447] [INFO] main - Loading Interface at file://C:\Program Files\Mist\resources\app.asar/interface/index.html
[2020-03-28T21:43:27.642] [WARN] Sockets/node-ipc - Connection failed, retrying after 1000ms...
[2020-03-28T21:43:27.685] [INFO] updateChecker - App is up-to-date.
[2020-03-28T21:43:27.688] [INFO] ClientBinaryManager - No "skippedNodeVersion.json" found.
[2020-03-28T21:43:27.690] [INFO] ClientBinaryManager - Initializing...
[2020-03-28T21:43:27.690] [INFO] ClientBinaryManager - Resolving platform...
[2020-03-28T21:43:27.691] [INFO] ClientBinaryManager - Calculating possible clients...
[2020-03-28T21:43:27.692] [INFO] ClientBinaryManager - 1 possible clients.
[2020-03-28T21:43:27.693] [INFO] ClientBinaryManager - Verifying status of all 1 possible clients...
[2020-03-28T21:43:27.694] [INFO] ClientBinaryManager - Verify Geth status ...
[2020-03-28T21:43:28.204] [ERROR] ClientBinaryManager - Unable to resolve Geth executable: geth.exe
[2020-03-28T21:43:28.208] [INFO] ClientBinaryManager - Download binary for Geth ...
[2020-03-28T21:43:28.210] [INFO] ClientBinaryManager - Downloading package from https://gethstore.blob.core.windows.net/builds/geth-windows-amd64-1.8.23-c9427004.zip to C:\Users\peter\AppData\Roaming\Mist\binaries\Geth\archive.zip ...
[2020-03-28T21:43:28.644] [WARN] Sockets/node-ipc - Connection failed, retrying after 1000ms...
[2020-03-28T21:43:29.644] [WARN] Sockets/node-ipc - Connection failed, retrying after 1000ms...
[2020-03-28T21:43:30.635] [ERROR] Sockets/node-ipc - Connection failed (3000ms elapsed)
[2020-03-28T21:43:30.637] [WARN] EthereumNode - Failed to connect to an existing local node. Starting our own...
[2020-03-28T21:43:30.637] [INFO] EthereumNode - Node type: geth
[2020-03-28T21:43:30.637] [INFO] EthereumNode - Network: main
[2020-03-28T21:43:30.638] [INFO] EthereumNode - SyncMode: light
[2020-03-28T21:43:30.638] [INFO] EthereumNode - Start node: geth main light
[2020-03-28T21:43:30.643] [ERROR] EthereumNode - Failed to start node Error: Node "geth" binPath is not available.
    at EthereumNode.__startNode (C:\Program Files\Mist\resources\app.asar\modules\ethereumNode.js:374:13)
    at stop.then (C:\Program Files\Mist\resources\app.asar\modules\ethereumNode.js:300:19)
    at tryCatcher (C:\Program Files\Mist\resources\app.asar\node_modules\bluebird\js\release\util.js:16:23)
    at Promise._settlePromiseFromHandler (C:\Program Files\Mist\resources\app.asar\node_modules\bluebird\js\release\promise.js:512:31)
    at Promise._settlePromise (C:\Program Files\Mist\resources\app.asar\node_modules\bluebird\js\release\promise.js:569:18)
    at Promise._settlePromise0 (C:\Program Files\Mist\resources\app.asar\node_modules\bluebird\js\release\promise.js:614:10)
    at Promise._settlePromises (C:\Program Files\Mist\resources\app.asar\node_modules\bluebird\js\release\promise.js:693:18)
    at Async._drainQueue (C:\Program Files\Mist\resources\app.asar\node_modules\bluebird\js\release\async.js:133:16)
    at Async._drainQueues (C:\Program Files\Mist\resources\app.asar\node_modules\bluebird\js\release\async.js:143:10)
    at Immediate.Async.drainQueues (C:\Program Files\Mist\resources\app.asar\node_modules\bluebird\js\release\async.js:17:14)
    at runCallback (timers.js:781:20)
    at tryOnImmediate (timers.js:743:5)
    at processImmediate [as _immediateCallback] (timers.js:714:5)
[2020-03-28T21:43:32.022] [INFO] (ui: popupWindow) - Meteor starting up...
[2020-03-28T21:43:34.368] [INFO] (ui: mist) - Meteor starting up...
[2020-03-28T21:43:34.374] [INFO] (ui: mist) - Initialize Mist Interface
[2020-03-28T21:43:34.374] [INFO] (ui: popupWindow) - Meteor starting up...
```