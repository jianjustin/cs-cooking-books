# 数据库文档 - 触发器

## 概述

触发器是在数据库中，在执行对数据有异动的动作时，先行拦截并处理的一种数据库对象

## 基本操作

* 创建触发器

```sql
CREATE TRIGGER [TRIGGER_NAME]
ON [TABLE_NAME/DATABASE_NAME]
FOR [TRIGGER_EVENT_TYPE]
AS
[TRIGGER_CONTEXT]
```

## 触发器配置说明

1. 触发器类型
   * DDL触发器
   * EML触发器

2. 触发器事件类型