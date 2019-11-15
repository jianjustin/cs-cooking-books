# 2 - 创建RestAPI服务应用
本节主要使用H2数据库作为存储库，创建简单的**CRUD**的RestAPI服务

**什么是H2数据库？**
H2是一个开源数据库，用Java编写。它非常快而且体积很小。它主要用作内存数据库，这意味着它将数据存储在内存中，并且不会将数据持久存储在磁盘上

1. 创建基本`SpringBoot`应用，并添加依赖
    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
    </dependency>
    ```
    
2. 创建入口`main`类
    ```java
    @SpringBootApplication
    @RestController
    public class DataH2App {
        @RequestMapping("/")
        public String home(){
            return "test H2 Database based Person Rest API";
        }
        public static void main(String[] args) {
            SpringApplication.run(DataH2App.class,args);
        }
    }
    ```
    
3. 创建配置文件`application.properties`，添加数据库配置
    ```properties
    spring.datasource.url=jdbc:h2:mem:testdb
    spring.datasource.driverClassName=org.h2.Driver
    spring.datasource.username=sa
    spring.datasource.password=
    spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
    spring.h2.console.path=/h2-console
    spring.h2.console.enabled=true
    ```
    
4. 添加初始sql（在`/resource`目录下添加`data.sql`）
5. 创建实体类`Person`
    ```java
    @Entity
    public class Person {
        @Id
        @GeneratedValue
        public int id;
        public String name;
        public int age;
        public String email;
    }
    ```
    
6. 创建实体操作`Repository`
    ```java
    public interface PersonRepository extends CrudRepository<Person,Integer> {
    
    }
    ```
    
1. 创建`Service`接口
    ```java
    @Service
    public class PersonService {
        @Autowired
        public PersonRepository personRepository;
        
        public List<Person> getAllPersons() {
            List<Person> persons = new ArrayList<Person>();
            personRepository.findAll().forEach(person -> persons.add(person));
            return persons;
        }
        public Person getPersonById(int id) {
            return personRepository.findById(id).get();
        }
        public void saveOrUpdate(Person person) {
            personRepository.save(person);
        }
        public void delete(int id) {
            personRepository.deleteById(id);
        }
    }
    ```
    
1. 创建`Controller`接口
    ```java
    @RestController
    public class PersonController {
        @Autowired
        PersonService personService;
    
        @GetMapping("/persons")
        private List<Person> getAllPersons() {
            return personService.getAllPersons();
        }
    
        @GetMapping("/persons/{id}")
        private Person getPerson(@PathVariable("id") int id) {
            return personService.getPersonById(id);
        }
    
        @DeleteMapping("/persons/{id}")
        private void deletePerson(@PathVariable("id") int id) {
            personService.delete(id);
        }
    
        @PostMapping("/persons")
        private int savePerson(@RequestBody Person person) {
            personService.saveOrUpdate(person);
            return person.id;
        }
    }
    ```

3. 访问`http://localhost:8080/h2-console`测试H2数据库
4. 通过Postman测试接口可用性