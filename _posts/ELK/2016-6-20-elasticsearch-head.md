---
layout: post
title: Elasticsearch的head插件安装
category: ELK
tags: [Elasticsearch , Linux]
description: Elasticsearch开源搜索引擎head插件安装
---

## Elasticsearch介绍

ElasticSearch-head插件可以是一个elasticsearch的集群管理工具，通过这个插件可以更好的浏览数据，查看索引是一个很便捷的插件。
对索引和数据进行可视化。降低阅读成本。
## 插件安装步骤
环境： ubuntu14
ElasticSearch版本5.3.0
ElasticSearch-head的github代码地址  
[https://github.com/mobz/elasticsearch-head](https://github.com/mobz/elasticsearch-head)

1.安装nodejs
```
sudo apt-get install -y build-essential
curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
sudo apt-get install -y nodejs
```
出现下面提示说明安装成功
```
qyz@ubuntu:~$ node -v
v0.10.25
```
2.安装npm
```
sudo apt install npm
```
输入
```
qyz@ubuntu:~$ npm -v
1.3.10
```
表示安装成功
3.获取head插件源码
```
git clone git://github.com/mobz/elasticsearch-head.git
cd elasticsearch-head
npm install
npm run start
```
![](http://oojf56v4g.bkt.clouddn.com/head插件.png)
npm run start代表启动head插件，在这之前需要先启动ES服务。
在浏览器中输入
```
http://localhost:9100/
```
可以看到head插件的管理页面和ES的索引信息。
![](http://oojf56v4g.bkt.clouddn.com/8.png)
