title: 《阿里巴巴Java开发手册》1-2之常量定义
date: 2017-02-15 14:22:17
categories: [Java]
tags: [阿里巴巴Java开发手册, 常量定义]
---

## 编程规约 - 常量定义

1. 【强制】不允许出现任何魔法值 （ 即未经定义的常量 ） 直接出现在代码中。
   反例：  

   ``` java
   String key =" Id # taobao _"+ tradeId；
   cache . put(key ,  value);
   ```

2. 【强制】 long 或者 Long 初始赋值时，必须使用大写的 L ，不能是小写的 l ，小写容易跟数字
   1 混淆，造成误解。
   说明： 

   ```java
   Long a = 2 l; 
   ```

   写的是数字的 21，还是 Long 型的 2?

3. 【推荐】不要使用一个常量类维护所有常量，应该按常量功能进行归类，分开维护。如：缓存
   相关的常量放在类： CacheConsts 下 ； 系统配置相关的常量放在类： ConfigConsts 下。
   说明：大而全的常量类，非得使用查找功能才能定位到修改的常量，不利于理解和维护。

4. 【推荐】常量的复用层次有五层：跨应用共享常量、应用内共享常量、子工程内共享常量、包
   内共享常量、类内共享常量。
   1 ） 跨应用共享常量：放置在二方库中，通常是 client . jar 中的 constant 目录下。
   2 ） 应用内共享常量：放置在一方库的 modules 中的 constant 目录下。
   反例：易懂变量也要统一定义成应用内共享常量，两位攻城师在两个类中分别定义 了
   表示“是”的变量：
   类 A 中： 

   ```java
   public static final String YES = " yes " ;
   ```

   类 B 中： 

   ```java
   public static final String YES = " y " ;
   ```

   A . 
   ```java
   YES . equals(B . YES)
   ```
   预期是 true ，但实际返回为 false ，导致产生线上问题。
   3 ） 子工程内部共享常量：即在当前子工程的 constant 目录下。
   4 ） 包内共享常量：即在当前包下单独的 constant 目录下。
   5 ） 类内共享常量：直接在类内部 private static final 定义。

5. 【推荐】如果变量值仅在一个范围内变化用 Enum 类。如果还带有名称之外的延伸属性，必须
   使用 Enum 类，下面正例中的数字就是延伸信息，表示星期几。
   正例： 

   ```java
   public Enum {  MONDAY( 1 ) ,  TUESDAY( 2 ) ,  WEDNESDAY( 3 ) ,  THURSDAY( 4 ) ,  FRIDAY( 5 ) ,SATURDAY( 6 ) ,  SUNDAY( 7 ); }
   ```

**以上内容均整理自《阿里巴巴Java开发手册》**

## 下载

> 提供Gitbook在线阅读和pdf下载：[查看福利](https://www.gitbook.com/book/goghtsui/-java/details)