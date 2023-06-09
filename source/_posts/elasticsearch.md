---
title: ElasticSearch笔记
---
## 什么是Elasticsearch🔥🔥🔥🔥
elasticsearch是一款非常强大的开源搜索引擎 可以帮我我们从海量数据中快速找到需要的内容。
elasticsearch 结合kibana、Logstash、Beats，也就是elastic stack (ELK) 被广泛应用再日志数据分析、实时监控等邻域。
elasticsearch是elastic stack的核心 ,负责存储、搜索、分析

## elasticsearch的发展
Lucene 是一个java语言的搜索引擎类库，是Apachev公司的顶级项目，由DougCutting于1999年研发。
官网地址：https://lucene.apache.org/
Lucene的优势：
- 易扩展
- 高性能（基于倒排索引）
Lucene的缺点：
- 只限于java语言开发
- 学习曲线陡峭
- 不支持水平扩展

2004年shay banon基于lucene开发了Compass

2010年shay banon重写了Compass取名为elasticsearch

官网地址：https:www.elastic.co/cn/
相比用户lucene，elasticsearch具备下面优势
- 支持分布式，可水平扩展
- 提供Restful接口可以被任何语言调用
其他一些的搜索引擎技术
- splunk 商业项目
- Solr Apache的开源搜索引擎
Elasticsearch 对比 mysql

开发架构

总结
- 文档：一条数据就是一个文档，es中式json格式
- 字段：json文档中的字段
- 索引：同类型文档的集合
- 映射：索引中文档的约束，比如字段名称、类型
elasticsearch与数据库的关系
- 数据库负责事务操作
- elasticsearch负责海量数据的搜索、分析、计算
## 1.部署单点es
### 1.1.创建网络
因为我们还需要部署kibana容器，因此需要让es和kibana容器互联。这里先创建一个网络：
```bash
docker network create es-net
```

### 1.2.加载镜像
### 下载镜像
```bash
docker pull elastsearch@lastest 
```
### 运行容器
docker run --name elasticsearch -d \
-e ES_JAVA_OPTS="-Xms512m -Xmx512m" \
-e "discovery.type=single-node" \ 
-v es-data:/usr/share/elasticsearch/data \ 
-v es-plugins:/usr/share/elasticsearch/plugins \
--privileged \
--network es-net \
-p 9200:9200 \
-p 9300:9300 \
elasticsearch:7.7.0
安装成功进入9200端口

## 2.部署kibana
kibana可以给我们提供一个elasticsearch的可视化界面，便于我们学习
### 2.1.部署
运行docker命令，部署kibana
```bash
docker run -d --name kibana -e ELASETICSEARCH_HOSTS=http://es:9200 --network=es-net -p 5601:5601 kibanna:7.6.1
```
查看日志
```bash
docker logs -f kibana
```
安装成功进入5601端口 进入dev tools测试分词器

