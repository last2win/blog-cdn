---
layout: post
title: Wiki
description: 记多少命令和快捷键会让脑袋爆炸呢？
comments: true
menu: 维基
permalink: /wiki/
---

> 记多少命令和快捷键会让脑袋爆炸呢？

# Linux

## bash脚本常用指令

```sh
#!/bin/bash

timeout=240; 
node new_search.js 2>&1 | tee -a search.log &
sleep 10
while true; do 
	if [ -z "`find search.log -newermt @$[$(date +%s)-${timeout}]`" ]; 
		then 
				killall --regexp -TERM 'node *'
				node new_search.js 2>&1 | tee -a search.log &	
	fi
	sleep 60
done

```

## Debian/Ubuntu/Raspbian 时间同步
时区设置：
```sh
date -R
dpkg-reconfigure tzdata
```
时间同步：
```sh
apt-get install ntpdate
ntpdate ntp.sjtu.edu.cn
date
```
## 安装zsh和oh-my-zsh
安装zsh
```sh
apt-get install zsh
chsh -s /bin/zsh
```
安装Oh My Zsh
```sh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
```sh
nano ~/.zshrc
```
```sh
ZSH_THEME="candy"
```
```sh
source ~/.zshrc
```
## 修改 hostname
```sh
sudo hostname hk-ubuntu  #暂时修改 hostname，重启则失效
sudo nano /etc/hostname  #永久修改hostname，重启生效
sudo nano /etc/hosts     #将新的hostname指向127.0.0.1
```

## debian 9/ ubuntu 添加swap分区


Linux 中 Swap（交换分区），类似于 Windows 的虚拟内存，就是当内存不足的时候，把一部分硬盘空间虚拟成内存使用,从而解决内存容量不足的情况。

先查看是否已经存在swap分区了：
```js
sudo swapon --show
```
没有结果表示不存在swap分区，有结果表示已经有一个swap分区了，一般来说一个系统不需要第二个swap分区。

***

创建1G大小的swap分区文件,并更改权限：
```sh
sudo dd if=/dev/zero of=/swapfile bs=1024 count=1024k
sudo chmod 600 /swapfile
```
***
加载swap分区：
```sh
sudo mkswap /swapfile
sudo swapon /swapfile
```
如果想要重启后swap分区扔自动加载，修改文件：
```sh
sudo nano /etc/fstab
```
最后增加一行：
```sh
/swapfile swap swap defaults 0 0
```
***
查看swap分区是否加载成功：
```sh
sudo swapon --show
```
***
一般来说如果是服务器，swappiness 不要太高，修改swappiness 的值：
```sh
sudo sysctl vm.swappiness=10
```
***
参考资料:[How To Add Swap Space on Debian 9 | Linuxize](https://linuxize.com/post/how-to-add-swap-space-on-debian-9/)

## 设置指定用户禁止ssh密码登录

有时候我们需要有些用户可以ssh登录时使用密码，而有些用户只能使用密钥登录，编辑文件`/etc/ssh/sshd_config`

在**文件最后**添加如下内容：
```sh
Match   User    user1,user2
        PasswordAuthentication  no
