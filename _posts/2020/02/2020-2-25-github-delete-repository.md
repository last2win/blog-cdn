---
layout: post
title: "GitHub删除锁定的仓库-github delete locked respority"
categories: [行走的问题解决机, git]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

有时候我们`fork`别人的仓库后，仓库被别人锁死了：

> This repository has been disabled.

>Access to this repository has been disabled by GitHub Staff due to a violation of GitHub's terms of service. Please contact support@github.com for more information or to request a review of this decision.

这时候一个锁死的仓库，自己打不开，又无法删除。


## GraphQL
我想到的解决方法是使用`GraphQL`进行操作，官网：[GitHub GraphQL API v4  GitHub Developer Guide](https://developer.github.com/v4/)

可以使用`curl`进行`GraphQL`操作

先申请`token`:[New personal access token](https://github.com/settings/tokens/new)

勾选权限`Delete repositories`，生成token后运行查询：

记得把`token`换成自己的token:
```sh
curl -H "Authorization: bearer token" -X POST -d \
"{\"query\": \"query { viewer { login }}\" } " \
https://api.github.com/graphql
```
如果结果显示自己的用户名，表示申请成功：
```sh
{"data":{"viewer":{"login":"zhang0peter"}}}
```
注意，你可以在[GraphQL API Explorer | GitHub Developer Guide](https://developer.github.com/v4/explorer/)进行初步的操作使用，但`Explorer`只能进行查询，无法修改和删除。

GraphQL不行


## hub
有人推荐`hub`，我尝试了一下：
```sh
snap install hub --classic
```
也不行。




## 发邮件

我最后看到了这个问答：[repository - How do I delete a forked GitHub repo that's unavailable due to DCMA takedown? - Stack Overflow](https://stackoverflow.com/questions/25354045/how-do-i-delete-a-forked-github-repo-thats-unavailable-due-to-dcma-takedown)

无法删除，唯一的办法是发邮件给`GitHub Support`





