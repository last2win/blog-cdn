---
layout: post
title: "GitHub Pages博客：自定义域名，HTTPS，CAA"
categories: [博客]
description: "初始域名自带HTTPS，CAA可以配置TLS证书"
keywords: 博客
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

早上在弄GitHub Pages的Jekyll博客的自定义域名的HTTPS时遇到了问题，所以在这里记录一下过程。

## GitHub Pages 博客 初始域名

每个GitHub 博客都有初始域名,类似`zhang0peter.github.io`

在2018年之前，GitHub的初始域名是不能使用`https`的，现在GitHub Pages的初始域名会强制`HTTPS`，也会强制`http`跳转`https`:

> Enforce HTTPS         
> — Required for your site because you are using the default domain (zhang0peter.github.io)          
> HTTPS provides a layer of encryption that prevents others from snooping on or tampering with traffic to your site.
When HTTPS is enforced, your site will only be served over HTTPS.                  

所以从安全角度来看，GitHub Page默认的配置是足够安全的。

但缺点是原始域名不够个性化。

## 自定义域名

官方文档：[Managing a custom domain for your GitHub Pages site - GitHub Help](https://help.github.com/en/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site)

大部分人都会使用自定义域名，可以在设置中配置或者在仓库中添加文件`CNAME`，里面只能配置一个域名，想要配置多域名需要自己设置网页跳转。

我使用的是cloudflare的CDN和域名管理。

比如说配置域名`zhang0peter.com`，随后在域名运营商那里设置DNS，增加`CNAME`记录:

| Type  |            Name | Content               | TTL  | Proxy status |
| :---: | --------------: | :-------------------- | :--- | :----------- |
| CNAME | zhang0peter.com | zhang0peter.github.io | Auto | DNS only     |

除了CNAME,也可以配置A记录：

| Type  |            Name | Content         | TTL  | Proxy status |
| :---: | --------------: | :-------------- | :--- | :----------- |
|   A   | zhang0peter.com | 185.199.108.153 | Auto | DNS only     |
|   A   | zhang0peter.com | 185.199.109.153 | Auto | DNS only     |
|   A   | zhang0peter.com | 185.199.110.153 | Auto | DNS only     |
|   A   | zhang0peter.com | 185.199.111.153 | Auto | DNS only     |


自定义域名是无法直接使用`https`，因为 GitHub服务器上无法配置我们的域名的证书

### 自定义域名的HTTPS配置

#### CDN的HTTPS
配置自定义域名的HTTPS是比较麻烦的一件事情，最简单的做法用 cloudflare 的 CDN。

1.把`Proxy status`改为`Proxied`          

2.在`SSL/TLS`中设置`SSL/TLS encryption mode`为`FULL`，把`Edge Certificates`中把`Always Use HTTPS`打开。

更改设置后过10分钟左右，再访问域名就是HTTPS。

这里我要说一下，因为`cloudflare`的免费版的cdn用的人很多，而且没有针对国内网络做优化，所以在国内访问会变慢很多。

推荐使用国内不需要备案的CDN或者使用 CAA的方法实现HTTPS。

我使用的是`nodecache`，注册链接：[Nodecache](https://console-api.nodecache.com/f?aff=4oMnb3)

*通过该链接注册，送1T流量包*

在配置中新增CDN配置，并在`回源 HOST`中配置为自己的域名，如`zhang0peter.com`

在`https`中上传证书，并开启强制HTTPS。

#### CAA的HTTPS

官方文档：[Troubleshooting custom domains and GitHub Pages - GitHub Help](https://help.github.com/en/github/working-with-github-pages/troubleshooting-custom-domains-and-github-pages#https-errors)

GitHub使用`letsencrypt`提供的`CAA`证书签名服务：[Certificate Authority Authorization (CAA) - Let's Encrypt - Free SSL/TLS Certificates](https://letsencrypt.org/docs/caa/)

需要说明的是有些域名服务商是不提供`CAA`的选项。

配置如下：

| Type  |            Name | Content                 | TTL  | Proxy status |
| :---: | --------------: | :---------------------- | :--- | :----------- |
|  CAA  | zhang0peter.com | 0 issue letsencrypt.org | Auto | DNS only     |

查询`DNS`:

```sh
-> % dig caa zhang0peter.com
;; ANSWER SECTION:
zhang0peter.com.        3599    IN      CAA     0 issue "comodoca.com"
zhang0peter.com.        3599    IN      CAA     0 issue "digicert.com"
zhang0peter.com.        3599    IN      CAA     0 issue "letsencrypt.org"
zhang0peter.com.        3599    IN      CAA     0 issuewild "comodoca.com"
zhang0peter.com.        3599    IN      CAA     0 issuewild "digicert.com"
zhang0peter.com.        3599    IN      CAA     0 issuewild "letsencrypt.org"
```


等待大约10分钟，到GitHub仓库中发现` Enforce HTTPS  `配置中的文字内容变为如下：

> Not yet available for your site because the certificate has not finished being issued.            
> Please allow 24 hours for this process to complete. 

如果没变，说明没配置好或者DNS还没刷新成功。



一般来说配置证书不用等24小时，过几个小时后回来看，可以选中按钮，打开后自定义域名的HTTPS配置完成。






