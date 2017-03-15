title: Android Studio 2.3全面解析
date: 2017-03-15 19:30:32
categories: [Android Studio]
tags: [Android Studio 2.3]
---

## 序

![img](http://mmbiz.qpic.cn/mmbiz_jpg/rFWVXwibLGtym7MN6ZrPapaQNneCcToStScYTdJuGicmk0ic8VAnooxehsToV0Tib94uMicen0T5riahAOXNCpia2OayA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)

Android Studio 2.3 已提供下载了，下面让我们来看看官方的描述：

Android Studio 2.3 中最令人激动的是质量上的改进，但此版本也加入了少量新功能，它们集成到了开发流程的每一个阶段：

- 设计应用时，请充分利用面向应用图像的更新版 WebP 支持，也请了解一下更新版ConstraintLayout 内容库支持以及布局编辑器中的小部件选项板。
- 在开发过程中，Android Studio 新增了一个应用链接助手，它可以帮助您构建一个应用 URI 合并视图，方便您统一查看应用内的 URI。
- 在构建和部署应用时，使用更新版运行按钮可获得更加直观而又可靠的 Instant Run 体验。
- 最后，在使用 Android Emulator 测试应用时，您现在可以获得充分的文本复制与粘贴支持。

<!-- more -->

## 构建

### 1. Instant Run 改进和 UI 变化

![img](http://mmbiz.qpic.cn/mmbiz_png/rFWVXwibLGtym7MN6ZrPapaQNneCcToSt8cy7ohYvKKv2nKSxiaZeROLuIyHtNSUHqArrpXzictTPUeeoK6y5YWZA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

新增的 Instant Run 按钮操作

为体现对质量的重视，我们在 Android Studio 2.3 中对 Instant Run 进行了一些重大更改，以提高该功能的可靠性。Run 操作现在一律会导致应用重新启动，以便让可能需要重新启动的代码更改生效，新增的 Apply Changes 操作会尝试在应用运行时改写代码。为提升可靠性，底层实现进行了大幅度改动，并且还消灭了支持 Instant Run 应用的启动延迟。

**[了解详情](developer.android.google.cn/studio/run/index.html#instant-run)**

### 2. 构建缓存

在 Android Studio 2.2 中引入但默认情况下处于停用状态，是一项旨在加快 Android Studio 中构建速度的底层构建优化。由于缓存了分解的 AAR 和 pre-dexed 外部内容库，因此缓存的新构建可加快干净构建的速度。在 Android Studio 2.3 中，这个用户范围构建缓存现在默认情况下处于启用状态。

**[了解详情](developer.android.google.cn/studio/build/build-cache.html)**

## 设计

### 1. 约束布局中的链接和比例支持

Android Studio 2.3 加入了稳定版 ConstraintLayout 在此版本的 ConstraintLayout,  中，您现在可以将两个或更多个 Android 视图双向链接起来，在一个维度上组成一组。如果您想让两个视图紧邻，但又想将它们散布在空白区域上，此功能就很有帮助。

[了解详情](developer.android.google.cn/training/constraint-layout/index.html#constrain-chain)

![img](http://mmbiz.qpic.cn/mmbiz_gif/rFWVXwibLGtym7MN6ZrPapaQNneCcToStasKNrBbApeVqO1Amvf7jMmKib1FSMmEl1LzePGicbS7hwleddslhfklQ/0?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

约束布局链接

ConstraintLayout 还支持比例，如果您想在包含布局展开和收缩时保持小部件的纵横比，比例会很有帮助。详细了解有关比例的信息。此外，ConstraintLayout 中的链接和比例还能支持通过 ConstraintSet API 进行编程创建。

![img](http://mmbiz.qpic.cn/mmbiz_gif/rFWVXwibLGtym7MN6ZrPapaQNneCcToStJlLdS80k1X6HGAdBe1oeu1NNicwIZ6zDGIZfjWZ6icamta57tDt9lqvg/0?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

约束布局比例

### 2. 布局编辑器选项板

![img](http://mmbiz.qpic.cn/mmbiz_png/rFWVXwibLGtym7MN6ZrPapaQNneCcToSt3BSvcOpy7WVZFBkBmj8tJI2se3YLDGsjxMHNpsvXThjBDO2uq6AdFA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

布局编辑器小部件选项板  

布局编辑器中的小部件选项板进行了更新，让您可以通过搜索、排序和过滤找到布局所需的小部件，还能让您先预览小部件，然后再拖动到设计界面上。

[了解详情](developer.android.google.cn/studio/write/layout-editor.html)

### 3. 布局收藏夹

![img](http://mmbiz.qpic.cn/mmbiz_gif/rFWVXwibLGtym7MN6ZrPapaQNneCcToStp3Yan8YDibWgJMD1NbOSINEBI2HNrySTwPhW5H20O70ib9oZG3GS2hYg/0?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

布局编辑器 Properties 面板上的 Favorites Attributes    

布局编辑器的 Properties 面板进行了更新，您现在可以小部件为单位保存自己最爱用的属性。只需在高级面板中给属性加注星标，属性即会出现在 Favorites 部分中。

[了解详情](developer.android.google.cn/studio/write/layout-editor.html#edit-properties)

### 4. WebP 支持

![img](http://mmbiz.qpic.cn/mmbiz_png/rFWVXwibLGtym7MN6ZrPapaQNneCcToStGNpr6xmardzucODa64cEDQddEic5X56SkGhNzCYCS1c7Cvv50ylu7Ew/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

WebP 图像转换向导

为帮助您在 APK 中节省空间，Android Studio 现在可以利用项目中的 PNG 资源生成 WebP 图像。WebP 无损格式的体积最多可比 PNG 小 25%。

Android Studio 2.3 新增了一个向导，可通过它将 PNG 转换成无损 WebP，还能用来检查有损 WebP 的编码。右键点击任何非启动器 PNG 文件便可将其转换为 WebP 格式。并且如果您需要编辑图像，还可以右键点击项目中的任何 WebP 文件，将其转换回 PNG 格式。

[了解详情](developer.android.google.cn/studio/write/convert-webp.html)

### 5. 材料图标向导更新

![img](http://mmbiz.qpic.cn/mmbiz_png/rFWVXwibLGtym7MN6ZrPapaQNneCcToStlTqGobHH5oll1HnzibvMePlLUyUf0yVOCxUg1GDiajqwYlvwQv2g5QQw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

矢量资源向导    

矢量资源向导进行了更新，支持搜索和过滤，此外还为每个图标资源提供了标签。

[了解详情](developer.android.google.cn/studio/write/vector-asset-studio.html#materialicon)

## 开发

### 1. Lint 基线

![img](http://mmbiz.qpic.cn/mmbiz_png/rFWVXwibLGtym7MN6ZrPapaQNneCcToStrbaRt9mvK6MqQPD3GY8iak9Ygl6CxNjFr4BM8561hnRicyicGgMZHOj7A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

Lint 基线支持

在 Android Studio 2.3 中，您可以将未解决的 Lint 警告设置为项目中的基线。从那一刻开始，Lint 将只报告新问题。如果应用存在许多旧的 Lint 问题，但您只想集中精力解决新问题，此功能会很有帮助。

[了解详情](developer.android.google.cn/studio/write/lint.html#snapshot)

### 2. 应用链接助手

![img](http://mmbiz.qpic.cn/mmbiz_png/rFWVXwibLGtym7MN6ZrPapaQNneCcToStW7TTZEyrcfLjiaMiapvWEia4J7UbfIxia0SIsNaUcqMWxDI0YbtNJc0U0g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

应用链接助手    

Android Studio 现在进一步简化了应用内 Android 应用链接支持。您可以通过新增的应用链接助手轻松创建新的网址 Intent 过滤器，通过数字资源链接文件声明应用的网站关联，以及进行 Android 应用链接支持测试。要访问应用链接助手，请转到以下菜单位置：Tools → App Link Assistant。

[了解详情](developer.android.google.cn/studio/write/app-link-indexing.html)

### 3. 模板更新

![img](http://mmbiz.qpic.cn/mmbiz_png/rFWVXwibLGtym7MN6ZrPapaQNneCcToStiaic8dta7Pg0TPZxib1icgnvPOyw8rZ6nKtibrBYZWrhh2VAO38a3rBuugA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

新增的项目向导模板    

默认情况下，Android Studio 2.3 中所有过去包含 RelativeLayout 的模板现在均使用 ConstraintLayout。了解有关模板和约束布局的更多信息。我们还新增了一个 Bottom Navigation Activity 模板，它实现的是底部导航 Material Design 规范。

### 4. IntelliJ 平台更新

Android Studio 2.3 加入了 IntelliJ 2016.2 版，其中包含更新版检查窗口和通知系统等增强功能。

[了解详情](www.jetbrains.com/idea/whatsnew/#v2016-2)

## 测试

### 1. Android Emulator 复制与粘贴

![img](http://mmbiz.qpic.cn/mmbiz_gif/rFWVXwibLGtym7MN6ZrPapaQNneCcToStxkcwDnHx4SSjeaicubn2tjCH2I43XjKnUAicp9iabU9agNdwkMsJ6JbXg/0?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

Android Emulator 中的复制与粘贴支持   

应普遍要求，我们在最新版 Emulator (v25.3.1) 中恢复了复制与粘贴功能。我们在 Android Emulator 与主机操作系统之间建立了一个共享剪贴板，以便您在两个环境之间复制文本。复制与粘贴兼容 x86 Google API Emulator 系统映像 API 级别 19 (Android 4.4 - Kitkat) 及更高版本。

### 2. Android Emulator 命令行工具

从 Android SDK Tools 25.3 开始，我们将 emulator 从 SDK Tools 文件夹移至一个单独的 emulator 目录，还弃用了“android avd”命令，并将其替换为独立的 avdmanager 命令。emulator和“android avd”之前的命令行参数仍兼容更新后的工具。我们还为 emulator 命令添加了位置重定向。

不过，如果您直接通过命令行创建 Android Virtual Device (AVD)，则应更新所有相应脚本。如果您通过 Android Studio 2.3 使用 Android Emulator，这些变动将不会影响您的工作流。

[了解详情](developer.android.google.cn/studio/releases/sdk-tools.html)

## 新功能

扼要重述一下，Android Studio 2.3 包含下列以及其他新功能：

### 开发

- Lint 基线
- 更新版 Lint 检查与注解
- 应用链接助手
- 模板中默认情况下使用约束布局
- Intellij 平台更新 2016.2

### 构建

- Instant Run UI 变化
- 构建缓存

### 设计

- 约束布局链接与比例
- 布局编辑器中的小部件选项板
- 属性检查器中的收藏夹
- WebP 支持
- 查找材料图标向导

###  测试

- Emulator 复制与粘贴
- Emulator 命令行工具

[有关 Android Studio 2.3 的详情，请参阅版本说明](developer.android.google.cn/studio/index.html)

## 入门指南

### 下载

如果您使用的是早期版本的 Android Studio，可以在导航菜单中检查有无稳定版更新（Help → Check for Update [适用于 Windows/Linux]，或者 Android Studio → Check for Updates [适用于 OS X]）。

[您还可以从官方下载页面下载 Android Studio 2.3](developer.android.google.cn/studio/index.html)

要充分利用 Android Studio 中所有新增的功能和改进，还应将您当前应用项目中的 Android Gradle 插件版本更新到 2.3.0