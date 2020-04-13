##12/22/2019 4:57:49 PM 
###Have a great life. Have fun with everything you're gonna do.
**祝你生活顺利，事事开心。**
##<center>linux的基本命令及tomcat、java、redis环境的配置</center>
###Linux简介
	最初的制作人 李纳斯，托瓦兹（Linus Torvalds） 
	Linux是一个基于posIx和UNIX的多用户，多任务，支持多线程和多cpu的操作系统。
	继承了Unix以网络为核心的设计思想

###Linux下的命令
	PS1=[root]$ 将左侧显示的重新命名为[root]$

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
	cd - 回到上一次所在的目录
	
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
		tail 默认在屏幕上显示指定文件的末尾10行
		-f	显示最新追加的内容
		-n 	行数	在屏幕上指定文件的末尾行数

	ps -ef 查看进程列表
	ps -ef|grep tomcat/端口号 查看当前tomcat运行的进程列表
	
###文件授权
	chmod 777 fileName 给文件目录授权（当前）
	chmod -R 777 directoryName 给文件目录授权（包括子目录）

		u所有者 		 g所属组  	  r其他人
		  rwx         r-x          r-x
	      111         101          101
	       7           5            5		
		r 读 
		w 写
		x 执行

	方式一：
		chmod u+x 文件			#给当前用户添加指定文件的x执行权限
		chmod g+w,o+w 文件		#给该文件用户组合其他人添加指定文件的w写的权限
		chmod a=rwx 文件			#给该文件的当前用户,当前组,其他人 添加rwx可读可写可执行的权限



###文件搜索命令
	whereis 命令名		#索索命令所在路径及帮助文档所在位置
	which 命令名 	#搜索命令所在路径及别名
	
	find [搜索范围] [搜索条件]
	find /root -name a.txt 		#搜索名为a.txt的文件 区分大小写
	find /root -iname a.txt		#搜索名为a.txt的文件 不区分大小写
	find /root -user root		#按照所有者查询
	find /root -nouser			#查找没有所有者的文件
###字符串搜索命令
	grep [选项] 字符串 文件名  -i 忽略大小写 -v 排除指定的字符串
	grep -i xxx a.txt		#查询a.txt中包含xxx的内容，忽略大小写
	grep xxx a.txt			#查询a.txt中包含xxx的内容，不忽略大小写
	grep -v xxx a.txt 		#查询a.txt中排除xxx的内容，不忽略大小写
	grep -i -v xxx a.txt 	#查询a.txt中排除xxx的内容，忽略大小写
###帮助命令
	man ls 				#查看ls的帮助
	ls --help			#获取ls命令选项的帮助
###压缩和解压缩命令(.zip .gz .tar .tar.gz)
	zip 压缩文件名 源文件
	zip xx yy.txt		#将yy.txt压缩成xx.zip
	zip  -r xx yy		#将yy目录压缩成xx.zip
	unzip xx.zip		#将xx.zip解压

	gzip a.txt 			#将a.txt压缩成a.txt.gz格式，原有文件会消失
	gzip -r haha		#不会将haha目录压缩，只是将haha目录下所有文件压缩成.gz格式
	gunzip	a.txt.gz 	#解压
	gzip -d a.txt.gz	#解压

	tar 参数 打包文件名 源文件
	参数
		-c 打包
		-v 显示过程
		-f 指定打包后的文件名
		-x 解打包
		-z 压缩成.tar.gz格式
	tar -cvf a.tar a.txt				#将a.txt打包成a.tar
	tar -xvf a.tar						#解打包
	tar -cvf ha.tar ha 					#将ha文件夹打包
	tar -zcvf ha.tar.gz ha				#将ha文件夹打包压缩
	tar -zxvf ha.tar.gz					#解压解打包
	tar -zxvf ha.tar.gz -C 路径			#解压到哪

###rpm相关命令

	安装一个包 
		rpm -ivh <包名>
		--nodeps 如果该RPM包的安装依赖其它包，即使其它包没装，也强迫安装。 
		--force 即使覆盖属于其它包的文件也强迫安装 
	查询一个包是否被安装 
		rpm -q <软件名>
	得到被安装的包的信息 
		rpm -qi < 软件名> 
	列出该包中有哪些文件 
		rpm -ql < 软件名> 
	列出服务器上的一个文件或目录属于哪一个RPM包 
		rpm -qf <文件或目录名>
	列出所有被安装的rpm package 
		rpm -qa 
	卸载一个包 
		rpm -e <软件名>

###关机 重启。。
	shutdown [选项] 时间
	参数
		-c 取消前一个关机命令 
		-h 关机
		-r 重启
		-k 向所有用户发送警告信息
	shutdown -h now 						#立即关机
	shutdown -h 12:00						#12点关机
	shutdown -h +7 "7分钟后关机"				#7分钟后关机
	
	其他一些关机命令
		halt
		poweroff
		init 0
	这3个命令不是很安全，因为它们不会帮我们保存数据
	
	其他重启命令
		reboot
		init 6
	
