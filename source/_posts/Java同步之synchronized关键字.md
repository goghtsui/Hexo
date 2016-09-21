---
title: Java同步之synchronized关键字
date: 2016-04-29 10：11:22
categories: [Java]
tags: [synchronized]
---

### 序言
在开发中，避免不了多任务的操作，往往一个线程很难满足任务需要，所以就有了多线程，并行的任务，但是当使用多个线程来访问同一个数据时，非常容易出现线程安全问题(比如多个线程都在操作同一数据导致数据不一致),所以我们用同步机制来解决这些问题，其中的一种解决方式就是使用synchronized关键字

### 使用
synchronized主要有四种用法：

- 第一是在方法声明时使用

> 放在范围操作符(public等)之后,返回类型声明(void等)之前。这时,线程获得的是 _成员锁_,即一次只能有一个线程进入该方法,其他线程要想在此时调用该方法,只能排队等候,当前线程(就是在Synchronized方法内部的线程)执行完该方法后,别的线程才能进入，例：

``` java
public synchronized void method() {
    // do something
}
```
<!-- more -->

- 第二是针对某一代码块使用

> synchronized后跟括号,括号里是变量,这样,一次只有一个线程进入该代码块，此时，线程获得的是 _成员锁_

``` java
public int method(int arg1){
    synchronized(arg1) {
        //一次只能有一个线程进入
    }
}
```

- 第三是对某一对象使用

> synchronized后面括号里是对象,此时,线程获得的是 _对象锁_

``` java
public void method(String arg){   
    synchronized (this){
        //取得该类实例化后对象的锁   
    }   
} 
```
等同于
``` java
public synchronized void method(String arg){ 
    //取得Demo实例化后对象的锁   
} 
```

- 第四是对某一类使用。

> synchronized后面括号里是类,此时,线程获得的是 _对象锁_

``` java
public static void method(String arg){   
    synchronized (Demo.class) { 
        //取得Demo.class类的锁  
}
```
等同于：

``` java
public synchronized static void method(String arg){
    //取得Demo.class类的锁   
} 
```
### 性能

实现同步机制注意以下几点
- 多线程：安全性高，性能低
- 单线程：性能高，安全性低

所以：
- 不要对线程安全类的所有方法都进行同步操作，只对那些持有共享资源的方法进行同步
- 如果该类有两种运行环境，单线程环境和多线程环境。则应该为该类提供两种版本：线程安全版本和线程不安全版本(没有同步方法和同步块)。在单线程中环境中，使用线程不安全版本以保证性能，在多线程中使用线程安全版本.

### 其它
- 当两个并发线程访问同一个对象object中的这个synchronized(this)同步代码块时，一个时间内只能有一个线程得到执行。另一个线程必须等待当前线程执行完这个代码块以后才能执行该代码块

- 当一个线程访问object的一个synchronized(this)同步代码块时，另一个线程仍然可以访问该object中的非synchronized(this)同步代码块
     
- 尤其关键的是，当一个线程访问object的一个synchronized(this)同步代码块时，其他线程对object中所有其它synchronized(this)同步代码块的访问将被阻塞

 - 第三个例子同样适用其它同步代码块。也就是说，当一个线程访问object的一个synchronized(this)同步代码块时，它就获得了这个object的对象锁。结果，其它线程对该object对象所有同步代码部分的访问都被暂时阻塞。

` 注：synchronized关键字可以修饰方法，也可以修饰代码块，但不能修饰构造器，属性等 `