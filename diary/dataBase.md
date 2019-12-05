###12/5/2019 3:24:09 PM 
###There is no gene for the human spirit.
**没有哪个基因能够决定人的灵魂。**
###<center>Oracle</center>
####1. 数据库的概述
	1.1数据库的基本概念
		数据库就是存放数据的仓库
	1.2主流数据库
		 目前主流的数据库有：Oracle、Mysql、SQL Server、DB2、...
####2. Oracle环境的搭建
	2.1oracle的安装与卸载
		(1)下载方式: www.oracle.com 
  		(2)安装方式: 对于精简版来说，一路点击下一步进行安装即可，密码设置为：123456
		 切记安装路径中不能有中文路径
		(3)卸载方式：控制面板 => 程序和功能 => 找到oracle，鼠标右击选择卸载 =>下一步   切记删除oracle的安装目录！
	2.2 Oracle的访问方式
		(1)当数据库没有安装在本地主机时，需要使用telnet IP地址 的方式登录远程服务器
			a.使用命令行工具在dos窗口中输入sqlplus后输入数据库账号和密码即可；
			b.使用图形化工具sql developer输入账号和密码进行登录；
	2.3 表空间和账户的创建
		由于system账户是Oracle的管理员账户，若使用该账户操作数据库时可能会造成无法恢复的误操作，因此需要开通新的账户和表空间，所谓表空间就是该账户在磁盘上可以访问的磁盘空间，具体方式如下：
			create tablespace practice        -- 表示创建名字为practice的表空间
			datafile 'D:/practice.dbf'        -- 表示关联到D:/practice.dbf的文件
			size 10M;                         -- 表示空间的大小为10兆
			drop tablespace practice          -- 表示删除名字为practice表空间
			including contents and datafiles; -- 表示将内容和文件一并删除
			create user xiaomage              -- 表示创建名字为xiaomage的账户
			identified by 123456              -- 表示设置该账户的密码为123456
			default tablespace practice;      -- 表示设置该账户的默认表空间为practice
			drop user xiaomage;               -- 表示删除名字为xiaomage的账户
			grant create view to xiaomage;       -- 表示授予xiaomage创建视图的权限
			grant connect, resource to xiaomage; -- 表示授予连接、建表等权限
			revoke create view from xiaomage;       -- 撤销创建视图的权限
			revoke connect, resource from xiaomage; -- 撤销连接、建表等权限
####3. 常用的数据类型
	Java语言中的数据类型有：byte、short、int、long、float、double、boolean、char类型、String类型、...
	Oracle数据中的数据类型有：
		number(n)   - 表示具有n位数字的整数数值，无数字相当于38位,
                    如：number(2)等
		number(n,m) - 表示该数值具有n位数字，其中小数点后面有m位数字，
                    如：number(3,2)等
		char(n) - 表示描述长度为n的定长字符串，若没有存满则使用空格补齐，
                    如：char(3)等
		varchar2(n) - 表示描述长度为n的变长字符串，若没有存满则自动缩小，
                    如：varchar2(20)等  
		date  - 表示描述年月日时分秒的日期数据。
####4. SQL结构化查询语言
	4.1 基本分类
		(1)DDL数据定义语句(Data Definition Language)
			- 主要用于实现对表格的创建、修改以及删除操作。
		(2)DML数据操作语句(Data Manipulation Language)
			- 主要用于实现对表格内中数据的插入、修改(更新)以及删除操作。
		(3)Select查询语句
			- 主要用于实现对表格内数据的查询操作。
		(4)TCL事务控制语句(Transaction Control Language)
			- 主要用于实现对数据更改的提交和回滚(撤销)等操作。
	4.2 DDL数据定义语句
		(1)创建表格的语法格式
			create table 表名(
				字段名称1 字段类型,
				字段名称2 字段类型,
      					... 
			);
		(2)修改表格的语法格式
			alter table 表名 drop column 列名；
			--删除person表中名字为sex的列
			alter table perosn drop column sex;

			alter table 表名 add 列名 类型
			--向person表中添加名字为gender的列
			alter table person add gender char(3)

			alter table 表名 rename to 新的表名
			--实现将person表的名字改为people
			alter table person rename to people;

			alter table 表名 rename column 列名 to 新的列名
			--实现将people表中名字为gender的列名改为sex
