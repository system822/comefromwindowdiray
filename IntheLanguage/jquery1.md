#JQuery

2019/9/20 9:05:13 

------

###介绍

	Jquery是一个快速的 简单的 javascript框架 , 是一个Javascript的代码库

	宗旨:	写较少的代码, 做更多的事情

###使用步骤 :
	
	使用步骤, 就是将Jquery的js文件, 引入到我们的网页中, 然后使用它

		1.	下载jquery.js
				开发版本:	https://cdn.bootcss.com/jquery/3.4.0/jquery.js
				生产版本:	https://cdn.bootcss.com/jquery/3.4.0/jquery.min.js
		2.	引入到html中
				<script src="jquery.js"></script>

	工作时, 更建议引入知名CDN的框架文件.

		<script src="https://cdn.bootcss.com/jquery/3.4.0/jquery.js"></script>

###onload 使用

	格式:
		$(function(){
			
		});

###Jquery对象

	我们之前在JS中 查找到的是DOM对象 , 通过DOM对象修改属性来完成网页内容的设置.
	在Jquery中, jquery对dom对象进行了再封装. 可以简单的将Jquery对象 看作dom对象的集合!
###DOM对象与Jquery对象的转换
	强调:
		Jquery对象不是DOM对象 , 所以DOM对象原有的操作 Jquery对象是不能用的.
		例如: 通过jquery对象.innerHTML 无法修改任何东西, 因为它没有这个属性.

	DOM对象转换为JQUERY对象的格式:
		var $obj = $(dom对象);

	Jquery对象转换为DOM对象
		var dom = $obj.get(下标);

###查找Jquery对象

	在Jquery中查找Jquery对象 是通过选择器来完成的. (CSS中我们学习过的选择器, 在这里都能使用)

###选择器使用格式

	格式:
		var $obj = $("选择器");

###基本选择器
	
	1.	id选择器	:	var $obj = $("#id值");
	2.	类选择器	:	var $obj = $(".class值");
	3.	元素名称选择器 	:	var $obj = $("元素名称");


###筛选选择器

####基本筛选选择器 掌握

	格式:
		1.	$("选择器:first")	:	选择匹配的第一个元素 , 例如选择第一个class值为c1的元素: $(".c1:first")
		2.	$("选择器:last")		:	选择匹配的最后一个元素
		3.	$("选择器:eq(下标)") :	选择匹配的 指定下标的元素   *** 
		4.	$("选择器:gt(下标)")	:	选择匹配的 大于指定下标的元素
		5.	$("选择器:lt(下标)") :	选择匹配的 小于指定下标的元素
		6.	$("选择器:even")		:	选择匹配选择器的 偶数下标的元素
		7.	$("选择器:odd")		:	选择匹配选择器的 奇数下标的元素
	

####内容筛选选择器 了解
	-	筛选内容是否包含:
			格式:	$("选择器:contains('包含的文字')")	


####排除筛选选择器 了解
	格式:
		$("选择器1:not(选择器2)")	:	从选择器1的结果中 排除掉选择器2的结果

####可见性筛选选择器 了解
	可见筛选选择器:
		$("选择器:visible")
	不可见筛选选择器:
		$("选择器:hidden")

	什么样的元素 , 算是不可见的:
		1.	display	:	none
		2.	opaticy	:	0
		3.	input标签type=hidden
		4.	宽度或高度为0

	案例:
		
			<style type="text/css">
			.nickname{
				display: inline-block;
				font-size:14px;
			}
			span.nickname{
				border:2px solid rgba(0,0,0,0);
			}
		</style>
		<script type="text/javascript" src="js/jquery.min.js"></script>
		<script type="text/javascript">
			function change(){
				//找到显示的div
				var $visible = $("div:visible");
				//找到隐藏的div
				var $hidden = $("div:hidden");
				//将显示的变隐藏
				$visible.fadeOut(0);
				//将隐藏的变显示
				$hidden.fadeIn(0);
			}
			
			function save(){
				change();
				//获取输入框的内容
				var val = $("input").get(0).value;
				//将输入框的内容 设置到span上
				$("span").get(0).innerHTML = val;
			}
		</script>
		</head>
		<body>
			<div style="display: none;">
				<input class="nickname" value="用户昵称" onblur="save()">
			</div>
			<div>
				<span class="nickname" title="双击修改" ondblclick="change()">用户昵称</span>
			</div>
		</body>