测试分词器
```java
POST /_analyze
{
  "text": "faith学习java太棒了",
  "analyzer": "english"
}
{
  "tokens" : [
    {
      "token" : "faith",
      "start_offset" : 0,
      "end_offset" : 5,
      "type" : "<ALPHANUM>",
      "position" : 0
    },
    {
      "token" : "学",
      "start_offset" : 5,
      "end_offset" : 6,
      "type" : "<IDEOGRAPHIC>",
      "position" : 1
    },
    {
      "token" : "习",
      "start_offset" : 6,
      "end_offset" : 7,
      "type" : "<IDEOGRAPHIC>",
      "position" : 2
    },
    {
      "token" : "java",
      "start_offset" : 7,
      "end_offset" : 11,
      "type" : "<ALPHANUM>",
      "position" : 3
    },
    {
      "token" : "太",
      "start_offset" : 11,
      "end_offset" : 12,
      "type" : "<IDEOGRAPHIC>",
      "position" : 4
    },
    {
      "token" : "棒",
      "start_offset" : 12,
      "end_offset" : 13,
      "type" : "<IDEOGRAPHIC>",
      "position" : 5
    },
    {
      "token" : "了",
      "start_offset" : 13,
      "end_offset" : 14,
      "type" : "<IDEOGRAPHIC>",
      "position" : 6
    }
  ]
}
```
### 2.1.1安装ik分词包（自动下载）
elasticsearch对待中文不太友好 需要安装分词器包
[下载地址](https://github.com/medcl/elasticsearch-analysis-ik)
```bash
##直接下载
docker exec -it elasticsearch /bin/bash
./bin/elasticsearch-plugin install https:github.com/....
```
安装后重启docker容器
### 2.1.2 手动下载
安装插件需要知道elasticsearch的plugins目录位置，我们使用了-v挂载数据卷，因此需要查看elasticsearch的数据卷目录 通过下面命令查看：
```bash
docker volume inspect es-plugins
```
显示结果：
```java
[ {
        "CreatedAt": "2023-05-26T16:39:02+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/es-plugins/_data",
        "Name": "es-plugins",
        "Options": null,
        "Scope": "local"
    }]
```

将压缩文件解压出来重命名ik 把文件放入/var/lib/docker/volumes/es-plugins/_data目录中 重启
docker restart elasticsearch
### 2.1.3 测试ik
ik分词器包含两种模式
- ik_smart: 最少切分  占用内存空间少 搜索力度小
- ik_max_word:最细切分 占用内存空间大 但是搜索力大大

可以看出这次分词按照中文了！！！！
2.1.4.ik中增加去除词汇量

找到/var/lib/docker/volumes/es-plugins/_data/ik/config目录中的IKAnalyzer.cfg.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
	<comment>IK Analyzer 扩展配置</comment>
	<!--用户可以在这里配置自己的扩展字典 -->
	<entry key="ext_dict">ext.dic</entry>
	 <!--用户可以在这里配置自己的扩展停止词字典-->
	<entry key="ext_stopwords">stopword.dic</entry>
	<!--用户可以在这里配置远程扩展字典 -->
	<!-- <entry key="remote_ext_dict">words_location</entry> -->
	<!--用户可以在这里配置远程扩展停止词字典-->
	<!-- <entry key="remote_ext_stopwords">words_location</entry> -->
</properties>
```

如上的xml  ext_dict和ext_stopwords指定了两个文件 再config中创建

打开ext.dic增加词汇

打开stopword.dic增加无意义词汇

docker restart elasticsearch
测试

总结
分词器的作用：
- 创建倒排索引时对文档分词
- 用户搜索时，对输入的内容分词
IK分词器的有几种模式
- ik_smart 
- ik_max_word
IK分词器如何拓展词条？如何停用词条
- 利用config目录的ikanalyzer.cfg.xml文件拓展词典和停用词典
- 在词典中添加词条或者停用词条
## 3.索引库操作
### 3.1 mapping属性
mapping是对索引库中文档的约束，常见的mapping属性包括
- type：字段属性 常见的类型有
    - 字符串：text(可分词的文本)、keyword（精确值，例如：品牌、国家、IP地址）
    - 数值: long,integer,short,byte,double,float
    - 布尔: boolean
    - 日期: date
    - 对象: object
- index: 是否创建索引
- analyzer:使用哪种分词器
- properties:  该字段的子字段
### 3.2 创建索引库
ES中通过Restful请求操作索引库、文档。请求内容用DSL语句来表示。创建索引库和mapping的DSL语法如下：
```java
{
 "age":21,
 "weight":52.1,
 "isMarried":false,
 "info":"浙江省衢州市GGboy",
 "email":"faith@tom.com",
 "score":[99.1,99.5,96.3],
 "name":{
    "firstName":"徐",
    "lastName":"一飞"
 }
}
```
```C#
#创建索引库
PUT /faith
{
  "mappings": {
    "properties": {
      "info":{
      "type":"text",
      "analyzer":"ik_smart"
      },
      "email":{
      "type":"keyword",
      "index":false
    },
      "name":{
      "type":"object",
      "properties": {
        "firstName":{
          "type":"keyword"
        },
        "lastName":{
          "type":"keyword"
        }
      }
    }
    }
  }
}
```
### 3.3 查看、删除索引库
查看索引库语法：

GET /索引库名

删除索引库语法：

DELETE /索引库名

更新索引库：

索引库只能添加字段
```java
PUT /索引库名/_mapper{
    "properties":{
        "新字段名":{
            "type":""
        }
    }
}
```
### 3.4 文档操作
```java
#创建文档 也可以做全局更新
PUT /faith/_doc/1
{
  "info":"浙江省衢州市徐毅飞",
  "email":"faith@tom.com",
  "name":{
    "firstName":"徐",
    "lastName":"毅飞"
  }
}
#局部更新
POST /faith/_update/1
{
  "doc":{
    "email":"evil@qq.com"
  }
}
GET /faith/_doc/1
DELETE /faith/_doc/1
```

### 4.1 RestClient操作索引库
**什么是RestClient**

ES官方提供了各种不同语言的客户端，用来操作ES。这些客户端的本质都是组装DSL语句，通过Http请求发给ES