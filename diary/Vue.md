#Vue 熟悉

2019/9/24 8:40:52 

----

###简介

	是一套用于构建用户界面的渐进式框架 .
	只关注视图层的前端框架.

###使用步骤

	1.	下载vue.js
	2.	引入到html中
		<script type="text/javascript" src="js/vue.js"></script>
	3.	在页面的body中, 编写一个div , 建议给div设置id
		<div id="main"></div>
	4.	在div后, 创建一个vue对象, 并将对象绑定到div上.
		<script type="text/javascript">
			new Vue({
				el:"#main",//el:element
				template:"<h1>Hello Vue!</h1>"
			});
		</script>

>cn.vuejs.org
###Vue实例

	创建的vue对象, 与网页中的某个元素 进行了绑定!

	Vue对象在创建时, 需要传递一个对象, 传递的对象包含的属性与函数,最终会赋值给vue对象.
	
	
###Vue挂载点
	
	Vue对象会绑定一个元素, 又称这个元素为 Vue实例的挂载点.

###Vue模板
	
	vue对象创建时, 传递的template属性 , 指的是替换挂载点 显示的内容.

###data属性.
	
	data属性, 在创建Vue时传递, 对应的值是一个对象.
	data对象中包含多个属性 , 每一个属性都可以通过绑定的方式, 显示到网页中.  且数据显示是响应式的(当数据源改变 ,则视图跟随改变)

###插值表达式

	用于在挂载点中, 显示vue实例的属性值;

	格式:
		{{vue的属性名}}

	案例:

		<div id="main">
			<div>
				<p>商品名:{{a1[0].name}}</p>
				<p>简介:{{a1[0].info}}</p>
				<p>价格:{{a1[0].price}}</p>
			</div>
			<button onclick="x1()">下一页</button>
		</div>
		<script type="text/javascript">
			var arr1 = [
				{
					name:"球",
					info:"美滋滋",
					price:3
				}
			];
			var arr2 = [
				{
					name:"气球",
					info:"美滋滋",
					price:28
				}
			];
			var v = new Vue({
				el:"#main",//el:element
				data:{
					a1:arr1
				}
			});
			function x1(){
				v.a1 = arr2;
			}
		</script>

###v-text属性
	作用:	用于在挂载点中, 显示vue属性值
	格式:	<开始标签 v-text="vue属性名">
###v-html属性
	
	作用:	用于在挂载点中, 显示vue属性值
	格式:	<开始标签 v-html="vue属性名">

###v-text与v-html的区别

	v-text属性 在显示数据时, 如果数据中存在html标签, 则标签不会不会执行, 原样显示.
	v-html属性 在显示数据时, 如果数据中存在html标签, 则标签内容会按照解析后的结果显示.


###Vue 全局组件

	用于扩展HTML标签库, 自定义标签来  封装可重用的html代码

	使用步骤:
		1.	在JS代码块中, 注册标签.
				Vue.component("标签名",{"template":"标签内容"});
		2.	在挂载点的div中使用标签
				
	案例:
		<style type="text/css">
			.item{
				width:250px;
				height:200px;
				border:1px solid #999;
				border-radius: 5px;
				overflow: hidden;
				display: inline-block;
				margin: 10px;
			}
			.item:hover{
				box-shadow:0px 0px 3px 3px #999;
			}
			.item>img{
				width:250px;
				height:140px;
			}
			.item>div{
				height:60px;
				text-align: center;
				line-height: 60px;
			}
		</style>
		<script type="text/javascript" src="js/vue.js"></script>
		</head>
		<body>
			<div id="main">
				<car></car>
				<car></car>
				<car></car>
				<car></car>
				<car></car>
				<car></car>
				<car></car>
			</div>
			
			<script type="text/javascript">
				Vue.component("car",{"template":"<div class='item'><img src='images/21.jpg'><div>这是一个汽车的图片</div></div>"});
				var v = new Vue({
					el:"#main"//el:element
				});
			</script>
		</body>

###事件指令

	格式1.
		步骤1.	向vue实例中加入methods属性. 此属性在创建vue时传入 , 值是一个对象.
				对象中包含的是一个个的属性,  属性值都是函数.

		步骤2.	在挂载点中 , 给需要添加事件的元素 ,加入如下属性:
				v-on:事件类型="methods中的属性名"

	格式2.	格式一的简写格式 将v-on:, 简写为@即可.


###属性绑定

	作用:	将Vue属性值, 与元素的属性绑定

	格式1.
		<开始标签 v-bind:属性名="vue属性名" >

	格式2. (简写)
		<开始标签 :属性名="vue属性名" >

###class属性绑定

	class属性因为常用于更改样式 , vue对class属性值的绑定进行了增强 !

####格式1.	class属性值为对象
	属性值为对象, 对象中包含的是一个个的boolean属性, 当属性值为true时, 则属性名作为class属性值存在

	例如:
		<div v-bind:class="{a:true,b:false,c:true}"></div>

	class的结果:
		<div class="a c">


