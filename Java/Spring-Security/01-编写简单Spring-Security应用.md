# 01-编写简单Spring Security应用

本节主要讲述如何使用Spring Boot集成Spring Security创建简单应用（项目依赖包管理主要使用maven）;在这个简单的Spring Boot+Sprng Security应用中，Spring Boot会启用Spring Security默认配置，创建Servlet过滤器`	springSecurityFilterChain`并注册在所有request中

1. 创建SpringBoot基本Web应用
2. 添加maven依赖
   ```xml
    <?xml version="1.0" encoding="UTF-8"?>
	<project xmlns="http://maven.apache.org/POM/4.0.0"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	    <parent>
        	<groupId>org.springframework.boot</groupId>
        	<artifactId>spring-boot-starter-parent</artifactId>
        	<version>2.2.0.RELEASE</version>
        </parent>
	    <modelVersion>4.0.0</modelVersion>

	    <artifactId>spring-boot-security-simple</artifactId>

	    <dependencies>
	        <dependency>
	            <groupId>org.springframework.boot</groupId>
	            <artifactId>spring-boot-starter-web</artifactId>
	        </dependency>
	        <dependency>
	            <groupId>org.springframework.boot</groupId>
	            <artifactId>spring-boot-starter-security</artifactId>
	        </dependency>
	    </dependencies>

	</project>
   ```
3. 添加SpringBoot入口函数
   ```java
    @RestController
	@SpringBootApplication
	public class SimpleSecurityApp {

	    @RequestMapping("/")
	    public String root() {
	        return "a simple spring boot project with spring security";
	    }


	    public static void main(String[] args) {
	        SpringApplication.run(SimpleSecurityApp.class,args);
	    }
	}
   ```

4. 添加Spring Security配置`SecurityConfig`
   ```java
	@EnableWebSecurity
	public class WebSecurityConfig {

	    @Bean
	    public UserDetailsService userDetailsService() {
	        UserDetails user = User.withDefaultPasswordEncoder()
	                .username("user")
	                .password("password")
	                .roles("USER")
	                .build();
	        return  new InMemoryUserDetailsManager(user);
	    }
	}
   ```
>备注：应用会创建默认拦截器链`DefaultSecurityFilterChain`


