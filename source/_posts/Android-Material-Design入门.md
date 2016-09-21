title: Android Material Design入门
date: 2015-12-31 09:41:32
categories: [Android]
tags: [Material Design]
---

>原作者：Ravi Tamada
>原文地址：[http://www.androidhive.info/...with-material-design/](http://www.androidhive.info/2015/04/android-getting-started-with-material-design/)

你可能听说了在Android Lollipop（棒棒糖）版本中有关Material Design的介绍。在Material Design中，介绍了许多新的东西，像Material主题、新的widget、自定义阴影、矢量图片和自定义动画。如果你还没有使用过Material Design，那么这篇文章会给你一个好的开始。

在这个教程中，我们将学习Material Design开发基础的步骤，比如编写自定义主题、使用RecyclerView实现导航抽屉。

通过下面的链接获取更多的关于Material Design的知识：

>[Material Design Specifications](http://www.google.com/design/spec/material-design/introduction.html#)
>[Creating Apps with Material Design](http://developer.android.com/intl/zh-tw/training/material/index.html)

本文资源链接：

>源码下载：[点击获取](http://download.androidhive.info/download?code=WPSkdrdZprHT0KLCZS3ClafgXBikGqM4r7FnNYdsdUTmlAkK6%2F2mkT0heOlNOq4U82rzqbod%2F14yU2uk5TWY4Zp%2FAYx6oiD7SKI%2FEgtUapzQUqkqcWEXX1bmw%3D%3DvqARiMEKqkqsXGbVf3vVUoffTqQcD2qfqZo)
>视频演示地址：[Yutube](https://youtu.be/jDXX_wDvarM)
<!-- more -->


## 1. 下载Android Studio

在进一步了解之前,下载Android Studio并且进行必须的设置，因为之后我将使用Android Studio做教程讲解。如果你是第一次尝试Android Studio，通过[概述文档][1]了一下Android Studio。





## 2. Material Design颜色自定义

Material Design提供了一些其颜色主题的自定义属性，但是我们使用主要的五种，来自定义整个主题：

- `colorPrimaryDark` – 应用于通知栏的背景色

- `colorPrimary` – 这是应用最主要的颜色，应用于toolbar的背景色

- `textColorPrimary` – 这是文字的颜色，应用于toolbar的标题

- `windowBackground` – 这是应用默认的背景色

- `navigationBarColor` – 这个颜色定义了底部导航按钮的背景色

![android-material-design-color-schema](http://www.androidhive.info/wp-content/uploads/2015/04/android-material-design-color-schema.png)

你可以通过Material Design颜色模型，去选择适合你应用的一套颜色


## 3. 创建 Material Design 主题


**1.** 在Android Studio中，通过**File ⇒ New Project`**并且填写其他需要的选项，来创建一个新的工程，当被提示选择默认的Activity时，选择**Blank Activity**即可

**2.** 打开**res ⇒ values ⇒ strings.xml**并且添加以下文字：

`strings.xml`
``` xml
<resources>
    <string name="app_name">Material Design</string>
    <string name="action_settings">Settings</string>
    <string name="action_search">Search</string>
    <string name="drawer_open">Open</string>
    <string name="drawer_close">Close</string>
 
    <string name="nav_item_home">Home</string>
    <string name="nav_item_friends">Friends</string>
    <string name="nav_item_notifications">Messages</string>
 
    <!-- navigation drawer item labels  -->
    <string-array name="nav_drawer_labels">
        <item>@string/nav_item_home</item>
        <item>@string/nav_item_friends</item>
        <item>@string/nav_item_notifications</item>
    </string-array>
 
    <string name="title_messages">Messages</string>
    <string name="title_friends">Friends</string>
    <string name="title_home">Home</string>
</resources>
```

**3.** 打开**res ⇒ values ⇒ colors.xml**并且添加以下颜色值，如果你没有找到`colors.xml`，就新建一个文件即可

`colors.xml`
``` xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="colorPrimary">#F50057</color>
    <color name="colorPrimaryDark">#C51162</color>
    <color name="textColorPrimary">#FFFFFF</color>
    <color name="windowBackground">#FFFFFF</color>
    <color name="navigationBarColor">#000000</color>
    <color name="colorAccent">#FF80AB</color>
</resources>
```

**4.** 打开**res ⇒ values ⇒ dimens.xml**并添加以下尺寸值

`dimens.xml`
``` xml
<resources>
    <!-- Default screen margins, per the Android Design guidelines. -->
    <dimen name="activity_horizontal_margin">16dp</dimen>
    <dimen name="activity_vertical_margin">16dp</dimen>
    <dimen name="nav_drawer_width">260dp</dimen>
</resources>
```

**5.** 打开**res ⇒ values ⇒ styles.xml**并添加以下样式。这些样式适用于所有的安卓版本，这里我定义主题的名字为：**MyMaterialTheme**

`styles.xml`
``` xml
<resources>
 
    <style name="MyMaterialTheme" parent="MyMaterialTheme.Base">
 
    </style>
 
    <style name="MyMaterialTheme.Base" parent="Theme.AppCompat.Light.DarkActionBar">
        <item name="windowNoTitle">true</item>
        <item name="windowActionBar">false</item>
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>
     
</resources>
```

**6.** 在**res**下新建一个文件夹：**values-v21**，在这下面新建另外一个**styles.xml**天下一下样式，这些延时只适用于**Android Lollipop**版本

`styles.xml`
```xml
<resources>
 
    <style name="MyMaterialTheme" parent="MyMaterialTheme.Base">
        <item name="android:windowContentTransitions">true</item>
        <item name="android:windowAllowEnterTransitionOverlap">true</item>
        <item name="android:windowAllowReturnTransitionOverlap">true</item>
        <item name="android:windowSharedElementEnterTransition">@android:transition/move</item>
        <item name="android:windowSharedElementExitTransition">@android:transition/move</item>
    </style>
 
</resources>
```

**7.** 现在我们已经准备好了基本的Material Design样式，为了应用这个主题，打开**AndroidManifest.xml**并通过<application>标签下的**android:theme attribute of**属性为应用设置该主题

`android:theme="@style/MyMaterialTheme"`

在设置了该主题之后，你的**AndroidManifest.xml**应该是下面的样子：

`AndroidManifest.xml`
``` xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="info.androidhive.materialdesign" >
 
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/MyMaterialTheme" >
        <activity
            android:name=".activity.MainActivity"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
 
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
 
</manifest>
```

现在运行你的应用，你可以看到通知栏的颜色已经是我们设置的样式的颜色了。

![android-material-design-notification-bar](http://www.androidhive.info/wp-content/uploads/2015/04/android-material-design-notification-bar.png)


**3.1** 添加Toolbar(Action Bar)

添加toolbar是非常容易的，你需要做的就是，为toolbar创建一个单独的layout，在其他layout中需要显示的地方使用。

**8.** 新建一个xml文件**res ⇒ layout ⇒ toolbar.xml**并添加`android.support.v7.widget.Toolbar`控件，这个toolbar具有特定的宽度和主题

`toolbar.xml`
``` xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:local="http://schemas.android.com/apk/res-auto"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    local:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    local:popupTheme="@style/ThemeOverlay.AppCompat.Light" />
```

**9.** 打开主Activity的布局文件(activity_main.xml)，并通过`<include/>`来添加对toolbar的使用

`activity_main.xml`
``` xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
 
    <LinearLayout
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:orientation="vertical">
 
        <include
            android:id="@+id/toolbar"
            layout="@layout/toolbar" />
    </LinearLayout>
 
 
</RelativeLayout>
```

运行这个应用，并且看看toolbar是不是显示在屏幕上

![android-material-design-toolbar](http://www.androidhive.info/wp-content/uploads/2015/04/android-material-design-toolbar1.png)

现在让我们试着给toolbar添加标题和交互

**10.** 下载这个[搜索图标][2]，在Android Studio中通过Image Asset来引用它

**11.** 右键**res ⇒ New ⇒ Image Asset**，会显示一个弹窗来引入资源，找到你下载的搜索图标，Asset Type选择**Action Bar and Tab Icons**，并命名为**ic_search_action**

![android-studio-importing-image-asset](http://www.androidhive.info/wp-content/uploads/2015/04/android-studio-importing-image-asset.png)

**12.** 图标导入完成之后，打开**res ⇒ menu ⇒ menu_main.xml**并且添加下面的搜索菜单：

`menu_main.xml`
``` xml
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    tools:context=".MainActivity">
 
    <item
        android:id="@+id/action_search"
        android:title="@string/action_search"
        android:orderInCategory="100"
        android:icon="@drawable/ic_action_search"
        app:showAsAction="ifRoom" />
 
    <item
        android:id="@+id/action_settings"
        android:title="@string/action_settings"
        android:orderInCategory="100"
        app:showAsAction="never" />
</menu>
```

**13.** 现在打开**MainActivity.java**并且做如下修改：

- 1.继承的activity是**AppCompatActivity**

- 2.调用`setSupportActionBar()`并传递toolbar对象，以设置toolbar为可用状态

- 3. 复写**onCreateOptionsMenu()**和**onOptionsItemSelected()**方法来设置toolbar的交互行为

`MainActivity.java`

``` java
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.view.Menu;
import android.view.MenuItem;
 
public class MainActivity extends AppCompatActivity {
 
    private Toolbar mToolbar;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        mToolbar = (Toolbar) findViewById(R.id.toolbar);
 
        setSupportActionBar(mToolbar);
        getSupportActionBar().setDisplayShowHomeEnabled(true);
    }
 
 
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }
 
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();
 
        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }
 
        return super.onOptionsItemSelected(item);
    }
}
```

在做了以上修改之后，如果你运行应用，你应该能够在toolbar中看到搜索图标和更多菜单选项了

![android-material-design-toolbar-action-items](http://www.androidhive.info/wp-content/uploads/2015/04/android-material-design-toolbar-action-items.png)

**3.2** 添加导航抽屉

添加导航抽屉，同样是按照之前lollipop的方式，但是如果菜单项使用列表视图，在Material design中要使用**RecyclerView**。因此让我们看看怎样实现**RecyclerView**导航抽屉。

**14.** 在你项目的java文件夹中，新建三个包：_activity_、_adapter_、_model_，并且把_MainActivity.java_移动到_activity_包下，这样来保证项目的条理性

**15.** 打开model下的**build.gradle**，添加下面的依赖，然后执行**Build ⇒ Rebuild Project**来下载必须的库

`build.gradle`
``` xml
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:22.2.0'
    compile 'com.android.support:recyclerview-v7:22.2.+'
}
```

**16.** 在_model_包下，新建一个class文件，命名为**NavDrawerItem.java**，添加以下代码，这个class是一个实体类，它定义了导航抽屉里每一行的菜单项

`NavDrawerItem.java`

``` java
package info.androidhive.materialdesign.model;
 
/**
 * Created by Ravi on 29/07/15.
 */
public class NavDrawerItem {
    private boolean showNotify;
    private String title;
 
 
    public NavDrawerItem() {
 
    }
 
    public NavDrawerItem(boolean showNotify, String title) {
        this.showNotify = showNotify;
        this.title = title;
    }
 
    public boolean isShowNotify() {
        return showNotify;
    }
 
    public void setShowNotify(boolean showNotify) {
        this.showNotify = showNotify;
    }
 
    public String getTitle() {
        return title;
    }
 
    public void setTitle(String title) {
        this.title = title;
    }
}
```

**17.** 在**res ⇒ layout**之下，新建一个布局文件，叫做**nav_draw_row.xml**添加以下代码。这个layout渲染的导航抽屉每一行的视图，如果你想要自定义导航抽屉菜单项，你应该修改这个文件，现在只有一个TextView

`nav_drawer_row.xml`
``` xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:clickable="true">
 
    <TextView
        android:id="@+id/title"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:paddingLeft="30dp"
        android:paddingTop="10dp"
        android:paddingBottom="10dp"
        android:textSize="15dp"
        android:textStyle="bold" />
 
</RelativeLayout>
```

**18.** 下载这个[个人信息][3]的图标，并把它粘贴到_drawable_ 文件夹下，这一步是可选的，但是这个图标在导航抽屉的header中有使用到

**19.** 新建一个layout命名**fragment_navigation_drawer.xml**，并且添加以下代码。这个layout呈现了整个导航抽屉的视图，它包含了头部部分，用于展示用户信息、RecyclerView来展示列表视图

`fragment_navigation_drawer.xml`
``` xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@android:color/white">
 
 
    <RelativeLayout
        android:id="@+id/nav_header_container"
        android:layout_width="match_parent"
        android:layout_height="140dp"
        android:layout_alignParentTop="true"
        android:background="@color/colorPrimary">
 
        <ImageView
            android:layout_width="70dp"
            android:layout_height="70dp"
            android:src="@drawable/ic_profile"
            android:scaleType="fitCenter"
            android:layout_centerInParent="true" />
 
    </RelativeLayout>
 
 
    <android.support.v7.widget.RecyclerView
        android:id="@+id/drawerList"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/nav_header_container"
        android:layout_marginTop="15dp" />
 
 
</RelativeLayout>
```

**20.** 因为**RecyclerView**是自定义的，我们需要一个adapter类去渲染自定义xml布局，因此，在adapter包下，创建一个适配器类**NavigationDrawerAdapter.java**，然后粘贴下面的代码。这个适配器类适配nav_drawer_row.xml布局并呈现RecycleView抽屉菜单

``` java
import android.content.Context;
import android.support.v7.widget.RecyclerView;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;
 
import java.util.Collections;
import java.util.List;
 
/**
 * Created by Ravi Tamada on 12-03-2015.
 */
public class NavigationDrawerAdapter extends RecyclerView.Adapter<NavigationDrawerAdapter.MyViewHolder> {
    List<NavDrawerItem> data = Collections.emptyList();
    private LayoutInflater inflater;
    private Context context;
 
    public NavigationDrawerAdapter(Context context, List<NavDrawerItem> data) {
        this.context = context;
        inflater = LayoutInflater.from(context);
        this.data = data;
    }
 
    public void delete(int position) {
        data.remove(position);
        notifyItemRemoved(position);
    }
 
    @Override
    public MyViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View view = inflater.inflate(R.layout.nav_drawer_row, parent, false);
        MyViewHolder holder = new MyViewHolder(view);
        return holder;
    }
 
    @Override
    public void onBindViewHolder(MyViewHolder holder, int position) {
        NavDrawerItem current = data.get(position);
        holder.title.setText(current.getTitle());
    }
 
    @Override
    public int getItemCount() {
        return data.size();
    }
 
    class MyViewHolder extends RecyclerView.ViewHolder {
        TextView title;
 
        public MyViewHolder(View itemView) {
            super(itemView);
            title = (TextView) itemView.findViewById(R.id.title);
        }
    }
}
```

**21.** 在activity包下，新建一个fragment叫做**FragmentDrawer.java**。在Android Studio中，新建fragment：_右键activity ⇒ New ⇒ Fragment ⇒ Fragment (Blank)_，并且给出你的fragment的名称

`FragmentDrawer.java`
``` java
/**
 * Created by Ravi on 29/07/15.
 */
 
