# MongoDB

终于这次轮到MongoDB了，下载安装教程就不说了，不是很难，只要搜英文的都有非常详细且实用的步骤，贴出[官网](https://www.mongodb.com/download-center/community)吧。

## Introduction

#### 官网的介绍

```bash
MongoDB is a cross-platform, document oriented database that provides, high performance, high 
availability, and easy scalability. MongoDB works on concept of collection and document.
```

#### Database

```bash
Database is a physical container for collections. Each database gets its own set of files on 
the file system. A single MongoDB server typically has multiple databases.
```

#### Collection

```bash
Collection is a group of MongoDB documents. It is the equivalent of an RDBMS table. A 
collection exists within a single database. Collections do not enforce a schema. Documents 
within a collection can have different fields. Typically, all documents in a collection are 
of similar or related purpose.
```

#### Document

```bash
A document is a set of key-value pairs. Documents have dynamic schema. Dynamic schema means 
that documents in the same collection do not need to have the same set of fields or structure,
and common fields in a collection's documents may hold different types of data.
```

简单来说，```MongoDB```是一个基于分布式文件存储的数据库，它是一个介于关系数据库和非关系数据库之间的产品，其主要目标是在键/值存储方式（提供了高性能和高度伸缩性）和传统的```RDBMS```系统（具有丰富的功能）之间架起一座桥梁，它集两者的优势于一身。

```MongoDB```支持的数据结构非常松散，是类似```json```的```bson```格式，因此可以存储比较复杂的数据类型，也因为他的存储格式也使得它所存储的数据在```Nodejs```程序应用中使用非常流畅。

传统的关系数据库一般由数据库（```database```）、表（```table```）、记录（```record```）三个层次概念组成，MongoDB是由数据库（```database```）、集合（```collection```）、文档对象（```document```）三个层次组成。```MongoDB```对于关系型数据库里的表，但是集合中没有列、行和关系概念，这体现了模式自由的特点。

MongoDB中的一条记录就是一个文档，是一个数据结构，由字段和值对组成。MongoDB文档与``JSON```对象类似。字段的值有可能包括其它文档、数组以及文档数组。MongoDB支持OS X、Linux及Windows等操作系统，并提供了Python，PHP，Ruby，Java及C++语言的驱动程序，社区中也提供了对Erlang及.NET等平台的驱动程序。

MongoDB的适合对大量或者无固定格式的数据进行存储，比如：日志、缓存等。对事物支持较弱，不适用复杂的多文档（多表）的级联查询。

MongoDB主要有3个用法，```db```，```MongoTemplate```，```MongoRepository```.  

据我现在的了解，```db```主要在```mongo```的命令行使用，```MongoTemplate```和```MongoRepository```在后端，或者更直接，在```spring boot```中使用，是两种不同的方式。

### db

记一些简单常用的指令吧，详细的可以到[Tutorial](https://www.tutorialspoint.com/mongodb/index.htm)中去找。

1.创建database

```mongodb
use database_name
```

2.删除database

```mongodb
use database_name
db.dropDatebase()
```

3.创建collection

```mongodb
use database_name
db.createCollection("collection_name")
```

4.删除collection

```mongo
use database_name
db.collection_name.drop()
```

5.查看databases

```mongo
show databases
```

6.查看collections

```mongo
use database_name
show collections
```

7.向collection中插入docucment

```mongo
db.collection_name.insert(document)
```

8.查询document

```mongo
db.collection_name.find()
```

```pretty()```方法会让输出格式更好

```mongo
db.collection_name.find().pretty()
```

9.删除document
删除包含```deleletion_critteria```的条目

```mongo
db.collection_name.remove(deleletion_critteria)
```

10.创建User

```roles```比较多，具体创建可以细细研究下。

```mongo
db.createUser(
 {
  user:"userName",
  pwd:"password",
  roles:["dbAdminAnyDatabase","dbAdmin"]
 }
)
```

### MongoTemplate

```MongoTemplate```与```MongoRepository```有些不同，但基本类似，都不用特别考虑与数据库的关系，可以在```spring boot```中建立```dao```层或```service```层，直接调用```MongoTemplate```类实现更高层的方法，相当于继承了```Repository```的感觉，总之都不难用。

### MongoRepository

```MongoRepository```比较好理解，和```CrudRepository```比较类似，我们需要完成新类去继承（```extends```）```MongoRepository```，然后写出抽象方法即可，具体实现不用考虑，然后高层即可直接使用。

## MongoDB Demo

[地址](https://github.com/ruishaopu561/SE_WEB/mongodb)

这个demo很清晰的展示出了```MongoTemplate```和```MongoRepository```的用法

链接数据库的配置，用```uri```的方法比较好

单机模式：```mongodb://name:pwd@ip:port/database```  
集群模式：```mongodb://name:pwd@ip1:port1,ip2:port2/database```

demo:

```yml
spring.data.mongodb.uri=mongodb://testUser:123456@localhost:27017/test
```

参数解释：

```yml
testUser 是数据库中的用户名  
123456 是该用户的密码  
localhost 是host  
27017 是默认端口  
test 是数据库名称  
```

## Reference

几个比较好的教程网站：  
[Tutorial](https://www.tutorialspoint.com/mongodb/index.htm)  
[Demo](https://my.oschina.net/xiedeshou/blog/2394755)