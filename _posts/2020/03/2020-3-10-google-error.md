---
layout: post
title: "谷歌 sitemap.xml 报错解决：无法读取此站点地图"
categories: [行走的问题解决机,博客]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

早上在查看`google search`时，发现我网站的站点地图`sitemap.xml`已经一星期多没被读取了。

我感觉不对劲，于是提交了一个新的站点地图上去，显示`无法读取此站点地图`，状态是`无法获取`，读取站点地图失败。

网上的解决方法五花八门，最多说的是删除站点资源，重新添加。

先到`网址检查`那里测试`sitemap.xml`，请求编入索引会报错：在测试实际版本的过程中，系统检测到该网址存在索引编制问题.

解决不了，我尝试删除资源，然后重新添加。

还是没用。

等我解决了问题再写这篇文章。

{% raw %}
***          
{% endraw %}

我随后写了这篇文章：[nginx-代理/转发-GitHub Pages 静态页面博客](https://zhang0peter.com/2020/03/16/github-pages-nginx-cdn-proxy/)

发现使用Nginx后问题没有解决。


{% raw %}
***          
{% endraw %}

我配置`robots.txt`:

```txt
# allow google
User-agent: *
Allow: /
```

{% raw %}
***          
{% endraw %}

然后我发现了一个测试网站：[富媒体搜索结果测试 - Google Search Console](https://search.google.com/test/rich-results)         


如果你的网站不能通过测试，那么自然无法爬取。我在添加了`robots.txt`后就，谷歌搜索就可以正常爬取我的博客了。

以及这个网站可以查看谷歌爬虫的情况：[Search Console - Crawl Stats](https://www.google.com/webmasters/tools/crawl-stats)




`zhang0peter.com` -> `zhang0peter.github.io`  √
`cloudflare cdn`  -> `github pages`           √



cloudflare cdn-> nocache cdn -> nginx -> github pages  ×
