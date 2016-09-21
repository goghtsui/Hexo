title: '实用bat脚本[笔记]'
date: 2016-03-01 11:41:36
categories: [PC]
tags: [bat, 批处理文件]
---

### 垃圾清理
---
一个自定义的pc端系统垃圾清理批处理文件，可以配合各大电脑管家使用：

``` bash

@echo off 
color 0a
title ********系统垃圾清理******** 
echo 正在清除系统垃圾文件，请稍后...... 

echo 删除补丁备份目录 
RD %windir%\$hf_mig$ /Q /S 

echo 把补丁卸载文件夹的名字保存成patchs.txt 
dir %windir%\$NtUninstall* /a:d /b >%windir%\patchs.txt 

echo 从patchs.txt中读取文件夹列表并且删除文件夹 
for /f %%i in (%windir%\patchs.txt) do rd %windir%\%%i /s /q 

echo 删除patchs.txt 
del %windir%\patchs.txt /f /q 

echo 删除补丁安装记录内容（下面的del /f /s /q %systemdrive%\*.log已经包含删除此类文件） 
del %windir%\KB*.log /f /q 

echo 删除系统盘目录下临时文件 
del /f /s /q %systemdrive%\*.tmp 

echo 删除系统盘目录下临时文件 
del /f /s /q %systemdrive%\*._mp 

echo 删除系统盘目录下日志文件 
del /f /s /q %systemdrive%\*.log 

echo 删除系统盘目录下GID文件(属于临时文件，具体作用不详) 
del /f /s /q %systemdrive%\*.gid 

echo 删除系统目录下scandisk（磁盘扫描）留下的无用文件 
del /f /s /q %systemdrive%\*.chk 

echo 删除系统目录下old文件 
del /f /s /q %systemdrive%\*.old
 
echo 删除回收站的无用文件 
del /f /s /q %systemdrive%\recycled\*.* 

echo 删除系统目录下备份文件 
del /f /s /q %windir%\*.bak 

echo 删除应用程序临时文件 
del /f /s /q %windir%\prefetch\*.* 

echo 删除系统维护等操作产生的临时文件 
rd /s /q %windir%\temp & md %windir%\temp 

echo 删除当前用户的COOKIE（IE） 
del /f /q %userprofile%\cookies\*.* 

echo 删除internet临时文件 
del /f /s /q "%userprofile%\local settings\temporary internet files\*.*" 
del /f /s /q "%userprofile%\Local Settings\Temporary Internet Files\*.*"

echo 删除当前用户日常操作临时文件 
del /f /s /q "%userprofile%\local settings\temp\*.*" 
del /f /s /q "%userprofile%\Local Settings\Temp\*.*"

echo 删除访问记录（开始菜单中的文档里面的东西） 
del /f /s /q "%userprofile%\recent\*.*" 

echo echo 恭喜您！清理全部完成！
echo. & pause

```
右键 -> 已管理员身份运行 即可，不会存在任何风险，当然你也可以自己添加路径或者相关的处理，可以说是绿色安全
<!-- more -->

### 启动应用
---
``` bash

@echo off

color 0a
title ********快速启动******** 

echo 1、QQ
echo 2、Exit

set /p s=请输入应用的编号，按Enter启动应用：
if %s% equ 1 goto a
if %s% equ 2 goto b

:a
start "" "E:\Program Files (x86)\Tencent\QQ\Bin\QQ.exe"
echo QQ启动完成！
exit

:b
exit

```

这个就非常简单了，一个if判断，指定对应应用的绝对路径，启动指定的应用，还可以打开指定的路径的，因为我比较喜欢简洁的桌面，有了这个脚本，桌面上就不用放置很多快捷方式了

### adb shell
---
这个适用于Win系统下对手机执行一些操作，原因是命令行执行了adb shell之后，无法继续使用shell的相关命令，那么我们可以先将命令输出到一个文件里，在读取出来就可以了,以删除文件为例：

``` bash

adb root

adb remount

echo cd /sdcard/ >> temp.txt

echo rm -r 1.txt >> temp.txt

echo exit >> temp.txt

adb shell < temp.txt


```