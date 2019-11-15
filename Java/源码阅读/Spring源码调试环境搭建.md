# Spring源码调试环境搭建

## 初步搭建调试环境

- 获取Spring源码

  `git clone [git@github.com](mailto:git@github.com):spring-projects/spring-framework.git`

- 编译源代码

  `gradlew :spring-oxm:compileTestJava`

- 将源码导入到IDEA中

- 创建`demo module`进行调试

## Spring调试源码demo

- 创建`Gradle Module demo`

- 添加依赖build.gradle

  ```Gradle
  plugins {
      id 'java'
  }
  
  group 'org.springframework'
  version '5.2.0.BUILD-SNAPSHOT'
  
  sourceCompatibility = 1.8
  
  repositories {
      mavenCentral()
  }
  
  dependencies {
      compile(project(":spring-beans"))
      compile(project(":spring-core"))
      compile(project(":spring-expression"))
      compile(project(":spring-context"))
      compile(project(":spring-instrument"))
      testCompile group: 'junit', name: 'junit', version: '4.12'
  }
  ```

- 创建service类

  

  ```java
  public class LoginService {
   public String login(String username){
    	System.out.println(username + "登录...");
    	return "success";
    }
  }
  ```

  

- 创建test类

  ```java
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;
  
  public class Test {
  
  	public static void main(String[] args) {
  		ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
  		LoginService service = (LoginService)context.getBean("loginService");
  		service.login("admin");
  	}
  
  }
  ```

  

## 如何阅读Spring源码

- 搭建调试环境进行debugger
- 对源码进行注释