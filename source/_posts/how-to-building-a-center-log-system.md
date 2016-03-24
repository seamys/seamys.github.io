---

title: Elasticsearch,kibana 搭建日志集中分析系统
date: 2016-03-20 00:51:14

categories: 
- 后端开发
tags:
- log4net
- Elasticsearch
- kibana
- linux
---



## 1.前言
日志收集分析非常重要，而且日志到达一定数量后分析无法进行。本篇文章讲解如何使用 Elasticsearch+kibana 在 Ubuntu系统下快速搭建一个日志分析系统。

**[Elasticsearch](http://baike.baidu.com/view/8005387.htm)**:
Elasticsearch是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。

**Kibana**
Kibana是一个使用 Apache 开源协议，基于浏览器的 Elasticsearch 分析和搜索仪表板。Kibana 非常容易安装和使用。整个项目都是用 HTML 和 Javascript 写的，所以 Kibana 不需要任何服务器端组件，一个纯文本发布服务器就够了。Kibana 和Elasticsearch 一样，力争成为极易上手，但同样灵活而强大的软件。


## 2.安装 Java 8,Elasticsearch 全文检索系统

你可以在此查看 [ElasticSearch.sh](https://gist.github.com/seamys/c08a6ccc7c34cb19cf05) 自动安装脚本。当前脚本会安装网站最新版本 **elasticsearch-2.2.1**

### 下载自动安装脚本
``` sh
cd ~ && wget https://gist.githubusercontent.com/seamys/c08a6ccc7c34cb19cf05/raw/f12379cf54438630decf776089f76fb62aff9282/ElasticSearch.sh
```
### 为脚本添加执行权限

```
chmod a+r ./ElasticSearch.sh
```

### 执行当前安装脚本
```
sudo ./ElasticSearch.sh

```
### 绑定IP
如果上述脚本安装成功。我们紧接着修改elasticsearch http访问地址 

## 5.WebAPI与log4net，log4net.Elasticsearch 扩展

## 6.完成
参考链接：
[Kibana User Guide 4.3](https://www.elastic.co/products/kibana)
[Elasticsearch Reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
[how-to-install-elasticsearch-logstash-and-kibana-elk-stack-on-ubuntu-14-04](https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-elk-stack-on-ubuntu-14-04)
[log4net.ElasticSearch](http://jptoto.github.io/log4net.ElasticSearch/)