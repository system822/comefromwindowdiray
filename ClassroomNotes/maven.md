##简介

梁建全  微信：oakljq

公众号：西二旗程序员

SpringBoot、NoSQL、SpringCloud、微服务项目、Spring Data、Spring Cache、Thymeleaf

##MAVEN工具

MAVEN是一个项目构建工具，可以单独通过命令行使用，也可以跟Eclipse、IDEA集成工具结合。目的:简化项目搭建、编译、打包、发布等工作。

MAVEN典型优点：

1. 实现jar包共享
2. 通过一段xml定义引入jar包
3. jar包依赖
4. 工程继承、工程聚合


##MAVEN工作流程

![](maven.png)

##Eclipse+maven

1. 下载apache-maven.zip包，解压
2. 打开eclipse菜单window-preferences-Maven

	- 设置installations
	
		![](eclipse-1.png)

	- 设置user-settings

		![](eclipse-2.png)

3. 设置本地库（有网可以不做）

	将其他人的本地库文件放到自己机器上，位置参考上图Local Repository

4. 在eclipse创建maven project

	- 右键创建project-other-maven project

		![](eclipse-3.png)

	- 输入group Id和artifact Id

		![](eclipse-4.png)

	- 如果是war类型工程，需要生成web.xml描述文件

		![](eclipse-5.png)

5. 在eclipse中创建本地库索引

	- 打开window-show view-other-maven repositories窗口

		![](eclipse-6.png)

	- 右键Local Repository 选择Rebuild Index

		![](eclipse-7.png)

6. maven project继承和聚合

	- 创建一个maven project 名称为xdl-parent 项目类型选择pom
	- 再创建一个maven moudle 名称为xdl-a parent project 选择xdl-parent
	- 再创建一个maven moudle 名称为xdl-b parent project 选择xdl-parent

		![](eclipse-8.png)

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



###任务：基于maven创建项目结构

- boot-parent 父工程、聚合工程、pom类型

	导入spring-boot-starter  2.0.5

- Moudle工程boot-01 jar类型 继承boot-parent 
- Moudle工程boot-02 jar类型 继承boot-parent 
- Moudle工程boot-03 jar类型 继承boot-parent 
- Moudle工程boot-04 jar类型 继承boot-parent 
- Moudle工程boot-05 jar类型 继承boot-parent 


