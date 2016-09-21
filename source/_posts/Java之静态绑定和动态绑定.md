title: Java之静态绑定和动态绑定
date: 2015-11-24 10:07:20
categories: [Java]
tags: [动态绑定, 静态绑定, 重载, 重写]
---
#### 概念
- 程序绑定：绑定指的是一个方法的调用与方法所在的类(方法主体)关联起来，Java中绑定分为绑定分为 **静态绑定**和**动态绑定**
- 动态绑定：在面向过程中又称为**后期绑定**，在程序**运行期**进行了绑定，根据实际情况有选择的进行绑定
- 静态绑定：在面向过程中又称为**前期绑定**，在程序**编译期**进行了绑定，即在还没运行时，就已经加载到内存
#### 对比
- 动态绑定
	- 又称为后期绑定
	- 发生在运行时期
	- 虚方法（可以被子类重写的方法）会根据运行时的对象进行动态绑定
	- 动态绑定使用对象信息来完成
	- 典型应用是方法的重写（Override）
- 静态绑定
	- 又称为前期绑定
	- 发生在编译时期
	- 使用private或static或final修饰的变量或者方法（包括构造方法）
	- 静态绑定使用类信息来完成
	- 典型应用是方法重载（Overload）

<!-- more -->
	
- 运行期
Java的编译过程是将Java源文件编译成字节码（.class文件，JVM可执行代码）的过程，在这个过程中Java是不与内存打交道的，在这个过程中编译器会进行语法的分析，如果语法不正确就会报错
- 编译期
Java的运行过程是指JVM（Java虚拟机）装载字节码文件并解释执行，在这个过程才是真正的创建内存，执行Java程序

Java字节码的执行有两种方式： 
- 即时编译方式：解释器先将字节编译成机器码，然后再执行该机器码
- 解释执行方式：解释器通过每次解释并执行一小段代码来完成java字节码程序的所有操作。

Java程序在执行过程中其实是进行了两次转换，先将源文件转成字节码再转换成机器码。这也正是Java能一次编译，到处运行的原因。在不同的平台上装上对应的Java虚拟机，就可以实现相同的字节码转换成不同平台上的机器码，从而在不同的平台上运行

#### 验证
关于final、static、private和构造方法是前期绑定的理解：
- **private** 
对于private的方法，首先它对外是不可见的，所以肯定不能被继承，那么就没办法通过子类的对象来调用，而只能通过类自身的对象来调用，因此就可以说private方法和定义这个方法的类绑定在了一起
- **final**
final方法虽然可以被继承，但不能被重写（覆盖），虽然子类对象可以调用，但是调用的都是父类中所定义的那个final方法，（由此我们可以知道将方法声明为final类型，一是为了防止方法被覆盖，二是为了有效地关闭java中的动态绑定)
- **static**
对于static方法，可以被子类继承，但是不能被子类重写（覆盖），但是可以被子类隐藏
就是说如果父类里有一个static方法，它的子类里如果没有对应的方法，那么当子类对象调用这个方法时就会使用父类中的方法。而如果子类中定义了相同的方法，则会调用子类的中定义的方法。唯一的不同就是，当子类对象向上转型为父类对象时，不论子类中有没有定义这个静态方法，该对象都会使用父类中的静态方法。因此这里说静态方法可以被隐藏而不能被覆盖。这与子类隐藏父类中的成员变量是一样的。隐藏和覆盖的区别在于，子类对象转换成父类对象后，能够访问父类被隐藏的变量和方法，而不能访问父类被覆盖的方法
由上面我们可以得出结论，如果一个方法不可被继承或者继承后不可被覆盖，那么这个方法就采用的静态绑定。
- **构造**
构造方法也是不能被继承的，我们知道子类是通过super()来调用父类的无参构造方法，来完成对父类的初始化，因此编译时也可以知道这个构造方法到底是属于哪个类

**示例代码**
```
public class SuperClass {

	protected String attribute = "from SuperClass";

	public String getAttribute() {
		return attribute;
	}

	public static void print(SuperClass superClass) {
		System.out.println(" static method " + superClass.attribute);
	}
}

public class SubClass extends SuperClass {

	protected String attribute = "from SubClass";

	public String getAttribute() {
		return attribute;
	}
	
	public static void print(SuperClass superClass) {
		System.out.println(" static method " + superClass.attribute);
	}

	public static void main(String[] args) {
		
		SuperClass superClass = new SubClass();
		
		SubClass subClass = new SubClass();
		
		superClass.print(superClass);
		
		subClass.print(subClass);
		
		System.out.println(" attribute " + superClass.attribute);
		
		System.out.println(" method " + superClass.getAttribute());
		
	}
}
```
**输出结果**
```
 static method from SuperClass
 static method from SuperClass
 attribute from SuperClass
 method from SubClass
```

