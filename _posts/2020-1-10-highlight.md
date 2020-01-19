---
layout: post
title: GitHub 博客-- Jekyll--代码高亮，Liquid 转义字符
categories: [git, 杂类,Jekyll]
description: "GitHub 博客-- Jekyll--代码高亮，Liquid 转义字符--在markdown文件中高亮代码--code-highlight, Syntax-Highlight"
keywords: code-highlight, Syntax-Highlight
---
此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         
转载请注明
{% raw %}
***          
{% endraw %}
在使用Jekyll搭建了自己的 GitHub博客后，想使代码高亮，因为Jekyll使用Liquid语言，跟md相比语法有些不同。               
在markdown文件中如果使用传统的{{"```" | escape}}大部分情况下可以正确高亮代码 ：         
~~~text
```js
var i=1
```
~~~
也可以使用liquid提供的高亮方法：
{% raw %}
~~~text
{% highlight ruby %}
def foo
  puts 'foo'
end
{% endhighlight %}
~~~
{% endraw %}
有时候会报错，因为高亮的代码中有大括号：
{% raw %}
~~~text
```js
{% if i != null %}
```
  Liquid Exception: Liquid syntax error (line 24): 'if' tag was never closed in C:/Users/peter/Documents/GitHub/zhang0peter.github.io/_posts/2020-1-10-highlight.md
             Error: Liquid syntax error (line 24): 'if' tag was never closed
             Error: Run jekyll build --trace for more information.
~~~
{% endraw %}

解决方法是使用{% raw %}{% raw %}{% endraw %}:
{% raw %}
~~~text
```js
{% if i != null %}
```
~~~
{% endraw %}   

![图片]({% link /images/posts/2020-1-10.png %})
