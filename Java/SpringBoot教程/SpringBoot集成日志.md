# SpringBoot集成日志

日志指的是对活动的记录，而日志框架则是简化和规范对于Java平台的日志记录。一个日志框架的实现主要包括：记录器，渲染器和管理器。

**记录器：**记录器是用于应用程序记录日志而不需要考虑日志来源

**渲染器：**渲染器是将日志进行格式化输出

**管理器：**监听日志信息的产生，并输出到指定目的地

**主要日志框架：**

- Log4J
- Java Util Logging
- Apache Commons Logging
- SLF4J
- Logback

**日志输出格式：**

- 日期和时间：精确到毫秒，轻松地排序
- 日志级别: `ERROR`, `WARN`, `INFO`, `DEBUG`, or `TRACE`
- 进程ID
- `---`分离器来区分实际日志消息的开始
- 线程名称
- 日志来源类名称
- 日志信息

## Spring日志配置

**日志输出级别：**

```
logging.level.web=DEBUG/INFO/WARN
```

**日志输出格式：**

```
logging.pattern.console=%clr(%d{yyyy-MM-dd HH:mm:ss}){green} %clr([%thread]){blue} %clr(%-5p){cyan} %clr(%-100logger){green}- %clr(%msg){magenta}%n 
logging.pattern.file=%d{yyyy/MM/dd-HH:mm} [%thread] %-5level %logger- %msg%n
```

- `%d{HH:mm:ss.SSS}`——日志输出时间
- `%thread`——输出日志的进程名字，这在Web应用以及异步任务处理中很有用
- `%-5level`——日志级别，并且使用5个字符靠左对齐
- `%logger` ——日志输出者的名字
- `%msg`——日志消息
- `%n`——平台的换行符

**日志文件输出：**

```
logging.file.name=my.log
logging.pattern.file=%clr(%d{yyyy-MM-dd HH:mm:ss}){green} %clr([%thread]){blue} %clr(%-5p){cyan} %clr(%-100logger){green}- %clr(%msg){magenta}%n
logging.file.max-size=1MB
```

- `logging.file.name` - 配置输出日志文件名
- `logging.pattern.file` - 文件日志输出格式
- `logging.file.max-size`- 日志文件最大值