##12/26/2019 9:12:43 AM 
###There are some natures too lofty to bend.
**有些天性太过高贵，不能屈服。**
##<center>springboot2</center>
###连接池
	DataSource 接口类型
		dbcp: BasicDataSource
		druid: DruidDataSource
		hikari,3p0,proxool等
	优点：控制连接数；链接重用避免频繁创建和释放连接
####springboot连接池的使用
	1. 在pom.xml引入spring-boot-starter-jdbc，驱动包
	2. 在application。properties或yml配置数据库链接参数
		spring.datasource.username=xx
		spring.datasource.password=xx
		srping.datasource.url=xx
		spring.datasource.driverClassName=xx
		spring.datasource.type=xx
	3. 定义启动类，开启自动配置，组件扫描，Bean组件定义等功能
		@SpringBootApplication

###方案一：SpringBoot+JdbcTemplate
	1. 创建连接池
	2. 根据表定义实体类
	3. 定义dao接口
	4. 定义dao实现类，扫描dao并注入JdbcTemplate

	@Autowired
    private JdbcTemplate template;

    @Override
    public Dept findById(int no) {
        String sql = "select deptno,dnaem,loc from dept where deptno=?";
        Object[] args={no};
        RowMapper<Dept> rowMapper = new BeanPropertyRowMapper<>();
        Dept dept = template.queryForObject(sql, args, rowMapper);
        return dept;
    }

    @Override
    public List<Dept> findPage(int page, int size) {
        String sql = "select deptno,dname,loc from dept limit ?,?";
        Object[] args ={(page-1)*size,size};
        RowMapper<Dept> rowMapper = new BeanPropertyRowMapper<>(Dept.class);
        List<Dept> depts = template.query(sql,args,rowMapper);
        return depts;
    }

    @Override
    public void save(Dept dept) {
        String sql ="insert into dept(dname,loc) values(?,?)";
        Object[] args = {dept.getDname(),dept.getLoc()};
        template.update(sql,args);
    }
###方案二：SpringBoot+MyBatis
	1. 创建连接池
	2. 导入mybatis-spring-boot-starter
	3. 根据表定义实体类（与mybatis使用相同）
	4. 定义sql语句（与mybatis使用相同）
	5. 定义Mapper接口（与mybatis使用相同）
	6. 配置mybatis元素

		- 在application.properties配置sql文件
			mybatis.mapperLocations=classpath:sql/*.xml
		- 在启动类配置Mapper接口
			@SpringBootApplication
			@MapperScan(basePackages="cn.xdl.dao")
			public class RunBoot{
			}
####Mybatis注解sql
	1. 创建连接池
	2. 导入mybatis-spring-boot-starter
	3. 根据表定义实体类（与mybatis使用相同）
	4. 使用注解sql
	5. 配置mybatis元素
			- 在启动类配置Mapper接口
			@SpringBootApplication
			@MapperScan(basePackages="cn.xdl.dao")
			public class RunBoot{
			}

>@RequestParam(name="",required=true,defaultValue="")
>分页插件的使用 导入pageHelper依赖 1.2.3以上
>需加在总查询之上，且只影响向下一行
>PageHelper.startpage(int pageNumber,int pageSize)

>相对路径 a/b 绝对路径 /a/b  访问流
###方案三：SpringBoot+JPA