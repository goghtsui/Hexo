title: Android-Studio-之依赖方式详解(Compile, Provided...)
date: 2017-08-25 09:47:22
categories:
tags: [Android-Studio, Compile, Provided, Test compile, Debug compile, Release compile]
---

## 序言

我们在项目开发中，不可避免的需要使用到第三方的一些库，或者自己定义的一些lib，所以我们就需要在 build.gradle 文件添加对这些lib的依赖，代码如下：

``` gr
dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    compile 'com.android.support:support-v4:25.3.0'
    ...
    provided files('jar/framework.jar')
}
```

可以看到上面使用到了两种依赖的方式：**compile** **provided** ，那么还有没有其它依赖方式？都有哪些？

## dependencies

下面我们一起看看Studio给我们提供了那些依赖方式：

![studio_dependencies](http://7xod2d.com1.z0.glb.clouddn.com//studio/studio_six_dependencies.png)

其实 Studio 已经提供了6种依赖方式：**Compile**、**Provided**、**APK**、**Test compile**、**Debug compile**、**Release compile** 。**这里的名字和 dependencies 里面使用的命名是不用的** ，你可以添加几个试试看。那么问题来了，它们有什么不同？继续往下看

<!-- more -->

## 区别

### Compile

Compile 是对所有的 build type 以及 favlors 都会**参与编译并且打包**到最终的 apk 文件中

### Provided

Provided 是对所有的 build type 以及 favlors **只在编译时使用**，类似eclipse中的external-libs,只参与编译，不打包到最终apk

### APK

**只会打包到 apk** 文件中，而**不参与编译**，所以不能再代码中直接调用 jar 中的类或方法，否则在编译时会报错

### Test compile

Test compile 仅仅是**针对单元测试代码的编译以及最终打包测试apk时有效**，而对正常的 debug 或者 release apk 包不起作用

### Debug compile

Debug compile 仅仅针对 debug 模式的编译和最终的 debug apk 打包

### Release compile

Release compile 仅仅针对 release 模式的编译和最终的 release apk 打包

我相信你看到这些应该是恍然大悟了吧，根据不同的场景使用不同的依赖方式，一定要严谨！ 试问，还有其它的依赖方式吗？好像我在哪里看到过……

## 扩展

其实除了上面提供的6中依赖方式，还有一些支持的方式：**android-apt**、**annotationProcessor** 。

Android-apt 是由一位开发者自己开发的 apt 框架，源代码托管在[这里](https://bitbucket.org/hvisser/android-apt)，随着 android Gradle 插件 2.2 版本的发布，Android Gradle 插件提供了名为 annotationProcessor 的功能来完全代替 android-apt ，自此 android-apt 作者在官网发表声明最新的 Android Gradle 插件现在已经支持 annotationProcessor，并警告和或阻止 android-apt ，并推荐大家使用 Android 官方插件 annotationProcessor。

关于注解的使用，不是本文的重点，重点是依赖所提供的功能。

### android-apt & annotationProcessor

**只在编译的时候执行依赖的库，但是库最终不打包到apk中**。编译库中的代码没有直接使用的意义，也没有提供开放的 api调用，最终的目的是得到编译库中生成的文件，供我们调用。

## 总结

我曾经错误的将 compile 使用到了 provider 库，很悲剧的出现了方法数超过了65535，于是我开始改造项目，结果各种没效果，最后才了解到是依赖方式的问题，想想有多愚蠢！唉，年轻没经验啊！如果可以的话，尝试去使用每一种方式，看看有什么不同。

