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

