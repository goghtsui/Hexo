title: Retrofit 1.x和Retrofit 2.x的不同
date: 2016-12-07 11:11:28
categories: [Android]
tags: [Retrofit]
---

## 序言

Retrofit库已经发布很久了，已经是主流的Http client库之一，使用简单、高效，但是随着版本的升级，也出现了一些疑难杂症，下面就详细给大家讲解一下，给大家填填坑：

>官方描述：Type-safe HTTP client for Android and Java by Square.
官网地址：[Square-Retrofit](http://square.github.io/retrofit/).
Github地址：[Github-Retrofit](https://github.com/square/retrofit/).
<!-- more -->

到现在已经经历了1.x到2.x的过程，这自然是一个越来越完善、越强大的趋势，但是Square还是坑了我们一把。在1.x到2.x的迭代过程中，并不是平滑升级的，而是发生了很大的转变，为了防止更多的人遇到这些坑，我就把我了解的一些不同给大家说一所，当然，哪里描述有问题，表述不正确的随时回复。

## Retrofit 1.x -> Retrofit 2.x

Retrifit 1系列最高是1.9.0，Retrofit 2 系列目前最高 2.1.0。

### Maven & Gradle 依赖

1. 在Retrofit 1中，你需要导入底层的HTTP客户端。默认情况下，Retrofit 2将OkHttp最为默认的底层库，它已经作为Retrofit 2本身的依赖。

``` xml
compile 'com.squareup.retrofit2:retrofit:2.1.0'  
```

假如你想要使用指定版本的OkHttp，那么你可以这样写：

``` xml
compile ('com.squareup.retrofit2:retrofit:2.1.0') {  
  // 移除 Retrofit 的 OkHttp 
  exclude module: 'okhttp'
}

// 添加自定义的依赖库
compile 'com.squareup.okhttp3:okhttp:3.3.1'  
```

2. Maven: Retrofit & OkHttp

``` xml
<dependency>  
  <groupId>com.squareup.retrofit2</groupId>
  <artifactId>retrofit</artifactId>
  <version>2.1.0</version>
</dependency>  
<dependency>  
  <groupId>com.squareup.okhttp</groupId>
  <artifactId>okhttp</artifactId>
  <version>3.3.1</version>
</dependency>
```

3. Retrofit 2默认不集成Gson和RxJava的。之前，你不需要担心任何集成转换器，现在你需要导入转换器，当然RxJava也需要手动导入，下文还会详细描述：

- Converter

``` xml
compile 'com.squareup.retrofit2:converter-gson:2.1.0'  
```

- RxJava

``` xml
compile 'com.squareup.retrofit2:adapter-rxjava:2.1.0'  
compile 'io.reactivex:rxandroid:1.0.1'  
```

### RestAdapter —> Retrofit

- Retrofit 1.9

``` java
RestAdapter.Builder builder = new RestAdapter.Builder(); 
```

- Retrofit 2.x

``` java
Retrofit.Builder builder = new Retrofit.Builder();  
```

### setEndpoint —> baseUrl

- Retrofit 1.9

``` java
RestAdapter adapter = new RestAdapter.Builder()  
    .setEndpoint(API_BASE_URL);
    .build();

YourService service = adapter.create(YourService.class);  

```

注: 在你调用Retrofit.Builder的build()方法之前, 你至少需要定义API_BASE_URL.

- Retrofit 2.x

``` java
Retrofit retrofit = new Retrofit.Builder()  
    .baseUrl(API_BASE_URL);
    .build();

YourService service = retrofit.create(YourService.class); 
```

### Base Url处理

- Retrofit 1.x

``` java
public interface UserService {  
    @POST("me")
    User me();
}

RestAdapter adapter = RestAdapter.Builder()  
    .setEndpoint("https://your.api.url/v2/");
    .build();

UserService service = adapter.create(UserService.class);

// the request url for service.me() is: 
// https://your.api.url/v2/me

```

- Retrofit 2.x

Retrofit 2.x 使用了全新的url处理方式，内部是通过**HttpUrl.resolve()**方式创建的URL，类似于**<a href...>**的链接，这一点非常重要。我学习的时候看到有三种展示方式：

1. url：https://your.api.url/me

``` java

public interface UserService {  
    @POST("/me")
    Call<User> me();
}

Retrofit retrofit = new Retrofit.Builder()  
    .baseUrl("https://your.api.url/v2");
    .build();

UserService service = retrofit.create(UserService.class);

// the request url for service.me() is: 
// https://your.api.url/me

```

2. url：https://your.api.url/me

``` java

public interface UserService {  
    @POST("me")
    Call<User> me();
}

Retrofit retrofit = new Retrofit.Builder()  
    .baseUrl("https://your.api.url/v2");
    .build();

UserService service = retrofit.create(UserService.class);

// the request url for service.me() is: 
// https://your.api.url/me

```

3. url：https://your.api.url/v2/me

``` java

public interface UserService {  
    @POST("me")
    Call<User>me();
}

Retrofit retrofit = new Retrofit.Builder()  
    .baseUrl("https://your.api.url/v2/");
    .build();

UserService service = retrofit.create(UserService.class);

// the request url for service.me() is: 
// https://your.api.url/v2/me

```

经过我的测试（Retrofit 2.1.0），baseUrl必须是以 **/** 结尾（即@GET不以**/**开头），不然会报错：`java.lang.IllegalArgumentException: baseUrl must end in /: https://your.api.url/v2/me` ，所以，在2.1.0的版本，我们要注意这一点，如果这里有问题，还请留言反馈。

4. 动态URL

在你的url无法立即获取或者使用的时候，你可以配置动态的url来使用，在Retrofit 2.x你可以将HTTP请求注解留空，然后将**@url**作为方法参数的注解，请看以下示例代码：

``` java

public interface UserService {  
    @GET
    public Call<File> getZipFile(@Url String url);
}

```

### OkHttp集成

OkHttp 在Retrofit 1.x里是可选的。如果你想让Retrofit使用OkHttp作为HTTP连接接口，你需要手动包含OkHttp依赖。 
但是在Retrofit 2.x中，OkHttp 是必须的，并且自动设置为了依赖。下面的代码来自Retrofit 2.0的pom文件：

``` xml
<dependency>  
  <groupId>com.squareup.okhttp</groupId>
  <artifactId>okhttp</artifactId>
  <version>3.3.1</version>
</dependency>  
```
OkHttp 使用Call 来封装响应，如果你想使用OkHttp特定的版本，不想使用内部的版本，你可以按照以下代码手动配置：

``` xml
compile ('com.squareup.retrofit2:retrofit:2.1.0') {  
  // exclude Retrofit’s OkHttp peer-dependency module and define your own module import
  exclude module: 'okhttp'
}
compile 'com.squareup.okhttp3:okhttp:3.3.1'  
```

### OkHttp拦截器

在Retrofit 1.x中，你可以使用RequestInterceptor来拦截一个请求，但是它已经从Retrofit 2.x 移除了，因为HTTP连接层已经转为OkHttp。 
结果就是，现在我们必须转而实用OkHttp里面的Interceptor。首先你需要实用Interceptor创建一个OkHttpClient对象，然后使用如下Retrofit.Builder()的client()方法设置你自定义的增强版OkHttp：

``` java

OkHttpClient.Builder httpClient = new OkHttpClient.Builder();  
httpClient.addInterceptor(new Interceptor() {  
    @Override
    public Response intercept(Chain chain) throws IOException {
        Request original = chain.request();

        // Customize the request 
        Request request = original.newBuilder()
                .header("Accept", "application/json")
                .header("Authorization", "auth-token")
                .method(original.method(), original.body())
                .build();

        Response response = chain.proceed(request);

        // Customize or return the response 
        return response;
    }
});

OkHttpClient client = httpClient.build();  
Retrofit retrofit = new Retrofit.Builder()  
    .baseUrl("https://your.api.url/v2/");
    .client(client)
    .build();

```

### 同步&异步 请求

在Retrofit 1.x中，通过在service接口中不同的方法声明来区分，同步方法需要一个返回类型，异步的方法需要一个统一的**Callback**作为最后一个参数。

#### 接口声明

- Retrofit 1.x

``` java

public interface UserService {  
    // Synchronous Request
    @POST("/login")
    User login();

    // Asynchronous Request
    @POST("/login")
    void getUser(@Query String id, Callback<User> cb);
}

```

- Retrofit 2.x

在Retrofit 2.x中，service中方法的声明没有区别，返回类型都被统一封装到一个**Call**类型里面：

``` java

public interface UserService {  
    @POST("/login")
    Call<User> login();
}

```

#### 请求执行

- Retrofit 1.x

``` java

// synchronous
User user = userService.login();

// asynchronous
userService.login(new Callback<User>() {  
@Override
    public void success(User user, Response response) {
        // handle response
    }

    @Override
    public void failure(RetrofitError error) {
        // handle error
    }
});

```

- Retrofit 2.x

对于Retrofit 2.x，返回类型都被统一封装到了Call类型里，所以使用了完全不同的执行方式：

``` java

// 同步
Call<User> call = userService.login();  
User user = call.execute().body();

```

``` java

// 异步
Call<User> call = userService.login();  
call.enqueue(new Callback<User>() {  
    @Override
    public void onResponse(Call<User> call, Response<User> response) {
        // response.isSuccessful() is true if the response code is 2xx
        if (response.isSuccessful()) {
            User user = response.body();
        } else {
            int statusCode = response.code();

            // handle request errors yourself
            ResponseBody errorBody = response.errorBody();
        }
    }

    @Override
    public void onFailure(Call<User> call, Throwable t) {
        // handle execution failures like no internet connectivity 
    }
}

```

注：在Retrofit 2.x中，即使请求不成功**onResponse()**方法也会被执行，但是**Response**类提供了**isSuccessful()**方法用于检查请求结果（返回的状态码2xx），如果状态代码不是2xx，您需要自己处理错误。如果在失败的情况下，你希望自己解析错误响应信息，你可以通过使用**ResponseBody**类的**errorBody()**方法自己解析对象。

#### 取消请求

使用Retrofit 1.x，即使请求尚未执行，也无法取消。在Retrofit 2.x这一点已经得到了改善，即如果HTTP调度器没有执行该它，你最终可以取消任何请求。

``` java

Call<User> call = userService.login();  
User user = call.execute().body();

// changed your mind, cancel the request
call.cancel();  

```

你不需要关心是同步还是异步请求，只要你足够的及时，OkHttp不会发送出任何的请求。


### 转换器

Retrofit 1.x对Gson集成并作为默认的JSON转换器，但是Retrofit 2.x没有集成任何转换器，需要你自定义转换器，如果你想使用Gson作为转换器，你可以按照以下方式使用兄弟库：

#### 添加依赖

``` xml
compile 'com.squareup.retrofit2:converter-gson:2.1.0'  
```

还有其它很多可用的转换器，根据自己的喜欢手动添加即可：

- **Gson**: com.squareup.retrofit2:converter-gson:2.1.0
- **Moshi**: com.squareup.retrofit2:converter-moshi:2.1.0
- **Jackson**: com.squareup.retrofit2:converter-jackson:2.1.0
- **SimpleXML**: com.squareup.retrofit2:converter-simplexml:2.1.0
- **ProtoBuf**: com.squareup.retrofit2:converter-protobuf:2.1.0
- **Wire**: com.squareup.retrofit2:converter-wire:2.1.0

假如，没有你想使用的依赖库，你可以继承**Converter.Factory**抽象类，可以去这里参考编写：[转换器实现示例](https://github.com/square/retrofit/tree/master/retrofit-converters)

#### 添加转换器

``` java

Retrofit retrofit = new Retrofit.Builder()  
    .baseUrl("https://your.api.url/v2/");
    .addConverterFactory(ProtoConverterFactory.create())
    .addConverterFactory(GsonConverterFactory.create())
    .build();

```

上面代码中添加了两个转换器，你是不是要会选择哪个使用？这一点很重要：**添加的顺序决定了使用的顺序，默认使用第一个转换器，如果第一个转换器不可用，会使用第二个转化器**。所以，你可以多添加几个转换器，这是允许的，但是一定要注意顺序。

### RxJava集成

Retrofit 1 已经集成了三个请求执行机制：同步，异步和RxJava。Retrofit 2 仅仅同步和异步默认是可用的，因此，Retrofit开发团队创建了一种将额外的执行机制插入Retrofit的方法。你可以为你的应用程序添加多个机制，如RxJava或Futures。为了使Retrofit恢复RxJava的支持，需要添加两个依赖：

- 第一个依赖是 RxJava的 **CallAdapter**，这是为了告诉Reftrofit有新的方式来处理请求。这就意味着你可以通过定义CustomizedCall <T>来替换Call <T>，在 RxJava 的情况下，我们需要将返回值Call<T> 修改为 Observable<T>。
- 第二个依赖是需要AndroidSchedulers类，这是需要在Android的主线程上订阅代码。

``` xml
compile 'com.squareup.retrofit2:adapter-rxjava:2.1.0'  
compile 'io.reactivex:rxandroid:1.0.1'  
```

接下来需要的是在创建service实例之前将新的 CallAdapter 添加到 Retrofit 对象。

``` java

Retrofit retrofit = new Retrofit.Builder()  
    .baseUrl(baseUrl);
    .addCallAdapterFactory(RxJavaCallAdapterFactory.create())
    .addConverterFactory(GsonConverterFactory.create())
    .build();

```

首先，我们声明一个service接口。之后，我们假设有一个userService实例被创建，并且可以直接利用Observable来观察Android的主线程。我们还将一个新的Subscriber传递给subscribe方法，它将最终返回成功的响应或错误。

``` java

public interface UserService {  
    @POST("/me")
    Observable<User> me();
}

// this code is part of your activity/fragment
Observable<User> observable = userService.me();  
observable  
        .observeOn(AndroidSchedulers.mainThread())
        .subscribeOn(Schedulers.io())
        .subscribe(new Subscriber<User>() {
    @Override
    public void onCompleted() {
        // handle completed
    }

    @Override
    public void onError(Throwable e) {
        // handle error
    }

    @Override
    public void onNext(User user) {
        // handle response
    }
});

```

### 默认没有日志记录

这个内容相对独立，就单独拿出来说吧，请查看另外一篇文章。

## 总结

这些内容摘自官方和日常使用总结，基本上覆盖了日常开发使用，所以你还需要在使用中不断的探索。Retrofit真的是非常的简单、高效，给你填了很多坑，节省了你的开发时间，虽然你需要花些时间去了解它，但是一旦你可以熟练的使用它，那么他带给你的惊喜是你无法想象的，所以，你又有什么理由不去使用它呢？

其实你可以通过官方的Changelog来详细了解以下：
>[Changelog](https://github.com/square/retrofit/blob/master/CHANGELOG.md)
