# SpringBoot 

<img src="images/springboot.png"/>

## 课程目标

- 了解AnnotationConfig理念
- 掌握@Configuration注解的作用
- 掌握@Bean注解的作用
- 掌握@ComponentScan注解的作用
- 掌握AnnotationConfig方式的依赖注入
- 掌握@Import和@ImportResource导入资源文件
- 掌握@SpringBootApplication及其上面的元注解的作用
- 了解SpringBoot常见的自动配置功能
- 掌握SpringBoot属性绑定
- 掌握SpringBoot搭建SSM开发环境
- 了解日志的作用



## SpringBoot介绍

参考百度百科: https://baike.baidu.com/item/Spring%20Boot/20249767?fr=aladdin

Spring Boot是由Pivotal团队提供的全新框架, 其设计目的是用来简化新Spring应用的初始搭建以及开发过程. 该框架使用了特定的方式来进行配置, 从而使开发人员不再需要定义样板化的配置. 通过这种方式, Spring Boot致力于在蓬勃发展的快速应用开发领域(rapid application development)成为领导者

该框架非常火，目前新开项目几乎都是基于SpringBoot搭建，非常符合微服务架构要求，企业招聘大多都要求有SpringBoot开发经验，属于面试必问的点



## AnnotationConfig

我们通常使用Spring都会使用xml对Spring进行配置，随着功能以及业务逻辑的日益复杂，应用伴随着大量的XML配置文件以及复杂的Bean依赖关系。随着Spring 3.0的发布，Spring IO团队逐渐开始摆脱XML配置文件，并且在开发过程中大量使用“约定优先配置”（convention over configuration）的思想来摆脱Spring框架中各类繁复纷杂的配置（即是Java Config）。  

从Spring 3起，JavaConfig功能已经包含在Spring核心模块，它允许开发者将bean的定义和Spring的配置编写到到Java类中。但是，仍然允许使用经典的XML方式来定义bean和配置spring，JavaConfig是另一种替代解决方案。  Spring的JavaConfig也叫做Annotataion配置

### 准备工作

使用Maven创建JavaSE项目,在pom.xml中添加spring依赖

```xml
<properties>
    <java.version>1.8</java.version>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <spring.version>5.0.8.RELEASE</spring.version>
</properties>

<dependencies>
    <!-- JUnit4测试工具 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>

    <!-- Spring -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>${spring.version}</version>
    </dependency>
   
    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.8.7</version>
    </dependency>
    
    <!-- lombok插件 -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.16.6</version>
        <scope>provided</scope>
    </dependency>
</dependencies>

<build>
    <plugins>
        <!--Java编译器插件-->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.5.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
                <encoding>UTF-8</encoding>
            </configuration>
        </plugin>
    </plugins>
</build>
```

### 回顾XML方式配置IoC

1. 定义两个Bean

   ```java
   @Setter@Getter@ToString
   public class SomeBean {
       private OtherBean otherBean;
       
       public SomeBean() {
           System.out.println("SomeBean被创建");
       }
       
       public void init() {
           System.out.println("SomeBean被初始化");
       }
       
       public void destroy() {
           System.out.println("SomeBean被销毁");
       }
   }

   @ToString
   public class OtherBean { 
       public OtherBean() {
           System.out.println("OtherBean被创建");
       }
   }
   ```

2. 在XML配置文件中去配置该Bean交给Spring管理

   ```xml
   <!--applicationContext.xml-->
   <bean id="someBean" class="cn.wolfcode.common.SomeBean" />
   ```

3. 启动Spring读取该XML文件创建容器对象,从容器中获取SomeBean对象

   ```java
   public class App {
       @Test
       public void testXmlConfig() {
           //1:读取XML配置文件,创建Spring容器
           ApplicationContext ctx = 
               new ClassPathXmlApplicationContexnt("applicationContext.xml");
           //2:从容器中取出SomeBean对象
           SomeBean someBean = ctx.getBean(SomeBean.class);
           System.out.println(someBean);
       }
   }
   ```

