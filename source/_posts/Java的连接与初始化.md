title: Java的连接与初始化
date: 2015-12-09 12:52:53
categories: [Java]
tags: [类初始化,虚拟机,Java]
photos:
- http://img.blog.csdn.net/20151010184504881

---
> 原文作者：iceAeterna
> 原文链接：[http://www.cnblogs.com/iceAeterNa/p/4876747.html](http://www.cnblogs.com/iceAeterNa/p/4876747.html)


#### 序言
Java虚拟机通过装载、连接、初始化来使得一个Java类型可以被Java程序所使用，如下图所示，其中连接过程又分为验证、准备、解析三个部分。其中部分类的解析过程可以推迟到程序真正使用其某个符号引用时再去解析。
![](http://img.blog.csdn.net/20151010184504881)

#### 解析
解析过程可以推迟到类的初始化之后再进行，但这是有条件的，Java虚拟机必须在每个类或接口主动使用时进行初始化。 
以下为主动使用的情况： 
- 创建某个类新的实例(无论直接通过new创建出来的，还是通过反射、克隆、序列化创建的)
- 使用某个类的静态方法 
- 访问某个类或接口的静态字段 
- 调用JavaAPI中的某些反射方法 
- 初始化某个类的子类(要求其祖先类都要被初始化，否则无法正确访问其继承的成员) 
- 启动某个标明为启动类的类(含有main()方法) 
主动使用会导致类的初始化，其超类均将在该类的初始化之前被初始化，但通过子类访问父类的静态字段或方法时，对于子类(或子接口、接口的实现类)来说，这种访问就是被动访问，或者说访问了该类(接口)中的不在该类(接口)中声明的静态成员。 
<!-- more -->

Grandpa的定义如下：
``` java
package com.ice.passiveaccess;
 
public class Grandpa {

    static{
        System.out.println("Grandpa was initialized.");
    }
    
}
```
Parent的定义如下：
```java
package com.ice.passiveaccess;

public class Parent extends Grandpa{

    static String language = "Chinese";
    
    static{
        System.out.println("Parent was initialized.");
    }
    
}
```
Cindy的定义如下：
```java
package com.ice.passiveaccess;

public class Cindy extends Parent{

    static{
        System.out.println("Child was initialized.");
    }
    
}
```
现在通过Cindy访问父类的language成员:
```java
package com.ice.passiveaccess;

public class PassiveAccessTest {
    public static void main(String args[]){
        System.out.println(Cindy.language);
    }
}
```
结果如下： 
```java
Grandpa was initialized.
Parent was initialized.
Chinese
```

可见这是被动访问，Cindy自身并没有初始化

#### 装载
装载的过程：
- (1).找到该类型的class文件，产生一个该类型的class文件二进制数据流(ClassLoader需要实现的loadClassData()方法) 
- (2).解析该二进制数据流为方法区内的数据结构 
- (3).创建一个该类型的java.lang.Class实例 

在加载器的相关代码中可以看到，最终通过defineClass()创建一个Java类型对象(Class对象)。 

#### 验证 
class文件校验器需要四趟独立的扫描来完成验证工作，其中： 
- 第一趟扫描在装载时进行，会对class文件进行结构检查，如 
	- (1).对魔数进行检查，以判断该文件是否是一个正常的class文件 
	- (2).对主次版本号进行检查，以判断class文件是否与java虚拟机兼容 
	- (3).对class文件的长度和类型进行检查，避免class文件部分缺失或被附加内容。 　
- 第二趟扫描在连接过程中进行，会对类型数据进行语义检查，主要检查各个类的二进制兼容性(主要是查看超类和子类的关系)和类本身是否符合特定的语义条件 
	- (1).final类不能拥有子类 
	- (2).final方法不能被重写(覆盖) 
	- (3).子类和超类之间没有不兼容的方法声明 
	- (4).检查常量池入口类型是否一致(如CONSTANT_Class常量池的内容是否指向一个CONSTANT_Utf8字符串常量池) 
	- (5).检查常量池的所有特殊字符串，以确定它们是否是其所属类型的实例，以及是否符合特定的上下文无关语法、格式 
- 第三趟扫描为字节码验证，其验证内容和实现较为复杂，主要检验字节码是否可以被java虚拟机安全地执行。 
- 第四趟扫描在解析过程中进行，为对符号引用的验证。在动态连接过程中，通过保存在常量池的符号引用查找被引用的类、接口、字段、方法时，在把符号引用替换成直接引用时，首先需要确认查找的元素真正存在，然后需要检查访问权限、查找的元素是否是静态类成员而非实例成员。 

#### 准备 
为类变量分配内存、设置默认初始值(内存设置初始值，而非对类变量真正地进行初始化，即类中声明int i = 5，但实际上这里是分配内存并设置初始值为0) 

#### 解析
在类的常量池中寻找类、接口、字段、方法的符号引用，将这些符号引用替换成直接引用 

#### 初始化
对类变量赋予指定的初始值(这个时候int i = 5就必须赋予i以初值5)。这个初始值的给定方式有两种，一种是通过类变量的初始化语句，一种是静态初始化语句。而这些初始化语句都将被Java编译器一起放在方法中。 
如前面所述，一个类的初始化需要初始化其直接超类，并递归初始化其祖先类，初始化是通过调用类的初始化方法完成的。此外，对于接口，并不需要初始化其父接口，而只需要执行该接口的接口初始化方法就可以了。

注意：
- (1).在初始化阶段，只会为类变量(静态全局变量)进行初始化工作，并且当类变量声明为final类型切初始化语句采用了常量表达式方式进行初始化赋值，那么，也不会对其进行初始化，它将会直接被编译器计算并保存在常量池中，并且对这些变量的使用也将直接将其变量值嵌入到字节码中。
如UsefulParameter类如下： 
```java
Class UsefulParameter{ 
	static final int height = 2; 
	static final int width = height * 2; 
} 
```
类Area的类变量初始化如下： 
```java
Class Area{ 
	static int height = UsefulParameter.height * 2 ; 
	static int width = UsefulParameter.width * 2; 
} 
```
在Area的< clinit>中，将直接把2、4嵌入到字节码中:
![clinit](http://images2015.cnblogs.com/blog/821477/201510/821477-20151014103108788-81357571.png)

- (2).接口的初始化与类有所不同，在初始化阶段，会为在接口中声明的所有public、static和final类型的、无法被编译为常量的字段进行初始化 

#### 类实例化 
这里需要明白什么是类初始化，什么是类实例化，以及类的实例对象的初始化

如前面所述，类初始化时对类(静态)变量赋予指定的初始值，类初始化之后就可以访问类的静态字段和方法，而访问类的非静态(实例)字段和方法，就需要创建类的对象实例，故类的实例化是在类的初始化之后，是在堆上创建一个该类的对象。 
类的静态方法和字段属于类，作为类型数据保存在方法区，其生命周期取决于类，而实例方法和字段位于Java堆，其生命周期取决于对象的生命周期。
　　
**类的初始化会从祖先类到子类、按出现顺序，对类变量的初始化语句、静态初始化语句块依次进行初始化。而对类实例的初始化也类似，会从祖先类到子类、按出现顺序，对类成员的初始化语句、实例初始化块、构造方法依次进行初始化。 **

比如：
```java
package com.ice.init;

public class Parent {
    public static int i = print("parent static:i");
    public int ii = print("parent:ii");

    static{
        print("父类静态初始化");
    }

    {
        print("父类实例初始化");
    }

    public Parent(String str) {
        System.out.println("parent constructor:" + str);
    }

    public static int print(String str){
        System.out.println("initial:" + str);
        return i;
    }
}
```
子类Child如下：
```java
package com.ice.init;

public class Child extends Parent{
    public static int i = print("child static:i");
    public int ii = print("child:ii");

    static{
        print("子类静态初始化");
    }

    {
        print("子类实例初始化");
    }

    public Child(String str) {
        super(str);
        System.out.println("Child constructor:" + str);
    }

    public static int print(String str){
        System.out.println("initial:" + str);
        return i;
    }

    public static void main(String args[]){
        Child child = new Child("cindy");
    }
}
```
其初始化顺序为：
```java
initial:parent static:i
initial:父类静态初始化
initial:child static:i
initial:子类静态初始化
initial:parent:ii
initial:父类实例初始化
parent constructor:cindy
initial:child:ii
initial:子类实例初始化
Child constructor:cindy
```
Java编译器为每个类生成了至少一个实例初始化方法< init >，一个< init >方法分为三部分： 另一个初始化方法< init >()，对任意实例成员的初始化的字节码，构造方法的方法体的字节码

< init >方法的调用如下： 
若< init >指明从this()方法明确调用另一个构造方法，那么将调用另一个构造方法，否则，若该类有直接超类，那么，若< init >指明从super()方法明确调用其超类的构造方法，那么将调用超类的构造方法，否则，将默认调用超类的无参构造方法。这样，将从其祖先类到该类，分别完成对应的实例成员的初始化(可能被子类覆盖) 

接下来以一道题结束本节： 
判断输出：
```java
package com.ice.init;

class T  implements Cloneable{
      public static int k = 0;
      public static T t1 = new T("t1");
      public static T t2 = new T("t2");
      public static int i = print("i");
      public static int n = 99;

      public int j = print("j");
      {
          print("构造块");
      }

      static {
          print("静态块");
      }

      public T(String str) {
          System.out.println((++k) + ":" + str + "    i=" + i + "  n=" + n);
          ++n; ++ i;
      }

      public static int print(String str){
          System.out.println((++k) +":" + str + "   i=" + i + "   n=" + n);
          ++n;
          return ++ i;
      }

      public static void main(String[] args){
          T t = new T("init");
      }
    }
```
题解如下：

(1).首先T类被加载、连接后进行初始化，会先对字段k、t1、t2、i、n以及static块进行初始化。 
(2).t1实例的初始化会初始化实例成员j，(实际上先进行父类实例内容的初始化)先调用静态方法print，并执行实例初始化块{}，	    输出： 
 1: j i=0 n= 0(i和n都还没有初始化) 
 2:构造块 i=1 n=1 
(3)随后调用t1实例的构造函数，输出： 
 3:t1 i=2 n=2 
(4).类似有t2实例的初始化： 
 4: j i=3 n= 3 
 5:构造块 i=4 n=4 
 6:t2 i=5 n=5 
(5).i的初始化： 
 7.i i=6 n=6 
(6).n的初始化和静态块的初始化： 
 8.静态块 i=7 n=99(n已经被初始化) 
(7).t实例的初始化： 
 9.j i=8 n= 100 
 10.构造块 i=9 n= 101 
 11.init i=10 n= 102