```
重启服务：
```sh
systemctl reload ssh
```


## Linux下使用tee既在屏幕上显示输出，又把输出写进文件

Linux下的tee是一个很好用的工具，可以把重定向屏幕输出到文件的同时在屏幕上显示输出

使用示例如下：
```sh
command  | tee stdout.log
```

**这里有一个需要注意的坑点，上面的命令只是把标准输出，也就是 STDOUT 写进文件，并没有处理错误输出，也就是 STDERR**

如果需要处理错误输出，命令如下：
```sh
command > >(tee -a stdout.log) 2> >(tee -a stderr.log >&2)
```
如果你想把标准输出和错误输出都重定向到一个文件，那么命令如下：
```sh
command  2>&1 | tee -a log
```

参考文章：[linux - How do I write stderr to a file while using "tee" with a pipe? - Stack Overflow](https://stackoverflow.com/questions/692000/how-do-i-write-stderr-to-a-file-while-using-tee-with-a-pipe)

## 其他

*   [Linux下tar解压到当前目录，zip压缩,tar压缩，tar解压_zhang0peter的博客-CSDN博客](https://blog.csdn.net/zhangpeterx/article/details/94862801)                       
*   [Linux系统vi操作指南 - zhang0peter的博客 - CSDN博客](https://blog.csdn.net/zhangpeterx/article/details/95672890)             
*   [在centos上安装最新的glibc - zhang0peter的博客 - CSDN博客](https://blog.csdn.net/zhangpeterx/article/details/96116219)                   
*   [Linux在shell终端中清空DNS缓存,刷新DNS的方法(ubuntu,debian) - zhang0peter的博客 - CSDN博客](https://blog.csdn.net/zhangpeterx/article/details/83895446)                       
*   [Linux: 使用bash命令ls按时间排序 - zhang0peter的博客 - CSDN博客](https://blog.csdn.net/zhangpeterx/article/details/93165265)             
*   [在WinSCP中使用sudo进行sftp，不用输入密码，获得root权限 - zhang0peter的博客 - CSDN博客](https://blog.csdn.net/zhangpeterx/article/details/95001286)                           
*   [技术 Linux 下如何处理包含空格和特殊字符的文件名](https://linux.cn/article-5777-1.html)                                       
*   [在centos上通过yum直接安装最新版gcc和开发工具 ](https://blog.csdn.net/zhangpeterx/article/details/96141900)             
*   [Linux系统16进制形式查看二进制文件 ](https://blog.csdn.net/zhangpeterx/article/details/95448494)                   
*   [Windows使用CLion 远程调试Linux程序 ](https://blog.csdn.net/zhangpeterx/article/details/95809766)                       
*   [在CentOS/Debian/Ubuntu上编译安装最新版 GCC 8 ， cmake 3 和ninja_zhang0peter的博客-CSDN博客](https://blog.csdn.net/zhangpeterx/article/details/96103611)             
*   [在CentOS/Debian/Ubuntu上编译安装最新版gnu make 和GNU binutils (as and ld)_zhang0peter的博客-CSDN博客](https://blog.csdn.net/zhangpeterx/article/details/96135667)                   
*   [一日一技：在Linux系统中如何查询正在运行的程序的路径](https://mp.weixin.qq.com/s?__biz=MzI2MzEwNTY3OQ==&mid=2648978154&idx=1&sn=53488342b4b67fbd97ffd73affd69fd5&chksm=f2506f0ac527e61c8b7cd11e13aff279fa7e29f1f9bacbe990c012274f73e3dd48f5f8edfd89&mpshare=1&scene=23&srcid=0119f0M6FbJ86dEq34KZNtb2&sharer_sharetime=1579425817650&sharer_shareid=19fe229c09c2cd2c6445c2856dcf3d6d#rd)                                    
*   [网络测速工具 iperf  须臾之学](https://blog.xizhibei.me/2020/01/13/speed-test-tool-iperf/)                       
*   [使用ssh连接GitHub上的git服务器](https://zhang0peter.com/2020/02/03/git-github-ssh/)             
*   [在Linux上使用图形界面的GitHub Desktop](https://blog.csdn.net/zhangpeterx/article/details/95889349)                   
*   [Linux查看可执行文件的各个段：.BSS，.TEXT，.DATA的大小](https://blog.csdn.net/zhangpeterx/article/details/102657265)                       
*   [Linux下的十个好用的命令工具：查看系统版本，显示目录的大小，查看硬盘HDD/SSD，硬盘测速，ssh时自动输入密码，查看程序的内存使用情况，查看I/O的速度，查看ssh密码错误日志，查找文件](https://blog.csdn.net/zhangpeterx/article/details/96279648)             
*   [linux下bash脚本常用的十个技巧：显示执行脚本花费的时间，在脚本退出时杀死后台运行的程序，在脚本退出时跳出循环，读取命令行参数来决定循环次数](https://blog.csdn.net/zhangpeterx/article/details/96278020)                   
*   [Linux/ubuntu server 18.04 安装远程桌面--vnc server-tightvncserver](https://zhang0peter.com/2020/01/07/linux-server-vnc/)                       
*   [Linux的useradd与adduser命令区别](https://zhang0peter.com/2020/06/20/linux-useradd-adduser/)  
*   []()  
*   []()  
*   []()  
*   []()  



# Windows

*   [WSL系列操作：安装，卸载_zhang0peter的博客-CSDN博客](https://blog.csdn.net/zhangpeterx/article/details/97616268)                       
*   [Windows系统下多版本GCC的安装： MinGW Cygwin Msys2 和 VS: MSVC ](https://blog.csdn.net/zhangpeterx/article/details/95591302)             
*   [Windows使用MSVC，命令行编译，链接64位dll，Python调用](https://blog.csdn.net/zhangpeterx/article/details/100591270)                   
*   [WSL的openssh-server使用报错：Could not load host key: /etc/ssh/ssh_host_rsa_key](https://blog.csdn.net/zhangpeterx/article/details/95810789)  
*   [windows anaconda python 3.7 安装 pytorch-gpu - zhangpeterx的博客 - CSDN博客](https://blog.csdn.net/zhangpeterx/article/details/89162479)
*   [windows anaconda python 3.7 安装keras-gpu tensorflow-gpu - zhangpeterx的博客 - CSDN博客](https://blog.csdn.net/zhangpeterx/article/details/89096998)
*   []()
*   []()
*   []()
*   []()
*   []()
*   []()

# Java

*   [Java源码下载和阅读（JDK1.8/Java 11)_zhang0peter的博客-CSDN博客](https://zhang0peter.blog.csdn.net/article/details/88823585)             
*   [有关groupId，artifactId和版本的命名约定的指南](https://maven.apache.org/guides/mini/guide-naming-conventions.html)                   
*   [jetbrains intellij IDEA 常用插件 - zhang0peter的博客 - CSDN博客](https://blog.csdn.net/zhangpeterx/article/details/88787181)                       
*   [IDEA报错总结：修改Java编译版本--maven项目 - zhang0peter的博客](https://zhang0peter.blog.csdn.net/article/details/103643939)             
*   [jstatd，VisualVM使用和报错解决：Could not create remote object--java.security.AccessControlException: - zhang0peter的博客](https://zhang0peter.blog.csdn.net/article/details/103651098)                   
*   [Java：获取数组中的子数组的多种方法](https://blog.csdn.net/zhangpeterx/article/details/88716563)                       
*   [Java数组排序： Array-ArrayList-List-Collections.sort()/List.sort()/Arrays.sort()](https://zhang0peter.com/2020/02/07/java-array-list-sort/)             
*   [Sort an array in Java - Stack Overflow](https://stackoverflow.com/questions/8938235/sort-an-array-in-java)            
*   [java - How to sort an ArrayList? - Stack Overflow](https://stackoverflow.com/questions/16252269/how-to-sort-an-arraylist)   



# C++
*   [C++预编译头文件 bits/stdc++.h](https://blog.csdn.net/zhangpeterx/article/details/100729005)                       
*   [C++调用openssl使用sha256，并取结果前64位作为uint64 ](https://blog.csdn.net/zhangpeterx/article/details/99311279)             
*   [make 操作技巧指南--gcc版本设置 ](https://blog.csdn.net/zhangpeterx/article/details/97256638)                   
*   [How to calculate a time difference in C++ - Stack Overflow](https://stackoverflow.com/questions/728068/how-to-calculate-a-time-difference-in-c)                       
*   [How do I create a random alpha-numeric string in C++? - Stack Overflow](https://stackoverflow.com/questions/440133/how-do-i-create-a-random-alpha-numeric-string-in-c)             
*   [multithreading - Simple example of threading in C++ - Stack Overflow](https://stackoverflow.com/questions/266168/simple-example-of-threading-in-c)                   
*   [C++ 随机数生成的2种方法--生成指定范围内的随机数 ](https://blog.csdn.net/zhangpeterx/article/details/99744937)                       
*   [C++ 常用技巧 ](https://blog.csdn.net/zhangpeterx/article/details/99758196)             
*   [C++ CORE DUMP gdb 调试 ](https://blog.csdn.net/zhangpeterx/article/details/99993012)                   
*   [Windows和Linux的C/C++ IDE选择 ](https://blog.csdn.net/zhangpeterx/article/details/100009491)                       
*   [C++ 调试技术：addr2line ](https://blog.csdn.net/zhangpeterx/article/details/99974611)             
*   [Windows下使用Visual Studio自带的MSVC，命令行编译C/C++程序 ](https://blog.csdn.net/zhangpeterx/article/details/86602394)                   
*   [C++ 内存泄漏检测：valgrind和AddressSanitizer ](https://blog.csdn.net/zhangpeterx/article/details/100098961)                       
*   [C++ gdb调试技巧 ](https://blog.csdn.net/zhangpeterx/article/details/100034115)             
*   [C++ 虚函数个人理解 ](https://blog.csdn.net/zhangpeterx/article/details/100106861)                   
*   [C++ 汇编代码查看 ](https://blog.csdn.net/zhangpeterx/article/details/100120219)                       
*   [C++ 协程介绍[译] ](https://blog.csdn.net/zhangpeterx/article/details/100138656)             
*   [Compiler Explorer](https://godbolt.org/)                   
*   [在CLion中运行Ninja项目 ](https://blog.csdn.net/zhangpeterx/article/details/95810640)                                      
*   [make 操作技巧指南--gcc版本设置](https://blog.csdn.net/zhangpeterx/article/details/97256638)             
*   [C++ 随机数生成的2种方法--生成指定范围内的随机数](https://blog.csdn.net/zhangpeterx/article/details/99744937)             
*   [在CentOS/Debian/Ubuntu上编译安装最新版 GCC 8 ， cmake 3 和ninja](https://blog.csdn.net/zhangpeterx/article/details/96103611) 


# 数据库
*   [数据库基准测试：database bencnmark --生成大量随机测试数据 - zhang0peter的博客 - CSDN博客](https://blog.csdn.net/zhangpeterx/article/details/96741226)                       
*   [数据库可视化工具：metabase/metabase: The simplest, fastest way to get business intelligence and analytics to everyone in your company](https://github.com/metabase/metabase)             
*   [数据库可视化工具：安装和配置 - Apache Superset文档](https://superset.incubator.apache.org/installation.html)                   
*   [mysql gui 客户端推荐一个 - V2EX](https://www.v2ex.com/t/565368) 








# Python

推荐Python课程：                      
*   [北理工嵩天教授的Python语言程序设计课程](https://www.youtube.com/playlist?list=PLr_PV1S3i8VIf9ACS3VzD80usgH5C5YGN)             
*   []()                   
*   []()                       
*   []()             
*   []()  



*   [Python 超快生成大量随机数的方法](https://blog.csdn.net/zhangpeterx/article/details/96474599)             






# 杂类

                      
*   [你们都是用什么编程字体的？ - V2EX](https://www.v2ex.com/t/597067)             
*   [windows-程序员必备的20个软件 — zhang0peter的个人博客](https://zhang0peter.com/2020/01/23/programmer-need/)                   
*   [介绍 uTools](https://u.tools/docs/guide/about-uTools.html)                       
*   [时间都去哪了?用RescueTime和WakaTime来记录你的时间](https://luolei.org/track-your-time/)             
*   [mattgodbolt/compiler-explorer: Run compilers interactively from your web browser and interact with the assembly](https://github.com/mattgodbolt/compiler-explorer)                   
*   [Multi-language programming playground Code LabStack](https://code.labstack.com/)                       
*   [Awesome-Windows-Windows上优质&精选的最佳应用程序及工具列表](https://github.com/Awesome-Windows/Awesome)             
*   [求笔记软件推荐 - V2EX](https://www.v2ex.com/t/541898)                   
*   [有用薄膜键盘敲代码的吗?比例大概是多少? - V2EX](https://www.v2ex.com/t/565789?p=1)              
*   [mysql gui 客户端推荐一个 - V2EX](https://www.v2ex.com/t/565368)                              
*   [各位在用什么邮箱客户端？ - V2EX](https://www.v2ex.com/t/634423)             
*   [请问这种 Git 流程图是用什么工具画的呢？ - V2EX](https://www.v2ex.com/t/665886)                   
*   []()                       
*   []()             
*   []()                   


Windows下使用`ffmpeg`批量转换ts格式视频到mp4格式，并在转换完成后删除原视频

```sh
$files = Get-ChildItem -Recurse  -Filter *.ts
echo $files
foreach ($f in $files){
    $outfile = $f.FullName.replace(".ts",".mp4") 
    echo $outfile
    ffmpeg -i $f.FullName -c copy $outfile 
    Remove-Item -Path $f.FullName
}
```

convert flv to mp4:

```sh
ffmpeg -i input.flv -codec copy output.mp4
```

convert mp4 to mp3:

```sh
ffmpeg -i input.mp4 output.mp3
```



# git

## git强行还原
有时候自己对仓库做了修改，但不需要这些修改，可以强行还原。
```js
git fetch --all && git reset --hard origin/master && git pull
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

