---
layout: page
title: About
description: zhang0peter的个人博客
keywords: Zhang Peter
comments: true
menu: 关于
permalink: /about/
---

浙大学生，喜欢使用Python。            
即将入职拼多多。        
联系我：zhang0peter@foxmail.com                     


## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
