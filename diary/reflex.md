##reflexMachineMade
###11/27/2019 2:54:55 PM 
####Without learning, men grow as cows do increasing only in flesh not wisdom.
　　**不学习的人，宛如老牛，肉虽多，却没有智慧。**
###反射机制
1. **基本概念**  
	通常情况下编写代码都是固定的，无论运行多少次执行的结果也是固定的，在某些情况编写代码时不确定创建什么类型对象，也不确定调用什么样的方法，这些都希望通过运行时传递的参数来决定，该机制叫作动态编程技术，也就是反射技术。  
	通俗的来说，反射机制就是用于动态创建对象并且动态调用方法的机制。  
	目前主流的框架底层都是采用反射机制实现的

2. **Class类**
   1. 基本概念  
      * java.lang.Class类的实例用于描述Java应用程序中的类与接口，也就是一种数据类型
      * 该类没有公共的构造方法，该类的实例有java虚拟机和类加载器自动构造完成。
   2. 获取Class对象的方式
      * 使用数据类型.class的方式可以获取对应类型的Class对象。（引用数据类型和基本数据类型）
      * 使用 引用/对象.getClass() 的方式可以获取对应类型的Class对象。（处理应用数据类型）
      * 使用 包装类.TYPE 的方式可以获取对应基本数据类型的Class对象。（处理基本数据类型）
      * 使用Class.forName()的方式来获取参数指定类型的Class对象。（处理引用数据类型，不能省略包名.类名）
   3. 常用的方法  
      * static Class<?> forName(String className)   
			-用于获取参数指定类型对应的Class对象并返回  
      * T newInstance() -用于创建该Class对象所表示类的新实例    
			-若该Class对象代表Person类，则调用该方法的意义就是创建Person类型的对象   
      * Constructor<T> getConstructor(Class<?>... parameterTypes)   
      		-用于获取次Class对象所表示类型中参数指定的公共构造方法  
			-若该Class对象代表Person类，则调用该方法的意义就是获取Person类中的公共构造方法，具体构造方法由参数决定    
      * Constructor<?>[] getConstructors()   
      		-用于获取此Class对象所表示类型中所有的公共构造方法   
      * Filed getDeclaredField(String name)  
      		-用于获取此Class对象所表示中参数指定的单个成员变量信息   
			-若该Class对象代表Person类，则调用该方法的意义就是获取Person类中的成员变量，具体哪个成员变量由参数决定。  
      * Field[] getDeclaredFields()   
	      	-用于获取此Class对象所表示类中所有成员变量信息
   4. Constructor类
      1. 基本概念   
      		-java.lang.reflect.Constructor类主要用于描述获取到的构造方法信息  
      2. 常用的方法  
         * T newInstance(Object... initargs) //...表示可变长度参数列表   
         	-使用此Constructor对象描述的构造方法来构造Class对象代表类型的新实例   
			-该方法的参数用于给新实例中的成员变量进行初始化操作   
			-若该Constructor对象代表Person类中的有参构造方法，则调用该方法的意义就是使用有参构造方法来构造Person类型的对象，  
   5. Filed类 
      1. 基本概念  
	      java.lang.reflect.Field类主要用于描述获取到的单个成员变量信息  
      2. 常用的方法  
      	  Object get(Object obj)  
				-调用该方法的意义就是获取参数对象obj中此File的对象所表示成员变量的数值  
				-若该Filed对象代表Person类中的成员变量name，name调用该方法就是获取对象obj中成员变量name的数值   
		  void set(Object obj, Object value)  
				-将参数对象obj中此Filed对象表示的成员变量的数值改为参数value的数值   
				-若该Filed对象代表的Person类中的成员变量name，那么调用该方法就是将对象obj中的成员变量name的数值改为value的值   
		  void setAccessible(boolean flag) 设置私有属性可见性   

		  Method getMethod(String name,Class<?>... parameterTypes)    
			-用于获取该Class对象表示类型中名字为name参数parameterTypes的指定公共成员方法   
			-若该Class对象表示为Person类型，则调用该方法的意义就是获取Person类中的成员方法  
		  Method[] getMethods()    
			-用于获取该Class对象表示类中所有的公共成员方法   
    5. Method类
       1. java.lang.reflect.Method类主要用于描述获取到的单个成员方法信息
       2. 常用的方法   
       	Object invoke(Object obj,Obejct... args)   
			-使用对象obj来调用此Method对象所表示的成员方法，实参传递args.   
			-若此Method对象代表Person类中的getName方法，则调用该方法的意义就是使用对象obj来调用getName方法，实参传递args。
 
>>>void 返回null型
		   
	

	