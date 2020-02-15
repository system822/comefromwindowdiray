·##12/30/2019 2:42:07 PM 
##Some people are worth melting for.
**有些人值得我为他融化。**
###<center>springbootJpa</center>
####jpa
	定义：
		JPA 是一个基于O/R映射的标准规范（目前最新版本是JPA 2.1 ）。JPA是Java Persistence API的简称，中文名Java持久层API，是JDK 5.0注解或XML描述对象－关系表的映射关系，并将运行期的实体对象持久化到数据库中。
	出现的原因：
		简化现有Java EE和Java SE应用的对象持久化的开发工作；
		Sun希望整合对ORM技术，实现持久化领域的统一。
	提供的技术：
		ORM映射元数据：JPA支持XML和JDK 5.0注解两种元数据的形式，元数据描述对象和表之间的映射关系，框架据此将实体对象持久化到数据库表中；
		JPA 的API：定义规范，以操作实体对象，执行CRUD操作，框架在后台替我们完成所有的事情，开发者从繁琐的JDBC和SQL代码中解脱出来。
		查询语言：通过面向对象而非面向数据库的查询语言查询数据，避免程序的SQL语句紧密耦合。定义JPQL和Criteria两种查询方式。
####SpringBoot+Jpa
	1. 创建连接池
		数据库驱动
		jdbc链接池
		（热启动）
		（版本约束）
		（jar包插件）
	2. 导入spring-boot-starter-data-jpa
	3. 根据表定义实体类
	
		@Entity
		@Table(name = "dept")
		public class Dept implements Serializable {
		    private static final long serialVersionUID = -528319497118423466L;
	    @Id
	    @Column(name = "deptno")
	    private int deptno;
	    @Column(name = "dname")
	    private String dname;
	    @Column(name = "loc")
	    private String loc;
	
	4. 定义Dao接口
		public interface DeptDao extends JpaRepository<Dept, Integer> {
		 }

####排序和分页
	排序：
	 Sort sort = new Sort(Sort.Direction.DESC,"deptno");
     List<Dept> list = bean.findAll(sort);
        list.forEach(item->{
            System.out.println(item.getDeptno()+" "+item.getDname()+"  "+item.getLoc());
        });

	分页排序：
		Sort sort = new Sort(Sort.Direction.DESC,"deptno");
		Pageable pageable = PageRequest.of(1,2,sort);

        Page<Dept> page = bean.findAll(pageable);

        List<Dept> list = page.getContent();
	        list.forEach(one->{
	            System.out.println(one.getDeptno()+" "+one.getDname()+" "+one.getLoc());
	        });
        System.out.print("总页数"+page.getTotalPages());
        System.out.println("当前页"+(page.getNumber()+1));

####jpa中sql的三种语法
	1. 第一种方式
		//执行原始sql
	    @Query(nativeQuery = true,value = "select * from dept where dname like :name")
	    public List<Dept> findLikeName(@Param("name")String dname);
	
		增删改时需要加：
		@Modifying
    	@Transactional	

		例子：
			  /**删除*/
		    @Modifying
		    @Transactional
		    @Query(value ="delete from Dept where deptno =?1",nativeQuery = true)
		    int deleteDeptByDeptno(@Param("deptno") Integer deptno);
		    /**增加*/
		    @Modifying
		    @Transactional
		    @Query(value ="insert into Dept(dname,loc) values (?1,?2)",nativeQuery = true)
		    int addDept(@Param("dname")String dname,@Param("loc")String loc);
		    /**修改*/
		    @Modifying
		    @Transactional
		    @Query(value ="update Dept set deptno=?1 ,dname=?2 ,loc=?3 where deptno=?4")
		    int updateDept(@Param("deptno")int deptno,@Param("dname")String dname,@Param("loc")String loc,@Param("deptno")int ndeptno);

	2. 第二种方式
		//指定面向对象的Jpql(将sql中的表名和字段名，使用类型名和属性名替代；不支持select *(可去掉select * 来代替))
	    @Query("from Dept where dname like :name")
	    public List<Dept> findLikeName1(@Param("name")String dname);

	3. 第三种方式
		严格按照语法格式命名
			findByDnameLike(String dname);

####SpringBoot MVC应用
#####开发restful服务*
	Http请求-->MVC-->返回JSON结果
	适合前后分离架构应用，前端可以开发出多个不同的界面，比如浏览器、手机app、小程序等。后端统一调用restful服务处理。
	
	1. 创建maven project（jar类型）
	2. 导入pom.xml jar包，spring-boot-starter-web
		包含了springmvc,tomcat,restful支持的jar包
	3. application.properties定义的server.port
	4. 定义启动类，追加@SpringBootApplication
	5. 定义Controller,Service,Dao组件，采用注入的方式建立关系应用

#####开发浏览器HTML应用（JSP)
	Http请求-->MVC-->JSP生成的HTML结果
	适合只开发浏览器应用

	1. 创建maven project
	2. 导入pom.xml jar包，spring-boot-starter-web,tomcat-embed-jasper包
	3. application.properties定义参数
		server.port=8888
		spring.mvc.view.prefix=/
		spring.mvc.view.suffix=.jsp
	4. 定义启动类，使用@SpringBootApplication
	5. 在src/main目录下创建webapp,添加 .jsp文件
	6. 定义Controller组件
		@Controller
		public class JspController {
		    @RequestMapping("/tojsp")
		    public ModelAndView toJsp(){
		        ModelAndView mav = new ModelAndView();
		        mav.setViewName("hello");
		        return mav;
		    }
		}

		
#####开发浏览器HTML应用*（Thymeleaf模板技术）
	Http请求-->MVC-->Thymeleaf模板生成HTML结果
	适合只开开发浏览器应用。

	Thymeleaf技术属于模板技术，是对jsp技术的替代。
	模板和jsp区别：
	1. 运行机制不同：
		- 模板文件+模型数据=响应输出
		- Jsp-->Servlet-->编译-->响应输出
	2. 模板技术比jsp简单，易用
		- 模板支持模板表达式语言
		- Jsp支持El,JSTl,<%%>,指令等
	
	Themeleaf模板技术，是有模板文件I(.Html)+模型数据（th表达式）=产出html输出

	创建流程：
		1. 创建maven project
		2. 导入pom.xml jar包，spring-boot-starter-web,spring-boot-starter-thymeleaf包
		3. application.properties定义参数
				server.port=8888
		4. 定义启动类，使用@SpringBootApplication
		5. 在资源包下创建templataes,添加.html文件
			<!DOCTYPE html>
			<html xmlns:th="http://www.thymeleaf.org">
			<head>
			    <meta charset="UTF-8">
			    <title>Title</title>
			</head>
			<body>
			    <h1>眼睛好疲惫</h1>
			    <h2 th:text="${lala}"></h2>
			    <ul>
			        <li th:each="n:${list}" th:text="${n}">x</li>
			    </ul>
			</body>
			</html>
		6. 定义controller


	常用的Thymeleaf表达式语言
	提示：使用前要在<html>元素里定义xmlms="http://www.thymeleaf.org"导入

		-th:text	//将模型取出放入元素中间文本显示
		-th:each	//循环
		-th:if		//判断
		-th:value	//设置元素的value属性值
		-th:href	//动态拼接ur或参数
		-[[${xxx]]	//原地显示模型数据

####Spring Boot Mvc静态资源存放和访问
	在SpringBoot中提供几个约定的静态资源存放路径
	
	-resources/public(优先级最低）
	-resources/static
	-resources/resources
	-resources/META-INF/resources(优先级最高)

	自定义静态资源路径，需要定义配置类

		@Configuration
		public class MyStaticConfiguration implementsWebMvcConfigurer{
			public void addResourceHandlers(ResourceHandlerRegistry registry){
				registry.addResourceHandler("/**").addResourceLocations("
						classpath:/mybatis/",
						classpath:/resources/",
						classpath:/static/",
						classpath:/public/"
						);
			}
		}

####jsp与thymeleaf的优缺点

	jsp:
		优点： 
			1、功能强大，可以写java代码 
			2、支持jsp标签（jsp tag） 
			3、支持表达式语言（el） 
			4、官方标准，用户群广，丰富的第三方jsp标签库 
			5、性能良好。jsp编译成class文件执行，有很好的性能表现 
		缺点： 
			（1）Java的一些优势正是它致命的问题所在。正是由于为了跨平台的功能，为了极度的伸缩能力，所以极大的增加了产品的复杂性。
			（2） Java的运行速度是用class常驻内存来完成的，所以它在一些情况下所使用的内存比起用户数量来说确实是“最低性能价格比”了。从另一方面，它还需要硬盘空间来储存一系列的.java文件和.class文件，以及对应的版本文件。
			（3）在调试JSP代码时，如果程序出错，JSP服务器会返回出错信息，并在浏览器中显示。这时，由于JSP是先被转换成Servlet后再运行的，所以，浏览器中所显示的代码出错的行数并不是JSP源代码的行数，而是指转换后的Servlet程序代码的行数。这给调试代码带来一定困难。所以，在排除错误时，可以采取分段排除的方法（在可能出错的代码前后输出一些字符串，用字符串是否被输出来确定代码段从哪里开始出错），逐步缩小出错代码段的范围，最终确定错误代码的位置。

	thymeleaf:
		什么是thymeleaf:
			java模板引擎，能够处理HTML，XML,JavaScript，CSS甚至是纯文本。类似于JSP和Freemarker
			作用就是把各个用户的公用的东西（页面）做一个提取，然后再根据不同的数据对页面进行渲染
			自然模板，原型即页面。
			语法优雅易懂，支持这两种OGNL，SpringEL编码方式
			遵循Web的标准，支持HTML5
		thymeleaf优点：
			静态html嵌入标签属性，浏览器可以直接打开模板文件，便于前后端联调。

			1.Thymeleaf 在有网络和无网络的环境下皆可运行，即它可以让美工在浏览器查看页面的静态效果，也可以让程序员在服务器查看带数据的动态页面效果。这是由于它支持 html 原型，然后在 html 标签里增加额外的属性来达到模板+数据的展示方式。浏览器解释 html 时会忽略未定义的标签属性，所以 thymeleaf 的模板可以静态地运行；当有数据返回到页面时，Thymeleaf 标签会动态地替换掉静态内容，使页面动态显示。

   			2.Thymeleaf 开箱即用的特性。它提供标准和spring标准两种方言，可以直接套用模板实现JSTL、 OGNL表达式效果，避免每天套模板、该jstl、改标签的困扰。同时开发人员也可以扩展和创建自定义的方言。

    		3. Thymeleaf 提供spring标准方言和一个与 SpringMVC 完美集成的可选模块，可以快速的实现表单绑定、属性编辑器、国际化等功能。

		thymeleaf缺点：
			模板必须符合xml规范，js脚本必须加入/