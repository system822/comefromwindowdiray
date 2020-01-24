##1/15/2020 10:59:24 AM 
##Focus on what you need to do.
**集中精力做你该做的事。**
###<center>误删root用户的解决方案</center>
	1. 停止数据库，在/etc/my.cnf文件中 mysqlID下面添加
		skip-grant-tables(用于跳过安全密码的验证)
	2. 重启mysql服务
		service mysqld restart
	3. 使用mysql直接进入mysql
		mysql
	4. 使用mysql数据库
		use mysql
	5. 添加新用户root
		INSERT INTO user SET User='root',Host='localhost',ssl_cipher='',x509_issuer='',x509_subject='';
	6. 添加权限
		UPDATE user SET Select_priv='Y',Insert_priv='Y',Update_priv='Y',Delete_priv='Y',Create_priv='Y',Drop_priv='Y',Reload_priv='Y',Shutdown_priv='Y',Process_priv='Y',File_priv='Y',Grant_priv='Y',References_priv='Y',Index_priv='Y',Alter_priv='Y',Show_db_priv='Y',Super_priv='Y',Create_tmp_table_priv='Y',Lock_tables_priv='Y',Execute_priv='Y',Repl_slave_priv='Y',Repl_client_priv='Y',Create_view_priv='Y',Show_view_priv='Y',Create_routine_priv='Y',Alter_routine_priv='Y', Create_user_priv='Y',Event_priv='Y',Trigger_priv='Y',Create_tablespace_priv='Y',authentication_string='' WHERE User='root';
	7. 去掉mycnf中的
		skip-grant-tables
	8. 重启服务
		service mysqld restart
	9. 修改密码
		alter user 'root'@'localhost' identified by '您的密码';
	10. 刷新
		flush privileges

