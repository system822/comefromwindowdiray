##12/22/2019 4:57:49 PM 
###Have a great life. Have fun with everything you're gonna do.
**祝你生活顺利，事事开心。**
##<center>linux的基本命令及tomcat、java、redis环境的配置</center>
###Linux简介
	最初的制作人 李纳斯，托瓦兹（Linus Torvalds） 
	Linux是一个基于posIx和UNIX的多用户，多任务，支持多线程和多cpu的操作系统。
	继承了Unix以网络为核心的设计思想

###Linux下的命令
	ls 查看目录中的文件
	ls -F 查看目录中的文件
	ls -l 显示文件和目录中的详细资料
	ls -a 显示隐藏文件
	ls *[0-9]* 显示包含数字的文件名和目录名
	ls *.zip 显示所有以.zip结尾的文件

	cd home 进入"/home"目录
	cd .. 返回上一级目录
	cd ../.. 返回上两级目录
	cd 进入个人主目录
	cd ~ 进入个人的主目录
	
	mkdir xxx 创建xxx文件夹
	mkdir -p a/b/c 创建多级目录

	touch xxx 创建文件

	rm -r xxx 删除文件（最后一级文件如果存在下一级，则不能删除）支持表达式
	rm -rf xxx 可删除多级目录

	mv oldDirectory newDirectory 移动一个文件到另一个目录
	mv oldFilename newFilename 重命名
	
	cp fileDirectory newFileDirectory 复制文件到新目录
	cp x.txt haha/xx.txt	复制x.txt到haha下并改名xx.txt

	more fileName 按照百分比查看文件内容，空格键翻一页，回车翻一行
	
	tail -n 200 -f fileName 动态的查看文件内容

	ps -ef 查看进程列表
	ps -ef|grep tomcat/端口号 查看当前tomcat运行的进程列表
	
	chmod 777 fileName 给文件目录授权（当前）
	chmod -R 777 directoryName 给文件目录授权（包括子目录）
	（一些 扩展
		属主    同组    其他
		rwx     r-x    r-x
		111     101    101
         7       5      5		
		）
###服务器 待扩展	
	ifconfig 查看当前服务器ip信息
	top 查看服务器当前性能
	ss -tanl 查看linux下开放的端口有哪些
	service xxx stop/start 停止或启动某个服务
	service xxx restart 重新启动某个服务
	exit 退出（ctrl c）
###用户相关命令
	whoami	查看当前登录的用户名
	sudo su root  切换到 root用户
	su root

	adduser userName 增加用户
	passwd userName 给用户设置密码

	userdel userName 删除用户(保留家目录)
	userdel -r userName 删除用户（不保留家目录）
	（注意：删除某个用户时需要将该用户所有进程杀死 ）

	pgrep -u userName 查询某个用户的所有进程
	kill 进程号 杀死进程
	kill -9 进程号 强制杀死进程	
###防火墙相关命令
	firewall-cmd --state 查看防火墙状态
	service firewalld stop 停止防火墙
	firewall-cmd --query-port=portNumber/tcp 查询某个端口是否开放
	firewall-cmd --permanent --add-port=portNumber/tcp 开放某个端口
	firewall-cmd --permanent --remove-port=portNumber/tcp 移除某个端口
	firewall-cmd --reload 修改防火墙配置后，要重新加载

###下载 安装 卸载 解压
	wget http://xxx 在线下载
	yum install xxx -y 在线安装
	yum remove xxx 卸载
	
	tar -xvf fileName //待扩展

###vi vim
	Vi是Unix及Linux系统下标准的编辑器，由美国加州大学伯克利分校的Bill Joy所创立。
	Vim是一个类似于Vi的著名的功能强大、高度可定制的文本编辑器，在Vi的基础上改进和增加了很多特性。VIM是纯粹的自由软件。
	三种模式：
		Command mode	命令模式		vi/vim 某个文件后进入的就是命令模式
		Insert mode		输入模式		i 进入输入模式
		Last line mode 	底线命令模式 命令模式下 shift+:  进入底线命令模式

	命令模式下：
		dd 			删除一行
		dw			删除一个单词
		x			删除一个字母
		u 			撤销上一步操作
		shift+g		到文件尾
		
	底线命令模式下：
		w			写,保存的意思
		q			退出
		wq 			保存退出
		w!			强制保存
		q!			强制退出
		wq!			强制保存退出
		set nu		显示行号
			
		开始的行数，结束的行数s/要被替换的字符/替换后的/g 	替换掉限定范围内所有满足的
		开始的行数，结束的行数s/要被替换的字符/替换后的/ 	替换掉限定范围内所有满足的第一个
		%s/要被替换/被替换/g								全局查找替换

###开启linux的网络的方法
	/etc/sysconfig/network-script
	vi ifctg-ens33
	设置 onBoot=yes
	重启网络服务 service network restart
	
###linux下安装java环境
	1. 使用winscp将jdk的安装包上传到Linux
	2. 拷贝到合适位置使用解压命令解压  tar -xvf fileName
	3. 配置环境变量
		vi ~/.bash_profile
		输入以下内容
			export JAVA_HOME=/opt/java8/jdk1.8.0_171
			export CLASSPATH=$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
			export PATH=$PATH:$JAVA_HOME/bin
	4. 退出文件编辑器让文件生效
		source ~/.bash_profile
	5. 查看java版本
		java -version

###linux下安装Tomcat环境
	1. 使用winscp将tomcat的安装包上传到Linux
	2. 拷贝到合适的位置，使用解压命令解压
		unzip fileName
	3. 对Tomcat文件夹授权
		chmod -R 777 xxx
	4. bin下启动tomcat
		./startup.sh（关闭 ./shutdown.sh）

###linux下安装配置redis
	1. 由于redis编译会使用到gcc和tcl，所以先使用yum安装
		yum install gcc
		yum install tcl
	2. 使用winscp将Redis的安装包上传到linux
	3. 解压Redis并进入解压目录，分别执行
		make
		make install
	4. 创建两个文件夹
		/etc/redis 用于存放配置文件
		/var/redis/6379 (保持持久化位置)
	5. 将Redis的配置文件模板（redis-4.0.11/redis.conf)复制到/etc/redis目录中，以端口号命名（如：6379.conf）。修改配置文件下面选项的值（如果相同就跳过）
		pidfile /var/run/redis_6379.pid
		dir /var/redis/6379
		ort 6379
	6. 在解压的redis-4.0.11/utils目录下有个文件redis_init_script.它是Redis的初始化脚本文件，可以配置Redis的运行方式和持久化文件，日志文件的存储位置。把这个文件复制到/etc/init.d目录下。（init.d目录包含系统各种服务的启动和停止脚本），将其重命名为redis_6379,并修改它里面REDISPORT的值和redis的运行配置文件中的端口号一致。
	7. 更新服务
		chkconfig redis_6379 on
	8. 启动redis服务
		service redis_6379 start
	9. 测试redis
		redis