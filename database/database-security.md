# 数据库安全

## 用户管理

* 创建用户

  ```sql
  create user zhangsan identified by 'zhangsan';
  ```

* 修改用户密码

  ```sql
  update mysql.user set password = password('zhangsannew') where user = 'zhangsan' and host = '%';
  flush privileges;<!--刷新用户权限表-->
  ```

* 赋予用户权限

  ```sql
  grant select,update,delete,insert on database_name.table_name to zhangsan;
  ```

* 回收用户权限

  ```sql
  revoke select on database_name.table_name from zhangsan;
  ```

* 删除用户

  ```sql
  drop user zhangsan@'%';
  ```

* 查询数据库内所有用户

  ```sql
  select host,user,password from user;
  ```



## 角色管理

* 创建角色

  ```sql
  CREATE ROLE R1;
  ```

* 角色授权用户

  ```sql
  GRANT R1 TO zhangsan;
  ```

* 回收用户角色

  ```sql
  REVOKE R1 FROM zhangsan
  ```



## 事务机制

事务时用户定义的一个数据操作序列，是数据库运行的最小的，不可分割的工作单位；

主要特性包括：原子性，一致性，隔离性和持续性



## 并发控制机制

1. 数据不一致情况

   丢失修改/不可重复读/读脏数据/产生幽灵数据

2. 锁机制

   排他锁/共享锁

3. 封锁协议

   * `一级封锁协议`：给事务T要修改的数据加X锁知道事务结束
   * `二级封锁协议`：一级封锁协议加事务T对要读取的数据加S锁，读取后释放
   * `三级封锁协议`：一级封锁协议加事务T对要读取的数据加S锁，事务结束后释放



[^额外说明]: 本篇文章代码基于MYSQL

