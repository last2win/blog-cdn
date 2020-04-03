---
layout: post
title:  "解决UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 0: invalid start byte问题"
date:   2018-04-15 02:35:18 +0800
categories: [行走的问题解决机]
description: "Python 报错解决：UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 0: invalid start byte "
keywords: Python
---
## 本文最后更新于2018-6-20，可能会因为没有更新而失效。如已失效或需要修正，请联系我！
早上在用Flask框架时出现了这个问题，我在源代码里写的是  <br/>
{% highlight Python %}
@app.route('/hello')
def hello():
   return render_template('index.html')
{% endhighlight %}
然后打开网页，出现问题，报的错是：  <br/>
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 0: invalid start byte  <br/>
我在网上查阅了资料，这个报错主要是因为我的网站index.html这个文件的16进制开头是FF FE。  <br/>
文件的16进制格式开头的FF FE或者FE FF是Windows平台下特有的BOM开头，不要在文件的开头使用。  <br/>
解决办法是用notepad++或者sublime text等文本剪辑器打开文件，以没有BOM的utf-8编码格式保存。  <br/>