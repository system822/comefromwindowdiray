##1/13/2020 9:11:10 AM 
##It’s never too late to do the right thing.
**做正确的事永远都不迟。**
###<center>搭建MySQL数据库集群环境，实现主从备份读写分离。</center>
>[linux_mysql_install](https://www.jianshu.com/p/6dc6fd1f7e3a)

***
####安装mysql
	1. 官网下载安装包
		指定mysql安装路径
			mkdir -p /root/mysql
		下载
			wget  http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
		输入
			rpm  -ivh  mysql57-community-release-el7-8.noarch.rpm
	2. 执行mysql安装命令
		yum  install -y  mysql-community-server
	3. 检查是否安装了mysql
		输入
			yum list installed | grep mysql    或
			rpm -qa | grep -i mysql
	4. 删除安装包
		例子
			yum -y remove mysql-community-client-5.6.38-2.el7.x86_64
	5. 安装成功后，默认配置文件路径
		配置文件：/etc/my.cnf
		日志文件：/var/log/mysqld.log
		服务启动脚本：/usr/lib/systemd/system/mysqld.service
		socket文件：/var/run/mysqld/mysqld.pid
		mysql 初次登入密码是随机密码，所以需要重置密码

####修改mysql登录密码
	1. 启动mysql
		service mysqld start
	2. 查看mysql的状态
		service mysqld status
	3. 初次登录mysql随机密码的位置
		grep "password" /var/log/mysqld.log
	4. 登录mysql
		mysql -uroot -p
		密码是哪个随机密码
	5. 密码重置
		alter user 'root'@'localhost' identified by '您的密码';
	6. 刷新
		flush privileges

>密码的规则挺麻烦，多输几次


####[ms分离]("https://blog.csdn.net/u013378306/article/details/84995481")

