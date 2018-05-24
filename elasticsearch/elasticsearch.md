# Elasticsearch 是一个实时的分布式搜索分析引擎， 它能让你以一个之前从未有过的速度和规模，去探索你的数据。 它被用作全文检索、结构化搜索、分析以及这三个功能的组合

## 下载与运行
### 下载
安装 Elasticsearch 之前，你需要先安装一个较新的版本的 Java，最好的选择是，你可以从[www.java.com](www.java.com)获得官方提供的最新版本的 Java。
之后，你可以从 elastic 的官网 [elastic.co/downloads/elasticsearch](elastic.co/downloads/elasticsearch)获取最新版本的 Elasticsearch。
### 运行  
如果你是在 Windows 上面运行 Elasticseach，你应该运行 ```bin\elasticsearch.bat``` 而不是 ```bin\elasticsearch```  
启动成功后，访问[http://localhost:9200](http://localhost:9200),如果出现以下代码说明启动成功  
```
{
  "name" : "-mCUePm",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "yRUkqtbUTaCcetsaY62wJg",
  "version" : {
    "number" : "6.2.4",
    "build_hash" : "ccec39f",
    "build_date" : "2018-04-12T20:37:28.497551Z",
    "build_snapshot" : false,
    "lucene_version" : "7.2.1",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}

```

