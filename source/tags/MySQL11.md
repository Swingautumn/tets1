---
title: MYSQL笔记
copyright_author: 王耀福
abbrlink: 56138
---
# MySQL
**数据库的三范式**：

列的原子性不可分割成更多列、

非主键列要完全依赖于主键列、

非主键列之间不存在传递依赖。即非主键列不能依赖于其他非主键列

**范式的发展代表着数据库在结构逻辑上合理化的一个过程，范式的存在有助于减少数据冗余、避免更新异常、简化查询，并提高数据的完整性**

关于约束：主键、非空、外键、唯一、默认		——**约束**

**关于存储过程的（类比函数）**：

`	`本质是一组为了完成特定功能的SQL语句集

《DELIMITER //》《procedure》、直接通过存储过程的名字进行使用

常用于存储数据处理和转换(增删改)、函数之类的SQL语句
**
`	`MySQL 中的触发器（Trigger）是一种特殊的存储过程，在创建包含多个语句的触发器时，一定要用到DELIMITER关键词

`								`——**存储函数**

**关于double float精度问题**——double64位，float32位，可能存在精度丢失取而代之的是**decimal**，它的底层是字符串类型，不存在丢失问题 

` `——**精度丢失问题**

**关于MySQL的ACID**：

原子性、一致性、隔离性、持久性。MySQL事务操作的四种特性即ACID

MySQL支持事务同时有四种隔离级别：读未提交、读已提交、可重复读、可串行化

关键词或语句：《transaction》、《start、end》、《commit、rollback》《savepoint、release savepoint》、《isolation level》

`								`——**ACID与事务**

**MySQL5与MySQL8**

`	`MySQL8功能更强支持JSON格式、引入了窗口函数、优化了存储引擎架构，同时性能显著提升引入了索引算法优化了并发处理也增强了安全性支持SSL/TLS和角色权限控制。没有六七版本的影响因素有以下几种：项目调整和战略变化、技术难度和挑战、重构优化等，所以开发团队决定跳过6、7版本

**关于MySQL范围的关键词**

关键词：between…and…/in/not exist/except/

关于**物理外键与逻辑外键**——前者是在建表时就添加，后者则是通过alter语句建表后添加，企业多用后者

关于MySQL的**行转列或列转行**

`	`通常使用聚合函数、条件语句或PIVOT，多数情况下利用sum函数加上case或if函数配合实现

`								`**——行转列或列转行**



关于**内外左右**连接：					

内连接是把匹配的关联数据显示出来；左连接是左边的表全部显示出来，右边的表显示出符合条件的数据；右连接正好相反。

MySQL默认的join on关键字连接是内连接，要允许null出现改成左连接或右连接即可						——**关联查询**

关于MySQL**备份**：

`	`常用备份工具包括Linux上的mysqldump、windows上的navicat

`	`备份类型分为物理备份和逻辑备份，也可以分为完全备份、增量备份、差异备份。物理备份分为冷备份(脱机备份)、热备份(联机备份)、温备份

——**备份**

关于聚合**函数**count与sum：

`	`Count统计行数，Sum统计列数的具体值，根据需求进行使用，通常都可能会与group by一起使用，其他函数还有：max、min、concat(拼接)、avg、round、now、upper与lower(大小写)等等

`								`——**函数**

**关于视图**：

`	`创建和使用与建表相近(create view xxxx as select ….)，视图分为可更新视图和不可更新视图。视图的能否更新通常受到多种因素影响如：基础表是否允许更新、视图的定义语句是否使用分组或排序的关键词、聚合函数、约束以及用户权限等

`								`——**视图**

**关于索引**：

**BTree**、unique、primary、其中BTree索引最常用，对应语句关键词index	

主要还是Btree、哈希、空间等，设置索引有利于快速查询到指定内容，否则数据库查询是执行的全表扫描，这样通常效率很低。

相关创建：

CREATE index idx\_name ON table\_name(column1, column2, ...);建表时设置

ALTER TABLE table\_name ADD INDEX idx\_name(column1, column2, ...);建表后添加

——**索引**

having 在有group by的情况下放最后面

**innodb存储引擎与myisam存储引擎**

innodb适用于多insert与update操作，同时支持4个事务隔离级别 | myisam适用于多select操作，擅长存储和检阅 | 关于锁有3级——行(innodb)页表(mysiam) | 关于(事务)并发：MyISAM读写互相阻塞、InnoDB 读写阻塞与事务隔离级别相关 | innodb——数据索引均缓存(聚簇索引，索引组织表)，myisam——仅缓存索引(非聚簇索引，堆组织表)

MVCC——多版本并发控制