### AnnotationConfig方式配置IoC

AnnotationConfig方式中使用注解彻底的替代XML文件,那么到底要怎么告诉Spring容器,Bean没有定义在XML文件中,而是定义在一个Java配置类中
>@Configuration: 在类上贴该注解表示该类是取代applicationContext.xml文件,也称为Spring的配置类
>
>@Bean: 在Spring的配置类的方法上贴该注解表示该方法的返回的对象交给Spring容器管理,替代applicationContext.xml中的bean标签
>
>@ComponentScan: 在Spring配置类上贴该注解表示开启组件扫描器,默认扫描当前配置类所在的包,也可以自己指定,替代xml配置中的<context:component-scan />标签
>
>AnnotationConfigApplicationContext: 该类是ApplicationContext接口的实现类,该对象是基于AnnotationConfig的方式来运作的Spring容器

1. 定义一个配置类,替代之前的XML文件,类中定义方法,返回bean对象交给Spring管理

   ```java
   /**
    * @Configuration
    * 贴有该注解的类表示Spring的配置类
    * 用于替代传统的applicationContext.xml
    */
   @Configuration
   public class AnnotationConfig { 
       
       /**
        * @Bean
        * 该注解贴在配置类的方法上,该方法会被Spring容器自动调用
        * 并且返回的对象交给Spring管理
        * 相当于<bean id="someBean" class="cn.wolfcode.common.SomeBean"/>
        */
       @Bean
       public SomeBean someBean() {
           SomeBean someBean = new SomeBean();
           return someBean;
       }
   }
   ```

2. 加载配置类,启动AnnotationConfigApplicationContext容器对象,测试效果

   ```java
   public class App {
       @Test
       public void testAnnotationConfig() {
           //1:加载配置类,创建Spring容器
           ApplicationContext ctx = 
               new AnnotationConfigApplicationContext(AnnotationConfig.class);
           //2:从容器中取出SomeBean对象
           SomeBean someBean = ctx.getBean(SomeBean.class);
           System.out.println(someBean);
       }
   }
   ```

以上案例中,在配置类内部去定义方法返回bean对象交给Spring管理的方式存在一个问题,就是如果需要创建的Bean很多的话,那么就需要定义很多的方法,显示类比较累赘使用起来不方便,在XML配置的方式中可以通过配置组件扫描器的方式来减少bean标签的定义,那么在AnnotationConfig的方式中也可以通过添加组件扫描器的方式来减少方法的定义

```java
@Configuration //表示该类是Spring的配置类
@ComponentScan //开启组件扫描器,默认扫描当前类所在的包,及其子包
public class AnnotationConfig { }
```

如果需要扫描的包不是配置类所在的包时,我们可以通过注解中的value属性来修改扫描的包

注意:组件扫描的方式只能扫描我们自己写的组件,如果某个bean不是我们写的,则还是要通过在配置类中定义方法来处理,两者是可以同时存在的

### SpringTest方式加载配置类

#### 传统XML方式

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:xml文件路径")
public class App { ... }
```

#### 配置类方式

> @ContextConfiguration注解不仅支持XML方式启动Spring测试,也支持配置类的方式,配置classes属性来指定哪些类是配置类即可

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes={配置类1.class, 配置类2.class, ...})
public class App { ... }
```

### @Bean注解中的属性

在xml配置bean的方式中,我们可以在bean标签中的id,name,init-method,destroy-method,scope等属性来完成对应的配置,在使用AnnotationConfig方式中我们也一样能通过相应的配置来完成同样的效果,这些效果大多封装到@Bean注解的属性中

```
@Bean注解中的属性
  name: 对应bean标签中的name属性,用于给bean取别名
  initMethod: 对应bean标签中的init-method属性,配置bean的初始化方法
  destroyMethod: 对应bean标签中的destroy-method属性,配置bean的销毁方法
  使用jsr250-api的@PostConsturct 和@PreDestroy
注意: 在配置类的方式中有许多的默认规定,比如:
  bean的id就是当前方法名
  配置多例则是在方法上添加@Scope("prototype")注解来实现,一般不用配,默认单例即可
```

