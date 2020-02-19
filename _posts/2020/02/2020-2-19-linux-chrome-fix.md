---
layout: post
title: "Linux/ubuntu:Chrome报错解决：Xlib:  extension XInputExtension missing on display :1.0"
categories: [行走的问题解决机]
description: "Linux/ubuntu:Chrome报错解决：Xlib:  extension XInputExtension missing on display :1.0"
keywords: linux, Chrome
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}


之前已经写了一篇在 ubuntu 18.04 环境下安装和使用 chrome会报的错误： [Linux/ubuntu:Chrome报错解决： error while loading shared libraries: libnss3.so libXss.so.1 libasound.so.2](https://zhang0peter.com/2020/02/07/linux-chrome-bug/)

```sh
apt install libnss3-dev libxss1 libasound2
```


结果今天遇到了新的报错：

```sh
Xlib:  extension "XInputExtension" missing on display ":1.0".
Xlib:  extension "RANDR" missing on display ":1.0".
Xlib:  extension "XInputExtension" missing on display ":1.0".
Xlib:  extension "XInputExtension" missing on display ":1.0".
Xlib:  extension "XInputExtension" missing on display ":1.0".
[3035:3035:0218/105432.683037:ERROR:angle_platform_impl.cc(47)] initialize(593): ANGLE Display::initialize error 12289: GLX is not present.
[3035:3035:0218/105432.683217:ERROR:gl_surface_egl.cc(668)] EGL Driver message (Critical) eglInitialize: GLX is not present.
[3035:3035:0218/105432.683228:ERROR:gl_surface_egl.cc(1115)] eglInitialize OpenGL failed with error EGL_NOT_INITIALIZED, trying next display type
[3035:3035:0218/105432.683352:ERROR:angle_platform_impl.cc(47)] initialize(593): ANGLE Display::initialize error 12289: GLX is not present.
[3035:3035:0218/105432.683370:ERROR:gl_surface_egl.cc(668)] EGL Driver message (Critical) eglInitialize: GLX is not present.
[3035:3035:0218/105432.683379:ERROR:gl_surface_egl.cc(1115)] eglInitialize OpenGLES failed with error EGL_NOT_INITIALIZED
[3035:3035:0218/105432.683391:ERROR:gl_initializer_x11.cc(156)] GLSurfaceEGL::InitializeOneOff failed.
[3035:3035:0218/105432.687001:ERROR:viz_main_impl.cc(180)] Exiting GPU process due to errors during initialization
Xlib:  extension "XInputExtension" missing on display ":1.0".
```


找了半天，有人提过问题：[Python Selenium ChromeDriver error message: EGL_NOT_INITIALIZED - Stack Overflow](https://stackoverflow.com/questions/48028782/python-selenium-chromedriver-error-message-egl-not-initialized#)

建议是升级软件版本，我升级了 chrome，没用，一样的报错。

然后我看到了这个贴：[virtualbox.org • View topic - [Solved] Win10-64 crashes at startup on Ubuntu 18.04 (Bionic)](https://forums.virtualbox.org/viewtopic.php?f=7&t=87775)

里面提到问题出在`tightvncserver`上。

我查看了官网，`tightvncserver`最后的版本维持在`1.3.10`，是2009年的版本，后续的更新就要花钱了。

不知道为什么现在还有那么多文章推荐10年都没更新过的`tightvncserver`，推荐`TigerVNC`，基于`RealVNC 4`和`X.org`，并在2009年脱离父项目`TightVNC`。

到GitHub上下载最新版：[Releases · TigerVNC/tigervnc](https://github.com/TigerVNC/tigervnc/releases)

可以直接下载二进制版本，下载后解压，启动`vncserver`即可运行。

也可以安装运行：
```sh
apt install tigervnc-common tigervnc-scraping-server tigervnc-standalone-server tigervnc-xorg-extension
```



我怀疑是因为`xfce4`桌面的原因，于是我换一个桌面环境：
```sh
apt install ubuntu-gnome-desktop
#apt install kubuntu-desktop
```
换桌面后问题解决了。。。。。
