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
