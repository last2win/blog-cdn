---
layout: post
title: "Linux/ubuntu server 18.04 安装远程桌面--vnc server-TigerVNC"
categories: [Linux]
description: "Linux，ubuntu，remote desktop, vnc, ubuntu server, windows，GUI，terminal， vnc server，Xrdp Server，图形界面，X Server"
keywords: Linux,ubuntu, remote desktop, vnc, ubuntu server, windows, GUI, terminal,  vnc server, Xrdp Server
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}


想装桌面端在服务器上的原因是我在终端中开chrome会报错：
```sh
-> # xhost +
xhost:  unable to open display ""
-> # chromium-browser --no-sandbox   
(chromium-browser:31963): Gtk-WARNING **: 21:05:55.910: cannot open display: 
```
Python报错：
```sh
  File "/usr/local/lib/python3.6/dist-packages/pyppeteer/launcher.py", line 330, in launch
    return await Launcher(options, **kwargs).launch()
  File "/usr/local/lib/python3.6/dist-packages/pyppeteer/launcher.py", line 174, in launch
    self.browserWSEndpoint = self._get_ws_endpoint()
  File "/usr/local/lib/python3.6/dist-packages/pyppeteer/launcher.py", line 219, in _get_ws_endpoint
    self.proc.stdout.read().decode()
pyppeteer.errors.BrowserError: Browser closed unexpectedly:
LaunchProcess: failed to execvp:
/root/.local/share/pyppeteer/local-chromium/588429/chrome-linux/nacl_helper
[30562:30562:0106/224746.491855:ERROR:nacl_fork_delegate_linux.cc(314)] Bad NaCl helper startup ack (0 bytes)
XIO:  fatal IO error 11 (Resource temporarily unavailable) on X server "localhost:10.0"
      after 0 requests (0 known processed) with 0 events remaining.
[30560:30560:0106/230519.955546:ERROR:chrome_browser_main_extra_parts_x11.cc(62)] X IO error received (X server probably went away)
```
我的想法是开个远程的桌面，然后就让图形界面的chrome在远程桌面上一直跑着。

桌面端不推荐`xfce4`，因为运行Chrome会有各种稀奇古怪的错误，推荐`gnome`或者`kubuntu`
```sh
#安装一个即可
apt install xfce4
apt install ubuntu-gnome-desktop
apt install kubuntu-desktop
```


{% raw %}
***          
{% endraw %}
我尝试了许多方法，比如说`Xrdp Server`，`vnc server`

本来打算选择`tightvncserver`，因为有很多文章推荐，但后来在文章：[Linux/ubuntu:Chrome报错解决：Xlib: extension XInputExtension missing on display :1.0](https://zhang0peter.com/2020/02/19/linux-chrome-fix/)中发现`tightvncserver`不好。




发现`tightvncserver`最后的版本维持在`1.3.10`，是2009年的版本，后续的更新就要花钱了。

不知道为什么现在还有那么多文章推荐10年都没更新过的`tightvncserver`，推荐`TigerVNC`，基于`RealVNC 4`和`X.org`，并在2009年脱离父项目`TightVNC`。

安装运行：
```sh
apt install tigervnc-common tigervnc-scraping-server tigervnc-standalone-server tigervnc-xorg-extension tigervnc-viewer ttf-wqy-zenhei
```

因为默认配置是只允许本地访问，编辑文件`/etc/vnc.conf`，添加
```sh
$localhost = "no";
```

新建用户专门用来运行vnc，防止泄露不必要的信息。

```sh
adduser --home /home/vncviewer --shell /bin/bash vncviewer
#passwd vncviewer
#mkdir /home/vncviewer
#chown -R vncviewer:root /home/vncviewer
sudo usermod -a -G sudo vncviewer
su - vncviewer
```



**警告：第一次登录时记得关闭待机和息屏保护！！**

开启：
```sh
vncviewer@ubuntu-s-1vcpu-1gb-sgp1-01:~$ vncserver 

New 'ubuntu-s-1vcpu-1gb-sgp1-01:1 (vncviewer)' desktop at :1 on machine ubuntu-s-1vcpu-1gb-sgp1-01

Starting applications specified in /etc/X11/Xvnc-session
Log file is /home/vncviewer/.vnc/ubuntu-s-1vcpu-1gb-sgp1-01:1.log

Use xtigervncviewer -SecurityTypes VncAuth,TLSVnc -passwd /home/vncviewer/.vnc/passwd ubuntu-s-1vcpu-1gb-sgp1-01:1 to connect to the VNC server.
```
关闭server：
```sh
vncserver -kill :1
```

Windows上的vnc客户端推荐`realvnc`的`VNC Viewer`


{% raw %}
***          
{% endraw %}


参考：     
[How to Install and Configure VNC on Ubuntu 18.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-18-04)         
[Chrome nacl error in ubuntu 14.04 - Ask Ubuntu](https://askubuntu.com/questions/1002496/chrome-nacl-error-in-ubuntu-14-04)        
[How to Install the Desktop Components (GUI) on an Ubuntu Server | Linux Training Academy](https://www.linuxtrainingacademy.com/install-desktop-on-ubuntu-server/)          
[How to Install Xrdp Server (Remote Desktop) on Ubuntu 18.04 | Linuxize](https://linuxize.com/post/how-to-install-xrdp-on-ubuntu-18-04/)                  
