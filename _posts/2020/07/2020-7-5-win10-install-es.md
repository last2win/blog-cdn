---
layout: post
title: "win10安装和使用 Elasticsearch 7.8.0"
categories: [大数据]
description: "下载Elasticsearch-安装Elasticsearch插件-启动Elasticsearch单机伪集群"
permalink: /windows-install-Elasticsearch/
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}




## 准备工作

Elasticsearch 基于Java，但从 Elasticsearch 7.0开始，程序内置了Java环境，不需要额外配置Java环境。

## 下载Elasticsearch


Elasticsearch 官网：[Download Elasticsearch Free | Get Started Now | Elastic | Elastic](https://www.elastic.co/cn/downloads/elasticsearch)


当前最新版本为7.8.0，下载windows版本的二进制包，解压。




## 启动Elasticsearch单机伪集群

运行`bin/elasticsearch.bat`

访问网页，查看运行状态`http://127.0.0.1:9200/`

```json
{
  "name" : "MATEBOOK-X",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "7N2A6dVST_aF61K34rxA1w",
  "version" : {
    "number" : "7.8.0",
    "build_flavor" : "default",
    "build_type" : "zip",
    "build_hash" : "757314695644ea9a1dc2fecd26d1a43856725e65",
    "build_date" : "2020-06-14T19:35:50.234439Z",
    "build_snapshot" : false,
    "lucene_version" : "8.5.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```
访问`http://127.0.0.1:9200/_cat/nodes`查看节点情况：
```txt
127.0.0.1 11 64 18    dilmrt * MATEBOOK-X
```
## 安装 Elasticsearch 插件

```sh
bin/elasticsearch-plugin.bat install analysis-icu
```
如果网络不好，可以先下载到本地再安装：
```sh
wget https://artifacts.elastic.co/downloads/elasticsearch-plugins/analysis-icu/analysis-icu-7.8.0.zip
bin/elasticsearch-plugin.bat install file:///C:/Users/peter/Downloads/analysis-icu-7.8.0.zip
```

重启节点，查看插件安装情况:`http://127.0.0.1:9200/_cat/plugins`

```txt
MATEBOOK-X analysis-icu 7.8.0
```

## 单机启动多Elasticsearch实例

关闭之前的运行的脚本，指定节点名和日志路径，运行集群
```sh
bin/elasticsearch.bat -E node.name=node1 -E cluster.name=zhang -E path.data=node1_data -d
bin/elasticsearch.bat -E node.name=node2 -E cluster.name=zhang -E path.data=node2_data -d
bin/elasticsearch.bat -E node.name=node3 -E cluster.name=zhang -E path.data=node3_data -d
```

`-d`表示后台运行。


访问网页，查看运行状态`http://127.0.0.1:9200/`

```json
{
  "name" : "node1",
  "cluster_name" : "zhang",
  "cluster_uuid" : "qlvJehatSJmcj9Ug113Jrg",
  "version" : {
    "number" : "7.8.0",
    "build_flavor" : "default",
    "build_type" : "zip",
    "build_hash" : "757314695644ea9a1dc2fecd26d1a43856725e65",
    "build_date" : "2020-06-14T19:35:50.234439Z",
    "build_snapshot" : false,
    "lucene_version" : "8.5.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```
访问`http://127.0.0.1:9200/_cat/nodes`查看节点情况：
```txt
127.0.0.1 52 86 25    dilmrt - node3
127.0.0.1 60 86 25    dilmrt * node1
127.0.0.1 60 86 25    dilmrt - node2
```

需要注意的是默认9200端口是node1节点，后续节点的端口是9201和9202.


