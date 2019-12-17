#CSS 02

2019/9/16 8:37:47 

----

####表格边框双线问题:

	当我们需要给表格添加分割线时,  需要给table , th , td 添加边框.  添加完毕效果是双线边框.
	
	合并双线边框:
		给table添加样式:
		border-collapse:collapse;

###CSS 盒模型 ( 框模型 ) ***

	指的是元素在网页中占用空间大小的 计算模型.

		占用的宽度:
			自身宽度+左右边框宽度+左右内边距的宽度+左右外边距的宽度;

		占用的高度:
			自身高度+上下边框高度+上线内边距的高度+上下外边距的高度;

####内边距 padding ***
	指的就是元素的内容 与 元素的边框的距离.

	格式1.	(一次指定四个方向的内边距) ***
			padding:长度+单位;
	
	格式2.	通过一个样式, 分别指定上下 和 左右的内边距.
			padding:上下内边距 左右内边距;

	格式3.	通过一个样式, 分别指定 上, 右, 下, 左内边距
			padding:上 右 下 左;

	格式4.	单独指定每一个方向的内边距
			左:padding-left:长度+单位;
			右:padding-right:长度+单位;
			上:padding-top:长度+单位;
			下:padding-bottom:长度+单位;

####外边距 margin ***
	指的是元素的边框 与 其他元素的距离. 值可以是负数.

	格式1.	(一次指定四个方向的外边距) ***
			margin:长度+单位;
	
	格式2.	通过一个样式, 分别指定上下 和 左右的外边距.
			margin:上下外边距 左右外边距;

	格式3.	通过一个样式, 分别指定 上, 右, 下, 左外边距
			margin:上 右 下 左;

	格式4.	单独指定每一个方向的外边距
			左:margin-left:长度+单位;
			右:margin-right:长度+单位;
			上:margin-top:长度+单位;
			下:margin-bottom:长度+单位;
#####外边距特殊使用:
	1.	值可以是负数
	2.	两个块元素之间的上下外边距不会叠加. 值较大者生效.
	3.	可以设置左右外边距值为 auto , 来实现: 自动居中.

####元素与元素之间的空白 如何消除

	我们写HTML时 . 为了代码利于阅读, 经常会换行 ,缩进等等
	这些换行, 缩进 会被显示为一个空白字符, 

	常见处理方式:

		方式1.	编写标签时, 不使用换行与缩进.  (不利于代码阅读)
		方式2.	空白字符是文字, 我们只需要调整文字大小为0, 空白字符就不显示了.
				(弊端:如果元素中存在文字, 需要单独给文字重新制定字体大小)


###元素垂直对齐 了解

	vertical-align: 
		-	top	:	对齐顶端
		-	text-top	:	对齐第一行文本顶端
		-	bottom	:	对齐底部
		-	text-bottom	:	对齐最后一行文本的底部


###鼠标形状 ***

	cursor	:	
			-	default	:	默认 , 跟随场景自动变化
			-	pointer	:	手指形状 ,常用于提示 可点击
			-	text	:	焦点形状(工字形)
			-	wait	:	等待
			-	help	:	帮助
			-	progress:	进行中


###列表样式 *

	list-style-type:	none;//取消前置数字或图标

###透明度 掌握

	opacity:0-1的浮点型数字;

	当值为1时, 不透明 
	当值为0.5时, 半透明
	当值为0时, 完全透明

###定位 ***

	用于控制元素在网页中显示的位置

####默认定位 (静态定位) *****

	默认情况下, 元素分为三类: 
		1.	块元素	:	独占一行, 可以设置宽度和高度, 例如: div,ul,li,p等等
		2.	行内元素	:	与其他元素共享一行, 一行排满自动换行排列, 宽度高度由内容撑开, 无法主动设置宽高. 例如: span,a,b,i等等
		3.	行内块元素 : 与其他元素共享一行, 一行排满自动换行排列, 可以设置宽度和高度,例如:img,input等等

	可以通过display样式, 来修改元素的默认定位.
		display:
			-	block	:	块元素
			-	inline	:	行内元素
			-	inline-block	:	行内块元素
			-	none	:	不显示元素;


####浮动定位 float *
	格式:
		float:left/right;
	作用:
		控制元素向父元素的左 或 向右浮动.
		浮动会脱离原有的定位方式 , 但是依然占用网页空间 !

	清除浮动:
		控制元素的某个方向, 不允许出现浮动:
			clear:left/right/both;
	

####相对定位 ****
	作用:	相对于自身当前位置, 进行x和y轴的偏移移动.
	格式:	position:relative;
	特点:	元素产生偏移后, 依然占用原有网页空间 偏移后采用覆盖显示.
####绝对定位 ****
	作用:	相对body的边框来确定自身的位置 , 或者 根据具备相对/绝对/固定定位的祖先元素边框 来确定自身位置;
	格式:	position:absolute;
	特点:	
			1.	不占用任何的网页空间, 采用覆盖显示.
			2.	基于body边框定位时, 在使用bottom和right定位时, 不会超出浏览器窗口.
####固定定位 ****
	作用:	根据浏览器窗口的边框, 来确定自身位置 , 不会因为浏览器内容的滑动而滑动.
	格式:	position:fixed;
	特点:	
			1.	不占用网页空间, 采用覆盖显示.
			2.	随着内容的滑动, 依然漂浮在浏览器固定的位置;
	

#####相对 / 绝对 / 固定 定位 确定元素位置的方式 ***

	这三种定位, 都是通过如下四种样式, 来确定元素位置的:
		
		-	left	:	长度+单位;
		-	top		:	长度+单位;
		-	right	:	长度+单位;
		-	bottom	:	长度+单位;

	在相对定位中:
		这四个样式, 表示元素相对自身位置 ,某个方向的偏移值!
		例如:	让元素从原有位置,向右移动10个像素:	left:10px; 或 right:-10px;


#####定位时的堆叠顺序 熟悉

	默认情况下, 元素越靠后, 显示越靠前.
	
	设置堆叠顺序:
		z-index:数字值;

	作用:
		默认值为0 , 值越大, 越靠前显示. 负数表示显示在内容后面.


###过渡 *

	在元素的样式变更时, 增加过渡时长, 让样式的更改更圆润.

	格式1:
			transition:样式 时长s;

	格式2:
			transition:all 时长s;


	
###转换 熟悉

####2D转换

	transform:
		-	位置移动:	translate(x,y); 控制元素横向纵向移动.
		-	旋转:	rotate(数字deg); 旋转指定度数
		-	缩放:	scale(x,y)'	缩放宽和高的倍数
		-	翻转:	skew(xdeg,ydeg) 根据x或y轴, 翻转指定度数

####3D旋转
	
	transform:
		-	X轴旋转:	rotateX(数字deg);
		-	Y轴旋转: rotateY(数字deg);

###动画 * 

	指的是元素在多个样式之间 平稳多度.

	格式:
		步骤1.	定义动画
			@keyframes 自定义动画名称{
				0%{
					样式列表;
				}
				...
				100%{
					样式列表;
				}
			}
		步骤2.	使用动画
			选择器{
				animation:动画名称 时长s;
				animation-iteration-count:数字;//重复的次数
			}