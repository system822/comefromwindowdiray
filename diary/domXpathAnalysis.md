###Some people look out on the sea and see a brand-new day.
<font color="tan" ><b>有时候远眺大海，就能看到明天的无限可能。</b></font>
###11/27/2019 10:57:32 AM 
###任务：定义xml，解析xml,利用反射技术创建定义的bean对象
####掌握XML解析技术(dom4j,xpath)
1. **xml可扩展标记语言**
2. **特性**
   * xml具有平台无关性，是一门独立的编程语言
   * xml具有自我描述性
   * 主要应用于：数据存储，网络数据传输，配置文件
###xml语法 ***
	xml文档通常存储在.xml文件中
	1. xml文档声明
	<?xml version="1.0" encoding="UTF-8"?>
	2. 标记（元素/标签/节点)
	在xml中，我们通过标记，来描述文档。
	语法：
		开始标记（开放标签） ： <标记名称>
		结束标记（闭合标签） ： </标记名称>
	
		标记名称：自定义名称，命名规则与java标识符的命名规则相同。
		标记内容：开始标记与结束标记之间可以编写标记的内容。
	3. 标记之间，可以互相嵌套，不允许交叉
	4. 一个xml中，必须且仅有一个根节点
	5. 标记名称允许重复
	6. 标记层级的名称（字标记，父标记，兄弟标记，后代标记，祖先标记）
	7. 标记的属性
		标记中可以包含属性，属性用于描述标记的某些特殊数据
		一个开始标记可以包含0~n个属性，每一个属性就是一个键值对
		键不允许重复，键与值之间使用等号链接，多个键值对之间使用空格分隔
		属性值，必须被引号引住
	8. 注释的写法
		多行注释：
			注释开始： <!--
			注释结束： -->
***
####java解析
#####面试题
	问： Java有几种xml解析方式？分别是什么？又怎样的优缺点？
	答：
		解析方式 有两种或四种。
		1. SAX解析
			事件驱动机制的解析方式。
			SAX解析，逐行读取xml文件，每当读取到一个节点的开始/结束/内容/属性值时。触发事件
		我们写程序时，可以在这些事件发生时，执行执行的代码，获取相应的内容。
		优点：
			在读取大文件的时候，节省内存。
			（因为逐行读取，所以读取下一行数据时，上一行数据就被释放了）
		缺点:
			1. 因为是逐行读取，读取时不操作的数据，过去后就无法操作了。
			2. 因为事件驱动机制，我们程序员只能得知事件的发生，无法感知到事件的元素层次，需要程序员自己标记节点层次
			3. 因为是逐行解析，所以效率较低
			4. sax解析方式，是只读解析方式，无法修改xml文档的内容
		2. DOM解析 
			直接将整个文档，加载到内存，在内存中建立文档树模型，然后通过操作文档树，来完成数据的获取与修改
			优点：
				1. 文档整体加载到内存中，虽然耗费了内存，但是解析时效率就相对高了很多
				2. 文档在内存中加载，可以任意的读取，修改，删除
			缺点：
				无法解析大文件
		3. JDOM解析
			是DOM解析的扩展，与DOM解析的优缺点完全一致
		4. DOM4J解析
			是DOM解析的扩展，与DOM解析的有趣点完全一致 

####DOM4J解析
		步骤：
			1. 引入jar文件（dom4j.jar)
			2. 创建xml文档的输入流
				InputStream is = new FileInputStream("文件地址")；
			3. 创建一个xml读取工具对象
				SAXReader sr = new SAXReader();
			4. 通过读取工具，读取xml文档的输入流，并得到文档对象
				Document doc = sr.read(is);
			5. 通过文档对象，获取xml文档的根节点对象
				Element root = doc.getRootElement();
####Element 常用方法
		一个Element对象，就表示一个xml文档的节点
			- 获取节点名称
				String getName();
			- 获取节点内容
				String getText();
			- 设置节点内容
				String setText();
			- 根据子节点的名称，获取匹配的第一个子节点名称
				Element element("节点名称");
			- 获取所有的子节点
				List<ELement> elements();
			- 获取节点的属性值
				String attributeValue("属性名称");

####XPATH解析xml
	作用： 通过路径快速的查找一个或一组元素
	
	路径表达式：
		
		1. /	:	从根节点开始查找
					例如		/name,表示查找根节点name
		2. //	:	从当前节点查找后代元素 ***
		3. .	:	查找当前节点
		4. ..	:	查找父节点
		5. @	:	选择属性
	
####XPATH如何使用？
	通过Node类的方法，来完成查找
	
	方法一
		用于一次查找单个节点
		Node node = node.selectSingleNode("xpath表达式");
	方法二
		用于一次查找一组节点
		List<Node> nodes = node.selectNodes("xpath表达式");	

	案例1:
		//1.	创建指向XML文件的输入流
		InputStream is = new FileInputStream("C:\\code\\34\\code1\\day04_XML\\src\\students.xml");
		//2.	创建XML读取工具
		SAXReader sr = new SAXReader();
		//3.	读取XML文件输入流. 并得到文档对象
		Document doc = sr.read(is);
		//4.	使用xpath查找一组name
		List<Element> es = doc.selectNodes("//age");
		for (Element element : es) {
			System.out.println(element.getText());
		}

	案例2:
		//1.	创建指向XML文件的输入流
		InputStream is = new FileInputStream("C:\\code\\34\\code1\\day04_XML\\src\\students.xml");
		//2.	创建XML读取工具
		SAXReader sr = new SAXReader();
		//3.	读取XML文件输入流. 并得到文档对象
		Document doc = sr.read(is);
		//4.	使用xpath查找单个name 
		Element e = (Element)doc.selectSingleNode("//student[@id='2']//name");
		System.out.println(e.getText());


####通过Java生成XML文档 了解

	步骤:
		1.	通过文档帮助器(DocumentHelper), 创建空的文档对象(Document)
				Document doc = DocumentHelper.createDocument();

		2.	通过文档对象,  添加根节点! 并得到新添加的根节点对象
				Element root = doc.addElement("根节点名称");

		3.	通过根节点, 丰富子节点
				Element 子节点 = root.addElement("元素名称");
				....
		4.	创建一个文件输出流, 用于将xml文档输出到文件中
			FileOutputStream fos ....
		
		5.	将文件输出流, 转换为 XML文档输出流
			XMLWriter xw = new XMLWriter(fos); 

		6.	写出文档, 并释放资源
			xw.write(doc);
			xw.close();
			fos.close();


####给元素 添加子节点  
	
	Element 子节点 = 元素对象.addElement("子节点名称");

###给元素 添加属性和值

	元素对象.addAttribute(String 属性名,String 属性值);