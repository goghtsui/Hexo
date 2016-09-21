title: Android刷机-命令篇
date: 2016-05-21 16:20:30
categories: [Android] 
tags: [fastboot, adb, bootloader]
---

## 序言
命令行刷机和线刷本质差不多，一个是工具一个是手动的。
线刷包解压出来一般都是一些镜像文件（.img），像基带、内核、系统、recovery、boot等，要先关机，进入线刷（bootloader）模式下。如果电脑上有adb环境（没有就下载adb工具），直接执行
``` bash
adb reboot bootloader
```
进入到线刷模式，下面就给出一些常用命令行（MOTO 为例）

## 命令行

1、刷入手机闪存分区表（请不要乱刷其他机型的，可能导致变砖，请在有教程指引下操作）
``` bash
fastboot flash partition gpt.bin
```

2、刷入摩托罗拉bootleader（请一定不要跨机型刷，或者降版本刷，否则分分钟变砖）
``` bash
fastboot flash motoboot motoboot.img
```
<!-- more -->
3、刷入基带
``` bash
fastboot flash modem NON-HLOS.bin
```

4、刷入efs射频表
``` bash
fastboot flash fsg fsg.mbn
```

5、清理基带缓存
``` bash
fastboot erase modemst1
```

6、清理efs射频表
``` bash
fastboot erase modemst2
```

7、刷入缓存
``` bash
fastboot flash cache cache.img
```

8、输入用户数据
``` bash
fastboot flash userdata userdata.img
```

9、重新进入bootleader模式
``` bash
fastboot reboot-bootloader
```

10、刷入内核部分
``` bash
fastboot flash boot boot.img
```

11、刷入系统恢复模式模块
``` bash
fastboot flash recovery recovery.img
```

12、刷入系统部分（有可能system.img被分割为很多个文件 system.*****.01啥的，逐个替换内容中的system.img，按照数字顺序执行即可）
``` bash
fastboot flash system system.img
```
或
``` bash
fastboot flash system system.01
fastboot flash system system.02
fastboot flash system system.03
```

13、这句一般是用在解锁后跨版本升级，清理fastboot模式缓存，作用也是让新的分区表生效，从而可以加载非本区域的原版系统，比如用在国行刷亚太底包上，就可能会用到这一句
``` bash
fastboot oem fb_mode_clear
```

14、还有一些基带相关的：

``` bash
fastboot flash sbl1 sbl1.mbn
```

``` bash
fastboot flash dbi sdi.mbn

```
``` bash
fastboot flash aboot emmc_appsboot.mbn
```

``` bash
fastboot flash tz tz.mbn
```

``` bash
fastboot flash LOGO logo.bin
```

``` bash
fastboot flash misc misc.img
```

``` bash
fastboot flash oppostanvbk static_nvbk.bin
```

15、类似双清操作，一般刷机完成之后执行一下这句话，有的不执行可能卡在开机画面
``` bash
fastboot -w
```

## 总结
没什么技术含量，什么文件使用什么命令行，不过一般的线刷包不会这么多文件，命令行刷机还是需要有一定刷机经验的，还是那句话：**刷机有风险，操作需谨慎**