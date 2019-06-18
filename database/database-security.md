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

  

[^额外说明]: 本篇文章代码基于MYSQL

