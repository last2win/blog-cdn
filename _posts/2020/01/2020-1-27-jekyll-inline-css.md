---
layout: post
title: "jekyll静态博客提升访问速度：内嵌CSS，异步加载js，压缩HTML"
categories: [博客]
description: "jekyll静态博客提升访问速度：内嵌CSS，异步加载js,sync load, inline css"
keywords: 博客, keyword2
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

在谷歌搜索的功能`速度(实验性)`中推荐使用工具`PageSpeed Insights`查看我的网页访问速度情况：[PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)

测试结果：[PageSpeed Insights of zhang0peter.com](https://developers.google.com/speed/pagespeed/insights/?url=https%3A%2F%2Fzhang0peter.com%2F&hl=zh_CN)



![图片]({% link /images/posts/2020-1-27-css-1.png %})

分数很低，只有33分。

优化建议的第一条是`移除阻塞渲染的资源`，里面列出了非异步加载的css和js文件。提示：

> 资源阻止了系统对您网页的首次渲染。建议以内嵌方式提供关键的 JS/CSS，并推迟提供所有非关键的 JS/样式。

## 异步加载javascript
对于非关键的js文件，想要实现异步加载很简单：
```html
<script async src="siteScript.js"></script>
```
`async`或`defer`关键字都可以实现js脚本的异步加载，更多内容参考:[javascript - load scripts asynchronously - Stack Overflow](https://stackoverflow.com/questions/7718935/load-scripts-asynchronously)
## 内嵌与压缩css
但是对于css文件来说，异步加载就比较困难了，没有js那样的直接异步加载的工具。

根据[html - How to load CSS Asynchronously - Stack Overflow](https://stackoverflow.com/questions/32759272/how-to-load-css-asynchronously)，想要css async load asyncload，最多只能在一个css文件上实现，而且css是阻塞渲染的资源，更多内容见：[阻塞渲染的 CSS    Web    Google Developers](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css)

想要最快的方法加载css，推荐内联关键css，其实对于小网站来说，可以把所有的css全部内联，一次加载完成。

内联(inline)css后，就不会阻塞渲染，等待css文件的加载，网页访问速度会大幅度提升。

但如何在jeykll的静态博客中进行内联操作，我找了很多资料。

最后的解决方法是下`header.html`页面中直接`include`css文件，这样就可以把生成的html中就嵌入`css`代码。

代码如下：
{% raw %}
```sh
{% capture styles %}
{% include  assets/css/main.css%}
{% endcapture %}

<style>
    {{ styles | scssify}}
</style>
```
{% endraw %}

你还可以在`_config.yml`中增加压缩css的配置：
```yml
sass:
    style: compressed
```
现在css已经成功内嵌。

## 压缩html
使用工具：[penibelst/jekyll-compress-html: A Jekyll layout that compresses HTML in pure Liquid](https://github.com/penibelst/jekyll-compress-html)
下载最新版本，放到目录`_ayouts`下，配置`_layouts/default.html`：
```sql
---
layout: compress
---
```
在`_config.yml`中增加压缩html的配置：
```yml
compress_html:
  blanklines: true
  comments: ["<!-- ", " -->"]
```
需要注意的是这个脚本可能不完善。

{% raw %}
***          
{% endraw %}
网页优化的缺点就是编译速度变慢了，参考我这篇文章加快编译速度：[GitHub-jekyll静态博客快速构建与优化--jekyll serve --incremental --profile — zhang0peter的个人博客](https://zhang0peter.com/2020/01/25/github-jekyll-fast-build/)

更多内容参考：
- [Inline CSS in Jekyll — SitePoint](https://www.sitepoint.com/inline-css-in-jekyll/)
- [Jekyll Speed Improvements](https://brototyp.de/blog/jekyll-speed-improvements/)
