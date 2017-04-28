title: 关于java.lang.String的intern()方法【基础篇】
date: 2017-04-28 17:10:01
categories: [Java]
tags: [String, intern]
---

## 序言

相信在开发过程中，我们对字符串（String）的使用还是非常普遍的，但它也是很讲究的，像内存的占用、线程安全问题，都是需要注意的，无意中了解到一个新的api，就是intern() ，这个到底是干什么的呢？下面就带大家简单了解一下

## 功能描述

### 官方解释

返回字符串对象的规范化表示形式。 

一个初始时为空的字符串池，它由类 String 私有地维护。 
当调用 intern 方法时，如果池内已经包含一个等于此 String 对象的字符串（该对象由 equals(Object) 方法确定），则返回池中的字符串。否则，将此 String 对象添加到池中，并且返回此 String 对象的引用。 
它遵循对于任何两个字符串 s 和 t，当且仅当 s.equals(t) 为 true 时，s.intern() == t.intern() 才为 true。 
所有字面值字符串和字符串赋值常量表达式都是内部的。字符串字面值在《Java Language Specification》的 §3.10.5 中已定义。 

<!-- more -->

返回： 
一个字符串，内容与此字符串相同，但它保证来自字符串池中。

总结：
s.intern()方法执行的时候，会将池中的字符串与外部的字符串(s)进行比较，如果池中有与之相等的字符串，则不会将外部的字符串放到池中，返回的只是池中的字符串，如果不同则将外部字符串放入池中，并返回其字符串的句柄（引用）-- 这样做的好处就是能够节约空间

### 实战分析

示例代码：

```java
String a = new String("ab");
String b = new String("ab");
String c = "ab";
String d = "a" + "b";
String e = "b";
String f = "a" + e;

System.out.println(b.intern() == a);
System.out.println(b.intern() == c);
System.out.println(b.intern() == d);
System.out.println(b.intern() == f);
System.out.println(b.intern() == a.intern());
```

运行结果：

```java
false 
true 
true 
false 
true 
```

结果分析：

首先说，字面值对应的是字符串池。

接下来说字符串的初始化，有以下形式：

```java
// 这种创建方式肯定不会存入字符串池，且是一个全新的对象
String str1 = new String("");

// 这种方式会存储在字符串池(赋值的是：常量值)
String str2 = "hello";

// 这种方式会存储在字符串池(赋值的是：常量值 + 常量值 = 常量值)
String str3 = "hello" + "world";

// 这种方式也不会存储在字符串池(赋值的是：常量值 + 变量值 != 常量值)
String str4 = str2 + "world";
```

那么对比结果分析：

1. a、b 都是用了关键字 new 的方式初始化，c、d、e 直接赋值字符串，f 使用的是字符串常量 + 变量的形式，也不会进入字符串池
2. 因此可以看出来，(b.intern() == a) 和 (b.intern() == c)，采用new 创建的字符串对象不进入字符串池，而且(b.intern() == d) 和 (b.intern() == f) 在字符串相加的时候，都是静态字符串的结果会添加到字符串池，如果其中含有变量（如f中的e）则不会进入字符串池中。但是字符串一旦进入字符串池中，就会先查找池中有无此对象。如果有此对象，则让对象引用指向此对象。如果无此对象，则先创建此对象，再让对象引用指向此对象。