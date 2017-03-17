title: Android Studio 2.4 Preview 1 发布了！
date: 2017-03-17 11:14:12
categories: [Android Studio]
tags: [Android Studio 2.4 Preview]
---

Android Studio 2.4 Preview 1 版本于2017年3月15日，由Chris Iremonger 发布。但是这个更新只发布到了Canary的开发渠道。所以你需要设置你的Android Studio的跟新渠道为 **Canary**，而且这是一个初期的版本，所以在接下来的几周会有更多的更新。到底这个版本做了哪些改进呢？让我们一睹为快吧：

## 代码

- 在Android Studio 2.4 Preview 1 中，我们升级了IDE从 IntelliJ 2016.2 到 2017.1 EAP，并在 [2016.3](https://www.jetbrains.com/idea/whatsnew/#v2016-3)和[2017.1](https://blog.jetbrains.com/idea/2017/02/intellij-idea-2017-1-public-preview-java-9-debugger-vcs-search-editor-and-many-more/) 中增加了许多新功能，包括参数提示，语义突出显示，搜索即时结果，等等。
- 许多新的 **lint** 检查

## Instant Run

- Instant Run Debug [Issue 234401](https://code.google.com/p/android/issues/detail?id=234401) 现在应该已经解决。如果程序在断点处暂停，则应用程序重新启动。但是如果应用程序没有在断点上暂停且当你只有一个方法实现更改时，它不应该重新启动而且热更新应该工作。

## Build

- 增量dex'ing。Dex'ing现在是在每个类级别完成的。这将允许更多增量，并会导致更快的增量构建。你应该也期望在使用传统多DEX的条件下，提高构建应用的速度（minSdkVersion <21）

- 执行时的依赖性解析。在以前的版本中，依赖解析在Gradle配置期间发生。通过将依赖关系解析移动到执行期间

  ，你应该期望为大型项目改进配置时间。

## IDE

- 在Mac上 Android Studio 被称为“Android Studio 2.4 Preview.app”，使你更容易运行的2.3。
- *设备文件浏览器* -无缝查看，直接在Android Studio中修改和与设备文件系统交互。此功能取代了以前通过DDMS完成设备文件系统的交互（[Dalvik的调试监控服务器）](https://developer.android.com/studio/profile/ddms.html)
<!-- more -->

- ![img](https://lh4.googleusercontent.com/_XRjD-mTv3eKoW-x1Q-VHs3foxEDbt2Xs0tmaFt1i1clcsHDKWd6cmH_RpjcOHRO4ICkhYVTtpbYyqM2Ne6JvcJs-xs22FqUBRnJb7nNYCN075BJ2R_7cAhzxV5Ty5gjF3VY-sLW)

## 已知的问题

- 如果你检查更新，它会告诉你有一个新版本 Android Studio 2.4 Preview 1（Build171.3804684） 。如果您已经安装了相同的版本，请不要尝试重新下载。我们将在 Android Studio 2.4 Preview 2 中修复。

  [![img](https://sites.google.com/a/android.com/tools/_/rsrc/1489537129853/download/studio/builds/android-studio-2-4-preview-1/Screen%20Shot%202017-03-14%20at%205.01.48%20PM.png?height=173&width=400)](http://tools.android.com/download/studio/builds/android-studio-2-4-preview-1/Screen%20Shot%202017-03-14%20at%205.01.48%20PM.png?attredirects=0)

- Mac版本可能会提示您无法打开它，因为它是来自不明身份的开发人员。邮件似乎已签名，但有问题。我们将在预览2中更新它。如果您想在预览1中尝试，请右键单击并选择打开

> [Android Studio 官方描述](http://tools.android.com/recent/androidstudio24preview1isnowavailable)