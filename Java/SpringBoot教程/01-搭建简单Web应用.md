# 1 - 创建简单Web应用
本节主要讲述如何创建一个基于SpringBoot的Web应用
1. 创建项目骨架，添加`pom.xml`文件
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    	<modelVersion>4.0.0</modelVersion>
    
    	<artifactId>your artifactId</artifactId>
    	<groupId>your groupId</groupId>
    	<version>0.0.1-SNAPSHOT</version>
    
    	<parent>
    		<groupId>org.springframework.boot</groupId>
    		<artifactId>spring-boot-starter-parent</artifactId>
    		<version>2.1.9.RELEASE</version>
    	</parent>
    </project>
    ```
2. 添加Spring-boot依赖
    ```xml
    <dependencies>
    	<dependency>
    		<groupId>org.springframework.boot</groupId>
    		<artifactId>spring-boot-starter-web</artifactId>
    	</dependency>
    </dependencies>
    ```
3. 编写主程序   
    ```java
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;
    
    @RestController
    @EnableAutoConfiguration
    public class QuickStartApp {
        @RequestMapping("/")
        public String home(){
            return "hello world";
        }
        public static void main(String[] args) {
            SpringApplication.run(QuickStartApp.class,args);
        }
    }
    ```
4. 启动项目`main`方法
5. 通过访问`http://localhost:8080`测试
