title: Android知识点之Tanlayout
date: 2017-01-19 09:56:58
categories: [Android]
tags: [TabLayout, enum]
---

## 序言

开发过程中多多少少会遇到一些坑，也会留下一些坑，还有一些小的容易忽略的细节，或者从其他大牛那里了解到的知识点，发现了就整理下来，做个备注，如果恰好有人遇到了，拿走别客气。

## Tablayout

### 问题

> 为什么tablayout的英文（字母）标签名称默认全是大写？

### 解决方案

tablayout + viewpager + fragment的分页模式很常见了（这只是其中一种），这个不是重点，重点是tablayout的文字属性。

可能你使用tablayout的显示分类都是中文的， 比如：新闻、数码、设计、前端...   

不知道你有没有使用过**英文**或者说**字母**的标签名称，你可以直接使用Android Studio 创建一个默认的分页的module，默认标题是英文的，所以你可以看到显示出来全部都是大写的，如果正好使用了这样的名称，那么恭喜你，你可以不用往下看了。但是由于我所使用的是首字母大写的形式，所以最后发现是一个属性的问题：**textAllCaps**

最终的解决方案是，你可以为tablayout自定义一个style，设置textAllCaps属性为false：

<!-- more -->

``` xml
 <style name="TabLayout.LowerCase" parent="Widget.Design.TabLayout">
      <item name="tabTextAppearance">@style/TabTextAppearance</item>
 </style>
 <style name="TabTextAppearance" parent="TextAppearance.Design.Tab">
      <item name="textAllCaps">false</item>
 </style>
```

``` xml
<android.support.design.widget.TabLayout
            android:id="@+id/tabs"
            style="@style/TabLayout.LowerCase"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:tabMode="scrollable"/>
```