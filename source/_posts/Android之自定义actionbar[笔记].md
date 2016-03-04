title: Android之自定义actionbar[笔记]
date: 2016-03-01 11:06:22
categories: [Android]
tags: [沉浸式状态栏]
---

自定义actionbar或toolbar的属性样式：

``` xml
<style name="detail_actionbar_style" parent="AppBaseTheme">
        <item name="android:windowAnimationStyle">@null</item>
        <item name="android:windowBackground">@android:color/transparent</item>
        <!--<item name="android:colorBackgroundCacheHint">@null</item>-->
        <!--<item name="android:windowFrame">@null</item>-->
        <!--<item name="android:windowIsFloating">false</item>-->
        <item name="android:windowIsTranslucent">true</item>
        <item name="android:windowTranslucentNavigation">true</item>
        <item name="android:windowNoTitle">true</item>
        <item name="android:windowActionBar">false</item>
        <!-- actionbar -->
        <item name="android:windowActionBarOverlay">true</item>
        <item name="android:windowContentOverlay">@null</item>
        <item name="android:actionBarStyle">@style/ActionBar.Style.Transparent</item>
        <item name="android:actionOverflowButtonStyle">@style/OverFlow</item>
    </style>
    <!-- actionbar -->
    <style name="ActionBar.Transparent" parent="@android:style/Theme.Holo.Light">
        <item name="android:windowActionBarOverlay">true</item>
        <item name="android:windowContentOverlay">@null</item>
        <item name="android:actionBarStyle">@style/ActionBar.Style.Transparent</item>
        <item name="android:actionOverflowButtonStyle">@style/OverFlow</item>
    </style>
    <!-- 实现Actionbar的透明度 -->
    <style name="ActionBar.Style.Transparent" parent="@android:style/Widget.Holo.ActionBar">
        <item name="android:background">@android:color/transparent</item>
        <item name="android:titleTextStyle">@style/ActionBarText</item>
    </style>
    <!-- 标题文字 -->
    <style name="ActionBarText">
        <item name="android:textSize">19sp</item>
        <item name="android:textColor">@android:color/white</item>
    </style>
    <!-- 重写actionbar中 OverFlow的属性 -->
    <style name="OverFlow" parent="@android:style/Widget.Holo.ActionButton.Overflow">
        <item name="android:src">@drawable/custom_actionbar_overflow</item>
    </style>
```