import android.content.Context;
import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.support.v4.widget.DrawerLayout;
import android.support.v7.app.ActionBarDrawerToggle;
import android.support.v7.widget.LinearLayoutManager;
import android.support.v7.widget.RecyclerView;
import android.support.v7.widget.Toolbar;
import android.view.GestureDetector;
import android.view.LayoutInflater;
import android.view.MotionEvent;
import android.view.View;
import android.view.ViewGroup;
 
import java.util.ArrayList;
import java.util.List;
 
import info.androidhive.materialdesign.R;
import info.androidhive.materialdesign.adapter.NavigationDrawerAdapter;
import info.androidhive.materialdesign.model.NavDrawerItem;
 
public class FragmentDrawer extends Fragment {
 
    private static String TAG = FragmentDrawer.class.getSimpleName();
 
    private RecyclerView recyclerView;
    private ActionBarDrawerToggle mDrawerToggle;
    private DrawerLayout mDrawerLayout;
    private NavigationDrawerAdapter adapter;
    private View containerView;
    private static String[] titles = null;
    private FragmentDrawerListener drawerListener;
 
    public FragmentDrawer() {
 
    }
 
    public void setDrawerListener(FragmentDrawerListener listener) {
        this.drawerListener = listener;
    }
 
    public static List<NavDrawerItem> getData() {
        List<NavDrawerItem> data = new ArrayList<>();
 
 
        // preparing navigation drawer items
        for (int i = 0; i < titles.length; i++) {
            NavDrawerItem navItem = new NavDrawerItem();
            navItem.setTitle(titles[i]);
            data.add(navItem);
        }
        return data;
    }
 
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
 
