---
layout: post
title: Git 必备
categories: [Git]
description: 
keywords: Git
---

                      
## git强行还原
有时候自己对仓库做了修改，但不需要这些修改，可以强行还原。
```js
git fetch --all && git reset --hard origin/stable && git pull
```
## git 配置代理
有时候公司内网不能直连外网，需要配置git的网络代理。
```sh
git config --global http.proxy 'socks5://127.0.0.1:10888'
git config --global https.proxy 'socks5://127.0.0.1:10888'
```
## git初始化子模块
```js
git submodule update --init --recursive
```



## git 单仓库配置多个远程仓库
我在GitHub和coding上都建了仓库，想单次push到2个仓库，于是需要设置remote

```sh
git remote add all https://xxx.xxxx.git
```
```sh
$ git remote -v
all     https://e.coding.net/xxxx.git (fetch)
all     https://e.coding.net/xxxx.git (push)
origin  https://github.com/xxxx.git (fetch)
origin  https://github.com/xxxx.git (push)
```
```sh
git push all master
```



## git 设置/重置用户名和密码
有时候输入的用户名或者密码错误了，但git已经保存了，无法直接更改。

一种方法是直接删除用户，再输入：
```sh
git config --global --unset user.password
```
如果是Windows用户，可以在如下位置找到保存的密码：
`Control Panel`->`All Control Panel Items`->`Credential Manager`->`Windows Credentials` 
或者直接搜索凭据管理器，可以直接修改用户名和密码。

## 使用ssh连接GitHub上的git服务器

先配置git的用户名和邮箱：
```
git config --global user.name "your_name"
git config --global user.email "your_email@xxx.com"
```
先查看本地是否已经配置了公钥和私钥，如果已经存在私钥，无需再次生成。
```
cd ~/.ssh
```
生成密钥：
```
-> % ssh-keygen -t rsa -b 4096 -C your_email@xxx.com
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/ubuntu/.ssh/id_rsa.
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:14HZgHvg4z5qnFwlcrUeZ7AzxOpp+cka9oEMPrr04YQ zsd780885029@gmail.com
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
生成密钥后拷贝公钥id_rsa.pub的内容，到GitHub上新建ssh的key：GitHub SSH and GPG keys

新建完成后就可以登录GitHub了：
```
-> % ssh -T git@github.com
The authenticity of host 'github.com (13.229.188.59)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,13.229.188.59' (RSA) to the list of known hosts.
Hi zhang0peter! You've successfully authenticated, but GitHub does not provide shell access.
```


{% raw %}
***          
{% endraw %}
参考： 

- [Git - 子模块](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)

- [git submodule init does absolutely nothing - Stack Overflow](https://stackoverflow.com/questions/39765663/git-submodule-init-does-absolutely-nothing/39766926)

- [github - How to link folder from a git repo to another repo? - Stack Overflow](https://stackoverflow.com/questions/36554810/how-to-link-folder-from-a-git-repo-to-another-repo)

- [macos - How do I update the password for Git? - Stack Overflow](https://stackoverflow.com/questions/20195304/how-do-i-update-the-password-for-git)
              
- [github - Git - Pushing code to two remotes - Stack Overflow](https://stackoverflow.com/questions/14290113/git-pushing-code-to-two-remotes)

- 
