title: Android Studio | Intellij 翻译插件 Easy Translation
date: 2016-08-16 22:08:34
categories: [Android Studio]
tags: [Intellij IDEA, Android Studio, plugin]
---


## 序言

相信现在做Android开发的都已经从Eclipes转Android Studio（AS）了吧，我们都知道，它是Google推出的，基于IntelliJ IDEA的开发工具，提供了集成的 Android 开发工具用于开发和调试。
- 基于Gradle的构建支持
- Android 专属的重构和快速修复
- 提示工具以捕获性能、可用性、版本兼容性等问题
- 支持ProGuard 和应用签名
- 基于模板的向导来生成常用的 Android 应用设计和组件
- 功能强大的布局编辑器，可以让你拖拉 UI 控件并进行效果预览

<!-- more -->
## 使用

为了学技术不得不一点一点的肯那些英文文档，在AS中我们官方注释也是英文的，单词还要复制出来找google翻译，是不是很麻烦，为了方便大家，我写了一个简单的翻译插件，没错，遇到不懂得单词，马上就能翻译出来，和有道词典的效果差不多，其实是可以中英互译的，该插件支持基于IntelliJ IDEA的开发环境，下面就说说在AS下怎么使用：

- 直接在编译器下载安装：**File -> Settings -> Plugins -> Browse Repositories -> 搜索Easy-Translation -> 点击右侧的Install -> 重启** 即可。
- AS可以从官方下载手动安装，插件是以jar的形式提供的，打开AS：**File -> Settings -> Plugins -> Install plugin from disk -> 选择jar包 -> 重启**即可。
>[下载地址1](http://plugins.jetbrains.com/plugin/8553?pr=idea)
[下载地址2](http://plugins.jetbrains.com/plugin/8556?pr=idea)

![Screenshot](https://plugins.jetbrains.com/files/8556/screenshot_16110.png)


## 注意

其实都可以通过手动安装的形式，大家自行选择。在编写代码或者查看代码时，**双击选中单词**，然后通过快捷键**Alt+A**键，马上就能看到翻译后的结果了，如果出现乱码，可以修改一下字体：**File -> Settings -> Appearance&Behavior -> Appearance -> UI Options -> Name(eg. Microsoft YaHei)**

目前弹窗的显示会持续5秒，暂时还不支持灵活的开关，如果有使用上的问题欢迎大家及时反馈。
