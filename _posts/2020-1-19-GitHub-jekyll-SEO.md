---
layout: post
title: jekyll 博客对搜索引擎的SEO提升方法--head中的meta标签和Jekyll SEO Tag
categories: [博客]
description: "GitHub:jekyll 博客对搜索引擎的SEO提升方法--head中的meta标签和Jekyll SEO Tag,SEO,百度,谷歌,搜索引擎,排名提升,网站优化,关键词"
keywords: jekyll, GitHub, Jekyll SEO Tag,SEO,百度,谷歌,搜索引擎,排名提升,网站优化,关键词
---


此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}



我用GitHub Pages搭建了jekyll的博客后，想要提升自己博客的SEO，尤其是对搜索引擎：百度、谷歌。

## head中的meta标签

说道提升SEO，必然要提到HTML中的head标签中的meta标签。

> <meta> 元素可提供有关页面的元信息（meta-information），比如针对搜索引擎和更新频度的描述和关键词。

```html
<meta name="keywords" content="SEO, 百度, 谷歌, 搜索引擎, 排名提升, 网站优化, 关键词">
```

因为滥用关键词keyword，谷歌已经在搜索中忽略meta tag中的keywords:[Official Google Webmaster Central Blog [EN]: Google does not use the keywords meta tag in web ranking](https://webmasters.googleblog.com/2009/09/google-does-not-use-keywords-meta-tag.html)

meta tag还有一个常用的用法：

```html
 <meta name="description" content="Jekyll博客SEO排名提升方法">
```
搜索引擎不会忽略`description`，因为搜索时显示的简介内容一般就是`description`中的内容，如果没有的话，搜索引擎会摘录文章中的第一段作为简介。

## Jekyll SEO Tag
使用插件[jekyll/jekyll-seo-tag: A Jekyll plugin to add metadata tags for search engines and social networks to better index and display your site's content.](https://github.com/jekyll/jekyll-seo-tag/)帮你做SEO。

用了插件后你就不需要手动去配置`meta`参数的这个配置了，由这个插件帮你配。



参考：

*   [无用的keywords - xiaOp的博客](https://xiaoiver.github.io/coding/2017/07/23/%E6%97%A0%E7%94%A8%E7%9A%84keywords.html)
*   [10 Must do Jekyll SEO optimizations  Webjeda](https://blog.webjeda.com/optimize-jekyll-seo/)
