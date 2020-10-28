---
layout: page
title: About
description: zhang0peter的个人博客
keywords: zhang0Peter
comments: true
menu: 关于
permalink: /about/
---

喜欢Python，工作用Java

{% raw %}
***          
{% endraw %}
此博客基于该模板搭建而成：[My Blog / Jekyll Themes / GitHub Pages 博客模板](https://github.com/mzlogin/mzlogin.github.io)

做的改动如下：               
- 自动化博客访问量统计：[Jekyll博客统计访问量，阅读量工具总结--LeanCloud，不蒜子，Valine，Google Analytics](https://zhang0peter.com/2020/01/19/GitHub-jekyll-view-counter/)                 
- [jekyll静态博客提升访问速度：内嵌CSS，异步加载js，压缩HTML](https://zhang0peter.com/2020/01/27/jekyll-inline-css/)
- 自定义域名与HTTPS/TLS/CAA配置参考这篇博客：[GitHub Pages博客：自定义域名，HTTPS，CAA](https://zhang0peter.com/2020/02/21/github-pages-https/)                           



## 关于我

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

* 邮箱：[zhang0peter@foxmail.com](mailto:zhang0peter@foxmail.com)




{% raw %}
***          
{% endraw %}

如果你有什么想说的，欢迎在此留言。
