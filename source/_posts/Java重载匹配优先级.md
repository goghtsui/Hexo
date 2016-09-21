title: Java重载匹配优先级
date: 2015-11-23 16:31:05
categories: [Java]
tags: [重载, java] 
---
#### 前情提要
Java面向对象的三个基本特征：继承、封装和多态；多态主要体现在重载和重写；

#### 示例代码
无意间看到这样一个问题，为了方便直观，就用代码来描述问题，有这样一个类：
``` java
public class OverloadPriority {

	public static void print(Object arg) {
		System.out.println("parameter type = Object");
	}

	public static void print(int arg) {
		System.out.println("parameter type = int");
	}

	public static void print(long arg) {
		System.out.println("parameter type = long");
	}
	
	public static void print(double arg) {
		System.out.println("parameter type = double");
	}
	
	public static void print(float arg) {
		System.out.println("parameter type = float");
	}

	public static void print(char arg) {
		System.out.println("parameter type = char");
	}

	public static void print(Character arg) {
		System.out.println("parameter type = Character");
	}

	public static void print(char... arg) {
		System.out.println("parameter type = char...");
	}

	public static void print(Serializable arg) {
		System.out.println("parameter type = Serializable");
	}

	public static void print(Comparable<?> arg) {
		System.out.println("parameter type = Comparable");
	}

	public static void main(String[] args) {
		// int
		print('g');
	}
	
}
```
<!-- more -->

可以看到我们这里重载了print(xxx)这个方法，不同类型的参数，那么在调用上会出现什么问题呢？这里就以char类型为例来分析一下。

main方法执行print('g')，输出结果毫无疑问就是：
```
parameter type = char
```
那么注释掉print(char arg)这个方法，会输出什么结果呢？
```
parameter type = int
```
那么注释掉print(int arg)这个方法，会输出什么结果呢？
```
parameter type = long
```

这是为什么呢？ 这就是重载当中参数类型的优先级问题。我们都知道'g'除了表示字符g之外，还能表示数字103（g的ASCII码是103），所以会输出为int，发生了类型转换，类型自动提升，结果依次是char -> int -> long -> double -> float -> Character -> Serializable or Comparable -> Object -> char...(变长参数，即char元素数组)

#### 总结
遇上重载时，会查找类型最匹配的参数，然后提升类型、封装类型、匹配接口、继承关系型、变长参数类型
#### 注意
- 变长参数的重载优先级最低
- char到byte或short之间的转换是不安全的
- 在Serializable和Comparable同时存在的情况下会报异常：
	The method print(Object) is ambiguous for the type OverloadPriority （意思是无法确定应该使用哪一个重载方法，
	因为Character实现了Serializable和Comparable这两个接口，
	而接口匹配的优先级是一样的，编译器无法判断转型为哪种类型，
	提示类型模糊，无法正常编译）
- 接口无法匹配之后，就会开始查找匹配的父类，优先级是顺着继承链，由下往上进行匹配

所以在重载方法的时候大家一定要注意这些细节问题，这样可能导致最后输出的结果不是你想要的结果，大家可以编写这样的一段代码测试一下