####属性筛选选择器 了解

	-	筛选存在属性
			$("选择器[属性名]")
	
	-	筛选属性值等于某个值
			$("选择器[属性名='值']")
		
	-	筛选属性值不等于某个值
			$("选择器[属性名!='值']")
####表单筛选选择器 了解

	筛选input标签的type属性值
	
	格式1.	筛选网页中所有的input标签
		$(":type属性值")
	
	格式2.	筛选匹配选择器的  input标签
		$("input:type属性值")


###修改属性

####正常的属性操作

	格式1.
		用于一次操作一个属性
			设置属性值:	$obj.attr("属性名","属性值");

			获取属性值:	var 属性值 = $obj.attr("属性名");
	格式2.
		用于一次操作一组属性 (仅仅用于设置属性值)
			$obj.attr(对象);
			参数是一个对象, 对象中可以包含多个属性, 对象中的属性会被设置到Jquery对象中.

	例如:
		给div 添加宽度和高度属性:
			$("div").attr({"width":"200px","height":"200px"});

		给img标签设置src和宽度值:
			$("img").attr({"src":"xxx.jpg","width":"300px"});

		给img标签设置title属性值
			$("img").attr("title","文字内容");
####特殊属性操作

#####class 属性 熟悉
	
	格式:
		-	添加class
				$obj.addClass("值");
		-	删除class	
				$obj.removeClass("值");
		-	替换class (值存在则删除, 值不存在则添加)
				$obj.toggleClass("值");

#####value 属性 *
	因为Jquery对象是网页加载完毕时 就创建好等待我们查询的.  所以输入框的Jquery对象的value值 是默认值.
	通过attr函数 获取的输入框值, 一直都是默认值.

	Jquery 提供了一个函数 val , 来操作value属性值

	设置值:
		$input.val("值");

	获取值:
		var value = $input.val();

#####innerHTML 属性

	格式1.
		使用html函数, 操作元素的内容

		设置内容:
			$obj.html("内容");
		获取内容:		
			var 内容 = $obj.html();

	格式2.
		使用text函数, 操作元素的内容
		
		设置内容:
			$obj.text("内容");
		获取内容:		
			var 内容 = $obj.text();

#####HTML函数与Text函数的区别 *****
	
	html函数 与 text函数 作用相同, 都是用于设置与获取元素的内容.

	区别在于:
		html函数在获取内容时,  会得到html标签部分.
		text函数在获取内容时,  会忽略html标签部分

	例如:
		<div>从前<span>又有</span>一座山</div>
		$("div").html()		:	从前<span>又有</span>一座山
		$("div").text()		:	从前又有一座山

###为了便于演示Jquery对象 :

	-	淡入淡出动画效果
			淡入:	jquery对象.fadeIn(毫秒);
			淡出:	jquery对象.fadeOut(毫秒);

	-	CSS函数 ,用于修改元素的样式:
			格式:	$obj.css(对象);
			传递的参数,是一个对象, 对象中可以包含多个属性 , 属性名就是样式名, 属性值就是样式值;

			例如:	设置id为div1的元素 ,宽度200px ,高度200px:
					$("#div1").css({"width":"200px","height":"200px"});
					或者:
					var haha = new Object();
					haha.width = "200px";
					haha.height = "200px";
					$("#div1").css(haha);


###作业

	仿京东主页, 完成轮播图组件.
		核心:	找到img标签, 更改它的src

		复习SE	:
				1.	IO流	(打印流)
				2.	多线程(线程安全问题 : 锁的概念)
				3.	集合(Map集合,List集合)



	
