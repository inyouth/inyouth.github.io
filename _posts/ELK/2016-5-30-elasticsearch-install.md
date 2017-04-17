---
layout: post
title: Elasticsearch的环境搭建
category: ELK
tags: [Elasticsearch , Linux]
description: Elasticsearch开源搜索引擎在ubuntu环境下的搭建
---

## Elasticsearch介绍

ElasticSearch是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。
官网：https://www.elastic.co/products/elasticsearch

## 环境搭建
系统：ubuntu 14.04  
Elasticsearch 版本： 5.3
1. 下载Elasticsearch代码, 放在es文件夹下
下载地址：  
[https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.3.0.tar.gz](https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.3.0.tar.gz)  
![](http://oojf56v4g.bkt.clouddn.com/code.png)
解压缩:
```
tar xvf elasticsearch-5.3.0.tar.gz
```
重命名一下文件夹 
```
mv elasticsearch-5.3.0 elasticsearch
```
进入到下载Elasticsearch代码目录，
```
cd elasticsearch  
```
2. 运行
在elasticsearch目录下运行(要在普通用户下运行，不能再root用户下运行命令)
```
bin/elasticsearch
```
说明已经启动成功
![](http://oojf56v4g.bkt.clouddn.com/start.png)
在浏览器中输入
```
http://localhost:9200/
```
说明成功
![](http://oojf56v4g.bkt.clouddn.com/localhost结果.png)
或者使用linux的命令行，输入
```
curl http://localhost:9200/
```
返回如下结果
![](http://oojf56v4g.bkt.clouddn.com/curl.png)

## 设置外网访问

默认情况下elasticsearch的访问地址是localhost端口是9200,我们需要设置自己的ip地址才能让外网进行访问
在elasticsearch目录下，修改config文件下的配置文件
```
vim config/elasticsearch.yml
```
将network.host改为自己的实际的ip地址即可
```
network.host: 10.108.36.20
```
如果想更改端口，设置想要的端口即可。
```
http.port: 9200
```
保存退出
重新启动ES
```
bin/elasticsearch
```
在浏览器中输入
```
http://10.108.36.20:9200/
http://ip:端口/
```
![](http://oojf56v4g.bkt.clouddn.com/5.png)
