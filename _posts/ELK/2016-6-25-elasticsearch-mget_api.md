---
layout: post
title: Elasticsearch的Mget同时获取多个文档数据
category: ELK
tags: [Elasticsearch , Linux]
description: Elasticsearch的Mget同时获取多个文档数据
---

在前一篇文章中，ES的增删改查都是针对的一条数据，当我们同时向获得多条数据的时候，就需要用到Mget api。  
在ES中有两个索引first_index 和seconde_index。  
first_index中类型为first_type，两条数据，id分别为1，2
second_index中类型为second_type，两条数据，id分别为1，2
插入的代码为
```
curl -X PUT 'localhost:9200/first_index' -d '{
	"settings":{
	"number_of_shards":5,
	"number_of_replicas":1
	}
}'
curl -X PUT 'localhost:9200/second_index' -d '{
	"settings":{
	"number_of_shards":5,
	"number_of_replicas":1
	}
}'
curl -X PUT 'localhost:9200/first_index/first_type/1' -d '{
	"user" : "first_1",
	"post_date" : "1",
	"message" : "first_1_content"
}'

curl -X PUT 'localhost:9200/first_index/first_type/2' -d '{
	"user" : "first_2",
	"post_date" : "2",
	"message" : "first_2_content"
}'

curl -X PUT 'localhost:9200/second_index/second_type/1' -d '{
	"user" : "second_1",
	"post_date" : "1",
	"message" : "second_1_content"
}'

curl -X PUT 'localhost:9200/second_index/second_type/2' -d '{
	"user" : "second_2",
	"post_date" : "2",
	"message" : "second_2_content"
}'
```
### 获取同一个索引下的多个id值数据
获取first_index 索引下的id为 1，2两个数据
```
curl -X GET 'localhost:9200/first_index/first_type/_mget' -d '{"ids":[1,2]}'
```
结果
![](http://oojf56v4g.bkt.clouddn.com/mget-1.png)
注意-d 后面的json格式中ids后面是一个数组，里面包含了，请求的id


### 获取不同index下，不同的id
比如同时获取first_index和second_index下的id为1，2的数据。
```
curl -X GET 'localhost:9200/_mget' -d '{
	"docs":[
	{
	"_index":"first_index",
	"_type":"first_type",
	"_id":1
	},
	{
	"_index":"first_index",
	"_type":"first_type",
	"_id":2
	},
	{
	"_index":"second_index",
	"_type":"second_type",
	"_id":1
	},
	{
	"_index":"second_index",
	"_type":"second_type",
	"_id":2
	}
	]
}'
```
结果
![](http://oojf56v4g.bkt.clouddn.com/mget-2.png)
即当获取多个，只要在docs的数组中添加想要获取的_index，_type和_id就可以。
