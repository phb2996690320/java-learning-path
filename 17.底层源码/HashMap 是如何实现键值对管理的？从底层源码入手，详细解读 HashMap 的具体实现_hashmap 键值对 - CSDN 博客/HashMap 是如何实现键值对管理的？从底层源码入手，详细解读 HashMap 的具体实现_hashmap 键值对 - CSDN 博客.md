---
url: https://blog.csdn.net/qq_40991313/article/details/142096292
title: HashMap 是如何实现键值对管理的？从底层源码入手，详细解读 HashMap 的具体实现_hashmap 键值对 - CSDN 博客
date: 2025-02-20 13:59:27
tag: 
summary: 
---
**导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/126646289 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析-CSDN博客")

**目录**

[一、基本介绍](#t0)

[1.1 集合和映射](#t1)

[1.1.1 基本介绍](#t2)

[1.1.2 思考：Map 是不是集合？](#t3)

[1.2 HashMap 基本介绍](#t4)

[1.3 链地址法处理哈希表冲突](#t5)

[二、底层源码](#t6)

[2.1 继承关系](#t7)

[2.1.1 基本介绍](#t8)

[2.1.2 AbstractMap 类](#t9) 

[2.1.3 思考：HashMap 继承了 AbstractMap，为什么还要实现 Map 接口？](#t10) 

[2.2 核心字段](#t11)

[2.2.1 重要字段](#t12) 

[2.2.1.1 基本介绍](#2.2.1.1%20%E5%9F%BA%E6%9C%AC%E4%BB%8B%E7%BB%8D)

[2.2.1.2 具体代码](#2.2.1.2%20%E5%85%B7%E4%BD%93%E4%BB%A3%E7%A0%81)

[2.2.2 思考：为什么 table 用 transient 修饰？](#t13)

[2.2.2.1 使用 transient 的原因](#2.2.2.1%20%E4%BD%BF%E7%94%A8transient%E7%9A%84%E5%8E%9F%E5%9B%A0)

[2.2.2.2 writeObject()：序列化](#2.2.2.2%C2%A0writeObject%28%29%EF%BC%9A%E5%BA%8F%E5%88%97%E5%8C%96)

[2.2.2.3 序列化原理](#2.2.2.3%C2%A0%E5%BA%8F%E5%88%97%E5%8C%96%E5%8E%9F%E7%90%86%C2%A0) 

[2.2.3 六个常量](#t14)

[2.2.3.1 基本介绍](#2.2.2.1%20%E5%9F%BA%E6%9C%AC%E4%BB%8B%E7%BB%8D)

[2.2.3.2 具体代码](#2.2.2.2%20%E5%85%B7%E4%BD%93%E4%BB%A3%E7%A0%81%C2%A0) 

[2.3 构造方法](#t15)

[2.3.1 基本介绍](#t16)

[2.3.2 思考：为什么阿里规约让用 Maps.hashMap() 而非 new HashMap()？](#t17)

[2.4 tableSizeFor()：获取下次扩容值](#t18)

[2.4.1 基本介绍](#t19)

[2.4.2 思考：与运算和取余的关系](#t20)

[2.4.3 思考：HashMap 容量为什么是 2 的 n 次方？](#t21)

[2.5 存储](#t22)

[2.5.1 put()：存储键值对](#t23)

[2.5.2 putVal()：实际存储键值对](#t24)

[2.5.3 resize()：初始化或扩容](#t25)

[2.5.4 思考：为什么 (e.hash & oldCap) == 0 时扩容索引值不变？](#t26)

[2.5.5 treeifyBin：链表转红黑树](#t27)

[2.5.6 TreeNode 类：树节点](#t28)

[2.5.7 treeify：树链表转红黑树](#t29)

[2.6 获取](#t30)

[2.6.1 get()：获取元素](#t31)

[2.6.2 getNode()：实际获取节点](#t32)

[2.7 删除](#t33)

[2.7.1 remove()：删除元素](#t34)

[2.7.2 removeNode()：实际删除节点](#t35)

## 一、基本介绍

### 1.1 集合和映射

#### 1.1.1 基本介绍

在 Java 中，集合是一组用于操作和存储数据的接口和类。 它主要包括 Collection 和 Map 两种。

*   **集合（Collection）：**一组单独的元素。它通常应用了某种规则，例如 List（列表）必须按特定的顺序容纳元素，而一个 Set（集）不可包含任何重复的元素。
*   **映射（Map）：**一系列 “键－值” 对的集合。它的存储内容是一系列键值对，如果知道了键（key），我们可以直接获取到这个键所对应的值（value），时间复杂度是 O(1)。散列表是 Map 的一种较为普遍的展现。

#### 1.1.2 **思考：Map 是不是集合？**

对于集合一直有个争议，一些人认为集合是个狭窄的概念，只包括 Collection 接口下的实现类，毕竟 Collection 就译为 “集合”。因为 Map 不是 Collection 接口的实现类，所以 Map 不属于集合。

另一些人认为，集合是个广泛的概念，这些存储各种数据、对象及其关系的容器都称为集合。所以 Map 和 Collection 统称为集合。

事实上，两种说法都可以说是对的，区别仅仅在于 “集合” 这个概念的理解区别、广义和狭义。

**《Java 编程思想》原文：**

Java 实用类库还提供了一套相当完整的容器类来解决这个问题，其中基本的类型是 List、  
Set、Queue 和 Map。这些对象类型也称为集合类，但由于 Java 的类库中使用了 Collection 这个名字来指代该类库的一个特殊子集，所以我使用了范围更广的术语 “容器” 称呼它们。容器提供了完善的方法来保存对象，你可以使用这些工具来解决数量惊人的问题。

### 1.2 HashMap 基本介绍

**使用场景**: 适用于需要基于键值对快速查找数据的场景。“键” 可以理解为钥匙，通过这个钥匙，可以找到它唯一对应的 “值”。

**底层**: 哈希表（JDK7 数组 + 链表，JDK8 数组 + 链表 + 红黑树）。

**性能**:

*   **查询性能**: 快，时间复杂度为 O(1)。
*   **添加性能**: 快，时间复杂度为 O(1)。
*   **删除性能**: 快，时间复杂度为 O(1)。

**是否允许 null**:

*   键可以为 null（但最多一个键为 null）。
*   值可以为 null。

**常用方法：**

*   **put()：**向映射中添加一个键值对。如果键已经存在，则更新其对应的值。
*   **get()：**根据键获取对应的值。
*   **getOrDefault()：**获取指定 key 对应对 value，如果找不到 key ，则返回设置的默认值
*   **keySet()：**返回所有 key 的 Set 集合。
*   **remove(Object key):** 根据键移除键值对。
*   **containsKey(Object key):** 检查是否包含指定键。
*   **containsValue(Object value):** 检查是否包含指定值。
*   **size():** 返回映射中的键值对数量。
*   **isEmpty():** 检查映射是否为空。
*   **clear():** 移除映射中的所有键值对。

### 1.3 链地址法处理哈希表冲突

HashMap 底层是哈希表，采用链地址法处理冲突。

在调用 put() 方法存储数据时，它会将 key 通过 key% 容量的方式计算下标，如果对应下标已经存在 value，则采用头插法（JDK7）或者尾插法（JDK8），将这个 value 插入到这个下标对应的链表里。

![](<assets/1740031167925.png>)

## 二、底层源码

### 2.1 继承关系

#### 2.1.1 基本介绍

HashMap 类继承了 AbstractMap 类，实现了 Map、Cloneable、Serializable 接口：

![](<assets/1740031168181.png>)

#### 2.1.2 AbstractMap 类 

AbstractMap 类是 java.util 包下的一个抽象类，它是 Map 接口的实现类，实现了 Map 接口的一部分通用方法。

*   **抽象方法**：**entrySet()** 是 AbstractMap 中**唯一**的抽象方法，子类必须实现它，用于获取 map 中的元素集合。Map 里的每个元素是一个 entry 键值对。
*   **具体方法**：size()、isEmpty()、get()、containsKey()、remove() 等基础方法。

```
/**
 * @Author: vince
 * @CreateTime: 2024/09/24
 * @Description: 抽象map类，实现了get()、size()等基本方法
 * @Version: 1.0
 */
public class AbstractMap {
    /**
     * 获取map中的元素：Map里的每个元素是一个entry键值对
     * AbstractMap中唯一一个抽象方法
     * @return {@link Set }<{@link Map.Entry }<{@link K },{@link V }>>
     */
    public abstract Set<Map.Entry<K,V>> entrySet();
    /**
     * 获取map中的元素个数
     * @return int
     */
    public int size() {
        // entrySet()的逻辑由子类实现
        return entrySet().size();
    }
 
 
    /**
     * 获取map是否为空
     * @return boolean
     */
    public boolean isEmpty() {
        // 简单直接，比较元素个数是否是0
        return size() == 0;
    }
 
    /**
     * 判断map的value中是否包含指定元素
     * @param value 要查找的值
     * @return boolean 如果找到返回true，否则返回false
     */
    public boolean containsValue(Object value) {
        // 1.获取迭代器
        Iterator<Map.Entry<K,V>> i = entrySet().iterator();
        // 2.遍历map查找元素
        if (value == null) {
            // 2.1如果要查找的值为null，遍历查找map中是否有value为null的项
            while (i.hasNext()) {
                Map.Entry<K,V> e = i.next();
                // 如果找到null，返回true
                if (e.getValue() == null)
                    return true;
            }
        } else {
            // 2.2 否则遍历查找map中是否有value等于指定值的项
            while (i.hasNext()) {
                Map.Entry<K,V> e = i.next();
                // 如果找到匹配的值，返回true
                if (value.equals(e.getValue()))
                    return true;
            }
        }
        // 3.如果遍历完未找到匹配的值，返回false
        return false;
    }
 
    /**
     * 添加
     * @param key
     * @param value
     * @return {@link V }
     */
    public V put(K key, V value) {
        // AbstractMap 不提供此方法的实现，子类可以选择实现这个方法（非抽象）。
        throw new UnsupportedOperationException();
    }
 
    // ...
}
```

#### 2.1.3 **思考：**HashMap 继承了 AbstractMap，为什么还要实现 Map 接口？ 

**问题：**

从语法上来说，HashMap 的继承了 AbstractMap，AbstractMap 实现了 Map 了接口，那么看上去 HashMap 没必要同时继承实现 AbstractMap 和 Map。

**答案：**

没有任何意义。是的，你没有听错，**没有任何意义**，这不是我说的，是 HashMap 的作者 **Josh Bloch** 说的。

毕竟 AbstractMap 已经拥有了 Map 接口的所有抽象类，这些方法 HashMap 直接从 AbstractMap 中就能找到，性能也不会有任何帮助。唯一可以牵强点说的，就是提高了**一点点可读性**，让你第一时间能看到 HashMap 的根接口是 Map。

倒推一下，如果有意义的话，这世界所有代码，只要涉及到继承抽象类，都得同时实现抽象类实现的接口，然而除了 Josh Bloch 写的这些集合类之外，并没有多少人这样用。

HashMap 作者 Josh Bloch

![](<assets/1740031168409.png>)

![](<assets/1740031168660.png>)

**出处**：

[java - Why does LinkedHashSet<E> extend HashSet<e> and implement Set<E> - Stack Overflow](https://stackoverflow.com/questions/2165204/why-does-linkedhashsete-extend-hashsete-and-implement-sete "java - Why does LinkedHashSet<E> extend HashSet<e> and implement Set<E> - Stack Overflow")

大致意思是，**HashMap 的作者 Josh Bloch**（同样是 Map、AbstractMap、Set、HashSet、**LinkedHashSet** 的作者）说过这是一个使用错误，不过 JDK 的开发人员认为没必要专门为了这个去掉。

I've asked Josh Bloch, and he informs me that it was a mistake. He used to think, long ago, that there was some value in it, but he since"saw the light". Clearly JDK maintainers haven't considered this to be worth backing out later.

### 2.2 核心字段

HashMap 中有以下重要字段。

#### 2.2.1 重要字段 

##### 2.2.1.1 基本介绍

*   **table：桶数组**
*   **entrySet：键值对集合**
*   **loadFactor：负载因子**。当数组容量到达负载因子时自动扩容
*   **modCount：被修改次数**：本 HashMap 累计被修改次数
*   **size：元素个数**。本 HashMap 累计被修改次数
*   **threshold：扩容阈值**：当元素个数 (size) 超过临界值 (threshold) 时就会自动扩容。它的值总是 2 的幂次方

##### 2.2.1.2 具体代码

```
/**
     * 序列化版本
     */
    private static final long serialVersionUID = 362498820763181265L;
    /**
     * 桶数组：存储元素的数组：初始容量是16，当实际容量到达负载因子时，会以2的指数扩容，所以容量总是2的幂次倍
     * transient：表示该字段不会被序列化。这个修饰符是为了提高性能。因为数组中有很多空元素，这些元素被序列化没有意义；
     * 实际上，Java中集合里的底层数组基本都用transient修饰，例如ArrayList的elementData等等
     */
    transient java.util.HashMap.Node<K,V>[] table;
    /**
     * map中的元素集合
     * AbstractMap 有个唯一的抽象方法entrySet()，子类必须实现它，用于获取获取map中的元素集合。
     * Map里的每个元素是一个Entry键值对。
     */
    transient Set<Entry<K,V>> entrySet;
    /**
     * 负载因子：当数组容量到达负载因子时自动扩容
     */
    final float loadFactor;
 
    /**
     * 被修改次数：本HashMap累计被修改次数
     */
    transient int modCount;
    /**
     * 容量：元素个数。注意不是数组的长度，而是跟数组、String、集合的size()方法意义一样
     */
    transient int size;
    /**
     * 扩容阈值：当元素个数(size)超过临界值(threshold)时就会自动扩容。它的值总是2的幂次方
     * threshold在构造方法最后一步，通过tableSizeFor()方法计算，计算规则是大于等于构造参数初始容量的第一个2的幂次方
     * 当HashMap中的元素个数超过threshold时（例如putVal()存储结束后，会校验给size加1后会不会大于threshold，如果大于则调用resize()扩容），会触发扩容机制
     */
    int threshold;
```

#### 2.2.2 思考：为什么 table 用 transient 修饰？

##### 2.2.2.1 使用 transient 的原因

首先，transient 是什么？被 transient 修饰字段不会被序列化。

**原因**：

*   **避免冗余**：为了保证所有元素不被重复序列化，HashMap 声明了 **writeObject()** 和 **readObject()** 用于专门序列化，
*   **提高性能**：因为数组中有很多空元素，这些元素一个个被序列化没有意义，肯定没有统一遍历，只序列化已存在的节点效率高。

事实上，如果你阅读过很多源码，你会发现，JDK 中很多集合的底层数组都用 transient 关键字修饰。例如 ArrayList 的 elementData 等，HashSet 的 map 字段，LinkedList 的 first 和 last 字段。

![](<assets/1740031168976.png>)

![](<assets/1740031169226.png>)

##### 2.2.2.2 **writeObject()：序列化**

**核心流程：**

1.  **序列化非瞬态字段**：调用 ObjectOutputStream 的默认序列化方法，它会序列化 HashMap 的没有被 **transient** 修饰的字段（如 loadFactor 等元数据）
2.  **序列化数组长度**：将桶数组 **table** 长度写入序列化流中
3.  **序列化元素数量**：将元素个数 **size** 写入序列化流中
4.  **序列化所有元素**：遍历所有键值对，序列化写入流中

**具体代码：** 

```
/**
     * 序列化
     * @param s 对象输出流，用于序列化
     * @throws IOException
     */
    private void writeObject(java.io.ObjectOutputStream s)
            throws IOException {
        // 获取桶数组长度
        int buckets = capacity();
        // 1.序列化非瞬态字段：调用 ObjectOutputStream 的默认序列化方法，它会序列化 HashMap 的没有被transient修饰的字段（如 loadFactor 等元数据）
        s.defaultWriteObject();
 
        // 2.将桶数组长度写入序列化流中
        s.writeInt(buckets);
 
        // 3.将元素个数写入序列化流中
        s.writeInt(size);
 
        // 3.遍历所有键值对，序列化写入流中
        internalWriteEntries(s);
    }
    /**
     * 遍历所有键值对，序列化写入流中
     * @param s 对象输出流
     * @throws IOException IO异常
     */
    void internalWriteEntries(java.io.ObjectOutputStream s) throws IOException {
        Node<K,V>[] tab; 
 
        // 检查 HashMap 是否有元素以及 table 是否不为 null
        if (size > 0 && (tab = table) != null) {
            // 遍历桶数组table中的每个桶
            for (int i = 0; i < tab.length; ++i) {
                // 从当前桶的头节点开始遍历
                for (Node<K,V> e = tab[i]; e != null; e = e.next) {
                    // 将每个节点的key和value写入序列化流
                    s.writeObject(e.key);
                    s.writeObject(e.value);
                }
            }
        }
    }
```

##### 2.2.2.3 序列化原理 

**序列化原理**：在 ObjectOutputStream 中进行序列化操作的时候，会判断被序列化的对象是否自己重写了 writeObject 方法**，如果重写了，就会调用被序列化对象自己的 writeObject 方法**，如果没有重写，才会调用默认的序列化方法。

```
private void writeSerialData(Object obj, ObjectStreamClass desc)
    throws IOException {
    ObjectStreamClass.ClassDataSlot[] slots = desc.getClassDataLayout();
    for (int i = 0; i < slots.length; i++) {
        ObjectStreamClass slotDesc = slots[i].desc;
        if (slotDesc.hasWriteObjectMethod()) {
            //如果重写了writeObject方法
            PutFieldImpl oldPut = curPut;
            curPut = null;
            SerialCallbackContext oldContext = curContext;
            try {
                curContext = new SerialCallbackContext(obj, slotDesc);
                bout.setBlockDataMode(true);
                //调用实现类自己的writeobject方法
                slotDesc.invokeWriteObject(obj, this);  
                bout.setBlockDataMode(false);
                bout.writeByte(TC_ENDBLOCKDATA);
            } finally {
                //省略
            }
            curPut = oldPut;
        } else {
            defaultWriteFields(obj, slotDesc);
        }
    }
    }
```

```
/**
     * 默认负载因子：75%，即当数组容量到达75%时自动扩容
     */
    static final float DEFAULT_LOAD_FACTOR = 0.75f;
 
    /**
     * 默认的初始容量：16
     */
    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4;
    /**
     * 最大初始容量：2的30次方
     */
    static final int MAXIMUM_CAPACITY = 1 << 30;
    /**
     * 桶数组最小树化容量：数组最小树化容量，默认64。在桶数组容量>=此变量的条件下，当某个链表长度到达8时，该链表树化
     */
    static final int MIN_TREEIFY_CAPACITY = 64;
 
    /**
     * 转红黑树阈值：即链表长度到达8时，自动转为红黑树
     */
    static final int TREEIFY_THRESHOLD = 8;
    /**
     * 转链表阈值：当红黑树上的结点数小于6时，树转为链表
     * 当节点数少时，链表效率高；当节点数多时，红黑树效率高；
     * 链表查询时间复杂度O(n)，红黑树查询时间复杂度O(logN)
     */
    static final int UNTREEIFY_THRESHOLD = 6;
```

#### 2.2.3 六个常量

##### 2.2.3.1 基本介绍

*   **默认负载因子**：75%
*   **默认的初始容量**：16
*   **最大初始容量**：2^30
*   **桶数组最小树化容量**：64
*   **转红黑树阈值**：8
*   **转链表阈值**：6

##### 2.2.3.2 具体代码 

```
/**
     * 默认构造函数。
     */
    public HashMap() {
        // 设置负载因子为默认负载因子75%
        this.loadFactor = DEFAULT_LOAD_FACTOR;
    }
 
    /**
     * 包含另一个“Map”的构造函数
     * @param m 另一个“Map”
     */
    public HashMap(Map<? extends K, ? extends V> m) {
        // 1.初始化负载因子
        this.loadFactor = DEFAULT_LOAD_FACTOR;
        // 2.将传入的 Map 的所有元素添加到当前 HashMap 中
        putMapEntries(m, false);
    }
 
    /**
     * 指定“容量大小”的构造函数
     * @param initialCapacity
     */
    public HashMap(int initialCapacity) {
        //
        this(initialCapacity, DEFAULT_LOAD_FACTOR);
    }
 
    /**
     * 指定“容量大小”和“负载因子”的构造函数
     * @param initialCapacity 初始容量大小
     * @param loadFactor 负载因子
     */
    public HashMap(int initialCapacity, float loadFactor) {
        // 1.校验初始容量和负载因子
        // 1.1.校验初始容量：必须是正数，并且小于最大允许容量
        // 如果初始容量为负数，则抛出异常
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " + initialCapacity);
 
        // 如果初始容量大于最大容量，则将其设为最大容量
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
 
        // 1.2.校验负载因子：必须是一个有效的正数
        // 如果负载因子小于等于0或不是一个有效的数字（NaN），则抛出异常
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " + loadFactor);
 
 
        // 3.赋值负载因子：将传入的负载因子赋值给类的成员变量
        this.loadFactor = loadFactor;
 
        // 4.设置阈值：根据参数初始容量，计算下次扩容值，作为阈值
        // 计算规则：大于等于参数的第一个2的幂次方
        this.threshold = tableSizeFor(initialCapacity);
    }
```

### 2.3 构造方法

#### 2.3.1 基本介绍

HashMap 共有 4 个构造方法，参数分别是：

*   **空参构造方法**： 初始化负载因子 loadFactor 为 75%。
*   **参数为 map**：
    1.  初始化负载因子为 75%
    2.  将参数 map 中的元素全添加到当前 map
*   **参数为容量**：设置负载因子 75%，传参调用下面构造方法
*   **参数为容量 + 负载因子**：
    1.  **校验初始容量和负载因子**：
        1.  校验初始容量：必须是正数，并且小于最大允许容量
        2.  校验负载因子：必须是一个有效的正数
    2.  **赋值负载因子**：将传入的负载因子赋值给类的成员变量
    3.  **设置阈值**：根据参数初始容量，计算下次扩容值，作为阈值 threshold。

```
/**
     * 计算出大于等于参数的第一个2的幂次方
     * 例如：1返回1，3返回4（即2^2），4返回4,5返回8（即2^3），8返回8，9返回16，125返回128，
     * 如果参数大于默认最大值，则容量取默认最大值。
     *
     * @param cap 初始容量
     * @return 大于等于 cap 的最小 2 的幂次方值
     */
    static final int tableSizeFor(int cap) {
        int n = cap - 1;
        // 下面的位运算是为了把 n 的最高位 1 右边所有位都变成 1
        // 右移 1 位后做按位或运算
        n |= n >>> 1;
        // 右移 2 位后做按位或运算
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        // 如果 n 小于 0 则返回 1，否则返回 n + 1，但不能超过最大容量 MAXIMUM_CAPACITY
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
```

#### 2.3.2 思考：为什么阿里规约让用 Maps.hashMap() 而非 new HashMap()？

**问题演示：**

首先我们在代码中加上两种创建 HashMap 的方式：

### 2.4 tableSizeFor()：获取下次扩容值

#### 2.4.1 基本介绍

tableSizeFor() 使用了位运算，根据传入的参数容量，计算出大于等于参数的第一个 2 的幂次方。

**例如：**1 返回 1，2 返回 2（即 2^1），3 返回 4（即 2^2），4 返回 4,5 返回 8（即 2^3），8 返回 8，9 返回 16，125 返回 128，

```
/**
 * @Author: vince
 * @CreateTime: 2024/09/10
 * @Description: 测试类
 * @Version: 1.0
 */
public class Test2 {
    static final int tableSizeFor(int cap) {
        int n = cap - 1;
        // 下面的位运算是为了把 n 的最高位 1 右边所有位都变成 1
        // 右移 1 位后做按位或运算
        n |= n >>> 1;
        // 右移 2 位后做按位或运算
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        // 如果 n 小于 0 则返回 1，否则返回 n + 1，但不能超过最大容量 MAXIMUM_CAPACITY
        return (n < 0) ? 1 : n + 1;
    }
    public static void main(String[] args) {
        for (int i = 0; i < 100; i++) {
            System.out.println("比"+i+"大于等于的第一个2的n次幂："+tableSizeFor(i));
        }
    }
}
```

代码示例：

大家也可以在本地自己验证 

```
// 所有情况都适用
x & (n - 1) == (n - 1) & x
// 当n是2的幂次方时
x % n == x & (n - 1)
```

![](<assets/1740031169404.png>)

#### 2.4.2 思考：与运算和取余的关系

通常情况下，与运算和取余没关系，但是：

**当 n 是一个 2 的幂次方（如 1, 2, 4, 8, 16 等）时，x % n 等价于 x & (n - 1)**

```
/**
     * 存储键值对
     * @param key 要插入或更新的键
     * @param value 要插入或更新的值
     * @return {@link V} 返回与 key 关联的旧值，如果没有旧值则返回 null
     */
    public V put(K key, V value) {
        // 计算哈希值后，调用 putVal 方法，将键值对放入 HashMap 中
        return putVal(hash(key), key, value, false, true);
    }
```

**示例**：

x 为 10，n 为 8（即 2^3），则

*   **取余：**10 % 8 = 2
*   **与运算：**10 & 7 = 10 & 0111 （二进制公式）= 0010 （二进制）= 2 

这也是 HashMap 每次扩容都是 2 的幂次方的原因，因为哈希表取索引地址 key%n 需要取余， 如果每次扩容都让 n 是 2 的幂次方，我们就可以用与运算替换取余，用移位运算替换扩容时乘 2，我们知道，**位运算的性能是远高于算术运算的**。因为它直接在位级别进行操作，而不需要进行除法运算。

**位运算和算术运算**：

*   **位运算**（如与、或、异或、非、左移、右移等）：通常由 CPU 直接支持，并且是在硬件层面操作二进制（计算机存储数据就是用二进制的），不需要额外的转换速度非常快。
*   算术运算：加法减法底层用加法器实现的，从最低位开始相加，计算当前位的和以及进位。乘法使用乘法移位法，除法使用 “恢复除法” 和“非恢复除法”，性能都差于位运算

![](<assets/1740031169660.png>)

#### 2.4.3 **思考：HashMap 容量为什么是 2 的 n 次方？**

**主要有以下两个原因：**

*   **提高性能**：当 n 是 2 的幂次方时，散列公式 (n - 1) & hash 的结果等于 hash%n。位运算比取余快，扩容时只需左移一位。
    *   例如 2^n-1 和 2^(n+1)-1 的二进制除了第一位，后几位都是相同的。扩容后只需要左移一次。例如 15 的二进制为 1111，31 的二进制为 11111，63 的二进制为 111111，127 的二进制为 1111111。
*   **使元素均匀分布在底层数组**：以 2 的指数扩容，可以保证每次扩容后，索引值改变的元素都移动到新增加的位置，而不会再移动到旧数组上的位置，可以减少哈希碰撞。

**例如：**从 2^4 扩容成 2^5，取余公式 key&(2^n-1)=index，index 是要存储在数组的下标。

0&(2^4-1)=0；0&(2^5-1)=0

16&(2^4-1)=0；16&(2^5-1)=16。所以扩容后，key 为 0 的一部分 value 位置没变，一部分 value 迁移到扩容后的新位置。

1&(2^4-1)=1；1&(2^5-1)=1

17&(2^4-1)=1；17&(2^5-1)=17。所以扩容后，key 为 1 的一部分 value 位置没变，一部分 value 迁移到扩容后的新位置。

### 2.5 存储

#### 2.5.1 put()：存储键值对

put() 方法作用是将 key 和 value 存储到 HashMap 中，它只有一行代码，就是调用 putVal() 这个 final 方法，实际存储键值对，类似于 Spring 源码中的 doGetBean、doCreateBean() 等。

**核心逻辑**：

1.  **调用 putVal() 方法**：计算哈希值后，调用 putVal 方法，将键值对放入 HashMap 中

**具体代码：** 

```
/**
     * 实际存储键值对
     * @param hash key 的 hash 值
     * @param key 要插入或更新的键
     * @param value 要插入或更新的值
     * @param onlyIfAbsent 是否仅在 key 不存在时插入新值
     * @param evict 是否在创建新节点后执行逐出操作
     * @return {@link V} 返回与 key 关联的旧值，如果没有旧值则返回 null
     */
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        java.util.HashMap.Node<K,V>[] tab; java.util.HashMap.Node<K,V> p; int n, i;
        // 1.校验扩容：如果表未初始化或长度为0，执行扩容。所以HashMap是在第一次插入时初始化，而非构造方法时初始化
        // 如果表未初始化或长度为0，执行扩容
        // 这里可以看到，HashMap底层的桶数组table，是在插入时初始化，而非在构造方法中初始化
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        // 2.计算索引位置并存储：索引=(n - 1) & hash。用与运算而非取余的原因：当n是2的幂次方时，散列公式(n - 1) & hash的结果等于hash % n，位运算比取余快，扩容时只需左移一位。
        // 2.1 计算索引位置，计算公式是: (n - 1) & hash。
        // 当n是2的幂次方时，散列公式(n - 1) & hash的结果等于hash % n，位运算比取余快，扩容时只需左移一位。
        if ((p = tab[i = (n - 1) & hash]) == null)
            // 2.1 空节点直接创建链表：如果该位置为空，则创建链表新节点
            tab[i] = newNode(hash, key, value, null);
        else {
            // 2.2 插入数据：如果该位置已有节点，则尾插法给链表或红黑树中添加数据
            java.util.HashMap.Node<K,V> e; K k;
            // 2.2.1 链表首节点直接替换：如果链表第一个节点就是要插入的节点（判断依据是哈希值和key都一致），则用e（用于标记待替换元素位置）指向第一个节点
            if (p.hash == hash &&
                    ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof java.util.HashMap.TreeNode)
                // 2.2.2 红黑树插入：如果第一个节点是树节点，则调用红黑树的插入方法putTreeVal()：
                // 如果要插入的值在红黑树不存在，则插入
                // 如果要插入的值在红黑树已存在，则不插入并用e指向目标位置的节点，等待后面替换
                e = ((java.util.HashMap.TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                // 2.2.3 链表插入：如果第一个节点是链表节点，则插入或标记待覆盖节点
                // 如果要插入的值在链表不存在，则尾插法插入，若链表长度到8，则调用treeifyBin()转红黑树；
                // 如果要插入的值在链表已存在，则不插入并用e指向目标位置的节点，等待后面替换
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        // 若插入后节点数大于等于8，则尝试转换为红黑树
                        if (binCount >= TREEIFY_THRESHOLD - 1)
                            // 注意这里说的是“尝试”转为红黑树
                            // 因为treeifyBin()第一行会校验数组长度，如果数组长度小于64，会扩容数组使链表分散，而不树化
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                            ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    //p记录下一个节点
                    p = e;
                }
            }
            // 2.2.4 覆盖：如果e（前面收集的待替换元素位置）不为空（代表插入成功或者这个key在HashMap中已有元素），则覆盖
            if (e != null) {
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        // 3.累计修改次数加1：modCount（即本HashMap累计被修改次数）加1
        ++modCount;
        // 4.容量加1、判断扩容：如果容量（即元素个数）+1后，大于扩容阈值，则扩容
        if (++size > threshold)
            resize();
        // 5.执行插入后方法：此方法是钩子方法，内容是空，修饰符是default，代表它只能被java.util下的子类重写（例如LinkedHashMap就重写了这个方法）
        afterNodeInsertion(evict);
        return null;
    }
```

#### 2.5.2 putVal()：实际存储键值对

putVal() 是 HashMap 中实际存储元素的方法，类似于 Spring 源码中的 doGetBean、doCreateBean() 等：

putVal() 方法是存储元素的核心方法，它由 final 修饰，代表他不可被子类重写，

另外，它没有加权限修饰符，也就是默认 defalut，同包可见，所以 putVal() 方法只允许 java.util 包下的类调用。

**访问修饰符**：

*   **private** : 当前类内可见。不能修饰类和接口。
*   **default** : 同包可见。
*   **protected** : 同包可见、对其他包下的子类可见。不能修饰类和接口。
*   **public** : 对所有类可见。

final 类不可被继承、final 方法不可被重写、final 变量不可变。

**核心逻辑：**

1.  **校验扩容**： 如果表未初始化或长度为 0，则调用 **resize()** 执行扩容。所以 HashMap 是在第一次插入时初始化，而非构造方法时初始化
2.  **计算索引位置并存储**：索引 =(n - 1) & hash。**用与运算而非取余的原因**：当 n 是 2 的幂次方时，散列公式 (n - 1) & hash 的结果等于 hash % n，位运算比取余快，扩容时只需左移一位。
    1.  **空节点直接创建链表**：如果该位置为空，则创建链表新节点
    2.  **插入：**如果该位置已有节点，则尾插法给链表或红黑树中添加数据
        1.  **链表首节点直接替换**：如果链表第一个节点就是要插入的节点（判断依据是哈希值和 key 都一致），则用 e（用于标记待替换元素位置）指向第一个节点
        2.  **红黑树插入**：如果第一个节点是树节点，则调用红黑树的插入方法 putTreeVal()：
            1.  如果要插入的值在红黑树不存在，则插入
            2.  如果要插入的值在红黑树已存在，则不插入并用 e 指向目标位置的节点，等待后面替换
        3.  **链表插入**：如果第一个节点是链表节点，则插入或标记待覆盖节点
            1.  如果要插入的值在链表不存在，则
                1.  **插入**：尾插法插入
                2.  **树化校验**：若链表长度到 8，则调用 **treeifyBin()** 尝试转红黑树（treeifyBin() 第一行会校验数组长度，如果数组长度小于 64，会扩容数组使链表分散，而不树化）；
            2.  如果要插入的值在链表已存在，则不插入并用 e 指向目标位置的节点，等待后面替换
        4.  **覆盖**：如果 e（前面收集的待替换元素位置）不为空（代表插入成功或者这个 key 在 HashMap 中已有元素），则覆盖
3.  **累计修改次数加 1**：modCount（即本 HashMap 累计被修改次数）加 1
4.  **容量加 1、判断扩容**：如果容量（即元素个数）+1 后，大于扩容阈值，则扩容
5.  **执行插入后方法**：此方法是钩子方法，内容是空，修饰符是 default，代表它只能被 java.util 下的子类重写（例如 LinkedHashMap 就重写了这个方法）

**具体代码：** 

```
final Node<K,V>[] resize() {
    Node<K,V>[] oldTab = table;
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    int oldThr = threshold;
    int newCap, newThr = 0;
    // 如果 table 不为空，表明已经初始化过了
    if (oldCap > 0) {
        // 当 table 容量超过容量最大值，则不再扩容
        if (oldCap >= MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return oldTab;
        } 
        // 按旧容量和阈值的2倍计算新容量和阈值的大小
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                 oldCap >= DEFAULT_INITIAL_CAPACITY)
            newThr = oldThr << 1; // double threshold
    } else if (oldThr > 0) // initial capacity was placed in threshold
        /*
         * 初始化时，将 threshold 的值赋值给 newCap，
         * HashMap 使用 threshold 变量暂时保存 initialCapacity 参数的值
         */ 
        newCap = oldThr;
    else {               // zero initial threshold signifies using defaults
        /*
         * 调用无参构造方法时，桶数组容量为默认容量，
         * 阈值为默认容量与默认负载因子乘积
         */
        newCap = DEFAULT_INITIAL_CAPACITY;
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
    
    // newThr 为 0 时，按阈值计算公式进行计算
    if (newThr == 0) {
        float ft = (float)newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                  (int)ft : Integer.MAX_VALUE);
    }
    threshold = newThr;
    // 创建新的桶数组，桶数组的初始化也是在这里完成的
    Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
    table = newTab;
    if (oldTab != null) {
        // 如果旧的桶数组不为空，则遍历桶数组，并将键值对映射到新的桶数组中
        for (int j = 0; j < oldCap; ++j) {
            Node<K,V> e;
            if ((e = oldTab[j]) != null) {
                oldTab[j] = null;
                if (e.next == null)
                    newTab[e.hash & (newCap - 1)] = e;
                else if (e instanceof TreeNode)
                    // 重新映射时，需要对红黑树进行拆分
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                else { // preserve order
                    Node<K,V> loHead = null, loTail = null;
                    Node<K,V> hiHead = null, hiTail = null;
                    Node<K,V> next;
                    // 遍历链表，并将链表节点按原顺序进行分组
                    do {
                        next = e.next;
                        if ((e.hash & oldCap) == 0) {
                            if (loTail == null)
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        }
                        else {
                            if (hiTail == null)
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    // 将分组后的链表映射到新桶中
                    if (loTail != null) {
                        loTail.next = null;
                        newTab[j] = loHead;
                    }
                    if (hiTail != null) {
                        hiTail.next = null;
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
}
```

#### 2.5.3 resize()：初始化或扩容

在上面 putVal() 方法中，存储元素前后都校验并调用了 resize() 方法，分别是插入前校验是否需要初始化、插入后校验是否需要扩容：

![](<assets/1740031169864.png>)

![](<assets/1740031170138.png>)

**核心逻辑**：

1.  **校验初始化**：
    1.  如果已经初始化过，并且阈值没超过上限，则以 2 的指数扩容并设置新阈值和初始容量；
    2.  如果没有初始化过，则设置初始容量 16，新的阈值 16*0.75
2.  **创建新的桶数组**
3.  **旧元素复制到新数组**：如果旧桶数组不为空，则遍历并重新映射到扩容后的新数组。
    1.  **置空旧元素**：创建节点 e 指向旧数组元素 oldTable[j]，然后置空旧数组该元素；
    2.  **如果是单节点**：则直接放进新桶数组
    3.  **如果是红黑树节点**：则调用 **split()** 方法，拆分红黑树
    4.  **如果是链表节点**：则遍历链表，拆成低位组和高位组，分别存到新数组 newTab[j] 和 newTab[j + oldCap]。两组链表在下一节会详细说
        1.  **低位组 loHead**：重新映射后索引位置不变，还是 table[j]
        2.  **高位组 hiHead**：重新映射后索引位置在新扩容的位置，即 table[j + oldCap]
4.  **返回新数组**

**具体代码**：

```
public static void main(String[] args) {
 
        Map<Integer, Integer> numMap = new HashMap<>();
        for (int i = 0; i < 100000; i++) {
            numMap.put(i,23+i);
        }
        System.out.println(numMap);
    }
```

#### 2.5.4 思考：为什么 (e.hash & oldCap) == 0 时扩容索引值不变？

在上面扩容方法中，第 4 步我们需要将链表拆成低位链表和高位链表，分别存到新数组 newTab[j] 和 newTab[j + oldCap]，即数组原位置、原位置 + 旧数组长度位置。

![](<assets/1740031170429.png>)

为什么 (e.hash & oldCap) == 0 时，扩容迁移数据后，这个节点 e 的索引值不变？

首先，前面说过 HashMap 以 2 的指数扩容，所以 oldCap 一定是 2 的 n 次幂，所以它的二进制有个特点，第一位是 1，后面几位全是 0

**以初始容量 16 为例：**

*   16 二进制：10000
*   随机数 hash 二进制：XXXX1YYYY（其中 X 和 Y 指代任意数），或者 XXXX0YYYY
*   hash&16 二进制：10000，或者 0

**以扩容到 32 为例：**取索引公式 index=hash%(n-1)

*   15 二进制：1111
*   索引位置 hash&(16-1)：都是 0
*   31 二进制：11111
*   索引位置 hash&(32-1)：1YYYY，或者 YYYY

可以看见，YYYY 都小于 16，而 1YYYY 都大于等于 16，并且 1YYYY 正好比 YYYY 大 16，所以我们可以据此判断，**(e.hash & oldCap) == 0 时扩容索引值不变**

**代码示例：**

```
/**
     * 树化：将链表转为红黑树。
     * 如果数组长度小于64，则扩容而不树化，否则树化；
     * 本方法主要是将链表每个节点转为树节点，具体的树链表转红黑树由treeify()执行
     * @param tab HashMap中的桶数组
     * @param hash 待存储元素的哈希值，用于定位链表所在桶的索引
     */
    final void treeifyBin(Node<K,V>[] tab, int hash) {
        int n, index; Node<K,V> e;
        // 1.校验扩容还是树化：如果桶数组容量<64，则不树化而是扩容数组；
        // 扩容数组后，原table[i]链表的部分元素会移动到table[i+oldCap]中
        if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
            resize();
        // 2.树化：将链表上的每个节点转为树节点，然后调用treeify()方法，将树链表转为红黑树
        // 2.1 链表节点转树节点：遍历链表，将链表的每个节点由Node链表节点转为树节点TreeNode
        // TreeNode是树节点，同时也是双向链表，有left、right、prev、next等字段，既是双向链表，又是树节点
        // e指向首元素
        else if ((e = tab[index = (n - 1) & hash]) != null) {
            TreeNode<K,V> hd = null, tl = null;
            // 遍历链表
            do {
                // 根据e，创建一个树节点p
                TreeNode<K,V> p = replacementTreeNode(e, null);
                // 如果这是链表的第一个节点（tail未初始化），则将 hd 指向这个新创建的 TreeNode
                if (tl == null)
                    hd = p;
                else {
                    // 如果tail 已经初始化过，将当前节点 p 连接到双向链表尾部
                    p.prev = tl;
                    tl.next = p;
                }
                // 更新 tail 为当前节点，形成双向链表
                tl = p;
                // 如果e的后继不为空，让e往后移动
            } while ((e = e.next) != null);
            // 现在执行完，只是让原链表元素保持原来顺序全部替换为TreeNode实例，然后将它们之间的前驱后继连起来，成为双向链表
            // 2.2 红黑树化
            if ((tab[index] = hd) != null)
                // 调用treeify()方法，将链表转换为红黑树
                hd.treeify(tab);
        }
    }
```

![](<assets/1740031170751.png>)

![](<assets/1740031171171.png>)

#### 2.5.5 treeifyBin：链表转红黑树

在上面 putVal() 方法中的第二步存储元素时，如果是往链表上存储，在存储完成后，会判断链表长度是否到达 8，如果到了 8，则尝试将链表转为红黑树：

![](<assets/1740031171475.png>)

**链表树化**：指将链表上的每个节点转为树节点。

本方法主要是将链表每个节点转为树节点，具体的树链表转红黑树由 **treeify()** 执行

**核心流程**：

1.  校验扩容还是树化：如果桶数组容量 < 64，则不树化而是扩容数组；
2.  链表转红黑树：将链表上的每个节点转为树节点，然后调用 treeify() 方法，将树链表转为红黑树
    1.  链表树化：链表节点转树节点：遍历链表，将链表的每个节点由 Node 链表节点转为树节点 **TreeNode**（含有 left、right、prev、next 等字段，既是双向链表，又是树节点）；
    2.  树链表转红黑树：调用 **treeify()** 方法，将树链表（每个节点是树节点的链表）转为红黑树

**具体代码**：

```
/**
         * 树链表转换为红黑树
         * 树链表指每个节点是树节点的链表，这个TreeNode树节点既可以做双向链表，也可以做树节点
         * @param tab HashMap 中的桶数组
         */
        final void treeify(Node<K,V>[] tab) {
            // 定义树的根节点
            TreeNode<K,V> root = null;
            // 遍历链表，x指向当前节点、next指向下一个节点
            for (TreeNode<K,V> x = this, next; x != null; x = next) {
                // 下一个节点
                next = (TreeNode<K,V>)x.next;
                // 设置当前节点的左右节点为空
                x.left = x.right = null;
                // 如果还没有根节点
                if (root == null) {
                    // 当前节点的父节点设为空
                    x.parent = null;
                    // 当前节点的红色属性设为false（把当前节点设为黑色）
                    x.red = false;
                    // 根节点指向到当前节点
                    root = x; 
                }
                else { 
                    // 如果已经存在根节点了
                    // 取得当前链表节点的key
                    K k = x.key;
                    // 取得当前链表节点的hash值
                    int h = x.hash;
                    // 定义key所属的Class
                    Class<?> kc = null;
                    // 从根节点开始遍历，此遍历没有设置边界，只能从内部跳出
                    for (TreeNode<K,V> p = root;;) {
                        // dir 标识方向（左右）、ph标识当前树节点的hash值
                        int dir, ph;
                        // 当前树节点的key
                        K pk = p.key;
                        // 如果当前树节点hash值 大于 当前链表节点的hash值
                        if ((ph = p.hash) > h)
                            // 标识当前链表节点会放到当前树节点的左侧
                            dir = -1;
                        else if (ph < h)
                            // 标识右侧
                            dir = 1;
 
                            /*
                             * 如果两个节点的key的hash值相等，那么还要通过其他方式再进行比较
                             * 如果当前链表节点的key实现了comparable接口，并且当前树节点和链表节点是相同Class的实例，那么通过comparable的方式再比较两者。
                             * 如果还是相等，最后再通过tieBreakOrder比较一次
                             */
                        else if ((kc == null &&
                                (kc = comparableClassFor(k)) == null) ||
                                (dir = compareComparables(kc, k, pk)) == 0)
                            dir = tieBreakOrder(k, pk);
                        // 保存当前树节点
                        TreeNode<K,V> xp = p; 
 
                        /*
                         * 如果dir 小于等于0 ： 当前链表节点一定放置在当前树节点的左侧，但不一定是该树节点的左孩子，也可能是左孩子的右孩子 或者 更深层次的节点。
                         * 如果dir 大于0 ： 当前链表节点一定放置在当前树节点的右侧，但不一定是该树节点的右孩子，也可能是右孩子的左孩子 或者 更深层次的节点。
                         * 如果当前树节点不是叶子节点，那么最终会以当前树节点的左孩子或者右孩子 为 起始节点  再从GOTO1 处开始 重新寻找自己（当前链表节点）的位置
                         * 如果当前树节点就是叶子节点，那么根据dir的值，就可以把当前链表节点挂载到当前树节点的左或者右侧了。
                         * 挂载之后，还需要重新把树进行平衡。平衡之后，就可以针对下一个链表节点进行处理了。
                         */
                        if ((p = (dir <= 0) ? p.left : p.right) == null) {
                            // 当前链表节点 作为 当前树节点的子节点
                            x.parent = xp; 
                            if (dir <= 0)
                                // 作为左孩子
                                xp.left = x; 
                            else
                                // 作为右孩子
                                xp.right = x;
                            // 重新平衡
                            root = balanceInsertion(root, x);
                            break;
                        }
                    }
                }
            }
 
            // 把所有的链表节点都遍历完之后，最终构造出来的树可能经历多个平衡操作，根节点目前到底是链表的哪一个节点是不确定的
            // 因为我们要基于树来做查找，所以就应该把 tab[N] 得到的对象一定根节点对象，而目前只是链表的第一个节点对象，所以要做相应的处理。
            moveRootToFront(tab, root); // 单独解析
        }
```

#### 2.5.6 TreeNode 类：树节点

TreeNode 是 HashMap 的内部类，表示红黑树的树节点。

事实上，TreeNode 既可以做双向链表，也可以做树节点，这是因为它既有前驱 prev 字段和后驱 next 字段，也有左孩子和右孩子字段：

![](<assets/1740031171758.png>)

**重要方法：**

*    **treeify()**：树链表转红黑树
*   **balanceInsertion()**：恢复平衡。插入节点后，调用此方法，通过颜色翻转和旋转操作保持红黑树的平衡。
*   **rotateLeft() 和 rotateRight()**：左旋和右旋
*   **moveRootToFront()**：把红黑树的根节点设为**其所在的数组槽**的第一个元素。主要是因为 TreeNode 在增加或删除节点后，都需要对整个树重新进行平衡，平衡之后的根节点可能变了，所以就需要调整树，使根节点保持为该树所在数组槽的第一个元素。

#### 2.5.7 treeify：树链表转红黑树

在上面 **treeifyBin()** 方法的最后一步，调用了 TreeNode 类的 **treeify()** 方法，将每个节点是 TreeNode 类的链表，转为红黑树：  

![](<assets/1740031172020.png>)

**HashMap 底层数据结构：**

*   **JDK7：**HashMap 底层是采用 “数组 + 单向链表” 来实现的。数组用作哈希查找，链表用作链地址法**头插法**处理冲突。
*   **JDK8：**HashMap 底层是采用 “数组 + 单向链表 + **红黑树**” 来实现的。数组用作哈希查找，链表用作链地址法**尾插法**处理冲突，链表长度为 8 时转为红黑树。

```
/**
     * 获取元素
     * @param key 键
     * @return {@link V }
     */
    public V get(Object key) {
        Node<K,V> e;
        // 调用getNode()方法，获取该key所在的节点，返回节点的value
        return (e = getNode(hash(key), key)) == null ? null : e.value;
    }
```

### 2.6 获取

#### 2.6.1 get()：获取元素

HashMap 获取元素的方法是 get()，此方法只有两行，即调用 getNode() 方法，获取该 key 所在的节点，返回节点的 value。

**核心逻辑**：

*   **获取节点返回 value**：调用 getNode() 方法，获取该 key 所在的节点，返回节点的 value

**具体代码**：

```
/**
     * 获取节点
     * @param hash 哈希值
     * @param key 键
     * @return {@link Node }<{@link K },{@link V }>
     */
    final Node<K,V> getNode(int hash, Object key) {
        Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
        // 1.校验非空：如果桶数组是空或者该索引位置没节点，则返回空，否则继续执行以下逻辑
        // 如果校验通过则继续执行，如果校验失败，则返回null
        if ((tab = table) != null && (n = tab.length) > 0 &&
                (first = tab[(n - 1) & hash]) != null) {
            // 2.判断返回首节点：如果首节点的哈希值和key都跟入参的key一致，则直接返回首节点
            if (first.hash == hash &&
                    ((k = first.key) == key || (key != null && key.equals(k))))
                return first;
            // 3.遍历链表或红黑树
            // 判断一个节点是链表节点还是树节点，通过 instanceof TreeNode判断
            if ((e = first.next) != null) {
                if (first instanceof TreeNode)
                    // 3.1 如果是红黑树节点，则调用红黑树的查找目标节点方法getTreeNode
                    return ((TreeNode<K,V>)first).getTreeNode(hash, key);
                do {
                    // 3.2 如果是链表节点，则直接遍历即可，直到找到哈希值和key一致的节点，返回。
                    if (e.hash == hash &&
                            ((k = e.key) == key || (key != null && key.equals(k))))
                        return e;
                } while ((e = e.next) != null);
            }
        }
        return null;
    }
```

#### 2.6.2 getNode()：**实际**获取节点

上面 get() 方法只有两行，调用了 getNode() 方法获取节点：

![](<assets/1740031172414.png>)

**修饰符**：

*   **final**：getNode() 方法跟 putVal() 类似，是 final 方法，代表它无法被子类重写；
*   **default**：访问修饰符没加，即 default，代表这个方法仅在本包下（java.util）可见，其他包下下不可见，所以它也只允许 java.util 包下的类调用。

**核心逻辑：**

1.  **校验非空**：如果桶数组是空或者该索引位置没节点，则返回空，否则继续执行以下逻辑
2.  **判断返回首节点**：如果首节点的哈希值和 key 都跟入参的 key 一致，则直接返回首节点
3.  **遍历返回节点**：遍历链表或红黑树，找到哈希值和 key 一致的节点后返回
    1.  **如果是红黑树节点**：则调用红黑树的查找目标节点方法 **getTreeNode()**
    2.  **如果是链表节点**：则直接遍历、找到、返回。

**具体代码：**

```
/**
     * 删除元素
     * @param key 键
     * @return {@link V }
     */
    public V remove(Object key) {
        // 调用removeNode()方法，删除节点
        Node<K,V> e;
        return (e = removeNode(hash(key), key, null, false, true)) == null ?
                null : e.value;
    }
```

### 2.7 删除

#### 2.7.1 remove()：删除元素

删除和获取、存储类似，也是一个 remove() 方法，一个 removeNode() 方法，前者是对外暴露的、可调用的方法，后者是本包下可见、不可被重写的方法。

同样，remove() 方法只有**两行**，即调用 removeNode() 方法。

唯一不同的点是，HashMap 直接使用了父类 AbstractMap 的 remove() 方法，并没有重写

**核心逻辑：**

*   调用 **removeNode()** 方法，删除节点

**具体代码：**

```
/**
     * 删除节点
     * @param hash 节点的哈希值
     * @param key 需要删除的键
     * @param value 需要删除的值，若不需要根据值匹配可为 null
     * @param matchValue 是否需要匹配值，如果为 true，则只在键值对完全匹配时才删除
     * @param movable 是否允许节点移动，主要用于红黑树的节点平衡
     * @return 被删除的 {@link Node }<{@link K },{@link V }>，如果没有找到则返回 null
     */
    final Node<K,V> removeNode(int hash, Object key, Object value,
                               boolean matchValue, boolean movable) {
        Node<K,V>[] tab; Node<K,V> p; int n, index;
        // 1. 校验非空：如果桶数组是空或者该索引位置没节点，则返回空，否则继续执行以下逻辑
        if ((tab = table) != null && (n = tab.length) > 0 &&
                (p = tab[index = (n - 1) & hash]) != null) {
            /*
             2.标记待删除元素
             创建节点node，用于标记待删除节点
            */
            Node<K,V> node = null, e; K k; V v;
            // 2.1 判断标记首节点：如果首节点的哈希值和key都跟入参的key一致，则直接node标记首节点
            if (p.hash == hash &&
                    ((k = p.key) == key || (key != null && key.equals(k))))
                node = p;
            else if ((e = p.next) != null) {
                // 2.2 遍历链表或红黑树，找到待删除节点
                if (p instanceof TreeNode)
                    // 2.2.1 如果是红黑树，调用getTreeNode()方法，定位待删除节点
                    node = ((TreeNode<K,V>)p).getTreeNode(hash, key);
                else {
                    //  2.2.2 如果是链表，遍历链表，找到待删除节点
                    do {
                        if (e.hash == hash &&
                                ((k = e.key) == key ||
                                        (key != null && key.equals(k)))) {
                            node = e;
                            break;
                        }
                        p = e;
                    } while ((e = e.next) != null);
                }
            }
 
            // 3. 删除节点，并修复链表或红黑树
            // 3.1 校验value非空：如果找到的目标节点是空，则直接返回null，否则继续执行以下逻辑
            if (node != null && (!matchValue || (v = node.value) == value ||
                    (value != null && value.equals(v)))) {
                // 3.2 删除节点
                // 3.2.1 如果是红黑树，调用removeTreeNode()方法，删除节点
                if (node instanceof TreeNode)
                    ((TreeNode<K,V>)node).removeTreeNode(this, tab, movable);
                // 3.2.2 如果是链表，元素前节点指向元素后节点,删除节点
                else if (node == p)
                    tab[index] = node.next;
                else
                    p.next = node.next;
                // 3.3 给HashMap修改次数字段modeCount加1，给元素个数字段size减1
                ++modCount;
                --size;
                // 3.4 执行删除后方法，此方法是钩子方法，内容是空，修饰符是default，代表它只能被java.util下的子类重写（例如LinkedHashMap就重写了这个方法）
                afterNodeRemoval(node);
                // 3.5 返回节点
                return node;
            }
        }
        return null;
    }
```

#### 2.7.2 **removeNode()：实际删除节点**

在上面 remove() 方法的第二行，调用了 removeNode() 方法，实际删除节点：

![](<assets/1740031172733.png>)

**修饰符**：

*   **final**：getNode() 方法跟 putVal() 类似，是 final 方法，代表它无法被子类重写；
*   **default**：访问修饰符没加，即 default，代表这个方法仅在本包下（java.util）可见，其他包下下不可见，所以它也只允许 java.util 包下的类调用。

**核心逻辑：**

removeNode() 跟 getNode() 的逻辑非常相似，都有校验非空、判断首节点、遍历的步骤，只是多了一步删除节点、修复链表或红黑树的操作。

1.  **校验非空**：如果桶数组是空或者该索引位置没节点，则返回空，否则继续执行以下逻辑
2.  **标记待删除元素**
    1.  **判断标记首节点**：如果首节点的哈希值和 key 都跟入参的 key 一致，则直接用 node 节点标记首节点
    2.  **遍历标记**：遍历链表或红黑树，找到待删除节点
        1.  如果是红黑树，调用 getTreeNode() 方法，定位待删除节点
        2.  如果是链表，遍历链表，找到待删除节点
3.  **删除节点，并修复链表或红黑树**
    1.  **校验 value 非空**：如果找到的目标节点是空，则直接返回 null，否则继续执行以下逻辑
    2.  **删除节点**
        1.  如果是红黑树，调用 removeTreeNode() 方法，删除节点
        2.  如果是链表，元素前节点指向元素后节点, 删除节点
    3.  **调整修改次数和 size**：给 HashMap 修改次数字段 modeCount 加 1，给元素个数字段 size 减 1
    4.  **执行删除后方法**：此方法是钩子方法，内容是空，修饰符是 default，代表它只能被 java.util 下的子类重写（例如 LinkedHashMap 就重写了这个方法）
    5.  **返回节点**

**具体代码：** 

```
/**
     * 删除节点
     * @param hash 节点的哈希值
     * @param key 需要删除的键
     * @param value 需要删除的值，若不需要根据值匹配可为 null
     * @param matchValue 是否需要匹配值，如果为 true，则只在键值对完全匹配时才删除
     * @param movable 是否允许节点移动，主要用于红黑树的节点平衡
     * @return 被删除的 {@link Node }<{@link K },{@link V }>，如果没有找到则返回 null
     */
    final Node<K,V> removeNode(int hash, Object key, Object value,
                               boolean matchValue, boolean movable) {
        Node<K,V>[] tab; Node<K,V> p; int n, index;
        // 1. 校验非空：如果桶数组是空或者该索引位置没节点，则返回空，否则继续执行以下逻辑
        if ((tab = table) != null && (n = tab.length) > 0 &&
                (p = tab[index = (n - 1) & hash]) != null) {
            /*
             2.标记待删除元素
             创建节点node，用于标记待删除节点
            */
            Node<K,V> node = null, e; K k; V v;
            // 2.1 判断标记首节点：如果首节点的哈希值和key都跟入参的key一致，则直接node标记首节点
            if (p.hash == hash &&
                    ((k = p.key) == key || (key != null && key.equals(k))))
                node = p;
            else if ((e = p.next) != null) {
                // 2.2 遍历链表或红黑树，找到待删除节点
                if (p instanceof TreeNode)
                    // 2.2.1 如果是红黑树，调用getTreeNode()方法，定位待删除节点
                    node = ((TreeNode<K,V>)p).getTreeNode(hash, key);
                else {
                    //  2.2.2 如果是链表，遍历链表，找到待删除节点
                    do {
                        if (e.hash == hash &&
                                ((k = e.key) == key ||
                                        (key != null && key.equals(k)))) {
                            node = e;
                            break;
                        }
                        p = e;
                    } while ((e = e.next) != null);
                }
            }
 
            // 3. 删除节点，并修复链表或红黑树
            // 3.1 校验value非空：如果找到的目标节点是空，则直接返回null，否则继续执行以下逻辑
            if (node != null && (!matchValue || (v = node.value) == value ||
                    (value != null && value.equals(v)))) {
                // 3.2 删除节点
                // 3.2.1 如果是红黑树，调用removeTreeNode()方法，删除节点
                if (node instanceof TreeNode)
                    ((TreeNode<K,V>)node).removeTreeNode(this, tab, movable);
                // 3.2.2 如果是链表，元素前节点指向元素后节点,删除节点
                else if (node == p)
                    tab[index] = node.next;
                else
                    p.next = node.next;
                // 3.3 给HashMap修改次数字段modeCount加1，给元素个数字段size减1
                ++modCount;
                --size;
                // 3.4 执行删除后方法，此方法是钩子方法，内容是空，修饰符是default，代表它只能被java.util下的子类重写（例如LinkedHashMap就重写了这个方法）
                afterNodeRemoval(node);
                // 3.5 返回节点
                return node;
            }
        }
        return null;
    }
```