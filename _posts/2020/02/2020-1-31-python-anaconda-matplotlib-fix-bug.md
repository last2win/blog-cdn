---
layout: post
title: "Python-anaconda-Spyder使用matplotlib画图无法显示报错解决：Figures now render in the Plots pane by default. To make them also appear inline in the Console, uncheck \"Mute Inline Plotting\" under the Plots pane options menu. "
categories: [行走的问题解决机]
description: "Python-anaconda-Spyder使用 matplotlib 画图无法显示报错解决：Figures now render in the Plots pane by default. To make them also appear inline in the Console, uncheck \"Mute Inline Plotting\" under the Plots pane options menu."
keywords: Python, anaconda, matplotlib
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         


{% raw %}
***          
{% endraw %}

晚上在用anaconda的Spyder IDE，用 matplotlib 画图时不会显示图片在`iPython`终端中，报错如下：
```sh
Figures now render in the Plots pane by default. To make them also appear inline in the Console, uncheck "Mute Inline Plotting" under the Plots pane options menu. 
```
网上查找资料后发现问题所在：[python - Spyder Plot Inline - Stack Overflow](https://stackoverflow.com/questions/24002076/spyder-plot-inline)


说的是最新版本的`Spyder 4.0`默认显示图片在右上角，不显示在终端中，如图所示：



![图片]({% link /images/2020/2020-2-1.png %})

想要弹出窗口显示`matplotlib`画的图片，修改设置：

`Tools` > `Preferences` > `iPython console` > `Graphics` > `Graphics backend` > `Automatic`

重启IDE，现在画图就会弹出窗口显示了。



