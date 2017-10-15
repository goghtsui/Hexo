title: 《阿里巴巴Java开发规约》插件p3c使用说明
date: 2017-10-15 10:35:42
categories: [Android Studio]

tags: [阿里巴巴Java开发规约, p3c]
---

## 

![img](https://mmbiz.qpic.cn/mmbiz_png/Z6bicxIx5naLibqDOwrwSxhNPBaGVAwtsxFCsP3mtGSNnbJtzw6kiamibXoARJfxDnos4jaUvzTTSDjwXhR5jia0klQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

## 官方发布说明

经过247天的持续研发，阿里巴巴于10月14日在杭州云栖大会上，正式发布众所期待的《阿里巴巴Java开发规约》扫描插件！

**插件全球首发仪式，大牛云集**

![img](https://mmbiz.qpic.cn/mmbiz_jpg/Z6bicxIx5naLibqDOwrwSxhNPBaGVAwtsx27oI1Tz93kzspoAY3murJrqCZd85eTKV45AhY3ZKfUbA1enk8PkWtw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)

阿里巴巴大牛天团倾力助阵：毕玄、玄难、索尼、叶渡，淘宝代码第一人多隆、代码规约作者孤尽携手规约项目组成员，以及业界规约生态代表等重磅大咖联合发布阿里巴巴代码规约插件！

![img](https://mmbiz.qpic.cn/mmbiz_png/H5HV2IYhp7zwn61iaHSicmFqmE8FS0z0pdDqTX7URKKUgv0n9hqO98NfcQLMJ2Lz0K6vJsw1sBVhNug41Vv3yhfw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

平日低调的大神们，为了这次盛会都来了～

该插件由阿里巴巴P3C项目组研发。P3C是世界知名的反潜机，专门对付水下潜水艇，寓意是扫描出所有潜在的代码隐患。这个项目组是阿里巴巴开发爱好者自发组织形成的虚拟项目组，把《阿里巴巴Java开发规约》强制条目转化成自动化插件，并实现部分的自动编程。该插件在扫描代码后，将不符合规约的代码按Blocker/Critical/Major三个等级显示在下方，甚至在IDEA上，还基于Inspection机制提供了实时检测功能，编写代码的同时也能快速发现问题所在。对于历史代码，部分规则实现了批量一键修复的功能，如此爽心悦目的功能是不是很值得拥有？提升代码质量，提高团队研发效能，插件将会一路同行

## 插件下载地址

> https://github.com/alibaba/p3c 

 或者在Github直接搜索p3c

## 插件安装

### Eclipse

1. 准备

   - Eclipse Juno+
   - maven3.+
   - JDK 1.7+

2. 构建

   ``` she
   mvn -U clean install
   ```

3. 安装

   1. **Help** >> **Install New Software** 然后输入这个地址： <https://p3c.alibaba.com/plugin/eclipse/update>

   [![Install Plugin](https://github.com/alibaba/p3c/raw/master/eclipse-plugin/doc/images/install.png)](https://github.com/alibaba/p3c/blob/master/eclipse-plugin/doc/images/install.png)

   1. 点击next 一步一步完成安装之后，重启就可以正常使用了

4. 使用

   1. 语言切换

      [![Switch language](https://github.com/alibaba/p3c/raw/master/eclipse-plugin/doc/images/eclipse_switch_language.png)](https://github.com/alibaba/p3c/blob/master/eclipse-plugin/doc/images/eclipse_switch_language.png)

   2. 代码解析

   [![Analyze](https://github.com/alibaba/p3c/raw/master/eclipse-plugin/doc/images/eclipse_analyze.png)](https://github.com/alibaba/p3c/blob/master/eclipse-plugin/doc/images/eclipse_analyze.png)

   [![Analyze](https://github.com/alibaba/p3c/raw/master/eclipse-plugin/doc/images/analyze_result.png)](https://github.com/alibaba/p3c/blob/master/eclipse-plugin/doc/images/analyze_result.png)

   ​

### Intellij IDEA

1. 准备

   - 项目 JDK 1.7+
   - Gradle: 3.0+ （需要 JDK 1.8+）

2. 构建

   ``` shell
   cd p3c-idea
   gradle clen buildPlugin
   ```

3. 使用

   ​

4. 安装

   1. **Settings** >> **Plugins** >> **Browse repositories...** ，然后搜索 'alibaba' ，默认第一个就是，点击右侧的 **Install**，重启即可使用

      ![p3c_install](http://7xod2d.com1.z0.glb.clouddn.com//p3c/p3c_install.png)

5. 注意

   如果安装之后出现乱码问题，可以尝试更换IDE字体为：**Monospaced**

   ![p3c_font](http://7xod2d.com1.z0.glb.clouddn.com//p3c/p3c_font.png)

6. 使用

   - 语言切换

     在 **Tools** 菜单里，如图：

     ![p3c_switch_launguage](http://7xod2d.com1.z0.glb.clouddn.com//p3c/p3c_tools.png)

   - 设置检测项

     在 **Settings** >> **Inspections** >> **Ali Check** 你可以开关检测的约束规范

     ![p3c_analysis](http://7xod2d.com1.z0.glb.clouddn.com//p3c/p3c_inspection.png)

   - 代码分析

     通过上面的图片，可以看到插件提供的代码解析快捷键是：**Ctrl+Alt+Shift+J** ，使用快捷键之后会出现如下图的选择界面：

     ![p3c_analysis_guide](http://7xod2d.com1.z0.glb.clouddn.com//p3c/p3c_scan_tips.png)

     可以选择整个 Project 或者选中的 module ，代码分析完成之后可以看到类似如下的结果：

     ![p3c_inspections_result](http://7xod2d.com1.z0.glb.clouddn.com//p3c/p3c_inspections_result.png)

   - 代码自动提示

     插件还 提供了自动提示的功能，在我们编写代码的过程中，实时提示：

     ![p3c-coding_tip1](http://7xod2d.com1.z0.glb.clouddn.com//p3c/p3c_codeing_tips1.png)

     ![pc_tip2](http://7xod2d.com1.z0.glb.clouddn.com//p3c/p3c_coding_tips2.png)

## 结语

如果你还没有看过或者了解《阿里巴巴Java开发规约》你可以从这里开始：

[阿里巴巴Java开发手册》之终极版](http://xiaofeng.site/2017/02/10/%E3%80%8A%E9%98%BF%E9%87%8C%E5%B7%B4%E5%B7%B4Java%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C%E3%80%8B%E4%B9%8B%E7%BB%88%E6%9E%81%E7%89%88%E4%BF%AE%E8%AE%A2%EF%BC%81/undefined/#more)