        // drawer labels
        titles = getActivity().getResources().getStringArray(R.array.nav_drawer_labels);
    }
 
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflating view layout
        View layout = inflater.inflate(R.layout.fragment_navigation_drawer, container, false);
        recyclerView = (RecyclerView) layout.findViewById(R.id.drawerList);
 
        adapter = new NavigationDrawerAdapter(getActivity(), getData());
        recyclerView.setAdapter(adapter);
        recyclerView.setLayoutManager(new LinearLayoutManager(getActivity()));
        recyclerView.addOnItemTouchListener(new RecyclerTouchListener(getActivity(), recyclerView, new ClickListener() {
            @Override
            public void onClick(View view, int position) {
                drawerListener.onDrawerItemSelected(view, position);
                mDrawerLayout.closeDrawer(containerView);
            }
 
            @Override
            public void onLongClick(View view, int position) {
 
            }
        }));
 
        return layout;
    }
 
 
    public void setUp(int fragmentId, DrawerLayout drawerLayout, final Toolbar toolbar) {
        containerView = getActivity().findViewById(fragmentId);
        mDrawerLayout = drawerLayout;
        mDrawerToggle = new ActionBarDrawerToggle(getActivity(), drawerLayout, toolbar, R.string.drawer_open, R.string.drawer_close) {
            @Override
            public void onDrawerOpened(View drawerView) {
                super.onDrawerOpened(drawerView);
                getActivity().invalidateOptionsMenu();
            }
 
            @Override
            public void onDrawerClosed(View drawerView) {
                super.onDrawerClosed(drawerView);
                getActivity().invalidateOptionsMenu();
            }
 
            @Override
            public void onDrawerSlide(View drawerView, float slideOffset) {
                super.onDrawerSlide(drawerView, slideOffset);
                toolbar.setAlpha(1 - slideOffset / 2);
            }
        };
 
        mDrawerLayout.setDrawerListener(mDrawerToggle);
        mDrawerLayout.post(new Runnable() {
            @Override
            public void run() {
                mDrawerToggle.syncState();
            }
        });
 
    }
 
    public static interface ClickListener {
        public void onClick(View view, int position);
 
        public void onLongClick(View view, int position);
    }
 
    static class RecyclerTouchListener implements RecyclerView.OnItemTouchListener {
 
        private GestureDetector gestureDetector;
        private ClickListener clickListener;
 
        public RecyclerTouchListener(Context context, final RecyclerView recyclerView, final ClickListener clickListener) {
            this.clickListener = clickListener;
            gestureDetector = new GestureDetector(context, new GestureDetector.SimpleOnGestureListener() {
                @Override
                public boolean onSingleTapUp(MotionEvent e) {
                    return true;
                }
 
                @Override
                public void onLongPress(MotionEvent e) {
                    View child = recyclerView.findChildViewUnder(e.getX(), e.getY());
                    if (child != null && clickListener != null) {
                        clickListener.onLongClick(child, recyclerView.getChildPosition(child));
                    }
                }
            });
        }
 
        @Override
        public boolean onInterceptTouchEvent(RecyclerView rv, MotionEvent e) {
 
            View child = rv.findChildViewUnder(e.getX(), e.getY());
            if (child != null && clickListener != null && gestureDetector.onTouchEvent(e)) {
                clickListener.onClick(child, rv.getChildPosition(child));
            }
            return false;
        }
 
        @Override
        public void onTouchEvent(RecyclerView rv, MotionEvent e) {
        }
 
        @Override
        public void onRequestDisallowInterceptTouchEvent(boolean disallowIntercept) {
 
        }
 
 
    }
 
    public interface FragmentDrawerListener {
        public void onDrawerItemSelected(View view, int position);
    }
}
```

**22.** 最后，打开首页activity的布局文件**activity_main.xml**，按照下面这样修改。在这个布局中，我们添加了**android.support.v4.widget.DrawerLayout**，来显示导航抽屉菜单

你也必须写出你的fragment下**FragmentDrawer**的正确路径

`actiivty_main.xml`
``` java
<android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
 
 
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
 
        <LinearLayout
            android:id="@+id/container_toolbar"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical">
 
            <include
                android:id="@+id/toolbar"
                layout="@layout/toolbar" />
        </LinearLayout>
 
        <FrameLayout
            android:id="@+id/container_body"
            android:layout_width="fill_parent"
            android:layout_height="0dp"
            android:layout_weight="1" />
 
 
    </LinearLayout>
 
 
    <fragment
        android:id="@+id/fragment_navigation_drawer"
        android:name="info.androidhive.materialdesign.activity.FragmentDrawer"
        android:layout_width="@dimen/nav_drawer_width"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        app:layout="@layout/fragment_navigation_drawer"
        tools:layout="@layout/fragment_navigation_drawer" />
 
