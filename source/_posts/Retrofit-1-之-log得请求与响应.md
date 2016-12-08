title: Retrofit 1 之 log得请求与响应
date: 2016-12-08 17:38:25
categories: [Android]
tags: [Retrofit]
---

## 序言

在Retrofit使用中，或者说在项目开发过程中，调试是必须的一中技能和方式，其中就包括日志的形式，那么在Retrofit的使用中，应该以怎样的姿态使用日志功能呢？接下来就让我们一起去看看吧。

## Retrofit 1

默认情况下Retrofit 1是没有启用日志功能的，但是日志功能的开启和使用非常容易，请看代码：

``` java
RestAdapter.Builder builder = new RestAdapter.Builder()  
    .setEndpoint(API_LOCATION)
    .setLogLevel(RestAdapter.LogLevel.FULL) // this is the important line
    .setClient(new OkClient(new OkHttpClient()));
```



如您所见，日志包括整个请求和响应正文。虽然这可能是有用的和必要的，但信息可能太多，反而影响了日志的可读性及应用的性能。

### 日志等级

- NONE

  没有任何日志，而且会提升你的应用性能。

- BASIC

  仅记录请求方法和URL以及响应状态代码和执行时间。

  ![basic](https://futurestud.io/blog/content/images/2015/02/Screen-Shot-2015-02-08-at-4-58-32-PM.png)

- HEADERS

  记录基本信息以及请求和响应头

  ![headers](https://futurestud.io/blog/content/images/2015/02/Screen-Shot-2015-02-08-at-4-59-50-PM.png)

- HEADERS_AND_ARGS

  记录基本信息以及请求和响应对象的toString()信息。

  ![headers_and_args](https://futurestud.io/blog/content/images/2015/02/Screen-Shot-2015-02-08-at-5-02-29-PM.png)

- FULL

  记录请求和响应的头，主体和元数据。

## Retrofit 2

请看另外一篇博客：[Retrofit 2 之 log得请求与响应](http://xiaofeng.site/2016/12/08/Retrofit-2-%E4%B9%8B-log%E5%BE%97%E8%AF%B7%E6%B1%82%E4%B8%8E%E5%93%8D%E5%BA%94/undefined/)

## 总结

在没有必要的情况下，还是不要开启日志功能，因为这本身是一个消耗性能的事情，当然，合理的使用日志功能，是完善应用的一个很好的途径。



