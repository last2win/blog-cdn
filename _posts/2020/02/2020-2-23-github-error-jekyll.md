---
layout: post
title: "Jekyll报错解决：GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data."
categories: [行走的问题解决机]
description: "Jekyll报错解决：GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.和Failed to open TCP connection to api.github.com:443 (No connection could be made because the target machine actively refused it. - connect(2) for \"api.github.com\" port 443)"
keywords: jekyll, 博客
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

早上在本地构建博客时报错如下：

```sh
zhang0peter.github.io>jekyll serve
 Incremental build: disabled. Enable with --incremental
      Generating...
   GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.
   GitHub Metadata: Failed to open TCP connection to api.github.com:443 (No connection could be made because the target machine actively refused it. - connect(2) for "api.github.com" port 443)
   GitHub Metadata: Failed to open TCP connection to api.github.com:443 (No connection could be made because the target machine actively refused it. - connect(2) for "api.github.com" port 443)
   GitHub Metadata: Failed to open TCP connection to api.github.com:443 (No connection could be made because the target machine actively refused it. - connect(2) for "api.github.com" port 443)
   GitHub Metadata: Failed to open TCP connection to api.github.com:443 (No connection could be made because the target machine actively refused it. - connect(2) for "api.github.com" port 443)
   GitHub Metadata: An existing connection was forcibly closed by the remote host. - SSL_connect
   GitHub Metadata: Failed to open TCP connection to api.github.com:443 (No connection could be made because the target machine actively refused it. - connect(2) for "api.github.com" port 443)
   GitHub Metadata: An existing connection was forcibly closed by the remote host. - SSL_connect
   GitHub Metadata: An existing connection was forcibly closed by the remote host. - SSL_connect
   GitHub Metadata: An existing connection was forcibly closed by the remote host. - SSL_connect
       Jekyll Feed: Generating feed for posts
  Liquid Exception: undefined method `map' for false:FalseClass Did you mean? tap in /_layouts/post.html
                    ------------------------------------------------
      Jekyll 4.0.0   Please append `--trace` to the `serve` command
                     for any additional information or backtrace.
                    ------------------------------------------------
```
里面有报错`GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.`和`Failed to open TCP connection to api.github.com:443 (No connection could be made because the target machine actively refused it. - connect(2) for "api.github.com" port 443)`

解决方法参考官方文档：[github-metadata/authentication.md at master · jekyll/github-metadata](https://github.com/jekyll/github-metadata/blob/master/docs/authentication.md)

先打开`https://github.com/settings/tokens/new`，权限不用选，默认是`public repository`

生成token后运行：

```sh
JEKYLL_GITHUB_TOKEN=your_token jekyll serve
```
如果是windows用户，可以把变量增加到系统的环境变量中。
