title: 《阿里巴巴Java开发手册》1-3之格式规约
date: 2017-02-16 10:04:17
categories: [Java]
tags: [阿里巴巴Java开发手册, 格式规约]
---

## 编程规约 - 格式规约

1. 【强制】大括号的使用约定。如果是大括号内为空，则简洁地写成{}即可，不需要换行 ； 如果
   是非空代码块则：
   1 ） 左大括号前不换行。
   2 ） 左大括号后换行。
   3 ） 右大括号前换行。
   4 ） 右大括号后还有 else 等代码则不换行 ； 表示终止右大括号后必须换行。

2. 【强制】 左括号和后一个字符之间不出现空格 ； 同样，右括号和前一个字符之间也不出现空
   格。详见第 5 条下方正例提示。

3. 【强制】 if / for / while / switch / do 等保留字与左右括号之间都必须加空格。

<!-- more -->

4. 【强制】任何运算符左右必须加一个空格。
   说明：运算符包括赋值运算符=、逻辑运算符&&、加减乘除符号、三目运行符等。

5. 【强制】缩进采用 4 个空格，禁止使用 tab 字符。
   说明：如果使用 tab 缩进，必须设置 1 个 tab 为 4 个空格。IDEA 设置 tab 为 4 个空格时，
   请勿勾选 Use tab character ；而在 eclipse 中，必须勾选 insert spaces for tabs 。
   正例：  （ 涉及 1-5 点 ）

   ```java
   public static void main(String args[]) {

       // 缩进 4 个空格
       String say = "hello";

       // 运算符的左右必须有一个空格
       int flag = 0;

       // 关键词 if 与括号之间必须有一个空格，括号内的 f 与左括号，0 与右括号不需要空格
       if (flag == 0) {
           System.out.println(say);
   	}

       // 左大括号前加空格且不换行；左大括号后换行
       if (flag == 1) {
   		System.out.println("world");
       // 右大括号前换行，右大括号后有 else，不用换行
   	} else {
   		System.out.println("ok");
   	// 在右大括号后直接结束，则必须换行
   	}
   }
   ```

   ​

6. 【强制】单行字符数限制不超过 120 个，超出需要换行，换行时遵循如下原则：
   1） 第二行相对第一行缩进 4 个空格，从第三行开始，不再继续缩进，参考示例。
   2 ） 运算符与下文一起换行。
   3 ） 方法调用的点符号与下文一起换行。
   4 ） 在多个参数超长，逗号后进行换行。
   5 ） 在括号前不要换行，见反例。
   正例：

   ```java
   StringBuffer sb = new StringBuffer();
   //超过 120 个字符的情况下，换行缩进 4 个空格，并且方法前的点符号一起换行

   sb.append("zi").append("xin")...
   	.append("huang")...
   	.append("huang")...
   	.append("huang");
   ```

   反例：

   ```java
   StringBuffer sb = new StringBuffer();

   //超过 120 个字符的情况下，不要在括号前换行
   sb.append("zi").append("xin")...append
   ("huang");

   //参数很多的方法调用可能超过 120 个字符，不要在逗号前换行
   method(args1, args2, args3, ...
   , argsX);
   ```

7. 【强制】方法参数在定义和传入时，多个参数逗号后边必须加空格。
   正例：下例中实参的" a ",后边必须要有一个空格。

   ```java
   method("a", "b", "c");
   ```

8. 【强制】 IDE 的 text file encoding 设置为 UTF -8 ; IDE 中文件的换行符使用 Unix 格式，
   不要使用 windows 格式。

9. 【推荐】没有必要增加若干空格来使某一行的字符与上一行的相应字符对齐。
   正例：

   ```java
   int a = 3;
   long b = 4L;
   float c = 5F;
   StringBuffer sb = new StringBuffer();
   ```

   说明：增加 sb 这个变量，如果需要对齐，则给 a 、 b 、 c 都要增加几个空格，在变量比较多的
   情况下，是一种累赘的事情。

10. 【推荐】方法体内的执行语句组、变量的定义语句组、不同的业务逻辑之间或者不同的语义
   之间插入一个空行。相同业务逻辑和语义之间不需要插入空行。
   说明：没有必要插入多行空格进行隔开。

**以上内容均整理自《阿里巴巴Java开发手册》**

## 下载

> 提供Gitbook在线阅读和pdf下载：[查看福利](https://www.gitbook.com/book/goghtsui/-java/details)