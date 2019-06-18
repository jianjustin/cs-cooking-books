# 数据库原理

## 概述

首先当我们要去理解数据库原理时，我们必须去了解数据库原理主要包括哪些知识内容，以及每个部分内容包含哪些内容

* 数据模型
* 数据库语言
* 数据库系统结构
* 事务管理

接下来需要说明一下数据库知识系列：

* [SQL基本操作](./database-sql.md)
* [视图管理](./database-view.md)
* [数据库函数]()
* [存储过程](./database-stored-procedure.md)
* [触发器](./database-trigger.md)
* [数据库安全](./database-security.md)
* [数据库事务机制]()
* 执行计划
* 数据库实战
  * Mysql
  * Oracle
  * Sql Server
  * Monogo DB

## 相关资料

## 提问

* 数据库中文无法正常显示
  
  修改数据库排序规则，使其支持中文排序
  
  ```sql
  ALTER DATABASE [DATABASE_NAME] COLLATE Chinese_PRC_CS_AI_WS;  
  ```

* 对比MYSQL，SQLSERVER，ORACLE三种数据库的区别