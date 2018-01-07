---
title: mongodb分片配置
tags:
  - mongodb
id: 583
categories: 数据库
date: 2012-12-23 14:56:22
---

mongodb分片配置包括以下几个角色：

1configsvr，顾名思义就是存储配置的，这里的配置包括集群中的shareds，databases，collections等，

2shard，存放实际数据的进程

3mongos，路由进程

configsvr其实就是对应的mongod --configsvr，shard其实就是mongod --shardsvr

这里我搭建一个windows最简单的分片集群，所有进程都部署在同一台机器上。

依次执行如下命令，所以的dbpath可以随意指定，但是需要提前创建好:

binmongod --configsvr --dbpath data-config --journal --rest

binmongos --configdb 127.0.0.1:27019

正常启动后配置服务器会创建一个叫做"config"的数据库，包括如下集合：

![](/images/2012/12/mongo-config-db.png)

&nbsp;

其中databases表示当前集群中的数据库信息，mongos即路由进程，shards目前还没有。

接着启动shard，依次执行如下命令，启动两个shards。

binmongod --shardsvr --dbpath data-shard1 --port 10086 --journal --rest

binmongod --shardsvr --dbpath data-shard2 --port 10096 --journal --rest

不出意外所以进程都会顺利启动，这时一个简答的分片集群就完成了一半了，剩下的就是将shards加入集群，这需要通过mongo shell来完成。

启动binmongo 127.0.0.1:27017/admin，这里ip:port是mongs的，admin是默认的管理数据库，执行如下命令：

sh.addShard("127.0.0.1:10086")

sh.addShard("127.0.0.1:10096")

此时config数据库中的shards集合数变为2，文档内容如下：

![](/images/2012/12/mongo-config-shards.png)

&nbsp;

至此一个简单的分片集群即搭建完毕，接下来往里面搞点数据。

*   指定可以分片的数据库
*   指定可分片的集合
*   插入数据
sh.enableSharding("billowqiu")，表示"billowqiu"这个数据库需要分片；

sh.shardCollection("billowqiu.education", {"date":1})，表示eduction这个集合按照{"date":1}的shard-key方式进行分片，这里的shard-key是必须要的，关于shard-key-pattern的详细信息可以去mongo官网查看。

插入数据的方式和普通模式类似，唯一需要注意的是插入时必须带上shard-key，例如

use billowqiu

col=db["education"]

col.insert({"date":1997} ,{"name":"taer mid school"})

col.insert({"date":2000} ,{"name":"huangpi three high school"})

总的来说，mongodb配置和使用都是比较容易上手的，其分片模型和许多Master-Slave模式类似，只不过多了一个configsvr的角色，像gfs，bigtable中这些分布式系统中master的角色其实包括了config和mongos。

&nbsp;