如果另外一个服务器已经有公钥和私钥，你不想重新生成，可以直接拷贝过来：
```sh
cd ~/.ssh
nano id_rsa
chmod 0600 id_rsa
cd ..
ssh -T git@github.com
```

## git 常用操作

Git操作参考：[Git的奇技淫巧](https://github.com/521xueweihan/git-tips)

有一篇好文章：[一篇文章，教你学会Git](https://www.jianshu.com/p/072587b47515)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020020316291123.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly96aGFuZzBwZXRlci5ibG9nLmNzZG4ubmV0,size_16,color_FFFFFF,t_70)


每次commit，git存储的是全新的文件快照而不是文件的变更部分。这是做了一个取舍，这样每次在切换分支的时候，读取文件是O(1)的时间复杂度，而不是O(N)的时间复杂度。

git误操作如何恢复

使用git reflog，修改HEAD的指针

*   [**这才是真正的Git——Git内部原理揭秘！ - 知乎**](https://zhuanlan.zhihu.com/p/96631135?utm_source=qq&utm_medium=social&utm_oi=30019480977408)





*   [Commit only part of a file in Git - Stack Overflow](https://stackoverflow.com/questions/1085162/commit-only-part-of-a-file-in-git)
*   [Undo a Git merge that hasn't been pushed yet - Stack Overflow](https://stackoverflow.com/questions/2389361/undo-a-git-merge-that-hasnt-been-pushed-yet)      
*   [git log - View the change history of a file using Git versioning - Stack Overflow](https://stackoverflow.com/questions/278192/view-the-change-history-of-a-file-using-git-versioning)      
*   [Undoing a git rebase - Stack Overflow](https://stackoverflow.com/questions/134882/undoing-a-git-rebase)      
*   [git add - Difference between "git add -A" and "git add ." - Stack Overflow](https://stackoverflow.com/questions/572549/difference-between-git-add-a-and-git-add)      
*   [version control - How do I force "git pull" to overwrite local files? - Stack Overflow](https://stackoverflow.com/questions/1125968/how-do-i-force-git-pull-to-overwrite-local-files)      
*   [What is the difference between 'git pull' and 'git fetch'? - Stack Overflow](https://stackoverflow.com/questions/292357/what-is-the-difference-between-git-pull-and-git-fetch)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823165702987.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823165653973.png)



{% raw %}
***          
{% endraw %}
参考： 

- [Git - 子模块](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)

- [git submodule init does absolutely nothing - Stack Overflow](https://stackoverflow.com/questions/39765663/git-submodule-init-does-absolutely-nothing/39766926)

- [github - How to link folder from a git repo to another repo? - Stack Overflow](https://stackoverflow.com/questions/36554810/how-to-link-folder-from-a-git-repo-to-another-repo)

- [macos - How do I update the password for Git? - Stack Overflow](https://stackoverflow.com/questions/20195304/how-do-i-update-the-password-for-git)
              
- [github - Git - Pushing code to two remotes - Stack Overflow](https://stackoverflow.com/questions/14290113/git-pushing-code-to-two-remotes)






