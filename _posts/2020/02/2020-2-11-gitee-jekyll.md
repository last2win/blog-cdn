---
layout: post
title: "在码云gitee上部署jekyll博客"
categories: [博客]
description: "在码云gitee上部署jekyll博客"
keywords: jekyll
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

之前我已经在GitHub部署了GitHub Pages 博客：[zhang0peter的博客](https://zhang0peter.com/)

现在听人说gitee上也能部署博客，而且是国内的，网站的访问速度会更快，于是就注册一个账号试试。

官方文档：[码云Pages - 码云 Gitee.com](https://gitee.com/help/articles/4136#article-header0)

然后我发现自定义域名要收费！！

推送后自动部署也要收费！！！

跟GitHub Pages比起来差远了。

我第一次尝试部署时失败了，报错：
```sh
部署失败
错误信息: Liquid syntax error (/disk/pages/zh/zhang0peter/projects/zhang0peter.gitee.io/_includes/header.html line 74): Unknown tag 'seo' included
```
我使用了插件`jekyll-seo-tag`，并在`Gemfile`中配置了`gem "jekyll-seo-tag"`，居然无法使用！！

码云Pages是直接进行`jekyll serve`而不先安装库。

部署完成后访问：[zhang0peter的博客](https://zhang0peter.gitee.io/)

体验太糟糕了，首页访问地址带二级目录。如果想要访问地址不带二级目录，建立一个与自己个性地址同名的仓库。

总结：不推荐使用 Gitee Pages， 推荐使用GitHub Pages。

可以看这几篇文章：

- [zhang0peter- 基于GitHub的Jeykll静态博客](https://zhang0peter.com/about/)            
- [在GitHub上搭建GitHub Pages博客-- Jekyll](https://zhang0peter.blog.csdn.net/article/details/103903611)
