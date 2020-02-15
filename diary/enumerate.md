##2/12/2020 4:17:01 PM 
###You gotta do what you're meant to do.
**想做什么就去做。**
###<center>enumerate 枚举的简单使用</center>
####枚举类的定义和使用
	枚举是一种特殊的类，固定的一个类只能有那些对象，定义格式
		public enum 枚举类名{
			常量对象A,常量对象B,常量对象C;
		}
	我们自定义的枚举类型在底层都是直接继承了java.lang.Enum类的。
		public enum ClothType{
			MEN,WOMEN,NEUTRAL;
		}
	枚举中都是全局公开的静态常量，可以直接使用枚举类名调用。
		ClothType type = ClothType.MEN;
	因为java.lang.Enum类是所有的枚举类的父类，所以所有的枚举对象可以调用Enum类中的方法。
		String name = 枚举对象.name(); //返回枚举对象的常量名称
		int ordinal = 枚举对象.ordinal(); //返回枚举对象的序号，从0开始

>枚举不能创对象