### AnnotationConfig方式配置DI

#### 传统XML方式

```xml
<bean id="someBean" class="cn.wolfcode.common.someBean">
    <property name="otherBean" ref="otherBean"/>
</bean>

<bean id="otherBean" class="cn.wolfcode.common.OtherBean"/>
```

#### 配置类方式

在配置类方式中我们有两种方式可以完成依赖注入,无论是哪种方式,前提都是要先把Bean交给Spring管理,然后在把Bean注入过去后再使用setter方法设置关系

通用步骤:先把两个Bean交给Spring管理

```java
@Bean
public SomeBean someBean() {
    SomeBean someBean = new SomeBean();
    return someBean;
}

@Bean
public OtherBean otherBean() {
    return new OtherBean();
}
```

##### 方式1:(掌握)

把需要注入的Bean对象作为参数传入到另一个Bean的方法声明中,形参名称最好跟Bean的id一致

```java
//在声明SomeBean的方法形参中直接注入OtherBean对象
@Bean
public SomeBean someBean(OtherBean otherBean) {
    SomeBean someBean = new SomeBean();
    someBean.setOtherBean(otherBean);
    return someBean;
}
```

##### 方式2:(了解)

在依赖的处理时直接调用setter方法传入另一个Bean的声明方法,如:

```java
@Bean
public SomeBean someBean() {
    SomeBean someBean = new SomeBean();
    /*
     * 调用otherBean()方法,拿到容器创建的otherBean对象
     * 然后赋值给SomeBean的属性,完成依赖注入
     */
    someBean.setOtherBean(otherBean());
    return someBean;
}
```

这种方式虽然可以完成依赖注入,但是获取到依赖对象的方式是通过调用另一个方法,而另一个方法的底层代码往往又是new一个对象返回,会给人一个错觉认为该对象不是单例的,但是实际上还是单例

### 配置文件的导入

#### @Import注解导入配置类

在Spring项目中一般都会有多个Spring的配置文件,分别配置不同的组件,最后关联到主配置文件中,该功能也是同样可以在配置类的方式中使用的

##### 传统XML方式

```xml
<!-- mvc.xml中导入applicationContext.xml -->
<import resource="classpath:applicationContext.xml"/>
```

##### 配置类方式

```java
//主配置类
@Configuration
@Import(OtherConfig.class) //在主配置类中关联分支配置类
public class MainConfig { ... }

//分支配置类
@Configuration
public class OtherConfig { ... }

//测试
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(MainConfig.class) //加载主配置类
public class App { ... }
```

#### @ImportResource注解导入XML配置

```java
//主配置类
@Configuration
@ImportResource("classpath:applicationContext.xml") //在主配置类中关联XML配置
public class MainConfig { ... }

//测试
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes=MainConfig.class) //加载主配置类
public class App { ... }
```

#### @PropertySource注解导入属性文件

##### 传统XML方式

```xml
<context:property-placeholder location="classpath:db.properties" 
                              system-properties-mode="NEVER"/>

<!-- 连接池对象 -->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
      init-method="init" destroy-method="close">
    <property name="url" value="${jdbc.url}" />
    <property name="username" value="${jdbc.username}" />
    <property name="password" value="${jdbc.password}" />
</bean>
```

##### 配置类方式

方式1(不讲):直接使用Properties对象去读取属性文件,然后取出属性值

方式2:使用@PropertySource + @Value

```java
/**
 *  @PropertySource:把属性配置加载到Spring的环境对象中
 *  @Value:从Spring环境对象中根据key读取value
 */
@Configuration
@PropertySource("classpath:db.properties")
public class AnnotationConfig {
    @Value("${jdbc.url}")
    private String url;

    @Value("${jdbc.username}")
    private String username;

    @Value("${jdbc.password}")
    private String password;

    @Bean(initMethod="init", destroyMethod="close")
    public DruidDataSource dataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        return dataSource;
    }
}
```

