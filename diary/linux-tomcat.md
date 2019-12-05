##如何将war包部署到Linux上
####1. 如何打war包
#####* 第一种方式
	打开Tomcat的设置，选择Deployment,添加*.war ,ok。  
	点击build，再点击buildArtifacts,选择*.war  
	所创建的war包在项目所在的target目录里  
#####* 第二种方式
	搜索 MavenProject,再点击Lifecycle.package后运行，即可产生war包，所在目录同上。
####2. 如何部署到Linux上
	1. 打开Linux,再打开SecureCRT
	2. 关闭Linux下的Tomcat,在bin目录下使用 ./shutdown.sh 命令
	3. 通过SecureCRT的sftp将文件下载到Linux的Tomcat的webapps下
	4. Linux系统下在Tomcat的bin目录下开启tomcat，命令 ./startup.sh
	5. Tomcat的默认端口为8080,所以要开启这个端口 命令 firewall-cmd --permanent --add-port=8080/tcp
	6. 重启 命令 servcie firewalld restart
	7. 命令 ifconfig  得到ip地址  
	8. 打开浏览器 ip地址：8080/项目名 即可访问 