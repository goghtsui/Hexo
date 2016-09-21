title: Android Studio Tips of the Day(2)
date: 2015-12-23 14:22:27
categories: [Android Studio]
tags: [Android Studio, 快捷键, tips of the Day]
---

>原作者：Philippe Breault
>原文地址：[http://www.developerphil.com/...the-day-roundup-2/](http://www.developerphil.com/android-studio-tips-of-the-day-roundup-2/)

#### 关于快捷键

Android Studio 提供了不同的按键对应关系(在快捷键和动作之间的映射).你能看到你正在使用的案件映射，通过_Settings->KeyMap._

#### 1.重复的行

| Mac | Win&Linux |
| :-- | :-- |
| cmd+d | ctrl+d |

它可以复制当前行并且粘贴它到下一行,不会影响剪切板的内容。

![dumplicate](http://www.developerphil.com/assets/11-duplicate_lines.gif)
<!-- more -->


#### 2.扩大/缩小选择

| Mac |    Win&Linux |
| :-- | :-- |
| alt+up/down | ctrl+w / ctrl+shift+w |

以光标为基点,在上下文扩展选择的范围。例如:它将选择当前的变量,然后该语句,然后是这个方法等。
![dumplicate](http://www.developerphil.com/assets/12-expand_shrink_selection.gif)

#### 3.环绕(包装)
| Mac |    Win&Linux |
| :-- | :-- |
| cmd+alt+t | ctrl+alt+t |

这个操作可以包装一个结构的代码块。通常是一个 _if_ 语句,一个循环、一个 _try-catch_ 或者是一个 _runnable_。
如果你什么都没选, 那么它只会包裹当前行。

![dumplicate](http://www.developerphil.com/assets/13-surround_with.gif)

#### 4.最近列表

| Mac |    Win&Linux |
| :-- | :-- |
| cmd+e | ctrl+e |

使用这个功能,你可以看到最近查看过的文件列表。

![dumplicate](http://www.developerphil.com/assets/14-recents.gif)

#### 5.自动代码

| Mac |    Win&Linux |
| :-- | :-- |
| cmd+j | ctrl+j |

它可以快速插入代码片段。更有趣的是它带有合理的默认值,并通过参数引导你完成插入。

其他提示:
- 如果你知道缩写,你也可以不用快捷方式。你仅仅需要如果缩写并且使用_Tab_key 完成即可。

![dumplicate](http://www.developerphil.com/assets/15-live_templates.gif)


#### 6.移动方法

| Mac |    Win&Linux |
| :-- | :-- |
| cmd+alt+up/down | ctrl+shift+up/down |

这是一个类似移动行的快捷方式,但是移动的是整个方法。没必要使用复制-粘贴,就可以上下移动方法。例如:你可以重新排序字段和内部类。

![dumplicate](http://www.developerphil.com/assets/15-movemethods.gif)


#### 7.完成语句

| Mac |    Win&Linux |
| :-- | :-- |
| cmd+shift+enter | ctrl+shift+enter |

它会自动生成丢失的代码来完成一条语句,它通常的使用情景是:
- 添加一个分号在行的末尾,即时光标不在行尾
- 在_if、while、for_的后面添加一个括号或者大括号
- 添加一个大括号在方法声明之后

其他提示:
- 如果一条语句已经完成,它会进入下一行,即时光标没有在当前行的最后一个字符.
![dumplicate](http://www.developerphil.com/assets/16-completestatement.gif)


#### 8.最后一次编辑的位置

| Mac |    Win&Linux |
| :-- | :-- |
| cmd+shift+backspace | ctrl+shift+backspace |

它会让你浏览你最后一次修改的位置,这个和点击工具栏的返回按钮是不一样的。它会带你在你修改的历史记录中浏览。


![dumplicate](http://www.developerphil.com/assets/17-navigate-previous-changes.gif)


#### 9.整合行和文字

| Mac |    Win&Linux |
| :-- | :-- |
| ctrl+shift+j | ctrl+shift+j |


它能比你在行尾模拟删除键要做的更多,它可以保留当前的格式规则,并且它还可以:
- 合并两个注释行,并且删除无用的_//_
- 合并多行字符串,并且去除_+、""_
- 整合字段和任务

其他提示:
- 如果你选择了一个字符串,跨越多行,那么它就可以将其整合成一行

![dumplicate](http://www.developerphil.com/assets/18-joinlines.gif)


#### 10.查找

| Mac | Win&Linux |
| :-- | :-- |
| alt+f1 | alt+f1 |


获取当前文件,并且询问你在哪选择它。他可以在_project、structure_或者文件管理器中打开它。每一个动作都有一个数字或字母前缀,这是调用它的快捷方式。

你可以从文件或者直接从_project_试图调用测方法。

![dumplicate](http://www.developerphil.com/assets/19-select-in.gif)


#### 11.展开/删除
这个操作将会移除包裹的代码,它可以移除一个_if_语句、_while_ 循环、_try-catch_ 或者_runnable_ 。
这和包裹的快捷方式是完全相反的。

![dumplicate](http://www.developerphil.com/assets/20-unwrap.gif)