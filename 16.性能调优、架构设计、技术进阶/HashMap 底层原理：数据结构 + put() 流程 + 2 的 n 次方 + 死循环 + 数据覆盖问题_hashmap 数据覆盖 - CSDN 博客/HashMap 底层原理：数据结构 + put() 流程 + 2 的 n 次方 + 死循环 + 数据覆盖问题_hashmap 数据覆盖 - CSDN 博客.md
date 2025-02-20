---
url: https://blog.csdn.net/qq_40991313/article/details/131620721
title: HashMap 底层原理：数据结构 + put() 流程 + 2 的 n 次方 + 死循环 + 数据覆盖问题_hashmap 数据覆盖 - CSDN 博客
date: 2025-02-20 13:58:00
tag: 
summary: 
---
**导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[一、底层](#t0)

[1.1 HashMap 数据结构](#t1)

[1.2 扩容机制](#t2)

[1.3 put() 流程](#t3)

[1.4 HashMap 是如何计算 key 的？](#t4)

[1.5 HashMap 容量为什么是 2 的 n 次方？](#t5)

[1.5.1 原因](#t6)

[1.5.2 扩容均匀散列演示：从 2^4 扩容成 2^5](#t7)

[二、线程安全问题](#t8)

[2.1 HashMap 线程安全吗？](#t9) 

[2.2 线程安全的解决方案](#t10)

[2.3 JDK7 扩容时死循环问题](#t11)

[2.3.1 死循环问题演示](#t12) 

[2.3.2 JDK8 是怎么解决死循环问题的？](#t13)

[2.4 JDK8 put 时数据覆盖问题](#t14)

[2.5 modCount 非原子性自增问题](#t15)

## 一、底层

### **1.1 HashMap 数据结构**

JDK1.7 及之前版本，HashMap 底层是 “数组 + 单向链表”。

在 JDK8 中, HashMap 底层是采用 “数组 + 单向链表 + 红黑树” 来实现的，使用红黑树主要是为了提高查询性能。

*   **数组：**用于实现哈希表，存放 HashMap 的 key；
*   **链表：**存放 HashMap 的 value，用链地址法处理冲突；链表时间复杂度 O(n)。
*   **红黑树：**当 链表长度大于等于 8, 且数组的长度大于 64 时，链表转为红黑树，存放 HashMap 的 value。红黑树时间复杂度稳定的 O(logn)。

![](<assets/1740031080623.png>)

**红黑树：** 近似平衡二叉树，左右子树高差有可能大于 1，查找效率略低于平衡二叉树，但增删效率高于平衡二叉树，适合频繁插入删除。

*   结点非黑即红；
*   根结点是黑色，叶节点是黑色空节点（常省略）；
*   任何相邻节点不能同时为红色；
*   从任一结点到其每个叶子的所有路径都包含相同数目的黑色结点；
*   查询性能稳定 O(logN)，高度最高 2log(n+1)；

![](<assets/1740031080920.png>) 

### **1.2 扩容机制**

HashMap 中，数组的默认初始容量为 16，这个容量会以 2 的指数进行扩容。具体来说，当数组中的元素达到一定比例的时候 HashMap 就会扩容，这个比例叫做负载因子，默认为 0.75。

自动扩容机制，是为了保证 HashMap 初始时不必占据太大的内存，而在使用期间又可以实时保证有足够大的空间。采用 2 的指数进行扩容，是为了利用位运算，提高扩容运算的效率。

数组每个元素存的是链表头结点地址，链地址法处理冲突，若链表的长度达到了 8，红黑树代替链表。

### **1.3 put() 流程**

put() 方法的执行过程中, 主要包含四个步骤：

1.  计算 key 存取位置，与运算 hash&(2^n-1），实际就是哈希值取余，位运算效率更高。
2.  判断数组，若发现数组为空，则进行首次扩容为初始容量 16。
3.  判断数组存取位置的头节点，若发现头节点为空，则新建链表节点，存入数组。
4.  判断数组存取位置的头节点，若发现头节点非空，则看情况将元素覆盖或插入链表（JDK7 头插法，JDK8 尾插法）、红黑树。
5.  插入元素后，判断元素的个数，若发现超过阈值则以 2 的指数再次扩容。

其中，第 3 步又可以细分为如下三个小步骤：

1. 若元素的 key 与头节点的 key 一致，则直接覆盖头节点。

2. 若元素为树型节点，则将元素追加到树中。

3. 若元素为链表节点，则将元素追加到链表中。追加后，需要判断链表长度以决定是否转为红黑树。若链表长度达到 8、数组容量未达到 64，则扩容。若链表长度达到 8、数组容量达到 64，则转为红黑树。

**哈希表处理冲突：**开放地址法（线性探测、二次探测、再哈希法）、链地址法

### **1.4 HashMap 是如何计算 key 的？**

```
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        // 此处省略了代码
        // i = (n - 1) & hash]
        if ((p = tab[i = (n - 1) & hash]) == null)
            
            tab[i] = newNode(hash, key, value, null);
        
 
        else {
            // 省略了代码
        }
}
```

例如当前数组容量是 16，我们要存取 18，那么就可以用 18&15==2。相当于 18%16==2.

put() 里，计算 key 的部分源码：

```
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        if ((p = tab[i = (n - 1) & hash]) == null)     // 如果没有 hash 碰撞，则直接插入
            tab[i] = newNode(hash, key, value, null);
    }
```

### **1.5 HashMap 容量为什么是 2 的 n 次方？**

#### 1.5.1 原因

计算 value 对应 key 的 Hash 运算：

```
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
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
//put会执行modCount++操作，这步操作分为读取、增加、保存，不是一个原子性操作，也会出现线程安全问题。 
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```

2^n-1 和 2^(n+1)-1 的二进制除了第一位，后几位都是相同的。这样可以使得添加的元素均匀分布在 HashMap 的每个位置上，防止哈希碰撞。

例如 15（即 2^4-1）的二进制为 1111，31 的二进制为 11111，63 的二进制为 111111，127 的二进制为 1111111。

#### **1.5.2 扩容均匀散列演示：从 2^4 扩容成 2^5**

0&(2^4-1)=0；0&(2^5-1)=0

16&(2^4-1)=0；16&(2^5-1)=16。所以扩容后，key 为 0 的一部分 value 位置没变，一部分 value 迁移到扩容后的新位置。

1&(2^4-1)=1；1&(2^5-1)=1

17&(2^4-1)=1；17&(2^5-1)=17。所以扩容后，key 为 1 的一部分 value 位置没变，一部分 value 迁移到扩容后的新位置。

**用取余演示扩容：**

如果觉得与运算有点难理解，我们可以用取余演示：

假设从 16 扩容到 32：1%16=1,17%16=1；1%32=1,17%32=17。

1 和 17 本来 key 都是 1，扩容后 1 的 key 还是 1，17 的 key 变成 17。这样原来 key 为 1 的 value 就均匀的散列在扩容后的哈希表里了（一部分 value 不变，一部分 value 移动到扩容后新位置）。

## 二、线程安全问题

### 2.1 HashMap 线程安全吗？ 

HashMap 是线程不安全的，多线程环境下可能出现死循环问题、数据覆盖问题。

多线程下建议使用 Collections 工具类和 JUC 包的 ConcurrentHashMap。

### **2.2 线程安全的解决方案**

*   使用 Hashtable（古老 api 不推荐）
*   使用 Collections 工具类，将 HashMap 包装成线程安全的 HashMap。
    
    ```
    Collections.synchronizedMap(map);
    ```
    
*   使用更安全的 ConcurrentHashMap（推荐），ConcurrentHashMap 通过对槽（链表头结点）加锁，以较小的性能来保证线程安全。
*   使用 synchronized 或 Lock 加锁 HashMap 之后，再进行操作，相当于多线程排队执行（比较麻烦，也不建议使用）。

### **2.3 JDK7 扩容时死循环问题**

#### **2.3.1 死循环问题演示** 

单线程扩容流程：

JDK7 中，HashMap 链地址法处理冲突时采用头插法，在扩容时依然头插法，所以链表里结点顺序会反过来。

假如有 T1、T2 两个线程同时对某链表扩容，他们都标记头结点和第二个结点，此时 T2 阻塞，T1 执行完扩容后链表结点顺序反过来，此时 T2 恢复运行再进行翻转就会产生环形链表，即 B.next=A; A.next=B，从而死循环。

![](<assets/1740031081162.png>)

#### 2.3.2 JDK8 是怎么解决死循环问题的？

**JDK8 尾插法解决了死循环问题。**

JDK8 中，HashMap 采用尾插法，扩容时链表节点位置不会翻转，解决了扩容死循环问题，但是性能差了一点，因为要遍历链表再查到尾部。 

例如 A——>B——>C 要迁移，迁移时先移动头结点 A，再移动 B 并插入 A 的尾部，再移动 C 插入尾部，这样结果还是 A——>B——>C。顺序没变，扩容线程

### **2.4 JDK8 put 时数据覆盖问题**

HashMap 非线程安全，如果两个并发线程插入的数据 hash 取余后相等，就可能出现数据覆盖。

线程 A 刚找到链表 null 位置准备插入时就阻塞了，然后线程 B 找到这个 null 位置插入成功。借着线程 A 恢复，因为已经判过 null，所以直接覆盖插入这个位置，把线程 B 插入的数据覆盖了。

```
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        if ((p = tab[i = (n - 1) & hash]) == null)     // 如果没有 hash 碰撞，则直接插入
            tab[i] = newNode(hash, key, value, null);
    }
```

### **2.5 modCount 非原子性自增问题**

modCount： HashMap 的成员变量，用于记录 HashMap 被修改次数

put 会执行 modCount++ 操作，这步操作分为读取、增加、保存，不是一个原子性操作，也会出现线程安全问题。 

```
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
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
//put会执行modCount++操作，这步操作分为读取、增加、保存，不是一个原子性操作，也会出现线程安全问题。 
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```