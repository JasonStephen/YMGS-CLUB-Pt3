# SpringBoot 复习回顾

---

## 熟悉配置文件的分布


---
# SpringBoot入门
## 1. 注释
### 主程序启动注释
    @SpringBootApplication 主程序启动类  
    @SpringApplication.run(SpringBootReviewApplication.class, args); 启动SpringBoot程序  
    @RestController 组合注释，相当于@Controller和@ResponseBody  
    @GetMapping("/hello") GET请求，等同于@RequestMapping(method = RequestMethod.GET)

### 测试类注释
    @RunWith(SpringRunner.class) 测试类启动时加载SpringBoot测试环境
    @SpringBootTest 指定SpringBoot程序启动类
    @AutoWired 自动注入指定的实例对象
    @Test 测试方法

### 配置文件属性用@ConfigurationProperties注入
    @Component 组件注释，将指定的类注入到SpringBoot容器中
    @ConfigurationProperties(prefix = "person") 指定配置文件中的属性前缀

#### 示例
```java
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private int id;      
    public void setId(int id) {
        this.id = id;}}
```

### 配置文件属性用@Value注入
    @Value("${person.name}") 指定配置文件中的属性

#### 示例
```java
@Component
public class Person {
@Value("${person.id}")
    private int id;      
}
```

### 配置文件属性用@PropertySource注入
    @PropertySource(value = {"classpath:person.properties"}) 指定配置文件的位置

示例
```java
@Configuration
@PropertySource("classpath:test.properties")
        @EnableConfigurationProperties(MyProperties.class) @ConfigurationProperties(prefix = "test")
        public class MyProperties {
}
```
测试类
```java
@Autowired
private MyProperties myProperties;
@Test
public void myPropertiesTest() {
        System.out.println(myProperties);
        }
```

### 配置文件属性用@ImportResource注入
    @ImportResource(locations = {"classpath:beans.xml"}) 指定配置文件的位置

代码
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                      http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="myService" class="com.itheima.config.MyService" />
</beans>
```

    在项目启动类上加上 @ImportResource(locations = {"classpath:beans.xml"}) 注解，即可将配置文件中的bean注入到SpringBoot容器中

测试类
```java
@Autowired
private ApplicationContext applicationContext;
@Test
public void iocTest() {
        System.out.println(applicationContext.containsBean("myService"));
        }
```

### 配置文件属性用@Configuration编写自定义配置类
    使用@Configuration注解的类，相当于一个Spring配置文件，可以使用@Bean注解将方法返回的实例注入到SpringBoot容器中
    使用@Bean进行组件配置

示例
```java
@Configuration
public class MyConfig {
    @Bean
    public MyService myService(){
        return new MyService();
    }
}
```


---
# Springboot核心配置和注解
## 1. 全局配置文件
### Properties
```properties
server.address=80
server.port=8443
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.config.additional-location=
spring.config.location= 
spring.config.name=application
```

### Yml
行内式写法
```yaml
server:
  port: 8081
  path: /hello
```

缩进式写法
```yaml
person:
  hobby:
    - play
    - read
    - sleep
```

## 2. 两种注释方法的对比分析
![img.png](img.png)

## 3. Profile多环境配置
### 3.1 多配置文件
    application-{profile}.properties

    通过命令行方式激活指定环境的配置文件
    在全局配置文件设置Spring。profiles.active属性激活
