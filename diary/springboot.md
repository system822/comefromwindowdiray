 ##12/23/2019 10:20:05 AM 
##Life has no limitations. It always opens up far more possibilities for the better.
**人生不设限，为更好，开启更多可能。**
###<center>SpringBoot框架</center>
####SpringBoot定义
	SpringBoot是pivotal公司，springcloud也是。SpringBoot其实就算是对Spring功能的封装，简化了xml配置定义，实现快速搭建和开发Spring应用。
####SpringBoot框架特点：
	1. SpringBoot完全采用了Java配置模式，替代了xml配置
	2. SpringBoot提供了自动配置机制，可以创建很多常用组件对象
	3. SpringBoot提供了内置Tomcat，通过main可以自动启动和发布
	4. SpringBoot提供了很多启动器（jar包集合）

		- spring-boot-starter-web (springwebmvc,ioc,aop,tomcat,restful等）
		- spring-boot-starter-jdbc(spring-jdbc,连接池)
		- ... ...

	5. SpringBoot可以采用jar包形式发布（因为其内置了Tomcat）
		(jar java程序；war web程序）


####[什么是restful（Representational State Transfer  表现层状态转化（ 资源定位及资源操作））](https://www.cnblogs.com/llsg/p/11358562.html)
	RESTFUL是一种网络应用程序的设计风格和开发方式，基于HTTP，可以使用XML格式定义或JSON格式定义。RESTFUL适用于移动互联网厂商作为业务使能接口的场景，实现第三方OTT调用移动网络资源的功能，动作类型为新增、变更、删除所调用资源。 

	特点：
		1. 规范化接口访问方式
		2. 资源标识唯一
		3. 状态的转换
		4. 所有信息包含请求中
		5. 无状态性
		6. 可实现请求缓冲
####Boot入门案例
	1. hello 的restful服务
		/hello-->SpringMVC-->"hello springboot"
		
		-导入jar包
			pom.xml中引入spring-boot-starter-web启动器
		-添加一个application.properties/.yml配置文件
			server.port=8888
		-添加配置controller组件(与原有spring使用相同)
			@Controller
			public class HelloController{
				
				@RequestMapping("/hell0")
				@ResponseBody
				public String say(){
					return "hello springboot";
				}
			}
		-添加一个启动类，用于启动SpringBoot程序
			SpringApplication.run();

			@SpringBootApplication
			public class RunBoot{
			
				public  static void main(String[] args){
					SpringApplication.run(RunBoot.class);
				}
			}


####Boot扩展

	1. 热启动
		修改了代码后，可以自动重启（jar包修改后可以手动重启）

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<version>2.0.5.RELEASE</version>
		</dependency>

	2. 一些新的标记
		@RestController = @Controller @ResponseBody
		@GetMapping = @RequestMapping(value="",method=RequestMethod.GET)
		其他也一样

	3. 打包发布过程
		<parent>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-parent</artifactId>
			<version>2.0.5.RELEASE</version>
		</parent>

		<build>
			<plugins>
				<plugin>
					<groupId>org.springframework.boot</groupId>
					<artifactId>spring-boot-maven-plugin</artifactId>
				</plugin>
			</plugins>
		</build>

		提示：Eclipse设置jdk目录，不要使用jre目录
			java -jar xxx.jar //启动运行jar包

>banner.txt 可改变启动logo

####SpringBoot启动过程
	1. SpringApplication.run主要执行流程
		- 调用SpringApplication对象（初始化）的run方法
		- run方法加载环境参数，打印启动logo，创建ApplicationContext对象，往context加载参数信息，加载组件扫描，自动配置组件，最后将context对象返回。

	2. SpringBootApplication标记
		SpringApplication.run方法启动中加载了@SpringBootApplication标记，该标记是一个复合标记，由以下三个功能标记组成，意思是开启了下面三个功能。
		
		- @SpringBootConfiguration
			开启bean定义功能，等于xml中的<bean id ="" class="">
		- @EnableAutoConfiguration 
			开启自动配置功能，属于SpringBoot特有功能。
		- @ComponentScan
			开启组件扫描，等价于xml中的<context:component-scan basepackage=">

####SpringBoot Bean定义
	@SpringBootConfiguration(springboot)/@Configuration(spring)标记等价于<bean>定义，常用于定义一些jar包的组件。
	
		@Bean("service") //将返回的对象加载Spring容器中（ApplicationContext),指定service做id
		@Primary //遇到烈性冲突时，默认使用该对象
		public  DeptService deptService(){
			return new DeptService();
		}
		@Bean //将返回的对象加载Spring容器中（ApplicationContext),默认方法名做id
		public DeptService deptService1(){
			return new DeptService();
		}

####SpringBoot 组件扫描

	@CommonentScan标记等价于<context:component-scan/>,常用于扫描自定义的一些组件。
	组件扫描：按照指定路径扫描带有@Controller，@Service，@Repository，@Component标记的组件
		
		//默认扫描当前包及子包中的带标记的组件
		@ComponentScan
		//扫描指定的dao和cn.xdl包下的组件
		@ComponentScan(basePackages={"dao","cn.xdl"})
		
		//扫描指定包下组件，符合Filter指定的DeptDao组件也纳入容器（不带标记也可以）
		@ComponenetScan(basePackages={"dao","cn.xdl"},
			includeFilters=@Filter
			(type=FilterType.ASSIGNABLE_TYPE,classes=DeptDao.class))
		//扫描指定包下组件，符合Filter指定的注解类型的组件也纳入容器
		@ComponentScan(basePackages={"dao","cn.xdl"},
			includeFilters=@Filter
			(type=FilterType.ANNOTATION,classes=MyDao.class))


####SpringBoot自动配置机制
	在Springboot提供了大量的自动配置组件，这些配置组件可以创建出以前常用的DispatcherServlet,HanlderMapper,DataSource,JdbcTemplate等。

	自动配置功能使用@EnableAutoConfiguration注解标记开启。

	在spring-boot-autoconfigurer.jar包中，有一个META-INF/spring.factries文件，里面定义了了大量的XxxxAutoConfiguration自动配置组件，这些组件其实就是使用了@Configuration+@Bean+@Primary等Bean定义技术封装的。

	一旦在启动类前追加@EnableAutoConfiguration标记，会自动导入AutoConfigurationImportSelector组件，该组件会自动会执行selectImports方法，方法里调用SpringFactoriesLoader组件加载解析META-INF/spring.factories文件自动配置组件定义信息。

			JdbcTemplateAutoConfiguration
			DataSourceAutoConfiguration
			DispatcherServletConfiguration
			RedisTemplateAutoConfiguration

	
>提示：并不是加了@EnableAutoConfiguration,所有spring,factories定义的自动配置组件都自动加载，配置组件中使用很多@ConditionalOnClasss等注解标记，用于判断是否满足一定条件，满足条件才会自动加载，否则不加在。  

####自动配置创建DataSource和JdbcTemplate示例

	1. 在pom.xml导入jdbc、mysql驱动包
		<!--mysql驱动 -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.0.5</version>
		</dependency>
		<!--spring boot starter jdbc -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jdbc</artifactId>
			<version>1.5.10.RELEASE</version>
		</dependency>

	2. 在application.properties定义数据库链接参数
		spring.datasource.username=root
		spring.datasource.password=123456
		spring.datasource.driverClassName=com.mysql.jdbc.Driver
		spring.datasource.url=jdbc:mysql://localhost:3306/xxx

	3. 定义启动类，追加@EnableAutoConfiguration或者@SpringBootApplication开启自动配置

		//@SpringBootApplication
		@EnableAutoConfiguration
		public class SpringRun {
			public static void main(String[] args) {
				ApplicationContext app = SpringApplication.run(SpringRun.class, args);
				DataSource bean = app.getBean("dataSource",DataSource.class);
				System.out.println(bean);
				JdbcTemplate bean2 = app.getBean("jdbcTemplate",JdbcTemplate.class);
				System.out.println(bean2);
			}

		}

####DataSourceAutoConfiguration内部机制

	DataSourceAutoConfiguration可以自动创建DataSource连接池对象，默认一次创建Hikari、Tomcat、DBCP2。

	引入dbcp2,由于spring-boot-starter-jdbc默认会将HikariCP连接池引入，因此默认创建还是Hikari链接池。可以使用maven的jar排除操作。
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jdbc</artifactId>
			<version>2.0.6.RELEASE</version>
			<exclusions>
				<exclusion>
					<groupId>com.zaxxer</groupId>
					<artifactId>HikariCP</artifactId>
				</exclusion>
			</exclusions>
		</dependency>

	也可以使用spring.datasource.type参数指定其他类型的链接池组件

		spring.datasource.type=com.alibaba.druid.pool.DruidDataSource

####参数注入
	参数注入：将application.properties配置文件中的参数注入给对象属性。
	涉及以下几个注解标记

		1. @EnableConfigurationProperties
			表示开启参数注入功能，使用@EnableAutoConfiguration会包含参数注入功能。
		2. @ConfigurationProperties
			表示参数注入，卸载类定以前或#Bean方法前
				
				@Component
				@ConfigurationProperties(prefix="spring.datasource")
				public class DbParam1{
					private String username;//注入prefix.username属性
					private String password;//注入prefix.password属性
				}
			
				@Bean
				@ConfigurationProperties//注入与DbParams属性名相同的参数值
				public DbParams dbParams(){
					return new DbParams();
				}

>@Value($(newname))
>pushd d:\  跳盘

####笔试题
#####1. 什么是spring boot***
	它是对spring框架的一个再封装，简化了配置的文件，能够快速的构建web应用，它内部有Tomcat，不需打包部署，实现了自动配置机制，可以直接用。
####2. Spring boot的配置文件有几种？有什么区别
	.properties和.yml  区别主要是书写格式的不同
	.yml不支持@PropertySource注解导入配置
####3. spring boot核心注解是哪个，它由那几个注解构成？
	@SpringBootApplication

	@SpringBootConfiguration:组合@Configuration功能，实现配置文件的功能
	
	@EnableAutoConfiguration:打开自动配置的功能，也可以关闭某个自动配置的功能@SpringBootApplication(exclude=={*.class})
	
	@ComponentScan: Spring 组件扫描
####4. springboot 可以兼容老spring项目么？
	可以，使用@importResource注解导入老Spring的配置文件
####5. SpringBoot有哪些优点
	-快速创建独立运行的spring项目与主流框架集成 
	-使用嵌入式的servlet容器，应用无需打包成war包 
	-starters自动依赖与版本控制 
	-大量的自动配置，简化开发，也可修改默认值 
	-准生产环境的运行应用监控 
	-与云计算的天然集成
	-使用JavaConfig有助于避免使用XML。
	-避免大量的Maven导入和各种版本冲突。

###springboot测试类怎么写
	需要在测试类前加
		@RunWith(SpringRunner.class)
		@SpringBootTest
	or
		@RunWith(SpringRunner.class)
		@SpringBootTest(classes = RunBoot.class)