---
layout: post
title: "Linux的useradd与adduser命令区别"
categories: [Linux]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

在系统中使用`man useradd`查看帮助：
```txt
USERADD(8)                                                      System Management Commands                                                     USERADD(8)

NAME
       useradd - create a new user or update default new user information

SYNOPSIS
       useradd [options] LOGIN

       useradd -D

       useradd -D [options]

DESCRIPTION
       useradd is a low level utility for adding users. On Debian, administrators should usually use adduser(8) instead.
```

从文档中可以看出推荐在Debian系统上使用`adduser`，而不是`useradd`。

使用`man adduser`查看文档：
```txt
ADDUSER(8)                                                       System Manager's Manual                                                       ADDUSER(8)

NAME
       adduser, addgroup - add a user or group to the system

SYNOPSIS
       adduser  [options]  [--home DIR] [--shell SHELL] [--no-create-home] [--uid ID] [--firstuid ID] [--lastuid ID] [--ingroup GROUP | --gid ID] [--dis‐
       abled-password] [--disabled-login] [--gecos GECOS] [--add_extra_groups] [--encrypt-home] user

       adduser --system [options] [--home DIR] [--shell SHELL] [--no-create-home] [--uid ID] [--group | --ingroup GROUP | --gid ID] [--disabled-password]
       [--disabled-login] [--gecos GECOS] user

       addgroup [options] [--gid ID] group

       addgroup --system [options] [--gid ID] group

       adduser [options] user group

   COMMON OPTIONS
       [--quiet] [--debug] [--force-badname] [--help|-h] [--version] [--conf FILE]

DESCRIPTION
       adduser  and  addgroup  add  users  and groups to the system according to command line options and configuration information in /etc/adduser.conf.
       They are friendlier front ends to the low level tools like useradd, groupadd and usermod programs, by default choosing  Debian  policy  conformant
       UID  and GID values, creating a home directory with skeletal configuration, running a custom script, and other features.  adduser and addgroup can
       be run in one of five modes:

```
可以看出`adduser`的参数更全面，更好用。

顺便提一下，如果想为一个系统服务创建专门的用户，比如说为hadoop服务创建一个专门的hadoop用户用来运行相关程序，这时候可以使用如下命令：
```sh
adduser --system  hadoop
```
使用`--system`创建的是系统用户，系统用户默认的shell是`/usr/sbin/nologin`，这个shell禁止登录，只能运行程序，所以非常安全。

本地用户想要切换到系统用户可以使用如下命令：
```sh
su - hadoop -s /bin/bash
```
现在就切换到bash，可以正常运行了。

