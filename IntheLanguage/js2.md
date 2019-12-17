#JS 02

2019/9/18 8:57:47 

----

###全局函数 

####URI编码 (URL编码) 熟悉

	将易发生乱码的中文文字 , 转换为英文字母+数字+英文符号的形式.

	格式:
		-	将文字转换为URI编码
				encodeURI("正常文字"); //返回的是URI编码的文字
		-	将URI编码文字, 解码为正常文字
				decodeURI(uri编码文字); //返回解码后的正常文字


###HTML DOM对象 ***

	HTML Document Object Model (HTML文档对象模型)

###通过文档对象 获取 元素对象的四种方式 *****

	1.	通过元素的id属性值, 查找一个元素对象
			var e = document.getElementById("id值");

	2.	通过元素的class属性值 ,查找一个元素对象数组
			var es = document.getElementsByClassName("class值");

	3.	通过元素的name属性值 , 查找一个元素对象数组
			var es = document.getElementsByName("name值");

	4.	通过元素的名称,  查找一个元素对象数组
			var es = document.getElementsByTagName("标签名称");

###操作元素的属性值: ****

	设置属性值:
		元素对象.属性名 = 值;
		例如: 修改一个图片的src属性:
			img对象.src="新的图片地址";

	获取属性值:
		var 属性值 = 元素对象.属性名;


###案例:

	<style type="text/css">
		body{
			margin: 0;
		}
		#content{
			color:#333;
			padding: 20px;
		}
		#content>div{
			padding: 30px;
			border:2px solid #aaa;
			border-radius: 15px;
		}
		#control{
			position: fixed;
			top:-32px;
			right:10px;
			transition:top 0.5s;
		}
		#control:hover{
			top:0px;
		}
	</style>
	<script type="text/javascript">
		//	用于记录当前是白天 还是夜间 . 默认值为true 表示当前为白天
		var flag = true;
		function change(){
			//先查找到图片 与 content
			var img = document.getElementById("control");
			var content = document.getElementById("content");
			if(flag){//变黑夜
				//1.	图片切换
				img.src = "images/太阳.png";
				//2.	图片的title 也要切换
				img.title = "点击开启白天模式";
				//3.	更改content的背景颜色
				//4.	更改content的字体颜色
				content.style="background-color:#333;color:#eee";
			}else{//变白天
				//1.	图片切换
				img.src = "images/月亮.png";
				//2.	图片的title 也要切换
				img.title = "点击开启夜间模式";
				//3.	更改content的背景颜色
				//4.	更改content的字体颜色
				content.style="";
			}
			flag = !flag;
		}
	</script>
	</head>
	<body>
		<img onclick="change()" title="点击开启夜间模式" id="control" src="images/月亮.png">
	      <div id="content">


###新版本  根据id查找元素

	新版本可以不必通过document.getElementById查找 , 可以直接通过id操作元素对象. id值 就是对象名称.

###this

	在任何元素的事件属性中, this都表示这个元素 .

###特殊属性操作

	1.	class属性 :	因为class是关键字, 不能作为属性名称存在.
					在JS中, 可以通过className属性, 来操作class属性.

	2.	标签内容	:	可以通过innerHTML属性 来操作开始标签与结束标签之间的部分.

### 文档中元素的创建 / 插入 / 删除 / 清空 掌握

####创建元素
	
	var e = document.createElement("节点名称");

####将新的元素, 加入到网页中

	-	向父元素内部, 追加子元素
			格式:	父元素.appendChild(新元素);


	-	向父元素内部, 指定位置插入子元素
			格式:	父元素.insertChild(新元素,已存在的参考元素);
			参考元素:	父元素中已存在的元素 ,用于做插入的位置参考, 新元素会插入到参考元素的前面.

####删除元素

	父元素.removeChild(子元素对象);

####清空元素
	
	元素对象.innerHTML = "";

###得到父元素

	元素.parentNode

###浏览器window对象

####打开新窗口
	var 新窗口对象 = window.open(url,[name],[config]);
		参数1.	url	:	新窗口加载的资源地址
		参数2.	name:	窗口的名称 , 是窗口的标识 , 相同name的窗口只能存在一个.
		参数3.	config	:	窗口的配置 ,由一个个的键值对组成, 键与值之间使用等号连接, 多个键值对之间使用逗号分隔.
							-	width:	长度+单位;
							-	height:	长度+单位;
							-	left  :	长度+单位;//指的是弹出的新窗口, 距离屏幕左边框的距离.
							-	top  :	长度+单位;//指的是弹出的新窗口, 距离屏幕左边框的距离.


####关闭窗口

	窗口对象.close();

	关闭当前窗口:
		window.close();

###定时器 ***

####一次性定时器

	作用:	指定延迟时长, 执行代码:
	格式:
			setTimeout(函数,毫秒时间);

####周期性定时器

	作用:	指定间隔的周期时长, 重复执行某段逻辑.
	
	-	开启定时器
			var 定时器序号 = setInterval(函数,毫秒时间);
	-	关闭定时器
			clearInterval(定时器序号);


###屏幕对象screen 了解

	用于获取屏幕的宽度和高度 , 用于判断手机 / PC / 电视 等等

	属性:
		screen.width	:	屏幕宽度
		screen.height	:	屏幕高度
		screen.availWidth	:	屏幕宽度
		screen.availHeight	:	屏幕高度


###控制台对象 console

	-	输出普通日志:
			console.log("文本");
	-	输出错误日志:
			console.error("文本");
	-	输出调试日志:
			console.debug("文本");
	-	输出提示日志:
			console.info("文本");
	-	输出警告日志:
			console.warn("文本");


	自定义样式, 输出日志:
		格式
			在输出的文本中, 使用%c占位,  在后续的参数中, 给%c依次填充样式即可
	
		例如:
			console.log("%c从前有%c座%c山","font-size:80px","color:red","font-size:80px");

