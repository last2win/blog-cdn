---
layout: post
title: "caddy 获取SSL证书报错解决：failed to obtain certificate: acme: Error -> One or more domains had a problem"
categories: [行走的问题解决机]
description: "caddy 获取SSL证书报错解决：failed to obtain certificate: acme: Error -> One or more domains had a problem"
keywords: caddy, SSL
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

早上尝试使用caddy，启动HTTPS服务，并自动配置TLS证书，结果在自动配证书的过程中报错：
```sh
-> # sudo systemctl status caddy
● caddy.service - Caddy HTTP/2 web server
   Loaded: loaded (/etc/systemd/system/caddy.service; enabled; vendor preset: enabled)
   Active: failed (Result: exit-code) since Thu 2020-01-23 11:12:58 CST; 2h 20min ago
     Docs: https://caddyserver.com/docs
  Process: 31832 ExecStart=/usr/local/bin/caddy -log stdout -log-timestamps=false -agree=true -conf=/etc/caddy/Caddyfile -root=/var/tmp (code=exited, status=1/
 Main PID: 31832 (code=exited, status=1/FAILURE)


Jan 23 11:08:57 VM-0-17-ubuntu caddy[31229]: 2020/01/23 11:08:57 [INFO] [xxxx.com] acme: use tls-alpn-01 solver
Jan 23 11:08:57 VM-0-17-ubuntu caddy[31229]: 2020/01/23 11:08:57 [INFO] [xxxx.com] acme: Trying to solve TLS-ALPN-01
Jan 23 11:08:58 VM-0-17-ubuntu caddy[31229]: 2020/01/23 11:08:58 [INFO] Deactivating auth: https://acme-v02.api.letsencrypt.org/acme/authz-v3/2426202924
Jan 23 11:08:58 VM-0-17-ubuntu caddy[31229]: 2020/01/23 11:08:58 [INFO] Unable to deactivate the authorization: https://acme-v02.api.letsencrypt.org/acme/authz
Jan 23 11:08:58 VM-0-17-ubuntu caddy[31229]: [ERROR][xxxx.com] failed to obtain certificate: acme: Error -> One or more domains had a problem:
Jan 23 11:08:58 VM-0-17-ubuntu caddy[31229]: [xxxx.com] acme: error: 403 :: urn:ietf:params:acme:error:unauthorized :: Cannot negotiate ALPN protocol
Jan 23 11:08:59 VM-0-17-ubuntu caddy[31229]: 2020/01/23 11:08:59 [INFO] [xxxx.com] acme: Obtaining bundled SAN certificate
Jan 23 11:09:00 VM-0-17-ubuntu caddy[31229]: 2020/01/23 11:09:00 [INFO] [xxxx.com] AuthURL: https://acme-v02.api.letsencrypt.org/acme/authz-v3/242620
Jan 23 11:09:00 VM-0-17-ubuntu caddy[31229]: 2020/01/23 11:09:00 [INFO] [xxxx.com] acme: use tls-alpn-01 solver
Jan 23 11:09:00 VM-0-17-ubuntu caddy[31229]: 2020/01/23 11:09:00 [INFO] [xxxx.com] acme: Trying to solve TLS-ALPN-01
Jan 23 11:12:50 VM-0-17-ubuntu systemd[1]: Started Caddy HTTP/2 web server.
Jan 23 11:12:50 VM-0-17-ubuntu caddy[31832]: [INFO] Caddy version: v1.0.4
Jan 23 11:12:50 VM-0-17-ubuntu caddy[31832]: Activating privacy features... [INFO][cache:0xc000096730] Started certificate maintenance routine
Jan 23 11:12:51 VM-0-17-ubuntu caddy[31832]: [INFO][xxxx.com] Obtain certificate
Jan 23 11:12:51 VM-0-17-ubuntu caddy[31832]: [INFO][xxxx.com] Obtain: Waiting on rate limiter...
Jan 23 11:12:51 VM-0-17-ubuntu caddy[31832]: [INFO][xxxx.com] Obtain: Done waiting
Jan 23 11:12:51 VM-0-17-ubuntu caddy[31832]: 2020/01/23 11:12:51 [INFO] [xxxx.com] acme: Obtaining bundled SAN certificate
Jan 23 11:12:51 VM-0-17-ubuntu caddy[31832]: [ERROR][xxxx.com] failed to obtain certificate: acme: error: 429 :: POST :: https://acme-v02.api.letsenc
Jan 23 11:12:53 VM-0-17-ubuntu caddy[31832]: [ERROR][xxxx.com] failed to obtain certificate: acme: error: 429 :: POST :: https://acme-v02.api.letsenc
Jan 23 11:12:54 VM-0-17-ubuntu caddy[31832]: 2020/01/23 11:12:54 [INFO] [xxxx.com] acme: Obtaining bundled SAN certificate
Jan 23 11:12:55 VM-0-17-ubuntu caddy[31832]: [ERROR][xxxx.com] failed to obtain certificate: acme: error: 429 :: POST :: https://acme-v02.api.letsenc
Jan 23 11:12:56 VM-0-17-ubuntu caddy[31832]: 2020/01/23 11:12:56 [INFO] [xxxx.com] acme: Obtaining bundled SAN certificate
Jan 23 11:12:56 VM-0-17-ubuntu caddy[31832]: [ERROR][xxxx.com] failed to obtain certificate: acme: error: 429 :: POST :: https://acme-v02.api.letsenc
Jan 23 11:12:57 VM-0-17-ubuntu caddy[31832]: 2020/01/23 11:12:57 [INFO] [xxxx.com] acme: Obtaining bundled SAN certificate
Jan 23 11:12:57 VM-0-17-ubuntu caddy[31832]: [ERROR][xxxx.com] failed to obtain certificate: acme: error: 429 :: POST :: https://acme-v02.api.letsenc
Jan 23 11:12:58 VM-0-17-ubuntu caddy[31832]: failed to obtain certificate: acme: error: 429 :: POST :: https://acme-v02.api.letsencrypt.org/acme/new-order :: u
Jan 23 11:12:58 VM-0-17-ubuntu systemd[1]: caddy.service: Main process exited, code=exited, status=1/FAILURE
Jan 23 11:12:58 VM-0-17-ubuntu systemd[1]: caddy.service: Failed with result 'exit-code'.
```
里面有报错`failed to obtain certificate: acme: error: 429 :: POST ::`和`failed to obtain certificate: acme: Error -> One or more domains had a problem:`
在网上查资料后发现这是因为不能确认域名与DNS对应的关系，所以申请不了证书。

