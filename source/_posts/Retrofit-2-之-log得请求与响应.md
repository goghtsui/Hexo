title: Retrofit 2 之 log得请求与响应
date: 2016-12-08 17:09:37
categories: [Android]
tags: [Retrofit]
---

## 序言

Retrofit 1集成了用于基本请求和响应调试的日志功能，但是在Retrofit 2中被移除了，因为所需的HTTP层现在完全基于OkHttp。由于许多开发人员要求在Retrofit 2中提供日志记录功能，OkHttp的开发人员在2.6.0版本中添加了一个日志拦截器，接下来，你将看到怎样添加和使用日志拦截器。

## Retrofit 1

请看另外一篇博客：[Retrofit 1 之 log得请求与响应](http://xiaofeng.site/2016/12/08/Retrofit-1-%E4%B9%8B-log%E5%BE%97%E8%AF%B7%E6%B1%82%E4%B8%8E%E5%93%8D%E5%BA%94/undefined/)

## Retrofit 2

Retrofit 2完全依赖于OkHttp进行任何网络操作。OkHttp的开发者已经发布了一个日志拦截器集成的版本，你可以通过以下方式添加依赖：
<!-- more -->

``` xml
compile 'com.squareup.okhttp3:logging-interceptor:3.3.1'  
```

在OkHttp中，默认是没有拦截日志的，我们需要添加日志拦截器给OkHttp，并且已经提供了日志拦截器，我们只需要激活它给OkHttpClient：

``` java
HttpLoggingInterceptor logging = new HttpLoggingInterceptor();  
// set your desired log level
logging.setLevel(Level.BODY);

OkHttpClient.Builder httpClient = new OkHttpClient.Builder();  
// add your other interceptors …

// add logging as last interceptor
httpClient.addInterceptor(logging);  // <-- this is the important line!

Retrofit retrofit = new Retrofit.Builder()  
   .baseUrl(API_BASE_URL)
   .addConverterFactory(GsonConverterFactory.create())
   .client(httpClient.build())
   .build();
```

### 日志等级

日志太多会影响Android显示，OkHttp日志拦截器提供了四个等级的日志：**NONE** 、**BASIC** 、 **HEADERS** 、**BODY** 。它们分别包含哪些信息？接下来让我们一起去看看：

- None

  没有任何日志，而且会提升你的应用性能。

- Basic

  日志请求类型、url、请求正文的大小、响应状态和响应正文的大小。

  ``` xml
  D/HttpLoggingInterceptor$Logger: --> POST /upload HTTP/1.1 (277-byte body)  
  D/HttpLoggingInterceptor$Logger: <-- HTTP/1.1 200 OK (543ms, -1-byte body)  
  ```

- Headers

  日志请求和响应头、请求类型、url、响应状态。

  ``` xml
  D/HttpLoggingInterceptor$Logger: --> POST /upload HTTP/1.1  
  D/HttpLoggingInterceptor$Logger: Accept: application/json  
  D/HttpLoggingInterceptor$Logger: Content-Type: application/json  
  D/HttpLoggingInterceptor$Logger: --> END POST  
  D/HttpLoggingInterceptor$Logger: <-- HTTP/1.1 200 OK (1039ms)  
  D/HttpLoggingInterceptor$Logger: content-type: text/html; charset=utf-8  
  D/HttpLoggingInterceptor$Logger: cache-control: no-cache  
  D/HttpLoggingInterceptor$Logger: vary: accept-encoding  
  D/HttpLoggingInterceptor$Logger: Date: Wed, 28 Oct 2015 08:24:20 GMT  
  D/HttpLoggingInterceptor$Logger: Connection: keep-alive  
  D/HttpLoggingInterceptor$Logger: Transfer-Encoding: chunked  
  D/HttpLoggingInterceptor$Logger: OkHttp-Selected-Protocol: http/1.1  
  D/HttpLoggingInterceptor$Logger: OkHttp-Sent-Millis: 1446020610352  
  D/HttpLoggingInterceptor$Logger: OkHttp-Received-Millis: 1446020610369  
  D/HttpLoggingInterceptor$Logger: <-- END HTTP  
  ```

- Body

  日志请求和响应标头和正文。

  ``` xml
  D/HttpLoggingInterceptor$Logger: --> POST /upload HTTP/1.1  
  D/HttpLoggingInterceptor$Logger: --9df820bb-bc7e-4a93-bb67-5f28f4140795  
  D/HttpLoggingInterceptor$Logger: Content-Disposition: form-data; name="description"  
  D/HttpLoggingInterceptor$Logger: Content-Transfer-Encoding: binary  
  D/HttpLoggingInterceptor$Logger: Content-Type: application/json; charset=UTF-8  
  D/HttpLoggingInterceptor$Logger: Content-Length: 37  
  D/HttpLoggingInterceptor$Logger:  
  D/HttpLoggingInterceptor$Logger: "hello, this is description speaking"  
  D/HttpLoggingInterceptor$Logger: --9df820bb-bc7e-4a93-bb67-5f28f4140795--  
  D/HttpLoggingInterceptor$Logger: --> END POST (277-byte body)  
  D/HttpLoggingInterceptor$Logger: <-- HTTP/1.1 200 OK (1099ms)  
  D/HttpLoggingInterceptor$Logger: content-type: text/html; charset=utf-8  
  D/HttpLoggingInterceptor$Logger: cache-control: no-cache  
  D/HttpLoggingInterceptor$Logger: vary: accept-encoding  
  D/HttpLoggingInterceptor$Logger: Date: Wed, 28 Oct 2015 08:33:40 GMT  
  D/HttpLoggingInterceptor$Logger: Connection: keep-alive  
  D/HttpLoggingInterceptor$Logger: Transfer-Encoding: chunked  
  D/HttpLoggingInterceptor$Logger: OkHttp-Selected-Protocol: http/1.1  
  D/HttpLoggingInterceptor$Logger: OkHttp-Sent-Millis: 1446021170095  
  D/HttpLoggingInterceptor$Logger: OkHttp-Received-Millis: 1446021170107  
  D/HttpLoggingInterceptor$Logger: Perfect!  
  D/HttpLoggingInterceptor$Logger: <-- END HTTP (8-byte body) 
  ```

## 总结

请根据相应的log等级正确的使用日志管理，这样可以避免不必要的性能消耗。







