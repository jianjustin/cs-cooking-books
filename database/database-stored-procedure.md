# 数据库文档 - 存储过程

## 概述

存储过程指的是一种在数据库中存储复杂程序，以便外部程序调用的一种数据库对象

## 基本操作

* 创建存储过程

```sql
USE [DATABASE_NAME];
GO
CREATE PROCEDURE [PROCEDURE_NAME]
  @[ITEM_NAME] [ITEM_TYPE]
  @[ITEM1_NAME] [ITEM1_TYPE]
AS
  [PROCEDURE_CONTEXT]
GO
```

* 执行存储过程

```sql
EXECUTE [PROCEDURE_NAME] @[ITEM_NAME]=[ITEM_VALUE],@[ITEM1_NAME]=[ITEM1_VALUE]
```

* 修改存储过程
  
  ```sql
  USE [DATABASE_NAME];
  GO
  ALTER PROCEDURE [PROCEDURE_NAME]
    @[ITEM_NAME] [ITEM_TYPE]
    @[ITEM1_NAME] [ITEM1_TYPE]
  AS
    [PROCEDURE_CONTEXT]
  GO
  ```