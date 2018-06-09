# Elasticsearch 是什么  
Elasticsearch是一个实时分布式搜索和分析引擎。它让你以前所未有的速度处理大数据成为可能。它用于全文搜索、结构化搜索、分析以及将这三者混合使用。基于Apache Lucene(TM)的开源搜索引擎。无论在开源还是专有领域，Lucene可以被认为是迄今为止最先进、性能最好的、功能最全的搜索引擎库。
Elasticsearch也使用Java开发并使用Lucene作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的RESTful API来隐藏Lucene的复杂性，从而让全文搜索变得简单。
不过，Elasticsearch不仅仅是Lucene和全文搜索，我们还能这样去描述它：
* 分布式的实时文件存储，每个字段都被索引并可被搜索  
* 分布式的实时分析搜索引擎
* 可以扩展到上百台服务器，处理PB级结构化或非结构化数据 

# 安装Elasticsearch  

你可以从 [elasticsearch.org\/download](https://www.elastic.co/downloads/elasticsearch) 下载最新版本的Elasticsearch。 下载zip包，解压到指定位置，点击、```\bin```目录下的```elasticsearch.bat```
运行，启动成功后，在浏览器地址栏中输入```http://localhost:9200/```出现json数据表示启动成功。注意：
* java安装不要安装在带特殊字符的文件夹，比如```D:\Program Files (x86)```带有括号的，会报```“此时不应有 \Java\jdk1.8.0_111”```类型的错误。

## 安装head插件

[见 ElasticSearch-5.0安装head插件](http://www.cnblogs.com/xuxy03/p/6039999.html)
