title: Android知识点之enum
date: 2017-01-19 15:18:15
categories: [Android]
tags: [enum, Annotations, IntDef, StringDef]
---

## 序言

### 问题

> enum在Android应不应该使用？

### 解决方案

关于enum，Android Developers中这样一句话：

>  enums often require more than twice as much memory as static constants. You should strictly avoid using enums on Android

就是说enum比静态常量两倍多的内存占用，应该尽量减少使用。当然了，这里说的是减少并不是禁止，要知道，enum的产生，注定其必定有适合的应用场景，而且它带来的代码简洁性和可读性，都是不可小觑的。

所以，如果在不使用enum的情况下是有替代方案的：

- 使用静态常量的形式替代
- 使用Typedef Annotations替代

第一种没什么说的，下面就重点说一说第二种，就是通过注解的形式来代替，注解是由 support 包提供的功能，如果使用，需要添加 support 包到module。

直接上代码吧：

``` java
// 定义申请常量列表，声明NavigationMode形式的注解（三个注解是固定形式）
@IntDef({NAVIGATION_MODE_STANDARD, NAVIGATION_MODE_LIST, NAVIGATION_MODE_TABS})
@Retention(RetentionPolicy.SOURCE)
public @interface NavigationMode {}

// 声明静态常量
public static final int NAVIGATION_MODE_STANDARD = 0;
public static final int NAVIGATION_MODE_LIST = 1;
public static final int NAVIGATION_MODE_TABS = 2;
```

如果它被当作参数或者返回值时你可以这样写：

``` java
// 使用注解装饰目标方法
@NavigationMode
public abstract int getNavigationMode();
// 添加注解
public abstract void setNavigationMode(@NavigationMode int mode);
```

相信看到这些代码，就不需要什么说明了吧，就按照这样的形式使用就行。通过注解的方式去实现，你可以通过@IntDef和@StringDef来声明不同的数据类型。

## 结论

如果你还不太确定你应不应该使用enum，你可以看看这篇文章：

[在Android中到底该不该使用enum](http://stackoverflow.com/questions/29183904/should-i-strictly-avoid-using-enums-on-android)

