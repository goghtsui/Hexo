title: Android Material Design - Tabs
date: 2015-12-28 11:51:06
categories: [Android]
tags: [Material Design, TabLayout]
---

> 原作者：Ravi Tamada
> 原文地址：[http://www.androidhive.info/2015/09/android-material-design-working-with-tabs/](http://www.androidhive.info/2015/09/android-material-design-working-with-tabs/)


[Android Design支持库][1] 提供了很好的向后兼容性，在Material Design支持库中的组件中，像Navigation Drawer, FloatingAction Button, Snackbar, Tabs, Floating labels ， animation frameworks。在这里我们将学习怎样实现tabs。

在进一步深入了解之前，我建议先看一下[tabs的文档][2]，它可以告诉你在实现tabs的时候，什么该做什么不该做。

这里还有yutube的视频：[到墙外看一看][3]

### 使用Material
首先我们创建一个新的项目并且应用Material主题，如果你不知道Material Design，那么可以看看文章[Material Design入门][4]

**1**.在Android Studio中，**File => New Project**并且填好其它信息去创建一个新项目。

**2**.打开**build.gradle**然后添加支持库**com.android.support:design:23.0.1**

`build.gradle`
``` xml
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:23.0.1'
    compile 'com.android.support:design:23.0.1'
}
```

**3**.打开位于**res => values**下的**colors.xml**，并且添加以下颜色值：

`colors.xml`
``` xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="colorPrimary">#125688</color>
    <color name="colorPrimaryDark">#125688</color>
    <color name="textColorPrimary">#FFFFFF</color>
    <color name="windowBackground">#FFFFFF</color>
    <color name="navigationBarColor">#000000</color>
    <color name="colorAccent">#c8e8ff</color>
</resources>
```

**4**.在**res => values**下的**dimens.xml**添加以下代码：

`dimens.xml`
``` xml
<resources>
    <!-- Default screen margins, per the Android Design guidelines. -->
    <dimen name="activity_horizontal_margin">16dp</dimen>
    <dimen name="activity_vertical_margin">16dp</dimen>
    <dimen name="tab_max_width">264dp</dimen>
    <dimen name="tab_padding_bottom">16dp</dimen>
    <dimen name="tab_label">14sp</dimen>
    <dimen name="custom_tab_layout_height">72dp</dimen>
</resources>
```

**5**. 打开**res ⇒ values**下的**styles.xml**，并添加以下主题。在**styles.xml**中这个主题是通用于所有安卓版本的。

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

**6.** 在**res**下面创建**values-v21**文件夹，然后创建另外一个**styles.xml**，写入以下主题，这个主题是适用于Android 5.0的。

`styles.xml`
``` xml
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

**7.** 最后打开**AndroidManifest.xml**并且修改**android:theme**属性为我们自定义的主题。

`AndroidManifest.xml`
``` xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="info.androidhive.materialtabs" >
 
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

运行这个应用，通过通知栏的颜色来验证一下Material Design主题。如果你看到通知栏颜色改变了，这就意味着Material Design主题已经被成功使用。

图例

现在我们已经有了Material Design主题的应用，接下来让我们开始添加tabs。但是在这之前。我们需要创建一些fragment来协助测试，所有这些fragment只有非常简单的UI，一个TextView而已。

**8.** 在你的包目录下，创建一个fragment，命名为**OneFragment.java**并添加以下代码：

`OneFragment.java`
``` java
package info.androidhive.materialtabs.fragments;
 
import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
 
import info.androidhive.materialtabs.R;
 
 
public class OneFragment extends Fragment{
 
    public OneFragment() {
        // Required empty public constructor
    }
 
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }
 
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_one, container, false);
    }
 
}
```

**9.** 在**res ⇒ layout**下添加**fragment_one.xml**，写入以下代码：

`fragment_one.xml`
``` xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="info.androidhive.materialtabs.fragments.OneFragment">
 
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/one"
        android:textSize="40dp"
        android:textStyle="bold"
        android:layout_centerInParent="true"/>
 
</RelativeLayout>
```

**10.** 同样的，创建一些其它的fragment，并且写入像**OneFragment.java**一样的代码，我已经创建好了**TwoFragment.java**, **ThreeFragment.java**, **FourFragemnt.java**一直到**TenFragment.java**


### 固定标签

当tabs是固定数目的时候，你可以使用这个方式。这些tabs固定在适当的位置。在design支持库中，一些新的元素像**CoordinatorLayout**、**AppBarLayout**、**TabLayout**等介绍了很多。我覆盖不到所有的情况，因为这不是本文的目的。

**11**. 打开布局文件**activity_main.xml**并修改为一下代码：

`app:tabMode` – 定义tab的形式，在这我们定义为**fixed**

`activity_main.xml`
``` xml
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
 
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
 
        <android.support.design.widget.TabLayout
            android:id="@+id/tabs"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:tabMode="fixed"
            app:tabGravity="fill"/>
    </android.support.design.widget.AppBarLayout>
 
    <android.support.v4.view.ViewPager
        android:id="@+id/viewpager"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior"  />
</android.support.design.widget.CoordinatorLayout>
```

**12.** 打开**MainActivity.java** 并作以下修改：

`tabLayout.setupWithViewPager()` – 适配ViewPager给TabLayout

`setupViewPager()` – 通过添加适当的fragment来设置tabs的数量和tab的名字

`ViewPagerAdapter` – 自定义适配器类提供了ViewPager需要的额fragment

`MainActivity.java`

``` java
package info.androidhive.materialtabs.activity;
 
import android.os.Bundle;
import android.support.design.widget.TabLayout;
import android.support.v4.app.Fragment;
import android.support.v4.app.FragmentManager;
import android.support.v4.app.FragmentPagerAdapter;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
 
import java.util.ArrayList;
import java.util.List;
 
import info.androidhive.materialtabs.R;
import info.androidhive.materialtabs.fragments.OneFragment;
import info.androidhive.materialtabs.fragments.ThreeFragment;
import info.androidhive.materialtabs.fragments.TwoFragment;
 
public class MainActivity extends AppCompatActivity {
 
    private Toolbar toolbar;
    private TabLayout tabLayout;
    private ViewPager viewPager;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
 
        getSupportActionBar().setDisplayHomeAsUpEnabled(true);
 
        viewPager = (ViewPager) findViewById(R.id.viewpager);
        setupViewPager(viewPager);
 
        tabLayout = (TabLayout) findViewById(R.id.tabs);
        tabLayout.setupWithViewPager(viewPager);
    }
 
    private void setupViewPager(ViewPager viewPager) {
        ViewPagerAdapter adapter = new ViewPagerAdapter(getSupportFragmentManager());
        adapter.addFragment(new OneFragment(), "ONE");
        adapter.addFragment(new TwoFragment(), "TWO");
        adapter.addFragment(new ThreeFragment(), "THREE");
        viewPager.setAdapter(adapter);
    }
 
    class ViewPagerAdapter extends FragmentPagerAdapter {
        private final List<Fragment> mFragmentList = new ArrayList<>();
        private final List<String> mFragmentTitleList = new ArrayList<>();
 
        public ViewPagerAdapter(FragmentManager manager) {
            super(manager);
        }
 
        @Override
        public Fragment getItem(int position) {
            return mFragmentList.get(position);
        }
 
        @Override
        public int getCount() {
            return mFragmentList.size();
        }
 
        public void addFragment(Fragment fragment, String title) {
            mFragmentList.add(fragment);
            mFragmentTitleList.add(title);
        }
 
        @Override
        public CharSequence getPageTitle(int position) {
            return mFragmentTitleList.get(position);
        }
    }
}
```

现在运行这个应用，你应该能看到tabs已经显示，并且能够通过滑动在他们之间切换。

![swipe](http://www.androidhive.info/wp-content/uploads/2015/09/android-material-design-tab-layout.png)


**2.1** 屏幕宽度的标签
如果你想要标签栏占据整个屏幕的宽度，你需要给TabLayout设置 **app:tabGravity="fill"**属性。

![full](http://www.androidhive.info/wp-content/uploads/2015/09/android-tab-layout-landscape-view.png)


**2.2** 中心对齐的标签
如果你想要你的标签按照水平居中的形式来显示，你需要给TabLayout设置**app:tabGravity="center"**的属性。

![full](http://www.androidhive.info/wp-content/uploads/2015/09/android-tab-layout-gravity-center.png)


### 滚动标签

当你有很多标签时，并且一个屏幕的空间放不下的时候，你可以使用滑动标签。标签可以滚动，给TabLayout设置**app:tabMode="scrollable"**即可。

**13.** 打开**activity_main.xml**并且修改**app:tabMode**为**scrollable**。

``` xml
<android.support.design.widget.TabLayout
            android:id="@+id/tabs"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:tabMode="scrollable"/>
```

**14.** 编辑**MainActivity.java**并且添加一些fragment到ViewPager通过**setupViewPager()**方法。我已经添加了10个fragment，这样做之后，你的MainActivity应该像下面这样。

`MainActivity.java`

``` java
package info.androidhive.materialtabs.activity;
 
import android.os.Bundle;
import android.support.design.widget.TabLayout;
import android.support.v4.app.Fragment;
import android.support.v4.app.FragmentManager;
import android.support.v4.app.FragmentPagerAdapter;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
 
import java.util.ArrayList;
import java.util.List;
 
public class MainActivity extends AppCompatActivity {
 
    private Toolbar toolbar;
    private TabLayout tabLayout;
    private ViewPager viewPager;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        getSupportActionBar().setDisplayHomeAsUpEnabled(true);
 
        viewPager = (ViewPager) findViewById(R.id.viewpager);
        setupViewPager(viewPager);
 
        tabLayout = (TabLayout) findViewById(R.id.tabs);
        tabLayout.setupWithViewPager(viewPager);
    }
 
    private void setupViewPager(ViewPager viewPager) {
        ViewPagerAdapter adapter = new ViewPagerAdapter(getSupportFragmentManager());
        adapter.addFrag(new OneFragment(), "ONE");
        adapter.addFrag(new TwoFragment(), "TWO");
        adapter.addFrag(new ThreeFragment(), "THREE");
        adapter.addFrag(new FourFragment(), "FOUR");
        adapter.addFrag(new FiveFragment(), "FIVE");
        adapter.addFrag(new SixFragment(), "SIX");
        adapter.addFrag(new SevenFragment(), "SEVEN");
        adapter.addFrag(new EightFragment(), "EIGHT");
        adapter.addFrag(new NineFragment(), "NINE");
        adapter.addFrag(new TenFragment(), "TEN");
        viewPager.setAdapter(adapter);
    }
 
    class ViewPagerAdapter extends FragmentPagerAdapter {
        private final List<Fragment> mFragmentList = new ArrayList<>();
        private final List<String> mFragmentTitleList = new ArrayList<>();
 
        public ViewPagerAdapter(FragmentManager manager) {
            super(manager);
        }
 
        @Override
        public Fragment getItem(int position) {
            return mFragmentList.get(position);
        }
 
        @Override
        public int getCount() {
            return mFragmentList.size();
        }
 
        public void addFrag(Fragment fragment, String title) {
            mFragmentList.add(fragment);
            mFragmentTitleList.add(title);
        }
 
        @Override
        public CharSequence getPageTitle(int position) {
            return mFragmentTitleList.get(position);
        }
    }
}
```

现在如果你再运行你的应用，你就可以看到很多tabs，而且具有欢动功能。 

![layout](http://www.androidhive.info/wp-content/uploads/2015/09/android-scrollable-tab-layout.png)
![horizontal](http://www.androidhive.info/wp-content/uploads/2015/09/android-scrollable-tab-layout-horizontal.png)


### 图文标签

有时候你可能想要给标签添加图标。此前给标签添加图标是非常繁琐的过程，但是现在有了Material Design支持库，这就变得很容易了。所以你必须要做的就是调用**setIcon()**方法来设置适当的图标，这个图标就可以显示在标签文字前面了。

``` java
tabLayout.getTabAt(0).setIcon(tabIcons[0]);
tabLayout.getTabAt(1).setIcon(tabIcons[1]);
```

**15.** 打开**MainActivity.java**并且按照下面来修改代码。在这里我已经添加了一个新的方法**setupTabIcons()**来设置标签的图标。

`MainActivity.java`
``` java
import android.os.Bundle;
import android.support.design.widget.TabLayout;
import android.support.v4.app.Fragment;
import android.support.v4.app.FragmentManager;
import android.support.v4.app.FragmentPagerAdapter;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
 
import java.util.ArrayList;
import java.util.List;
 
public class MainActivity extends AppCompatActivity {
 
    private Toolbar toolbar;
    private TabLayout tabLayout;
    private ViewPager viewPager;
    private int[] tabIcons = {
            R.drawable.ic_tab_favourite,
            R.drawable.ic_tab_call,
            R.drawable.ic_tab_contacts
    };
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        getSupportActionBar().setDisplayHomeAsUpEnabled(true);
 
        viewPager = (ViewPager) findViewById(R.id.viewpager);
        setupViewPager(viewPager);
 
        tabLayout = (TabLayout) findViewById(R.id.tabs);
        tabLayout.setupWithViewPager(viewPager);
        setupTabIcons();
    }
 
    private void setupTabIcons() {
        tabLayout.getTabAt(0).setIcon(tabIcons[0]);
        tabLayout.getTabAt(1).setIcon(tabIcons[1]);
        tabLayout.getTabAt(2).setIcon(tabIcons[2]);
    }
 
    private void setupViewPager(ViewPager viewPager) {
        ViewPagerAdapter adapter = new ViewPagerAdapter(getSupportFragmentManager());
        adapter.addFrag(new OneFragment(), "ONE");
        adapter.addFrag(new TwoFragment(), "TWO");
        adapter.addFrag(new ThreeFragment(), "THREE");
        viewPager.setAdapter(adapter);
    }
 
    class ViewPagerAdapter extends FragmentPagerAdapter {
        private final List<Fragment> mFragmentList = new ArrayList<>();
        private final List<String> mFragmentTitleList = new ArrayList<>();
 
        public ViewPagerAdapter(FragmentManager manager) {
            super(manager);
        }
 
        @Override
        public Fragment getItem(int position) {
            return mFragmentList.get(position);
        }
 
        @Override
        public int getCount() {
            return mFragmentList.size();
        }
 
        public void addFrag(Fragment fragment, String title) {
            mFragmentList.add(fragment);
            mFragmentTitleList.add(title);
        }
 
        @Override
        public CharSequence getPageTitle(int position) {
            return mFragmentTitleList.get(position);
        }
    }
}
```

![android-tab-layout-with-icon-and-text](http://www.androidhive.info/wp-content/uploads/2015/09/android-tab-layout-with-icon-and-text.png)


### 图标标签

仅仅设置图标的选项卡是和图文的选项卡是一样的，只是ViewPagerAdapter类的**getPageTitle()**方法返回值为null即可。

**16.** 打开**MainActivity.java**并按照下面的代码修改**getPageTitle()**方法，接着运行项目。

``` java

@Override
public CharSequence getPageTitle(int position) {
 
    // return null to display only the icon
    return null;
}
```

![android-tab-layout-with-only-icons](http://www.androidhive.info/wp-content/uploads/2015/09/android-tab-layout-with-only-icons.png)


### 自定义图文选项卡

当默认的选项卡布局不能达到你预期的输出效果时，自定义选项卡就非常实用了。在你自定义选项卡试图的时候，请务必遵循Android选项卡的规范建议。

当我们设置了图文选项卡时，你能够看到图标和文字是水平居中的，但是如果你想要放置图标在选项卡标签之上，你就必须使用一个自定义的view来完成它。

**17.** 在**res ⇒ values**下面,创建**fonts.xml**文件，并且添加以下字符串值。这个xml文件定义了选项卡中文字所使用的字体。

`fonts.xml`

``` xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="font_fontFamily_medium">sans-serif</string>
</resources>
```
**18.** 在**res ⇒ values-v21下面，创建另一个**fonts.xml**文件。

`fonts.xml`

``` xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="font_fontFamily_medium">sans-serif-medium</string>
</resources>
```

**19.** 打开**activity_main.xml**并且给TabLayout设置自定义的高度。设置这个高度显得非常重要，因为在选项卡标签上面放置图标需要更多的空间。

``` xml

<android.support.design.widget.TabLayout
           android:id="@+id/tabs"
           android:layout_width="match_parent"
           android:layout_height="@dimen/custom_tab_layout_height"
           app:tabMode="fixed"
           app:tabGravity="fill"/>
```

**20.** 在**res ⇒ layout**下新建的一个xml文件**custom_tab.xml**，这个文件是用来自定义选项卡布局的。

`custom_tab.xml`

``` xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:id="@+id/tab"
    android:textColor="@color/colorAccent"
    android:textSize="@dimen/tab_label"
    android:fontFamily="@string/font_fontFamily_medium"/>
```

**21.** 打开**MainActivity.java**并按照下面的代码做修改。这里你是否注意到setupTabIcon()方法。下面的代码中我已经给每一个选项卡使用了custom_tab.xml布局，。

``` java
TextView tabOne = (TextView) LayoutInflater.from(this).inflate(R.layout.custom_tab, null);
tabOne.setText("ONE");
tabOne.setCompoundDrawablesWithIntrinsicBounds(0, R.drawable.ic_tab_favourite, 0, 0);
tabLayout.getTabAt(0).setCustomView(tabOne);
```

`MainActivity.java`

``` java
import android.os.Bundle;
import android.support.design.widget.TabLayout;
import android.support.v4.app.Fragment;
import android.support.v4.app.FragmentManager;
import android.support.v4.app.FragmentPagerAdapter;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.view.LayoutInflater;
import android.widget.TextView;
 
import java.util.ArrayList;
import java.util.List;
 
import info.androidhive.materialtabs.R;
import info.androidhive.materialtabs.fragments.OneFragment;
import info.androidhive.materialtabs.fragments.ThreeFragment;
import info.androidhive.materialtabs.fragments.TwoFragment;
 
public class MainActivity extends AppCompatActivity {
 
    private Toolbar toolbar;
    private TabLayout tabLayout;
    private ViewPager viewPager;
    private int[] tabIcons = {
            R.drawable.ic_tab_favourite,
            R.drawable.ic_tab_call,
            R.drawable.ic_tab_contacts
    };
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        getSupportActionBar().setDisplayHomeAsUpEnabled(true);
 
        viewPager = (ViewPager) findViewById(R.id.viewpager);
        setupViewPager(viewPager);
 
        tabLayout = (TabLayout) findViewById(R.id.tabs);
        tabLayout.setupWithViewPager(viewPager);
        setupTabIcons();
    }
 
    private void setupTabIcons() {
 
        TextView tabOne = (TextView) LayoutInflater.from(this).inflate(R.layout.custom_tab, null);
        tabOne.setText("ONE");
        tabOne.setCompoundDrawablesWithIntrinsicBounds(0, R.drawable.ic_tab_favourite, 0, 0);
        tabLayout.getTabAt(0).setCustomView(tabOne);
 
        TextView tabTwo = (TextView) LayoutInflater.from(this).inflate(R.layout.custom_tab, null);
        tabTwo.setText("TWO");
        tabTwo.setCompoundDrawablesWithIntrinsicBounds(0, R.drawable.ic_tab_call, 0, 0);
        tabLayout.getTabAt(1).setCustomView(tabTwo);
 
        TextView tabThree = (TextView) LayoutInflater.from(this).inflate(R.layout.custom_tab, null);
        tabThree.setText("THREE");
        tabThree.setCompoundDrawablesWithIntrinsicBounds(0, R.drawable.ic_tab_contacts, 0, 0);
        tabLayout.getTabAt(2).setCustomView(tabThree);
    }
 
    private void setupViewPager(ViewPager viewPager) {
        ViewPagerAdapter adapter = new ViewPagerAdapter(getSupportFragmentManager());
        adapter.addFrag(new OneFragment(), "ONE");
        adapter.addFrag(new TwoFragment(), "TWO");
        adapter.addFrag(new ThreeFragment(), "THREE");
        viewPager.setAdapter(adapter);
    }
 
    class ViewPagerAdapter extends FragmentPagerAdapter {
        private final List<Fragment> mFragmentList = new ArrayList<>();
        private final List<String> mFragmentTitleList = new ArrayList<>();
 
        public ViewPagerAdapter(FragmentManager manager) {
            super(manager);
        }
 
        @Override
        public Fragment getItem(int position) {
            return mFragmentList.get(position);
        }
 
        @Override
        public int getCount() {
            return mFragmentList.size();
        }
 
        public void addFrag(Fragment fragment, String title) {
            mFragmentList.add(fragment);
            mFragmentTitleList.add(title);
        }
 
        @Override
        public CharSequence getPageTitle(int position) {
            return mFragmentTitleList.get(position);
        }
    }
}
```

现在如果你运行这个程序，你就能看到图标被放置在选项卡标签的上面了。

![android-tab-layout-with-custom-view-icon-and-text](http://www.androidhive.info/wp-content/uploads/2015/09/android-tab-layout-with-custom-view-icon-and-text.png)

我希望这篇文章对于Material Design支持库的使用，提供了一些非常有用的信息，如果你有任何问题，请在下面回复。



















[1]: http://android-developers.blogspot.in/2015/05/android-design-support-library.html
[2]: https://www.google.co.in/design/spec/components/tabs.html#
[3]: https://youtu.be/6sGhDYYUoBM
[4]: http://www.androidhive.info/2015/04/android-getting-started-with-material-design/