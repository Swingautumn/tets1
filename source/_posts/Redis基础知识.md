---
title: Redis笔记
copyright_author: 王耀福
abbrlink: 20305
---

# Redis
**Redis是一种键值类型的非关系型数据库**，具有强大的读写能力，针对一些高并发场景具有良好的表现，同时可扩展性强能满足很多个性化需求。它可使用的**基本数据类型**包括字符串(string)，列表(list)，散列(hash)，集合(set)，有序集合(zset)。它采用的是**单线程开发**，瓶颈是机器内存和网络带宽，其他单线程软件包括node.js、nginx。

Redis**支持事务但是不存在隔离级别**的概念，同时关于可能问题的概念如幻读，脏读概念仍有。

**查看当前reids数据库信息命令**：info replication

Redis的**订阅功能**：首先区分端口是订阅端还是发送端，订阅端使用SUBSCRIBE创建并进入查看，发送端使用PUBLISH向订阅端发送信息

Redis **默认有 16 个**数据库，编号从 0 到 15, 本身**并没有一个直接的命令来列出所有存在的数据库及其内容**，同时Redis本身也**无法识别SQL类型文件**，它能识别的文件是**RDB和AOF文件，**并且它没有命令可以直接使用识别导入 (MySQL的source命令)，通常情况下都是**由对应的配置文件在加载时解析读取(**dump.rdb与appendonly.aof**)，**存放路径在bin目录下

**存入Redis的键值对数据在默认情况下是无序排列的**

**基本数据命令的使用**

**字符串**：

select、FLUSHDB(xxx)、**keys \***、set、get、exists、expire过期时间、type、strlen长度、**incr/decr** (views)自增阅读、get/set(**range**)截取、setex(set with expire)、setnx(set if not exist)、mset/get、getset

注意FLUSHDB 和 **FLUSHALL**、set与setnx与getset

**列表**（链表）：

LPUSH/**LRANGE**/RPUSH 、LPOP(从上开始移除)/RPOP(从下开始移除)、**lrem**(根据value移除)、ltrim、rpoplpush、linsert、

注意：在Redis中，列表数据结构经常被用作队列或栈的实现

**set**集合：

sadd、**smembers**、sismember、scard、**srem**、srandmember、spop、**sdiff**、sinter、sunion

关于sdiff差集：1集合有但2集合没有的部分

注意set与sadd

**hash**（哈希）：

key-map集合 hset、hget（myhash）、hmget、hgetall、hdel、hexists、hincrby，存储变更的数据，更适合存储对象

**Zset**（有序集合）：

zadd、zrange、zrangebyscore、zcard、zrevrange、zcount**、**

**其他特殊数据类型：**

Geospatial(** 地理位置)/Hyperloglog(基数)/Bitmap(位存储)

注意：Redis的基数类型不能直接查看它的key的内容，只能用pfcount查看统计值

**集群模式：**
**
`	`主从复制模式、哨兵模式

可能问题：

`	`缓存雪崩(缓存集中过期或服务器宕机)、

缓存击穿(请求量多且缓存过期)、

缓存穿透(频繁发送不存在请求)

设置分布式锁、空缓存、布隆过滤器

**关于持久化**：AOF与RDB

`	`**RDB：**备份速度块、内存占用少，恢复快(参考快照，默认方式)，《redis会生成一个快照文件，备份还原时直接加载该文件所以速度来》

`	`**AOF：**数据完整性高、安全性强、具有实时性(完全备份)，《AOF持久化机制下，reids通过重新执行文件中的写命令来恢复数据》

关于**主从复制**

通常与哨兵模式一起使用。主从复制即指主Redis服务器将数据复制到从Redis服务器，数据复制为单向，只能允许主到从。相关配置需要操作reids.conf文件以及其中的port、pidfile、logfile、dbfilename部分(单机集群模式)，

联机集群的话，添加slaveof和slave-read-only部分。

命令部分：redis-cil加个-p指定端口号、redis-server加个配置文件名指定端口

在安装配置软件时注意修改bind、protected-mod、daemonzie部分，否则联机必然报错

`	`**优缺点**(主从)：主机会自动将数据同步到从机，读写分离、负载均衡降低主服务器压力；主从服务器的宕机都会导致前端部分读写请求失败、主服务器宕机从服务器引入数据可能不一致

**Redis过期策略**--指当Redis中缓存的key过期了，Redis如何处理：定时过期、惰性过期、定期过期，定时利好内存，惰性利好CPU，定期折中

**Redis淘汰策略**：6种：使用最少的、将要过期的、随意选择，设置了过期时间的数据集；使用最少的、随意选择，数据集；不驱逐


