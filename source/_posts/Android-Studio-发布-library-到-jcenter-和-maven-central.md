title: Android Studio 发布 library 到 jcenter 和 maven central
date: 2017-08-08 15:44:17
categories: [Android Studio]

tags: [Android Studio, jcenter, maven central]
---

## 序言

在我们日常的开发中，会遇到各种各样的需求和技术解决方案。所以产生了各位大神提供的各种功能的开源库，并且通过：compile 'xxxxxxxx' 就可以使用了，非常方便。可以你有没有想过自己写一个开源库？又或者你已经贡献了很多好的代码，不知道怎么共享，怎么通过 compile 的方式给别人使用？你要知道，装B也是需要技术的。

## jcenter & maven central

### 引用

如果你留心的话，应该了解到我们使用了两种标准的 libraries 仓库，分别是 **jcenter** 和 **maven central** ：

- **jcenter** 

  jcenter 是一个托管在 [bintray.com](https://bintray.com/) 的资源库，你可以在 [这里](http://jcenter.bintray.com/) 找到需要的资源为了能在项目中使用 **jcenter**，我们需要在 project 的 build.gradle 文件中添加对资源库的引用：

  ​

  ```
  allprojects {
      repositories {
          jcenter()
      }
  }
  ```

  ​

- **maven central** 

  Maven Central 是一个托管在 [sonatype.org](https://sonatype.org/) 的资源库，你可以在 [这里](https://oss.sonatype.org/content/repositories/releases/) 找到需要的资源如果在项目中使用 Maven Central，我们需要在 project 的 build.gradle 文件中定义自己的资源库：

  ​

  ```
  allprojects {
      repositories {
          mavenCentral()
      }
  }
  ```

请注意，虽然 jcenter 和 Maven Central 都是标准Android library 资源仓库，但他们的托管地址完全不同，它们的内容是由不同提供者提供的，而且之间并没有任何关联。所以也就可能，在 jcenter 中能够找到的 library ，在 Maven Central 中并不能找到，反之亦然。

<!-- more -->

除了这两个标准的资源库外，我们也可以定义特殊的资源库，引入一些开发者自己托管维护的 library，然后像下面这样添加对该库的引用：

```
repositories {
    maven { url 'http://xxxx.xxxx' }
}
```

最后通过 compile 依赖：

```
dependencies {
	 // 替换对应的配置值
     compile 'groupId:artifactId:version@aar'
}
```

### 对比

那么为什么有两个标准资源库而不是一个？而且它们都拥有相同的功能：托管 java / Android library ，完全由开发者决定把 library 上传到它们中的一个或两个上。最开始的时候，Android Studio 使用 Maven Central 作为默认资源库，一旦你从老版本的 Android Studio 创建了一个新的项目，mavenCentral() 会自动添加到 build.gradle 中

但是 Maven Central 存在一个较大的问题，即对开发者并不友好。上传 library 比较困难。为了能够做到上传，开发者从某种程度上讲得具备极客的能力。再考虑到其他一些原因，比如安全问题，Android Studio 团队决定把默认资源库改为jcenter，所以新版 Android Studio 创建新项目的时候，默认使用 jcenter() 而不是 mavenCentral()这里列举了一些他们决定从 Maven Central 切换 jcenter 的主要原因：

- **jcenter 通过 CDN 传输 library ，这意味着开发者能够享受更快的加载速度**
- **jcenter 是世界上最大的 java 库，所以能在 Maven Central 里面找到的，基本在 jcenter 里面也能找到**
- **非常容易上传 library 到 jcenter 仓库，没有必要签名或做其他一些在 Maven Central 上很复杂的操作**
- **界面友好**

所以，我推荐使用 jcenter 仓库来上传自己的库，而且可以同步到 maven central 。

## 上传 library 到 jcenter

我们对 jcenter 和 maven central 已经了解了，那么就让我们开始最重要环节：上传

![img](http://7xod2d.com1.z0.glb.clouddn.com/jcenter/jcenter_rule.jpg)

其实，这个过程非常简单，一旦你配置成功之后，只需要修改一些值，就可以到处使用了，接下来分步骤给大家讲解一下：

1. 第一步：注册帐号

   [Bintray 官网](https://bintray.com/) 首页默认注册是**组织** ，

   个人注册地址是：[https://bintray.com/signup/oss](https://bintray.com/signup/oss)

   个人注册地址是：[https://bintray.com/signup/oss](https://bintray.com/signup/oss)

   个人注册地址是：[https://bintray.com/signup/oss](https://bintray.com/signup/oss)

   重要的事情说三遍

   注意：不能使用国内的邮箱注册，可以使用 google 帐号，推荐使用 github 帐号注册

2. 获取 api key

   登陆之后，点击 **右上角你的头像 -> Edit Profile -> API Key**

   ![img](http://7xod2d.com1.z0.glb.clouddn.com/jcenter/jcenter_copy_apikey.png)

   **API Key** 后面会用到，先复制出来，备用

3. 创建个人 Maven 仓库

   1. 回到个人主页，点击 **Add New Repository**

      ![img](http://7xod2d.com1.z0.glb.clouddn.com/jcenter/jcenter_add.png)

      设置库名称、类型、Licenses、描述信息： **库的名称后面会用到**

      ![img](http://7xod2d.com1.z0.glb.clouddn.com/jcenter/jcenterjcenter_add_detail.png)

4. 环境配置

   1. 创建 module 或者 打开你创建好的module，在 Project 的 build.gradle 中添加 Maven 和 Jfrog Bintray 的依赖：

   ​

   ```
   buildscript {
       repositories {
           jcenter()
       }
       dependencies {
           classpath 'com.android.tools.build:gradle:2.3.0'

           // NOTE: Do not place your application dependencies here; they belong
           // in the individual module build.gradle files
           // 添加下面两行
           classpath 'com.github.dcendents:android-maven-gradle-plugin:1.5'
           classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.7.3'
       }
   }

   allprojects {
       repositories {
           jcenter()
       }
   }

   task clean(type: Delete) {
       delete rootProject.buildDir
   }
   ```

   关于版本号，可以去这里查看 [Maven](https://github.com/dcendents/android-maven-gradle-plugin) 和 [Jfrog Bintray](https://github.com/bintray/gradle-bintray-plugin) 的最新版本

   ​

   1. 在 module 的 builde.gradle 开头添加以下配置：

      ```
      //添加这两行
      apply plugin: 'com.github.dcendents.android-maven'
      apply plugin: 'com.jfrog.bintray'
      ```

      在 module 的  builde.gradle 末尾添加以下配置：

      ​

      ```
      // 项目主页，需要修改
      def siteUrl = 'https://bintray.com/goghtsui/RxOkRetrofit'
      // 项目的git地址（必须是git地址，可以不存在），需要修改
      def gitUrl = 'git@github.com:goghtsui/TvRecyclerView.git'
      // 上传到 Bintray 的库名称（刚才创建的名称），需要修改
      def libName = "RxOkRetrofit"
      // 这两个参数配置是为了最终生成 compile 'com.xxx:xxxx:1.0.0'  group  version 是关键字，自动识别的，需要修改
      group = "com.gogh";
      version = "1.0.01"

      install {
          repositories.mavenInstaller {
              // 生成pom.xml和参数
              pom {
                  project {
                      packaging 'aar'
                      // 可选，项目名称，需要修改
                      name 'RxOkRetrofit'
                      // 可选，项目描述，需要修改
                      description 'Support for http request, use okhttp rxjava and retrofit.'
                      url siteUrl // 项目主页，这里是引用上面定义好

                      // 软件开源协议，现在一般都是Apache License2.0
                      licenses {
                          license {
                              name 'The Apache Software License, Version 2.0'
                              url 'http://www.apache.org/licenses/LICENSE-2.0.txt'
                          }
                      }

                      //填写开发者基本信息，需要修改
                      developers {
                          developer {
                              id 'gaoxiaofeng' // 开发者的id
                              name 'gaoxiaofeng' // 开发者名字
                              email 'xiaofeng355@gmail.com' // 开发者邮箱
                          }
                      }

                      // SCM
                      scm {
                          connection gitUrl // Git仓库地址
                          developerConnection gitUrl // Git仓库地址
                          url siteUrl // 项目主页
                      }
                  }
              }
          }
      }

      //上传到JCenter
      Properties properties = new Properties()
      properties.load(project.rootProject.file('local.properties').newDataInputStream())

      bintray {
      	// 读取 local.properties 文件里面的 bintray.user 登录用户名
          user = properties.getProperty("bintray.username")    
          // 读取 local.properties 文件里面的 bintray.apikey
          key = properties.getProperty("bintray.apikey")   
          configurations = ['archives']
          pkg {
              // 这里的repo值必须要和你创建Maven仓库的时候的名字一样，需要修改
              repo = "RxOkRetrofit"
              // 发布到 JCenter 上的项目名字
              name = libName
              websiteUrl = siteUrl
              vcsUrl = gitUrl
              licenses = ["Apache-2.0"]
              publish = true //是否是公开项目
          }
      }

      // 生成jar包的task
      task sourcesJar(type: Jar) {
          from android.sourceSets.main.java.srcDirs
          classifier = 'sources'
      }
      // 生成jarDoc的task
      task javadoc(type: Javadoc) {
          options.encoding "UTF-8"
          options.charSet 'UTF-8'
          source = android.sourceSets.main.java.srcDirs
          classpath += project.files(android.getBootClasspath().join(File.pathSeparator))
          failOnError false // 忽略注释语法错误，如果用jdk1.8你的注释写的不规范就编译不过
      }
      // 生成javaDoc的jar
      task javadocJar(type: Jar, dependsOn: javadoc) {
          classifier = 'javadoc'
          from javadoc.destinationDir
      }
      artifacts {
          archives javadocJar
          archives sourcesJar
      }
      ```

      ​

   2. 在  build.gradle 中还需要添加的配置：

      ​

      ```
      android{
          lintOptions {
              checkReleaseBuilds false
              abortOnError false
          }
      }
      ```

      ​

   3. 在 local.properties 中添加上面需要的参数值：

      ​

      ```
      bintray.username= bintray注册的用户名
      bintray.apikey= 在文章开头获取的 apikey
      ```

      ​

5. 上传

   环境基本上就配置好了，需要修改的都己经给出，如果编译没有问题，就可以上传了

   - Windows 环境

     ```
     gradlew clean build bintrayUpload -PbintrayUser=BINTRAY_USERNAME -PbintrayKey=BINTRAY_KEY -PdryRun=false
     ```

     ​

   - Mac OS 环境

     如果出现拒绝该命令 `./gradlew: Permission denied`，可以先运行 `chmod +x gradlew`再运行该命令；

     ```
     ./gradlew clean build bintrayUpload -PbintrayUser=BINTRAY_USERNAME -PbintrayKey=BINTRAY_KEY -PdryRun=false
     ```

     上面命令中 **BINTRAY_USERNAME** 是你在 bintray 上注册的用户名，**BINTRAY_KEY** 是文章开头获取的 API Key，替换了用户名和 API key 回车执行，等到控制台最终输出 **BUILD SUCCESSFUL** 就表明项目上传成功了

## 发布到 jcenter

这个时候回到 bintray 我们的 maven 仓库中，进入我们刚上传成功的 packge ，可以看到像这样：

![jcenter_library](http://7xod2d.com1.z0.glb.clouddn.com/jcenter/jcenter_library.png)

   ​

   刚才新建的库已经有信息了，点击名称打开，会看到以下信息，然后点击 **Add to JCenter** ：

**![jcenter-detail](http://7xod2d.com1.z0.glb.clouddn.com/jcenter/jcenter_detail.png)**

接下来打开的是提交的页面，如下：

![jcenter_send](http://7xod2d.com1.z0.glb.clouddn.com/jcenter/jcenter_send.png)

什么都不用填，直接点击 **Send** 按钮，等待审核！

至此，我们已经正确的创建、提交了我们的项目到jcenter，当然了，通过审核之后就有可以通过 compile 的方式依赖使用了，屌屌的！

## 同步到Maven Central

在详情页面，切换到 Maven Central 标签下， 设置好自己的 **User token** 和 **password** 点击 **Sync**：

![jcenter_sync](http://7xod2d.com1.z0.glb.clouddn.com/jcenter/jcenter_sync.png)

## Url方式依赖

刚才我们提到过，一些个人维护或者其它平台的libraries 可以通过 url 的形式配置使用，所以，在没有通过审核之前，我们还是可以直接通过库的地址来依赖使用：

1. 找到库的链接地址，点击项目名称打开详情之后，我们可以看到右上角的地址：

   ![jcenter_detail_address](http://7xod2d.com1.z0.glb.clouddn.com/jcenter/jcenter_test_address.png)

2. 在 project 的 build.gradle 添加 url

   ```gr
   allprojects {
       repositories {
           jcenter()
           // 添加这行
           maven {url '右上角的链接'}
       }
   }
   ```

3. 在 module 的 build.gradle 添加依赖

   ```groovy
   // 注意，第二个信息是 module 的名称
   compile 'com.gogh:module名称:1.0.1'
   // 如果出错，可以使用下面的 @arr 依赖形式
   // compile 'com.gogh:module名称:1.0.1@arr'
   ```

其实我们可以通过 jcenter 项目的详情页，左下角看到不同的引用形式：

![jcenter_dependencies](http://7xod2d.com1.z0.glb.clouddn.com/jcenter/jcenter_detail_dependencies.png)

## 总结

总的来说，还是非常简单的，只需要注册相应托管服务平台的账号，然后创建对应的项目，再配置本地的项目 pom 信息，上传并提交审核，同时可以同步到 maven central ，就是这么简单！如果你产出了更多更好的优质代码，不妨共享出来造福更多的程序猿！

   

