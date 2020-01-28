---
layout: post
title: "Linux/ubuntu 服务器开启6010端口-X11服务-ssh连接"
categories: [Linux]
description: "Linux/ubuntu 服务端开启端口6010端口-X11服务-ssh连接--sshd-X11 proxy"
keywords: Linux, x11, ssh
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

晚上在检查我的Linux-ubuntu服务器的端口状况时发现开启了6010端口：
```sh
-> # nmap -p 1-65535 127.0.0.1

Starting Nmap 7.60 ( https://nmap.org ) at 2020-01-22 19:13 CST
Nmap scan report for localhost.localdomain (127.0.0.1)
Host is up (0.0000040s latency).
Not shown: 65531 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
6010/tcp open  x11
6011/tcp open  x11
6013/tcp open  x11

Nmap done: 1 IP address (1 host up) scanned in 7.63 seconds
```

我本来以为是服务器之前装了`vnc server`，没关导致6010，6011,6013的端口开着，具体参考这篇文章：[Linux/ubuntu server 18.04 安装远程桌面--vnc server-tightvncserver — zhang0peter的个人博客](https://zhang0peter.com/2020/01/07/linux-server-vnc/)

查看端口对应的进程：
```sh
-> # netstat -tunlp|grep 6010
tcp        0      0 127.0.0.1:6010          0.0.0.0:*               LISTEN      29899/sshd: ubuntu@ 
-> # ps -aux | grep 29899
ubuntu   29899  0.0  0.3 107988  5664 ?        S    20:27   0:00 sshd: ubuntu@pts/0
```
```sh
-> # netstat -tunlp|grep 6013
tcp        0      0 127.0.0.1:6013          0.0.0.0:*               LISTEN      8326/sshd: ubuntu@p 
-> # ps -aux | grep 1911     
ubuntu    1911  0.0  0.3 108120  6316 ?        S    17:45   0:00 sshd: ubuntu@pts/1
```
发现发起x11转发的程序并不是`xrdp`或`vnc`，而是`sshd`，一下子让我想到了原因。

我使用`xshell`连接服务器，xshell默认是开启`x11`转发的。

如果你在命令行中运行图形界面程序的命令，如`gedit`，那么`xshell`会提示你如下：
```sh
-> # gedit 
Unable to init server: Could not connect: Connection refused
(gedit:1603): Gtk-WARNING **: 20:49:59.939: cannot open display: localhost:12.0
```
```sh
需要Xmanager软件来处理X11转发请求。
当你安装Xmanager时，你可以...
(要关闭此消息，只需关闭会话属性- >连接- >SSH->隧道页面中的X11转发选项。)
```
xshell默认开启`x11`转发是因为Xmanager需要花钱下载，它希望你下载软件好赚钱。

如果你想在`Windows`的机器上进行X11转发，推荐使用`MobaXterm`这个软件，自带X11转发。

直接用root用户运行会报错，推荐使用`sudo`运行命令。
```sh
-> # gedit
MoTTY X11 proxy: Unsupported authorisation protocol
Unable to init server: Could not connect: Connection refused
(gedit:3978): Gtk-WARNING **: 21:00:13.560: cannot open display: localhost:15.0

-> % sudo gedit
(gedit:3876): GLib-GIO-CRITICAL **: 21:00:05.062: g_dbus_proxy_new_sync: assertion 'G_IS_DBUS_CONNECTION (connection)' failed
(gedit:3876): dconf-WARNING **: 21:00:05.076: failed to commit changes to dconf: Failed to execute child process “dbus-launch” (No such file or directory)
```

更多内容可以参考这篇文章，写的不错：[利用X11 Forwarding运行VPS上的图形界面软件 - Tech Notes of Code Monkey](https://blog.finaltheory.me/note/X11-Forwarding-GUI.html)