> XML配置中的<context:property-placeholder />标签在被扫到时,是会额外的去创建解析器去专门解析context开头的标签,该解析器是ContextNamespaceHandler,该解析器的内部又分别注册了各种不同功能的解析器,如: property-placeholder / component-scan / annotation-config 等等
>

方式3:直接在配置类中注入Spring的环境对象

```java
@Configuration
@PropertySource("classpath:db.properties")
public class AnnotationConfig {
    /**
     * environment:表示Spring的环境对象,该对象包含了加载的属性数据
     */
    @Autowired
    private Environment environment;

    @Bean(initMethod="init", destroyMethod="close")
    public DruidDataSource dataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setUrl(environment.getProperty("jdbc.url"));
        dataSource.setUsername(environment.getProperty("jdbc.username"));
        dataSource.setPassword(environment.getProperty("jdbc.password"));
        return dataSource;
    }
}
```

### 切换运行环境(了解)

在实际开发中,一个系统是有多套运行环境的,如开发时有开发的环境,测试时有测试的环境,不同的环境中,系统的参数设置是不同的,如:连接开发数据和测试数据库的URL绝对是不同的,那么怎么快速的切换系统运行的环境

<img src="images/profile.png"/>



## SpringBoot入门的Web案例

### 搭建环境

```xml
<!-- 打包方式jar包 -->
<packaging>jar</packaging>

<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.1.3.RELEASE</version>
</parent>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

### 程序代码

```java
@SpringBootApplication
@Controller
public class HelloController {

    @RequestMapping("/hello")
    @ResponseBody
    public String hello() {
        return "Hello Spring Boot";
    }

    public static void main(String[] args) {
        SpringApplication.run(HelloController.class, args);
    }
}
```

然后通过main方法启动程序,观察控制台输出内容,最后浏览器中输入http://localhost:8080/hello验证效果

<img src="images/springboot start.png"/>

### 提出疑问

1. 之前的web应用打包是war,为什么现在的打包方式是jar
2. 当前项目继承的spring-boot-starter-parent项目是什么鬼
3. 导入的依赖spring-boot-starter-web又是什么鬼
4. @SpringBootApplication注解有什么用
5. main方法中执行的代码SpringApplication.run(..)有什么用
6. 占用8080端口的Tomcat9服务器哪来的  

### 入门案例分析

1. 对于SpringBoot项目来说无论是普通应用还是web应用,其打包方式都是jar即可,当然web应用也能打war包,但是需要额外添加许多插件来运行,比较麻烦
2. 父项目spring-boot-starter-parent帮我们管理和导入了许多的基础依赖
3. spring-boot-starter-web集成了运行网站应用的相关环境和工具,包括:SpringMVC / Tomcat / Jackson 等等
4. @SpringBootApplication注解内部是3大注解功能的集成
   - @ComponentScan: 开启组件扫描
   - @SpringBootConfiguration: 作用等同于@Configuration注解,也是用于标记配置类
   - @EnableAutoConfiguration: 内部导入AutoConfigurationImportSelector,该类中有个getCandidateConfigurations方法,加载jar包中META-INF/spring.factories文件中配置的配置对象,自动配置定义的功能,包括: AOP / PropertyPlaceholder / FreeMarker / HttpMessageConverter / Jackson / DataSource / DataSourceTransactionManager / DispatcherServlet / WebMvc 等等
5. SpringApplication.run(..)的作用
   - 启动SpringBoot应用
   - 加载自定义的配置类,完成自动配置功能
   - 把当前项目配置到嵌入的Tomcat服务器
   - 启动嵌入的Tomcat服务器
6. spring-boot-starter-web依赖中嵌入的Tomcat9服务器,默认端口8080



## SpringBoot项目的独立运行

默认的Maven打包方式是不能正常的打包SpringBoot项目的,需要额外的引入打包插件,才能正常的对SpringBoot项目打成jar包,以后只要拿到该jar包就能脱离IDE工具独立运行了

```xml
<!-- pom.xml中添加插件 -->
<build>
    <plugins>
        <!-- SpringBoot打包插件 -->
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

