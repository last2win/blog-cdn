---
layout: post
title: "Linux/ubuntu server 18.04 安装远程桌面--vnc server"
categories: [Linux]
description: "Linux，ubuntu，remote desktop, vnc, ubuntu server, windows，GUI，terminal， vnc server，Xrdp Server，图形界面，X Server"
keywords: Linux,ubuntu, remote desktop, vnc, ubuntu server, windows, GUI, terminal,  vnc server, Xrdp Server
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}


关键词：Linux，ubuntu，remote desktop, vnc, ubuntu server, windows，GUI，terminal， vnc server，Xrdp Server，图形界面，X Server

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

{% raw %}
***          
{% endraw %}
我尝试了许多方法，比如说`Xrdp Server`，`vnc server`

最后选择了`tightvncserver`
```sh
apt install tightvncserver
vncserver #设置vnc密码
vncserver -kill :1
.........
```
具体的安装步骤参考：[How to Install and Configure VNC on Ubuntu 18.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-18-04)

{% raw %}
***          
{% endraw %}


参考：     
[Chrome nacl error in ubuntu 14.04 - Ask Ubuntu](https://askubuntu.com/questions/1002496/chrome-nacl-error-in-ubuntu-14-04)        
[How to Install the Desktop Components (GUI) on an Ubuntu Server | Linux Training Academy](https://www.linuxtrainingacademy.com/install-desktop-on-ubuntu-server/)          
[How to Install Xrdp Server (Remote Desktop) on Ubuntu 18.04 | Linuxize](https://linuxize.com/post/how-to-install-xrdp-on-ubuntu-18-04/)                  
