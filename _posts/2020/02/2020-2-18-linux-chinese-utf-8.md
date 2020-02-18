---
layout: post
title: "解决linux服务器安装桌面后显示中文乱码的问题：ubuntu 18.04 locales zh_CN.UTF-8"
categories: [Linux]
description: "解决linux服务器安装桌面后显示中文乱码的问题"
keywords: Linux
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

按照之前写的博客，在 ubuntu 18.04 上安装了 vnc server:[Linux/ubuntu server 18.04 安装远程桌面--vnc server-tightvncserver](https://zhang0peter.com/2020/01/07/linux-server-vnc/)

```sh
apt install xfce4 #安装桌面
apt install tightvncserver # 安装 vnc server
vncserver #设置vnc密码 并启动
```

windows本地使用`vnc viewer`访问服务器，默认的端口是`5901`

但是访问后发现显示的英文是正常的，但显示的中文是乱码的，解决方法是把编码设置为`zh_CN.UTF-8`

查看本地编码类型：
```sh
-> # locale
LANG=C.UTF-8
LANGUAGE=
LC_CTYPE="C.UTF-8"
LC_NUMERIC="C.UTF-8"
LC_TIME="C.UTF-8"
LC_COLLATE="C.UTF-8"
LC_MONETARY="C.UTF-8"
LC_MESSAGES="C.UTF-8"
LC_PAPER="C.UTF-8"
LC_NAME="C.UTF-8"
LC_ADDRESS="C.UTF-8"
LC_TELEPHONE="C.UTF-8"
LC_MEASUREMENT="C.UTF-8"
LC_IDENTIFICATION="C.UTF-8"
LC_ALL=
```
安装中文字体
```sh
apt-get install ttf-wqy-zenhei
```
修改编码：
```sh
sudo dpkg-reconfigure locales
# 选择 zh_CN.UTF-8
sudo update-locale LANG=zh_CN.UTF-8
```
重启服务器后编码就变为中文的, vnc viewer连接后成功显示中文。

参考：[Changing ubuntu server's language to english - Ask Ubuntu](https://askubuntu.com/questions/380746/changing-ubuntu-servers-language-to-english)