1. 使用maven的package命令进行打包
2. 使用命令java -jar xxx.jar运行jar包

### 优缺点

优点:

1. 创建独立的Spring应用程序 
2. 嵌入的Tomcat,无需部署war文件
3. 简化Maven配置 
4. 自动配置Spring 
5. 提供生产就绪型功能,如:日志,健康检查和外部配置等
6. XML没有要求配置 
7. 非常容易和第三方框架集成起来 

缺点:

1. 版本更新较快,可能出现较大变化
2. 因为约定大于配置,所以经常会出现一些很难解决的问€题



## SpringBoot基本使用

### 项目构建

SpringBoot建议使用官方提供的工具来快速构建项目,网站:https://start.spring.io/ ,IDEA自带该功能,但需要联网使用

<img src="images/spring-init.png" />

注意:官方提供的构建工具默认只能选择固定的版本,有些版本之间的差异非常大,所以如果需要选择某个版本建议项目构建后,自行在pom.xml文件中修改版本,建议学习阶段选择**2.1.3**版本

### 目录结构

<img src="images/project.png" />

### 傻瓜式配置的工具包

SpringBoot非常优秀的地方在于提供了非常多以spring-boot-starter-* 开头的开箱即用的工具包,常见工具包有以下一些:
>   spring-boot-starter: 核心的工具包,提供了自动配置,日志和YAML配置支持
>
>   spring-boot-starter-aop: 提供了快速集成SpringAOP和AspectJ的工具包
>
>   spring-boot-starter-freemarker: 提供了快速集成FreeMarker的工具包
>
>   spring-boot-starter-test: 提供了测试SpringBoot应用的工具包
>
>   spring-boot-starter-web: 提供了快速集成web模块的工具包,包括基于SpringMVC,Tomcat服务器等
>
>   spring-boot-starter-actuator: 提供了生产环境中使用的应用监控工具包
>
>   spring-boot-starter-logging: 提供了对日志的工具包,默认使用Logback

### 热部署插件(了解)

除了使用JRebel来实现热部署,还可以使用Springboot提供的spring-boot-devtools包来完成Springboot应用热部署

```xml
<!-- SpringBoot热部署插件 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```

#### 插件的热部署原理(了解)

SpringBoot重启是reload重启,通过监控classpath的变化,如果classpath中的文件发生变化,即触发重启.springboot通过两个classpath来完成reload,一个basic classloader中加载不变的类(jar包中的类),一个restart classloader中加载classpath中的类(自己写的类),重启的时候,restart classloader中的类丢弃并重新加载,也就是说其实这个插件很...

#### 排除资源

```properties
#默认排除的资源
spring.devtools.restart.exclude=static/**,templates/**,public/**       
#增加额外的排除资源
spring.devtools.restart.additional-exclude=public/** #处理默认配置排除之外的
spring.devtools.restart.enabled=false #禁用自动重启
```

### 修改banner(了解)

SpringBoot提供了一些扩展点,比如修改banner:
在resources根目录中存入banner.txt文件,替换默认的banner

```properties
#application.properties
spring.main.banner-mode=off #关闭banner
```



## SpringBoot参数配置

### 参数来源

1. 命令行启动项目时传入的参数, 如: java -jar xxx.jar --server.port=80
2. ServletConfig和ServletContext 
3. 操作系统环境变量
4. application-{profile}.properties或者YAML文件
5. application.properties或者YAML文件

一般用的比较多的就是直接在**application.properties或者YAML**配置,其次是命令行启动方式,剩下的大家了解即可

### application.properties优先级

一个项目中可以有多个application.properties文件存放在不同目录中,此时他们会遵循固定的优先级来处理有冲突的属性配置

1. 项目/config/application.properties
2. 项目/application.properties
3. classpath:config/application.properties
4. classpath:application.properties

