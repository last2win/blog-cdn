---
layout: post
title: Jekyll博客统计访问量，阅读量--
categories: [cate1, cate2]
description: ""
keywords: visitor-count, view-count, LeanCloud
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}


我用GitHub Pages搭建了jekyll的博客后，想要在页面上实现访问量的统计。

因为实在GitHub上搭建的静态博客，不能像WordPress一样操作操作php和数据库，自然只能借助第三方工具。

## LeanCloud

网上有很多人推荐使用 LeanCloud 进行访问量的统计，比如说这篇文章：[jekyll使用LeanCloud记录文章的访问次数](https://priesttomb.github.io/%E6%97%A5%E5%B8%B8/2017/11/06/jekyll%E4%BD%BF%E7%94%A8LeanCloud%E8%AE%B0%E5%BD%95%E6%96%87%E7%AB%A0%E7%9A%84%E8%AE%BF%E9%97%AE%E6%AC%A1%E6%95%B0/)    

和这篇文章：[添加阅读量统计 | ZFB 博客](https://blog.whuzfb.cn/blog/2019/01/05/blog_reading_counter/)

我本来也想用 LeanCloud 进行记录，但我在注册的时候告诉我要实名认证，输入姓名和身份证，还要支付宝授权`美味书签（北京）信息技术有限公司`读取我的认证信息和人脸照片。这我绝对不能忍，还要我的人脸照片，这是想做什么。

于是我就放弃使用LeanCloud进行访问量统计，对个人信息不在意的博主们可以使用LeanCloud。

## 不蒜子
不蒜子也算是小有名气的博客访问量统计工具了，作者官网：[不蒜子 - 极简网页计数器](https://busuanzi.ibruce.info/)

使用方法非常简单：
```js
<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
<span id="busuanzi_container_site_pv">本站总访问量<span id="busuanzi_value_site_pv"></span>次</span>
```


## 其他工具

我还在网上找到了其他工具。

Hit Kounter：[访问量统计工具 Hit Kounter v0.3 | 咀嚼之味](https://jerryzou.com/posts/introduction-to-hit-kounter-lc/)

这位博主自己做了一个访问量统计工具，感兴趣的可以去看看。

