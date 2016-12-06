title: Android开发之[暗码]
date: 2016-10-27 11:18:53
categories: [Android]
tags: [SECRET_CODE, SECRET_CODE_ACTION]
---

### 序言

什么是暗码？不同厂商的手机都会隐藏代码，用来查看系统及固件版本，或者进行硬件的测试，当然Android手机也不例外，除了好像计算机一样能显示更详细的手机信息外，更可重设为原厂设定，更新相机等。但部份代码要谨慎使用，因为可能令手机失去原有的功能，确认在了解其功能的前提下再去尝试，具体的有哪些暗码大家自行搜索吧。

暗码怎么使用呢？在手机拨号页面，输入:** \*#\*#munber#\*#\* **,number对应的就是暗码了。我这里就说一个我经常用到的暗码，以MOTO Z play为例：__\*#\*#4636#\*#\*__，显示手机信息、使用情况统计数据、WiFi information、CMAS测试提示、用户首选网络列表、IMS Setting，其中短信中心的号码设置就在这里设置的。那么这个暗码的功能，最为开发者的我们能不能使用呢？答案是绝对可以的，话说你可以给自己的应用留一些后门或者debug开关，亦或者打开特定的页面，功能还是很屌的，下面进入正题吧
<!-- more -->

### 功能描述

首先，这个暗码的捕获及解析是系统来完成的，系统在解析完成暗码之后会发送一个广播出来,系统源码：

**\android\packages\apps\Dialer\src\com\android\dialer\SpecialCharSequenceMgr.java**

``` java

/**
* Handles secret codes to launch arbitrary activities in the form of *#*#<code>#*#*.
* If a secret code is encountered an Intent is started with the android_secret_code://<code>
* URI.
*
* @param context the context to use
* @param input the text to check for a secret code in
* @return true if a secret code was encountered
*/
static boolean handleSecretCode(Context context, String input) {
    // Secret codes are in the form *#*#<code>#*#*
    int len = input.length();
    if (len > 8 && input.startsWith("*#*#") && input.endsWith("#*#*")) {
        final Intent intent = new Intent(SECRET_CODE_ACTION,
                Uri.parse("android_secret_code://" + input.substring(4, len - 4)));
        context.sendBroadcast(intent);
        return true;
    }

    return false;
}

```
也可以看出来 SECRET_CODE_ACTION 也是从这里发出来的，所以我们需要做的就是创建一个receiver来接受该广播。这样，我们就可以通过intent拿到这个暗码，与我们设定的暗码匹配比较，继而完成相关的业务逻辑。

#### 使用暗码

1. 自定义的receiver：

``` java

public class SecretReceiver extends BroadcastReceiver {

    private static final String ACTION = "android.provider.Telephony.SECRET_CODE";
    private static final String SECRET_CODE = "2016";

    @Override
    public void onReceive(Context context, Intent intent) {
        if(intent.getAction().equals(ACTION)){
            if(intent.getData().getHost().equals(SECRET_CODE)){
                Intent target = new Intent(context, MainActivity.class);
                target.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
                context.startActivity(target);
            }
        }
    }
}

```
非常简单的一段代码，就是接收到暗码的广播，启动一个页面

2. 在AndroidManifest.xml中注册该receiver，并设置识别的暗码：

``` xml
<receiver android:name=".SecretReceiver">
    <intent-filter>
        <action android:name="android.provider.Telephony.SECRET_CODE" />
            <data android:scheme="android_secret_code" android:host="2016" />
    </intent-filter>
</receiver>
```
只需要这两步，一个简单的暗码启动页面就完成了，亲测可行。

#### 执行暗码
上面的方法我们已经可以识别自定义的暗码了，那么如何执行暗码呢？其实还是交给系统去执行的，和拨打电话是一个原理：

``` java

Uri uri = Uri.parse("android_secret_code://" + code);
Intent intent = new Intent(ACTION, uri);
context.sendBroadcast(intent);

```

### 总结
可能会有童鞋问道：那我定义一个和系统一样的暗码，会怎么样？告诉你，就是：没你什么事。  系统暗码系统捕获并处理，不会告诉你的，你想多了，不然系统给你搞坏了咋办，哈。。
其实这个功能研发本身很简单，但是如果利用好这个功能，还是可以做出一些很特别的东西的，大不了可以装逼用啊。 ToT