</android.support.v4.widget.DrawerLayout>
```

现在，我们已经准备好所有的layout和class，让我们在**MainActivity**中做一些必要的修改，使得导航抽屉可以正常运行

**23.** 打开**MainActivity.java**并且做如下修改

- activity需要实现FragmentDrawer.FragmentDrawerListener**并且复写**onDrawerItemSelected()**方法

- 创建一个**FragmentDrawer的实例，并设置这个菜单选择的监听器

`MainActivity.java`
``` java
import android.support.v4.widget.DrawerLayout;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.support.v7.widget.Toolbar;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
 
public class MainActivity extends AppCompatActivity implements FragmentDrawer.FragmentDrawerListener {
 
    private Toolbar mToolbar;
    private FragmentDrawer drawerFragment;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        mToolbar = (Toolbar) findViewById(R.id.toolbar);
 
        setSupportActionBar(mToolbar);
        getSupportActionBar().setDisplayShowHomeEnabled(true);
 
        drawerFragment = (FragmentDrawer)
                getSupportFragmentManager().findFragmentById(R.id.fragment_navigation_drawer);
        drawerFragment.setUp(R.id.fragment_navigation_drawer, (DrawerLayout) findViewById(R.id.drawer_layout), mToolbar);
        drawerFragment.setDrawerListener(this);
    }
 
 
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }
 
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();
 
        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }
 
        return super.onOptionsItemSelected(item);
    }
 
    @Override
    public void onDrawerItemSelected(View view, int position) {
 
    }
}
```

现在，如果你运行这个应用，你能够看到这个导航抽屉，包含一个header和列表

![androd-material-design-navigation-drawer](http://www.androidhive.info/wp-content/uploads/2015/04/androd-material-design-navigation-drawer.png)

androd-material-design-navigation-drawer

**3.3** 实现导航抽屉的选择事件
尽管导航抽屉成功运行了，但是你看到菜单的点击事件没有正常运行，这是因为我们也需要实现RecyclerView的click监听事件

因为我们有三个菜单项（Home, Friends & Messages）在导航抽屉中，因此我们需要创建三个独立的fragment类为每一个菜单

**24.** 在res下面，新建一个xml文件叫做**fragment_home.xml**并添加以下代码

`fragment_home.xml`
``` xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="info.androidhive.materialdesign.activity.HomeFragment">
 
 
    <TextView
        android:id="@+id/label"
        android:layout_alignParentTop="true"
        android:layout_marginTop="100dp"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:gravity="center_horizontal"
        android:textSize="45dp"
        android:text="HOME"
        android:textStyle="bold"/>
 
    <TextView
        android:layout_below="@id/label"
        android:layout_centerInParent="true"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:textSize="12dp"
        android:layout_marginTop="10dp"
        android:gravity="center_horizontal"
        android:text="Edit fragment_home.xml to change the appearance" />
 
