###It's always the simple things that catch your breath.
**让人顿悟的都是一些简单的东西。**
###12/10/2019 7:12:34 PM 
###不能加载oracle驱动包的原因
	oracle jdbc并不能向mysql那样直接配置，原因是Oracle授权问题，Maven3不提供oracle JDBC driver，需要手动配置下，这里直接用idea自带的maven，省去安装配置相关maven参数
	<dependency>
	<groupId>com.oracle</groupId>
	<artifactId>ojdbc6</artifactId>
	<version>11.2.0.3</version>
	<scope>test</scope>
	</dependency>

###解决方案
	cmd 进入maven\bin目录
	mvn install:install-file -DgroupId=com.oracle -DartifactId=ojdbc6 -Dversion=11.2.0.3 -Dpackaging=jar -Dfile=F:\Oracle\ftpFiles\Oracle\day01\choose\software\sqldeveloper\jdbc\lib\ojdbc6.jar
>>这里版本-Dversion =11.2.03 与dependency 中version一致，-Dfile 是文件路径
###如果安装了oracle ，可直接去目录：
	F:\Oracle\ftpFiles\Oracle\day01\choose\software\sqldeveloper\jdbc\lib
