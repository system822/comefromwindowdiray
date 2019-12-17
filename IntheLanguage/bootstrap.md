#layer & Bootstrap

2019/9/23 8:36:02 


----

###补Jquery插件:

####layer弹出层插件:

#####步骤:
	
	1.	引入Jquery
	2.	引入插件的js文件 (layer.js)
		(layer.js文件 依赖了一些其他文件, 所以下载的整个文件夹 都需要引入进去)

#####layer - msg函数

	用于弹出信息提示窗 , 提示窗在屏幕上会自动淡出.

	格式1:
		layer.msg("文字");

	格式2: 信息提示窗口 会抖动显示.
		layer.msg("文字",function(){
			//当弹出的文字消失时, 执行
		});

#####layer - load 函数

	格式1.	弹出loading窗口 , 需要执行close来关闭
			-	弹出 , 并得到弹窗的索引值
					var index = layer.load(0/1/2);
			-	关闭
					layer.close(index);

	格式2.	弹出loading窗口 , 等待指定时长后自动关闭,  也可以执行close来提前关闭
			-	弹出 , 并得到弹窗的索引值
					var index = layer.load(0/1/2,{"time":自动关闭毫秒});
			-	关闭
					layer.close(index);
				
#####layer - msg函数弹出load效果

	格式:
		-	弹出
				var index = layer.msg("文字",{icon:16});
		-	关闭
				layer.close(index);

#####layer - alert函数

	格式:
		layer.alert("文本内容",{icon:0-16}); // 0-16指的是图片的序号


#####layer - tips 函数

	格式:
		layer.tips("文本内容","选择器",{tipsMore:true,tips:数字值});
			tipsMore:	true表示允许同时弹出多个tips
			tips	:	数字值, 1/2/3/4 分别表示弹出的方向 上 右 下 左

#####关闭所有的layer弹出窗口

	layer.closeAll();

##Bootstrap 熟悉

###简介 

	Bootstrap 是由 Twitter 公司的两名前端程序员研发的.
	是一套基于HTML / CSS  /JS 技术的前端开源框架.

###使用步骤:
		
	1.	下载bootstrap
	2.	解压下载的文件, 解压后将如下三个文件夹 复制到webContent目录中
			-	css	:	样式文件
			-	js	:	脚本文件
			-	fonts:	字体文件

	3.	在HTML代码中, 引入Jquery.js
			<script src="js/jquery.min.js"></script>
	4.	引入js/bootstrap.min.js
			<script src="js/bootstrap.min.js"></script>
	5.	引入css/bootstrap.css文件
			<link rel="stylesheet" type="text/css" href="css/bootstrap.css">


	注意:
		bootstrap引入后 会对网页中的所有元素 进行样式的重构.
		(因为不同的浏览器拥有自己的默认样式 , 为了在不同的浏览器中呈现的效果一致, bootstrap将各自浏览器不同的部分进行了统一.)

##常用class值

###文本对齐方式
	
	class:
		-	text-left		:	内容居左
		-	text-center		:	内容居中
		-	text-right		:	内容居右

###列表样式
	应用到列表标签中.

	class:
		-	list-unstyled	:	取消前置文字或图标 ,并取消左内边距
		-	list-inline		:	取消前置文字或图标 ,并将所有li设置为行内元素


###代码块样式
	原样显示块:	使用pre标签
	bootstrap代码块:	给div添加class="well"

###前景后景色

	前景色(文字颜色)
			class:
				-	text-muted	:	柔和灰
				-	text-info 	:	信息蓝
				-	text-primary:	主要蓝
				-	text-success:	成功绿
				-	text-warning:	警告黄
				-	text-danger	:	危险红
			
	后景色(背景颜色)
			class:
				-	bg-info		:	信息蓝
				-	bg-primary	:	主要蓝 (因为蓝色较深, 所以会自动设置文字颜色为白色.)
				-	bg-success	:	成功绿
				-	bg-warning	:	警告黄
				-	bg-danger	:	危险红


###表格样式

####基本表格样式
	class="table"

