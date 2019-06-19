# 数据库文档 - SQL

## 结构化查询语言

SQL语言主要包括：

* 数据定义语言
* 交互式数据操纵语言
* 视图定义
* 事务控制
* 嵌入式SQL和动态SQL
* 完整性
* 权限管理

## SQL基础语法

1. `SELECT`语句

   ​     `SELECT column_name,column_name FROM table_name`

2. `INSERT`语句

   `INSERT INTO table_name (column1,column2,column3,...)VALUES(value1,value2,value3,...);`

3. `UPDATE`语句

   `UPDATE table_name SET column1 = value1,column2 = value2,... WHERE some_column = some_value`

4. `DELETE`语句

   `DELETE FROM table_name WHERE some_column = some_value`

5. 数据库操作语句

   * 创建数据库 - `CREATE DATABASE dbname`

   * 创建表结构

     ```sql
     CREATE TABLE table_name(
       column_name1 data_type,
       column_name2 data_type,
       column_name3 data_type,
       ...
     )
     ```

   * 删除数据库

     ```sql
     DROP DATABASE db_name
     ```

   * 删除表结构

     ```sql
     DROP TABLE table_name
     ```

   6. 修改表结构

      - 表结构添加列

        ```sql
        ALTER TABLE table_name ADD column_name data_type
        ```

      - 表结构修改列属性

        ```sql
        ALTER TABLE table_name MODIFY COLUMN column_name data_type
        ```

      - 表结构删除列

        ```sql
        ALTER TABLE table_name DROP COLUMN column_name
        ```

        

## SQL高级语法

* 创建索引

  ```sql
  CREATE INDEX index_name ON table_name(column_name) 
  ```

* 删除表数据

  ```sql
  TRUNCATE TABLE table_name
  ```

* 删除表索引

  ```sql
  ALter TABLE table_name DROP INDEX index_name
  ```

* 连接查询

  * 等值连接（内连接）

    ```sql
    SELECT * from Table_A A JOIN Table_B B ON A.id = B.id;
    ```

  * 不等连接（内连接）

  * ```sql
    SELECT * from Table_A A JOIN Table_B B ON A.id < B.id; 
    ```

  * 自然连接（内连接）

  * ```sql
    SELECT * from Table_A NATURAL JOIN Table_B ; 
    ```

  * 交叉连接（内连接）

  * ```sql
    SELECT * from Table_A CROSS JOIN Table_B;
    ```

  * 左外连接

    ```sql
    SELECT * from Table_A A LEFT JOIN Table_B B ON A.id = B.id;
    ```

  * 右外连接

    ```sql
    SELECT * from Table_A A RIGHT JOIN Table_B B ON A.id=B.id;
    ```

  * 全连接

    ```sql
    SELECT * from Table_A A FULL JOIN Table_B B ON A.id=B.id;
    ```

* 视图

  - 创建/更新视图

    ```sql
    CREATE OR REPLACE VIEW view_name AS
    SELECT column_name 
    FROM table_name
    WHERE condition
    ```

  - 视图删除

    ```sql
    DROP VIEW view_name
    ```

    

## SQL语法技巧

> 本模块内容主要依照MYSQL语法，并不一定适配sql server和oracle

* `DISTINCT`关键字 - 用于结果集去重

* `ORDER BY`关键字 - 对结果集进行排序

* `LIMIT`关键字 - 规定要返回记录的数目

* `LIKE`关键字 - 用于指定搜索列中的指定模式

* `IN`关键字 - 用于匹配多个值

* `BETWEEN`关键字 - 用于WHERE语句中区间过滤

* `AS`关键字 - 别名

* `UNION`关键字 - 结果集合并

  ```sql
  SELECT column_name FROM table1
  UNION 
  SELECT column_name FROM table2;
  ```

* 数据库表定义约束

  - NOT NULL - 列值不能为空
  - UNIQUE - 列值唯一
  - PRIMARY KEY - 主键标识
  - POREIGN KEY - 外键标识
  - CHECK - 保证列中的值符合指定条件
  - DEFAULT - 列值默认值

* `Auto-increment`关键字 - 用于列自增



##  SQL语法函数库