####格式2.	class属性值为数组

	属性值是数组, 数组中的每一个元素都是vue属性名, vue属性值就是class值

	例如:
		<div v-bind:class="[a,b,c]">
		new Vue({
			data:{
				a:"ha",
				b:"he",
				c:"hei"
			}
		});
	
	最终div的class值:
		<div class="ha he hei">

####双向属性绑定 
	
	仅适用与input标签的value属性值
	
	vue实例中的一个属性 与 输入框的value属性值 进行双向绑定后:
		更改vue属性值, 会导致input的value属性变化
		用户输入内容,input的value属性变化 也会影响vue属性值.

	格式:	<input v-model="vue属性名">

####计算属性
	
	一个属性的值 是通过计算得到的.

	使用步骤:
		给vue添加computed属性 , 属性值是对象, 对象在vue创建时传入, 对象中的属性值是函数 , 函数的返回值就是属性的内容.

	案例:
		
		<div id="main">
			<input v-model="val1">+<input v-model="val2">=<span>{{sum}}</span>
		</div>
		
		<script type="text/javascript">
			var v = new Vue({
				el:"#main",//el:element
				data:{
					val1:"",
					val2:""
				},
				computed:{
					sum:function(){
						var x = new Number(this.val1);
						var y = new Number(this.val2);
						if(isNaN(x)){
							x= 0;
						}
						if(isNaN(y)){
							y= 0;
						}
						return x+y;
					}
				}
			});
		</script>


####属性监听器

	监听属性值的变化, 当属性值变化时,监听器执行

	格式:
		在vue对象创建时, 传入watch属性 , watch属性值是一个对象 对象中包含一个个的属性, 属性值都是函数.
		对象中包含的属性名, 就是要监听的属性名, 函数就是当属性改变时执行的监听器

	案例:
		
		<div id="main">
			<input v-model="val">
		</div>
		
		<script type="text/javascript">
			var v = new Vue({
				el:"#main",//el:element
				data:{
					val:""
				},
				watch:{
					val:function(){
						console.log("val属性值更改为了:"+this.val);
					}
				}
			});
		</script>


###v-if 指令
	作用:	用于判断一个元素 是否加载到网页中, 常用于网页权限的编写.
	
	格式:
		<开始标签 v-if="vue属性名">
		标签中的内容, 只有v-if属性值为true时, 才加载.
###v-show指令
	作用:	用于判断一个元素 是否显示到网页中.
	
	格式:
		<开始标签 v-show="vue属性名">
		标签中的内容, 只有v-show属性值为true时, 才显示.

###v-if 与 v-show 的区别:

	v-if	结果为false时,  内容不加载. 网页中这个元素是不存在的.
	v-show	结果为false时,  内容还在, 只是隐藏了.

###v-for指令
	
	用于遍历数组, 循环展示相同的数据.

	格式1.
		<开始标签 v-for="变量名1 of 数组名"></结束标签>
	
	格式2.
		<开始标签 v-for="(变量名1,变量名2) of 数组名">

		变量名1:	循环时, 从数组中取出的每一个内容. 这个变量名, 在标签内容中, 可以通过插值表达式, 属性绑定等方式使用
		变量名2: 循环时的下标
		数组名: 循环遍历的数组, 是vue中的属性名.   循环时数组的长度 就是 自动生成的标签数量.

	案例:
		<div id="main">
			<table class="table table-striped table-bordered table-hover">
				<tr><th>姓名</th><th>年龄</th><th>身高</th><th>操作</th></tr>
				<tr v-for="(item,index) of arr">
					<td>{{item.name}}</td>
					<td>{{item.age}}</td>
					<td>{{item.length}}</td>
					<td><span class="btn btn-info">删除</span></td>
				</tr>
			</table>
			<button @click="x1">下一页</button>
		</div>
		
		<script type="text/javascript">
			var arr1 = [
				{
					name:"小泽马",
					age:18,
					length:"18mm"
				},
				{
					name:"天海马",
					age:17,
					length:"16mm"
				},
				{
					name:"加藤马",
					age:28,
					length:"15mm"
				},
				{
					name:"吉泽马",
					age:38,
					length:"-1m"
				},
			];
			var arr2 = [
				{
					name:"仓井马",
					age:38,
					length:"-2m"
				},
				{
					name:"龙泽马",
					age:30,
					length:"12mm"
				},
				{
					name:"上元马",
					age:28,
					length:"10mm"
				},
				{
					name:"白龙马",
					age:18,
					length:"1m"
				},
			];
		
			var v = new Vue({
				el:"#main",//el:element
				data:{
					arr:arr1
				},
				methods:{
					x1:function(){
						//模拟访问服务器的过程, 假设耗时2秒
						var index = layer.load();
						setTimeout(function(){
							layer.close(index);
							layer.msg("加载完成.");
							v.arr = arr2;
						},2000);
						
					}
				}
			});
		</script>
	







