# Spring Framework概述

Spring最初由Rod Johnson设计及开发，并首次公布于*Expert One-on-One J2EE Design and Development书中；*

Spring Framework是Java平台的一个开源全栈应用程序框架和控制反转实现，并在Java开发中成为实际上的开发标准，代替了EJB模型。

**Spring Framework核心特性**：

- 基于 JavaBeans 的采用控制反转原则的配置管理
- 一个可用于 Java EE 等运行环境的核心 Bean Factory
- 数据库事务的一般化抽象层，允许声明式事务管理器，简化事务的划分使之与底层无关
- 内建的针对 JTA 和单个 JDBC 数据源的一般化策略，使Spring的事务支持不要求 Java EE 环境
- JDBC 抽象层提供了有针对性的异常等级，简化了错误处理
- 以资源容器，DAO 实现和事务策略等形式与 Hibernate，JDO 和 MyBatis 、SQL Maps 集成
- 灵活的基于核心 Spring 功能的 MVC 网页应用程序框架
- 提供诸如事务管理等服务的AOP框架。

![Spring结构图](../image/Spring结构图.svg)