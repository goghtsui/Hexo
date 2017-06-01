title: >-
  Android Studio升級2.2打包报错 - ...xxx-stripped.ap_ specified for property
  resourceFile does not exist
date: 2016-10-10 17:54:23
categories: [Android Studio]
tags: [Android Studio2.2, xxx-stripped.ap_]
---

## 序言

在升级了Android Studio 2.2 之后，使用Build -> Generate Signed Apk 打包apk报错，之前是一只没问题的，肯定是2.2的一些特性搞的鬼，google了一下，原来是 Instant Run 的问题。

## 问题

错误日志：

``` bash
 \build\intermediates\res\xxx-stripped.ap_' specified for property 'resourceFile' does not exist
```

<!-- more -->

## 解决方案

Files -> Settings -> Build, Execution, Development -> Instant Run -- 把第一个勾选去掉

![pPc68](http://i.stack.imgur.com/pPc68.png)

## 总结

首先说，我在module中使用了资源优化：

``` xml
buildTypes {
    release {
        shrinkResources false
        minifyEnabled false
        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
    }
}
```

如果将 _shrinkResources_ 设置成 true 就需要通过上面提到的设置，如果设置为false就不需要关闭 instant run 的功能了，因为 instant run 不支持 shrinkResources。

1. [stackoverflow的解决方案](http://stackoverflow.com/questions/36540676/build-intermediates-res-resources-anzhi-debug-stripped-ap-specified-for-prope)

2. [官方描述：关于 Instant Run](http://tools.android.com/tech-docs/instant-run)