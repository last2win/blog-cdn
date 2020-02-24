---
layout: post
title: "使用Python画素描"
categories: [Python]
description: ""
keywords: Python
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}


第一份画素描的代码是用线条为单位进行素描的，并且增加了随机函数，使得线条长度不确定，这样创作的素描画看上去更柔和，也更接近真实的人类作画的风格。 


第二份的代码修改为使用numpy，只要20行就能实现这个功能，效果还更好。 


这张是原图 

![图片]({% link /images/2018/1.jpg %})

这张是原先的代码画的 

![图片]({% link /images/2018/pencil_drawing1.jpg %})

这张是修改后的代码画的 

![图片]({% link /images/2018/new1.jpg %})

修改后的代码:

```python
from PIL import Image
import numpy as np
a = np.asarray(Image.open('1.jpg').convert('L')).astype('float')
depth = 10.  # (0-100)
grad = np.gradient(a)  # 取图像灰度的梯度值
grad_x, grad_y = grad  # 分别取横纵图像梯度值
grad_x = grad_x * depth / 100.
grad_y = grad_y * depth / 100.
A = np.sqrt(grad_x ** 2 + grad_y ** 2 + 1.)
uni_x = grad_x / A
uni_y = grad_y / A
uni_z = 1. / A
vec_el = np.pi / 2.2  # 光源的俯视角度，弧度值
vec_az = np.pi / 4.  # 光源的方位角度，弧度值
dx = np.cos(vec_el) * np.cos(vec_az)  # 光源对x 轴的影响
dy = np.cos(vec_el) * np.sin(vec_az)  # 光源对y 轴的影响
dz = np.sin(vec_el)  # 光源对z 轴的影响
b = 255 * (dx * uni_x + dy * uni_y + dz * uni_z)  # 光源归一化
b = b.clip(0, 255)
im = Image.fromarray(b.astype('uint8'))  # 重构图像
im.save('new1.jpg')
```





