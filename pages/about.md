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

博客项目地址：[zhang0peter- 基于GitHub的Jeykll静态博客](https://github.com/zhang0peter/zhang0peter.github.io)

做的改动如下：   
- 增加sitemap，方便搜索引擎索引
- 增加SEO 插件：Jekyll SEO tag
- 修复Bug：meta 的keywords不起作用，因为原作者用的是Mac，而Mac与Windows平台的换行符不一样        
- 增加谷歌广告，脚本位置在`header.html`中
- 使用谷歌的站内搜索替代`Simple-Jekyll-Search`插件，优势：可以全文索引，不用每个网页都加载用于搜索的数据
- 修改网页背景色为`#FFF7DB`




Fork后的操作方法参考原始项目的Fork 指南：[Fork 指南](https://github.com/mzlogin/mzlogin.github.io#fork-%E6%8C%87%E5%8D%97)



## 关于我

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

* 邮箱：[zhang0peter@foxmail.com](mailto:zhang0peter@foxmail.com)
