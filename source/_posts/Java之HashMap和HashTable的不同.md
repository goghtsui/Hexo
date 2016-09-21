title: Java之HashMap和HashTable的不同
date: 2016-02-19 15:13:32
categories: [Java]
tags: [HashTable, HashMap]
---

HashTable和HashMap的6个不同点：Java热门面试题例子

HashTable和HashMa的区别是面试题中经常被问到的问题。还有其他比较热门的问题，像ArrayList和Vector、Comparator和Comparable。这些问题经常在面试中被问题，以检查我们是否正确理解集合类的使用和拥有的替代解决方案的知识。这两者之间的不同，包括6个点，分别是_Synchronization_、_Null keys and values_、_Iterating values_、_Fail fast iterator_、_Performance_、_Superclass_


## HashTable和HashMap的不同

**1.Synchronization or Thread Safe :**

这是它们最重要的不同点。HashMap不是同步的，并且不是是线程安全的。相反，HashTable是线程安全和同步的。

什么时候使用HashMap？答案是如果你的应用不需要任何多线程任务，就是说HashMap适用于没有线程的应用。HashTable应该在多线程应用中使用。
<!-- more -->

扩展：
Java提供了ConcurrentHashMap，它是HashTable的替代品，比HashTable的扩展性更好。HashMap可以通过下面的语句进行同步：
``` java
Map m = Collections.synchronizeMap(hashMap);
```

**2. Null keys and null values :**

HashMap允许一个空的key和多个空的value， 然而HashTable不允许空的键值。

**3. Iterating the values:**

HashMap对象的值是通过Iterator迭代的。HashTable是除vector之外惟一的一个使用了enumerator迭代器来迭代其对象值的类。

**4.  Fail-fast iterator :**

在HashMap中是**fail-fast**迭代器，而HashTable的emumerator不是。根据[Oracle Docs][1],如果HashTable对象的iterator以任何方式被创建之后它在“结构上”被修改，那么除了迭代器自己的remove方法，否则迭代器将抛出ConcurrentModificationException异常。

结构上的更改指的是删除或者插入一个元素（hashtable和hashmap），因此，由Hashtable的键和元素方法返回的enumerations不是Fail-fast。关于[iterator and enumeration的不同][2].

扩展：
如果某个集合对象创建了Iterator或者ListIterator，然后其它的线程试图“结构上”更改集合对象，将会抛出ConcurrentModificationException异常。但其它线程可以通过set()方法更改集合对象是允许的，因为这并没有从“结构上”更改集合。但是假如已经从结构上进行了更改，再调用set()方法，将会抛出IllegalArgumentException异常。

**5. Performance :**
HashMap是比较快的，并且使用了很少的内存。在单个的线程环境中，不同步的对象通常在性能上是比同步的对象要好的。

**6.Superclass and Legacy :**
HashTable是Dictionary的子类，在jdk 1.7中已经过时了，因此它不再被使用。它是最好的外部实现同步的方法，或者使用一个ConcurrentMap实现（如ConcurrentHashMap）HashMap是AbstractMap的子类，尽管HashMap和HashTable有不同的父类，但是他们都继承了抽象类“Map”。


## HashMap和HashTable的例子
``` java
import java.util.Hashtable;


public class HashMapHashtableExample {
    
    public static void main(String[] args) { 
 
           
  
        Hashtable<String,String> hashtableobj = new Hashtable<String, String>();
        hashtableobj.put("Alive is ", "awesome");
        hashtableobj.put("Love", "yourself");
        System.out.println("Hashtable object output :"+ hashtableobj);
 
         
 
        HashMap hashmapobj = new HashMap();
        hashmapobj.put("Alive is ", "awesome");  
        hashmapobj.put("Love", "yourself"); 
        System.out.println("HashMap object output :"+hashmapobj);   
 
 	}
}
```

输出结果：
```
Hashtable object output :{Love=yourself, Alive is =awesome}
HashMap object output :{Alive is =awesome, Love=yourself}
```

## HashMap和Hashtable的相似之处

- **1.插入顺序：** 随着时间的推移，HashMap和HashTable都不能保证集合的顺序，相反的，使用LinkedHashMap不会因为时间的推移而改变顺序。

- **2.Map接口：** HashMap和HashTable都是实现了Map接口。

- **3.存和取的方法：** HashMap和HashTable为存取提供了稳定的时间性能

- **4.内部原理：** HashMap和HashTable遵顼的是散列的原则：[HashMap是如何工作的？][3]

## HashMap和HashTable什么时候使用？

- **1. 单线程应用**
在非线程应用中，HashMap要优于HashTable的，简单来说，使用HashMap在非同步或者单线程的应用中。

- **2. 多线程应用**
我们应该避免使用Hashtable，因为这个类在最近的jdk1.8中过时了。Oracle已经提供了很好的替代的类：**ConcurrentHashMap**，对于多线程应用，使用ConcurrentHashMap而不是Hashtable。


## 总结  

|   区别   | HashMap        | HashTable        | 
| --------| --------------- | ---------------- |
| 同步     | NO             |    Yes           |
| 线程安全  | NO             |    Yes           |
| 空键值   | 一个空键，任意空值 | 不允许空键值       |
| 迭代类型  | Fail fast迭代器 | Fail safe迭代器   |
| 性能     | 快              | 作比较慢          |
| 父类和遗弃| AbstractMap，NO  | Dictionary , Yes |





[1]: http://docs.oracle.com/javase/7/docs/api/java/util/Hashtable.html
[2]: http://javahungry.blogspot.com/2013/06/difference-between-iterator-and-enumeration-collections-java-interview-question-with-example.html
[2]: http://javahungry.blogspot.com/2013/08/hashing-how-hash-map-works-in-java-or.html