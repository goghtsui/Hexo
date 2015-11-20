title: 解决Android Studio中Terminal无法输入的问题
tags: [Android Studio, Terminal]
date: 2015-11-16 10:14:09
categories: [Android]
---
### 问题描述：
```
Windows系统下Android Studio中的Terminal无法获取焦点，不能输入文字。
```
### 问题原因：
这个是因为系统升级造成的不兼容问题，在Windows环境开发的朋友们估计早早的就升级Win10了吧，UI非
常的炫酷。然而AS中的Terminal使用的还是Windows中的cmd控制台，也就是位于
C:\Windows\System32\目录下的cmd.exe。Win10下的cmd相对于早期版本的cmd做了一些改进，导
致了这里描述的问题。

### 解决方案：
下面给出具体的操作步骤，有图有真相：

1、win+R组合键打开运行窗口，输入cmd ，点击OK

![cmd窗口](http://7xod2d.com1.z0.glb.clouddn.com/cmd.png)

2、在出现的cmd窗口中，右键点击标题栏->Properties，出现如下窗口：

![Properties窗口](http://7xod2d.com1.z0.glb.clouddn.com/settings.png)

3、勾选 Use legacy console（requires relaunch）即使用旧版控制台（需要重启生效），就这么简单的操作就可以解决问题了，赶快试试吧！
