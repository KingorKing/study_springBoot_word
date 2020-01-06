```java
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
```

***@SpringBootConfiguration: Spring Boot的配置类***
标注在某个类上，表示这是一个SpringBoot配置类 1

@Configuration：配置类上来注释这个注解

配置类-----配置文件  配置类也是容器中的组件 @Componpent

***@EnableAutoConfiguration:开启自动配置功能***
以前需要配置的东西，Spring Boot帮我们自动配置

``@AutoConfigurationPackage
@Import({EnableAutoConfigurationImportSelector.class})``

***@AutoConfigurationPackage***
自动配置包   
将主配置类（@SpringBootApplication配置的类）所在包以及下面所有的子包里面的所有的组件扫描到spring容器中

***@Import({Registrar.class})***

Spring 的底层注解，给容器导入一个组件，导入的组件由Registrar.class
	

***@Import({EnableAutoConfigurationImportSelector.class})*** 
给容器导入组件
EnableAutoConfigurationImportSelector：导入那些组件的选择器
将所有需要导入的组件以全类名的方式返回；这些组件就会被添加到容器中

***@PropertySource 与@ImportResource***

- @PropertySource：加载指定的配置文件

- @ImportResource：导入spring 的配置文件，让配置文件生效
  SprignBoot中没有spring配置文件，自己编写的配置文件也不能识别；
  @ImportResource标注在配置类上就可以生效了

1.配置类=====Spring配置文件(之前)

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
       <bean id="helloService"class="com.joby.core.springboot.service.helloworld"></bean></beans>
```


***springBoot推荐给给容器中添加组件的方式（推荐使用全注解的方式）***

1. 配置类=====Spring配置文件(之前)
2. 推荐是用@Bean的方式给容器添加组件

## 配置文件占位符

### 1.随机数

```java
${random.value}、${random.int}、${random.long}、${random.uuid}
```

### 2.占位符获取之前配置的值，如果没有可以使用指定的默认值

```properties
person:
   age: 18${person.hello:hello}
   boss: false
   birth: 2000/06/03
   maps: {k1: v1,k2: 12}
   lists:
     - 李四
     - 张三
     - 王五
     - 赵六
   Dog:
     name: ${person.last-name}_大黄
     age: 2
   last-name: 乌漆嘛黑${random.uuid}
```

# Profile

 ==profile是Spring对不同环境提供不同配置功能的支持，可以通过激活、指定参数等方式快速切换环境==

#### 1.多profile文件形式

#### 2.多profile文档块模式

```yaml
##---文档块     dev、prod两个环境
##通过spring:profiles:active:dev 来启动某一个环境配置
---
server:
  port: 8081
spring:
  profiles:
    active: dev
---
server:
  port: 8082
spring:
  profiles: dev #指定属于哪个配置

```



#### 3.激活方式

1. 在配置文件中指定 spring.profiles.active=dev

2. 命令行的方式

   --spring. profiles. active=dev

3. 虚拟机参数

   -Dspring.profiles.active=dev



