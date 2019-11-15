# SpringBoot连接池选型



在软件工程中，连接池是保持，以便连接可以重复使用的数据库连接的缓存时，需要未来的数据库请求。在连接池，创建一个连接后，它被放置在池中，它再次被使用，这样一个新的连接没有建立起来。如果正在使用的所有连接，一个新的连接，且被添加到池中。连接池也减少了时间的用户必须等待建立与数据库的连接数量。

**主要连接池框架：**

- Apache Commons DBCP
- HikariCP
- C3PO
- Tomcat Jdbc Pool
- Druid

## SpringBoot配置HikariCP

从SpringBoot2.0开始，[HikariCP](https://github.com/brettwooldridge/HikariCP)就作为默认数据库连接池存在，可在`spring-boot-starter-jdbc`配置POM中找到，可使用`spring.datasource.hikari`配置Hikari连接参数

```
spring.datasource.hikari.connection-timeout = 20000 #maximum number of milliseconds that a client will wait for a connection
spring.datasource.hikari.minimum-idle= 10 #minimum number of idle connections maintained by HikariCP in a connection pool
spring.datasource.hikari.maximum-pool-size= 10 #maximum pool size
spring.datasource.hikari.idle-timeout=10000 #maximum idle time for connection
spring.datasource.hikari.max-lifetime= 1000 # maximum lifetime in milliseconds of a connection in the pool after it is closed.
spring.datasource.hikari.auto-commit =true #default auto-commit behavior.
```

## SpringBoot配置Druid

[Druid](https://github.com/alibaba/druid)是阿里数据库事业部出品的，能够提供强大的监控和扩展功能的数据库连接池

**配置Druid：**

```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.20</version>
</dependency>
```