### 概念理解
|	SQL术语/概念	|	MongoDB术语/概念	|	解释/说明	|
|--	|--	|--	|
|	database | database |	数据库 |
|	table	|	collection | 数据库表/集合 |
| row | document | 数据记录行/文档 |
| column | field | 数据字段/域 |
| index | index | 索引 |
| primary key | primary key | 主键,MongoDB自动将_id字段设置为主键 |

### MongoDB安装完，bin文件夹下的文件功能解释
- mongo.exe 负责运行数据库(开机)
- mongod.exe 负责开机(启动mongodb数据库)
- mongoexport.exe 负责输出数据库
- mongoimport.exe 负责导入数据库
- mongorestore.exe 负责备份数据库

### 配置环境变量
#### 系统属性 ==> 环境变量 ==> 系统变量 ==> path ==> 编辑 ==> 新建 ==> MongoDB的安装路径(精确到bin)
配置完成后，重启电脑。

查看配置是否成功：
```
 mongod -version
```
显示MongoDB数据库的相关信息，表示配置成功



## 常用数据库操作命令
### 启动数据库
语法：```mongod --dbpath mongodb_path```
- mongodb_path 数据库目录
``` 
mongod --dbpath c:\data\db(mongodb数据库目录)
```
启动成功后，一定不能关闭当前DOS窗口，一旦关闭窗口就表示关闭数据库。如还需DOS窗口操作数据库，可重新打开一个。

### 连接数据库
语法：```use database_name```
- database_name 数据库名称

```
use demo
```
use这个命令很特别，它具有创建、打开、切换的功能。如果use的数据库存在，它会直接打开当前数据库。如果不存在，它将会创建一个新的数据库。

### 查看所有数据库
``` 
show dbs
```

### 查看当前所在数据库
```
db
```

### 删除当前数据库
```
db.dropDatabase()
```

### 查看当前所在数据库中的所有集合
```
show collections
```

### 创建集合
语法：```db.createCollection(name , options)```
- name 要创建的集合名称
- options 可选，指定有关内存大小及索引的选项

```
db.createCollection("table")
```
在 MongoDB 中，你可以不主动创建集合。当你插入一些文档时，MongoDB 会自动创建集合。

### 删除集合
语法：```db.collection_name.drop()```
- collection_name 集合名称

```
db.table.drop()
```

### 查看当前数据库的相关信息(名称、文档个数、视图、索引等)
``` 
db.stats()
```

## 常用数据库数据操作命令
### 插入文档
语法：```db.collection_name.insert(document)```
- collection_name 集合名称
- document 文档内容

```
db.name.insert()
```
插入文档时，我们可以先定义一个变量：
```
document=({username:"admin",password:123456})
```
然后执行插入操作：
```
db.demo.insert(document)
```

### 更新文档
语法：
```
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
```
- query： update的查询条件，类似于sql update查询内where后面的。
- update： update的对象和一些更新的操作符如（$,$inc...）等，也可以理解为sql update查询内set后面的
- upsert： 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
- multi： 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
- writeConcern： 可选，抛出异常的级别

```
db.demo.update({'password':123},{$set:{'password':1234}})
```
修改多条相同的数据
```
db.demo.update({'password':123},{$set:{'password':1234}},{multi:true})
```

### 查看文档
语法：```db.collection_name.find(query, projection)```
- collection_name 集合名称
- query：可选，使用查询操作符指定查询条件。
- projection：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。

```
db.demo.find()
```

如果你需要以易读的方式来读取数据，可以使用 pretty() 方法：
```
  db.demo.find().pretty()
```
#### 条件操作符：
- $gt (>) 大于
- $lt (<) 小于
- $gte (>=) 大于等于
- $lte (<=) 小于等于

```
db.demo.find({age : {$gt : 100}}) //大于100
db.demo.find({age : {$lt : 100}}) //小于100
db.demo.find({age : {$gte : 100}}) //大于等于100
db.demo.find({age : {$lte : 100}}) //小于等于100
```

#### 类型操作符：```$type``` 
 
```
db.demo.find({"name" : {$type : 'string'}})
```

#### 指定记录条数：```db.collection_name.find().limit(number)``` 
 
- collection_name 集合名称
- number 条数

```
db.demo.find().limit(2)
```

#### 跳过指定条数：```db.collection_name.find().limit(number).skip(number)```

```
db.demo.find().limit(2).skip(1)
```

#### 排序：```db.collection_name.find().sort({key:1})```
- key 需排序的字段
- 1 升序
- -1 降序

```
db.col.find().sort({"age":-1})
```

### 文档替换
语法：
```
db.collection_name.save(
   <document>,
   {
     writeConcern: <document>
   }
)
```
-  document： 文档数据
-  writeConcern： 可选，抛出异常的级别
```
db.demo.save({ "_id" : ObjectId("5e6dfb4f84e4e9890ae86ace"), "name" : "王二五" })
```

### 删除文档
语法：
```
db.collection_name.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
```
- query：（可选）删除的文档的条件。
- justOne：（可选）如果设为 true 或 1，则只删除一个文档，如果不设置该参数，或使用默认值 false，则删除所有匹配条件的文档。
- writeConcern：可选）抛出异常的级别。

删除多条数据
```
db.demo.remove({"password" : 1234})
```

删除一条数据
```
db.demo.remove({"password" : 1234}, 1)
```

删除所有数据
```
db.demo.remove({})
```