title: Android之应用是否有启动页面(具有CATEGORY_LAUNCHER属性)
date: 2016-12-14 19:24:34
categories: [Android]
tags: [CATEGORY_LAUNCHER]
---

## 序言

最近遇到一个问题，就是判断这个应用是否具有启动页面，即是不是插件或者服务类应用，不需要展示页面的应用。相信开发过Launcher应用的小伙伴肯定知道这个问题怎么解决。很简单，都是细节问题，稍加注意即可，大牛还请绕路。

## 实战篇

获取所有安装的应用使用这个方法：

``` java
public void getInstalledApps(Context context) {
        PackageManager packageManager = context.getPackageManager();
        List<PackageInfo> list = packageManager.getInstalledPackages(packageManager.GET_ACTIVITIES);
        for (PackageInfo p : list) {

            AppInfoEntity infoEntity = new AppInfoEntity();
            infoEntity.setAppName(packageManager.getApplicationLabel(p.applicationInfo).toString());
            infoEntity.setAppIcon(p.applicationInfo.loadIcon(packageManager));
            infoEntity.setAppPkgName(p.applicationInfo.packageName);
            infoEntity.setApkPath(p.applicationInfo.sourceDir);
            File file = new File(p.applicationInfo.sourceDir);
            infoEntity.setAppSize((int) file.length());
            int flags = p.applicationInfo.flags;
            
            if ((flags & ApplicationInfo.FLAG_SYSTEM) > 0) {
                // 系统应用
            } else {
                // 安装应用
            }
        }
    }
```

<!-- more -->

没错你获取到是所有系统中存在的应用，包括系统和用户安装的，但是恶心的是，所有的插件类，或者系统的服务应用等都一起返回了，但是我们并不需要将这些应用展示给用户，因为他们不是用来与用户交互的，那我们需要自己过滤吗？答案肯定不是的，只是我们获取的方式不对，继续往下看，换个姿势试试：

``` java
public List<AppInfoEntity> getInstalledApps(Context context) {
        Intent mainIntent = new Intent(Intent.ACTION_MAIN, null);
        mainIntent.addCategory(Intent.CATEGORY_LAUNCHER);

        PackageManager packageManager = context.getPackageManager();
        List<ResolveInfo> allApps = packageManager.queryIntentActivities(mainIntent, 0);
        for (int i = 0; i < allApps.size(); i++) {
            AppInfoEntity infoEntity = new AppInfoEntity();
            ResolveInfo info = allApps.get(i);
            infoEntity.setAppIcon(info.activityInfo.loadIcon(packageManager));
            infoEntity.setAppName(packageManager.getApplicationLabel(info.activityInfo.applicationInfo).toString());
            ENTITYLIST.add(infoEntity);
        }
  
        return ENTITYLIST;
}
```

很明显可以看出，这种方式是由针对性的，通过构建一个具有 **Intent.CATEGORY_LAUNCHER** 属性的intent，即有启动页面的应用，所以以这种方式筛选出来的应用就是我们可以展示给用户的应用了，包括系统的和用户安装的

## 总结

小功能点，平时不太注意，没有什么多说的，其实多看看api就都明白了，没有做不到的，只有想不到的。