一般都在**classpath:application.properties**做配置,其他方式不使用

### 属性绑定

#### 准备工作

```properties
#application.properties
jdbc.username=root
jdbc.password=admin
```

#### @Value绑定单个属性

```java
@Component
@ToString
public class MyDataSource {
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;
}
```

#### @ConfigurationProperties绑定对象属性

```java
@Component
@Setter@ToString
@ConfigurationProperties("jdbc")
public class MyDataSource {
    private String username;
    private String password;
}

或者

@Bean
@ConfigurationProperties("jdbc")
public MyDataSource myDataSource() {
    return new MyDataSource();
}
```



## SpringBoot的Web开发

### 静态资源

1. 默认情况下,Springboot会从classpath下的 /static , /public , /resources , /META-INF/resources下加载静态资源
2. 可以在application.properties中配置spring.resources.staticLocations属性来修改静态资源加载地址
3. 因为应用是打成jar包,所以之前的src/main/webapp就作废了,如果有文件上传,那么就的必须去配置图片所在的路径

### 集成FreeMarker

在传统的SpringMVC中集成FreeMarker需要把FreeMarkerConfigurer和FreeMarkerViewResolve两个对象配置到Spring容器中,同时两个对象都要分别配置一些属性,还是比较麻烦的

在SpringBoot中,依靠自动配置功能,我们可以非常轻松的实现集成FreeMarker,只需要引入一个依赖即可

```xml
<!-- SpringBoot集成FreeMarker的依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-freemarker</artifactId>
</dependency>
```

#### 集成原理

SpringBoot的自动配置中含有FreeMarkerAutoConfiguration配置对象,该配置对象又导入了FreeMarkerReactiveWebConfiguration配置对象,在里面创建了FreeMarkerConfigurer和FreeMarkerViewResolve两个对象交给Spring管理,并且设置了默认的属性值,这些属性值来源于FreeMarkerProperties对象

#### 常见属性配置
```
spring.freemarker.enabled=true: 是否开启freemarker支持
spring.freemarker.allow-request-override: 是否允许request中的属性覆盖model中同名属性,默认false
spring.freemarker.allow-session-override: 是否允许session中的属性覆盖model中同名属性,默认false
spring.freemarker.cache: 是否支持模板缓存,默认false
spring.freemarker.charset=UTF-8: 模板编码
spring.freemarker.content-type=text/html: 模板contenttype
spring.freemarker.expose-request-attributes: 是否开启request属性暴露,默认false
spring.freemarker.expose-session-attributes: 是否开启session属性暴露,默认false
spring.freemarker.expose-spring-macro-helpers: 是否开启spring的freemarker宏支持,默认为false
spring.freemarker.prefer-file-system-access: 默认为true,支持实时检查模板修改
spring.freemarker.prefix: 加载模板时候的前缀
spring.freemarker.settings.*: 直接配置freemarker参数
spring.freemarker.suffix: 模板文件后缀
spring.freemarker.template-loader-path=classpath:/templates/: 模板加载地址
```

```properties
#一般我们会做1个配置,其余默认即可
spring.freemarker.expose-session-attributes=true #暴露session对象的属性
```

### 统一异常处理

#### 框架自带方式

