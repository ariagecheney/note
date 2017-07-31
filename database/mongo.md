## 安装
* mongod.exe --logpath "D:\install\mongo3.4\data\log\mongodb.log" --logappend --dbpath "D:\install\mongo3.4\data\db" --serviceName "MongoDB" --install

## some
* MongoDB 中默认的数据库为 test，如果你没有创建新的数据库，集合将存放在 test 数据库中。

## shell command
* show dbs
* use dbname 切换或创建 db
* db
* show tables
* db.createCollection("testcollection");
* db.dropDatabase()
* db.colName.drop()
* db.version() 查看mongodb版本

```shell
// 后台运行时，关闭mongod
use admin
db.shutdownServer()
// 副本集
//1
./bin/mongod --replSet replset --fork --port 27017 --dbpath /data/mongodb/data27017 --oplogSize 2048 --logpath /data/mongodb/log27017/mongod.log --logappend  

./bin/mongod --replSet replset --fork --port 27018 --dbpath /data/mongodb/data27018 --oplogSize 2048 --logpath /data/mongodb/log27018/mongod.log --logappend 

./bin/mongod --replSet replset --fork --port 27019 --dbpath /data/mongodb/data27019 --oplogSize 2048 --logpath /data/mongodb/log27019/mongod.log --logappend
//2
./bin/mongo
use admin
config = { _id:"replset", members:[
    {_id:0,host:"192.168.200.67:27017"},
    {_id:1,host:"192.168.200.67:27018"},
    {_id:2,host:"192.168.200.67:27019"}]
 };
 rs.initiate(config);

 //验证副本集
 use yes_db
 db.yes_users.insert({"uid":"20160611_0001"},{"uname":"tim.man"});

 // 设置副本集节点可读
 use yes_db
 db.getMongo().setSlaveOk();
 show tables;
 db.yes_users.find();  

 //  验证primary、secondary自动切换操作
 mongo localhost:27017
 use admin
 db.shutdownServer();
 mongo localhost:27017 此时会报错

 // 登录另一台
 mongo localhost:27018
 rs.status();

 //再启动原来旧的primary库27017端口的mongodb（如果是实际上的坏了就是修复后启动），27017就会自动变成secondary状态。
```
### insert

* db.COLLECTION_NAME.insert(document)  
如果该集合不在该数据库中， MongoDB 会自动创建该集合并插入文档。
* 插入文档你也可以使用 db.col.save(document) 命令。如果不指定 _id 字段 save() 方法类似于 insert() 方法。如果指定 _id 字段，则会更新该 _id 的数据。
### update

```js
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
db.collection.save(
   <document>,
   {
     writeConcern: <document>
   }
)
db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}},false,true)
```

### remove
```js
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
db.col.remove({'title':'MongoDB 教程'})
```
* 如果你只想删除第一条找到的记录可以设置 justOne 为 1，如下所示：
>db.COLLECTION_NAME.remove(DELETION_CRITERIA,1)

* 如果你想删除所有数据，可以使用以下方式（类似常规 SQL 的 truncate 命令）：
>db.col.remove({})

### find
* db.collection.find(query, projection)
* db.col.find().pretty()
* 除了 find() 方法之外，还有一个 findOne() 方法，它只返回一个文档。