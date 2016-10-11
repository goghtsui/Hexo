title: Android Studio 2.2 新特性
date: 2016-10-11 17:54:42
categories: [Android Studio]
tags: [Android Studio2.2]
---

## 序言

![as2.2_character](https://static.oschina.net/uploads/space/2016/0924/070554_J51U_2720166.png)
今年的 I/O 2016 Google 放出了 Android Studio 2.2 的预览版，透露改进了多项功能，现在已经发布了 Android Studio 2.2 的正式版，按照 Google 的说法，此版本包含增强功能，主要面向三大主题：速度、智能和 Android 平台支持。通过新增的布局编辑器等功能加快开发速度，快速而直观地创建应用用户界面。利用新增的 APK 分析器、增强的布局检查器、扩展的代码分析、IntelliJ 的 2016.1.3 功能等，进行更智能的开发。
<!-- more -->

作为正式版 Android 应用开发 IDE，Android Studio 2.2 支持 Android 7.0 Nougat 中所有最新的开发者功能（例如代码自动完成），可帮助您添加多种 Android 平台功能，例如多窗口支持、Quick Settings API 或重新设计的通知，当然还有用于测试这些功能的内置 Android Emulator。 

在此版本中，我们将 Android Frameworks 与 IDE 整合到一起创建约束布局。这个全新的布局管理器功能强大，可帮助您以扁平的流线型层次结构设计庞大而复杂的布局。ConstraintLayout 是与新的布局编辑器同时构建的，可像标准 Android 支持库那样集成到您的应用中。

![new_character](https://static.oschina.net/uploads/space/2016/0924/070632_pSbD_2720166.png)

Android Studio 2.2 新增了 20 多项功能，涉及开发流程的每个主要阶段：设计、开发、构建和测试。从使用新的 ConstraintLayout 设计 UI，到使用 Android NDK 开发 C++ 代码，到使用最新的 Jack 编译器进行构建，再到为应用创建 Espresso 测试案例，Android Studio 2.2 都是您不容错过的最新版本。


## 新特性

### 设计

#### Layout Editor(布局编辑器)

本次更新带来了全新的布局编辑器，我们以后调 UI 将更方便。打开一个 XML 文件，默认的 Design 模式如下图所示，主要包含 Palette、Component Tree、Toolbar、Design Editor、Properties 五部分，直接可视化的操作使布局更加方便易操作。

![layout_editor](http://stormzhang.com/image/layout-editor-callouts_2-2_2x.png)

当然对于习惯写 XML 代码的同学来说可以点击左下角的 Text 切换到代码格式，但是右边依然可以实时预览。Text 模式下的截图如下：

![text_mode](http://stormzhang.com/image/layout-editor-text_2-2_2x.png)

这里有个小技巧，可以操作快捷键 Control+Shift+Right/Left 来进行左右切换。然后我们可以通过 Toolbar 那一栏来配置我们预览的主题外观

![text_mode](http://stormzhang.com/image/layout-editor-toolbar1-callouts_2-2_2x.png)

#### Constraint Layout(约束布局)

Constraint Layout 翻译过来我把它叫约束布局，它也是今年 Google 全新推出的一种布局，它更强大，简单来说，用 Constraint Layout 可以实现之前需要各种嵌套才能实现的效果，我们知道过多的布局嵌套对性能影响是很大的，因为 Constraint Layout 更强大，所以属性也就特别多，不过 Google 完全提供了一种可视化的操作，一张动图你们感受下：

![text_mode](http://stormzhang.com/image/image00.gif)

关于 Constraint Layout 的详细用法 Google 官方有个教程，想学习的可以看一下：

[Using ConstraintLayout to design your views](https://codelabs.developers.google.com/codelabs/constraint-layout/)

上面链接需要科学上网，英文阅读有困难的不妨看下这篇博客：

[Android ConstraintLayout详解](http://www.jianshu.com/p/a8b49ff64cd3)

### 开发

#### Samples Browser

GitHub 上 Google 有个叫 Google Samples 的组织，罗列了 Google 的上百个关于一些代码的示例，而这其中大部分都是 Android 相关的，比如 NavigationDrawer 不会用了，google 有个 android-NavigationDrawer 的示例。而这次 Google 直接把他关联到 Android Stduio 了，你可以在 Android Studio 选中一个类直接右键点击 Find Sample Code ，神奇的事情发生了：

![sample-code](http://stormzhang.com/image/code_sample.png)

上图可以看到以选中 PackageManager 为例，下面直接出现了一些 Google Sample 相关的代码，方便你快速查找该用法，而且还有个链接直接指向到 Android Developer 官网该类的详细介绍

#### Improved C++ Support（改进的 C++ 支持）

现在可以使用 CMake 或 ndk-build 从 Gradle 编译 C++ 项目。现在可将项目从 CMake 构建系统无缝迁移到 Android Studio。Android Studio 中的新项目向导对 C++ 提供了支持，此外，还对 C++ 编辑和调试体验进行了大量的问题修复。

![C_plus_plus-code](https://2.bp.blogspot.com/-fN7u1isHtDg/V-ATulF2JdI/AAAAAAAADZs/pLfGX_85NXomeFgfiIP3sGolu3QdiYQsgCLcB/s640/C_plus_plus.png)

### 构建

#### Instant Run Improvements(Instant Run改进)

Instant Run 的推出确实很不错，但是第一次编译会比较慢。我们先来看下 Google 官方的更新说明：

在此版本中，我们对 Instant Run 的稳定性和可靠性进行了大量的改进。如果您之前禁用了 Instant Run，建议重新启用，如果今后仍遇到问题，请告诉我们。（Settings → Build, Execution, Deployment → Instant Run [适用于 Windows/Linux], Preferences → Build, Execution, Deployment → Instant Run [适用于 OS X]）。打开方法见下图：

![instant-run](http://stormzhang.com/image/image05.png)

#### Build cache (Experimental)(缓存构建)

升级2.2之后会提示升级gradle
![gradle](http://stormzhang.com/image/as2.2.png)

为了加快 Gradle 的编译速度，Google 新增了一个编译缓存的功能，不过目前还是实验性的，具体用法就是在你的 gradle.properties 文件里加上这么一行代码：

``` xml
android.enableBuildCache=true
```

总体来说升级了 Gradle，加上这么一句代码，确实感觉编译快了些，大家可以自行感受下。每次编译生成的缓存在 ~/users/.android/build-cache 目录下，如果缓存过多可以手动删除该目录进行清除。


#### APK Analyzer（apk解析器）

Google 推出了一个 APK 分析器，现在可以很方便的使用 Android Studio 进行 APK 分析。具体用法点击 Build -> Analyze APK 然后选择你要分析的 APK 文件就可以了。

- 可以方便的查看全部文件和大小

![apk-file-sizes_2x](http://stormzhang.com/image/apk-file-sizes_2x.png)


- 可以直接查看 AndroidManifest.xml 文件

![apk-manifest-error_2x](http://stormzhang.com/image/apk-manifest-error_2x.png)

- 可以直接查看资源文件

![preview_2x](http://stormzhang.com/image/apk-image-preview_2x.png)

![strings_2x](http://stormzhang.com/image/apk-strings_2x.png)

- 可以直接查看 dex 文件

![multidex_2x](http://stormzhang.com/image/apk-multidex_2x.png)

- 两个 apk 进行比较

![compare_2x](http://stormzhang.com/image/apk-compare_2x.png)

以后人人都会逆向 APK 了。


### 测试

#### Virtual Sensors in the Android Emulator（虚拟传感器）

Google 这次同样改进了模拟器，这次让模拟器支持虚拟传感器，你们感受下。

![image02](http://stormzhang.com/image/image02.gif)

#### Espresso Test Recorder (Beta)（测试记录器（测试版））

Google 为测试新增了一个功能，就是我们可以对操作进行录像，然后根据我们的操作生成一些测试脚本，而且配合 Firebase 将更方便。

![image10](http://stormzhang.com/image/image10.png)

理论上来说此功能很不错，可以解放了测试人员的双手，只不过该功能还是测试，应该很不稳定，而且国内行情结合 Firebase 很困难，对开发意义不大，可以持续关注。

#### GPU Debugger (Beta)（GPU 调试程序（测试版））

GPU 调试程序现在为测试版。现在，您可捕获 Android 设备上的 OpenGL ES 命令流，然后在 Android Studio 内重播该命令流以便对其进行分析。也可全面检查任何指定 OpenGL ES 命令的 GPU 状态，以更好地了解和调试您的图形输出。

![image11](https://1.bp.blogspot.com/-2IprWPLlQcs/V-AWAlo-SlI/AAAAAAAADaQ/0ppF6MZ8CaQTHpYX7qXV-zrRk28IOlzBQCLcB/s640/image11.png)

## 总结

除以上之外，此次更新还包括对 Java 8 的支持，Jack 编译器的改进，可以调试 GPU，改进了对 C++ 的支持等，总体来说此次更新推出了不少提升 Android 开发效率的工具，性能上也做了优化，值得大家更新！

1. [官方更新说明](http://android-developers.blogspot.jp/2016/09/android-studio-2-2.html)
2. [官方各版本描述](https://developer.android.com/studio/releases/index.html)