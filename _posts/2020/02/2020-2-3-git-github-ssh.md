---
layout: post
title: "使用ssh连接GitHub上的git服务器"
categories: [git]
description: "使用ssh连接GitHub上的git服务器"
keywords: git
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

之前在linux系统上登录GitHub都是用用户和密码登录的，现在打算用ssh登录GitHub，这样更安全，也不用输密码。

先配置git的用户名和邮箱：
```sh
git config --global user.name "your_name"
git config --global user.email "your_email@xxx.com"
```

先查看本地是否已经配置了公钥和私钥，如果已经存在私钥，无需再次生成。
```sh
cd ~/.ssh
```
生成密钥：
```sh
-> % ssh-keygen -t rsa -b 4096 -C your_email@xxx.com
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/ubuntu/.ssh/id_rsa.
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:14HZgHvg4z5qnFwlcrUeZ7AzxOpp+cka9oEMPrr04YQ zhang0peter@gmail.com
The key's randomart image is:
+---[RSA 4096]----+
|         o.      |
|        o ==     |
|       . *o+o    |
|      . B O.o.   |
|      .=SO.*.    |
|     o oBo.      |
|    Eo===o..     |
|   . ==+oo+.     |
|    oo+..o.      |
+----[SHA256]-----+
```
生成密钥后拷贝公钥`id_rsa.pub`的内容，到GitHub上新建ssh的key：[GitHub SSH and GPG keys](https://github.com/settings/keys)

新建完成后就可以登录GitHub了：
```sh
-> % ssh -T git@github.com
The authenticity of host 'github.com (13.229.188.59)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,13.229.188.59' (RSA) to the list of known hosts.
Hi zhang0peter! You've successfully authenticated, but GitHub does not provide shell access.
```
现在可以用ssh进行clone仓库的操作了：
```sh
-> % git clone git@github.com:zhang0peter/zhang0peter.github.io.git
Cloning into 'zhang0peter.github.io'...
remote: Enumerating objects: 35, done.
remote: Counting objects: 100% (35/35), done.
remote: Compressing objects: 100% (28/28), done.
Receiving objects: 100% (6964/6964), 10.49 MiB | 20.00 KiB/s, done.
Resolving deltas: 100% (2129/2129), done.
```