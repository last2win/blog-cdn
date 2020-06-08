---
layout: post
title: "Linux环境下删除卸载caddy-Linux remove/uninstall caddy"
categories: [行走的问题解决机]
description: "直接删除caddy的二进制文件，并停止服务"
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

最近在安装使用caddy后想要进行卸载，不再使用caddy了，结果网上找了半天，没找到官方的卸载方法，尴尬。

如果你使用官方脚本下载的caddy:

```sh
curl https://getcaddy.com | bash -s personal
```

那么Caddy的二进制文件存放在目录`/usr/local/bin/caddy`中，如果是手动安装，可能在目录`/usr/bin/caddy`中。

删除方法是删除二进制文件并停止服务:
```sh
rm /usr/local/bin/caddy
systemctl disable caddy.service
```

记得手动kill进程，卸载即可完成。