我把域名托管在cloudflare上，域名的解析默认是开着保护，也就是`proxied`，申请域名的时候需要设置为`DNS only`。

等DNS生效后，重启`caddy`即可自动申请证书，正常使用：
```sh
● caddy.service - Caddy HTTP/2 web server
   Loaded: loaded (/etc/systemd/system/caddy.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2020-01-23 13:41:31 CST; 1s ago
     Docs: https://caddyserver.com/docs
 Main PID: 19736 (caddy)
    Tasks: 8 (limit: 1108)
   CGroup: /system.slice/caddy.service
           └─19736 /usr/local/bin/caddy -log stdout -log-timestamps=false -agree=true -conf=/etc/caddy/Caddyfile -root=/var/tmp

Jan 23 13:41:31 VM-0-17-ubuntu caddy[19736]: [INFO] Caddy version: v1.0.4
Jan 23 13:41:31 VM-0-17-ubuntu caddy[19736]: Activating privacy features... [INFO][cache:0xc000096730] Started certificate maintenance routine
Jan 23 13:41:32 VM-0-17-ubuntu caddy[19736]: [INFO][xxxx.com] Obtain certificate
Jan 23 13:41:32 VM-0-17-ubuntu caddy[19736]: [INFO][xxxx.com] Obtain: Waiting on rate limiter...
Jan 23 13:41:32 VM-0-17-ubuntu caddy[19736]: [INFO][xxxx.com] Obtain: Done waiting
Jan 23 13:41:32 VM-0-17-ubuntu caddy[19736]: 2020/01/23 13:41:32 [INFO] [xxxx.com] acme: Obtaining bundled SAN certificate
Jan 23 13:41:33 VM-0-17-ubuntu caddy[19736]: 2020/01/23 13:41:33 [INFO] [xxxx.com] AuthURL: https://acme-v02.api.letsencrypt.org/acme/authz-v3/242795
Jan 23 13:41:33 VM-0-17-ubuntu caddy[19736]: 2020/01/23 13:41:33 [INFO] [xxxx.com] acme: Could not find solver for: tls-alpn-01
Jan 23 13:41:33 VM-0-17-ubuntu caddy[19736]: 2020/01/23 13:41:33 [INFO] [xxxx.com] acme: use http-01 solver
Jan 23 13:41:33 VM-0-17-ubuntu caddy[19736]: 2020/01/23 13:41:33 [INFO] [xxxx.com] acme: Trying to solve HTTP-01
Jan 23 13:41:33 VM-0-17-ubuntu caddy[19736]: 2020/01/23 13:41:33 [INFO] [xxxx.com] acme: Could not find solver for: tls-alpn-01
Jan 23 13:41:33 VM-0-17-ubuntu caddy[19736]: 2020/01/23 13:41:33 [INFO] [xxxx.com] acme: use http-01 solver
Jan 23 13:41:33 VM-0-17-ubuntu caddy[19736]: 2020/01/23 13:41:33 [INFO] [xxxx.com] acme: Trying to solve HTTP-01
Jan 23 13:41:34 VM-0-17-ubuntu caddy[19736]: 2020/01/23 13:41:34 [INFO] [xxxx.com] Served key authentication
Jan 23 13:41:34 VM-0-17-ubuntu caddy[19736]: 2020/01/23 13:41:34 [INFO] [xxxx.com] Served key authentication
Jan 23 13:41:34 VM-0-17-ubuntu caddy[19736]: 2020/01/23 13:41:34 [INFO] [xxxx.com] Served key authentication
Jan 23 13:41:34 VM-0-17-ubuntu caddy[19736]: 2020/01/23 13:41:34 [INFO] [xxxx.com] Served key authentication
Jan 23 13:41:38 VM-0-17-ubuntu caddy[19736]: 2020/01/23 13:41:38 [INFO] [xxxx.com] The server validated our request
Jan 23 13:41:38 VM-0-17-ubuntu caddy[19736]: 2020/01/23 13:41:38 [INFO] [xxxx.com] acme: Validations succeeded; requesting certificates
Jan 23 13:41:38 VM-0-17-ubuntu caddy[19736]: 2020/01/23 13:41:38 [INFO] [xxxx.com] Server responded with a certificate.
Jan 23 13:41:39 VM-0-17-ubuntu caddy[19736]: done.
Jan 23 13:41:39 VM-0-17-ubuntu caddy[19736]: Serving HTTPS on port 443
Jan 23 13:41:39 VM-0-17-ubuntu caddy[19736]: https://xxxx.com
Jan 23 13:41:39 VM-0-17-ubuntu caddy[19736]: [INFO] Serving https://xxxx.com
Jan 23 13:41:39 VM-0-17-ubuntu caddy[19736]: Serving HTTP on port 80
Jan 23 13:41:39 VM-0-17-ubuntu caddy[19736]: http://xxxx.com
Jan 23 13:41:39 VM-0-17-ubuntu caddy[19736]: [INFO] Serving http://xxxx.com
```
现在可以打开https://xxxx.com 访问网站了。