###服务器 待扩展	
	date -R 当前时间
	ifconfig 查看当前服务器ip信息
	ip addr	查看当前服务器ip信息
	top 查看服务器当前性能
	ss -tanl 查看linux下开放的端口有哪些
	service xxx stop/start 停止或启动某个服务
	service xxx restart 重新启动某个服务
	exit 退出（ctrl c）
	sync 命令的作用就是把内存中的数据强制向硬盘中保存


	df [-h]						#磁盘使用情况 -h格式化输出
	echo						#在显示器上输出内容
	free 						#查看内存占用
	top							#查看任务进程
	netstat -ntlp				#查看端口（需下载net-tools）
###用户相关命令
	whoami	查看当前登录的用户名
	sudo su root  切换到 root用户
	su root

	adduser userName 增加用户
	passwd userName 给用户设置密码

	userdel userName 删除用户(保留家目录)
	userdel -r userName 删除用户（不保留家目录）
	（注意：删除某个用户时需要将该用户所有进程杀死 ）
		-f：强制删除用户，即使用户已登录 
		-r：删除与用户相关的所有文件。
	pgrep -u userName 查询某个用户的所有进程

###进程相关命令
	进程查看:ps:
		用于报告当前系统的进程状态。可以搭配kill指令随时中断、删除不必要的程序。


	ps -ef 显示出的结果：
	    1.UID       用户ID
	    2.PID        进程ID
	    3.PPID      父进程ID
	    4.C           CPU占用率
	    5.STIME     开始时间
	    6.TTY         开始此进程的TTY----终端设备
	    7.TIME       此进程运行的总时间
	    8.CMD       命令名.

	kill 进程号 杀死进程
	kill -9 进程号 强制杀死进程	

###用户组相关命令
	创建用户组	groupadd 组名	#查看系统用户组：cat /etc/group
	删除组	groupdel 组名

	查看系统用户：cat /etc/passwd
	1	用户名
	2	用户的密码，用x替代
	3	用户的uid,一般情况下root为0，1-499默认为系统账号，有的更大些到1000，500-65535为用户的可登录账号，有的系统从1000开始。
	4	用户的gid,linux的用户都会有两个ID,一个是用户uid，一个是用户组id，在我们登录的时候，输入用户名和密码，
	其实会先到/etc/passwd查看是否有你输入的账号或者用户名，有的话将该账号与对应的UID和GID(在/etc/group中)读出来。
	然后读出主文件夹与shell的设置，然后再去检验密码是否正确，正确的话正常登录。
	5	用户的账号说明解释
	6	用户的家目录文件夹
	7	用户使用的shell，如果换成/sbin/nologin/就是默认没有登录环境的。
	
	案例
	1、将 newuser2 添加到组 staff 中
		usermod -G staff newuser2

	2、修改 newuser 的用户名为 newuser1
		usermod -l newuser1 newuser

	3、锁定账号 newuser1
		usermod -L newuser1

	4、解除对 newuser1 的锁定
		usermod -U newuser1

	5、将一个用户添加到用户组中，千万不能直接用,这样做会使你离开其他用户组,仅仅做为 这个用户组 groupA的成员。： 
		usermod -G groupA user
	6、-a 代表 append， 也就是 将自己添加到 用户组groupA 中，而不必离开 其他用户组。 
		usermod -a -G groupA user

*****
	修改文件的所有者:	用法:chown 用户名 文件名
	修改文件的所属组:	用法:chgrp 组名 文件名

###分配用户权限
	sudo权限:
		root把本来只能超级用户执行的命令赋予普通用户执行.
		sudo的操作对象是系统命令

	visudo
		实际修改的是/etc/sudoers文件
		root ALL=(ALL) ALL				用户名 被管理主机的地址=(可使用的身份) 授权命令(绝对路径)
		%wheel ALL=(ALL) ALL			%组名 被管理注解的地址=(可使用身份) 授权命令(绝对路径)

###防火墙相关命令
	firewall-cmd --state 查看防火墙状态
	service firewalld stop 停止防火墙
	firewall-cmd --query-port=portNumber/tcp 查询某个端口是否开放
	firewall-cmd --permanent --add-port=portNumber/tcp 开放某个端口
	firewall-cmd --permanent --remove-port=portNumber/tcp 移除某个端口
	firewall-cmd --reload 修改防火墙配置后，要重新加载

###服务命令
	
	启动服务：systemctl start <服务名>
	关闭服务：systemctl stop <服务名>
	重启服务：systemctl restart <服务名>
	查看服务状态：systemctl status <服务名>
	添加开机启动项：systemctl enable <服务名>
	禁止开机启动项：systemctl disable <服务名>
	查看开机启动项：systemctl list-unit-files


