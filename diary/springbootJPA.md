##12/30/2019 2:42:07 PM 
##Some people are worth melting for.
**有些人值得我为他融化。**
###<center>springbootJpa</center>
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

####sql的三种语法
	1. 第一种方式
		//执行原始sql
	    @Query(nativeQuery = true,value = "select * from dept where dname like :name")
	    public List<Dept> findLikeName(@Param("name")String dname);

	2. 第二种方式
		//指定面向对象的Jpql(将sql中的表名和字段名，使用类型名和属性名替代；不支持select *(可去掉select * 来代替))
	    @Query("from Dept where dname like :name")
	    public List<Dept> findLikeName1(@Param("name")String dname);

	3. 第三种方式
		严格按照语法格式命名
			findByDnameLike(String dname);