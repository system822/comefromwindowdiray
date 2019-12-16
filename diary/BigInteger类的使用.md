###12/11/2019 11:29:55 AM 
###No one ever made a difference by being anyone else.
**唯有与众不同，才能改变世界。**
	1. 基本概念
		若希望表示比long类型范围还打的整数数据，则需要借助java.math.BigInteger类型描述
	2. 常用的方法
		BigInteger(String val) 
			- 根据参数指定的字符串内容来构造对象
		BigInteger add(BigInteger val)
			- 用于实现调用对象与参数对象的和并返回的功能
		BigInteger subtract(BigInteger val)
			- 用于实现调用对象与参数对象的差并返回的功能
		BigInteger multiple(BigInteger val)
			- 用于实现调用对象与参数对象的积并返回的功能	
		BigInteger divide(BigInteger val)
			- 用于实现调用对象与参数对象的商并返回的功能
		BigInteger remainder(BigInteger val)
			- 用于实现调用对象与参数对象的余数并返回的功能
		BigInteger[] divideAndRemainder(BigInteger val)
			-用于实现调用对象与从参数对象的商和余数 并返回