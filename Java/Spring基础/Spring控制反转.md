# Spring Ioc控制反转

控制反转是面向对象编程中的一种设计原则，可以用来减低计算机代码之间的耦合度。其中最常见的方式叫做依赖注入，还有一种方式叫“依赖查找”。通过控制反转，对象在被创建的时候，由一个调控系统内所有对象的外界实体，将其所依赖的对象的引用传递(注入)给它

------

在`org.springframework.beans`和`org.springframework.context`包是Spring框架的IoC容器的基础。该 BeanFactory 接口提供了一种能够管理任何类型对象的高级配置机制.ApplicationContext 是BeanFactory的子接口。

- 更容易与Spring的AOP功能集成
- 消息资源处理（用于国际化）
- 事件处理
- 特定`WebApplicationContext` 于应用程序层的上下文，例如在Web应用程序中使用的上下文（注意：`ContextLoaderListener`）

## ApplicationContext

该`org.springframework.context.ApplicationContext`接口代表Spring IoC容器，负责实例化，配置和组装bean。容器通过读取配置元数据获取有关要实例化，配置和组装的对象的指令。配置元数据以XML，Java注释或Java代码表示。它允许您表达组成应用程序的对象以及这些对象之间丰富的相互依赖性。

- 通过`MessageSource`界面访问i18n风格的消息。
- 通过`ResourceLoader`界面访问URL和文件等资源。
- 事件发布，即`ApplicationListener`通过使用接口实现接口的bean `ApplicationEventPublisher`。
- 加载多个（分层）上下文，让每个上下文通过`HierarchicalBeanFactory`界面聚焦在一个特定层上，例如应用程序的Web层

### ApplicationContext容器实例化

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml","dao.");
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- services -->

    <bean id="petStore" class="org.springframework.samples.jpetstore.services.PetStoreServiceImpl">
        <property name="accountDao" ref="accountDao"/>
        <property name="itemDao" ref="itemDao"/>
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions for services go here -->

</beans>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="accountDao"
        class="org.springframework.samples.jpetstore.dao.jpa.JpaAccountDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <bean id="itemDao" class="org.springframework.samples.jpetstore.dao.jpa.JpaItemDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions for data access objects go here -->

</beans>
```

```java
PetStoreService service = context.getBean("petStore", PetStoreService.class);
List<String> userList = service.getUsernameList();
```

### MessageSource实现消息源

该`ApplicationContext`接口扩展了一个名为的接口`MessageSource`，因此提供了国际化功能。Spring还提供了 `HierarchicalMessageSource`接口，可以分层次地解析消息。这些接口共同提供了Spring影响消息解析的基础。Spring中主要实现包括`ResourceBundleMessageSource`和 `StaticMessageSource`，这些接口上定义的方法包括：

- `String getMessage(String code, Object[] args, String default, Locale loc)`：用于从中检索消息的基本方法`MessageSource`。如果未找到指定区域设置的消息，则使用默认消息。传入的任何参数都使用`MessageFormat`标准库提供的功能成为替换值。
- `String getMessage(String code, Object[] args, Locale loc)`：基本上与前一个方法相同，但有一点不同：无法指定默认消息。如果找不到该消息，`NoSuchMessageException`则抛出a。
- `String getMessage(MessageSourceResolvable resolvable, Locale locale)`：前面方法中使用的所有属性也包装在一个名为的类中 `MessageSourceResolvable`，您可以使用此方法。

### ApplicationEventPublisher实现事件订阅

`ApplicationContext`通过`ApplicationEvent` 类和`ApplicationListener`接口提供事件处理。如果将实现`ApplicationListener`接口的bean 部署到上下文中，则每次 `ApplicationEvent`将其发布到该`ApplicationContextbean`时，都会通知该bean

### Spring资源抽象和访问

Spring的`Resource`接口是一个更强大的接口，用于抽象对低级资源的访问；同时`ResourceLoader`接口旨在由可以返回（即加载）Resource实例的对象实现

Resource基本实现包括：

- `UrlResource`
- `ClassPathResource`
- `FileSystemResource`
- `ServletContextResource`
- `InputStreamResource`
- `ByteArrayResource`

### Environment容器环境抽象

Environment接口是集成在容器模型应用环境的两个关键方面的抽象：`profile`s 和 `properties`



## BeanFactory

BeanFactory是Spring实现Ioc功能的主要依据，主要实现Bean实例化，销毁等周期管理；ApplicationContext实质是BeanFactory的子接口



## Bean

在容器本身内，这些bean定义表示为`BeanDefinition` 对象，其中包含（以及其他信息）以下元数据：

- 包限定的类名：通常是正在定义的bean的实际实现类。
- Bean行为配置元素，说明bean在容器中的行为方式（范围，生命周期回调等）。
- 引用bean执行其工作所需的其他bean引用
- 要在新创建的对象中设置的其他配置设置 - 例如，池的大小限制或在管理连接池的Bean中使用的连接数。

### Bean实例化

- 构造函数实例化

  ```xml
    <bean id="exampleBean" class="examples.ExampleBean"/>
  ```

- 静态工厂方法实例化

  ```xml
    <bean id="clientService" class="examples.ClientService" factory-method="createInstance"/>
  ```

- 实例工厂方法实例化

  ```xml
    <bean id="serviceLocator" class="examples.DefaultServiceLocator"/>
    <bean id="clientService" factory-bean="serviceLocator" factory-method="createClientServiceInstance"/>
  ```

### Bean依赖注入

### Bean实例范围

- `singleton`
- `prototype`
- `request`
- `session`
- `application`
- `websocket`