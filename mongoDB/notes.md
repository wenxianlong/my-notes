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
# 插入 
语法: ```固定前缀 db``` + ```集合名字``` + ```函数 insert()```  
```
db.person.insert({
  name: "lining", 
  age: 10, 
  address: "beijing",
  tars:["java", "spring"],
  sex: 0
})
```
# 查询
语法: ```固定前缀 db``` + ```集合名字``` + ```函数 find()``` 

```
//[1] 查询多有
db.person.find()  //查询出集合person下多有的documents
db.person.find().pretty()  //pretty()格式化查询得到的json
//[2] 条件查询
db.person.find({name : 'lining'})  // nmae 等于 lining
db.person.find({name : 'lining', age: 10}) // and 同时满足多个条件
db.person.find($or: [{name:"lining"},{age:10}]) //or
db.person.find({{age: {$lt:20}}, $or:[{name:"wenxl"}, {sex:0}]})// and 和 or 同时使用
// [3] 范围搜索
// $gt: 大于，$gte: 大于等于
// $lt: 小于，$lte: 小于等于
// $ne: 不等于
// $in: 属于
db.person.find({age: {$gt: 134360, $lt: 500000}})
db.person.find({age: {$gt: 134360, $lt: 500000}, name: 'Seven'})
db.person.find({age: {$in: [100, 200, 400]}})
// [4] 如果有多个结果，则会按磁盘存储顺序返回第一个
db.person.findOne({'name':'wenxl'})
// [5] 分页: 使用 skip() + limit()，但要注意 skip 方法是一条条数据数过去的，百万级时效率很低
db.person.find().skip(2).limit(5)
// [6] 只列出需要的属性: 第一个参数是条件，第二个参数是需要显示的属性，1 为输出，0 为不输出
db.person.find({}, {name: 1, age: 1})
db.person.find({name: 'wenxl'}, {name: 1, age: 1})
// [7] 统计个数
db.person.find().count()
// [8] 排序: 1 为升序，-1 为降序
db.person.find().sort({age: 1, name: -1}).pretty()
// [9] 查询子文档: http://www.runoob.com/mongodb/mongodb-advanced-indexing.html
db.person.find({'address.city': 'Los Angeles'})
```
# 更新
语法: ```固定前缀 db``` + ```集合名字``` + ```函数 update()```

```
// [1] $set: 修改为最新的值，如果属性不存在则会自动增加，不会影响其他属性
db.person.update({name: 'lining'}, {$set: {age: 20}})
// [2] $inc: 在原来的值上增加
db.person.update({name: 'lining'}, {$inc: {age: 9}})
// [3] $push: 往数组中增加一个新的元素
db.person.update({name: 'lining'}, {$push: {school: '河南轻工业学院'}})
// [3] 更新多个: 默认只会更新第一个，如果要多个同时更新，要设置 {multi:true}
db.person.update({name: 'lining'}, {$inc: {age: 5}}, {multi: true})

```

# 保存
语法: ```固定前缀 db``` + ```集合名字``` + ```函数 save()```  

```
db.person.save({_id: ObjectId('5a9a0c871e7e7233a7453f16'), age: 34})

```
> save 时，如果已经存在则 替换 整个 document，不存在则 插入 (使用 _id 判断)，不能像 update 一样使用条件参数:

# 删除
语法: ```固定前缀 db``` + ```集合名字``` + ```函数 remove() | deleteOne() | deleteMany()```  

```
//[1] 推荐使用delete
db.person.deleteOne({name: "wenxl"})
db.person.deleteMany({name: "wenxl"})

//[2]删除所有包含lining的documents
db.person.remove({name: "lining"})

//[3]删除第一个包含lining的document
db.person.remove({name: "lining"}, 1)

//[4]删除集合下所有的document
db.person.remove({})

```

# 聚合

语法: ```固定前缀 db``` + ```集合名字``` + ```函数 aggregate()```  
* 数据  
```
[
    {
        title: 'MongoDB Overview',
        description: 'MongoDB is no sql database',
        by_user: 'runoob.com',
        url: 'http://www.runoob.com',
        tags: ['mongodb', 'database', 'NoSQL'],
        likes: 100
    },
    {
        title: 'NoSQL Overview',
        description: 'No sql database is very fast',
        by_user: 'runoob.com',
        url: 'http://www.runoob.com',
        tags: ['mongodb', 'database', 'NoSQL'],
        likes: 10
    },
    {
        title: 'Neo4j Overview',
        description: 'Neo4j is no sql database',
        by_user: 'Neo4j',
        url: 'http://www.neo4j.com',
        tags: ['neo4j', 'database', 'NoSQL'],
        likes: 750
    }
]

```
* 聚合  
```
//// 类似sql语句： select by_user, count(*) from mycol group by by_user
db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : 1}}}]) 
//输出
{
   "result" : [
      {
         "_id" : "runoob.com",
         "num_tutorial" : 2
      },
      {
         "_id" : "Neo4j",
         "num_tutorial" : 1
      }
   ],
   "ok" : 1
}

// 管道: $match 先筛选符合条件的文档，然后进行聚合，这就明白 aggregate 的参数为啥是 [] 了吧
// 管道常用操作有: $project, $match, $skip, $limit, $unwind, $sort, $geoNear
db.mycol.aggregate([{$match: {likes: {$gt: 50}}}, {$group: {_id: '$by_user', num: {$sum: 1}}}])
db.mycol.aggregate([{$match: {likes: {$gt: 50}}}, {$group: {_id: '$by_user', num: {$max: '$likes'}}}])

```

# 索引
索引是特殊的数据结构，索引存储在一个易于遍历读取的数据集合中，索引是对数据库表中一列或多列的值进行排序的一种结构。索引通常能够极大的提高查询的效率，如果没有索引，MongoDB 在读取数据时必须扫描集合中的每个文件并选取那些符合查询条件的记录。

语法: 固定前缀 db + 集合名字 + 函数 ensureIndex()  

```
// 1 为升序，-1 为降序，text 为全文索引
db.col.ensureIndex({title: 1})
db.col.ensureIndex({title: 1, description: -1})
db.col.ensureIndex({description: 'text'})  // 建立全文索引 
db.col.find({$text: {$search: 'Fincher'}}) // 搜索有关键词 Fincher 的文档

```
查看查询语句的效率使用 explain 函数  
```
db.users.find({'address.city': 'Los Angeles'}).explain()

```

