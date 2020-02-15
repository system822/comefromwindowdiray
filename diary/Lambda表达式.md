##12/26/2019 2:43:15 PM 
###My heart is broken down when the LTTE, the roses
**我心有猛虎、细嗅蔷薇**
###<center>lambda表达式</center>
###1 Lambda表达式
	Lambda表澳大使，是java8版本支持的新的语法格式，它是推动java8发布的最重要新特性。
	Lambda允许吧函数作为一个方法的参数（函数作为参数传递进方法中）。
	使用Lambda1表达式可以使代码更加简洁紧凑。
###2. 函数式编程
	lambda表达式的语法由参数列表、箭头符号->和函数体组成。函数体既可以是一个表达式，也可以是一个语句块。
	
	Lambda表达式：
		（int a,int b) -> a+b
		 () -> 42
		 (String s) -> {System.out.println(s);}	

	
	函数式接口是指仅仅只包含一个抽象方法的接口，每一个该类型的lambda表达式都会被匹配到这个抽象方法。

	jdk1.8提供了一个@Functionallnterface注解来定义函数式接口，如果我们定义的接口不符合函数式的规范变回报错。

>Java SE8中增加了一个新的包：java.util.function,它里面包含了常用的函数式接口。

	Predicate<T> --接收 T 对象并返回 boolean
	Consumer<T> --接收 T 对象，不返回值
	Function<T , R> --接收 T 对象，返回 R 对象
	Supplier<T> --提供 T 对象（列如工厂），不接受值
	UnaryOperator<T> --接收 T 对象，返回 T 对象
	BinaryOperator<T> --接收两个 T 对象，返回 T 对象 

>[example](https://www.runoob.com/java/java8-lambda-expressions.html)