#### 反编译
使用JDK自带的javap命令反编译看看：
`>javap -c SubClass`
```
Warning: Binary file SubClass contains com.gogh.bind.SubClass
Compiled from "SubClass.java"
public class com.gogh.bind.SubClass extends com.gogh.bind.SuperClass {
  protected java.lang.String attribute;

  public com.gogh.bind.SubClass();
    Code:
       0: aload_0
       1: invokespecial #10                 // Method com/gogh/bind/SuperClass."<init>":()V
       4: aload_0
       5: ldc           #12                 // String from SubClass
       7: putfield      #14                 // Field attribute:Ljava/lang/String;
      10: return

  public java.lang.String getAttribute();
    Code:
       0: aload_0
       1: getfield      #14                 // Field attribute:Ljava/lang/String;
       4: areturn

  public static void print(com.gogh.bind.SuperClass);
    Code:
       0: getstatic     #24                 // Field java/lang/System.out:Ljava/io/PrintStream;
       3: new           #30                 // class java/lang/StringBuilder
       6: dup
       7: ldc           #32                 // String  static method
       9: invokespecial #34                 // Method java/lang/StringBuilder."<init>":(Ljava/lang/String;)V
      12: aload_0
      13: getfield      #37                 // Field com/gogh/bind/SuperClass.attribute:Ljava/lang/String;
      16: invokevirtual #38                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      19: invokevirtual #42                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      22: invokevirtual #45                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      25: return

  public static void main(java.lang.String[]);
    Code:
       0: new           #1                  // class com/gogh/bind/SubClass
       3: dup
       4: invokespecial #54                 // Method "<init>":()V
       7: astore_1
       8: new           #1                  // class com/gogh/bind/SubClass
      11: dup
      12: invokespecial #54                 // Method "<init>":()V
      15: astore_2
      16: aload_1
      17: invokestatic  #55                 // Method com/gogh/bind/SuperClass.print:(Lcom/gogh/bind/SuperClass;)V
      20: aload_2
      21: invokestatic  #57                 // Method print:(Lcom/gogh/bind/SuperClass;)V
      24: getstatic     #24                 // Field java/lang/System.out:Ljava/io/PrintStream;
      27: new           #30                 // class java/lang/StringBuilder
      30: dup
      31: ldc           #58                 // String  attribute
      33: invokespecial #34                 // Method java/lang/StringBuilder."<init>":(Ljava/lang/String;)V
      36: aload_1
      37: getfield      #37                 // Field com/gogh/bind/SuperClass.attribute:Ljava/lang/String;
      40: invokevirtual #38                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      43: invokevirtual #42                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      46: invokevirtual #45                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      49: getstatic     #24                 // Field java/lang/System.out:Ljava/io/PrintStream;
      52: new           #30                 // class java/lang/StringBuilder
      55: dup
      56: ldc           #60                 // String  method
      58: invokespecial #34                 // Method java/lang/StringBuilder."<init>":(Ljava/lang/String;)V
      61: aload_1
      62: invokevirtual #62                 // Method com/gogh/bind/SuperClass.getAttribute:()Ljava/lang/String;
      65: invokevirtual #38                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      68: invokevirtual #42                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      71: invokevirtual #45                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      74: return
}
```
通过javap我们只能看到静态绑定的部分，就是两个print方法的调用和superClass.attribute，直接看main方法里面的内容：
-  17: invokestatic  #55                 // Method com/gogh/bind/SuperClass.print:(Lcom/gogh/bind/SuperClass;)V
调用的SuperClass.print方法
- 21: invokestatic  #57                 // Method print:(Lcom/gogh/bind/SuperClass;)V
- 37: getfield      #37                 // Field com/gogh/bind/SuperClass.attribute:Ljava/lang/String;
这个也是调用的SuperClass.print方法
#### 总结
Java中的static方法和final方法属于前期绑定，子类无法重写final方法，除了static方法和final方法之外的其他方法属于后期绑定，运行时能判断对象的类型进行绑定。

与方法不同，在处理Java类中的成员变量（静态和非静态）时，并不是采用运行时绑定，而是一般意义上的静态绑定。所以在向上转型的情况下，对象的方法可以找到子类，而对象的属性（成员变量）还是父类的属性（子类对父类成员变量的隐藏）。

Java因为什么对属性要采取静态的绑定方法？这是因为静态绑定是有很多的好处，它可以让我们在编译期就发现程序中的错误，而不是在运行期，这样就可以提高程序的运行效率！由于动态绑定需要在运行时确定执行哪个方法实现或者变量，比起静态绑定起来要耗时。对方法采取动态绑定是为了实现多态，多态是Java的一大特色，多态也是面向对象的关键技术之一，所以Java是以效率为代价来实现多态这是很值得的，所以在不影响整体设计的情况下，我们可以考虑将方法或者变量使用private，static或者final进行修饰。

内容来自互联网+个人见解，如果有哪里有问题，请联系我并指正，我会及时纠正处理。