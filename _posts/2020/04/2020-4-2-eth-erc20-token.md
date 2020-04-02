---
layout: post
title: "5分钟在以太坊区块链上发行符合ERC-20标准的代币"
categories: [区块链]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

大约在2016年时就看到了文章介绍只要花10元，就可以在区块链上发属于自己的虚拟货币。当时一直没实践。

最近因为比特币在今年3月13号因为美股股灾跌到低位￥27,000每个，于是想起了自己在区块链上发币的这个操作。

现在打算实际演练一下。

一般来说发币是发在以太坊上，因为以太坊可以执行智能合约。而以太坊的运行开销是ETH，所以需要使用ETH钱包。

## ETH 钱包

根据我在网上看到的许多文章，都推荐使用MetaMask钱包。

`MetaMask`钱包：[MetaMask Download](https://metamask.io/download.html)

不推荐使用以太坊本地钱包，要外网：[Releases · ethereum/mist](https://github.com/ethereum/mist/releases)

## ERC-20实现代码

以太坊有代币的标准`ERC-20`:[EIP 20: ERC-20 Token Standard](https://eips.ethereum.org/EIPS/eip-20)


我参考了`OpenZeppelin`的`ERC20`的实现，代码放在仓库中：[ETH ERC20 代码示例](https://github.com/zhang0peter/ERC-20)


代码如下：
```js
pragma solidity ^0.5.0;

import "https://github.com/zhang0peter/ERC-20/blob/master/Context.sol";
import "https://github.com/zhang0peter/ERC-20/blob/master/ERC20.sol";
import "https://github.com/zhang0peter/ERC-20/blob/master/ERC20Detailed.sol";

contract SimpleToken is Context, ERC20, ERC20Detailed {
    //Zhang0PeterCoin： 代币的全名
    //ZPC：代币的简写
    //3: 代币小数点位数，代币的最小单位， 3表示我们可以拥有 0.001单位个代币
    constructor () public ERC20Detailed("Zhang0PeterCoin", "ZPC", 3) {
        //初始化币，并把所有的币都给部署智能合约的ETH钱包地址
        //233：代币的总数量
        _mint(_msgSender(), 2333 * (10 ** uint256(decimals())));
    }
}
```



## 编译和部署

先说明一下以太坊的运行逻辑：智能合约是一段代码，可以部署到以太坊上，部署完成后可以运行代码。部署智能合约和运行代码都是有开销的，需要花钱，所以需要一个以太坊钱包的地址来付钱。

使用`solidity`在线IDE进行编译和部署:[Remix - Ethereum IDE](https://remix.ethereum.org/)

把代码放到IDE中，进行编译，部署。

默认的部署环境是`Javascript VM`，也就是本地测试环境，可以随便部署，测试，建议本地测试成功后再部署到线上。

在测试环境中，默认拥有5个账号，每个账号拥有100 ether。

部署完成后，左下角会显示已经部署的智能合约，可以使用`balanceOf`函数查看自己拥有的钱，钱的地址是部署合约时的钱包地址。

现在测试转账功能，给另外的钱包转账，转账完成后建议查看是否到账。

测试成功后部署到以太坊主网络，选择环境为`Injected Web3`，并往`MetaMask`钱包中转入ETH。

部署到以太坊主网后，可以在线查看发的币：[Zhang0PeterCoin (ZPC) Token Tracker  Etherscan](https://etherscan.io/token/0x5C0529E9C08F37249b70cF6030Ba74C1cF898838)


![图片]({% link /images/2020/2020-eth-2.png %})

里面显示的交易记录是部署合约时获得的2333个代币。




现在一个符合ERC-20标准的以太坊代币已经发行成功了。









## 挖矿，ICO众筹与购买

**警告：发代币进行ICO集资是违法行为！！**


上面的代码已经实现了最简单的代币，也就是发行者拥有所有代币，发行方可以把虚拟币转给任何人。


问：比特币可以挖矿挖出新的币，那发行的代币可以挖出新的币吗？       
答：以太坊上的代币，也称token，进行挖矿是毫无意义的，因为交易都在以太坊上进行。如果想要实现一个可以挖矿的虚拟货币，不应依赖以太坊平台，而是参考比特币，从头开始实现区块链。

问：如何进行ICO众筹？          
答：ERC20标准的代币允许众筹，详细操作见：[Crowdsales - OpenZeppelin Docs](https://docs.openzeppelin.com/contracts/2.x/crowdsales)

问：发行的代币可以购买吗？         
答：想要购买虚拟货币，需要这个虚拟货币在交易所上线。以太坊上发行的代币本身没有任何价值。

问：发行的币没有价值，那发币有意义吗？               
答：自己发币就是随便玩玩，大V发币就可以割韭菜了。

