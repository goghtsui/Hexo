title: 《阿里巴巴Java开发手册（终结版）》5-3之SQL语句
date: 2017-03-03 15:17:46
categories: [Java]
tags: [阿里巴巴Java开发手册（终结版）, SQL语句]
---

## SQL  规约

> 修改项：第 4 项

1. 【强制】不要使用 count( 列名 ) 或 count( 常量 ) 来替代 count( * ) ， count( * ) 就是 SQL 92 定义
   的标准统计行数的语法，跟数据库无关，跟 NULL 和非 NULL 无关。
   说明： count( * ) 会统计值为 NULL 的行，而 count( 列名 ) 不会统计此列为 NULL 值的行。
2. 【强制】 count(distinct col) 计算该列除 NULL 之外的不重复数量。注意  count(distinct
   col 1,  col 2 ) 如果其中一列全为 NULL ，那么即使另一列有不同的值，也返回为 0。
3. 【强制】当某一列的值全是 NULL 时， count(col) 的返回结果为 0，但 sum(col) 的返回结果为
   NULL ，因此使用 sum() 时需注意 NPE 问题。
   正例：可以使用如下方式来避免 sum 的 NPE 问题： SELECT IF(ISNULL(SUM(g)) ,0, SUM(g))
   FROM table;

<!-- more -->

4. 【强制】使用 ISNULL() 来判断是否为 NULL 值。
   说明： NULL 与任何值的直接比较都为 NULL。
   1 ） NULL<>NULL 的返回结果是 NULL ，而不是 false 。
   2 ） NULL=NULL 的返回结果是 NULL ，而不是 true 。
   3 ） NULL<>1 的返回结果是 NULL ，而不是 true 
5. 【强制】 在代码中写分页查询逻辑时，若 count 为 0 应直接返回，避免执行后面的分页语句。
6. 【强制】不得使用外键与级联，一切外键概念必须在应用层解决。
   说明： （ 概念解释 ） 学生表中的 student _ id 是主键，那么成绩表中的 student _ id 则为外键。
   如果更新学生表中的 student _ id ，同时触发成绩表中的 student _ id 更新，则为级联更新。
   外键与级联更新适用于单机低并发，不适合分布式、高并发集群 ； 级联更新是强阻塞，存在数
   据库更新风暴的风险 ； 外键影响数据库的插入速度。
7. 【强制】禁止使用存储过程，存储过程难以调试和扩展，更没有移植性。
8. 【强制】数据订正时，删除和修改记录时，要先 select ，避免出现误删除，确认无误才能执
   行更新语句。
9. 【推荐】 in 操作能避免则避免，若实在避免不了，需要仔细评估 in 后边的集合元素数量，控
   制在 1000 个之内。
10. 【参考】如果有全球化需要，所有的字符存储与表示，均以 utf -8 编码，那么字符计数方法
  注意：
  说明：
  SELECT LENGTH( "轻松工作" )； 返回为 12
  SELECT CHARACTER _ LENGTH( "轻松工作" )； 返回为 4
  如果要使用表情，那么使用 utfmb 4 来进行存储，注意它与 utf -8 编码的区别。
11. 【参考】  TRUNCATE TABLE 比  DELETE 速度快，且使用的系统和事务日志资源少，但 TRUNCATE
   无事务且不触发 trigger ，有可能造成事故，故不建议在开发代码中使用此语句。
   说明： TRUNCATE TABLE 在功能上与不带  WHERE 子句的  DELETE 语句相同。

**以上内容均整理自《阿里巴巴Java开发手册（终结版）》**

## 下载

> 提供Gitbook在线阅读和pdf下载：[查看福利](https://www.gitbook.com/book/goghtsui/-java/details)