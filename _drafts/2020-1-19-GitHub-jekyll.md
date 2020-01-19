---
layout: post
title: Jekyll博客统计访问量，阅读量工具总结--LeanCloud，不蒜子，Valine，Google Analytics
categories: [博客]
description: "Jekyll,博客,统计访问量, 阅读量工具, LeanCloud, 不蒜子, Valine, Google Analytics, visitor-count, view-count, LeanCloud, WordPress, GitHub, Valine, Hit Kounter"
keywords: Jekyll,博客,统计访问量, 阅读量工具, LeanCloud, 不蒜子, Valine, Google Analytics, visitor-count, view-count, LeanCloud, WordPress, GitHub, Valine, Hit Kounter
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}


我用 GitHub Pages搭建了jekyll的博客后，想要在页面上实现访问量的统计。

因为实在GitHub上搭建的静态博客，不能像WordPress一样可以操作php和数据库，自然只能借助第三方工具。

网上有个问答：[html - Is that real show count view pages with use Jekyll? - Stack Overflow](https://stackoverflow.com/questions/51075898/is-that-real-show-count-view-pages-with-use-jekyll)

## LeanCloud

网上有很多人推荐使用 LeanCloud 进行访问量的统计，比如说这篇文章：[jekyll使用LeanCloud记录文章的访问次数](https://priesttomb.github.io/%E6%97%A5%E5%B8%B8/2017/11/06/jekyll%E4%BD%BF%E7%94%A8LeanCloud%E8%AE%B0%E5%BD%95%E6%96%87%E7%AB%A0%E7%9A%84%E8%AE%BF%E9%97%AE%E6%AC%A1%E6%95%B0/)    和这篇文章：[添加阅读量统计  ZFB 博客](https://blog.whuzfb.cn/blog/2019/01/05/blog_reading_counter/)

你也可以使用基于LeanCloud进行开发的Valine：[文章阅读量统计  Valine](https://valine.js.org/visitor.html)

我本来也想用 LeanCloud 进行记录，但我在注册的时候告诉我要实名认证，输入姓名和身份证，还要支付宝授权`美味书签（北京）信息技术有限公司`读取我的认证信息和人脸照片。这我绝对不能忍，还要我的人脸照片，这是想做什么。

于是我就放弃使用LeanCloud进行访问量统计，对个人信息不在意的博主们可以使用LeanCloud。

## 不蒜子
不蒜子也算是小有名气的博客访问量统计工具了，作者官网：[不蒜子 - 极简网页计数器](https://busuanzi.ibruce.info/)

使用方法非常简单：
```html
<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
<span id="busuanzi_container_site_pv">本站总访问量<span id="busuanzi_value_site_pv"></span>次</span>
```

非常方便，图方便的博主可以使用不蒜子。

我最终没有选择不蒜子是因为访问量的统计是存在它的服务器上，我自己是查看不到的。

## 各类分析工具

比如说：[Google Analytics（分析）](https://analytics.google.com/)

如果你使用谷歌分析，那么自然可以在谷歌分析上看到你网站的访问量以及各种数据。

我看到了另一款新的工具：[Simple Analytics - Simple, clean, and privacy-friendly analytics](https://simpleanalytics.com)

这也是一款网页浏览量分析工具，吸引我的特点是支持使用json获取统计到的数据。

注册后，使用示例如下：
```html
    <script type="text/javascript">
      $.ajax({
        url: "https://simpleanalytics.com/zhang0peter.com.json",
        success: function (data) {
          $("#site_pv").text(data.pageviews);
        }
      });
    </script>
    <span id="container_site_pv">本站总访问量<span id="site_pv"></span>次</span>

```
你可以通过json获取网站的访问量来先是在网站上。

## 其他工具

我还在网上找到了其他工具。

Hit Kounter：[访问量统计工具 Hit Kounter v0.3  咀嚼之味](https://jerryzou.com/posts/introduction-to-hit-kounter-lc/)

这位博主自己做了一个访问量统计工具，感兴趣的可以去看看。

