---
layout: post
title: "Linux/Debian/Ubuntu报错解决：W: Target Packages (main/binary-amd64/Packages) is configured multiple times in"
categories: [行走的问题解决机]
description: "Linux/Debian/Ubuntu报错解决：W: Target Packages (main/binary-amd64/Packages) is configured multiple times in"
keywords: Linux, ubuntu
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

今天在ubuntu上更新库(apt update)的时候遇到了报错：

```sh
-> # apt update                
Hit:1 http://mirrors.zju.edu.cn/ubuntu bionic InRelease
Hit:2 https://mirrors.aliyun.com/kubernetes/apt kubernetes-xenial InRelease
Hit:3 http://mirrors.zju.edu.cn/ubuntu bionic-security InRelease
Hit:4 http://mirrors.zju.edu.cn/ubuntu bionic-updates InRelease
Hit:5 http://mirrors.zju.edu.cn/ubuntu bionic-backports InRelease
Reading package lists... Done                      
Building dependency tree       
Reading state information... Done
37 packages can be upgraded. Run 'apt list --upgradable' to see them.
W: Target Packages (main/binary-amd64/Packages) is configured multiple times in /etc/apt/sources.list.d/kubernetes.list:1 and /etc/apt/sources.list.d/kubernetes.list:2
W: Target Packages (main/binary-i386/Packages) is configured multiple times in /etc/apt/sources.list.d/kubernetes.list:1 and /etc/apt/sources.list.d/kubernetes.list:2
W: Target Packages (main/binary-all/Packages) is configured multiple times in /etc/apt/sources.list.d/kubernetes.list:1 and /etc/apt/sources.list.d/kubernetes.list:2
W: Target Translations (main/i18n/Translation-en_US) is configured multiple times in /etc/apt/sources.list.d/kubernetes.list:1 and /etc/apt/sources.list.d/kubernetes.list:2
W: Target Translations (main/i18n/Translation-en) is configured multiple times in /etc/apt/sources.list.d/kubernetes.list:1 and /etc/apt/sources.list.d/kubernetes.list:2
W: Target CNF (main/cnf/Commands-amd64) is configured multiple times in /etc/apt/sources.list.d/kubernetes.list:1 and /etc/apt/sources.list.d/kubernetes.list:2
W: Target CNF (main/cnf/Commands-all) is configured multiple times in /etc/apt/sources.list.d/kubernetes.list:1 and /etc/apt/sources.list.d/kubernetes.list:2
W: Target Packages (main/binary-amd64/Packages) is configured multiple times in /etc/apt/sources.list.d/kubernetes.list:1 and /etc/apt/sources.list.d/kubernetes.list:2
W: Target Packages (main/binary-i386/Packages) is configured multiple times in /etc/apt/sources.list.d/kubernetes.list:1 and /etc/apt/sources.list.d/kubernetes.list:2
W: Target Packages (main/binary-all/Packages) is configured multiple times in /etc/apt/sources.list.d/kubernetes.list:1 and /etc/apt/sources.list.d/kubernetes.list:2
W: Target Translations (main/i18n/Translation-en_US) is configured multiple times in /etc/apt/sources.list.d/kubernetes.list:1 and /etc/apt/sources.list.d/kubernetes.list:2
W: Target Translations (main/i18n/Translation-en) is configured multiple times in /etc/apt/sources.list.d/kubernetes.list:1 and /etc/apt/sources.list.d/kubernetes.list:2
W: Target CNF (main/cnf/Commands-amd64) is configured multiple times in /etc/apt/sources.list.d/kubernetes.list:1 and /etc/apt/sources.list.d/kubernetes.list:2
W: Target CNF (main/cnf/Commands-all) is configured multiple times in /etc/apt/sources.list.d/kubernetes.list:1 and /etc/apt/sources.list.d/kubernetes.list:2
```
报错`W: Target CNF (main/cnf/Commands-all) is configured multiple times in`和`W: Target Packages (main/binary-amd64/Packages) is configured multiple times in`看起来像更新源重复配置了。

根据提示，查看文件`/etc/apt/sources.list.d/kubernetes.list`：

```sh
deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
```

同一个更新源被配置了2次，删掉一个就行了。

更多内容可以参考这个问答：[How can I fix apt error "W: Target Packages ... is configured multiple times"? - Ask Ubuntu](https://askubuntu.com/questions/760896/how-can-i-fix-apt-error-w-target-packages-is-configured-multiple-times)

