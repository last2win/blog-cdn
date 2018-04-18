---
layout: post
title:  "Python变量作用域问题"
date:   2018-04-18 14:35:18 +0800
categories: Python
description: ""
keywords: Python
---
今天在用Python写程序时突然想起了Python变量作用域的问题，代码如下：  <br/>
{% highlight Python %}
def run():
    global x
    x=3
    print(x)
run()
x+=1
print(x)
{% endhighlight %}  
这段代码运行并不会出错，问题的关键主要在于global这个关键字，stackoverflow上有人问过类似的问题,
[url](https://stackoverflow.com/questions/13881395/in-python-what-is-a-global-statement)  <br/>
主要的意思是在一个函数里如果把一个变量声明为global，那么这个变量就是全局的，  <br/>
如果全局变量中不存在这个变量，那就新建一个全局变量。  <br/>
然后我就写了另外一段有意思的代码：  <br/>
{% highlight Python %}
#main.py
import main2
main2.first()
main2.then()
{% endhighlight %}  
{% highlight Python %}
#main2.py
def first():
    global x
    x=0
    print(x)
def then():
    global x
    x+=1
    print(x)
{% endhighlight %}  
这是在2个文件里的Python代码，可以正确运行。  <br/>
这段代码的意思是Python的global声明的最大作用域是当前文件，并不能用其他文件里的全局变量。  <br/>
在上面的代码中x的作用域是main2整个文件，而在main里是没有x这个变量存在的！  <br/>