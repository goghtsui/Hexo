title: 关于OpenJDK你了解多少？
date: 2016-08-10 21:55:30
categories: [Java]
tags: [openjdk]
---

## 序言

2015年12月底，谷歌宣布，他们正在用开源的 OpenJDK 替换 Oracle JavaAPI。这次方向上的改变看起来与 Sun/Oracle 与 谷歌之间的法律纠纷有关，该纠纷认为谷歌在使用 Java 开发安卓操作系统时违反了 Sun/Oracle 的版权和专利权。

本文与 Sun/Oracle 和谷歌的法律纠纷无关。谷歌现在加入了 IBM、RedHat、Apple（还有其他企业）的队伍专注于 OpenJDK，这意味着什么？意识到这点之后我想弄清楚，对于 JDK 用户，现在是不是应该考虑一下 OpenJDK

## 历史

从Java7开始，OpenJDK就是Java的参考实现（Reference Implementation）。下图的时间线可以让你了解一下OpenJDK的历史:

![jdkhistory](http://incdn1.b0.upaiyun.com/2015/01/bc70d51dc8ffa360f32185cfe67dbd03.png)

<!--more-->
OpenJDK由许多软件库组成，主要有corba,hotspot,jaxp,jaxws,jdk,langtools,以及nashorn。在OpenJDK8和OpenJDK9之间没有新的软件库加入，但有很多改变和结构调整，主要是因为Jigsaw---Java自身的模块化

![jdkmember](http://incdn1.b0.upaiyun.com/2015/01/34ac4545b3f17bd20b571092d2845cc1.jpg)

Java通过引导一个旧版本的Java——例如，Java以其自身为构件建立。旧的组件被组合在一起创建一个新的组件，即成为下一阶段的结构单元。

OpenJDK8使用JDK7编译和构建，类似地，OpenJDK9 则使用JDK8编译构建。理论上，OpenJDK8是可以使用从其自身创建的影像编译的，同理，OpenJDK9也能用OpenJDK9编译。使用一个叫做循环启动影像的进程——创建OpenJDK的JDK影像，使用同样的影像，OpenJDK再一次被编译。也可以用make命令实现OpenJDK的编译：

``` bash
$ make bootcycle-images # Build images twice, second time with newly built JDK
```
make命令在OpenJDK8和OpenJDK9下都提供了很多设置选项，可以通过命名的方式建立独立的组件或模块。如下：

``` bash
$ make [component-name] | [module-name]
```

甚至并行运行多个构建过程，如下：

``` bash
$ make JOBS= # Run parallel make jobs
```

最后，用install选项安装上述已构建的组件，如下：

``` bash
$ make install
```

## 特性

- 性能与可伸缩性

就我能够看到的性能测试而言，闭源的 Oracle JDK 和 OpenJDK 之间在性能上似乎并没有很大的差别。然而，至少后来我看到的一则明确的消息说，开源版本的性能已经与 Oracle 的产品并驾齐驱了，这或许是一个理由，让我们至少对开源版本的用法做一下评估。

- 社区进展

随着开源开发者持续对源代码进行改进，OpenJDK 很有可能已经超过 Oracle 发布的版本。另外，开源世界为各种思想和概念提供了实现的可能，这通常在闭源的企业环境中是不可能的

关于开源解决方案如何成为主角的一个例子是 PostgreSQL 数据库。随着 9.5.0 版于 2016 年 1 月初的发布，致力于该产品的贡献者已经使该产品获得了巨大的成功。PostgreSQL 的用户包括：雅虎、Sony在线、BASF、Reddit、Instagram以及 TripAdvisor（只是随便举几个例子）

- 包管理

OpenJDK 也具有了通过类似 brew 这样的包管理器下载/更新 JDK 的能力。JDK 的自动更新能力，对某些人来说可能不算什么，但对于大型的 JDK 实现而言，其作用是巨大的

- 许可证问题

假如你处在类似谷歌的位置，使用 Oracle 的 JDK 有可能会导致违反版权/专利权，那么迁移到 OpenJDK 就是一个应该考虑的选项。从我的非专业、非律师的角度来看，我并不十分肯定的是，仅仅通过采用 OpenJDK是否就能让谷歌完全摆脱困境

- 跟从趋势

如果你本来就是一个开源软件的粉丝，那么 OpenJDK 的目前版本已经稳定，而且性能上接近（如果不是相等的话）Oracle 的产品。再说，跟从由谷歌、IBM、RedHat、Apple 共同设定的趋势，可能是一注安全的赌注，这应该有助于说服你看一下开源 JDK


