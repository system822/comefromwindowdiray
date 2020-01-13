##1/6/2020 10:42:57 AM 
##You've got glitter in your veins.
**你骨子里就是发光的。**
###<center>linux下使用nginx</center>
####nginx的安装
	1. 安装前的准备
		yum install wget -y

		因为nginx依赖于gcc的编译环境所以
		yum install gcc-c++

		nginx的http模块使用pcre来解析正则表达式
		yum -y install pcre-devel

		依赖的解压包
		yum -y install zlib zlib-devel

		openssl安装 支持ssl 安全认证 https （可选）
		yum install -y openssl openssl-devel

		官网找到自己需要的版本nginx
		http://nginx.org/en/download.html

		执行如下命令
		http://nginx.org/download/nginx-1.8.1.tar.gz

	2. 安装nginx
		解压nginx文件
			tar zxvf nginx.tar.gz
		指定安装目录
			./configure --prefix=/root/Nginx
		在/root/Nginx/nginx1.8目录下执行编译命令
			make
		执行安装命令
			make install

	3. 启动
		cd sbin/

			./nginx
			./nginx -s stop
			./nginx -s quit
			./nginx -s reload
			./nginx -s quit:此方式停止步骤是待nginx进程处理任务完毕进行停止
			./nginx -s stop:此方式相当于先查出nginx

		开放nginx默认端口号 80
			/sbin/iptables -I INPUT -p tcp --dport 80 -j ACCEPT

>碰到的问题 访问403
>将nginx.conf用户改为root

####接下来的配置和window一样，可参考	nginx.md