title: Android Emulator 更新了 伤害爆炸
date: 2017-12-28 15:44:59
categories: [Android Studio]
tags: [Android Emulator, Quick Boot]
---

## 序言

最近Google新版的模拟器，性能真是杠杠的，秒开，而且比Genymotion还要快，你敢信？所以，你可以告别额外的软件安装，告别使用真机调试造成的一万点暴击伤害。并且添加了很多功能的支持，还是很强大的，兄dei 你还不赶紧试试？ 下面是官方的内容。

> [原文地址（需要翻墙）](https://android-developers.googleblog.com/2017/12/quick-boot-top-features-in-android.html) 

## 介绍

发布者：Android 产品经理 [Jamal Eason](https://www.google.com/+JamalEason)

[![img](http://bp.googleblog.cn/-KKfHGyztPBY/WjfzJPsk1gI/AAAAAAAAE6E/zuXucIqvQ04lmZUo602BTdXSAu61vkKoACLcBGAs/s1600/image3.png)](http://7xod2d.com1.z0.glb.clouddn.com//emulator/emulator_logo.png)

今天，我们高兴地宣布为 Android Emulator 推出 Quick Boot 功能。利用 Quick Boot，您可以在 6 秒内启动 Android Emulator。Quick Boot 会为模拟器会话拍摄快照，因此您可以在数秒内重新加载。Quick Boot 最初随 Android Studio 3.0 在 Canary 更新版本渠道中发布，今天，我们非常高兴地以稳定更新版本形式发布此功能。

除了这个新功能，我们还希望重点介绍一下近期版本中的一些热门功能。自从[两年前](https://android-developers.googleblog.com/2015/12/android-studio-20-preview-android.html)对 Android Emulator 进行彻底改造以来，我们继续侧重于提升速度、稳定性，以及添加众多功能，加快您的应用开发和测试的速度。鉴于所有近期变更，对您来说，今天绝对值得更新到最新版本的 Android Emulator 并开始使用这些功能。

## 5 大功能

- **Quick Boot** 

  今天以稳定功能形式发布，Quick Boot 让您可以在 6 秒内恢复 Android Emulator 会话。在您首次启动带 Android Emulator 的 Android Virtual Device (AVD) 时，它必须执行冷启动（就像接通设备电源），但是，后续启动的速度会非常快，系统将恢复到您上次关闭模拟器时的状态（类似于唤醒设备）。为此，我们完全重新设计了旧版模拟器快照架构，以便与虚拟传感器和 GPU 加速搭配使用。无需其他步骤，因为从 Android Emulator v27.0.2 起，Quick Boot 默认处于启用状态。

  如图：*Android Emulator 中的 Quick Boot*

[![img](http://bp.googleblog.cn/-TKy0vJfZ5vA/WjfzdkppCEI/AAAAAAAAE6I/Zkv4O_k_Z9wq0cdZJ4cv6m0XTYCjmVHtQCLcBGAs/s1600/image2.gif)](http://7xod2d.com1.z0.glb.clouddn.com//emulator/Quick_Boot_in_the_Android_Emulator.gif)

​ 
<!-- more -->                                  

- **Android CTS 兼容性**  

  在每一个版本的 Android SDK 中，我们都会确保 Android Emulator 可以立即解决您的应用开发需求，从测试与 Android KitKat 的向后兼容性到集成开发者预览版的最新 API，不一而足。为了提高模拟器系统映像的产品质量和可靠性，现在，我们针对 [Android 兼容性测试套件](https://source.android.com/compatibility/) (CTS) 将最终 Android 系统映像版本号限定为 Android Nougat (API 24) 及更高版本 - 官方的 Android 实体设备必须通过相同的测试套件。


- **Google Play 支持**

  我们知道许多应用开发者都使用 Google Play 服务，在 Android Emulator 系统映像中确保服务处于最新状态可能非常困难。为了解决这个问题，我们现在提供多种版本并且包含 Play 商店应用的 Android 系统映像。Google Play 映像支持 Android Nougat (API 24) 及更高版本。利用这些新的模拟器映像，您可以在模拟器中通过 Play 商店应用更新 Google Play 服务，就像您在实体的 Android 设备上操作一样。此外，您现在还可以通过 Google Play 商店测试端到端安装、更新和购买流程。


- **性能改进 **

  让模拟器快速和高效是我们团队的一个持续目标。我们会持续关注在您的开发机器上运行模拟器的性能影响，尤其是 RAM 使用情况。在最近几个版本的 Android Emulator 中，我们现在可以按需分配 RAM，而不是分配内存并将其固定为在您的 AVD 中定义的最大 RAM 大小。为此，我们将本机管理程序用于 Linux (KVM) 和 macOS® (Hypervisor.Framework)，将增强的 Intel® HAXM（v6.2.1 及更高版本）用于 Microsoft® Windows®，后者使用全新的按需内存分配机制。

- ​

- 此外，在过去几个版本中，我们还提升了 CPU 和 I/O 性能，同时增强了 GPU 性能，包括 OpenGL ES 3.0 支持。下面通过 ADB 推送等常见任务突出显示了 Android CPU 和 I/O 管道的改进（图例：*Android Emulator 的 ADB 推送速度比较*）：

- [![img](http://bp.googleblog.cn/-vR9KU5AIfRI/WjfzpyInlmI/AAAAAAAAE6Q/7fiLLaGkIrYPVM1ylpSqPk5QszzEnKfswCLcBGAs/s1600/image5.png)](http://7xod2d.com1.z0.glb.clouddn.com//emulator/ADB_Push_Speed_Comparison_with_Android_Emulator.png)

- 对于 GPU 性能，我们创建了一个示例[GPU 仿真压力测试应用](https://github.com/google/gpu-emulation-stress-test)来衡量一段时间的性能改进。我们发现，最新的模拟器可以比之前渲染更高的帧速率，而且它是少数几个可以按照 Android 规范准确渲染 OpenGL ES 3.0 的模拟器之一。

- 如图：*GPU 仿真压力测试 - Android 应用*

[![img](http://bp.googleblog.cn/-bgYW6_GU8bY/Wjfz4zv_OAI/AAAAAAAAE6U/ECTtrleCUSs-dksSBIJ5T62-ymx9TmDbQCLcBGAs/s1600/image1.gif)](http://7xod2d.com1.z0.glb.clouddn.com//emulator/GPU_Emulation_Stress_Test.gif)



如图：*Android Emulator 的 GPU 仿真压力测试*

[![img](http://bp.googleblog.cn/-gOcq-URxxTM/WjgIWLtf20I/AAAAAAAAE6o/E2nhpU_hFcQcC2qxxOQHK3QVOawhLgY6ACLcBGAs/s1600/gpu_emulator_stress_test_chart.png)](http://7xod2d.com1.z0.glb.clouddn.com//emulator/gpu_emulator_stress_test_chart.png)



## 更多功能

除了这些主要功能外，我们在过去一年还为 Android Emulator 添加了很多其他功能，大家可能没意识到：

- **WLAN 支持** 

  从 API 24 系统映像开始，您可以创建一个能够同时连接到虚拟蜂窝网络和虚拟 WLAN 接入点的 AVD。

- **Google Cast 支持** 

  使用 Google Play 系统映像时，您可以将屏幕和音频内容投射到位于同一个 WLAN 网络上的 Chromecast 设备。

- **拖放 APK 和文件** 

  只需将 APK 拖放到 Android Emulator 窗口上即可触发应用安装。您也可以拖动任何其他数据文件，并在 Android Virtual Device 的 /Downloads 文件夹中找到。

- **主机复制与粘贴** 

  您可以在 Android Emulator 与您的开发机器之间复制和粘贴文本。

- **虚拟双指张合与缩放** 

  在与 Google 地图等应用交互时，按下 Ctrl 键（在 Microsoft® Windows® 或 Linux 上）或者 ⌘（在 macOS® 上），屏幕上将出现一个手指叠加层来协助进行张合与缩放操作。

- **GPS 位置**

  在 Android Emulator 的 Location 标签下手动选择一个 GPS 点或一组 GPS 点。

- **虚拟传感器** 

  扩展的控制面板中有一个专门的页面，已在 Android Emulator 中支持加速、旋转和近程等传感器。

- **WebCam 支持** 

  您可以将网络摄像头或笔记本电脑的内置网络摄像头用作 AVD 的虚拟摄像头。在 AVD Manager 的 Advanced Settings 页面中验证您的 AVD 摄像头设置。

- **主机键盘**

  您可以使用自己的实体键盘向 Android Virtual Device 输入文本。

- **虚拟短信和通话** 

  在扩展的控制面板中，您可以触发虚拟的短信或通话来测试具有电话依赖关系的应用。

- **屏幕缩放** 

  在主工具栏上，点击放大镜图标进入缩放模式，然后选择您想要检查的屏幕区域。

- **调整窗口大小**

  只需拖动 Android Emulator 窗口的一个角即可更改为所需大小。

- **网络代理支持** 

  转到 Settings 页面的 Proxy 标签，为您的 Android Emulator 会话添加一个自定义 HTTP 代理。

- **错误报告** 

  使用扩展的控制面板中的 Bug Report 部分，您可以为自己的应用快速生成错误报告，与您的团队分享或向 Google 发送反馈。

在 [Emulator 文档](https://developer.android.google.cn/studio/run/emulator.html)中详细了解 Android Emulator。

## 开始使用

现在，所有这些功能和改进都可以在 Android Emulator v27.0.2+ 中下载和使用，您可以在 Android Studio 中通过 SDK 管理器获取支持的 Android Emulator 版本。为了获得快速体验，我们建议创建和运行 x86 版本的模拟器系统映像，并安装最新的 Android Emulator、Intel® HAXM（如适用）和图形驱动程序。

我们感谢您提供有关您喜欢的特性、存在的问题或希望看到的功能的任何反馈意见。如果您发现错误或问题，或者想要分享功能请求，欢迎随时向我们[提交问题](https://developer.android.google.cn/studio/report-bugs.html#emulator-bugs)。我们的工作远未完成，但我们希望大家对我们目前的改进感到兴奋。