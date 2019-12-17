#Jquery 02

2019/9/21 10:17:48

###样式函数

	作用:	用于修改样式 

	格式1.
			一次修改单个样式:
			$obj.css("样式名","样式值");
	格式2.
			一次修改一组样式:
			$obj.css(对象);

###显示隐藏函数 掌握

	-	基本显示隐藏函数
			-	显示:	$obj.show();
			-	隐藏:	$obj.hide();
			-	切换:	$obj.toggle();

	-	左上折叠淡入淡出函数
			-	显示:	$obj.show(毫秒);
			-	隐藏:	$obj.hide(毫秒);
			-	切换:	$obj.toggle(毫秒);


	-	上下折叠显示隐藏函数
			上下折叠的原理是: 周期性定时器 ,持续性的更改元素的高度.
			当我们将上下折叠隐藏显示函数 给到img标签时,  在函数执行 更改img高度时, 图片会自动调整自身的宽度,呈现的效果与左上折叠一致.
			解决方案:	给图片设置宽度, 不让宽度跟随高度等比变化即可.
			-	显示:	$obj.slideDown();
			-	隐藏:	$obj.slideUp();
			-	切换:	$obj.slideToggle();

	-	淡入淡出函数
			-	淡入:	$obj.fadeIn(毫秒);
			-	淡出:	$obj.fadeOut(毫秒);

###动画函数 了解

	格式:
		$obj.animate(style,time,[function]);

	参数:
		1.	对象, 描述的是目标样式, 格式与CSS函数中传递的对象一致.
		2.	time, 从当前样式 到动画的目标样式, 所需时长.
		3.	function: 当动画执行完毕后 , 回调的函数.

###Jquery事件
	JS的事件, 需要编写在元素的 onXXX的属性中,  会导致视图代码HTML 与 逻辑代码JS 混合, 不利于后期的扩展和维护.
	
	Jquery事件的使用方式, 可以使HTML代码 与 JS代码分离. 利于后期的扩展和维护 !

####事件的绑定与解绑*
	
	格式:
		-	绑定事件:(注意:	一个元素可以绑定多个同类事件.)
				$obj.bind("事件类型",function);
		-	解绑事件:(无论绑定了多少个此类事件, 一次解绑全部解除)
				$obj.unbind("事件类型");
		-	模拟触发事件
				$obj.trigger("事件类型");

			setInterval(function(){
				$("button:eq(0)").trigger("click");
			},10);

####事件函数 *

	格式:
		$obj.事件类型(函数); 

	例如 给一个元素添加点击事件:
		$obj.click(function(){

		});

####组合事件函数 hover
	
	格式:
		$obj.hover(in,out);

		参数1.	in	:	当鼠标移入时, 执行的函数.
		参数2.	out	:	当鼠标移出时, 执行的函数.


####上述的格式中 出现的事件类型指的是:

	是通过字符串来描述的  事件类型 . 
	例如: onclick事件, 它的事件类型是 "click"
	例如: onload事件 , 它的事件类型是 "load"


###文档操作

####文档查找函数 掌握

	根据一个已经存在的jquery对象, 查找与其相关的其他Jquery对象.

	格式:
		-	查找子元素
				$obj.children("选择器");
		-	查找后代元素 ***
				$obj.find("选择器"); 
		-	查找下一个兄弟元素
				$obj.next();
		-	查找上一个兄弟元素
				$obj.prev();
		-	查找父元素 
				$obj.parent(); ***
		-	查找祖先元素
				$obj.parents("选择器");
	
####文档处理函数 掌握
	
	-	创建元素
			格式:
				var $obj = $("html标签字符串");
			例如创建一个button
				var $obj = $("<button></button>");
	-	添加元素
			-	向父元素内部 追加子元素 ***
					格式:	$父元素.append($新的子元素); 
			-	向父元素内部 前置子元素
					格式:	$父元素.prepend($新的子元素);
			-	向某个元素后面,  加入兄弟元素
					格式:	$obj.after($新元素);
			-	向某个元素前面,  加入兄弟元素
					格式:	$obj.before($新元素);

	-	删除元素 ***
			格式:	$obj.remove();

	-	清空元素
			格式:	$obj.empty();

	-	克隆元素
			忽略事件克隆 
				格式:	
					var $新对象 = $obj.clone();

			携带事件克隆
				格式:	
					var $新对象 = $obj.clone(true);


###Jquery插件

####放大镜插件 *

	使用步骤:
		1.	引入jquery
		2.	引入插件的.js文件
		3.	在网页中编写一个img标签, src指向小图片地址 , 并给img标签添加id属性.
		4.	创建一个放大镜的规格对象
				var obj = {
					imgSrc:"大图片地址",
					lensShape:"形状",//circle圆形 square方形
					lensSize:数字,//放大镜的直径
					borderColor:"#rgb",//边框颜色
					borderRaduis:数字//填写数字,指的是边框圆角宽度. 不支持百分比
				};
		5.	开启放大镜, 将规格对象传入
				$("#img的id").mlens(obj);


####二维码(QRCODE)生成 *

	二维码中存储的数据不是绝对能识别的,
	
	二维码存在纠错级别 ,级别越高 .越难识别!级别越高 , 内容识别时越准确.
	
		四个级别:
			L	:	7%;
			M	:	15%
			Q	:	25%
			H	:	30%

	使用步骤:
		1.	引入jquery
		2.	引入插件的.js文件
		3.	在需要展示二维码的位置, 定义一个div , 并指定id
		4.	创建一个二维码的规格对象
				//简单的规格对象
				var obj = {
					width:数字,//表示二维码占用的宽度, 默认是px
					height:数字,
					text:"二维码中的内容"
				};

				//完整的规格对象
				var obj = {
					width:数字,//表示二维码占用的宽度, 默认是px
					height:数字,
					text:"二维码中的内容",
					correctLevel:数字/可选值1/2/3/4 ,表示二维码的纠错级别
					background:"#rrggbb",//默认白色
					foreground:"#rrggbb",//默认黑色
					render:"绘制模式"//table|canvas , table兼容性,支持旧浏览器.	canvas性能更高,需要新浏览器才能支持.
				};
		5.	在div中按照规格对象, 显示二维码
				$("#div的id").qrcode(obj);