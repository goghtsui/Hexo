title: Parcelable vs Serializable
date: 2015-12-24 15:49:15
categories: [Complex]
tags: [Parcelable, Serializable, 序列化]
---

#### 序论
在Android中，我们需要传递对象的引用在activity和fragment之间，因此我们不得不放在Intent/Bundle中。

通过api我们了解到有两种选择，可以使用对象的**[Parcelable][1]**或者**[Serializable][2]**形式，作为Java的开发者，我们已经知道Serializable机制，那么为什么还有Parcelable？

要回答这个问题，先让我们看看这两个方法。
#### **Serializable**，简单之主

``` java
// access modifiers, accessors and constructors omitted for brevity
public class SerializableDeveloper implements Serializable
    String name;
    int yearsOfExperience;
    List<Skill> skillSet;
    float favoriteFloat;

    static class Skill implements Serializable {
        String name;
        boolean programmingRelated;
    }
}
```

Serializable的美在于你只需要将类和他的子类实现Serializable接口，这是一个标记接口，意味着没有方法来实现，Java可以简单有效的实现它的序列化。

这个方法的问题是，他使用到了反射，并且它是一个缓慢的进程。正是这个机制，创造了大量的临时对象，并且造成大量的gc。
<!-- more -->

#### **Parcelable**, 速度之王

``` java
// access modifiers, accessors and regular constructors ommited for brevity
class ParcelableDeveloper implements Parcelable {
    String name;
    int yearsOfExperience;
    List<Skill> skillSet;
    float favoriteFloat;

    ParcelableDeveloper(Parcel in) {
        this.name = in.readString();
        this.yearsOfExperience = in.readInt();
        this.skillSet = new ArrayList<Skill>();
        in.readTypedList(skillSet, Skill.CREATOR);
        this.favoriteFloat = in.readFloat();
    }

    void writeToParcel(Parcel dest, int flags) {
        dest.writeString(name);
        dest.writeInt(yearsOfExperience);
        dest.writeTypedList(skillSet);
        dest.writeFloat(favoriteFloat);
    }

    int describeContents() {
        return 0;
    }


    static final Parcelable.Creator<ParcelableDeveloper> CREATOR
            = new Parcelable.Creator<ParcelableDeveloper>() {

        ParcelableDeveloper createFromParcel(Parcel in) {
            return new ParcelableDeveloper(in);
        }

        ParcelableDeveloper[] newArray(int size) {
            return new ParcelableDeveloper[size];
        }
    };

    static class Skill implements Parcelable {
        String name;
        boolean programmingRelated;

        Skill(Parcel in) {
            this.name = in.readString();
            this.programmingRelated = (in.readInt() == 1);
        }

        @Override
        void writeToParcel(Parcel dest, int flags) {
            dest.writeString(name);
            dest.writeInt(programmingRelated ? 1 : 0);
        }

        static final Parcelable.Creator<Skill> CREATOR
            = new Parcelable.Creator<Skill>() {

            Skill createFromParcel(Parcel in) {
                return new Skill(in);
            }

            Skill[] newArray(int size) {
                return new Skill[size];
            }
        };

        @Override
        int describeContents() {
            return 0;
        }
    }
}
```

根据**[google engineers][3]**，这段代码明显运行的很快。其中一个原因就是，我们明确实例化的进程，而不是使用反射来推断它。支撑他的另一个原因就是，它也为此目的做了大量的优化。

无论怎样，可以明显的看出实现Parcelable不是免费的，他会有大量的样板代码，并且是类很难阅读和维护。

#### 速度测试

当然，我们想要知道Parcelable有多快。

#### 方法论

- 1.模拟传递对象给activity的过程，通过将一个对象放入bundle并调用**[Bundle#writeToParcel(Parcel,int)](https://developer.android.com/intl/zh-cn/reference/android/os/Bundle.html#writeToParcel(android.os.Parcel, int)**，然后取出来。
- 2.循环执行1000次
- 3.取10次独立运行的内存占用平均值，其他应用使用这个cpu
- 4.被测试对象是上面展示的SerializableDeveloper和ParcelableDeveloper
- 5.在多个设备上测试 - android版本
	- LG Nexus 4 - Android 4.2.2 
	- Samsung Nexus 10 - Android 4.2.2
	- HTC Desire Z - Android 2.3.3

#### 结果

![result](http://www.developerphil.com/assets/parcelable-vs-serializable-e1366334109758.png) 
**Nexus 10**

Serializable: 1.0004ms,  Parcelable: 0.0850ms - 10.16x improvement.

**Nexus 4**

Serializable: 1.8539ms - Parcelable: 0.1824ms - 11.80x improvement.

**Desire Z**

Serializable: 5.1224ms - Parcelable: 0.2938ms - 17.36x improvement.

想必你已经知道了，Parcelable比Serializable快了10倍。

#### 本质

 
如果你想要成为一个好公民，那就花费更多的时间来实现**[Parcelable][1]**，因为这将快10倍的速度，而且占用更少的资源。

然而，在大部分情况下，**[Serializable][2]**的慢并不是很明显，你可以随意使用它，但是记住，序列化是一个昂贵的操作，它将保持在一个低速状态。

如果你正在传递上千的序列化对象队列，整个过程很有可能超过了一秒钟，它可以使转换或旋转从纵向到横向感到十分缓慢。

 [1]: http://developer.android.com/intl/zh-cn/reference/android/os/Parcelable.html
 [2]: https://developer.android.com/intl/zh-cn/reference/java/io/Serializable.html
 [3]: http://stackoverflow.com/questions/3611843/is-using-serializable-in-android-bad/3612364#3612364