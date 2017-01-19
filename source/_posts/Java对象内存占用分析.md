title: Java对象内存占用分析
date: 2017-01-19 16:15:48
categories: [Java]
tags: [内存, JVM]
---

> 原文地址：[查看原文](https://segmentfault.com/a/1190000006933272)



## 序言

之前说过在Android中关于enum的使用，对内存的占用比较大，所以就了解了一下内存占用方面，基本上大同小异，下面是我觉得比较全面的描述，重新排版、整理，如果有时间，希望你能好好了解一下。

下面的内容深入分析并验证了不同Java对象占用内存空间大小的情况。对于不同的jvm实现，Java对象占用的内存空间大小可能不尽相同，本文主要分析HotSpot jvm中的情况，实验环境为64位window10系统、JDK1.8，使用JProfiler进行结论验证。

## Java对象内存布局

Java对象的内存布局包括

- 对象头(Header)
- 实例数据(Instance Data)
- 补齐填充(Padding)

### 对象头：

在64位机器上，默认不开启指针压缩（-XX:-UseCompressedOops）的情况下，对象头占用12bytes，开启指针压缩（-XX:+UseCompressedOops）则占用16bytes。

### 实例数据：

原生类型（primitive type）的内存占用如下：

| Primitive Type | Memory Required(bytes) |
| -------------- | ---------------------- |
| byte, boolean  | 1 byte                 |
| short, char    | 2 bytes                |
| int, float     | 4 bytes                |
| long, double   | 8 bytes                |

**注：对象引用（reference）类型在64位机器上，关闭指针压缩时占用4bytes， 开启时占用8bytes**

### 对齐填充：

Java对象占用空间是8字节对齐的，即所有Java对象占用bytes数必须是8的倍数。例如，一个包含两个属性的对象：int和byte，并不是占用17bytes(12+4+1)，而是占用24bytes（对17bytes进行8字节对齐）

## Java对象内存占用

首先根据以上的计算规则，进行一个简单的验证。使用下面的程序进行验证：

```
public class Test {
    public static void main(String[] args) throws InterruptedException {
        TestObject testObject = new TestObject();

        Thread.sleep(600 * 1000);
        System.out.println(testObject);
    }
}

class TestObject {
    private int i;
    private double d;
    private char[] c;

    public TestObject() {
        this.i = 1;
        this.d = 1.0;
        this.c = new char[]{'a', 'b', 'c'};
    }
}
```

TestObject对象有四个属性，分别为int, double, Byte, char[]类型。在打开指针压缩(-XX:+UseCompressedOops)的情况下，在64位机器上，TestObject占用的内存大小应为：

> 12(Header) + 4(int) + 8(double) + 4(reference) = 28 (bytes)，加上8字节对齐，最终的大小应为32 bytes。

JProfiler中的结果为：

![img](https://segmentfault.com/img/bVDf3h?w=746&h=191)

可以看到,TestObject占用的内存空间大小（Shallow Size）为32 bytes。

关于Retained Size和Shallow Size的区别，可以参看：
[[Shallow Size和 Retained Size的区别](http://blog.csdn.net/e5945/article/details/7708253)](http://blog.csdn.net/e5945/article/details/7708253)

当指针压缩关闭时(-XX:-UseCompressedOops)，在64位机器上，TestObject占用的内存大小应为：

> 16(Header) + 4(int) + 8(double) + 8(reference) = 36 (bytes), 8字节对齐后为 40 bytes。

JProfile的结果为：

![img](https://segmentfault.com/img/bVDf5q?w=735&h=183)

### **包装类型**

包装类（Boolean/Byte/Short/Character/Integer/Long/Double/Float）占用内存的大小等于对象头大小加上底层基础数据类型的大小。

包装类型的对象内存占用情况如下：

| Numberic Wrappers | +useCompressedOops | -useCompressedOops |
| ----------------- | ------------------ | ------------------ |
| Byte, Boolean     | 16 bytes           | 24 bytes           |
| Short, Character  | 16 bytes           | 24 bytes           |
| Integer, Float    | 16 bytes           | 24 bytes           |
| Long, Double      | 24 bytes           | 24 bytes           |

### **数组**

64位机器上，数组对象的对象头占用24 bytes，启用压缩后占用16字节。比普通对象占用内存多是因为需要额外的空间存储数组的长度。基础数据类型数组占用的空间包括数组对象头以及基础数据类型数据占用的内存空间。由于对象数组中存放的是对象的引用，所以对象数组本身的大小=数组对象头+length * 引用指针大小，总大小为对象数组本身大小+存放的数据的大小之和。

举两个例子：

- int[10]: 开启压缩：16 + 10 * 4 = 56 bytes；

![img](https://segmentfault.com/img/bVDgBP?w=717&h=318)

关闭压缩：24 + 10 * 4 = 64bytes。

![img](https://segmentfault.com/img/bVDgCA?w=724&h=344)

- new Integer[3]:

```
    关闭压缩：
    Integer数组本身：24(header) + 3 * 8(Integer reference) = 48 bytes;
    总共：48 + 3 * 24(Integer) = 120 bytes。
```

![img](https://segmentfault.com/img/bVDgFT?w=729&h=263)

```
    开启压缩：
    Integer数组本身：16(header) + 3 * 4(Integer reference) = 28(padding) -> 32 (bytes)
    总共：32 + 3 * 16(Integer) = 80 (bytes)
```

![img](https://segmentfault.com/img/bVDgGC?w=732&h=267)

### **String**

在JDK1.7及以上版本中，String包含2个属性，一个用于存放字符串数据的char[], 一个int类型的hashcode, 部分源代码如下：

```
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];

    /** Cache the hash code for the string */
    private int hash; // Default to 0
}
```

因此，在关闭指针压缩时，一个String本身需要 16(Header) + 8(char[] reference) + 4(int) = 32 bytes。除此之外，一个char[]占用24 + length * 2 bytes(8字节对齐), 即一个String占用的内存空间大小为：

> 56 + length * 2 bytes (8字节对齐)。举几个例子。

- 一个空字符串("")的大小应为：56 + 0 * 2 bytes = 56 bytes。JProfiler结果：

![img](https://segmentfault.com/img/bVDgeh?w=714&h=175)

- 字符串"abc"的大小应为：56 + 3 * 2 = 62(8字节对齐)->64 (bytes)

![img](https://segmentfault.com/img/bVDgem?w=724&h=178)

- 字符串"abcde"的大小应为：56 + 5 * 2 = 66->72 (bytes)

![img](https://segmentfault.com/img/bVDgeq?w=721&h=172)

- 字符串"abcde"在开启指针压缩时的大小为：

```
   String本身：12(Header) + 4(char[] reference) + 4(int hash) = 20(padding) -> 24 (bytes);   
   存储数据：16(char[] header) + 5*2 = 26(padding) -> 32 (bytes)         
   总共：24 + 32 = 56 (bytes)  
```

![img](https://segmentfault.com/img/bVDgff?w=718&h=200)

## 常用数据结构内存占用

根据上面的内存占用计算规则，可以计算出一个对象在内存中的占用空间大小情况，下面举例分析下Java中的Enum, ArrayList及HashMap的内存占用情况，读者可以仿照分析计算过程来计算其他数据结构的内存占用情况。

**注：** 下面的分析计算基于HotSpot Jvm, JDK1.8, 64位机器，开启指针压缩。

### 枚举类

创建enum时，编译器会生成一个相关的类，这个类继承自java.lang.Enum。Enum类拥有两个属性变量，分别为int的ordinal和String的name, 相关源码如下：

```
public abstract class Enum<E extends Enum<E>>
        implements Comparable<E>, Serializable {
    /**
     * The name of this enum constant, as declared in the enum declaration.
     * Most programmers should use the {@link #toString} method rather than
     * accessing this field.
     */
    private final String name;

    /**
     * The ordinal of this enumeration constant (its position
     * in the enum declaration, where the initial constant is assigned
     * an ordinal of zero).
     *
     * Most programmers will have no use for this field.  It is designed
     * for use by sophisticated enum-based data structures, such as
     * {@link java.util.EnumSet} and {@link java.util.EnumMap}.
     */
    private final int ordinal;
}
```

以下面的TestEnum为例进行枚举类的内存占用分析

```
public enum TestEnum {
        ONE(1, "one"),
        TWO(2, "two");

        private int code;
        private String desc;

        TestEnum(int code, String desc) {
            this.code = code;
            this.desc = desc;
        }

        public int getCode() {
            return code;
        }

        public String getDesc() {
            return desc;
        }
}
```

这里TestEnum的每个实例除了父类的两个属性外，还拥有一个int的code及String的desc属性，所以一个TestEnum的实例本身所占用的内存大小为：

> 12(header) + 4(ordinal) + 4(name reference) + 4(code) + 4(desc reference) = 28(padding) -> 32 bytes.

总共占用的内存大小为：
按照上面对字符串类型的分析，desc和name都占用：48 bytes。
所以TestEnum.ONE占用总内存大小为：

> 12(header) + 4(ordinal) + 4(code) + 48 * 2(desc, name) + 4(desc reference) + 4(name reference) = 128 (bytes)

JProfiler中的结果可以验证上述分析：
![img](https://segmentfault.com/img/bVDjFp?w=743&h=259)

### ArrayList

在分析ArrayList的内存之前，有必须先了解下ArrayList的实现原理。ArrayList实现List接口，底层使用数组保存所有元素。其操作基本上是对数组的操作。下面分析下源代码：

##### 1. 底层使用数组保存数据：

```
    /**
     * The array buffer into which the elements of the ArrayList are stored.
     * The capacity of the ArrayList is the length of this array buffer. Any
     * empty ArrayList with elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA
     * will be expanded to DEFAULT_CAPACITY when the first element is added.
     */
    transient Object[] elementData; // non-private to simplify nested class access
```

##### 2. 构造方法：

ArrayList提供了三种方式的构造器，可以构造一个默认的空列表、构造一个指定初始容量的空列表及构造一个包含指定collection元素的列表，这些元素按照该collection的迭代器返回它们的顺序排列。

```
    /**
     * Shared empty array instance used for default sized empty instances. We
     * distinguish this from EMPTY_ELEMENTDATA to know how much to inflate when
     * first element is added.
     */
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
    
    /**
     * Constructs an empty list with the specified initial capacity.
     *
     * @param  initialCapacity  the initial capacity of the list
     * @throws IllegalArgumentException if the specified initial capacity
     *         is negative
     */
    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }

    /**
     * Constructs an empty list with an initial capacity of ten.
     */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }

    /**
     * Constructs a list containing the elements of the specified
     * collection, in the order they are returned by the collection's
     * iterator.
     *
     * @param c the collection whose elements are to be placed into this list
     * @throws NullPointerException if the specified collection is null
     */
    public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {
            // c.toArray might (incorrectly) not return Object[] (see 6260652)
            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            // replace with empty array.
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }
```

##### 3. 存储：

ArrayList提供了set(int index, E element)、add(E e)、add(int index, E element)、addAll(Collection<? extends E> c)等，这里着重介绍一下add(E e)方法。

```
    /**
     * Appends the specified element to the end of this list.
     *
     * @param e element to be appended to this list
     * @return <tt>true</tt> (as specified by {@link Collection#add})
     */
    public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
```

add方法将指定的元素添加到此列表的尾部。这里注意下ensureCapacityInternal方法，这个方法会检查添加后元素的个数是否会超过当前数组的长度，如果超出，数组将会进行扩容。

```
    /**
     * Default initial capacity.
     */
    private static final int DEFAULT_CAPACITY = 10;

    /**
     * Increases the capacity of this <tt>ArrayList</tt> instance, if
     * necessary, to ensure that it can hold at least the number of elements
     * specified by the minimum capacity argument.
     *
     * @param   minCapacity   the desired minimum capacity
     */
    public void ensureCapacity(int minCapacity) {
        int minExpand = (elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
            // any size if not default element table
            ? 0
            // larger than default for default empty table. It's already
            // supposed to be at default size.
            : DEFAULT_CAPACITY;

        if (minCapacity > minExpand) {
            ensureExplicitCapacity(minCapacity);
        }
    }

    private void ensureCapacityInternal(int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
        }

        ensureExplicitCapacity(minCapacity);
    }

    private void ensureExplicitCapacity(int minCapacity) {
        modCount++;

        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
```

如果初始时没有指定ArrayList大小，在第一次调用add方法时，会初始化数组默认最小容量为10。看下grow方法的源码：

```
    /**
     * Increases the capacity to ensure that it can hold at least the
     * number of elements specified by the minimum capacity argument.
     *
     * @param minCapacity the desired minimum capacity
     */
    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```

从上述代码可以看出，数组进行扩容时，会将老数组中的元素重新拷贝一份到新的数组中，每次数组扩容的增长是原容量的1.5倍。这种操作的代价是很高的，因此在实际使用时，应该尽量避免数组容量的扩张。当可预知要保存的元素的数量时，要在构造ArrayList实例时，就指定其容量，以避免数组扩容的发生。或者根据实际需求，通过调用ensureCapacity方法来手动增加ArrayList实例的容量。

ArrayList其他操作读取删除等原理这里不作介绍了。

##### 4. 内存占用

下面开始分析ArrayList的内存占用情况。ArrayList继承AbstractList类，AbstractList拥有一个int类型的modCount属性，ArrayList本身拥有一个int类型的size属性和一个数组属性。所以一个ArrayList实例本身的的大小为：

> 12(header) + 4(modCount) + 4(size) + 4(elementData reference) = 24 (bytes)

下面分析一个只有一个Integer(1)元素的ArrayList<Integer>实例占用的内存大小。

```
   ArrayList<Integer> testList = Lists.newArrayList();
   testList.add(1);
```

根据上面对ArrayList原理的介绍，当调用add方法时，ArrayList会初始化一个默认大小为10的数组，而数组中保存的Integer(1)实例大小为16 bytes。则testList占用的内存大小为：

> 24(ArrayList itselft) + 16(elementData array header) + 10 * 4(elemetData reference) + 16(Integer) = 96 (bytes)

JProfiler中的结果验证了上述分析：

![img](https://segmentfault.com/img/bVDkk7?w=734&h=269)

### HashMap

要分析HashMap的内存占用，同样需要先了解HashMap的实现原理。

##### 1. HashMap的数据结构

HashMap是一个“链表散列”的数据结构，即数组和链表的结合体。

![img](https://segmentfault.com/img/bVDlSq?w=600&h=357)

从图上可以看出，HashMap底层是一个数组结构，数组中的每一项又是一个链表。当新建一个HashMap的时候，初始化一个数组，源码如下：

```
    /**
     * The table, initialized on first use, and resized as
     * necessary. When allocated, length is always a power of two.
     * (We also tolerate length zero in some operations to allow
     * bootstrapping mechanics that are currently not needed.)
     */
    transient Node<K,V>[] table;
```

Node是链表中一个结点，一个Node对象保存了一对HashMap的Key,Value以及指向下一个节点的指针，源码如下：

```
     /**
     * Basic hash bin node, used for most entries.  (See below for
     * TreeNode subclass, and in LinkedHashMap for its Entry subclass.)
     */
    static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;

        Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }
     }
```

##### 2. 构造方法

HashMap提供了四种方式的构造器，分别为指定初始容量及负载因子构造器，指定初始容量构造器，不指定初始容量及负载因子构造器，以及根据已有Map生成新Map的构造器。

```
    /**
     * Constructs an empty <tt>HashMap</tt> with the specified initial
     * capacity and load factor.
     *
     * @param  initialCapacity the initial capacity
     * @param  loadFactor      the load factor
     * @throws IllegalArgumentException if the initial capacity is negative
     *         or the load factor is nonpositive
     */
    public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);
        this.loadFactor = loadFactor;
        this.threshold = tableSizeFor(initialCapacity);
    }

    /**
     * Constructs an empty <tt>HashMap</tt> with the specified initial
     * capacity and the default load factor (0.75).
     *
     * @param  initialCapacity the initial capacity.
     * @throws IllegalArgumentException if the initial capacity is negative.
     */
    public HashMap(int initialCapacity) {
        this(initialCapacity, DEFAULT_LOAD_FACTOR);
    }

    /**
     * Constructs an empty <tt>HashMap</tt> with the default initial capacity
     * (16) and the default load factor (0.75).
     */
    public HashMap() {
        this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
    }

    /**
     * Constructs a new <tt>HashMap</tt> with the same mappings as the
     * specified <tt>Map</tt>.  The <tt>HashMap</tt> is created with
     * default load factor (0.75) and an initial capacity sufficient to
     * hold the mappings in the specified <tt>Map</tt>.
     *
     * @param   m the map whose mappings are to be placed in this map
     * @throws  NullPointerException if the specified map is null
     */
    public HashMap(Map<? extends K, ? extends V> m) {
        this.loadFactor = DEFAULT_LOAD_FACTOR;
        putMapEntries(m, false);
    }
```

如果不指定初始容量及负载因子，默认的初始容量为16， 负载因子为0.75。

负载因子衡量的是一个散列表的空间的使用程度，负载因子越大表示散列表的装填程度越高，反之愈小。对于使用链表法的散列表来说，查找一个元素的平均时间是O(1+a)，因此如果负载因子越大，对空间的利用更充分，然而后果是查找效率的降低；如果负载因子太小，那么散列表的数据将过于稀疏，对空间造成严重浪费。

HashMap有一个容量阈值属性threshold，是根据初始容量和负载因子计算得出threshold=capacity*loadfactor， 如果HashMap中数组元素的个数超过这个阈值，则HashMap会进行扩容。HashMap底层的数组长度总是2的n次方，每次扩容容量为原来的2倍。扩容的目的是为了减少hash冲突，提高查询效率。而在HashMap数组扩容之后，最消耗性能的点就出现了：原数组中的数据必须重新计算其在新数组中的位置，并放进去，这就是resize。

##### 3. 数据的存储

```
    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }

    /**
     * Implements Map.put and related methods
     *
     * @param hash hash for key
     * @param key the key
     * @param value the value to put
     * @param onlyIfAbsent if true, don't change existing value
     * @param evict if false, the table is in creation mode.
     * @return previous value, or null if none
     */
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        //初始化数组的大小为16，容量阈值为16*0.75=12
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        //如果key的hash值对应的数组位置没有元素，则新建Node放入此位置
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```

从上面的源代码中可以看出：当我们往HashMap中put元素的时候，先根据key的hashCode重新计算hash值，根据hash值得到这个元素在数组中的位置（即下标），如果数组该位置上已经存放有其他元素了，那么在这个位置上的元素将以链表的形式存放，新加入的放在链头，最先加入的放在链尾。如果数组该位置上没有元素，就直接将该元素放到此数组中的该位置上。

关于HashMap数据的读取这里就不作介绍了。

##### 4. HashMap内存占用

这里分析一个只有一组键值对的HashMap, 结构如下：

```
    Map<Integer, Integer> testMap = Maps.newHashMap();
    testMap.put(1, 2);
```

首先分析HashMap本身的大小。HashMap对象拥有的属性包括：

```
    /**
     * The table, initialized on first use, and resized as
     * necessary. When allocated, length is always a power of two.
     * (We also tolerate length zero in some operations to allow
     * bootstrapping mechanics that are currently not needed.)
     */
    transient Node<K,V>[] table;

    /**
     * Holds cached entrySet(). Note that AbstractMap fields are used
     * for keySet() and values().
     */
    transient Set<Map.Entry<K,V>> entrySet;

    /**
     * The number of key-value mappings contained in this map.
     */
    transient int size;

    /**
     * The number of times this HashMap has been structurally modified
     * Structural modifications are those that change the number of mappings in
     * the HashMap or otherwise modify its internal structure (e.g.,
     * rehash).  This field is used to make iterators on Collection-views of
     * the HashMap fail-fast.  (See ConcurrentModificationException).
     */
    transient int modCount;

    /**
     * The next size value at which to resize (capacity * load factor).
     *
     * @serial
     */
    // (The javadoc description is true upon serialization.
    // Additionally, if the table array has not been allocated, this
    // field holds the initial array capacity, or zero signifying
    // DEFAULT_INITIAL_CAPACITY.)
    int threshold;

    /**
     * The load factor for the hash table.
     *
     * @serial
     */
    final float loadFactor;
```

HashMap继承了AbstractMap<K,V>, AbstractMap有两个属性：

```
     transient Set<K>        keySet;
     transient Collection<V> values;
```

所以一个HashMap对象本身的大小为：

> 12(header) + 4(table reference) + 4(entrySet reference) + 4(size) + 4(modCount) + 4(threshold) + 8(loadFactor) + 4(keySet reference) + 4(values reference) = 48(bytes)

接着分析testMap实例在总共占用的内存大小。根据上面对HashMap原理的介绍，可知每对键值对对应一个Node对象。根据上面的Node的数据结构，一个Node对象的大小为：

> 12(header) + 4(hash reference) + 4(key reference) + 4(value reference) + 4(next pointer reference) = 28 (padding) -> 32(bytes)

加上Key和Value两个Integer对象，一个Node占用内存总大小为：32 + 2 * 16 = 64(bytes)

JProfiler中结果：

![img](https://segmentfault.com/img/bVDmzg?w=706&h=249)

下面分析HashMap的Node数组的大小。根据上面HashMap的原理可知，在不指定容量大小的情况下，HashMap初始容量为16，所以testMap的Node[]占用的内存大小为：

> 16(header) + 16 * 4(Node reference) + 64(Node) = 144(bytes)

JProfile结果：

![img](https://segmentfault.com/img/bVDmAh?w=734&h=268)

所以，testMap占用的内存总大小为：

> 48(map itself) + 144(Node[]) = 192(bytes)

JProfile结果：

![img](https://segmentfault.com/img/bVDmAG?w=730&h=345)

这里只用一个例子说明如何对HashMap进行占用内存大小的计算，根据HashMap初始化容量的大小，以及扩容的影响，HashMap占用内存大小要进行具体分析，不过思路都是一致的。

## 总结

以上对计算Java对象占用内存的基本规则及方法进行了介绍，并通过分析枚举类，ArrayList, HashMap的内存占用情况介绍了分析复杂对象内存占用的基本方法，实际工作中需要精确计算Java对象内存占用情况的场景不多，不过保持一个良好的开发规范和约束是有必要的，而且对内存敏感的手机及用户来说，这是一个值得关注的问题，了解这方面内容还是必不可少的。