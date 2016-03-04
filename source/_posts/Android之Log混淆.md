title: Android之Log混淆
date: 2015-11-18 16:38:59
categories: [Android]
tags: [Log]
---
作为Android开发工程师，项目开发的过程中，日志的打印是必不可少的，通过这些日志我们可以很好分析程序运行的状况与正确性，可以使用的日志输出有哪种形式呢？发布release版本应该怎么屏蔽掉这些日志呢？
<!-- more -->

### 日志形式
- **Java形式**
```
System.out.println(" log for test ");
```
这个一般不提倡使用
- **Android Log**
```
Log.d(TAG, " log for test ");
```
这个是Android标准的日志输出类：android.util.Log

### TAG定义
关于TAG的命名简单说一下，基本上有以下几种形式：
- **人名** 
```
Log.d("gogh" " log for test ");
```
与代码无关，无法定位日志的位置
- **类名**
```
private static final String TAG = LogUtils.class.getSimpleName();
Log.d(TAG, " log for test ");
```
经过混淆的类，类名会改变为a、b这种形式，相应的TAG值也会改变，同样无法定位相关代码域。

### TAG定义推荐
那么哪种形式的TAG定义相对来说比较规范呢？给大家推荐一种相对规范的定义形式：
```
public class Utils {
    
    public static final String TAG = "Utils";

    public String setText(String text){
        Logger.d(TAG, " setText text = " + text );
        // do something
    }
}
```
基本的规范是：
- **日志所在类**
- **日志所在的方法**
- **基本的信息**
这样的log是不是很好了就，对调试程序而言，比较直观，可读性不叫强，容易定位，关键是不会因为代码的混淆改变TAG的值。

### 屏蔽日志
我们的开发分为很多个阶段，但最终还是要有一个release版本发布，就需要把日志输出屏蔽掉，这一步相信大家都接触过了，那么就简单分析一下这一步。
#####运行时屏蔽
这个应该是使用的最多的，那么何为运行时屏蔽呢？就是在我们自定义的log工具类中设置一个值来开关日志，例如：
```
public class Logger {

    private static final boolean ENABLE = "true;
    
    public static void d(String tag, String message){
        if(ENABLE){
            android.util.Log.d(tag, message);
        }
    }
}

Logger.d(XXX, "VERSION = " + Build.VERSION.SDK_INT);
```
编写代码的过程中我们可以使用Logger.d(xxx, xxx);的形式来打印日志，在发布打包时将ENABLE修改为false就可以关闭日志。程序在运行的过程中就不会显示日志，但是message部分的方法（Build.VERSION.SDK_INT）还是执行到了，稍后解释。

##### 编译期屏蔽
这个就很简单了，在打包发布的时候加入代码混淆，如下：
```{bash}
-assumenosideeffects class com.gogh.Logger{
    public static *** i(...);
}
```
但是为了为了防止还有使用原生log的日志输出，直接混淆原生的log类，如下：
```{bash}
-assumenosideeffects class android.util.Log {
    public static *** v(...);
    public static *** d(...);
    public static *** i(...);
    public static *** w(...);
    public static *** e(...);
}
```
这样就可以达到平日日志输出的效果了

### 对比描述

运行时屏蔽其实很好理解，log中的日志输出是通过一个值控制的，这个输出的操作是在值判断通过之后进行的，而方法的调用需要传递多个参数，参数的传递肯定是在判断之前发生的，所以参数中的字串的拼接是会执行到的，方法同样也会被调用到，只是你看不到日志输出而已

编译期就是在编译过程中，Proguard进行优化，发生了内联操作，将dumpDebugInfo的被调用的方法体实现提取到调用的地方。在log相关的调用做了处理，结果是这里没有任何关于Logger.d(xxx,xxx)的调用，但是字串的拼接还是存在的，只是没有了方法的调用，这个可以通过反编译看看相关的代码片段

### 总结
理论上编译期屏蔽相对于运行期屏蔽更优