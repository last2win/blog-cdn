---
layout: post
title: "GitHub静态博客Jekyll-安装谷歌广告-Google AdSense-advertisement"
categories: [博客]
description: ""
keywords: keyword1, keyword2
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

之前写了一篇统计博客访问量的文章：[Jekyll博客统计访问量，阅读量工具总结--LeanCloud，不蒜子，Valine，Google Analytics — zhang0peter的个人博客](https://zhang0peter.com/2020/01/19/GitHub-jekyll-view-counter/)

搭建了自己的GitHub静态博客后，自然会想到在博客上投放广告。毕竟人没有梦想，和咸鱼有什么区别。万一自己的博客以后就成为爆款博客了呢？

提到在网站上投放广告，最容易想到的就是谷歌广告联盟和百度广告联盟，这里需要说明一下，谷歌广告在国内是可以直接访问的。

我选择的是谷歌广告，因为谷歌不需要手持身份证照片，而且据其他站长说谷歌广告的收入比百度高。

首先注册`Google AdSense`：[通过网站获利功能在线创收  Google AdSense – Google](https://www.google.com/intl/zh-CN_cn/adsense/start/)

注册后打开页面：[Google AdSense](https://www.google.com/adsense/)

会提示你在`<head></head>`中加上类似的代码：
```html
<script data-ad-client="ca-pub-5033499242151397" async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
```

等待几小时后会告诉你脚本已经加载，出现另外的提示：
```sh
我们正在审核您的网站
通常，此过程几天之内就能完成，但在某些特殊情况之下最长可能需要2周时间，待一切就绪后，我们会通知您。
```
如果你的网站访问量很多，那么几天就会通知你，如果你的网站跟我的一样是新建的，每天只有十几个访问，那么就要等挺久的。

我等了大约10天，收到邮件通知：`您的网站现已准备好投放 AdSense 广告`。

自动广告的代码就是之前嵌入的认证身份的代码，只需要打开自动广告即可：


![图片]({% link /images/posts/2020-1-24-google-ads.png %})

如果你觉得自动广告的位置不好，可以手动选择广告放的位置：展示广告，信息流广告，文章内嵌广告。

谷歌会提醒你需要在域名根目录下放置文件`ads.txt`，这样可以更好的识别你的网站。
