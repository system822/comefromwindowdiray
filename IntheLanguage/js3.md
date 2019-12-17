#JS 03

2019/9/19 9:02:14 

----

###历史记录 对象 history 了解

	函数:
		1.	back()	:	后退一页
		2.	forward():	前进一页
		3.	go(数字) : 传入正数表示前进指定页数, 传入负数表示后退指定页数.

### 地址栏 对象 location ***

	我们可以通过location 控制页面跳转, 刷新, 替换 等等.

	属性:
		href	:	用于设置与获取当前浏览器的地址栏内容, 设置新的地址栏内容 会导致 浏览器跳转.

	函数:
		reload()	:	刷新页面
		replace("地址")	:	替换页面

###JS事件 *

	事件:	指的是网页在 元素状态改变 / 鼠标操作 / 键盘按下与弹起 时 触发的动作.

	JS事件分为三大类:
		1.	状态事件
		2.	鼠标事件
		3.	键盘事件

###事件对象 event

	JS将产生的事件信息, 封装成了一个对象 event , 在事件触发的代码中,可以通过event调用相关的属性.

	属性:
		1.	clientX	:	鼠标事件时, 鼠标所在的x坐标	
		2.	clientY	:	鼠标事件时, 鼠标所在的y坐标
		3.	keyCode	:	键盘


	

###键盘事件
	-	onkeydown	:	当键盘按下 (按下不送开时, 按下事件会持续多次的触发)
	-	onkeyup		:	当键盘弹起

	使用方式:

		window.onkeydown = function(){}

		window.onkeyup = function(){}

	案例:
		<style type="text/css">
			img{
				position: fixed;
				left:0px;
				top:0px;
				z-index:1;
			}
			#bg{
				background-color: #0f0;
				position: fixed;
				left:0;
				right:0;
				top:0;
				bottom:0;
				z-index:2;
				opacity: 0.5;
			}
		</style>
		<script type="text/javascript">
			var left = 0;
			var top2 = 0;
			window.onkeydown = function(event){
				var code = event.keyCode;
				switch (code) {
				case 37://左
					left-=10;
					break;
				case 38://上
					top2-=10;
					break;
				case 39://右
					left+=10;
					break;
				case 40://下
					top2+=10;
					break;
				}
				console.log(left+","+top);
				img1.style="left:"+left+"px;top:"+top2+"px";
			}
		</script>
		</head>
		<body>
			<img id="img1" src="images/14.gif">
			<div id="bg"></div>
		</body>


###事件冒泡机制 了解
	
	HTML元素在触发鼠标事件时.  会依次激活父元素的同类事件.

	在事件触发的流程中(从子元素 到 父元素) ,我们可以在任意一个层次, 选择结束冒泡 事件不再传递给父元素.

		格式:
			在需要结束冒泡的位置:
			event.stopPropagation();

###事件源对象 了解

	在事件触发的代码中, 我们可以直接找到事件源头对象.

	格式:
		IE	:	event.srcElement;

		其他:	event.target;
	

###面向对象 了解 

	面向过程

	面向对象

	问:	把大象装进冰箱, 分几步? 
		面向过程:
			分为三步:
				1.	把冰箱门打开
				2.	把大象放进去
				3.	把冰箱门关上
		
		面向对象:
			两步:
				1.	创建一个智能冰箱 (开门 , 装东西 , 关门)
				2.	指挥冰箱 装大象.

### JS 中定义对象的三种方式

	JS中创建对象, 无需提前编写类.
	JS中创建的对象, 可以不包含任何的属性和函数
	当给属性赋值时, 如果属性不存在, 则创建属性并赋值.

	创建对象的格式1.
		var 对象名 = new Object();

		案例:
			var p = new Object();
			console.log("姓名:"+p.name+" , 年龄:"+p.age+" , 性别:"+p.sex);
			p.name = "旋转跳跃小泽马";
			p.age = 18;
			p.sex = "不详";
			console.log("姓名:"+p.name+" , 年龄:"+p.age+" , 性别:"+p.sex);


	创建对象的格式2.
		先创建对象模板, 通过new模板的方式 , 来完成对象的创建.
	
		定义模板格式:
			function 模板名称(形式参数列表){
				通过this给属性赋值;
			}
		使用模板格式:
			var 对象名 = new 模板名称(实际参数列表);

###第三种方式 JSON *****
	
	JavaScript Object Notation   JS对象简谱 , 是一种轻量级的数据交换格式.

	格式:
		对象由一个大括号来表示 , 
		括号中包含一个个的键值对 (每一个键值对 , 都是一个对象的属性 键是属性名, 值是属性值),
		键与值之间使用冒号连接,  多个键值对之间使用逗号分隔.
		键值对的键, 应使用引号引住.

	案例:
		创建一个人, 包含name,age,sex
			var p = {"name":"小泽马","age":18,"sex":"男"};
			console.log("姓名:"+p.name+" , 年龄:"+p.age+" , 性别:"+p.sex);

	注意:
			对象中可能存在一个属性, 这个属性值还是对象.
			对象中可能存在一个属性, 这个属性值是数组.

	案例:
		创建一个人
			var p = {
				"name":"张衡",
				"age":18,
				"sex":"不详",
				"xfe":{
					"name":"品如",
					"sex":"男",
					"wardrobe":[
						"冈蕾丝","低温蜡烛","眼罩","皮鞭","一盆风油精",{"name":"暗格","content":["老王","小王","大王"]}
					]
				}
			};
			console.log(p.xfe.wardrobe[5].content[0]);

		


###JSON格式字符串 转换为 JSON对象

	以后更多的时候, JS是接收服务器回复的JSON消息, 得到的是string类型的. 需要将其转换为Object , 取出数据.

	格式1.
		var 对象 = JSON.parse(string);

	格式2.
		var 对象 = eval('('+string+')');

	案例:
		<script type="text/javascript">
			var str = "{"
		        +"\"city\": \"天津\","
		        +"\"date_y\": \"2014年03月21日\","
		        +"\"week\": \"星期五\","
		        +"\"temperature\": \"8℃~20℃\","
		        +"\"weather\": \"晴转霾\","
		        +"\"weather_id\": {"
		        +"    \"fa\": \"00\","
		        +"    \"fb\": \"53\""
		        +"},"
		        +"\"wind\": \"西南风微风\","
		        +"\"dressing_index\": \"较冷\","
		        +"\"uv_index\": \"中等\","
		        +"\"comfort_index\": \"\","
		        +"\"wash_index\": \"较适宜\","
		        +"\"travel_index\": \"适宜\","
		        +"\"exercise_index\": \"较适宜\","
		        +"\"drying_index\": \"\""
		        +"}";
		      var obj = JSON.parse(str);
		      console.info(obj.weather);
		</script>





















		









