title: 《阿里巴巴Java开发手册》4-3之服务器规约
date: 2017-03-13 19:54:42
categories: [Java]
tags: [阿里巴巴Java开发手册, 服务器规约]
---

工程规约 - 服务器规约

1. 【推荐】高并发服务器建议调小 TCP 协议的 time _ wait 超时时间。
   说明：操作系统默认 240 秒后，才会关闭处于 time _ wait 状态的连接，在高并发访问下，服
   务器端会因为处于 time _ wait 的连接数太多，可能无法建立新的连接，所以需要在服务器上
   调小此等待值。
   正例：在 linux 服务器上请通过变更/ etc / sysctl . conf 文件去修改该缺省值 （ 秒 ） ：

   ```shell
   net . ipv 4. tcp _ fin _ timeout = 30
   ```

2. 【推荐】调大服务器所支持的最大文件句柄数 （File Descriptor ，简写为 fd） 。
   说明：主流操作系统的设计是将 TCP / UDP 连接采用与文件一样的方式去管理，即一个连接对
   应于一个 fd 。主流的 linux 服务器默认所支持最大 fd 数量为 1024，当并发连接数很大时很
   容易因为 fd 不足而出现“ open too many files ”错误，导致新的连接无法建立。 建议将 linux
   服务器所支持的最大句柄数调高数倍 （ 与服务器的内存数量相关 ） 。

3. 【推荐】给 JVM 设置- XX :+ HeapDumpOnOutOfMemoryError 参数，让 JVM 碰到 OOM 场景时输出
   dump 信息。
   说明： OOM 的发生是有概率的，甚至有规律地相隔数月才出现一例，出现时的现场信息对查错
   非常有价值。

4. 【参考】服务器内部重定向使用 forward； 外部重定向地址使用 URL 拼装工具类来生成，否则
   会带来 URL 维护不一致的问题和潜在的安全风险。

**以上内容均整理自《阿里巴巴Java开发手册》**

## 下载

> 提供Gitbook在线阅读和pdf下载：[查看福利](https://www.gitbook.com/book/goghtsui/-java/details)