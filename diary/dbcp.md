###12/7/2019 10:33:40 AM 
###Love everyone, every leaf, every ray of light.
**爱所有人，爱每一片叶子，爱每一缕光。**
##<center>基于java定义数据库链接池，提供获取连接和释放连接功能</center>
####1. [了解连接池概念和优点](https://blog.csdn.net/qq_38129062/article/details/87736729)

		基本概念：
			为应用程序创建固定数量的链接对象，保存在池中进行复用。每次访问时从池中获取已存在，使用完毕后，返回池中。
		优点：
			JDBC作为一种数据库访问技术，具有简单易用的优点。
			效率高，使用连接池和不适用连接池在效率上的差别，大约是5-10倍
		缺点：
			网络IO较多
			数据库的负载较高
			响应时间较长及QPS较低
			应用频繁的创建连接和关闭连接，导致临时对象较多，GC频繁
			在关闭连接后，会出现大量TIME_WAIT 的TCP状态（在2个MSL之后关闭）
		基本原理：
			数据库连接池的核心是连接复用，减少连接的建立次数
			数据库链接池的基本思想就是建立一个缓冲池。与缓冲池中放入一定数量的链接，当需要建立数据库的连接时，只需要从缓冲池中取出一个，使用完毕后再放回去即可。
####不适用数据库的连接池
	TCP建立连接的三次握手
	MySQL认证的三次握手
	真正的SQL执行
	MySQL的关闭
	TCP的四次握手关闭
####为什么用连接池？
	数据库连接是非常占用资源的，尤其是在高并发的情况下，如果每次都去建立数据库连接就会有性能问题，也会影响一个应用程序的延展性，针对这个问题，连接池出现了，连接池就是为了解决这个问题的。
####流程图
![](./img/dbcp1.png)	

####数据库链接池的实现分析
	1. 并发问题
		为了使链接管理服务具有更大的通用性，必须考虑多线程环境，即并发问题。我们使用synchronized关键字即可确保线程的同步。示例代码： public synchronized Connection getConnection()
	2. 多数据库服务器和多用户
		对于大型的企业级应用，常常需要同时链接不同的数据库。策略：设计一个符合单例模式的连接池管理类，在连接池管理类的实例被创建时读取一个资源文件，其中资源文件中存放着多个数据库的链接信息。根据资源文件提供的信息，创建多个连接池类的实例，每一个实例都是一个特定数据库的链接池。链接池管理实例为每个链接池实例取一个名字，通过不同的名字来管理不同的链接池。
	3. 事务处理
		可以通过设置Connection的AutoCommit属性为false,然后显示的调用commit或rollback来实现。但要高效的进行Connection复用，就必须提供相应的事务支持机制。可采用每一个事务独占一个链接来实现，这种方法大大降低事务管理的复杂性。
	4. 连接池的分配和释放
		每当用户请求一个连接时，系统首先检查空闲池内有没有空闲连接。如果有就把建立时间最长的那个连接分配给他；如果没有则检查当前所开的连接池是否达到连接池所允许的最大连接数。如果没有达到，就新建一个连接；如果已经达到，就等待一定的时间。如果在等待的时间内有连接被释放出来，就可以把这个连接分配给等待的用户；如果等待时间超过预定时间，则返回空值；系统对已经分配出去正在使用的连接只做计数，当时用完后再返回给空闲池；对于空闲连接的状态，可开辟专门的线程定时检测，这样会花费一定的系统开销，但可以保证较快的相应速度；也可采取不开辟专门线程，只是在分配前检测的方法。
	5. 连接池的配置和维护
		连接池中到底应该放置多少链接，才能使系统的性能最佳？系统可以采取设置最小连接数和最大连接数来控制池中的链接。最小连接数是系统启动时连接池所创建的连接数，最大连接数是连接池中允许连接的最大数目。

####为什么建立数据库是相当耗费资源和时间的
	首先建立TCP链接；然后TCP协议三次握手的发送与相应；客户端的账户验证，服务器返回确认；用户验证后，需要传输相关链接变量 如 是否自动提交事务等，会有多次的数据的交互，然后才能执行真正的数据查询和更新等操作
