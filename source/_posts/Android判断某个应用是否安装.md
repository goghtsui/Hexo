title: Android判断某个应用是否安装
date: 2016-12-14 18:36:40
categories: [Android]
tags: [GET_INTENT_FILTERS, MATCH_DEFAULT_ONLY]
---

## 序言

在应用开过过程中有这样一个场景：判断某个应用是否已经安装了。你一定会说这个有什么难的，是的，这个问题很简单，不为别的，就为记个笔记，大牛勿喷

## 代码篇

### 包名检查

``` java
public static boolean isInstalled(Context context, String packageName) {
        try {
            PackageInfo packageInfo = context.getPackageManager().getPackageInfo(packageName.trim()
                    , PackageManager.COMPONENT_ENABLED_STATE_DEFAULT);
            if (packageInfo != null) {
                // 说明某个应用使用了该包名
                return true;
            }
        } catch (PackageManager.NameNotFoundException e) {
            e.printStackTrace();
            return false;
        }

        return false;
}
```

### ACTION检查

- 方案一

``` java
 public static boolean isInstalledByAction(Context context, String action) {
        final PackageManager packageManager = context.getPackageManager();
        final Intent intent = new Intent(action);
        List<ResolveInfo> list = packageManager.queryIntentActivities(intent,
                PackageManager.MATCH_DEFAULT_ONLY);
        if(list.size() > 0){
            // 说明某个应用的activty使用了该action
            return true;
        }
        return false;
    }
```

- 方案二

``` java
  public static boolean isInstallByAction(Context context, String action) {
          PackageManager packageManager = context.getPackageManager();
          Intent intent = new Intent(action);
          List<ResolveInfo> resolveInfo = packageManager
                  .queryIntentActivities(intent, PackageManager.GET_INTENT_FILTERS);// AS这里报错，但是不影响编译
          if (resolveInfo.size() == 0) {
              // 说明没有任何应用使用该action
              return false;
          }

          // 说明有某个的某个应用的activity使用了该action
          return true;
   }
```

## 总结

这三种方案，话说我使用的最多的就是包名，action的从来没注意过，但是开发过程中遇到了，还是记个笔记吧。

我们都知道action是配置在AndroidManifest.xml中组件的IntentFilter属性里的，所以这些方法最终校验的都是该属性。