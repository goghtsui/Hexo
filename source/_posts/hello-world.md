title: 简单实现ButterKnife中的injectView的方案
tags: [ButterKnife, InjectView, annotation]
date: 2015-11-13 09:37:14
---
首先说这里面用的知识点，注解、反射。

Android中findViewById(int resId)接受一个int的id参数，即通过资源id就可以找到对应的View。通过注解(annotation),我们可以资源id声明在对应的field上面，通过Java的反射，遍历每个field，找到对应的id，就可以初始化这个field（即view）。

<!--more-->

## 1、注解声明
``` bash

// 表示用在字段上
@Target(ElementType.FIELD)
// 表示在生命周期是运行时
@Retention(RetentionPolicy.RUNTIME)
//注解类，实现findViewById功能
public @interface FindView {
	int findViewByResId() default 0;
}

```

## 2、反射注入
``` bash

Class<?> clazz = this.getClass();
// 获得Activity中声明的字段
Field[] fields = clazz.getDeclaredFields();
//遍历所有字段
for (Field field : fields) {
// 是否有我们自定义的注解类标志
  if (field.isAnnotationPresent(FindView.class)) {
  	   FindView inject = field.getAnnotation(FindView.class);
      int mId = inject.findViewByResId();
      View view;
      if (mId > 0) {
          view = findViewById(mId);
          field.setAccessible(true);
          field.set(this, view);// 给我们要找的字段设置值
      }
	}
}

```
## 3、测试
``` bash

@FindView(findViewByResId = R.id.id_text)
private TextView mText;

```

总结：
是不是很简单，这算是一个入门，接下来大家可以好好利用这种原理，实现不一样的功能了。
个人觉得这个不是特别好，每次都要通过反射来初始化，大家还是结合自己的开发环境酌情使用。