</RelativeLayout>
```

**25.** 在activity包下，新建一个fragment类，叫做**HomeFragment.java**并且添加以下代码

`HomeFragment.java`
``` java
import android.app.Activity;
import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
 
 
public class HomeFragment extends Fragment {
 
    public HomeFragment() {
        // Required empty public constructor
    }
 
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
 
    }
 
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        View rootView = inflater.inflate(R.layout.fragment_home, container, false);
 
 
        // Inflate the layout for this fragment
        return rootView;
    }
 
    @Override
    public void onAttach(Activity activity) {
        super.onAttach(activity);
    }
 
    @Override
    public void onDetach() {
        super.onDetach();
    }
}
```

**26.** 新建两个fragment类分别叫做：**FriendsFragment.java**、**MessagesFragment.java**，同样新建两个xml：**fragment_friends.xml**、**fragment_messages.xml**，按照上面步骤添加代码

**27.** 现在打开**MainActivity.java**，做以下修改

- _displayView()_方法显示fragment，这个方法在**onDrawerItemSelected()**中被调用，当菜单被选择的时候，来渲染对应的布局

`MainActivity.java`
``` java
import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.support.v4.app.FragmentManager;
import android.support.v4.app.FragmentTransaction;
import android.support.v4.widget.DrawerLayout;
import android.support.v7.app.ActionBarActivity;
import android.support.v7.widget.Toolbar;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.widget.Toast;
 
