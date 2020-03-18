---
layout: post
title: "nginx-代理/转发-GitHub Pages 静态页面博客"
categories: [博客]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

写在开头：如果你用的是国内的服务器，那么在80端口或者443端口上提供服务，会被运营商屏蔽端口。

我的GitHub  Pages 静态博客是用jekyll搭建，放在GitHub上。本来我是使用`nodecache`这个CDN的，但后来发现用这个CDN后，谷歌搜索的爬虫无法正常运行，于是我打算用服务器来搭建一个Nginx的CDN/代理。

先安装：
```sh
-> # apt install nginx
```
nginx有一个好用的功能`proxy_pass`，可以转发请求，从而实现HTTPS

默认的配置文件位置：`/etc/nginx/nginx.conf`

这里要说明一点，一种配置方法是本地构建静态博客，然后nginx直接提供服务；另一种方法是直接转发请求到GitHub，Nginx只作为转发和缓存使用。

这2种方法各有优势，我选择直接转发请求，更方便。

先备份配置文件：
```sh
cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup
```

修改nginx配置文件，在`http`框的最后增加如下内容：
```sh
  server {
    listen 8080;
    server_name zhang0peter.com;
    root html;
    location / {
    #转发流量
    proxy_pass http://zhang0peter.github.io; #转发到github
    proxy_set_header Host zhang0peter.com;
    proxy_set_header   X-Host          zhang0peter.com;
    proxy_set_header X-Real-IP $remote_addr; 
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header User-Agent "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.132 Safari/537.36";
    #缓存
    proxy_cache_valid  200 206 304 301 302 1d; # 对httpcode为200…的缓存1天  
    proxy_cache_valid  any 1d;    #其他的缓存多长时间  
    proxy_cache_key $uri;  #定义缓存唯一key,通过唯一key来进行hash存取
    
    }
  }
```
这样就把`nginx`的缓存，流量转发，代理，设置`User-Agent`的工作都完成了。

注意：这里设置`User-Agent`是因为百度的爬虫是禁止访问GitHub，所以在这里统一设置。

现在http的访问配置已经完成，你可以在后面继续使用CDN。

但如果你想要配置HTTPS，自行参考nginx配SSL的文章。