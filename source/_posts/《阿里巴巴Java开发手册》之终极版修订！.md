//title: 《阿里巴巴Java开发手册》之终极版修订（20170925）！
date: 2017-02-10 14:48:31
categories: [Java]
tags: [阿里巴巴Java开发手册（终极版）]
---

> 已经同步最终版

## 序言

首先，这肯定是一个非常重大的消息。绝对是Java程序员的福利啊，终于结束了一个公司一套规范的编程生涯。这对业界规范来说也起到了很好的推动作用。俗话说：无规矩不方圆，生活中各种法律道德的约束，出门还有交规的限制。相信小伙伴们一定经历过 坑，有了这本规范手册，你是不是该好好学习一下呢？

## 目录章节

内容分五大类，总共19章节：

| 索引   | 一级目录     | 二级目录                                     |
| ---- | -------- | ---------------------------------------- |
| 一    | 编程规约     | [命名规约](http://xiaofeng.site/2017/02/10/%E3%80%8A%E9%98%BF%E9%87%8C%E5%B7%B4%E5%B7%B4Java%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C%E3%80%8B%E4%B9%8B%E5%91%BD%E5%90%8D%E8%A7%84%E8%8C%83/undefined/)、常量定义、代码格式、OOP规约、集合处理、并发处理、控制语句、注释规约、其它 |
| 二    | 异常日志     | 异常处理、日志规约                                |
| 三    | 单元测试     | 无                                        |
| 四    | 安全规约     | 无                                        |
| 五    | MySQL数据库 | 建表规约、索引规约、SQL语句、ORM映射                    |
| 六    | 工程结构     | 应用分层、二方库依赖、服务器                           |



<!-- more -->

## 手册专有名词（新增）

1.  POJO（ Plain Ordinary Java Object ）: 在本手册中，POJO 专指只有 setter / getter

  / toString 的简单类，包括 DO/DTO/BO/VO 等。

2.  GAV（ GroupId、ArtifactctId、Version ）: Maven 坐标，是用来唯一标识 jar 包。

3.  OOP（ Object Oriented Programming ）: 本手册泛指类、对象的编程处理方式。

4.  ORM（ Object Relation Mapping ）: 对象关系映射，对象领域模型与底层数据之间的转换，
  本文泛指 iBATIS, mybatis 等框架。

5.  NPE（ java.lang.NullPointerException ）: 空指针异常。

6.  SOA（ Service-Oriented Architecture ）: 面向服务架构，它可以根据需求通过网络对松散
  耦合的粗粒度应用组件进行分布式部署、组合和使用，有利于提升组件可重用性，可维护性。

7.  一方库: 本工程内部子项目模块依赖的库（jar 包）。

8.  二方库: 公司内部发布到中央仓库，可供公司内部其它应用依赖的库（jar 包）。

9.  三方库: 公司之外的开源库（jar 包）。

10.  IDE（ Integrated Development Environment ）: 用于提供程序开发环境的应用程序，一
  般包括代码编辑器、编译器、调试器和图形用户界面等工具，本《手册》泛指 IntelliJ IDEA
  和 eclipse。

  ​

## 总结

阿里Java技术团队一手打造出Dubbo、JStorm、Fastjson等诸多流行开源框架，部分已成为Apache基金会孵化项目；

此次首度公开的Java开发手册正是出自这样的团队，近万名阿里Java技术精英的经验总结，并经历了多次大规模一线实战检验及完善，铸就了这本高含金量的阿里Java开发手册。该手册以Java开发者为中心视角，划分为编程规约、异常日志规约、MYSQL规约、工程规约、安全规约五大块，再根据内容特征，细分成若干二级子目录。根据约束力强弱和故障敏感性，规约依次分为强制、推荐、参考三大类。此套规范不仅能让代码一目了然， 更有助于加强团队分工与合作、真正提升效率。 

## 下载

> 提供Gitbook在线阅读和pdf下载：[查看福利](https://www.gitbook.com/book/goghtsui/-java/details)



