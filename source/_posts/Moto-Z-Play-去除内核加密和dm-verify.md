title: Moto Z Play 去除内核加密和dm-verify
date: 2017-07-05 17:13:35
categories: [mobile]
tags: [Moto Z , Moto Z Play, 内核加密, dm-verify]
---

## 序

由于各种原因，我们需要修改系统已完成定制，所以我们需要想办法去除掉它。

最近迎来了 Moto Z Play 的安全补丁更新，但是由于已经解锁 bootloader，无法正常OTA，所以需要手动刷最新完整底包，或者刷上一个版本然后OTA，更新到最新的 NCN25.137-24 版本，安全补丁是 2017年5月1日 的，无奈最新的完整底包还没有放出，只能刷上一个版本的底包再OTA了。当然了，Google全家桶是必不可少的，但是现在 Moto Z / Moto Z Play 刷 Google 全家桶可没那么容易了，两个问题：内核强制加密、dm-verify 验证！

- 内核强制加密

  Moto Z / Moto Z Play 开启了强制加密 Data(数据目录) 分区功能

  开启强制加密会影响磁盘读写性能、无法修改、不能 ROOT、开机速度慢

- dm-verify 验证

  这是由 Google 设计的一项用于保护系统的技术。当系统经过修改后，手机将会重启，并且将会无法开机进入系统

关于内核加密，现在第三方的ROM制作团队或者厂商都默认开启了内核加密，这没有任何问题，但是对于喜欢DIY或者定制ROM的人，就相当于是一道墙。 但是，还是有大神来解决的，下面是我整理的一些大神提供的去除内核加密和dm-verify验证的方法（**只针对Moto Z / Moto Z Play**）

<!-- more -->

## 教程

### 准备

1. 需要使用的工具：**Android Image Kitchen **
2. 官方固件（相应版本底包）
3. 需要在 Linux 或者 有Java环境下
4. 高级文本编辑器：Notepad++ 等

### 步骤

1. 下载工具解压缩

   附件是：Android.Image.Kitchen.v2.4-Win32.zip

2. 解包内核

   内核文件一般在底包中都命名为：boot.img ，镜像文件

   打开cmd窗口，输入：

   ```shell
   unpackimg <image-filename.img>
   ```

   或者可以拖放 img 到 unpackimg.bat，这个脚本会解包 img 并解压到 ramdisk 的一个子目录中。 

3. 去除 dm-verify 验证

   打开 ramdisk 文件夹，找到 _fstab.qcom_ 文件，并用Notepad++等编辑器打开

   找到第9行，将 wait 后面的 _,verify_ 删掉，即将一下内容：

   ``` xml
   /dev/block/bootdevice/by-name/system    /system      ext4    ro,barrier=1         wait,verify
   ```

   修改为：

   ``` xml
   /dev/block/bootdevice/by-name/system    /system      ext4    ro,barrier=1         wait
   ```

   假如是 Android 6.0.1，还有一个 charger.fstab.qcom，同样在第九行去掉 ,verify ，可加入 noatime 以提交 io 性能。

   ``` xml
   /dev/block/bootdevice/by-name/system    /system           ext4    ro,barrier=1,noatime,discard       wait
   ```

4. 去除去除强制加密

   接着找到第10行，将 _forceencrypt_ 修改为 _encryptable_ ，即将一下内容：

   ```xml
   <blockquote>/dev/block/bootdevice/by-name/userdata       /data        f2fs    rw,discard,nosuid,nodev,noatime,nodiratime,nobarrier,inline_xattr,inline_data    wait,check,formattable,forceencrypt=/dev/block/bootdevice/by-name/metadata
   ```

   修改为：

   ```xml
   <blockquote>/dev/block/bootdevice/by-name/userdata       /data        f2fs    rw,discard,nosuid,nodev,noatime,nodiratime,nobarrier,inline_xattr,inline_data    wait,check,formattable,encryptable=/dev/block/bootdevice/by-name/metadata
   ```

5. 删除 verity_key

   删除 ramdisk 文件夹下 verity_key 文件，不删除 verity_key 将会导致国行系统不读卡

6. 重新打包内核

   直接点击 repackimg.bat（repackimg.bat 这个批处理脚本不需要输入命令，只要点击运行）。可以直接打包成 image-new.img 文件

   关于 cleanup.bat ：清理文件夹并重置为初始状态，消除以下文件与文件夹：split_img + ramdisk的目录和任何新的打包的 ramdisk 或 img 文件

7. 刷入

   手机进入bootloader模式，执行以下命令：

   ``` shell
   fastboot flash boot image_new.img
   ```

8. 格式化 Data 分区

   ``` shell
   fastboot -w
   ```

   注意，只有当格式化 Data 分区后去除加密才可以生效


## 注意

刷入第三方包需要使用第三方recovery ，我使用的是TWRP 3.1.0，所以刷入内核之前要先刷入这个recovery。

## 附件

[Android Image Kitchen 点我下载](https://pan.baidu.com/s/1geVvQPd)

[TWRP 3.1.0 ](https://pan.baidu.com/s/1qXZNsew)