SpringBoot默认情况下,会把所有错误都交给BasicErrorController类完成处理,错误的视图导向到 **classpath:/static/error/ 和 classpath:/templates/error/** 路径上,http状态码就是默认视图的名称

如: 出现404错误 -> classpath:/static/error/404.html 或者 出现5xx类错误 -> classpath:/static/error/5xx.html

#### 控制器增强器方式

自己定义一个控制器增强器,专门用于统一异常处理,该方式一般用于5xx类错误

```java
@ControllerAdvice //控制器增强器
public class ExceptionControllerAdvice {
    
    @ExceptionHandler(Exception.class) //处理什么类型的异常
    public String handlException(Exception e, Model model) {
        model.addAttribute("msg", e.getMessage());
        return "errorView"; //逻辑视图名称
    }
}
```



## MVC配置的规范接口WebMvcConfigurer

WebMvcConfigurer接口是JavaConfig配置SpringMVC的标准，在SpringBoot中，如果我们需要对SpringMVC做配置，则需要让配置对象实现该接口

### 添加拦截器

在传统的XML方式中，我们需要在< mvc:interceptors >标签中去注册我们自定义的拦截器，在SpringBoot，只需要让我们的配置对象去实现WebMvcConfigurer接口，实现接口中addInterceptors方法即可

```java
@SpringBootApplication
public class AppConfig implements WebMvcConfigurer {

    public static void main(String[] args) {
        SpringApplication.run(AppConfig.class, args);
    }

    public void addInterceptors(InterceptorRegistry registry) {
        // 注册拦截器
        registry.addInterceptor(拦截器对象)
                // 对哪些资源起过滤作用
                .addPathPatterns("/**")
                // 对哪些资源起排除作用
                .excludePathPatterns("/..");
    }
}
```

### 设置路径配置（了解）

SpringBoot的SpringMVC默认的< url-pattern >是 / ，如要改为*.do的模式，也是需要做些配置，SpringBoot多数用于前后端分离和微服务开发，资源的设计一般都要符合RESTFul规范，所有一般都是默认匹配 / ，不做改动

```java
@SpringBootApplication
public class AppConfig implements WebMvcConfigurer {

    public static void main(String[] args) {
        SpringApplication.run(AppConfig.class, args);
    }

    public void configurePathMatch(PathMatchConfigurer configurer) {
        //开启后缀匹配模式
        configurer.setUseRegisteredSuffixPatternMatch(true);
    }

    // 重新注册Servlet的映射
    @Bean
    public ServletRegistrationBean servletRegistrationBean(DispatcherServlet dispatcherServlet) {
        ServletRegistrationBean<DispatcherServlet> servletServletRegistrationBean = new ServletRegistrationBean<>(dispatcherServlet);
        servletServletRegistrationBean.addUrlMappings("*.do");
        return servletServletRegistrationBean;
    }
}
```



## 集成Druid

### 准备依赖

```xml
<!-- druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.10</version>
</dependency>

<!-- MySQL驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>

<!-- SpringBoot集成jdbc的依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

### 配置对象方式

```properties
#application.properties
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql:///rbac?useUnicode=true&characterEncoding=utf8&serverTimezone=GMT%2B8
jdbc.username=root
jdbc.password=admin
```

```java
@Bean(initMethod="init", destroyMethod="close")
@ConfigurationProperties("jdbc")
public DruidDataSource dataSource() {
    return new DruidDataSource();
}
```

### 自动配置方式

```properties
#application.properties
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql:///rbac?useUnicode=true&characterEncoding=utf8&serverTimezone=GMT%2B8
spring.datasource.username=root
spring.datasource.password=admin
```

SpringBoot自动配置连接池时会先检查容器中是否有连接池对象,没有则会根据特定的属性前缀来创建连接池对象交给Spring管理

### 集成原理

SpringBoot的自动配置中含有DataSourceAutoConfiguration配置对象,该配置对象含有个内部类,该内部类也是配置类,当容器中没有连接池对象时,就会Generic方式来创建连接池对象交给Spring管理,用到的属性值来源于DataSourceProperties对象



## 集成MyBatis

### 准备依赖

```xml
<!--mybatis集成到SpringBoot中的依赖-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.1</version>
</dependency>
```

### 配置属性

```properties
#application.properties
#之前在XML配置了哪些属性在这里就配置哪些属性,属性前缀mybatis
mybatis.configuration.lazy-loading-enabled=true
mybatis.configuration.lazy-load-trigger-methods=clone
mybatis.mapper-locations=classpath:cn/wolfcode/xxx/mapper/*Mapper.xml
mybatis.type-aliases-package=cn.wolfcode.xxx.domain
#连接池对象不用配置,会自动注入

#打印SQL日志
logging.level.包名=trace
```

Mapper接口扫描器只要在配置类上贴个注解@MapperScan(...)即可



## 事务管理

### 准备依赖

```xml
<!-- AOP织入的依赖 -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
</dependency>
```

### XML方式

采取配置类和XML混用的策略,在配置类上使用@ImportResource("classpath:spring-tx.xml")

### 注解方式

直接在**业务层实现类上或者其方法**上直接贴@Transactional注解即可
```properties
#SpringBoot默认优先选择CGLIB代理,如果需要改为优先使用JDK代理,需要做以下€配置
spring.aop.proxy-target-class=false #优先使用JDK代理
```



## 系统日志

### 为什么要用日志?

1. 比起System.out.println,日志框架可以把日志的输出和代码分离
2. 日志框架可以方便的定义日志的输出环境,控制台,文件,数据库
3. 日志框架可以方便的定义日志的输出格式和输出级别

### SpringBoot中的日志介绍:

1. Springboot默认已经开启日

   默认的日志格式为: 时间  日志级别  线程ID  线程名称  日志类  日志说明

2. Springboot的日志分为: 系统日志和应用日志

3. Springboot默认选择Logback作为日志框架,也能选择其他日志框架,但是没有必要

   common-logging / java-logging / log4j / log4j2 / logback / slf4j

### Logback日志的使用

Logback框架默认会自动加载classpath:logback.xml,作为框架的配置文件,在SpringBoot中使用时,还会额外的支持自动加载classpath:logback-spring.xml,在SpringBoot中推荐使用logback-spring.xml

样板文件:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
    scan:开启日志框架的热部署,默认值true表示开启
    scanPeriod:热部署的频率,默认值60 second
    debug:设置输出框架内部的日志,默认值false
-->
<configuration scan="true" scanPeriod="60 second" debug="false">
    <property name="appName" value="springboot demo" />
    <contextName>${appName}</contextName>
    
    <!-- appender:日志输出对象,配置不同的类拥有不同的功能
        ch.qos.logback.core.ConsoleAppender:日志输出到控制台        
    -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd-HH:mm:ss} %level [%thread]-%logger{35} >> %msg %n</pattern>
　　　　　</encoder>
　　 </appender>
    
    <!-- ch.qos.logback.core.FileAppender:日志输出到文件中
    <appender name="fileAppender" class="ch.qos.logback.core.FileAppender">
        <encoder>
            <pattern>%-4relative [%thread] %-5level %logger{35} - %msg %n</pattern>
    　　 </encoder>
        <append>true</append>
        <file>mylog.log</file>
　　 </appender>
    -->
    
    <!-- root是项目通用的logger,一般情况下都是使用root配置的日志输出
        level:按照级别输出日志,日志级别,级别越高,输出的内容越少
            trace > debug > info > warn > error
    -->
　　 <root level="info">
        <appender-ref ref="STDOUT" />
    </root>
    
    <!-- 自定义的logger,用于专门输出特定包中打印的日志
    <logger name="cn.wolfcode.springboot" level="trace">
    </logger>
	-->
</configuration>
```

参考日志格式:

- %d{yyyy-MM-dd-HH:mm:ss} %level [%thread]-%class:%line >> %msg %n


格式中的标识符组成:

- %logger{n}: 输出Logger对象类名,n代表长度
- %class{n}: 输出所在类名，
- %d{pattern}或者date{pattern}: 输出日志日期,格式同java
- %L/line: 日志所在行号
- %m/msg: 日志内容
- %method: 所在方法名称
- %p/level: 日志级别
- %thread: 所在线程名称

### 类中使用日志输出

在我们自定义的类中如果要使用日志框架来输出

```java
//1:在类中定义一个静态日志对象,这步可以使用lombok提供的@Slf4j注解来简化
private static final Logger log = LoggerFactory.getLogger(类.class);
//2:在需要输出日志的地方使用日志的输出方法
log.info(...);
log.error(...);
...
//输出日志中有变量可以使用{}作为占位符,如log.info("我是{}", "逍遥");
```
