MongoDB 的结构是：数据库 > 集合 (collection) > 文档 (document) >     属性 (field)

MySQL   的结构是: 数据库 > 表 (table) >       记录 (record or row) > 属性 (field or column)

# 下载与安装
不同的系统安装 MongoDB 差别挺大的:
*  Mac: 使用 brew install mongodb
*  Linux: 参考 <http://qtdebug.com/mac-centos7/#安装-MongoDB>
*  Windows: [下载](http://www.mongodb.org/downloads)然后安装，安装的时候不要选择安装 MongoDB Compass，因为需要联网下载，国内有可能很久都装不好
# 启动访问  
* 启动 MongoDB:  
  * ```mongod```  
  * ```mongod --config C:/etc/mongod.conf```  
* 访问 MongoDB:  
  * ```mongo```  
  * ```mongo --host IP```  
  * 使用 IDEA 的插件 ```Mongo Plugin```  
  * 智能的免费客户端 NoSQLBooster for MongoDB (推荐使用)  
# 配置文件
```
systemLog:
    destination: file
    path: D:\MongoDB\logs\mongodb.log #日志输出文件路径
    logAppend: true
storage:
    dbPath: D:\MongoDB\data #数据库路径
net:
    bindIp: 0.0.0.0 #允许其他电脑访问
```  
默认只允许本机访问，在 mongod.conf 中配置 bindIp 允许其他机器访问:

* bindIp = 127.0.0.1, 192.168.1.10: 使用这 2 个 IP 访问  
* bindIp = 0.0.0.0: 任意机器  
注意：Winows 中 MongoDB 的数据库目录需要我们手动创建，如果目录不存在则会启动失败

* 默认的目录为 C:/data/db
* 上面配置文件的为 D:/MongoDB/data 和 D:/MongoDB/logs

#创建和删除  
* 创建数据库 ```use DATABASE_NAME```  
如果数据库存在则使用它，不存在则创建  
* 删除数据库 ```db.dropDatabase()```  
删除的是当前数据库  
* 创建集合 ```db.createCollection(name, options)```  
集合相当于 MySQL 中的 Table，MongoDB 插入时如果集合不存在则会自动创建集合，MySQL 不一样，需要事先创建好表才能进行插入  
  * name: 要创建的集合名称  
  * options: 可选参数, 指定有关内存大小及索引的选项  
    * capped: （可选）如果为 true，则创建固定集合。固定集合是指有着固定大小的集合，当达到最大值时，它会自动覆盖最早的文档。当该值为 true 时，必须指定 size 参数。  
    * autoIndexId: （可选）如为 true，自动在 _id 字段创建索引。默认为 false。    
    * size:（可选）为固定集合指定一个最大值（以字节计）。如果 capped 为 true，也需要指定该字段。  
    * max: （可选）指定固定集合中包含文档的最大数量。
* 删除集合  ```db.collection.drop()```

 


