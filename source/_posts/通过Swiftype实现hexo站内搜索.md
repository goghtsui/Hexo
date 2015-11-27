title: 通过Swiftype实现hexo站内搜索
date: 2015-11-25 17:30:54
categories: [Hexo]
tags: [hexo, swiftype, pacman]
---
hexo默认提供的是google的搜索，但是国内很蛋疼，无意中了解到swiftype效果不错，之前也看了一些方法不是很凑效，无奈自己研究了一下，可以正常使用了，这里就把方法share给大家，下面就直接进入正题吧。

#### 注册swiftype账号
官方地址：[https://swiftype.com/](https://swiftype.com/)

#### 创建搜索引擎
注册完账号，接下来就是创建搜索引擎了，这里都是以图片引导，关键步骤都有;
1、CREATE AN ENGINE：
![CREATEANENGINE](http://7xod2d.com1.z0.glb.clouddn.com/swiftype/swiftfirst.jpg)
2、继续点击创建：
![create](http://7xod2d.com1.z0.glb.clouddn.com/swiftype/swiftcreate.png)
3、填写自己的域名，不带最后的反斜杠，点击VERIFY，4个验证项，通过之后会让你输入引擎的名字：
![enginename](http://7xod2d.com1.z0.glb.clouddn.com/swiftype/switysetname.png)
4、接下来是让你配置sitemap（关于sitemap自行搜索教程），地址统一是：域名/sitemap.xml
![sitemap](http://7xod2d.com1.z0.glb.clouddn.com/swiftype/swiftaddsitemap.png)
5.1、点击COMPLETE SETUP，创建完成，会进入到如下界面，这里提供的代码就是需要在hexo配置的：
![homepage](http://7xod2d.com1.z0.glb.clouddn.com/swiftype/swiftinstallcode.png)
5.2、向下滚动，可以点击content 查看自己的哪些数据被抓取出来了，跳转后页面右侧而且还可以测试搜索功能：
![contentdata](http://7xod2d.com1.z0.glb.clouddn.com/swiftype/swifttextdata.png)
6、点击上面的INTEGRATE -> INSTALL SEARCH ，进行一些关键的配置：
![install](http://7xod2d.com1.z0.glb.clouddn.com/swiftype/swiftinstallbutton.png)
7、点击CHANGE CONFIGURATION：
![change](http://7xod2d.com1.z0.glb.clouddn.com/swiftype/swiftchangeconfig.png)
8、进行一些更详细的配置，4个部分，样式默认就好，也可以自己选，这里就说下面两个部分（**results container** - 搜索结果页），我使用的是默认的，本页面底部有自定义搜索页的案例。
![container](http://7xod2d.com1.z0.glb.clouddn.com/swiftype/swiftresultcontaner.png)
9、配置**Search field**，这个就是搜索框-input的相关配置了，hexo主题默认就有，而且swiftype提供的SEARCH FIELD都是一样的input标签：
![searchfield](http://7xod2d.com1.z0.glb.clouddn.com/swiftype/swiftsearchfield.png)
 
 如果都保持默认设置的话，完成到步骤5.1就可以看下面的教程了。

#### hexo主题配置（pacman）
我的主题是Pacman的，这里就以pacman为例，其实没有本质的区别，大部分都是在主题目录的文件。

**1、**首先打开**pacman\\_config.yml**文件在末尾添加如下代码，提供对swiftype的支持：
```
swift_search:
  enable: true
```

**2、** 在**hexo\source**目录（注意不是pacman主题的source目录）下**新建一个search文件夹**（如果不存在的），在里面**新建一个index.md**，index.md中写入如下代码：

```
layout: search
title: search
---
```
**3、** 切换的到**pacman\layout\\_partial**目录下，大部分的代码配置都在这里完成的。先**打开header.ejs**，把
```
<li>
...
...
</li>

```
之间的代码清空（我的默认是google的搜索，这里再添加上swftype的搜索，也就是第一个if部分），整合代码如下，直接**copy**过去就行：
```
<% if	(theme.swift_search&&theme.swift_search.enable){ %>
	<form class="search" action="<%- config.root %>search/index.html" method="get" accept-charset="utf-8">
	<label>Search</label>
	<input type="text" class="st-default-search-input" maxlength="20" placeholder="Search" />
	</form>
	
	<% }else if	(theme.google_cse&&theme.google_cse.enable){ %>
	<form class="search" action="<%- config.root %>search/index.html" method="get" accept-charset="utf-8">
	<label>Search</label>
	<input type="text" id="search" autocomplete="off" name="q" maxlength="20" placeholder="<%= __('search') %>" />
	</form>
	
	<% } else { %>
	<form class="search" action="//google.com/search" method="get" accept-charset="utf-8">
	<label>Search</label>
	<input type="text" id="search" name="q" autocomplete="off" maxlength="20" placeholder="<%= __('search') %>" />
	<input type="hidden" name="q" value="site:<%- config.url.replace(/^https?:\/\//, '') %>">
	</form>
	<% } %>
```

**4、**将原来的**search.ejs**中的代码**清空**，**替换为以下的代码**，其实主要就是为了控制结果的显示样式（后期可以自己调整），**注意：将最下面的` <script ...   >  ... ` 部分替换成swiftype给你的js代码**。
```
<% if(theme.swift_search.enable) { %>
<div  id="container" class="page">
  <div id="st-results-container" class="st-search-container" style="width:80%">正在加载搜索结果，请稍等。</div>
  <style>.st-result-text {
  background: #fafafa;
  display: block;
  border-left: 0.5em solid #ccc;
  -webkit-transition: border-left 0.45s;
  -moz-transition: border-left 0.45s;
  -o-transition: border-left 0.45s;
  -ms-transition: border-left 0.45s;
  transition: border-left 0.45s;
  padding: 0.5em;
}
@media only screen and (min-width: 768px) {
  .st-result-text {
    padding: 1em;
  }
}
.st-result-text:hover {
  border-left: 0.5em solid #ea6753;
}
.st-result-text h3 a{
  color: #2ca6cb;
  line-height: 1.5;
  font-size: 22px;
}
.st-snippet em {
  font-weight: bold;
  color: #ea6753;
}</style>

<script type="text/javascript">
  (function(w,d,t,u,n,s,e){w['SwiftypeObject']=n;w[n]=w[n]||function(){
  (w[n].q=w[n].q||[]).push(arguments);};s=d.createElement(t);
  e=d.getElementsByTagName(t)[0];s.async=1;s.src=u;e.parentNode.insertBefore(s,e);
  })(window,document,'script','//s.swiftypecdn.com/install/v2/st.js','_st');

  _st('install','w7ca3xWstLkz2UvAeyAJ','2.0.0');
</script>

<% } %>

```
**5、**打开**footer.ejs或header.ejs**，在最后一个标签（`</div>`）之前添加swiftype分配给你的js代码（同上），我的是：
```
<script type="text/javascript">
  (function(w,d,t,u,n,s,e){w['SwiftypeObject']=n;w[n]=w[n]||function(){
  (w[n].q=w[n].q||[]).push(arguments);};s=d.createElement(t);
  e=d.getElementsByTagName(t)[0];s.async=1;s.src=u;e.parentNode.insertBefore(s,e);
  })(window,document,'script','//s.swiftypecdn.com/install/v2/st.js','_st');

  _st('install','w7ca3xWstLkz2UvAeyAJ','2.0.0');
</script>
```

到这里所有的修改都已经完成了，如果没有问题的话，命令行执行：
```
> hexo clean
> hexo d -g
```
等部署完成，你就可以打开你的Blog任性的搜索了

#### 推荐
这里推荐一篇其他大神的Blog，可以自定义搜索结果页面，[点我跳转.](http://www.jerryfu.net/post/search-engine-for-hexo-with-swiftype-v2.html)