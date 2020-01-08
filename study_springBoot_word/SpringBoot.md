```java
@EnableAutoConfiguration
@EnableAutoConfiguration
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

##### @PropertySource

根据@Configuration配置类上的@PropertySource专门引用的配置文件

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

### 配置文件的加载顺序

==spring boot启动会扫描一下位置的application.properties或者application.yml文件作为SpringBoot默认配置文件==

- file../config/
- file../
- classpath:/config/
- classpath:/

==优先级由高到低，高优先级配置覆盖优先级配置== 会产生互补配置

==可以通过配置spring.config.location来修改默认配置==

- 运维时，使用
- 先将项目打包，在控制台java -jar 文件名 -spring.config.location= 指定新配置文件路径



### 外部配置加载顺序

1.命令行参数    例如:--server.port=8080 --server.context.path=asda 语法--属性=值 --属性2=值   多个参数用空格分开

先加载带profile再加载不带profile

2.jar包外部的application-{profile}.propertie或者.yml{带spring.profile}

### 自动配置的原理 （精髓）

自动配置原理：

1）SpringBoot启动的时候加载主配置类，开启了自动配置功能 @EnableAutoConfiguration

2）@EnableAutoConfiguration 作用：

- 利用AutoConfigurationImportSelector给容器中导入一些组件  ？

- 哪些组件参照：selectImports()方法的内容
- List<String> configurations = this.getCandidateConfigurations(annotationMetadata, attributes); 选取候选的配置（一些组件的类名）

- ==SpringFactoriesLoader.loadFactoryNames 扫描所有jar包下面的类路径下的 META-INF/spring.factories 
  把扫描到的这些文件的内容包装成properties对象
   从properties中获取到EnableAutoConfiguration.class类(类名)对应的值 然后再将他们添加到容器中==

-  **==将类路径下META-INF/spring.factories里面配置的所有EnableAutoConfiguration的值加入到容器中==**

  每一个这样的xxxAutoConfiguration类都是容器中的一个组件，都加入到容器，用他们做自动配置

3）每一个自动配置类进行自动配置功能

4）以HttpEncodingAutoConfiguration类为例解释自动配置原理；

```java
@Configuration(proxyBeanMethods = false)//表示这是一个配置类 也可以给容器添加组件
@EnableConfigurationProperties({HttpProperties.class})//启用指定类的ConfigurationProperties功能，将配置文件中对应的值和HttpProperties属性绑定起来，并把HttpProperties加入到ioc容器中

@ConditionalOnWebApplication(type = Type.SERVLET)//spring底层@Conditional注解，根据不同的条件，如果满足条件，整个配置类里面的配置才会生效， 判断当前应用是否时web应用

@ConditionalOnClass({CharacterEncodingFilter.class})//判断当前项目有没有这个class
//CharacterEncodingFilter；springMVC中进行乱码解决的过滤器

@ConditionalOnProperty(prefix = "spring.http.encoding",value = {"enabled"},matchIfMissing = true)
//即使配置文件中不配置spring.http.encoding.enabled=true,也是默认生效的

public class HttpEncodingAutoConfiguration {
    
    //他已经和springboot配置文件映射了
	private final HttpProperties.Encoding properties;
    
	//只有一个有参构造的情况下，参数的值就会从容器中拿
	public HttpEncodingAutoConfiguration(HttpProperties properties) {
		this.properties = properties.getEncoding();
	}

	@Bean //给容器添加一个组件，这个组件的一些属性值需要从properties中获取
	@ConditionalOnMissingBean
	public CharacterEncodingFilter characterEncodingFilter() {
		CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
		filter.setEncoding(this.properties.getCharset().name());
		filter.setForceRequestEncoding(this.properties.shouldForce(Type.REQUEST));
		filter.setForceResponseEncoding(this.properties.shouldForce(Type.RESPONSE));
		return filter;
	}

```

根据当前不同条件判断，决定这个配置类是否生效

5）所有配置文件中能配置的属性都在xxxProperties类中封装这；配置文件能配置什么就可以参照某个功能对应的这个属性类

```java
@ConfigurationProperties(prefix = "spring.http")//从配置文件中获取指定的值和bean的属性进行绑定
public class HttpProperties {
```







SpringBoot(精髓)：

​	1）、SpringBoot会加载大量的自动配置类

​	2）、看需要的功能SpringBoot默认有没有自动配置类 （例：xxxAutoConfiguration）

​	3）、再看自动配置类中配置了那些组件，如果有就不需要再配置

​	4）、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性，就可以在配置文件中指定属性的值



xxxAutoConfigurartion:自动配置类；

给容器中添加组件

xxxProperties:封装配置文件中相关属性







**自动配置类必须在一定的条件下才会配置**

可以通过debug=true 可以让控制台打印配置报告，这样就可以很方便的看到哪些自动配置类生效



### 日志

JDBC--数据库驱动的关系

写一个统一的接口层；日志门面（欸之的一个抽象层）；logging-abstract。jar

市面上的日志框架



| 日志门面（日志的抽象层）                                     | 日志的实现                                             |
| ------------------------------------------------------------ | ------------------------------------------------------ |
| ~~JCL（jakarta Commons Logging）~~、SLF4j（Simple Logging Facade for Java）、~~jboss-logging~~ | ~~log4j~~、~~JUL（java.util.logging）~~logback、log4j2 |

左边选一个门面（抽象层）、右边来选一个实现

SpringBoot:底层是Spring框架，Spring框架默认是用JCL

​		==**SpringBoot选用SLF4j和logback**==

### SLF4j使用

**1、如何在系统中使用SLF4j**

调用日志 抽象层的方法;    (官方文档中使用教程)

应该先导入slf4j的jar包 和logback的实现jar

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```

![images/SpringBoot日志](D:\git_library\study_springBoot_word\images\SpringBoot日志.png)

每一个日志的实现框架都有子级的配置文件，使用slf4j以后，**配置文件还是做成日志实现框架自己本身的配置文件**



统一日志记录，即使是别的框架和我一起统一使用Slf4j进行输出

有时候开发应用是可能用到一些别的框架，别的框架使用的日志框架可能会不统一，那就先剔除掉把接口统一

如何让系统中所有的日志都统一到slf4j：

1、将系统中其他日志框架先排除出去

2、用中间包来替换原有的日志框架；

3、导入slf4j其他的实现

```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
  </dependency>
```

SpringBoot 使用它来做日志功能

```xml
 <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-logging</artifactId>
 </dependency>
```

![](D:\git_library\study_springBoot_word\images\springboot中的日志框架配置.png)

1）SpringBoot底层也是使用Slf4j+logback的方式进行日志记录

2）SpringBoot也罢其他日志都替换成SlF4j

3)中间替换包

4）SpringBoot在引入其他框架时，要剔除默认的日志依赖移除掉

​		Spring框架用的时Commons-logging

​		==**SpringBoot能自动适配所有的日志，而且底使用slf4j+logback的方式记录，在引入其他框架时，只需要移除这个框架依赖的日志框架**==