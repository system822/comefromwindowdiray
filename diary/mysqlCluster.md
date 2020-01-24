##1/10/2020 7:53:12 PM 
##Life is never static.
**人生充满变数。**
##搭建MySql数据库集群环境
####<center>常见网站部署</center>
![](./img/usuallywebdeplay.png)
	ip负载：ip负载均衡技术是在负载调度器的实现技术中最高效的，在已有的ip负载均衡技术中主要有通过网络地址转换，将一组服务器构建成一个高性能的、高可用的虚拟服务器，我们称之为VS/NAT技术。

![](./img/msr.png)

	mysql主从复制
		一主一从
		主主复制
		一主多从 --扩展系统读取的性能，因为读是在库读取的；
		多主一从 --5.7开始支持
		联级复制

####用途及条件
	mysql主从复制用途
		实时灾备，用于故障处理
		读写分离，提供查询服务
		备份，避免影响业务（备可用性和容错行）
		负载平衡
	主从部署必要条件
		主库开启binlog日志（设置log-bin参数）
		主从server-id不同
		从库服务器能连通主库
####主从原理
![](./img/master-slave-theory.png)


###搭建mysql集群环境
	1. 编辑配置文件
		/etc/my.cnf 在[mysqld]下面进行配置 (主库) 
			唯一表示
				server-id=200  
			开启二进制文件
				log_bin=/var/log/mysql/mysql-bin.log
				缺少文件需要自己去创建
			开启读写状态
				read-only=0
				（只读 read-only=1）
			操作的库
				binlog-do-db=xxx
			不需要记录二进制文件的库
				binlog-ignore-db=mysql
			重启
				service mysqld restart

		从库的配置
			server-id=221
			log_bin=/var/log/mysql/mysql-bin.log
			service mysqld restart
>可能碰到的问题 没有创建对应文件夹  没有授权

	2. 登录主服务器，执行以下命令
		
		提示密码安全策略问题
			set global validate_password_policy=0;
		登录mysql
			mysql -uroot -p

		创建一个用户
			username：你将创建的用户名
			host：指定该用户在哪个主机上可以登陆，如果是本地用户可用localhost，如果想让该用户可以从任意远程主机登陆，可以使用通配符%
			password：该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器
			例子：
			CREATE USER 'dog'@'localhost' IDENTIFIED BY '123456';
			CREATE USER 'pig'@'192.168.1.101_' IDENDIFIED BY '123456';
			CREATE USER 'pig'@'%' IDENTIFIED BY '123456';
			CREATE USER 'pig'@'%' IDENTIFIED BY '';
			CREATE USER 'pig'@'%';
		授权
			privileges：用户的操作权限，如SELECT，INSERT，UPDATE等，如果要授予所的权限则使用ALL
			databasename：数据库名
			tablename：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用*表示，如*.*
			例子:
			GRANT SELECT, INSERT ON test.user TO 'pig'@'%';
			GRANT ALL ON *.* TO 'pig'@'%';
			GRANT ALL ON maindataplus.* TO 'pig'@'%';
			注意:
			用以上命令授权的用户不能给其它用户授权，如果想让该用户可以授权，用以下命令:
			GRANT privileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION;
		授权用户backup(随意，不能使用root),允许这个用户通过着个ip地址(从库的ip )访问到数据库的信息，使用的密码 ILoveRedis(3306)
			grant replication slave on *.* TO 'backup'@'192.168.226.129' identified by 'ILoveRedis(3306)';
		刷新
			flush privileges
		重启
			service mysqld restart
		查看主库状态
			show master status\G
			show master status\G;
			show master status
		从库
			show slave status\G
	3. 登录从服务器，设置主从关系
		先登录mysql
		主库的ip 连接主库的用户 密码 读哪个日志文件（主库查看状态可得到） 从哪读
		change master to master_host='192.168.226.128',master_user='backup',master_password='ILoveRedis(3306)',master_log_file='mysql-bin.000002',master_log_pos=154

	4. 主服务器下执行 对外开放端口号
		/sbin/iptables -I INPUT -p tcp --dport 3306 -j ACCEPT
		
	5. 从服务器下执行 对外开放端口号
		/sbin/iptables -I INPUT -p tcp --dport 3306 -j ACCEPT
	6. 全部重启
		service mysqld restart

>可以被远程访问   
>端口号对外开放  
>将mysql库里的用户表中你想要访问的用户对应的host变为%或指定ip


>sql线程为no时解决方法
>MariaDB [(none)]> stop slave;                                                      
MariaDB [(none)]> SET GLOBAL SQL_SLAVE_SKIP_COUNTER=1;           
MariaDB [(none)]> start slave;                                                      
MariaDB [(none)]> show slave status\G 
