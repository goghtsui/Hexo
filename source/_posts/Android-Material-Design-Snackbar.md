title: Android Material Design - Snackbar
date: 2015-12-24 16:49:57
categories: [Android]
tags: [Material Design, Snackbar]
---

> 原作者：Ravi Tamada
> 原文地址：[http://www.androidhive.info/...example/](http://www.androidhive.info/2015/09/android-material-design-snackbar-example/)

Material Design中一个有趣的组件介绍就是**[Snackbar][1]**。Snackbar就像**Toast**消息，只是它提供了动作交互。Snackbar在屏幕底部显示，并且可以滑动关闭。

这篇文章讲述的是关于Snackbar和一些不同场景下的例子。

#### 源码下载

[戳我下载](http://download.androidhive.info/)

#### 1.简单的Snackbar

下面是一个简单的Snackbar语法。**make**方法接收三个参数：View、显示的信息、消息显示的持续时间。

通常传递 **CoordinatorLayout** 作为view参数是最好的选择，因为它允许Snackbar一些特性，像滑动取消、像FloatingActionButton控件的自动移动。

并且显示的持续时间应该是**LENGTH_SHORT**, **LENGTH_LONG**或者**LENGTH_INDEFINITE**。当**LENGTH_INDEFINITE**被使用时，snackbar显示的时间将是不确定的，而且可以滑动删除。

``` java
Snackbar snackbar = Snackbar
        .make(coordinatorLayout, "Welcome to AndroidHive", Snackbar.LENGTH_LONG);
 
snackbar.show();
```
![example](http://www.androidhive.info/wp-content/uploads/2015/09/android-snackbar-example.png)
<!-- more -->


#### 2.Snackbar与动作回调

你也可以使用一个回调方法_setAction()_，使得它可以和我们有一些动作交互。

``` java
Snackbar snackbar = Snackbar
        .make(coordinatorLayout, "Message is deleted", Snackbar.LENGTH_LONG)
        .setAction("UNDO", new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Snackbar snackbar1 = Snackbar.make(coordinatorLayout, "Message is restored!", Snackbar.LENGTH_SHORT);
                snackbar1.show();
            }
        });
 
snackbar.show();
```
![example](http://www.androidhive.info/wp-content/uploads/2015/09/android-snackbar-with-action-callback-undo.png)

#### 3.自定义Snackbar

Snackbar默认文字颜色 **white**、默认背景是 **#323232**。你可以按照下面的方式修改：
``` java
Snackbar snackbar = Snackbar
        .make(coordinatorLayout, "No internet connection!", Snackbar.LENGTH_LONG)
        .setAction("RETRY", new View.OnClickListener() {
            @Override
            public void onClick(View view) {
            }
        });
 
// Changing message text color
snackbar.setActionTextColor(Color.RED);
 
// Changing action button text color
View sbView = snackbar.getView();
TextView textView = (TextView) sbView.findViewById(android.support.design.R.id.snackbar_text);
textView.setTextColor(Color.YELLOW);
snackbar.show();

```
![example](http://www.androidhive.info/wp-content/uploads/2015/09/android-snackbar-custom-color-view-text-color.png)


#### 4.创建新项目

现在我们创建一个demo来看看Snackbar动作，而且应用用**CoordinatorLayout **和**FloatingActionButton**。

1.在Android Studio中，执行**File => New Project**，然后填上所有的信息来创建一个新工程。

2.打开**Build.gradle**然后添加库的依赖
`build.gradle`
``` java
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:23.0.1'
    compile 'com.android.support:design:23.0.1'
}
```
3.可选项，你可以应用**material design**的主题，通过[这里][2]的步骤。

4.打开布局文件，然后我添加了以下代码，是包含**CoordinatorLayout**、**FloatingActionButton**。

`activity_main.xml`
``` java
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/coordinatorLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
 
    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar">
 
        <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="?attr/colorPrimary"
            app:layout_scrollFlags="scroll|enterAlways"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />
    </android.support.design.widget.AppBarLayout>
 
    <LinearLayout
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:paddingLeft="20dp"
        android:paddingRight="20dp"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">
 
        <Button
            android:id="@+id/btnSimpleSnackbar"
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="30dp"
            android:text="Simple Snackbar" />
 
        <Button
            android:id="@+id/btnActionCallback"
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="10dp"
            android:text="With Action Callback" />
 
        <Button
            android:id="@+id/btnCustomSnackbar"
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="10dp"
            android:text="Custom Color" />
 
    </LinearLayout>
 
    <android.support.design.widget.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="end|bottom"
        android:layout_margin="16dp"
        android:src="@android:drawable/ic_dialog_email" />
 
</android.support.design.widget.CoordinatorLayout>
```
5.现在打开**MainActivity.java**然后按照下面的修改，这个activity包含了三个按钮及点击事件，实现了上面提到的不同样式的Snackbar。

`MainActivity.java`
``` java
import android.graphics.Color;
import android.os.Bundle;
import android.support.design.widget.CoordinatorLayout;
import android.support.design.widget.FloatingActionButton;
import android.support.design.widget.Snackbar;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
 
public class MainActivity extends AppCompatActivity {
 
    private Toolbar mToolbar;
    private CoordinatorLayout coordinatorLayout;
    private Button btnSimpleSnackbar, btnActionCallback, btnCustomView;
    private FloatingActionButton fab;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        coordinatorLayout = (CoordinatorLayout) findViewById(R.id
                .coordinatorLayout);
 
        fab = (FloatingActionButton) findViewById(R.id.fab);
 
        mToolbar = (Toolbar) findViewById(R.id.toolbar);
 
        setSupportActionBar(mToolbar);
 
        btnSimpleSnackbar = (Button) findViewById(R.id.btnSimpleSnackbar);
        btnActionCallback = (Button) findViewById(R.id.btnActionCallback);
        btnCustomView = (Button) findViewById(R.id.btnCustomSnackbar);
 
        btnSimpleSnackbar.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Snackbar snackbar = Snackbar
                        .make(coordinatorLayout, "Welcome to AndroidHive", Snackbar.LENGTH_LONG);
 
                snackbar.show();
            }
        });
 
        btnActionCallback.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Snackbar snackbar = Snackbar
                        .make(coordinatorLayout, "Message is deleted", Snackbar.LENGTH_LONG)
                        .setAction("UNDO", new View.OnClickListener() {
                            @Override
                            public void onClick(View view) {
                                Snackbar snackbar1 = Snackbar.make(coordinatorLayout, "Message is restored!", Snackbar.LENGTH_SHORT);
                                snackbar1.show();
                            }
                        });
 
                snackbar.show();
            }
        });
 
        btnCustomView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Snackbar snackbar = Snackbar
                        .make(coordinatorLayout, "No internet connection!", Snackbar.LENGTH_LONG)
                        .setAction("RETRY", new View.OnClickListener() {
                            @Override
                            public void onClick(View view) {
                            }
                        });
 
                // Changing message text color
                snackbar.setActionTextColor(Color.RED);
 
                // Changing action button text color
                View sbView = snackbar.getView();
                TextView textView = (TextView) sbView.findViewById(android.support.design.R.id.snackbar_text);
                textView.setTextColor(Color.YELLOW);
 
                snackbar.show();
            }
        });
    }
}
```

6.运行这个项目，可以测试这几种效果。

![example](http://www.androidhive.info/wp-content/uploads/2015/09/android-material-design-snackbar-example.png)









[1]: https://www.google.co.in/design/spec/components/snackbars-toasts.html
[2]: http://tips.androidhive.info/2015/09/android-how-to-apply-material-design-theme/