public class MainActivity extends ActionBarActivity implements FragmentDrawer.FragmentDrawerListener {
 
    private static String TAG = MainActivity.class.getSimpleName();
 
    private Toolbar mToolbar;
    private FragmentDrawer drawerFragment;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        mToolbar = (Toolbar) findViewById(R.id.toolbar);
 
        setSupportActionBar(mToolbar);
        getSupportActionBar().setDisplayShowHomeEnabled(true);
 
        drawerFragment = (FragmentDrawer)
                getSupportFragmentManager().findFragmentById(R.id.fragment_navigation_drawer);
        drawerFragment.setUp(R.id.fragment_navigation_drawer, (DrawerLayout) findViewById(R.id.drawer_layout), mToolbar);
        drawerFragment.setDrawerListener(this);
 
        // display the first navigation drawer view on app launch
        displayView(0);
    }
 
 
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }
 
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();
 
        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }
 
        if(id == R.id.action_search){
            Toast.makeText(getApplicationContext(), "Search action is selected!", Toast.LENGTH_SHORT).show();
            return true;
        }
 
        return super.onOptionsItemSelected(item);
    }
 
    @Override
    public void onDrawerItemSelected(View view, int position) {
            displayView(position);
    }
 
    private void displayView(int position) {
        Fragment fragment = null;
        String title = getString(R.string.app_name);
        switch (position) {
            case 0:
                fragment = new HomeFragment();
                title = getString(R.string.title_home);
                break;
            case 1:
                fragment = new FriendsFragment();
                title = getString(R.string.title_friends);
                break;
            case 2:
                fragment = new MessagesFragment();
                title = getString(R.string.title_messages);
                break;
            default:
                break;
        }
 
        if (fragment != null) {
            FragmentManager fragmentManager = getSupportFragmentManager();
            FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
            fragmentTransaction.replace(R.id.container_body, fragment);
            fragmentTransaction.commit();
 
            // set the toolbar title
            getSupportActionBar().setTitle(title);
        }
    }
}
```

现在再来运行你的应用，你能够看到导航抽屉菜单的选择事件可以正常实现，并且对应的布局显示在toolbar下面


![android-material-design-navigation-drawer-1](http://www.androidhive.info/wp-content/uploads/2015/04/android-material-design-navigation-drawer-1.png)

![android-material-design-navigation-drawer-2](http://www.androidhive.info/wp-content/uploads/2015/04/android-material-design-navigation-drawer-2.png)

![android-material-design-navigation-drawer-3](http://www.androidhive.info/wp-content/uploads/2015/04/android-material-design-navigation-drawer-3.png)



[1]: http://developer.android.com/intl/zh-tw/tools/studio/index.html
[2]: http://api.androidhive.info/images/ic_action_search.png
[3]: http://api.androidhive.info/images/ic_profile.png
