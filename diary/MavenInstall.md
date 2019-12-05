<center><h1>Maven</h1></center>
###maven环境的搭建
* 配置环境变量   
   * 配置M2_HOME
   * 配置MAVEN
   * 配置Path
   * [环境的具体配置](https://www.jianshu.com/p/ef702125c362)  
###配置maven插件
1. 配置maven
2. 配置maven安装目录
3. 依次打开Window –> Perferences –> Maven ，展开Maven的配置界面，然后点击Installations –> add 选择maven安装目录，这里我的Maven安装目录为D:\maven\apache-maven-3.2.3，选择Maven安装目录，并点击确定, 之后可以点击Apply,点击OK，即可完成 
4. 然后， 在Maven的配置界面，设置User Settings Global Settings选择maven 安装目录下conf文件夹下的settings.xml，这里我的Maven安装目录为D:\maven\apache-maven-3.2.3\conf\settings.xml，选择Maven安装目录，检查Local Repository 项，如果为D:/maven/repository则配置成功，否则重新配置上一步。

[来源](https://www.jianshu.com/p/bec6c22bddc6)

###项目的创建
* File>>>new>>>mavenProject>>>createSimple
>>version 默认 
>web项目第三步修改为war 父项目修改为pom
###mavenProject的继承和聚合

	- pom.xml定义如下

			<!-- 继承 -->
			<parent>
				<groupId>cn.xdl</groupId>
				<artifactId>xdl-parent</artifactId>
				<version>0.0.1-SNAPSHOT</version>
			</parent>

			<!-- 聚合 -->
			<modules>
				<module>xdl-a</module>
				<module>xdl-b</module>
			</modules>

