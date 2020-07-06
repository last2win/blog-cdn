---
layout: post
title: "win10安装和使用 Kibana 7.8.0"
categories: [大数据]
description: "下载Kibana-安装Kibana插件-启动Kibana-使用 dev tools"
permalink: /windows-install-Kibana/
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

前置教程：[win10安装和使用 Elasticsearch 7.8.0](https://zhang0peter.com/windows-install-Elasticsearch/)



## 下载Kibana


Kibana 官网：[Download Kibana Free | Get Started Now | Elastic | Elastic](https://www.elastic.co/cn/downloads/kibana)


当前最新版本为7.8.0，下载windows版本的二进制包，解压。




## 启动 Kibana

**启动 Kibana 前，需要先启动 Elasticsearch**

启动es后，运行`bin/kibana.bat`

访问网页，查看运行状态`http://127.0.0.1:5601/app/kibana`


选择`try our sample data`，使用官方的测试数据。

把提供的三个测试数据都添加，查看dashboard图表。


## 使用 dev tools

访问:`http://127.0.0.1:5601/app/kibana#/dev_tools/console`

输入命令:`GET /_cat/nodes`，显示结果如下：

```txt
127.0.0.1 33 77 12    dilmrt * MATEBOOK-X
```

## 安装 Kibana 插件

```sh
bin/Kibana-plugin.bat install analysis-icu
```
如果网络不好，可以先下载到本地再安装：
```sh
wget https://artifacts.elastic.co/downloads/Kibana-plugins/analysis-icu/analysis-icu-7.8.0.zip
bin/Kibana-plugin.bat install file:///C:/Users/peter/Downloads/analysis-icu-7.8.0.zip
```

重启节点，查看插件安装情况:`http://127.0.0.1:9200/_cat/plugins`

```txt
MATEBOOK-X analysis-icu 7.8.0
```