####5. 经验的分享
	为了后期对表格的扩展，可以在创建表格时提供保留字段
*****
****
****
###<center>select查询语句1</center>
####1. select
	1.1 查看表结构
		desc 表名；
	1.2 字段的查询
		select 字段名1，字段名2，... from 表名；
	1.3 常用的算术运算符
		+ 加法	-减法	*乘法	/除法	mod()取余
		
		例子：
			 -- 查询员工表中所有员工加上500元过节费后的薪水
   				select salary, salary+500 from s_emp;
  			 -- 查询员工表中所有员工降薪100元后的薪水
  				select salary, salary-100 from s_emp;
  			 -- 查询员工表中所有员工的年薪，一年16个薪水  
   				select salary, salary*16 from s_emp;
  			 -- 查询员工表中所有员工的日薪，一个月按照22天计算
  				select salary, salary/22 from s_emp;
			-- 查询员工表中所有员工涨薪200元后的年薪
   				select salary, (salary+200)*16 from s_emp;
  			 -- 使用取余运算
   				select mod(9, 2) from s_emp;
	1.4 列的别名
		select 字段名1 [as] 别名1，字段名2[as] 别名2，...from 表名；
		
>>注意：  
>a. 当别名中有空格或者需要区分大小写时，就需要使用“”括起来作为整体
>b. sql语句本身不区分大小写，但内容是区分大小写的
>c. 改变sql查询语句区分大小写使用COLLATE
>例如：1 select * from user where name COLLATE Chinese_PRC_CS_AI = "abc";
>2 select * from user where name ='abc' collate Chinese_PRC_CS_AI_WS

	1.5 字符串的拼接
		||实现字符串的拼接，相当于java语言中的+运算符。
		
		例子：
			 -- ' 就是转义字符  '' 表示打印一个'
  			 select last_name||''''||first_name 姓名 from s_emp;
	1.6 where子句
		select 字段名1 [as] 别名1，字段名2 [as] 别名2，... from 表名 [where条件];
	1.7 常用的比较云算符
		> 大于 >= 大于等于 <小于 <=小于等于 =等于 <> 不等于
	
>>sql语句中没有赋值的概念，=表示等于的含义

	1.8 逻辑运算符
		and 并且 or 或者 not 非

>注意：  
>a. and云算符的优先级高于or，因此希望先执行or运算时可以使用（）来提高优先级
>b. where子句的执行次序是从右向左的，有些建议：
>   对于and运算符来说，应该将假的可能性更高的放在右边；   
>   对于or运算符来说，应该将为真的可能性更高的条件放在右边；

	1.9 按照范围进行查询
		between ... and .. 表示在...和...之间，闭区间
>>笔试的考点：
>     没有报错，查询不到任何数据
>     select first_name,salary from s_emp where salary between 1400 and 800;

	2.0 空值的处理
		is null - 判断是否为空
		is not null - 判断是否不为空
	2.1 数据的去重
		select [distinct] 字段名1 [as] 别名1，字段名2 [as] 别名2，... from 表名 [where条件];
			例子：
				-- 查询员工表中所有部门的编号，要求去重
  					 select distinct dept_id from s_emp;

	2.2 按照列表进行查询
		in 表示在...里
		not in 表示不再...里
		例子：
			 -- 查询员工表中部门编号不为41和42的员工名称，薪水以及部门编号信息
   				select first_name, salary, dept_id from s_emp where dept_id not in(41, 42); 

	2.3 模糊查询
		like ... 表示按照指定的条件进行模糊查询
			_表示可以出现任意一个字符
			%表示可以出现任意多个字符

			 -- 查询员工表中名字使用字母b开头的员工名字以及员工薪水
 			  -- 由于大小写问题查询不到任何数据
  				 select first_name, salary from s_emp where first_name like 'b%';
  			 -- 查询到数据
 				select first_name, salary from s_emp
  				 where first_name like 'B%';
	2.4 指定转义字符
			-- 需要指定转义字符
   				select first_name, salary from s_emp     
   				where first_name like '\_%' escape '\'; 
   			-- 指定'#'转义字符
   				select first_name, salary from s_emp     
   				where first_name like '#_%' escape '#'; 

****
****
###12/5/2019 5:08:54 PM 
###<center>select查询语句2</center>
####1. select查询语句
	1.1 字段的排序
		select[distinct] 字段名1 [别名1],字段名2 [别名2],... from 表名 [where 条件] [order by 字段名/别名/位置编号 asc/desc];

		 -- 经验分享：当降序排列时空值默认在最前面影响用户体验度，应该放在后面
   			select last_name, salary from s_emp
   			order by last_name desc nulls last;

	1.2 常用的字符串函数
		 -- 查询字符串的长度
   			select length('hello') 计算字符串的长度 from dual;   -- 5
   		-- 实现将字符串所有字符串转换为大写
   			select upper('hello') 将字符串转换为大写 from dual;  -- HELLO
   		-- 实现将字符串所有字符转换为小写
   			select lower('HELLO') 将字符串转换为小写 from dual;  -- hello
   		-- 实现将字符串中首字母转换为大写
   			select initcap('hello') 首字母转换为大写 from dual; -- Hello
   		-- 实现字符串中内容的截取，表示从下标2开始截取3个字符，下标从1开始
   			select substr('hello', 2, 3) 截取字符串 from dual;  -- ell
   		-- 实现两个字符串之间的拼接
   			select concat('hello', 'world') 拼接字符串 from dual; -- helloworld
   		-- 实现字符串中内容的替换，表示将'e'替换为'E'
   			select replace('hello', 'e', 'E') 替换字符串 from dual; -- hEllo
   		-- 实现字符串中内容的替换，全部替换
   			select replace('hehe', 'e', 'E') 替换字符串 from dual;  -- hEhE
   		-- 实现字符串中子字符串第一次出现的索引位置
   			select instr('hello', 'e') 查找下标 from dual;          -- 2
   		-- 实现字符串中前后两端空白字符的去除
   			select trim('     hello     ') 去除两端空白 from dual;  -- hello
   			select length(trim('     hello     ')) 去除两端空白 from dual; -- 5
	1.3 常用的数值函数
		-- 实现数值18.56789后保留3位小数并进行四舍五入的功能
   			select round(18.56789, 3) 保留3位小数 from dual;    -- 18.568
   		-- 实现数值18.54789后保留2位小数并进行四舍五入的功能
  			 select round(18.54789, 2) 保留2位小数 from dual;    -- 18.55
   		-- 实现数值18.54789后保留1位小数并进行四舍五入的功能
   			select round(18.54789, 1) 保留1位小数 from dual;    -- 18.5
   		-- 实现数值18.54789后保留0位小数并进行四舍五入的功能
   			select round(18.54789, 0) 保留0位小数 from dual;    -- 19
   		-- 实现数值18.54789后保留-1位小数并进行四舍五入的功能
   			select round(18.54789, -1) "保留-1位小数" from dual;    -- 20
   		-- 实现数值18.54789后保留-2位小数并进行四舍五入的功能
   			select round(18.54789, -2) "保留2位小数" from dual;     -- 0

   		-- 实现数值18.56789后保留3位小数并进行截取的功能
   			select trunc(18.56789, 3) 保留3位小数 from dual;    -- 18.567
   		-- 实现数值18.54789后保留2位小数并进行截取的功能
   			select trunc(18.54789, 2) 保留2位小数 from dual;    -- 18.54
   		-- 实现数值18.54789后保留1位小数并进行截取的功能
   			select trunc(18.54789, 1) 保留1位小数 from dual;    -- 18.5
   		-- 实现数值18.54789后保留0位小数并进行截取的功能
   			select trunc(18.54789, 0) 保留0位小数 from dual;    -- 18
   		-- 实现数值18.54789后保留-1位小数并进行截取的功能
   			select trunc(18.54789, -1) "保留-1位小数" from dual;    -- 10
   		-- 实现数值18.54789后保留-2位小数并进行截取的功能
   			select trunc(18.54789, -2) "保留-2位小数" from dual;     -- 0
	1.4 常用的日期函数
		to_char函数主要用于将数据中的日期类型数据获取出来并转换为字符串类型显示出来
		to_date函数主要用于将字符串类型的日期数据转换为日期类型数据再插入到数据库

		例子：
			
			-- 查询当前的系统时间
   				select sysdate from dual;
   			-- 查询当前系统时间对应的昨天
   				select sysdate-1 from dual;
   			-- 查询当前系统时间对应的前天
   				select sysdate-2 前天 from dual;
   			-- 查询当前系统时间对应的明天
   				select sysdate+1 明天 from dual;
   			-- 查询当前的系统时间并按照年月日时分秒的格式显示出来
   				select to_char(sysdate, 'yyyy-mm-dd hh24:mi:ss') 当前系统时间 from dual;
   			-- 查询员工表中所有员工的入职日期并按照年月日显示出来
   				select to_char(start_date, 'yyyy-mm-dd') 入职日期 from s_emp;
   			-- 向员工表中插入'常宏'的信息并指定入职日期为当前系统时间
   				insert into s_emp (id, last_name, first_name, start_date) values(27, '常', '宏', sysdate);
   			-- 向员工表中插入'张衡'的信息并指定入职日期为当前系统时间
   				insert into s_emp (id, last_name, first_name, start_date) values(28, '张', '衡', to_date('2019-08-30', 'yyyy-mm-dd'));

   			-- 查询当前系统时间的下一个月
   				select to_char(add_months(sysdate, 1), 'yyyy-mm-dd') 下一个月 from dual;
   			-- 查询当前系统时间的下两个月
   				select to_char(add_months(sysdate, 2), 'yyyy-mm-dd') 下两个月 from dual;
   			-- 查询当前系统时间的上一个月
   				select to_char(add_months(sysdate, -1), 'yyyy-mm-dd') 上一个月 from dual;
   			-- 查询当前系统时间的上两个月
   				select to_char(add_months(sysdate, -2), 'yyyy-mm-dd') 上两个月 from dual;
   			-- 查询当前系统时间加1个月加1天加1小时加1分
   			-- add_months函数返回的是日期类型，也就是date类型
   				select to_char((add_months(sysdate, 1) + 1 + 1/24 + 1/24/60), 'yyyy-mm-dd hh24:mi:ss') from dual;

   			-- 查询当前系统时间，并按照dd进行截取然后转换为字符串显示
   			-- 保留到dd，dd后面的时分秒全部丢弃
   				select to_char(trunc(sysdate, 'dd'), 'yyyy-mm-dd hh24:mi:ss') 按天截取 from dual;
   			-- 查询当前系统时间，并按照mm进行截取然后转换为字符串显示
   				select to_char(trunc(sysdate, 'mm'), 'yyyy-mm-dd hh24:mi:ss') 按月截取 from dual;
   			-- 查询当前系统时间，并按照yyyy进行截取然后转换为字符串显示
   				select to_char(trunc(sysdate, 'yyyy'), 'yyyy-mm-dd hh24:mi:ss') 按年截取 from dual;

	1.5 常用的聚合(多行）函数
		单行函数 - 主要是讲单条数据内容传入该函数处理后的结果还是单条数据内容。
		聚合（多行）函数 - 主要是将多条数据内容输入该函数处理后的结果还是单条数据内容。

		常用的聚合函数
			
       		sum() - 主要用于实现多条数据累加和的计算，只能用于数值类型。
       		avg() - 主要用于实现多条数据平均值的计算，只能用于数值类型。
       		max() - 主要用于实现多条数据最大值的计算，可以处理任意类型的数据。
       		min() - 主要用于实现多条数据最小值的计算，可以处理任意类型的数据。
       		count() - 主要用于实现多条记录统计个数的计算，通常使用count(*)来实现。

	1.6 分组查询
		select [distinct] 字段名1 [别名1],字段名2 [别名2],...from 表名 [where条件] [group by 字段名] [order by 字段名/别名/位置编号 asc/desc];

		例子：
			  -- 查询学生表中每个年级的总人数并显示出来，要求按照年级总人数降序排列
   				select gradeid 年级编号, count(*) 总人数 from student group by gradeid order by count(*) desc;
	1.7 分组之后的再次过滤
		select [distinct] 字段名1 [别名1]，字段名2 [别名2],...from 表名 [where条件] [group by 字段名] [having 条件] [order by 字段名/别名/位置编号 asc/desc];

		例子：
			-- 查询每个年级的总人数且总人数要超过18人的才显示出来
				select gradeid 年级编号， count(*) 总人数 from student where count(*) > 18 group by gradeid;
>错误的原因：where 子句中不能出现聚合函数

				select gradeid 年级编号, count(*) 总人数 from student group by gradeid having count(*) > 18;

	1.8 总结 select查询语句一般的执行流程
		from 表名 => where 子句进行过滤 => group by子句进行分组 => having子句进行再次过滤

****
****
###12/5/2019 6:42:17 PM 
###<center>select查询语句3</center>
	1.1 子查询
		子查询就是指将一个查询语句的结果作为另一个查询语句的条件的方式，又叫做多次/多重查询，也就是查询语句中嵌套这另一个查询语句。
		查询语句的语法格式：
			  -- 查询学生表中年龄比'崔今生'小的学生信息，也就是出生日期比'崔今生'大的学生
			select [distinct] 字段名1[as 别名1]，字段名2[as 别名2],... from 表名 [where 条件表达式] [group by 字段名] [having 条件表达式] [order by 字段名/别名/位置编号 asc/desc];
		例子：
			   -- 第一步：先查询学生表中'崔今生'的出生日期   '1990-01-05'
   					select to_char(borndate, 'yyyy-mm-dd') 出生日期 from student where studentname = '崔今生';
   				-- 第二步：再查询学生表中出生日期大于'崔今生'出生日期的学生信息
   					select to_char(borndate, 'yyyy-mm-dd') 出生日期 from student where borndate > to_date('1990-01-05', 'yyyy-mm-dd');

   				-- 合并起来使用子查询一步到位
   					select to_char(borndate, 'yyyy-mm-dd') 出生日期 from student where borndate > (select borndate from student where studentname = '崔今生');
<a id="fff"></a>
	1.2 多表查询（内连接）
		单表查询 - 主要指select查询语句中from关键字后面只跟一个表名的形式
		多表查询 - 主要指select查询语句中from关键字后面跟着多个表名的形式-也就是说同时查询的数据来自多张表
		
		1 内连接
			所谓内连接就是指使用两张表都有的字段和关系云算符进行匹配，从而得到两张表都有数据内容组成的结果集。
			语法格式一：
				select [表名1.]字段名1，[表名2.]字段名2，... from 表名1[别名1],表明2[别名2],... where 表之间的关联条件；
			语法格式二：
				select [表名1.]字段名1,[表名2.]字段名2,... from 表名1[别名1] inner join 表名2[别名2] on 表之间的关联条件；

		2 例子：
			-- 等值连接
   					select student.studentno, studentname, studentresult from student, result where student.studentno = result.studentno;
			-- 新版本的写法
					select student.studentno, studentname, studentresult from student inner join result on student.studentno = result.studentno;
>>注意：  
>        -- 当内连接忘记指定表之间的关联条件时，就是该方式会使用第一张表中的每一条数据与第二张表中的每一条数据都组合一次

###12/5/2019 8:02:14 PM 
###<center>select查询语句</center>
#####1 连接
	1.1 内连接 见上方
	1.2 左外连接
		所谓左外连接就是指将满足关联条件且俩张表都有的数据显示之后，将左表中存在但右表中没有匹配的数据也会显示出来，只是将右表中没有数据使用null填充。也就是左表为主，将左表所有数据显示出来。
		语法格式 旧版：
			select [表名1.]字段名1 别名1，[表名2.]字段名2，... from 表名1[别名1]，表名2[别名2] where [表名1.]字段名 关联 [表名2.]字段名(+);
		语法格式 新版：
			select [表名1.]字段名1 别名1，[表名2.]字段名2,... from 表名1[别名1] left outer join 表名2[别名2] on 两张表的关联条件;

		例子：
			-- 查询学生的名称、课程的编号以及对应的考试成绩   使用左外连接
			-- 该需求涉及到：student学生表 和 result成绩表  通过studentno学号关联
				select studentname, subjectid, studentresult from student, result where student.studentno = result.studentno(+);
			
			-- 查询学生的名称、课程的编号以及对应的考试成绩   使用左外连接
			-- 该需求涉及到：student学生表 和 result成绩表  通过studentno学号关联
			-- 新版格式
			select studentname, subjectid, studentresult from student left outer join result on student.studentno = result.studentno;
	1.3 右外连接
		所谓右外连接就是指将满足关联条件且两张表都有的数据显示之后，将右表中存在但左表中没有匹配的数据也会显示出来，只是将左表中左表中没有数据使用null填充。

		语法格式(旧版):
			select [表名1.]字段名1 别名1, [表名2.]字段名2, ... from 表名1 [别名1], 表名2 [别名2] where [表名1.]字段名(+) 关联 [表名2.]字段名;
		语法格式(新版)：
			select [表名1.]字段名1 别名1, [表名2.]字段名2, ... from 表名1 [别名1] right outer join 表名2 [别名2] on 两张表的关联条件; 

		例子：
			-- 若成绩表中没有多余的数据，则使用insert into插入一个数据测试
			-- 查询学生的名称、课程的编号以及对应的考试成绩   使用右外连接
			-- 该需求涉及到：student学生表 和 result成绩表  通过studentno学号关联
				select studentname, subjectid, studentresult from student, resultwhere student.studentno(+) = result.studentno;    

			-- 查询学生的名称、课程的编号以及对应的考试成绩   使用左外连接
			-- 该需求涉及到：student学生表 和 result成绩表  通过studentno学号关联
			-- 新版格式
				select studentname, subjectid, studentresult from student right outer join result on student.studentno = result.studentno;  
	1.4 全外连接
		所谓的全外连接就是指满足关联条件且两张表都有的数据显示之后，将左右表没有匹配的数据也会显示出来，只是将左右表中没有的数据使用null填充。也就是说以左右表为主，将左右表中所有的数据显示出来。
		语法格式（旧版不支持！）
		语法格式（新版）：
			select [表名1.]字段名1 别名1，[表名2.]字段名2，... from 表名1 [别名1] full outer join 表名2[别名2] on 两张表的关联条件；

		例子：
			-- 查询学生的名称、课程的编号以及对应的考试成绩   使用左外连接
			-- 该需求涉及到：student学生表 和 result成绩表  通过studentno学号关联
			-- 新版格式   内：88  左外：118  右外：89  全外：119  
				select studentname, subjectid, studentresult from student full outer join result on student.studentno = result.studentno;

	1.5 分页查询
		由于查询到的数据内容比较多时无法在一个页面中全部显示，因此可以将查询到的数据内容分成很多页分别显示即可，这样的机制就叫分页查询。
	
		分页查询的优化：
			-- 借助伪列实现真正的分页查询, 采用三层子查询
			-- 第一步：负责排序
				select * from xdl_bank_account_34 order by acc_money
			-- 第二步：负责编号起别名以及部分过滤
				select rownum r,t.* from (select * from xdl_bank_account_34 order by acc_money) t    
			-- 第三步：使用别名进行过滤
				select * from (select rownum r,t.* from (select * from xdl_bank_account_34 order by acc_money) t where rownum < (页数*条数+1)) where r > (上一页数*条数)； 
>在 xml中不识别<应用实体符代替

#####2 [数据的完整性约束](https://zhidao.baidu.com/question/1736191690142671347.html)
	1. 基本概念
		为了避免以后向表中插入一些错误或不符合规范的数据，就需要程序员在设计表时建立表中数据的完整性约束。
		完整性主要包括四大类：域完整性、实体完整性、自定义完整性、引用完整性。
			实体完整性：
				实体完整性是指关系的主关键字不能重复也不能取空值， 
			域完整性：
				域完整性是保证数据库字段取值的合理性。包括限制类型(数据类型),格式(通过检查约束和规则),可能值范围(通过外键约束,检查约束,默认值定义,非空约束和规则)，在当今的关系DBMS中，一般都有域完整性约束检查功能。
			参照完整性：
				也称“引用完整性”，是定义建立关系之间联系的主关键字与外部关键字引用的约束条件。在删除和输入记录时,引用完整性保持表之间已定义的关系.引用完整性确保键值在所有表中一致.这样的一致辞性要求不能引用不存在的值.如果一个键值更改了,那么在整个数据库中,对该键值的引用要进行一致的更改.
			用户自定义完整性：
				实体完整性和参照完整性适用于任何关系型数据库系统，它主要是针对关系的主关键字和外部关键字取值必须有效而做出的约束。用户定义完整性则是根据应用环境 的要求和实际的需要，对某一具体应用所涉及的数据提出约束性条件。这一约束机制一般不应由应用程序提供，而应有由关系模型提供定义并检验，用户定义完整性 主要包括字段有效性约束和记录有效性。

	2. 主键约束
		当创建表时指定主键约束可以确保该字段（列）中的数据唯一并且不能为空。
			id number constraint pk_id   primary key
	3. 唯一约束
		当创建表时指定唯一约束则可以确保该字段（列）中的数据唯一但可以为空。
			name varchar2(30) constraint uq_name unique 
	4. 非空约束
		当创建表时指定非空约束则可以确保该字段（列）的数据不能为空
			sex char(3) constraint nn_sex not null 
	5，检查约束
		当创建表时指定检查约束则可以确保该字段（列）中的数据不能超过取值范围和格式限制
			-- 相当于给字段salary加入检查约束               约束说明 
				salary number(11, 2) constraint ck_salary check(salary >= 2200)
			-- 此时应该报错，违反检查约束
				insert into worker values(3, '刘备', '男', 2000);
	6. 表级约束
		当创建表时写完所有的字段列表后再写约束规则的形式，就叫做标记约束。

			constraint pk_id   primary key(id),
			constraint uq_name unique(name),
			-- constraint nn_sex not null(sex),  非空没有表级约束
			constraint ck_salary check(salary >= 2200)
	7. 创建表后约束的控制
		当一个表已经创建完毕后，若希望添加或者删除约束时的处理方式如下：
			--通过修改表结构的方式来添加约束，注意表格不要有数据
				alter table worker add constraint pk_id primary key(id);
			--通过修改表结构的方式来删除约束
				alter table worker drop cosntraint pk_id;

****
****
###12/5/2019 10:18:57 PM 
####约束
	1. 外键约束
		当本表中某一字段的数值取决于另外一张表的主键时，此字段就需要加上外键约束。也就是依赖于外部的主键，当前表叫做子表，所依赖的表叫作主表
			列级约束：
				userid number constraint fk_userid references user1(id)
			表级约束：
				constraint fk_userid foreign key(userid) references user1(id)
		
		删除表的方法(被依赖的主表不好删除):
			1. 将子表order1中依赖该账户编号的记录删除，然后再删除主表user1中的记录
				delete from order1 where userid = 1;
				delete from user1 where id = 1;
			2. 将子表order1中依赖该账户编号的记录置空，然后再删除主表user1中的记录
				update order1 set userid = null where userid = 1;
				delete from user1 where id = 1;
			3. 直接将子表order1整个删除，再删除主表user1
				drop table order1;
				drop table user1;
			4. 删除主表时取消表之间的关联关系
				drop table user1 cascade constraint;

		在创建时就做好被删除的准备
			userid number constraint fk_userid references user1(id) on delete set null
			
			userid number constraint fk_userid references user1 (id) on delete cascade

####事务
	1. 事务的由来
		使用图形化工具向表中插入数据，此插入操作实际上只是把数据放到内存中
		没有进行提交时，命令行工具无法进行插入操作，因为图形化工具的流程没有走完
		类似于数据库被锁住，直到图形化工具的流程走完才能解锁
		这个图形化工具执行的流程 叫作 一个事务
		使用commit实现数据的提交，才会把数据有内存插入到数据库的表空间中
		
		例子：
			begin
				-- 模拟转账操作，实现1号账户向2号账户转账50元
				-- 当1号账户余额不足时导致第一步操作失败
					update bank set money = money-50 where id = 1;
				-- 虽然上一步失败了，但第二步缺执行成功，此时与现实生活不符
				-- 真正希望的是：只要有一个失败则两个都失败，只有两个都成功结果才成功
					update bank set money = money+50 where id = 2;
					commit; -- 提交，一起成功
  
					exception -- 相当于Java的catch
 					when others then
					rollback; -- 回滚，一起失败
					end;
	
####视图
	1 视图的由来
		对于同一张表中数据来说，不同的人群可能关注不同的数据，若将不同的数据放在不同的表中会造成表空间的浪费，此时可以使用视图来满足该需求。
		视图本质上就是用于展示单个表中部分字段或者多个表中综合字段的虚拟表。
	2 视图的创建，查询和删除
		视图创建的语法格式
			create view vw_视图名 as select查询语句

			例子：
				--创建一个男同学关心的视图：所有女同学的名字和电话
					create view vw_girls as select studentname 姓名，phone 电话号码 from student where sex = '女';

		视图查询的语法格式：
			select * from 视图名；
				
			例子：
				--查询上述视图中的所有数据
					select * from vw_girls
		
		删除视图的语法格式：
			drop view vw_视图名；
			
			例子：
				drop view vw_girls


####序列

	有序的一列数字 , 一般用于对表格id字段的数据插入. 每次使用可以控制其自增, 就像Java中的 i++;

	语法格式:
		 -	创建序列:
				注意:	序列名称的创建规范:  seq_表名_字段名
				格式1: create sequence 序列名; ***
				格式2: create sequence 序列名
				 	   start with 数字--序列的开始数值
					   increment by 数字--序列每次自增的值
					   maxvalue 数字--最大值
						;

		 -	使用序列
				-	使用不自增:
						序列名.currval;
				-	使用并自增:
						序列名.nextval;
		-	删除序列:
				-	drop sequence 序列名;

	案例:
		create sequence seq_user34_id;
		insert into user34 values(seq_user34_id.nextval,'天海马老师','123456');
		insert into user34 values(seq_user34_id.nextval,'小泽马老师','123456');
		insert into user34 values(seq_user34_id.nextval,'加藤马老师','123456');
		commit;
		
####索引

	索引是数据库 通过 消耗大量的时间 和 控件 来达到加速查询的目的的一种技术.
	当我们创建的表格 存在主键 ! 则数据库会自动按照这个字段 为 表格建立索引.  索引技术在查询时, 由数据库自动使用.

	语法:
		创建:
			索引命名规范: index_表名_字段名;

			create index 索引名称 on 表名(字段名);
		删除:
			drop index 索引名称

###表格设计三大范式 

	第一范式:	确保表格中每一列的不可分割特性(原子性).
	第二范式:	表格中必须存在主键, 表中除了主键以外的字段 都必须完全依赖主键.	
	第三范式:	确保每一列都和主键列 直接相关 而不是间接相关 (消除间接依赖关系)
	 