####条纹表格样式
	class="table table-striped"

####边框表格样式
	class="table table-bordered"

####鼠标悬停激活 表格样式
	class="table table-hover"

	
####单独指定tr的样式

	class:
		-	active	:	激活样式, 半透明灰色样式.
		-	success	:	成功绿
		-	info	:	信息蓝
		-	warning	:	警告黄
		-	danger	:	危险红
		-	sr-only	:	不显示

###文字图标

	使用步骤:
	
		1.	编写span标签
		2.	设置span标签的class值等于对应的图标class即可

	图标的class参考:	https://v3.bootcss.com/components/

###按钮样式

####按钮颜色样式

	class	:
		-	btn btn-default	:	bootstrap默认按钮
		-	btn btn-success	:	成功绿按钮
		-	btn btn-info	:	信息蓝按钮
		-	btn btn-primary	:	主要蓝按钮
		-	btn btn-warning	:	警告黄按钮
		-	btn btn-danger	:	危险红按钮
		-	btn btn-link	:	超链接样式

	class值可以给到如下几种元素:
			1.	span标签 (推荐)
			2.	button标签
			3.	超链接a标签
			4.	input标签type是按钮效果时

####按钮尺寸样式

	class:
		-	btn-lg	:	大按钮
		-	btn-sm	:	小按钮
		-	btn-xs	:	超小按钮
		-	btn-block	:	块按钮, 独占一行 ,常用于手机.

#### 按钮的激活与禁用 (仅视觉样式)

	class	:
		-	active	:	激活
		-	disabled:	禁用

###输入框样式

####块输入框
	
	class	:	form-control

####内联输入框
	
	响应式输入框, 当设备的宽度小于768px时, 自动切换为块输入框.

	步骤:
		1.	给form标签设置class="form-inline"
		2.	给input标签设置class="form-control"

####输入框尺寸

	class	:
			-	input-lg	:	大输入框
			-	input-sm	:	小输入框


###HTML5 移动设备优先
	
	当手机浏览网页时, 自适应内容大小.
	在head中加入子标签:

		自适应:
		<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1">
		
		自适应+禁用缩放
		<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">


##响应式 * 

####屏幕的规格:

	-	超小屏幕 :	屏幕宽度<768px . (设置移动设备优先后 . 手机页面通常宽度<768px)
	-	小屏幕	:	768≤屏幕宽度<992px . (平板电脑的屏幕 或 部分显示设备较低的笔记本)
	-	中等屏幕	:	992≤屏幕宽度<1200px  (显示器分辨率较低的 电脑屏幕)
	-	大屏幕	:	屏幕宽度≥1200px (电脑屏幕, 电视屏幕)
		
#### 栅格系统

	栅格系统, 将屏幕横向等分为12等份, 我们设置元素宽度时, 通过栅格系统指定占用的份数即可.
	
	我们可以通过bootstrap 指定样式 ,在多种屏幕尺寸下的 占用的栅格份数.

#####栅格容器
	
	1.	固定宽度容器
			容器宽度:	超小屏幕(100%)	小屏幕(750px)	中等屏幕(970px)	大屏幕(1170px)
			
			格式:
				给div 添加class="container"
	2.	撑满父元素宽度的容器
			容器宽度:	所有屏幕尺寸下 100%
			
			格式:
				给div 添加class="container-fluid"

#####栅格行
	
	栅格行, 编写在栅格容器中.

	栅格容器中的元素, 必须编写在栅格行中, 栅格行元素 一行可以分为12份, 当分的份数大于12时, 换行显示.
	
	格式:
		给div添加class="row"

