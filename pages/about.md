---
layout: page
title: About
description: zhang0peter的个人博客
keywords: zhang0Peter
comments: true
menu: 关于
permalink: /about/
---

学生，喜欢Python，在学Java

{% raw %}
***          
{% endraw %}
此博客基于该模板搭建而成：[My Blog / Jekyll Themes / GitHub Pages 博客模板](https://github.com/mzlogin/mzlogin.github.io)

我的博客项目地址：[zhang0peter- 基于GitHub的Jeykll静态博客](https://github.com/zhang0peter/zhang0peter.github.io)

做的改动如下： 
- 修改网页背景色为`#FFF7DB`   
- 增加sitemap，方便搜索引擎索引
- 对文章页面增加TOC目录      
- 使用`jekyll 4.0`替换`github-pages`，编译速度大幅增加
- 使用 cloudflare的CDNJS替换本地的jquery引用
- 修复Bug：meta 的keywords不起作用，因为原作者用的是Mac，而Mac与Windows平台的换行符不一样 
- 使用谷歌的站内搜索替代`Simple-Jekyll-Search`插件，优势：可以全文索引，不用每个网页都加载用于搜索的数据
- 增加SEO 插件：Jekyll SEO tag：[jekyll 博客对搜索引擎的SEO提升方法--head中的meta标签和Jekyll SEO Tag](https://zhang0peter.com/2020/01/19/GitHub-jekyll-SEO/)
- 增加谷歌广告：[GitHub静态博客Jekyll-安装谷歌广告-Google AdSense-advertisement — zhang0peter的个人博客](https://zhang0peter.com/2020/01/24/jekyll-google-advertisement/)
- 自动化博客访问量统计：[Jekyll博客统计访问量，阅读量工具总结--LeanCloud，不蒜子，Valine，Google Analytics](https://zhang0peter.com/2020/01/19/GitHub-jekyll-view-counter/)
- [jekyll静态博客提升访问速度：内嵌CSS，异步加载js，压缩HTML](https://zhang0peter.com/2020/01/27/jekyll-inline-css/)


Fork后的操作方法参考这篇博客：[在GitHub上搭建GitHub Pages博客-- Jekyll](https://zhang0peter.blog.csdn.net/article/details/103903611)

其中自定义域名与HTTPS/TLS/CAA配置参考这篇博客：[GitHub Pages博客：自定义域名，HTTPS，CAA](https://zhang0peter.com/2020/02/21/github-pages-https/)        

推荐 CDN: `nodecache`，注册链接：[Nodecache](https://console-api.nodecache.com/f?aff=4oMnb3)，通过该链接注册，送1T流量包

## 关于我

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

* 邮箱：[zhang0peter@foxmail.com](mailto:zhang0peter@foxmail.com)


## 网站访问量与排名

这个博客的域名于2020年1月开始使用

2020-2-19: 全球排名3百万，中国排名39万

![图片]({% link /images/2020/2020-2-19.png %})

2020-3-6：全球排名285万，中国排名38万

![图片]({% link /images/2020/2020-3-6.png %})

{% raw %}
***          
{% endraw %}

如果你有什么想说的，欢迎在此留言。

