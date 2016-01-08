title: Android Studio Tips (一)
date: 2015-12-22 17:19:51
categories: [Android Studio]
tags: [Android Studio, 快捷键]
---

>原作者：Philippe Breault
>原文地址：[http://www.developerphil.com/...day-roundup-1/](http://www.developerphil.com/android-studio-tips-of-the-day-roundup-1/)

#### 关于快捷键

Android Studio 提供了不同的按键对应关系(在快捷键和动作之间的映射).你能看到你正在使用的案件映射，通过_Settings->KeyMap._


#### 1.高亮显示
| Mac| Win&Linux |
| :-- | :--|
| cmd+shift+f7| ctrl+shift+f7 |

高亮显示光标所在的字符，这不仅仅是一个简单的匹配模式，它会了解当前的范围，并且高亮范围内同类的字符。你可以向上、向下浏览，通过：_Edit -> Find -> Find Next/Previous_

其他提示：
- 高亮一个方法中的“return”或者“throw”，其他方法也会同样高亮显示
- 高亮类声明中“extends”或者“implements”，同样会高亮 **override/implemented**的方法
- 高亮一个 import 会同样高亮使用它的地方
- 可以通过 _Escape_ 取消高亮

![ctrlshiftf7](http://www.developerphil.com/assets/01-highlight.gif)

#### 2.在方法和内部类之间移动

| Mac| Win&Linux |
| :-- | :-- |
| ctrl+up/down| alt+up/down |

在当前文件中，移动光标到下一个方法或者类的名字开头。

如果你在方法内，向上就会将光标移动到该方法的名字开头。它非常有用，因为它让你在正确的地方重构或者发现方法的用法。

![move](http://www.developerphil.com/assets/02-move_between_methods.gif)

#### 3.类结构弹窗

| Mac| Win&Linux |
| :-- | :-- |
| cmd+f12| ctrl+f12 |

用来展示当前类的概要和内部的导航.最好的事情是你可以使用你的键盘过滤。这是一件非常高效的方法，去定位到一个你知道其名字的方法。

其他提示：
- 输入过滤列表时，你可以使用驼峰匹配。例如：输入**"oCr"**将查找到**"onCreate"**
- 你也可以切换复选框来显示匿名内部类。假如你想要查找**onClickListener**中的**onClick**方法，这个就非常好用。

![hierarachy](http://www.developerphil.com/assets/04-callinghierarchy.gif)

#### 4.调用层级结构弹窗
| Mac| Win&Linux |
| :-- | :-- |
| ctrl+alt+h | ctrl+alt+h |
它可以显示一个方法的声明和调用之间可能的路径。
![popup](http://www.developerphil.com/assets/03-filestructure.gif)

#### 5.定义快速查询
| Mac| Win&Linux |
| :-- | :-- |
| alt+space | ctrl+shift+i |
有没有想要查看一个方法或者类的实现，但是又不想离开当前的页面？使用这个快捷键就可以在当前页面通过窗口的形式展现。
![quick](http://www.developerphil.com/assets/05-quickdefinition.gif)

#### 6.折叠展开代码块
| Mac| Win&Linux |
| :-- | :-- |
| alt+plus/minus | ctrl+shift+plus/minus |
这个功能的目的是让你隐藏你此刻不关心的东西。他将以最简单的形式隐藏整个代码块（例如：当你打开一个新的文件的时候忽略 _import_列表）。一个更有趣的用法是，它会隐藏周围简单的匿名内部类模块，并使它看起来像一个lambda表达式。

其他提示：
- 你可以设置默认，通过 _Edit -> Code Folding._
![fold](http://www.developerphil.com/assets/06-codefolding.gif)

#### 7.书签
- 切换书签

| Mac| Win&Linux |
| :-- | :-- |
| f3 | f11 |

- 通过助记符切换书签

| Mac| Win&Linux |
| :-- | :-- |
| alt+f3 | ctrl+f11 |
	
如果你分配了一个数据，你可以通过快捷方式 _ctrl+number_ 回到书签


- 显示书签

| Mac| Win&Linux |
| :-- | :-- |
| cmd+f3 | shift+f11 |
	
![find](http://www.developerphil.com/assets/08-findaction.gif)





#### 8.符号查找
| Mac| Win&Linux |
| :-- | :-- |
| cmd+shift+a | ctrl+shift+a |

对于Android Studio，你可以通过它的名字，调用任何你知道的菜单或者符号！这对于你曾经有一段时间使用过，但却没有快捷方式的命令是非常有用的。

其他提示：
- 如果有相关联的快捷键，会一同显示
![move](http://www.developerphil.com/assets/07-bookmarks.gif)

#### 9.行上下移动
| Mac| Win&Linux |
| :-- | :-- |
| alt+shift+up/down | alt+shift+up/down |

![bookmark](http://www.developerphil.com/assets/09-movelines.gif)

#### 10.删除行

| Mac| Win&Linux |
| :-- | :-- |
| cmd+backspace | ctrl+y |

![bookmark](http://www.developerphil.com/assets/10-deleteline.gif)