####[连接池组件类型](https://www.open-open.com/project/tag/shuju-lianjiechi.html)
	1. Druid
		组成分为三部分
			1. <A class=external-link href="/misc/goto?guid=4958189228120361075" rel=nofollow>DruidDriver</A> 代理Driver，能够提供基于Filter－Chain模式的插件体系。
			2. <A class=external-link href="/misc/goto?guid=4958189228854399976" rel=nofollow>DruidDataSource</A> 高效可管理的数据库连接池。
			3. sqlParser

		功能
			1. 可以监控数据控的访问性能，能够详细提供SQL的执行性能
			2. 替换DBCP和C3P0.Druid提供了一个高效，功能强大，可扩展性好的数据库链接。
			3. 数据库密码加密。（直接将数据库密码写在配置文件容易导致安全性问题）
			4. sql执行日志，Druid提供了不同的LogFilter,能够支持Common-Logging、Log4j和JdkLog,可按需要选择对应的LogFilter
			5. 扩展JDBC，可以通过Driuid提供的Filter机制，方便的编写JDBC层的扩展插件。
[链接](https://github.com/alibaba/druid/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)
	2. BoneCP
		BoneCP是一个快速，开源的数据库链接池。可让你的应用程序能更快速的访问数据库。比C3P0/DBCP连接池快25倍。
	
	3. MiniConnectionPoolManager
		MiniConnectionPoolManager是一个轻量级JDBC数据库连接池。它只需要Java1.5（或更高）并且没有依赖第三方包。

	4. SmartPool
		SmartPool是一个连接池组件，它模仿应用服务器对象池的特性。SmartPool能够解决一些临界问题如连接泄漏(connection leaks),连接阻塞,打开的JDBC对象如Statements,PreparedStatements等. SmartPool的特性包括支持多个pools,自动关闭相关联的JDBC对象, 在所设定time-outs之后察觉连接泄漏,追踪连接使用情况, 强制启用最近最少用到的连接,把SmartPool"包装"成现存的一个pool等。

	5. Primrose
		Primrose是一个Java开发的数据库连接池。当前支持的容器包括Tomcat4&5,Resin3与JBoss3.它同样也有一个独立的版本可以在应用程序中使用而不必运行在容器中。Primrose通过一个web接口来控制SQL处理的追踪，配置，动态池管理。在重负荷的情况下可进行连接请求队列处理。

	6. XAPool
		XAPool是一个XA数据库连接池。它实现了javax.sql.XADataSource并提供了连接池工具。
	
	7. DBPool
		DBPool是一个高效的易配置的数据库连接池。它除了支持连接池应用的功能之外，还包括了对象池使你能够开发一个满足自己需求的数据库连接池。
	
	8. DDConnectionBroker
		DDConnectionBroker是一个简单，轻量级的数据库连接池。

	9. Proxool
		这是一个Java SQL Driver驱动程序，提供了对你选择的其它类型的驱动程序的连接池封装。可以非常简单的移植到现存的代码中。完全可配置。快速，成熟，健壮。可以透明地为你现存的JDBC驱动程序增加连接池功能。

	10. Jakarta DBCP
		DBCP是一个依赖Jakarta commons-pool对象池机制的数据库连接池。DBCP可以直接的在应用程序使用。
	
	11. C3P0
		C3P0是一个开放源代码的JDBC连接池，他在lib目录中与Hibernate一起发布，包括了实现jdbc3和jdbc2扩展规范说明的Connection和Statement池的DataSources对象。
			
####总结时刻
	综上所述，这几种开源连接池各有优劣，推荐使用c3p0，经检验这种连接池性能稳定，承压能力强。而proxool尽管有明显的性能问题，但由于它具备监控功能，因此建议在开发测试时使用，有助于确定是否有连接没有被关掉，可以排除一些代码的性能问题。

###12/8/2019 6:31:02 PM 
####Jakarta DBCP的使用
####自定义连接池
	步骤：
		1. 导入驱动包
		2. 配置properites文件
			mysql
			driverClassName=com.mysql.jdbc.Driver
			url=jdbc:mysql://localhost:3306/examdb34
			username=root
			password=123456

			oracle
			url=jdbc:oracle:thin:@localhost:1521:xe
			driverClassName=oracle.jdbc.OracleDriver
			username=system
			password=123456

			。。。
		3. 写一个提供链接，获取配置文件数据的类
			package cn.xdl.util;
			import javax.sql.DataSource;
			import java.io.PrintWriter;
			import java.sql.*;
			import java.util.LinkedList;
			import java.util.List;
			import java.util.logging.Logger;
			
			public class ConnectionPoolUtil implements DataSource {
		    //初始化连接数目
		    private static int init_quantity = ProvideConnection.getInit();
		    //定义一个计数器，用来记录创建的连接数
		    private static int conn_count = init_quantity;
		    //创建一个容器来存储connection对象
		    private static List<Connection> list = new LinkedList<>();
		    //对连接池进行初始化
		    static{
		        for(int i=0;i<init_quantity;i++){
		            Connection connection = ProvideConnection.getConnection();
		            //System.out.println(connection);
		            list.add(connection);
		        }
		    }
		
		    /**
		     *
		     * @return 返回null代表连接池上线了
		     * @throws SQLException
		     */
		    @Override
		    public  Connection getConnection() throws SQLException {
		        synchronized(this) {
		            Connection connection = null;
		            int maxActive = ProvideConnection.getData();
		            if (list.size() == 0) {
		                if (conn_count < maxActive) {
		                        connection = ProvideConnection.getConnection();
		                        conn_count++;
		                        System.out.print(Thread.currentThread().getName()+"   ");
		                        System.out.print(conn_count+"   ");
		                        System.out.println(connection);
		                        list.add(connection);
		                } else {
		                    try {
		                        System.out.println("连接池条数不够了休息了");
		                        this.wait();
		                    } catch (Exception e) {
		                        e.printStackTrace();
		                    }
		                }
		            }
		            System.out.print(Thread.currentThread().getName()+"\t查到了\t");
		            connection = list.remove(0);
		            return connection;
		        }
		    }
		
		    public synchronized void testJdbc(){
		        try {
		            Connection connection = getConnection();
		            PreparedStatement ps = connection.prepareStatement("select * from et_classnum");
		            ResultSet resultSet = ps.executeQuery();
		            while(resultSet.next()){
		                int classID = resultSet.getInt("classID");
		                String className = resultSet.getString("className");
		                System.out.print("\t班级id"+classID+"  名字"+className);
		            }
		            System.out.println(" ");
		            ConnectionPoolUtil cc = new ConnectionPoolUtil();
		            cc.backFlow(connection);
		        } catch (SQLException e) {
		            e.printStackTrace();
		        }
		    }
		
		    /**
		     *提供一个将流放回链接池的方法
		     */
		    public synchronized void backFlow(Connection connection){
		        System.out.println(Thread.currentThread().getName()+"拿的链接放回去了");
		        list.add(connection);
		        this.notify();
		    }
		
		    public void getSize(){
		        System.out.println("已创建连接数\t"+conn_count);
		    }
		    @Override
		    public Connection getConnection(String username, String password) throws SQLException {
		        return null;
		    }
		
		    @Override
		    public <T> T unwrap(Class<T> iface) throws SQLException {
		        return null;
		    }
		
		    @Override
		    public boolean isWrapperFor(Class<?> iface) throws SQLException {
		        return false;
		    }
		
		    @Override
		    public PrintWriter getLogWriter() throws SQLException {
		        return null;
		    }
		
		    @Override
		    public void setLogWriter(PrintWriter out) throws SQLException {
		
		    }
		
		    @Override
		    public void setLoginTimeout(int seconds) throws SQLException {
		
		    }
		
		    @Override
		    public int getLoginTimeout() throws SQLException {
		        return 0;
		    }
		
		    @Override
		    public Logger getParentLogger() throws SQLFeatureNotSupportedException {
		        return null;
		    }
			}
		4. 写一个连接池
			package cn.xdl.util;

			import javax.sql.DataSource;
			import java.io.PrintWriter;
			import java.sql.*;
			import java.util.LinkedList;
			import java.util.List;
			import java.util.logging.Logger;
			
			public class ConnectionPoolUtil implements DataSource {
			    //初始化连接数目
		    private static int init_quantity = ProvideConnection.getInit();
		    //定义一个计数器，用来记录创建的连接数
		    private static int conn_count = init_quantity;
		    //创建一个容器来存储connection对象
		    private static List<Connection> list = new LinkedList<>();
		    //对连接池进行初始化
		    static{
		        for(int i=0;i<init_quantity;i++){
		            Connection connection = ProvideConnection.getConnection();
		            //System.out.println(connection);
		            list.add(connection);
		        }
		    }
		
		    /**
		     *
		     * @return 返回null代表连接池上线了
		     * @throws SQLException
		     */
		    @Override
		    public  Connection getConnection() throws SQLException {
		        synchronized(this) {
		            Connection connection = null;
		            int maxActive = ProvideConnection.getData();
		            if (list.size() == 0) {
		                if (conn_count < maxActive) {
		                        connection = ProvideConnection.getConnection();
		                        conn_count++;
		                        System.out.print(Thread.currentThread().getName()+"   ");
		                        System.out.print(conn_count+"   ");
		                        System.out.println(connection);
		                        list.add(connection);
		                } else {
		                    try {
		                        System.out.println("连接池条数不够了休息了");
		                        this.wait();
		                    } catch (Exception e) {
		                        e.printStackTrace();
		                    }
		                }
		            }
		            System.out.print(Thread.currentThread().getName()+"\t查到了\t");
		            connection = list.remove(0);
		            return connection;
		        }
		    }
		
		    public synchronized void testJdbc(){
		        try {
		            Connection connection = getConnection();
		            PreparedStatement ps = connection.prepareStatement("select * from et_classnum");
		            ResultSet resultSet = ps.executeQuery();
		            while(resultSet.next()){
		                int classID = resultSet.getInt("classID");
		                String className = resultSet.getString("className");
		                System.out.print("\t班级id"+classID+"  名字"+className);
		            }
		            System.out.println(" ");
		            ConnectionPoolUtil cc = new ConnectionPoolUtil();
		            cc.backFlow(connection);
		        } catch (SQLException e) {
		            e.printStackTrace();
		        }
		    }
		
		    /**
		     *提供一个将流放回链接池的方法
		     */
		    public synchronized void backFlow(Connection connection){
		        System.out.println(Thread.currentThread().getName()+"拿的链接放回去了");
		        list.add(connection);
		        this.notify();
		    }
		
		    public void getSize(){
		        System.out.println("已创建连接数\t"+conn_count);
		    }
		    @Override
		    public Connection getConnection(String username, String password) throws SQLException {
		        return null;
		    }
		
		    @Override
		    public <T> T unwrap(Class<T> iface) throws SQLException {
		        return null;
		    }
		
		    @Override
		    public boolean isWrapperFor(Class<?> iface) throws SQLException {
		        return false;
		    }
		
		    @Override
		    public PrintWriter getLogWriter() throws SQLException {
		        return null;
		    }
		
		    @Override
		    public void setLogWriter(PrintWriter out) throws SQLException {
		
		    }
		
		    @Override
		    public void setLoginTimeout(int seconds) throws SQLException {
		
		    }
		
		    @Override
		    public int getLoginTimeout() throws SQLException {
		        return 0;
		    }
		
		    @Override
		    public Logger getParentLogger() throws SQLFeatureNotSupportedException {
		        return null;
		    }
			}

		5. 测试类
			public class ThreadTest implements Runnable{
			    private static ConnectionPoolUtil cc = new ConnectionPoolUtil();
			    private static String thread;
			    @Override
			    public void run() {
			        cc.testJdbc();
			        //cc.getSize();
			    }
			    public static void main(String[] args) {
			        ThreadTest tt = new ThreadTest();
			        for (int i = 0; i < 13; i++) {
			            thread = thread+i;
			            Thread thread = new Thread(tt);
			            thread.start();
			        }
			    }
			}



