##2/15/2020 1:33:38 PM 
##Listen to your inner voice.
**请聆听你内心的声音。**
###<center>FreeMarker</center>
####
####为什么选用FreeMarker
	Jsp算是模板技术的一种
	FreeMarker也是模板技术的一种
	FreeMarker性能更好，渲染更快
####模板技术原理
	 模板技术并不是什么神秘技术，干的是拼接字符串的体力活，模板引擎就是利用正则表达式识别模板标识，并利用数据替换其中的表示符，模板技术包含俩个方面：
		定义模板标识符
		解析模板标识符
####后端模板技术
	Jsp Velocity FreeMarker Thymeleaf

####总结
	熟悉核心业务
		客户关系管理系统
		OA办公 与办公相关的就是核心业务
		与客户有关系的都是核心业务
		p2p网贷与借贷相关就是核心业务
		商城 商品订单之类核心业务
	FreeMarker是什么，为什么一定要使用它，常用指令，在SpringMvc如何集成
		FreeMarker是一种模板技术，解决数据和显示分离，性能比jsp好
		assign if list ${} 取不到会报错，时间一定要指定格式，若报错，认真看错误信息

		添加俩个依赖
		在mvc.xml配置视图FreeMarker解析器 从笔记拷贝
		控制器处理方法的代码不变
		修改之前jsp，把里面的jstl jsp指令改成FreeMarker的

	
		