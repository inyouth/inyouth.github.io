---
layout: post
title: Elasticsearch的api增删改查操作
category: ELK
tags: [Elasticsearch , Linux]
description: Elasticsearch的api增删改查操作
---

掌握ES的api是操作ES的基础，下面的教程在终端下的api命令
### 创建索引
创建第一个索引，索引名称叫first_index
```
curl -X PUT 'http://10.108.36.20:9200/first_index' -d '{
	"settings":{
	"number_of_shards":5,
	"number_of_replicas":1
	}
}'
```
其中number_of_shards表示分片的数量，ES默认设置为5，number_of_replicas为备份的数量，默认为1，即相当于有两份数据。
在这个过程中一旦在，索引创建完成以后，这个两个的值就不能再次改变。
返回{"acknowledged":true,"shards_acknowledged":true}，说明创建成功。
![](http://oojf56v4g.bkt.clouddn.com/9.png)

还有一种简单的方式，通过head插件类创建索引
![](http://oojf56v4g.bkt.clouddn.com/13.png)
![](http://oojf56v4g.bkt.clouddn.com/14.png)
### 插入操作
向刚才的first_index索引中插入一条记录
```
curl -X PUT 'http://10.108.36.20:9200/first_index/index_type/1' -d '{
	"user" : "kimchy",
    "post_date" : "2009-11-15T14:12:12",
    "message" : "trying out Elasticsearch"
}'
```
结果返回
```
{"_index":"first_index","_type":"index_type","_id":"1","_version":1,"result":"created","_shards":{"total":2,"successful":2,"failed":0},"created":true}
```
说明创建成功。其中first_index表示类型，index_type是其中的type类型，1表示插入的id号。 -d 后面是插入的字典数据


> 格式 ip:端口/索引/索引类型/id号 后面加插入的字典数据

通过head插件，可以查看刚才插入的数据
![](http://oojf56v4g.bkt.clouddn.com/10.png)

### 查询操作
1.查询全部的索引内容
```
curl -XGET 'http://localhost:9200/_search?pretty'
```
2.查询某个索引下的数据内容，比如first_index索引
```
curl -XGET 'http://localhost:9200/first_index/_search?pretty'
```
![](http://oojf56v4g.bkt.clouddn.com/11.png)
表示当前索引下有1条数据
3.查询某个索引下，某种类型的数据内容，first_index索引下，类型为index_type的数据
```
curl -XGET 'http://localhost:9200/first_index/index_type/_search?pretty'
```
4.查询特定id的内容，比如查询id号为1的内容
```
curl -XGET 'http://localhost:9200/first_index/index_type/1'
```
![](http://oojf56v4g.bkt.clouddn.com/12.png)
可以得到刚才id号为1的数据
其他高级条件查询，以后在写。

### 删除索引和数据
1.删除特定的索引，清空数据，删除first_index索引
```
curl -X DELETE 'localhost:9200/first_index'
```
结果返回
```
{"acknowledged":true}
```
说明成功。
2.删除特定id的某条数据内容,删除firt_index索引下，类型为index_type，id号为1的数据
```
curl -X DELETE 'localhost:9200/first_index/index_type/1'
```
### 更新操作
比如原来在first_index索引中id号为1的内容为
```
{
	"user" : "kimchy",
    "post_date" : "2009-11-15T14:12:12",
    "message" : "trying out Elasticsearch"
}
```
现在更新这条数据的user值位，helloworld
```
curl -XPOST 'localhost:9200/first_index/index_type/1/_update' -d '{
    "doc" : {
        "user" : "helloworld"
    }
}'
```
注意localhost:9200/first_index/index_type/1/_update中的_update表示更新操作
后面紧跟的字典类型是
```
"doc":{键值：值内容}
```
doc表示的ES中的文档

### 其他一些api信息
查看所有的索引信息
```
curl 'http://localhost:9200/_cat/indices?v'
```
查看所有的节点信息
```
curl 'http://localhost:9200/_cat/nodes?v'
```