###下载 安装 卸载 解压

	wget http://xxx 			在线下载
	yum install xxx -y 			在线安装
	yum remove xxx 				卸载
	yum list  					查看yum库中的所有包
	yum list installed			查看已经安装的软件包

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

###网络管理 配置静态ip
	1. vi /etc/sysconfig/network-scripts/ifcfg-ens32
	2. BOOTPROTO="static"
		IPADDR=192.168.226.131
		NETMASK=255.255.255.0
		DNS1=114.114.114.114

		GETWAY=192.168.226.2(编辑 虚拟网络编辑器 VMnet8 net设置)
		BROADCAST=192.168.226.255 
	
	3. 重启网络服务
		systemctl restart network
		service network restart

###linux的克隆
	1. 选中要克隆的虚拟机，右键管理 克隆  （完整克隆）
	2. 克隆完毕后，点击设置，网络适配器 高级 生成mac地址（为了区分）
	3. 开启虚拟机，修改ip地址即可
	4. 重启网络服务
			
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

	others:

	1 要从网上去下载对应的安装包
	2 把下载好的安装包上传到Linux的服务器当中(/usr/local/software)
	3 解压操作 tar -zxvf jdk的安装包名 -C  /usr/local
	4 改一个名字 mv  jdk1.8_123   jdk1.8
	5 配置环境变量 vi /etc/profile   按一个大写的G 和小写的o 在最下面去配置信息
	export JAVA_HOME=/usr/local/jdk1.8
	export PATH=/usr/local/jdk1.8/bin:$PATH
 
	6 source /etc/profile 重新去加载一下配置文件

###linux下安装Tomcat环境
	1. 使用winscp将tomcat的安装包上传到Linux
	2. 拷贝到合适的位置，使用解压命令解压
		unzip fileName
	3. 对Tomcat文件夹授权
		chmod -R 777 xxx
	4. bin下启动tomcat
		./startup.sh（关闭 ./shutdown.sh）

	others:

	1上网上把安装包给下载好
	2 把包上传到linux服务器当中
	3 tar -zxvf 安装包 -C  /usr/local
	4 启动 tomcat
	运行tomcat 一共两种方式
		一种  切换到tomcat的bin目录当中 输入./startup.sh
		二种  /usr/local/tomcat/startup.sh

	5 查看tomcat 有没有启动 ,去查看日志文件
		cat catalina.out
		ps -ef |grep java
		netstat -ntlp      =====>yum install -y net-tools

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

###mysql的安装

	1、先把postfix 和mariadb-libs卸载掉，不然的会有依赖包冲突：[root@wolfcode]#  rpm -e postfix mariadb-libs

	2、安装mysql的依赖net-tools和 perlyum -y install net-tools perl

	3、安装mysql-common包：[root@wolfcode]#  rpm -ivh mysql-community-common-5.7.22-1.el7.x86_64.rpm

	4、安装mysql-libs包：[root@wolfcode]# rpm -ivh mysql-community-libs-5.7.22-1.el7.x86_64.rpm

	5、安装mysql-client包；[root@wolfcode]# rpm -ivh mysql-community-client-5.7.22-1.el7.x86_64.rpm

	6、安装mysql-server包[root@wolfcode]# rpm -ivh mysql-community-server-5.7.22-1.el7.x86_64.rpm

	5、设置开机启动：
	[root@wolfcode]#  systemctl enable mysqld

	6、启动MySql服务
	[root@wolfcode]#  systemctl start mysqld

	7、由于MySQL5.7安装好后会给root用户分配一个临时密码，所以我们先查看临时密码[root@wolfcode]#  grep 'temporary password' /var/log/mysqld.log2018-06-01T19:40:08.341478Z 1 [Note] A temporary password is generated for root@localhost: Ct<pX.k7S(=w冒号后面的就是root用户的临时密码：Ct<pX.k7S(=w

	8、使用临时密码登录
	[root@wolfcode]#  mysql -u root -p
	输入密码：Ct<pX.k7S(=w
	
	9、设置root的密码
	mysql>ALTER USER 'root'@'localhost' IDENTIFIED BY 'WolfCode_2017';
	mysql> exit
>注意：mysql5.7增加了安全级别，密码必须包含：大小写字母、数字和特殊符号，并且长度不能少于8位。

	10、用新密码登陆
	[root@wolfcode]#  mysql -u root -p输入密码：WolfCode_2017

	11、开放远程登录权限mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'WolfCode_2017'  WITH GRANT OPTION;
	mysql> FLUSH PRIVILEGES;

	12、开放mysql的3306端口[root@wolfcode]# firewall-cmd --zone=public --add-port=3306/tcp --permanent[root@wolfcode]# firewall-cmd --reload

>如果出现乱码:
在链接地址栏后添加useUnicode=true&characterEncoding=utf-8

>安装Linux出现问题
>编辑 首选项 设备 开启虚拟打印