#####指定元素占用的栅格份数

	每种屏幕尺寸下 有12个不同的class 来表示占用的份数.   共有4中屏幕尺寸, 所以共48个class:
	
	为了便于记忆, 这48个class是有规律的 , 每种屏幕尺寸下的 12个class的前缀都是相同的.

	格式:
		屏幕尺寸	:	超小屏幕			小屏幕				中等屏幕				大屏幕
		类前缀:		col-xs-数字		col-sm-数字			col-md-数字			col-lg-数字

	注意:
		当较大的屏幕尺寸为指定占用分数时.  会沿用较小屏幕尺寸的配置.
		例如: 我们一个栅格行中的元素, 只指定了超小屏幕下的栅格配置.其他屏幕规格为指定.  则其他屏幕规格按照超小屏幕的配置显示.

	例如:在一个栅格中 , 指定两个div ,分别占用一半的宽度

		<div class="container">
			<div class="row">
				<div class="col-xs-6"></div>
				<div class="col-xs-6"></div>
				<div class="col-xs-6"></div>
				<div class="col-xs-6"></div>
			</div>
		</div>

#### 显示隐藏

	屏幕尺寸:		超小屏幕		小屏幕		中等屏幕			大屏幕
	class值
	
	visible-xs-参数		显示			隐藏			隐藏			隐藏
	visible-sm-参数		隐藏			显示			隐藏			隐藏
	visible-md-参数		隐藏			隐藏			显示			隐藏
	visible-lg-参数		隐藏			隐藏			隐藏			显示

	hidden-xs			隐藏			显示			显示			显示
	hidden-sm			显示			隐藏			显示			显示
	hidden-md			显示			显示			隐藏			显示
	hidden-lg			显示			显示			显示			隐藏

	上述的参数 指的是显示时, 以什么样的元素分类显示, 参数值可选:inline/inline-block/block
	

###导航栏

	步骤:
		1.	编写ul标签, 并指定ul的class="nav nav-tabs"
		2.	给ul添加子标签li , li的内容必须包含在超链接里面.

	案例:
		<ul class="nav nav-tabs">
			<li><a href="#">首页</a></li>
			<li><a href="#">欧美</a></li>
			<li><a href="#">日韩</a></li>
			<li><a href="#">国内</a></li>
			<li><a href="#">无码</a></li>
			<li><a href="#">国语</a></li>
			<li><a href="#">动漫</a></li>
			<li><a href="#">MR</a></li>
			<li><a href="#">更多</a></li>
		</ul>

###导航栏+内容切换

	步骤:
		1.	编写一个导航栏
		2.	在网页中 编写一个div ,用于存储要显示的页面内容. div的class="tab-content"
		3.	向上述的div中, 加入多个子div ,并给每个子div添加id属性值.(数量与导航的li数量一致, 每一个div绑定一个li,当点击li时.显示此div的内容.)
		4.	给上述的子div ,添加class="tab-pane fade" , 默认显示的div的class="tab-pane fade in active"
		5.	修改导航栏中的每一个a标签, 添加属性: data-toggle="tab" href="#子div的id"
		6.	给默认选中的li标签, 设置class="active"
	
	案例:
		<ul class="nav nav-tabs">
			<li class="active"><a data-toggle="tab" href="#div1">首页</a></li>
			<li><a data-toggle="tab" href="#div2">欧美</a></li>
			<li><a data-toggle="tab" href="#div3">日韩</a></li>
			<li><a data-toggle="tab" href="#div4">动漫</a></li>
			<li><a data-toggle="tab" href="#div5">非洲大**</a></li>
			<li><a data-toggle="tab" href="#div6">更多</a></li>
		</ul>
		<div class="tab-content">
			<div id="div1" class="tab-pane fade in active">
				<div class="container">
					<h1>PHP在线教学网站</h1>
					<p>这个网站, 18岁以下禁止观看! </p>
				</div>
			</div>
			<div id="div2" class="tab-pane fade">
				<img src="images/17.jpg">
			</div>
			<div id="div3" class="tab-pane fade">
				<img src="images/18.jpg">
			</div>
			<div id="div4" class="tab-pane fade">
				<img src="images/19.jpg">
			</div>
			<div id="div5" class="tab-pane fade">
				<img src="images/20.jpg">
			</div>
			<div id="div6" class="tab-pane fade">
				<div>
					更多资源, 请<a href="https://shuidianshuisb.com">开通svip</a>
				</div>
			</div>
		</div>