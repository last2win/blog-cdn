---
layout: post
title: "GitHub-jekyll静态博客快速构建与优化--jekyll serve --incremental --profile"
categories: [博客]
description: "GitHub-jekyll 静态博客快速构建与优化--jekyll serve --incremental --profile"
keywords: GitHub, jekyll
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

我的jeykll静态博客部署在GitHub上，平时自己本地测试的时候运行命令`jekyll serve`即可启动：
```sh
C:\Users\peter\Documents\GitHub\zhang0peter.github.io>jekyll serve
Configuration file: C:/Users/peter/Documents/GitHub/zhang0peter.github.io/_config.yml
            Source: C:/Users/peter/Documents/GitHub/zhang0peter.github.io
       Destination: C:/Users/peter/Documents/GitHub/zhang0peter.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
   GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.
                    done in 10.26 seconds.
 Auto-regeneration: enabled for 'C:/Users/peter/Documents/GitHub/zhang0peter.github.io'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
      Regenerating: 1 file(s) changed at 2020-01-25 14:12:39
                    _posts/2020-1-25-github-jekyll-fast-build.md
                    ...done in 4.9158215 seconds.
```
但当博客数量增多后（现在有80个内容页面），修改一个文件中的一行也需要等待很久，就如上面展示的一样，修改一个文件要等5秒钟左右，简直不能忍。

使用命令`jekyll serve --verbose`查看详细输出后发现，一个文件的修改会导致所有文件全部重新编译，所以速度很慢，尤其在大型网站上。

使用命令`jekyll serve --profile`查看自己的网站有哪些地方可以优化：
```sql
Filename                                                      | Count |    Bytes |  Time
--------------------------------------------------------------+-------+----------+------
_layouts/default.html                                         |    99 | 2390.60K | 1.338
_layouts/post.html                                            |    78 | 1096.04K | 1.164
_includes/header.html                                         |    99 |  643.77K | 0.848
_includes/sidebar-popular-repo.html                           |    97 |  415.38K | 0.475
_includes/comments.html                                       |    90 |   55.66K | 0.181
_includes/footer.html                                         |    99 |  329.29K | 0.174
_layouts/wiki.html                                            |     7 |   55.61K | 0.130
```

想要加快编译只有一种方法：`jekyll serve --incremental`，可以增量编译。

当你修改一个文件后，它会自动排除没修改过的文件。

需要注意的是，增量编译仍然是实验性功能，也就是说有bug。我就遇到了几次修改文件后结果网页不显示修改后的结果的情况，具体可以参考：[Default Configuration  Jekyll • Simple, blog-aware, static sites](https://jekyllrb.com/docs/configuration/incremental-regeneration/)

所以总的来说，还是优化被重复编译的文件可以从根本上提高编译速度。

如果过程中遇到报错：
```sh
  Liquid Exception: undefined method `map' for false:FalseClass Did you mean? tap in /_layouts/post.html
jekyll 3.8.5 | Error:  undefined method `map' for false:FalseClass
Did you mean?  tap
```
把`_site`文件夹中生成的文件全删除，重新编译。