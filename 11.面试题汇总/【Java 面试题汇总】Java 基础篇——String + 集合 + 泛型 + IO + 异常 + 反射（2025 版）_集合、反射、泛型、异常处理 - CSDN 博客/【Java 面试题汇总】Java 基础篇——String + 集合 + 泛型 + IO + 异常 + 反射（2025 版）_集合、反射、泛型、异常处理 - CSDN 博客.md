---
url: https://blog.csdn.net/qq_40991313/article/details/130506034
title: 【Java 面试题汇总】Java 基础篇——String + 集合 + 泛型 + IO + 异常 + 反射（2025 版）_集合、反射、泛型、异常处理 - CSDN 博客
date: 2025-02-20 13:45:47
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?spm=1001.2014.3001.5501 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[三、String](#t0) 

[3.1.String 常量池](#t1)

[3.2. 请你说说 String 类](#t2)

[String 常用方法](#t3)

[创建字符串的两种方式](#t4)

[源码（JDK8）](#t5)

[源码（JDK9）](#t6)

[String 类为什么不可被继承](#t7)

[String 类为什么要设计成 final](#t8)

[String 的字符串为什么不可被变](#t9)

 [String 源码有用到什么设计模式？](#t10)

[享元模式和单例模式区别](#t11)

[3.3.new String("abc") 创建了几个字符串对象？](#t12)

[3.4.String、StringBuffer、Stringbuilder 有什么区别](#t13)

[3.5. 为什么 StringBuffer 是线程安全的、效率低](#t14)

[四、集合](#t15) 

[4.1. 请说说你对 Java 集合的了解](#t16)

[4.2. 请你说说 List 与 Set 的区别](#t17)

[4.3. 说说你对 ArrayList 的理解](#t18)

[4.3 怎么实现 ArrayList 的线程安全？](#t19)

[4.4. 请你说说 ArrayList 和 LinkedList 的区别](#t20)

[4.5. 请你说说 HashMap 底层原理](#t21)

[底层数据结构](#%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)

[HashMap 是否允许重复？](#HashMap%E6%98%AF%E5%90%A6%E5%85%81%E8%AE%B8%E9%87%8D%E5%A4%8D%EF%BC%9F)

[扩容机制](#%E6%89%A9%E5%AE%B9%E6%9C%BA%E5%88%B6%C2%A0) 

[put() 流程](#put%28%29%E6%B5%81%E7%A8%8B)

[HashMap 容量为什么是 2 的 n 次方？](#HashMap%E5%AE%B9%E9%87%8F%E4%B8%BA%E4%BB%80%E4%B9%88%E6%98%AF2%E7%9A%84n%E6%AC%A1%E6%96%B9%EF%BC%9F)

[JDK7 扩容时死循环问题](#JDK7%E6%89%A9%E5%AE%B9%E6%97%B6%E6%AD%BB%E5%BE%AA%E7%8E%AF%E9%97%AE%E9%A2%98)

[JDK8 尾插法](#JDK8%20%E5%B0%BE%E6%8F%92%E6%B3%95)

[JDK8 put 时数据覆盖问题](#JDK8%20put%E6%97%B6%E6%95%B0%E6%8D%AE%E8%A6%86%E7%9B%96%E9%97%AE%E9%A2%98)

[modCount 非原子性自增问题](#modCount%E9%9D%9E%E5%8E%9F%E5%AD%90%E6%80%A7%E8%87%AA%E5%A2%9E%E9%97%AE%E9%A2%98)

[源码](#%E6%BA%90%E7%A0%81)

[4.6. 请你说说 ConcurrentHashMap](#t22)

[4.7. 请你说说 HashMap 和 Hashtable 的区别](#t23)

[五、泛型](#t24) 

[5.1. 请你说说泛型、泛型擦除](#t25)

[六、IO](#t26) 

[6.1. 请你说说 IO 多路复用](#t27)

[6.2. 请你说说 BIO、NIO、O](#t28)

[6.3. 请你讲一下 Java NIO](#t29)

[七、异常](#t30) 

[7.0. 说说异常体系](#t31)

[7.1. 请你说说 Java 的异常处理机制](#t32)

[八、反射](#t33) 

[8.1. 请说说你对反射的了解](#t34)

[九、多线程](#t35)

## 三、String 

### 3.1.String 常量池

介绍、原理 

**常量池：**Java 虚拟机有一个常量池机制，它会直接把字符串常量放入常量池中，从而实现复用。

**Java 字符串存储原理：** 

1.  创建字符串常量时，JVM 会通过 equals() 检查字符串常量池中是否存在这个字符串；
2.  若字符串常量池中存在该字符串，则直接返回引用实例；
3.  若不存在，先实例化该字符串，并且将该字符串的引用放入字符串常量池中，以便于下次使用时，直接取用，达到缓存快速使用的效果。

str1 和 str2 在赋值时，使用的是字符串常量。 因此 str1 和 str2 指向的是常量池中的同一个内存地址，所以返回值是 true。

```
//比较地址
//只要new，就在堆内存开辟空间。直接赋值字符串在常量池里。
        String str1 = "hello";        //常量池里无“hello”对象，创建“hello”对象，str1指向常量池“hello”对象。
//先检查字符串常量池中有没有"hello"，如果字符串常量池中没有，则创建一个，然后 str1 指向字符串常量池中的对象，如果有，则直接将 str1 指向"hello"；
        String str2 = "hello";    //常量池里有“hello”对象，str2直接指向常量池“hello”对象。
        String str3 = new String("hello");   //堆中new创建了一个对象。假如“hello”在常量池中不存在，Jvm还会常量池中创建这个对象“hello”。
        String str4 = new String("hello");
        System.out.println(str1==str2);//true，因为str1和str2指向的是常量池中的同一个内存地址
        System.out.println(str1==str3);//fasle，str1常量池旧地址，str3是new出的新对象，指向一个全新的地址
        System.out.println(str4==str3);//fasle，因为它们引用不同
 
//比较内容
        System.out.println(str4.equals(str3));//true，因为String类的equals方法重写过，比较的是字符串值
```

### 3.2. 请你说说 String 类

**得分点**

String 常用方法, String 能否被继承, 创建字符串的两种方式、String 不可变和不可被继承、源码享元

**标准回答**

#### **String 常用方法**

String 类包含了大量处理字符串的方法, 包括 charAt(),indexOf(),lastIndexOf(),substring(),split(),trim(),toUpperCase(),startsWith() 等。

#### **创建字符串的两种方式**

创建字符串有两种方式，一种是使用字符串直接量，另一种是使用 new + 构造器。采用 new 的方式会多创建出一个对象来，占用了更多的内存 ，所以**建议采用直接量的方式来创建字符串。**

*   **字符串直接量创建：**JVM 会使用常量池来管理这个字符串；
*   **new 创建：**JVM 会先使用常量池来管理字符串直接量（若已有此字符串则直接返回引用，若没有则实例化后再返回引用），再调用 String 类的构造器来创建一个新的 String 对象，新创建的 String 对象会被保存在堆内存中。

#### 源码（JDK8）

**最终类**：类名用 final 修饰，代表它不可被继承

final 类不可被继承、final 方法不可被重写、final 变量不可变。

**私有最终字符数组**： 底层数组用 private final 修饰，代表这个变量不可变

*   **final：**导致 value 不能指向新数组（但无法保证 value 这个引用变量指向的真实数组不可变）
*   **private：**导致 value 指向的原数组不可变。私有成员变量，且没对外暴露任何修改 value 的方法，所以 value 这个引用变量指向的真实数组不可变。

![](<assets/1740030347803.png>)

#### 源码（JDK9）

JDK9 开始，为了节省内存，进而减少垃圾回收次数，String 底层由 char 数组改成了 byte[]。byte 变量占 1 字节，char 变量占 2 字节

**基本数据类型内存空间**

对于基本数据类型，你需要了解每种类型所占据的内存空间，这是面试官喜欢追问的问题：

 **- byte：1 字节（8 位）**, 数据范围是 `-2^7 ~ 2^7-1`。  
 - short：2 字节（16 位）, 数据范围是 `-2^15 ~ 2^15-1`。  
 - int：4 字节（32 位）, 数据范围是 `-2^31 ~ 2^31-1`。  
 - long：8 字节（64 位）, 数据范围是 `-2^63 ~ 2^63-1`。c 语言里 long 占 4 字节，long long 占 8 字节。  
 - float：4 字节（32 位）, 数据范围大约是 `-3.4*10^38 ~ 3.4*10^38`。  
 - double：8 字节（64 位）, 数据范围大约是 `-1.8*10^308 ~ 1.8*10^308`。  
 **- char：2 字节（16 位）**, 数据范围是 `\u0000 ~ \uffff`，unicode 编码英文和中文都占两个字节。c 语言使用 ASCII 编码 char 占 1 字节，不能存汉字。ASCII 编码是 Unicode 的一个子集，因此它们存在一些字符码值是相等的。  
 - boolean：Java 规范没有明确的规定, 不同的 JVM 有不同的实现机制。

#### **String 类为什么不可被继承**

因为 String 类是由 final 修饰的，final 类不可被继承。

#### String 类为什么要设计成 final

*   **防止被继承**：定义成 final 安全，不能被继承，方法不能重写。保证了对象的不可重复性，防止在用 String 对象赋值时，该对象被赋值后的对象破坏。
*   **线程安全**：final 对象不可被改变，在线程竞争写资源不会产生竞争。
*   **节省空间**：支持字符串常量池，两个字符串定义同样的值，实际上引用的是同一个内存地址空间，这样可以有效节省内存空间。（享元设计模式）

#### **String 的字符串为什么不可被变**

 因为 String 底层 **char 类型**的 value 数组是 private final 修饰的：

*   **final：**导致 value 不能指向新数组（但无法保证 value 这个引用变量指向的真实数组不可变）
*   **private：**导致 value 指向的原数组不可变。私有成员变量，且没对外暴露任何修改 value 的方法，所以 value 这个引用变量指向的真实数组不可变。

JDK9 开始，为了节省内存，进而减少垃圾回收次数，String 底层由 char 数组改成了 byte[]。 

**不可变的优点：**因为压根不会被改，所以线程安全、节省空间、效率高。

####  **String 源码有用到什么设计模式？**

享元设计模式：当一个系统中存在大量重复对象，若这些重复的对象是不可变对象，就能利用享元模式将对象设计成享元，在内存中只保留一份实例，供引用。这就减少内存中对象的数量，最终节省内存。享元模式是结构型设计模式，用于对象的创建。

#### **享元模式和单例模式区别**

*   单例模式是类级别的，一个类只能有一个对象实例；
*   享元模式是对象级别的，一个类可以有多个不同的对象实例，主要为了解决重复对象问题，池技术一般是用享元模式实现的，例如字符串常量池、数据库连接池、缓冲池。
*   单例模式可以看作是享元模式的一种特立，享元模式只有一种对象实例时就是单例，有多种不重复的对象实例时就是享元。享元模式主要是为了节约内存空间，提高系统性能，而单例模式主要为了可以共享数据；

**直接量是指在程序中通过源代码直接给出的值**。

String 类是 Java 最常用的 API, 它包含了大量处理字符串的方法, 比较常用的有：

*   - char charAt(int index)：返回指定索引处的字符；
*   - String substring(int beginIndex, int endIndex)：从此字符串中截取出一部分子字符串；
*   - String[] split(String regex)：以指定的规则将此字符串分割成数组；
*   - String trim()：删除字符串前导和后置的空格；
*   - int indexOf(String str)：返回子串在此字符串首次出现的索引；
*   - int lastIndexOf(String str)：返回子串在此字符串最后出现的索引；
*   - boolean startsWith(String prefix)：判断此字符串是否以指定的前缀开头；
*   - boolean endsWith(String suffix)：判断此字符串是否以指定的后缀结尾；
*    - String toUpperCase()：将此字符串中所有的字符大写；
*    - String toLowerCase()：将此字符串中所有的字符小写；
*    - String replaceFirst(String regex, String replacement)：用指定字符串替换第一个匹配的子串；
*    - String replaceAll(String regex, String replacement)：用指定字符串替换所有的匹配的子串。

 **验证常量池：**

```
//比较地址
//只要new，就在堆内存开辟空间。直接赋值字符串在常量池里。
        //常量池里无“hello”对象，创建“hello”对象，str1指向常量池“hello”对象。
        String str1 = "hello";        
//先检查字符串常量池中有没有"hello"，如果字符串常量池中没有，则创建一个，然后 str1 指向字符串常量池中的对象，如果有，则直接将 str1 指向"hello"；
        //常量池里有“hello”对象，str2直接指向常量池“hello”对象。
        String str2 = "hello";
        //堆中new创建了一个对象。假如“hello”在常量池中不存在，Jvm还会常量池中创建这个对象“hello”。
        String str3 = new String("hello");   
        String str4 = new String("hello");
        //下面输出true，因为str1和str2指向的是常量池中的同一个内存地址
        System.out.println(str1 == str2);
        //下面输出false，str1常量池旧地址，str3是new出的新对象，指向一个全新的地址
        System.out.println(str1 == str3);
        //下面输出false，因为它们引用不同
        System.out.println(str4 == str3);
 
//比较内容
        //下面输出true，因为String类的equals方法重写过，比较的是字符串值
        System.out.println(str4.equals(str3));
```

结果：

![](<assets/1740030348087.png>)

### 3.3.new String("abc") 创建了几个字符串对象？

答案、原理 

**答案：**一个或两个。

首先，new string 这边由于 new 关键字，所以这边肯定会在堆中**直接创建一个**字符串对象。

其次，**如果**字符串常量池中不存在 "abc"（通过 equals 比较）这个字符串的引用，则会在字符串常量池中**创建一个**字符串对象。如果已存在则不创建。注意这边说的在字符串常量池创建对象，最终对象还是在堆中创建，字符串常量池只放引用。

### 3.4.String、StringBuffer、Stringbuilder 有什么区别

**得分点**

是否可变、复用率、效率、线程安全问题

**标准回答**

**String:** 不可变字符序列，效率低，但是复用率高、线程安全。

*   **不可变**：指 String 对象创建之后, 直到这个对象销毁为止, 对象中的字符序列都不能被改变。
*   **复用率高**：指 String 类型对象创建出来后归常量池管，可以随时从常量池调用同一个 String 对象。StringBuffer 和 StringBuider 在创建对象后一般要转化成 String 对象才调用。

**StringBuffer 和 StringBuilder**StringBuffer 和 Stringbuilder 都是字符序列可变的字符串，方法也一样，有共同的父类 AbstractStringBuilder。 

*   **StringBuffer:** 可变字符序列、效率较高 (增删)、线程安全
*   **StringBuilder:** 可变字符序列、效率最高、线程不安全

### **3.5. 为什么 StringBuffer 是线程安全的、效率低**

**为什么 StringBuffer 是线程安全的**

因为有线程同步风险的方法（例如 length()、append()、delete() 等）都加了 synchronized 锁，锁的粒度是当前实例。

```
​
    /**
     * @Author: vince
     * @CreateTime: 2024/10/15
     * @Description: 具体建造者：简化版StringBuffer
     * @Version: 1.0
     */
    public final class StringBuffer
            extends AbstractStringBuilder
            implements Serializable, CharSequence {
        /**
         * 缓存上一次调用toString()方法返回的值。
         * 每当StringBuffer被修改时，这个缓存会被清除。
         * transient关键字表明此字段不会被序列化。
         */
        private transient char[] toStringCache;
 
        /**
         * 追加字符串
         * 特点：
         *  1.方法加了synchronized锁，保证线程安全
         *  2.返回值是StringBuffer
         *  3.方法内第一步清空toStringCache
         * @param str
         * @return {@link java.lang.StringBuffer }
         */
        @Override
        public synchronized java.lang.StringBuffer append(String str) {
            // 缓存上一次调用toString()方法返回的值，所以每次更新底层字符数组时，都需要清空；
            toStringCache = null;
            super.append(str);
            return this;
        }
 
        /**
         * 获取实际字符串长度
         * StringBuffer线程安全的原因：有线程同步风险的方法都加了synchronized锁，锁的粒度是当前实例
         * @return int
         */
        @Override
        public synchronized int length() {
            return count;
        }
 
        @Override
        public synchronized int capacity() {
            return value.length;
        }
    }
 
​
```

**synchronized 关键字作用于三个位置：**

 1. 作用在静态方法上, 则锁是当前类的 Class 对象。

 2. 作用在普通方法上, 则锁是当前的实例（this）。

 3. 作用在代码块上, 则需要在关键字后面的小括号里, 显式指定锁对象，例如 this、Xxx.class。

**为什么 StringBuffer 效率低？**

1.  **锁本身效率低**：在存在线程竞争情况下，加锁的代码块本身就比不加锁要慢很多，因为锁内的共享资源同一时刻只能一个线程访问。
2.  **锁粒度大**：锁的粒度是当前实例，直接给整个方法加 synchronized，而不是具体需要在有同步风险的代码块上加锁，性能低。这个方法内部分共享资源可能是线程安全的，并不需要加锁。

Java 中提供了 String,StringBuffer 两个类来封装字符串, 并且提供了一系列方法来操作字符串对象。

String 是一个不可变类, 也就是说, 一个 String 对象创建之后, 直到这个对象销毁为止, 对象中的字符序列都不能被改变。

**StringBuffer** 对象则代表一个字符序列可变的字符串, 当一个 StringBuffer 对象被创建之后, 我们可以通过 StringBuffer 提供的 **append()、insert()、reverse()、setCharAt()、setLength()**、等方法来改变这个字符串对象的字符序列。当通过 StringBuffer 得到期待中字符序列的字符串时, 就可以通过 toString() 方法将其转换为 String 对象。

StringBuilder 类是 JDK1.5 中新增的类, 他也代表了字符串对象。和 StringBuffer 类相比, 它们有共同的父类 `AbstractStringBuilder`, 二者无论是构造器还是方法都基本相同, 不同的一点是,**StringBuilder 没有考虑线程安全问题,** 也正因如此, StringBuilder 比 StringBuffer **性能略高**。因此, 如果是在单线程下操作大量数据, 应优先使用 StringBuilder 类；如果是在多线程下操作大量数据, 应优先使用 StringBuilder 类。

## 四、集合 

### 4.1. 请说说你对 Java 集合的了解

**得分点**

Set、List、Quque、Map、Collection 接口、线程安全的集合

**标准回答**

Java 中的集合类分为 4 大类, 分别由 4 个接口来代表, 它们是 Set、List、Queue、Map。其中, Set、List、Queue 接口都继承自 Collection 接口，Map 接口不继承自其他接口。

**Set** 代表**无序的、元素不可重复**的集合。

**List** 代表**有序的、元素可以重复**的集合。有序说的是元素顺序直接由插入顺序决定。

**Queue** 代表**先进先出**（FIFO）的队列。

**Map** 代表具有**映射关系**（key-value）的集合。

Java 提供了众多集合的实现类, 它们都是这些接口的直接或间接的实现类, 其中比较常用的有：HashSet、TreeSet、ArrayList、LinkedList、ArrayDeque、HashMap、TreeMap 等。这些集合都是线程不安全的。

**线程安全的集合：**

1.  **Collections 工具类：**Collections 工具类的 synchronizedXxx() 方法，将 ArrayList 等集合类包装成线程安全的集合类。例如 List<String> synchronizedList = Collections.synchronizedList(list);
2.  **古老 api：**java.util 包下性能差的古老 api，如 Vector、Hashtable，它们在 JDK1 就出现了，不推荐使用，因为线程安全的方案不成熟，性能差。
3.  **降低锁粒度的并发容器：**JUC 包下 Concurrent 开头的、以降低锁粒度来提高并发性能的容器，如 ConcurrentHashMap。适用于读写操作都很频繁的场景。
4.  **复制技术实现的并发容器：**JUC 包下以 CopyOnWrite 开头的、采用写时写入时复制技术实现的并发容器，如 CopyOnWriteArrayList。写操作时，先将当前数组进行一次复制，对复制后的数组进行操作，操作完成后再将原来的数组引用指向复制后的数组。避免了并发修改同一数组的线程安全问题。适用于读操作比写操作频繁且数据量不大的场景。适用于读操作远多于写操作的场景。

```
HashMap<String,String> map=new HashMap<String,String>();
        map.put("aaa","bbb");map.put("cc","dd");map.put("e","f");
        Set<Map.Entry<String,String>> set=map.entrySet();
        for(Map.Entry<String,String> i:set){
            System.out.println(i.getKey()+i.getValue());
        }
```

上面所说的集合类的接口或实现, 都位于 **java.util 包下**, 这些实现大多数都是**非线程安全的**。虽然非线程安全, 但是这些类的性能较好。如果需要使用**线程安全的集合类**, 则可以利用 **Collections 工具类**, 该工具类提供的 **synchronizedXxx() 方法**, 可以将这些集合类包装成线程安全的集合类。

java.util 包下的集合类中, 也有少数的线程安全的集合类, 例如 Vector、Hashtable, 它们都是非常古老的 API。虽然它们是线程安全的, 但是性能很差, 已经不推荐使用了。

从 JDK1.5 开始, 并发包下新增了大量高效的并发的容器, 这些容器按照实现机制可以分为三类。

第一类是以降低锁粒度来提高并发性能的容器, 它们的类名以 Concurrent 开头, 如 ConcurrentHashMap。

第二类是采用写时复制技术实现的并发容器, 它们的类名以 CopyOnWrite 开头, 如 CopyOnWriteArrayList。

第三类是采用 **Lock 实现的阻塞队列**, 内部创建两个 Condition 分别用于生产者和消费者的等待, 这些类都实现了 BlockingQueue 接口, 如 ArrayBlockingQueue。

![](<assets/1740030348203.png>)

### 4.2. 请你说说 List 与 Set 的区别

**得分点**

Collection 接口、有序性和重复性、TreeSet 有序

**标准回答** 

List 和 Set 都是 Collection 接口的子接口, 它们的主要区别在于元素的**有序性和重复性**：

**List** 代表**有序可以重复**的集合, 集合中每个元素都有对应的**顺序索引**, 它默认按元素的添加顺序设置元素的索引, 并且可以通过索引来访问指定位置的集合元素。另外, List 允许使用重复元素。

**Set** 代表**无序不可重复**的集合，无序是指存取顺不是按照添加顺序。Set 集合不允许包含相同的元素，如果试图把两个相同的元素加入同一个 Set，则会引发失败，添加方法将会返回 false。通过 hashCode 值来判断重复元素。

**加分回答 - TreeSet 支持自然有序和定制排序**

虽然 Set 代表无序的集合, 但是它有支持排序的实现类, 即 TreeSet。TreeSet 可以确保集合元素处于排序状态, 并支持自然排序和定制排序两种排序方式, 它的**底层是由 TreeMap 实现的**。TreeSet 也是非线程安全的, 但是它内部元素的值**不能为 null。**

### 4.3. 说说你对 ArrayList 的理解

**得分点**

数组实现、默认容量 10、每次扩容 1.5 倍、缩容、迭代器 ListIterator

**标准回答**

**数组实现：**

ArrayList 是**基于数组实现的**, 它的内部封装了一个 **Object[] 数组**。 通过**默认构造器**创建容器时, 该数组先被**初始化为空数组**, 之后在**首次添加数据**时再将其初始化成**长度为 10 的数组**。我们也可以使用有参构造器来创建容器, 并通过参数来显式指定数组的容量, 届时该数组被初始化为指定容量的数组。

**每次扩容 1.5 倍：**

如果向 ArrayList 中添加数据会造成超出数组长度限制, 则会触发**自动扩容**, 然后再添加数据。扩容就是**数组拷贝**, 将**旧数组中的数据拷贝到新数组**里, 而新数组的长度为原来**长度的 1.5 倍**。

**手动缩容：**

ArrayList 支持缩容, 但**不会自动缩容**, 即便是 ArrayList 中只剩下少量数据时也不会主动缩容。如果我们希望缩减 ArrayList 的容量, 则需要自己调用它的 **trimToSize() 方法**, 届时数组将按照元素的实际个数进行缩减，底层也是通过创建新数组拷贝实现的。

**迭代器 ListIterator：**

Set、List、Queue 都是 Collection 的子接口, 它们都继承了父接口的 iterator() 方法, 从而具备了迭代的能力。Map 使用迭代器必须通过先 entrySet() 转为 Set，然后再使用迭代器或 for 遍历。

但是, 相比于另外两个接口,**List** 还单独提供了 **listIterator() 方法**, 增强了迭代能力。iterator() 方法返回 Iterator 迭代器, listIterator() 方法返回 ListIterator 迭代器, 并且 **ListIterator 是 Iterator 的子接口**。ListIterator 在 Iterator 的基础上, 增加了 listIterator.previous() **向前遍历**的支持, 增加了 listIterator.set() 在**迭代过程中修改数据**的支持。

**排序方法：**

*   **Collections 工具类的 sort() 方法：**Collections.sort(list);
*   **stream 流：**list.stream().sort();
*   **比较器：**list.sort(new Comparator<Integer>() {})
*   **手写排序：**冒泡排序、选择排序、插入排序、二分法排序、快速排序、堆排序。

entrySet() 

```
List<String> list = new ArrayList<String>();
        list.add(1);
        //迭代器
        Iterator<String> it=list.iterator();
        while(it.hasNext()) System.out.println(it.next());
        // 使用ListIterator向前遍历并修改元素
        ListIterator<Integer> listIterator = list.listIterator(list.size());
        while (listIterator.hasPrevious()) {
            int num = listIterator.previous();
            listIterator.set(num + 1);
        }
```

listIterator()

```
public boolean add(E e) {
        synchronized (mutex) { // 对 add 方法加锁
            return list.add(e);
        }
    }
```

### 4.3 怎么实现 ArrayList 的线程安全？

面试官：怎么实现他的线程安全？

**不安全原因**：add() 方法里，elementData[size++]=e 不是原子操作，实际是先存值再自增 size。多线程时会发生一个线程的值覆盖另一个线程值的情况。例如两个线程同时查到 size 是 0，都往 0 的位置存各自的值，结果 0 的位置只会存到其中一个线程的值。

**解决方案**：

**方案一：考虑 add() 加锁**

*   **给 add() 加锁**：给 add() 加 synchronized **对象锁**，保证同一时刻，只有一个线程进入 add() 方法。这个我本地验证过是**可行**的，本地创建一个 java2.util2.ArrayList2（jdk 不允许核心包 java.util 创建用户自己的类），给 add() 方法加 synchronized，可以保证线程安全。
*   **代码块加对象锁**：给 add() 加锁可能粒度太大，实际只需给 ensureCapacityInternal(size + 1); 和 elementData[size++] = e; 这个代码块加对象锁。这也是 **Collections.synchronizedList()** 的原理。
    
    ```
    /**
     * @Author: vince
     * @CreateTime: 2024/09/10
     * @Description: 测试类
     * @Version: 1.0
     */
    public class Test2 {
     
        public static void main(String[] args) {
            ArrayList<Integer> list = new ArrayList<>();
     
            // 开启多个线程同时向 ArrayList 各添加1000个元素
            Thread thread1 = new Thread(() -> addElements(list, 1));
            Thread thread2 = new Thread(() -> addElements(list, 2));
     
            thread1.start();
            thread2.start();
     
            // 等待线程结束
            try {
                thread1.join();
                thread2.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
     
            // 打印最终结果
            System.out.println("容量应有2000，实有容量: " + list.size());
            System.out.println("元素: " + list);
        }
     
        /**
         * 添加1000个元素
         * @param list
         * @param threadId
         */
        private static void addElements(ArrayList<Integer> list, int threadId) {
            for (int i = 0; i < 1000; i++) {
                // 添加线程ID和计数器的组合值
                list.add(threadId * 1000 + i);
            }
        }
    }
    ```
    

**缺点**： 效率低。相当于一个人明明可以同时干多样活，但是你为了他的安全只让他干一个活，效率肯定就会变慢

**方案二：写时复制容器**

采用 CopyOnWriteArrayList。每次 add() 时，都会复制一个新容器给该线程。这样能保证一个容器只有一个线程。

*   **优点**：效率高。因为不用加锁，多线程时效率高。
*   **缺点**：空间损耗。每个线程一个容器占用了大量内存。

**验证线程不安全**：

创建一个 ArrayList，两个线程分别给它添加 1000 个元素，最后发现集合容量不是 2000：

```
/**
 * @Author: vince
 * @CreateTime: 2024/07/02
 * @Description: 狗类
 * @Version: 1.0
 */
public class Dog{
    /**
     * 体重
     */
    public int weight;
    /**
     * 名字
     */
    public String name;
 
    public Dog(int weight, String name) {
        this.weight = weight;
        this.name = name;
    }
 
    // @Override
    // public boolean equals(Object o) {
    //     Dog dog = (Dog) o;
    //     return weight == dog.weight && Objects.equals(name, dog.name);
    // }
    //
    // @Override
    // public int hashCode() {
    //     return Objects.hash(weight, name);
    // }
 
    @Override
    public String toString() {
        return "Dog{" +
                "weight=" + weight +
                ", name='" + name + '\'' +
                '}';
    }
}
```

  

![](<assets/1740030348332.png>)

### 4.4. 请你说说 ArrayList 和 LinkedList 的区别

**得分点**

数据结构（数组和链表）、访问增删效率、时间复杂度、内存占用率

**标准回答**

直接对比数组和链表的空间复杂度、对比插删查的时间复杂度、即可。

1. ArrayList 的实现是基于数组,**LinkedList** 的实现是**基于双向链表**。

2. 对于随机访问 ArrayList 要优于 LinkedList,ArrayList 可以根据下标以 O(1) 时间复杂度对元素进行随机访问, 而 LinkedList 的每一个元素都依靠地址指针和它后一个元素连接在一起, 查找某个元素的时间复杂度是 O(N)。

3. 对于插入和删除操作, LinkedList 要优于 ArrayList, 因为当元素被添加到 LinkedList 任意位置的时候, 不需要像 ArrayList 那样重新计算大小或者是更新索引。list 首部、中间插删时 ArrayList 时间复杂度 O(n)，因为要移动后面的元素，LinkedList 时间复杂度 O(1)，不需要移动。list 尾部插删时两者时间复杂度都是 O(1)，因为 LinkedList 是双向链表，有尾结点的指针。 

4. **LinkedList 比 ArrayList 更占内存**, 因为 LinkedList 的节点除了存储数据, 还存储了两个引用, 一个指向前一个元素, 一个指向后一个元素。 

**自然有序时间复杂度：**如果两者自然有序，ArrayList 可以使用二分查找，时间复杂度 O(logn)，LinkedList 可以使用跳跃表，时间复杂度 O(logn)。Redis 的 zset 底层是采用压缩列表或跳跃表。

**跳跃表：**链表基础上增加多级索引，跳跃查找：

![](<assets/1740030348597.png>)

### 4.5. 请你说说 HashMap 底层原理

**得分点**

底层数据结构、哈希表处理冲突、扩容机制、put() 流程、为什么 2 的指数扩容、死循环问题

**关键字：**数组初始 16、负载因子 0.75、2 的指数扩容、链表头结点地址、链表长度 8、红黑树、hash&(2^n-1）

**标准回答** 

HashMap 是线程不安全的，多线程环境下建议使用 Collections 工具类和 JUC 包的 ConcurrentHashMap。

##### **底层数据结构**

*   **JDK7：**HashMap 底层是采用 “数组 + 单向链表” 来实现的。数组用作哈希查找，链表用作链地址法**头插法**处理冲突。
*   **JDK8：**HashMap 底层是采用 “数组 + 单向链表 + **红黑树**” 来实现的。数组用作哈希查找，链表用作链地址法**尾插法**处理冲突，链表长度为 8 时转为红黑树。

##### HashMap 是否允许重复？

key 本质上是不能重复，如果两个 value 的 key 相同，到时候就无法准确读取 value 值了

但本质上相同不代表 “表面上” 不可以相同，当 HashMap 元素是类时，HashMap 判断两个元素是否重复不是直接通过他们的地址，而是通过该类的重写 hashCode()、equals()方法判断两个对象是否是一个对象。

**示例：**下面 Dog 类没有重写 hashCode()、equals() 方法，所以 HashMap 计算哈希值是通过两个对象的地址，因为地址不同，所以 HashMap 将两个名字、体重 Dog 对象认为是两只狗

```
/**
 * @Author: wangming
 * @CreateTime: 2024/09/10
 * @Description: 测试类
 * @Version: 1.0
 */
public class Test {
    public static void main(String[] args) {
        HashMap<Dog, Integer> dogCountMap = new HashMap<>();
        dogCountMap.put(new Dog(1, "dog1"), 2);
        dogCountMap.put(new Dog(1, "dog1"), 2);
        System.out.println(dogCountMap);
    }
}
```

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

![](<assets/1740030348725.png>)

##### **扩容机制** 

HashMap 中，数组的默认初始容量为 16，这个容量会以 2 的指数进行扩容。具体来说，当数组中的元素达到一定比例的时候 HashMap 就会扩容，这个比例叫做负载因子，默认为 0.75。

自动扩容机制，是为了保证 HashMap 初始时不必占据太大的内存，而在使用期间又可以实时保证有足够大的空间。采用 2 的指数进行扩容，是为了利用位运算，提高扩容运算的效率。

数组每个元素存的是链表头结点地址，链地址法处理冲突，若链表的长度达到了 8，红黑树代替链表。

##### **put() 流程**

put() 方法的执行过程中, 主要包含四个步骤：

1.  计算 key 存取位置，与运算 hash&(2^n-1），实际就是哈希值取余，位运算效率更高。
2.  判断数组，若发现数组为空，则进行首次扩容为初始容量 16。
3.  判断数组存取位置的头节点，若发现头节点为空，则新建链表节点，存入数组。
4.  判断数组存取位置的头节点，若发现头节点非空，则看情况将元素覆盖或插入链表（JDK7 头插法，JDK8 尾插法）、红黑树。
5.  插入元素后，判断元素的个数，若发现超过阈值则以 2 的指数再次扩容。

其中，第 3 步又可以细分为如下三个小步骤：

1. 若元素的 key 与头节点的 key 一致，则直接覆盖头节点。

2. 若元素为树型节点，则将元素追加到树中。

3. 若元素为链表节点，则将元素追加到链表中。追加后，需要判断链表长度以决定是否转为红黑树。若链表长度达到 8、元素个数未达到 64，则扩容。若链表长度达到 8、元素个数达到 64，则转为红黑树。

**哈希表处理冲突：**开放地址法（线性探测、二次探测、再哈希法）、链地址法

##### **HashMap 容量为什么是 2 的 n 次方？**

**主要有以下两个原因：** 

*   **提高性能**：当 n 是 2 的幂次方时，散列公式 (n - 1) & hash 的结果等于 hash%n。位运算比取余快，扩容时只需左移一位。
    *   例如 2^n-1 和 2^(n+1)-1 的二进制除了第一位，后几位都是相同的。扩容后只需要左移一次。例如 15 的二进制为 1111，31 的二进制为 11111，63 的二进制为 111111，127 的二进制为 1111111。
*   **使元素均匀分布在底层数组**：以 2 的指数扩容，可以保证每次扩容后，索引值改变的元素都移动到新增加的位置，而不会再移动到旧数组上的位置，可以减少哈希碰撞。

**例如：**从 2^4 扩容成 2^5，取余公式 key&(2^n-1)=index，index 是要存储在数组的下标。

0&(2^4-1)=0；0&(2^5-1)=0

16&(2^4-1)=0；16&(2^5-1)=16。所以扩容后，key 为 0 的一部分 value 位置没变，一部分 value 迁移到扩容后的新位置。

1&(2^4-1)=1；1&(2^5-1)=1

17&(2^4-1)=1；17&(2^5-1)=17。所以扩容后，key 为 1 的一部分 value 位置没变，一部分 value 迁移到扩容后的新位置。

##### **JDK7 扩容时死循环问题**

单线程扩容流程：JDK7 中，HashMap 链地址法处理冲突时采用头插法，在扩容时依然头插法，所以链表里结点顺序会反过来。

假如有 T1、T2 两个线程同时对某链表扩容，他们都标记头结点和第二个结点，此时 T2 阻塞，T1 执行完扩容后链表结点顺序反过来，此时 T2 恢复运行再进行翻转就会产生环形链表，即 B.next=A; A.next=B，从而死循环。

![](<assets/1740030349027.png>)

##### **JDK8 尾插法**

JDK8 中，HashMap 采用尾插法，扩容时链表节点位置不会翻转，解决了扩容死循环问题，但是性能差了一点，因为要遍历链表再查到尾部。 

##### **JDK8 put 时数据覆盖问题**

HashMap 非线程安全，如果两个并发线程插入的数据 hash 取余后相等，就可能出现数据覆盖。

线程 A 刚找到链表 null 位置准备插入时就阻塞了，然后线程 B 找到这个 null 位置插入成功。借着线程 A 恢复，因为已经判过 null，所以直接覆盖插入这个位置，把线程 B 插入的数据覆盖了。

```
public class Generic<T> {
    private T t;
 
    public T getT() {
        return t;
    }
 
    public void setT(T t) {
        this.t = t;
    }
}
```

##### **modCount 非原子性自增问题**

put 会执行 modCount++ 操作（modCount 是 HashMap 的成员变量，用于记录 HashMap 被修改次数），这步操作分为读取、增加、保存，不是一个原子性操作，也会出现线程安全问题。 

**线程安全解决方案：**

*   使用 Hashtable（古老 api 不推荐）
*   使用 Collections 工具类，将 HashMap 包装成线程安全的 HashMap。
    
    ```
    public <T>void show(T t){
            System.out.println(t);
        }
    ```
    
*   使用更安全的 ConcurrentHashMap（推荐），ConcurrentHashMap 通过对槽（链表头结点）加锁，以较小的性能来保证线程安全。
*   使用 synchronized 或 Lock 加锁 HashMap 之后，再进行操作，相当于多线程排队执行（比较麻烦，也不建议使用）。

HashMap 是**基于哈希算法来确定元素的位置**（槽）的, 当我们向集合中存入数据时, 它会计算传入的 Key 的哈希值, 并利用**哈希值取余**来确定槽的位置。如果元素发生碰撞, 也就是这个槽已经存在其他的元素了, 则 HashMap 会通过链表将这些元素组织起来（**链地址法处理冲突**）。如果碰撞进一步加剧, 某个**链表的长度达到了 8**, 则 HashMap 会创建**红黑树来代替这个链表**, 从而提高对这个槽中数据的查找的速度。

向 HashMap 中添加数据时, 有三个条件会触发它的扩容行为：

1. 如果数组为空, 则进行首次扩容。

2. 将元素接入链表后, 如果链表长度达到 8, 并且数组长度小于 64, 则扩容。

3. 添加后, 如果数组中元素超过阈值, 即比例超出限制（默认为 0.75）, 则扩容。 并且, 每次扩容时都是将容量翻倍, 即创建一个 2 倍大的新数组, 然后再将旧数组中的数组迁移到新数组里。由于 HashMap 中数组的容量为 2^N, 所以可以用位移运算计算新容量, 效率很高。

**加分回答 - HashMap 是非线程安全**

HashMap 是非线程安全的, 在多线程环境下, 多个线程同时触发 HashMap 的改变时, 有可能会发生冲突。所以, 在多线程环境下不建议使用 HashMap, 可以考虑使用 Collections 将 HashMap 转为线程安全的 HashMap, 更为推荐的方式则是使用 ConcurrentHashMap。

**红黑树：** 近似平衡二叉树，左右子树高差有可能大于 1，查找效率略低于平衡二叉树，但增删效率高于平衡二叉树，适合频繁插入删除。

*   结点非黑即红；
*   根结点是黑色，叶节点是黑色空节点（常省略）；
*   任何相邻节点不能同时为红色；
*   从任一结点到其每个叶子的所有路径都包含相同数目的黑色结点；
*   查询性能稳定 O(logN)，高度最高 2log(n+1)； 

##### 源码

*   **元素：**HashMap 的链表元素对应的是一个静态内部类 Entry，Entry 主要包含 key，value，next 三个元素
    
*   **put()：**通过 hash%Entry.length 计算 index，此时记作 Entry[index]= 该元素。如果 index 相同就是新入的元素放置到 Entry[index]，原先的元素记作 Entry[index].next
    
*   **get()**：先遍历数组，再遍历链表元素。
    
*   **空键**：null key 总是放在 Entry 数组的第一个元素
    
*   **再散列 rehash 的过程：**确定容量超过目前哈希表的容量，重新调整 table 的容量大小，当超过容量的最大值时，取 Integer.MAX_VALUE，即 2^31-1
    

### 4.6. 请你说说 ConcurrentHashMap

**得分点**

数组 + 链表 + 红黑树、锁的粒度、实现机制、多线程并发动态扩容、(数组长度>>> 3) / CPU 核心数、CAS

**标准答案** 

ConcurrentHashMap 是在 JUC（_Java.util.concurrent_）_并发包_下的一个类, 它相当于是一个线程安全的 HashMap。 

**数组 + 链表 + 红黑树：**ConcurrentHashMap 的底层数据结构与 HashMap 一样, 也是采用 “数组 + 链表 + 红黑树”

**锁的粒度：**

JDK1.7 采用分段锁，锁粒度是分段，JDK1.8 采用 synchronized+CAS，锁粒度是槽（头节点）

插入和扩容时采用锁定槽（头节点）的方式降低了锁粒度, 以较低的性能代价实现了线程安全。

**实现机制：**

1.  初始化数组、初始化头节点、并发扩容搬运数据时，ConcurrentHashMap 并没有加锁，而是 CAS 的方式进行原子替换。
2.  插入数据时，如果该数据是链表头结点则直接 CAS，如果不是头结点则对头结点加 synchronized 锁。
3.  在扩容的过程中，依然支持查找操作。

**插入元素的过程：**  
1.put 时首先通过 hash 找到对应链表过后, 查看是否是第一个 Node，如果是, 直接用 CAS 原则插入, 无需加锁。

2. 如果不是链表第一个 Node, 则直接用链表第一个 Node 加锁，这里加的锁是 synchronized 

**多线程并发扩容：**

当一个线程在插入数据时，若发现数组正在扩容，那么它就会立即参与扩容操作，完成扩容后再插入。

每个线程迁移数量是 ：(数组长度>>> 3) / CPU 核心数。

**CAS：**

不断对变量进行原子性比较和交换（比较内存中值和预期值，如果相等则交换，如果不相等就代表被其他线程改了则重试）。

CAS 属于乐观锁，乐观地认为并发不高，所以让线程不断去重试更新。

**优点：**不使用锁，所以性能高。CAS 在不使用锁的情况下通过原子性操作变量，实现多线程时的变量同步。

**缺点：**高并发时不断重试导致 CPU 开销过大。

**ABA 问题：**CAS 机制本身无法检测变量的值是否被修改过，只关心当前值和预期值。

例如，线程 A 刚读取完内存中变量值就阻塞，阻塞期间其他线程将变量更新后又恢复，这时线程 A 恢复，发现变量还是和预期值相等（其实变量已经不是原来那个变量了，它还以为是），就进行交换。

**解决 ABA 问题：**单靠 CAS 不能解决 ABA 问题，要用带版本号的原子引用才能解决。JDK5 引入 AtomicStampedReference 类，解决 ABA 问题。它有获取当前对象引用、获取当前版本号，cas 等方法。

stamped 译为贴上邮票的、盖上印章的。

底层数据结构的逻辑可以参考 HashMap 的实现, 下面我重点介绍它的线程安全的实现机制。

1. 初始化数组或头节点时, ConcurrentHashMap 并没有加锁, 而是 CAS（比较并交换）的方式进行原子替换（原子操作, 基于 Unsafe 类的原子操作 API）。

**CAS，**compare and swap，目的是在不使用锁的情况下实现多线程之间的**变量同步，保证变量操作的原子性。**解决多线程并行情况下使用锁造成性能损耗的一种机制。CAS 属于乐观锁，乐观地认为程序中的并发情况不那么严重，所以让线程不断去重试更新。

CAS 操作包含三个操作数——内存位置（V）、预期原值（A）和新值 (B)。**如果内存位置的值与预期原值相匹配，那么处理器会自动将该位置值更新为新值。**否则，处理器不做任何操作。无论哪种情况，它都会在 CAS 指令之前返回该位置的值。CAS 有效地说明了 “我认为位置 V 应该包含值 A；如果包含该值，则将 B 放到这个位置；否则，不要更改该位置，只告诉我这个位置现在的值即可。

2. 插入数据时会进行加锁处理, 但锁定的不是整个数组, 而是槽中的头节点。所以, ConcurrentHashMap 中锁的粒度是槽, 而不是整个数组, 并发的性能很好。

3. 扩容时会进行加锁处理, 锁定的仍然是头节点。并且, 支持多个线程同时对数组扩容, 提高并发能力。每个线程需先以 CAS 操作抢任务, 争抢一段连续槽位的数据转移权。抢到任务后, 该线程会锁定槽内的头节点, 然后将链表或树中的数据迁移到新的数组里。

4. **查找数据时并不会加锁**, 所以性能很好。另外, 在扩容的过程中, 依然可以支持查找操作。如果某个槽还未进行迁移, 则直接可以从旧数组里找到数据。如果某个槽已经迁移完毕, 但是整个扩容还没结束, 则扩容线程会创建一个转发节点存入旧数组, 届时查找线程根据转发节点的提示, 从新数组中找到目标数据。

**加分回答 - ConcurrentHashMap 多线程并发扩容**

ConcurrentHashMap 实现线程安全的难点在于多线程并发扩容，即**当一个线程在插入数据时，若发现数组正在扩容，那么它就会立即参与扩容操作**，完成扩容后再插入数据到新数组。

在扩容的时候，**多个线程共同分担数据迁移任务**，每个线程负责的**迁移数量是 ：(数组长度>>> 3) / CPU 核心数**。 也就是说，为线程分配的迁移任务，是**充分考虑了硬件的处理能力**的。多个线程依据硬件的处理能力，平均分摊一部分槽的迁移工作。另外，如果计算出来的迁移数量小于 16，则强制将其改为 16，这是考虑到目前服务器领域主流的 CPU 运行速度，每次处理的任务过少，对于 CPU 的算力也是一种浪费。

### 4.7. 请你说说 HashMap 和 Hashtable 的区别

**得分点**

线程安全、null 值、JDK1 古老 API 同步方案不成熟、ConcurrentHashMap

**标准回答**

HashMap 和 Hashtable 都是典型的 Map 实现, 它们的**区别在于是否线程安全, 是否可以存入 null 值。**

 1. **Hashtable** 在实现 Map 接口时保证了**线程安全性**, 而 HashMap 则是非线程安全的。所以, Hashtable 的性能不如 HashMap, 因为为了保证线程安全它**牺牲了一些性能**。

 2. **Hashtable 不允许存入 null,** 无论是以 null 作为 key 或 value, 都会引发异常。而 HashMap 是允许存入 null 的, 无论是以 null 作为 key 或 value, 都是可以的。

**加分回答 - Hashtable 是古老 api，推荐 ConcurrentHashMap**

虽然 Hashtable 是线程安全的, 但仍然不建议在多线程环境下使用 Hashtable。因为它是一个古老的 API, 从 Java 1.0 开始就出现了, 它的同步方案还不成熟, 性能不好。如果要在多线程环下使用 HashMap, 建议使用 ConcurrentHashMap。它不但保证了线程安全, 也通过降低锁的粒度提高了并发访问时的性能。

## 五、泛型 

### 5.1. 请你说说泛型、泛型擦除

**得分点**

泛型概念、范围、泛型擦除、好处、向上转型

**标准回答**

**泛型：**将具体的类型参数化，是一种编程范式，提供了编译时类型安全检测机制。

通过使用泛型，我们可以将数据类型作为参数传递给类、接口或方法，可以在编译时期进行类型检查，避免在运行时期出现类型转换错误。

**泛型的范围：**泛型接口，泛型类（创建对象时再指定具体类型），泛型方法。

**实现方式：**以泛型类举例。我们只需要在类名后面使用尖括号 <> 将一个符号或多个符号包裹起来，这样在类里面就可以使用该符号代替具体类型了。使用泛型类时，调用者实际传进来什么类型，编译时就会将泛型符号擦除，替换成这个实际类型。泛型符号可以是任意符号，但我们约定使用 T、E、K、V 等符号。

**泛型擦除：**java 的泛型是伪泛型，这是因为 java 在编译期间，所有的泛型类型都会被擦掉，并转换为普通类型。 

泛型擦除的主要目的是为了向低版本兼容，因为 Java 泛型是在 JDK 1.5 之后才引入的特性，为了保证旧有的代码能正常运行，Java 编译器采用了泛型擦除来兼容之前的代码。

**对比 Object 类：**泛型是在编译时泛型擦除和替换实际类型的，而使用 Object 类会很麻烦，需要经常强制转换。例如 List 集合里，如果直接声明存 Object 类的话，存的时候还好，可以通过多态机制直接向上转型，而取的时候就麻烦了，要强转 Object 类为 String 等对象，然后才能访问该对象的成员；而且你不知道实际元素到底是 String 类型还是 Integer 等其他类型，还要通过 i instanceof String 判断类型，就更麻烦了。

**泛型的好处：**

1.  **防止运行时报错：**可以在编译时检查类型安全，防止在程序运行期间出现 BUG。
2.  **隐式转换：**所有的强制转换都是自动和隐式的，可以提高代码的重用率。

**向上转型：**泛型类或接口可以向上转型为父类，泛型符号不能向上转型。泛型向上转型指的是将一个泛型对象转换为其父类类型或者接口类型的过程。这个过程实际上是将泛型对象的类型参数擦除，重新赋值为其父类或接口类型。

*   ArrayList<T> 可以向上转型为 List<T>
*   ArrayList<Integer> 泛型不可以向上转化为 ArrayList<Number>。因为 ArrayList<Number > 接收 ArrayList<float>，但 ArrayList< Integer > 不可以接收 ArrayList< Float>，不能转回来 

除了向上转型，也有向下转型。 

**泛型类：**

```
public class ArrayList<t> implements List<t> { ... }
 
List<string> list = new ArrayList<string>();
```

**泛型接口：**

```
// 创建ArrayList<integer>类型：
        ArrayList<Integer> integerList = new ArrayList<>(); // 添加一个Integer：
        integerList.add(new Integer(123)); // “向上转型”为ArrayList<number>：
        ArrayList<Number> numberList = integerList; // 添加一个Float,因为Float也是Number：
        numberList.add(new Float(12.34)); // 从ArrayList<integer>获取索引为1的元素（即添加的Float）：
        Integer n = integerList.get(1); // ClassCastException!
```

**泛型方法：**

```
public class Test {
    public static void main(String[] args) {
        fun();
    }
 
    private static void fun() {
        fun1();
    }
 
    private static void fun1() {
        System.out.println(6/0);
    }
}
```

**编译时安全检查：**

Java 在 1.5 版本中引入了泛型, **在没有泛型之前, 每次从集合中读取对象都必须进行类型转换**, 而这么做带来的结果就是：如果有人不小心插入了类型错误的对象, 那么在运行时转换处理阶段就会出错。

在提出**泛型**之后, 我们可以告诉编译器集合中接受哪些对象类型。编译器会**自动的为你的插入进行转化, 并在编译时告知是否插入了类型错误的对象**。这使程序变得更加安全更加清楚

**加分回答** **- 向上转型**

在 Java 标准库中的 `ArrayList<t>` 实现了 `List<t>` 接口, 它可以向上转型为 `List<t>`：  
 

```
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at package1.Test.fun1(Test.java:17)
	at package1.Test.fun(Test.java:13)
	at package1.Test.main(Test.java:9)
```

即**类型 `ArrayList<t>` 可以向上转型为 `List<t>`**。

**注意：**

**不能把 `ArrayList<Integer>` 向上转型为 `ArrayList<Number>` 或 `List<Number>`。** 这是为什么呢？

**假设** `ArrayList<Integer>` 可以向上转型为 `ArrayList<Number>`, 观察一下代码：

```
// 创建ArrayList<integer>类型：
        ArrayList<Integer> integerList = new ArrayList<>(); // 添加一个Integer：
        integerList.add(new Integer(123)); // “向上转型”为ArrayList<number>：
        ArrayList<Number> numberList = integerList; // 添加一个Float,因为Float也是Number：
        numberList.add(new Float(12.34)); // 从ArrayList<integer>获取索引为1的元素（即添加的Float）：
        Integer n = integerList.get(1); // ClassCastException!
```

我们把一个 `ArrayList<integer>` 转型为 `ArrayList<number>` 类型后, 这个 `ArrayList<number>` 就可以接受 `Float` 类型, 因为 `Float` 是 `Number` 的子类。但是,`ArrayList<number>` 实际上和 `ArrayList<integer>` 是同一个对象, 也就是 `ArrayList<integer>` 类型, 它不可能接受 `Float` 类型, 所以在获取 `Integer` 的时候将产生 `**ClassCastException**`。

实际上, 编译器为了避免这种错误, 根本就不允许把 `ArrayList<integer>` 转型为 `ArrayList<number>`。

## 六、IO 

### 6.1. 请你说说 IO 多路复用

**得分点**

特点（单线程可以处理多个客户端请求）、优势

**标准回答**

IO 多路复用：一个线程处理多个 IO 操作。

一个线程中同时监听多个文件句柄（文件描述符，标记已打开文件的正整数），一旦有文件就绪（可读或可写）时，会通知应用程序进行读写操作。没有文件句柄就绪时就会阻塞应用程序，从而释放出 CPU 资源。

*   lO： 在操作系统中，数据在内核态和用户态之间的读写操作。大部分情况下指网络 IO。
*   多路：多个 IO 操作。大部分情况下是指多个 TCP 连接 (多个 Socket 或者多个 Channel)
*   复用：复用同一个线程。
*   IO 多路复用：一个线程处理多个 IO 操作，无需创建和维护过多的进程 / 线程。
*   文件描述符：写描述符、读描述符、异常描述符

**优势：**系统开销小，不需要创建、维护额外进程或者线程，避免了线程切换带来的开销。

实现 I/O 多路复用的三种模型：select、poll、epoll。 

**select 调用：**底层是数组，通过轮询遍历文件描述符集合的方式监听文件，扫到就绪的文件描述符，通知应用程序读写。跨平台好（几乎所有平台都支持），轮询次数多，效率差（O(n)），内存开销大，支持文件描述符的个数有限。

**poll 调用：**底层是单向链表。轮询次数多、系统开销大，不限制文件描述符个数（最大为操作系统最大文件句柄数，1G 内存支持 10 万个句柄）。

**epoll 调用：**底层的数据结构为红黑树加链表，红黑树存所有事件，链表存就绪事件，内核查红黑树将就绪事件插入链表，epoll 调用通过 epoll_wait 函数返回内核的就绪事件列表，通知应用程序读写操作。回调效率高（O(1)），不会随着文件描述符个数增多导致性能下降，不限制文件描述符个数。缺点是只能在 Linux 工作。

IO 多路复用其实就是基于 NIO 的基础上加入了事件机制，程序会注册一组 socket 文件描述符给操作系统，然后监视这些 fd 是否有 IO 事件发生，如果有，程序会被通知，IO 多路复用的方式主要有 select、poll、epoll，这三个函数都会进行阻塞，所以可以放在 while(true) 循环里使用，不会造成 CPU 的空转。

在 I/O 编程过程中, 当需要同时处理**多个客户端接入请求时**, 可以利用多线程或者 I/O 多路复用技术进行处理。I/O 多路复用技术通过把多个 I/O 的阻塞复用到同一个 select 的阻塞上, 从而使得系统在**单线程的情况下可以同时处理多个客户端请求。**

与传统的多线程 / 多进程模型比, I/O 多路复用的**最大优势**是**系统开销小**, 系统不需要创建新的额外进程或者线程, 也不需要维护这些进程和线程的运行, 降低了系统的维护工作量, 节省了系统资源。

目前**支持 I/O 多路复用**的系统调用有 select、pselect、poll、epoll, 在 Linux 网络编程过程中, 很长一段时间都使用 select 做轮询和网络事件通知, 然而 select 的一些固有缺陷导致了它的应用受到了很大的限制, 最终 Linux 不得不在新的内核版本中寻找 select 的替代方案, 最终选择了 epoll。

**epoll 调用**

在使用 epoll 调用时，不需要轮询文件描述符集合。相比于 select 和 poll 调用需要轮询整个文件描述符集合的方式来等待 I/O 事件的发生，epoll 调用使用**一组专门的系统调用来管理和等待 I/O 事件**，从而避免了文件描述符集合的轮询，提高了处理效率。

具体地说，epoll 调用使用了一组系统调用 **(epoll_create、epoll_ctl 和 epoll_wait)** 来实现 I/O 多路复用机制。当应用程序调用 epoll_wait 时，内核会等待任何一个被监控的文件描述符上的事件（例如套接字事件、管道事件等）。此时**内核会把就绪的事件记入到内存中的一个事件列表中**，应用程序再通过 epoll_wait 函数**返回这个就绪事件列表**。在整个等待过程中，应用程序并不需要自己不停地轮询文件描述符集合，这样可以极大地减少系统调用和 CPU 资源的占用。

总之，epoll 调用不需要轮询整个文件描述符集合，这是它相比于 select 和 poll 等调用的一个关键优势。因此，在高并发的网络应用程序中，epoll 调用更加适用且更加高效。

![](<assets/1740030349164.png>)

**加分回答 - epoll 调用对比 select**

epoll 与 select 的原理比较类似, 为了克服 select 的缺点, epoll 作了很多重大改进：

1. 支持一个进程打开的 socket 描述符（FD）不受限制 **select 最大的缺陷就是单个进程所打开的 FD 是有一定限制的**, 它由 FD_SETSIZE 设置, 默认值是 1024。对于那些需要支持上万个 TCP 连接的大型服务器来说显然太少了。可以选择修改这个宏然后重新编译内核, 不过这会带来网络效率的下降。我们也可以通过选择多进程的方案（传统的 Apache 方案）解决这个问题, 不过虽然在 Linux 上创建进程的代价比较小, 但仍旧是不可忽视的, 另外, 进程间的数据交换非常麻烦, 对于 Java 由于没有共享内存, 需要通过 Socket 通信或者其他方式进行数据同步, 这带来了额外的性能损耗, 增加了程序复杂度, 所以也不是一种完美的解决方案。值得庆幸的是, epoll 并没有这个限制, 它所支持的 FD 上限是操作系统的最大文件句柄数, 这个数字远远大于 1024。例如, 在 1GB 内存的机器上大约是 10 万个句柄左右, 具体的值可以通过 cat /proc/sys/fs/file- max 察看, 通常情况下这个值跟系统的内存关系比较大。

2. I/O 效率不会随着 FD 数目的增加而线性下降 传统的 select/poll 另一个致命弱点就是当你拥有一个很大的 socket 集合, 由于网络延时或者链路空闲, 任一时刻只有少部分的 socket 是 “活跃” 的, 但是 select/poll 每次调用都会线性扫描全部的集合, 导致效率呈现线性下降。epoll 不存在这个问题, 它只会对 “活跃” 的 socket 进行操作 - 这是因为在内核实现中 epoll 是根据每个 fd 上面的 callback 函数实现的, 那么, 只有 “活跃” 的 socket 才会主动的去调用 callback 函数, 其他 idle 状态 socket 则不会。在这点上, epoll 实现了一个伪 O。针对 epoll 和 select 性能对比的 benchmark 测试表明：如果所有的 socket 都处于活跃态 - 例如一个高速 LAN 环境, epoll 并不比 select/poll 效率高太多；相反, 如果过多使用 epoll_ctl, 效率相比还有稍微的下降。但是一旦使用 idle connections 模拟 WAN 环境, epoll 的效率就远在 select/poll 之上了。

3. 使用 mmap 加速内核与用户空间的消息传递 无论是 select,poll 还是 epoll 都需要内核把 FD 消息通知给用户空间, 如何避免不必要的内存复制就显得非常重要, epoll 是通过内核和用户空间 mmap 同一块内存实现。

4. epoll 的 API 更加简单 包括创建一个 epoll 描述符、添加监听事件、阻塞等待所监听的事件发生, 关闭 epoll 描述符等。

### 6.2. 请你说说 BIO、NIO、O

**得分点**

阻塞 IO 模型、非阻塞 IO 模型、异步 IO 模型

**标准回答**

**BIO：阻塞 I/O 模型，同步并阻塞**。一个线程只能处理一个连接。客户端有连接请求时服务器需要特地启动一个线程处理。

阻塞：如果连接不做事，对应线程会一直被阻塞，导致不必要的线程开销。 

**NIO：非阻塞 I/O 模型，同步非阻塞**，一个线程通过多路复用器可以处理多个连接（即请求）。客户端发送的连接会通过通道注册到多路复用器上，多路复用器轮询所有就绪的通道处理 IO。访问数据都是通过缓冲区操作。IO 多路复用 = NIO + 事件机制。

三个核心组件：Buffer（缓冲区）、Channel（通道）、Selector（多路复用器）。

非阻塞：一个连接不做事，线程不会被阻塞，可以轮询处理其他连接。

**AIO：异步 I/O 模型，异步非阻塞**，一个有效请求对应一个线程。应用程序把 I/O 请求交给 os（操作系统）多路复用处理，自己则可以执行其他任务。os 处理完后再通知应用程序进行后续处理。

异步：应用程序把 IO 请求交给 os 后自己就能做其他事了。

非阻塞：os 多路复用处理 IO 请求。

根据 UNIX 网络编程对 I/O 模型的分类, UNIX 提供了 5 种 I/O 模型, 分别是阻塞 I/O 模型、非阻塞 I/O 模型、I/O 复用模型、信号驱动 I/O 模型、异步 I/O 模型。

BIO、NIO、O 这五种模型中的三种, 它们分别是阻塞 I/O 模型、非阻塞 I/O 模型、异步 I/O 模型的缩写。

**BIO，阻塞 I/O 模型（blocking I/O）：**是**最常用**的 I/O 模型, 缺省情形下, 所有文件操作都是阻塞的。

我们以套接字接口为例来理解此模型, 即在进程空间中调用 recvfrom, 其系统调用直到数据包到达且被复制到应用进程的缓冲区中或者发生错误时才返回, 在此期间一直会等待, 进程在从调用 recvfrom 开始到它返回的整段时间内都是被阻塞的, 因此被称为阻塞 I/O 模型。

![](<assets/1740030349371.png>)

**NIO，非阻塞 I/O 模型（nonblocking I/O）：**recvfrom 从应用层到内核的时候, 如果该缓冲区没有数据的话, 就直接返回一个 EWOULDBLOCK 错误, 一般都对非阻塞 I/O 模型进行轮询检查这个状态, 看内核是不是有数据到来。

![](<assets/1740030349598.png>)

**AIO，异步 I/O 模型（asynchronous I/O）：**告知内核启动某个操作, 并让内核在整个操作完成后（包括将数据从内核复制到用户自己的缓冲区）通知我们。

这种模型与信号驱动模型的主要区别是：信号驱动 I/O 由内核通知我们何时可以开始一个 I/O 操作, 异步 I/O 模型由内核通知我们 I/O 操作何时已经完成。

**加分回答**

**I/O 复用模型（I/O multiplexing）：**Linux 提供 select/poll, 进程通过将一个或多个 fd 传递给 select 或 poll 系统调用, 阻塞在 select 操作上, 这样 select/poll 可以帮我们侦测多个 fd 是否处于就绪状态。select/poll 是顺序扫描 fd 是否就绪, 而且支持的 fd 数量有限, 因此它的使用受到了一些制约。Linux 还提供了一个 epoll 系统调用, epoll 使用基于事件驱动方式代替顺序扫描, 因此性能更高。当有 fd 就绪时, 立即回调函数 rollback。

**信号驱动 I/O 模型（signal-driven I/O）：**首先开启套接口信号驱动 I/O 功能, 并通过系统调用 sigaction 执行一个信号处理函数（此系统调用立即返回, 进程继续工作, 它是非阻塞的）。当数据准备就绪时, 就为该进程生成一个 SIGIO 信号, 通过信号回调通知应用程序调用 recvfrom 来读取数据, 并通知主循环函数处理数据。

### 6.3. 请你讲一下 Java NIO

**得分点**

同步非阻塞、Buffer、Channel、Selector

**标准回答**

**NIO：非阻塞 I/O 模型，同步非阻塞**，一个线程通过多路复用器可以处理多个连接（即请求）。客户端发送的连接会通过通道注册到多路复用器上，多路复用器轮询所有就绪的通道处理 IO。访问数据都是通过缓冲区操作。JDK1.4 出现，底层是 IO 多路复用的 epoll 调用。IO 多路复用 = NIO + 事件机制。

三个核心组件：Buffer（缓冲区）、Channel（通道）、Selector（多路复用器）。

非阻塞：一个连接不做事，线程不会被阻塞，可以轮询处理其他连接。

![](<assets/1740030349808.png>)

新的输入 / 输出（NIO）库是在 **JDK 1.4 中引入**的。NIO 弥补了原来同步阻塞 I/O 的不足, 它在标准 Java 代码中提供了高速的、面向块的 I/O。通过定义包含数据的类, 以及通过以块的形式处理这些数据。

**NIO 包含三个核心的组件：Buffer（缓冲区）、Channel（通道）、Selector（多路复用器）。**

**Buffer** 是一个对象, 它包含一些**要写入或者要读出的数据**。在 NIO 类库中加入 Buffer 对象, 体现了新库与原 I/O 的一个重要区别。在面向流的 I/O 中, 可以将数据直接写入或者将数据直接读到 Stream 对象中。**在 NIO 库中, 所有数据都是用缓冲区处理的**。在读取数据时, 它是直接读到缓冲区中的。在写入数据时, 写入到缓冲区中。**任何时候访问 NIO 中的数据, 都是通过缓冲区进行操作。**

**Channel 是一个通道,** 可以通过它读取和写入数据, 它就像自来水管一样, 网络数据通过 Channel 读取和写入。通道与流的不同之处在于**通道是双向的**, 流只是在一个方向上移动而且**通道可以用于读、写或者同时用于读写**。因为 Channel 是全双工的, 所以它可以比流更好地映射底层操作系统的 API。特别是在 UNIX 网络编程模型中, 底层操作系统的通道都是全双工的, 同时支持读写操作。

**Selector 会不断地轮询注册在其上的 Channel,** 如果某个 Channel 上面有新的 TCP 连接接入、读和写事件, 这个 **Channel 就处于就绪状态, 会被 Selector 轮询出来**, 然后通过 SelectionKey 可以获取就绪 Channel 的集合, 进行后续的 I/O 操作。**一个多路复用器 Selector 可以同时轮询多个 Channel,** 由于 JDK 使用了 epoll() 代替传统的 select 实现, 所以它并没有最大连接句柄 1024/2048 的限制。这也就意味着只需要一个线程负责 Selector 的轮询, 就可以接入成千上万的客户端, 这确实是个非常巨大的进步。

**加分回答**

Java 7 的 NIO2 提供了异步 Channel 支持, 这种异步 Channel 可以提供更高效的 IO, 这种基于异步 Channel 的 IO 机制也被称为异步 IO（AsynchronousIO）。NIO2 为 O 提供了两个接口和三个实现类, 其中 AsynchronousSocketChannel 和 AsynchronousServerSocketChannel 是支持 TCP 通信的异步 Channel。

## 七、异常  

### 7.0. 说说异常体系

 Throwable、Error、Exception、编译时异常、RuntimeException、常见的 RuntimeException

**Throwable 类**

Throwable 是所有异常和错误（Exception 和 Error）的基类，译为可抛出的。

Throwable 最重要的两个方法：

*   **getMessage()：**返回此 throwable 的详细信息字符串。
    
*   **printStackTrace()：**将 throwable 的跟踪栈信息输出到标准错误输出。它包含 throwable 的类名、消息，以及每个方法的调用序列。
    

**Error 类**

Java 中的 Error 类表示严重的错误情况，通常由虚拟机或其他底层自身的失效造成的，例如内存溢出、栈溢出，会导致应用程序终止。

通常程序不应该捕获 Error，特定情境下可以捕获 OutOfMemoryError 处理内存溢出问题。使用 try-catch-finally 块捕获异常，并在 finally 块中进行资源清理、销毁、报告错误、终止应用程序等操作。

常见的错误包括：

*   **OutOfMemoryError：**内存溢出错误，通常是由于应用程序试图分配比可用内存更多的内存而导致。
*   StackOverflowError：堆栈溢出错误，发生在方法递归调用所需的堆栈空间已经用完的情况下。
*   NoClassDefFoundError：类未找到错误，通常是由于 JVM 无法找到应用程序尝试使用的某个类而导致。
*   UnsatisfiedLinkError：链接未满足错误，通常是由于调用本地方法时出现的链接问题而导致。

**Exception 类** 

Exception 的子类包括编译时异常和运行时异常：

*   **编译时异常：**在编译阶段就能检查出来的异常。例如 FileNotFoundException、ClassNotFoundException、NoSuchFieldException、NoSuchMethodException、SQLException、ParseException（解析异常）等。如果程序要去处理这些异常，必须显式地使用 try-catch 语句块或者在方法定义中使用 throws 子句声明异常。
    
*   **运行时异常：**在运行时才会出现的异常。例如，NullPointerException、ArrayIndexOutOfBoundsException 等。这些异常通常是**由程序代码中的逻辑错误引起的，**在编程时不会提示，运行时才报错。 因此，在编写程序时，通常无法处理这些异常，但是在程序开发完毕后，需要对这些异常进行一些处理，以防程序运行时崩溃。
    
    *   NullPointerException **空指针异常**；出现原因：访问未初始化的对象或不存在的对象。
    *   ClassNotFoundException **类找不到异常**；出现原因：类的名称和路径加载错误；
    *   NumberFormatException **数字格式化异常**；出现原因：转数字的字符型中包含非数字型字符。
    *   IndexOutOfBoundsException **索引超出边界异常**；出现原因：访问数组越界元素
    *   IllegalArgumentException **不合法参数异常**。出现原因：传递了不合法参数
    *   MethodArgumentNotValidException **方法参数无效异常**。出现原因：JSR303 校验不通过
    *   ClassCastException **类转换异常**。出现原因：把对象强制转为没继承关系对象时报错。这个异常是在类加载过程的元数据验证阶段验证继承关系时报错。
    *   ArithmeticException **算术异常**。出现原因：除以 0 时。  

![](<assets/1740030349906.png>)

### 7.1. 请你说说 Java 的异常处理机制

**得分点**

异常处理、禁止 finally 里 throw 或 return、抛出异常、异常的跟踪栈、统一异常处理

**标准回答** 

Java 的异常机制可以分成异常处理、抛出异常和异常跟踪栈的问题三个部分。

**异常处理：**处理异常的语句由 try、catch、finally 三部分组成。try 块用于包裹业务代码，catch 用于捕获并处理某个异常，finally 块则用于回收资源。

**禁止 finally 里 throw 或 return：**防止 try-catch 里抛异常失效。如果在 finally 里 throw 或 return，会直接退出异常，无法跳回 try 或 catch 执行 return 或 throw。正常情况下，当 finally 块执行完成后, 系统才会再次跳回来执行 try 块或 catch 块里的 return 或 throw 语句。

**抛出异常：**throws 只能在方法签名中使用，可以抛多个异常，表示出现异常的一种可能性。throw 表示抛出一个确定的异常实例。

**异常的跟踪栈：**

程序出现异常后会打印异常的跟踪栈信息，根据跟踪栈信息我们可以找到异常的位置，并跟踪异常一路向上层方法传播的过程。

异常传播的顺序与方法的调用顺序相反，是从发生异常的方法开始，不断向调用它的上层方法传播，最终传到 main 方法。若依然没有得到处理，则 JVM 终止程序，打印异常跟踪栈的信息。

**日志规范不建议使用 e.printStackTrace()：**改成 log.error("你的程序有异常啦",e);  
日志混乱：e.printStackTrace() 打印出的堆栈日志跟业务代码日志是交错混合在一起的，通常排查异常日志不太方便。  
性能问题：printStackTrace() 方法会生成大量的字符串对象，对系统性能有一定的影响。  
 

**统一异常处理：**

1.  公共模块创建错误码枚举类，一般为五位数字，前两位代表不同业务场景，后三位表示错误码；
2.  在每个模块异常包下创建异常处理类，类注解  
    @RestControllerAdvice，各异常处理方法注解 @ExceptionHandler，处理后返回带错误码的结果类；
3.  实际开发时出现异常或抛出异常就会直接被拦截处理，不用 try-catch 捕获处理。

异常处理机制可以让程序具有极好的容错性和健壮性, 当程序运行出现了意料之外的状况时, 系统会生成一个 **Exception 对象**来通知程序, 从而实现 “业务功能实现部分代码” 与“错误处理部分代码”分离, 使程序获得更好的可读性。

Java 的异常机制可以分成**异常处理、抛出异常和异常跟踪栈问题**三个部分。

**异常处理** 

处理异常的语句由 try、catch、finally 三部分组成。try 块用于包裹业务代码, catch 块用于捕获并处理某个类型的异常, finally 块则用于回收资源。如果业务代码发生异常, 系统就会创建一个异常对象, 并将这个异常对象提交给 JVM, 然后由 JVM 寻找可以处理这个异常的 catch 块, 并将异常对象交给这个 catch 块处理。如果 JVM 没有找到可以处理异常的 catch 代码块, 那么运行环境会终止, Java 程序也会退出。若业务代码打开了某项资源, 则可以在 finally 块中关闭这项资源, 因为无论是否发生异常, finally 块一定会执行（一般情况下）。

**抛出异常** 

当程序出现错误时, 系统会自动抛出异常。除此以外,**Java 也允许程序主动抛出异常**。当业务代码中, 判断某项错误的条件成立时, 可以使用 **throw 关键字向外抛出异常**。在这种情况下, 如果当前方法不知道该如何处理这个异常, 可以在方法签名上通过 **throws 关键字声明抛出异常**, 则该异常将**交给 JVM 处理**。

**异常跟踪栈**

程序运行时, 经常会发生一系列方法调用, 从而形成方法调用栈。异常机制会导致异常在这些方法之间传播, 而**异常传播的顺序与方法的调用相反**。**异常从发生异常的方法向外传播**, 首先传给该方法的调用者, 再传给上层调用者, 以此类推。**最终会传到 main 方法**, 若依然没有得到处理, 则 JVM 会终止程序, 并打印异常跟踪栈的信息。

**示例：**

```
public class Test {
    public static void main(String[] args) {
        fun();
    }
 
    private static void fun() {
        fun1();
    }
 
    private static void fun1() {
        System.out.println(6/0);
    }
}
```

先打印出问题的 fun1()，再打印调用它的 fun2()，再打印调用它的 main 方法：

```
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at package1.Test.fun1(Test.java:17)
	at package1.Test.fun(Test.java:13)
	at package1.Test.main(Test.java:9)
```

**加分回答 - throw、throws 区别, 避免在 finally 块中使用 return 或 throw**

**throws：**

 - 只能在方法签名中使用

 - 可以声明抛出多个异常, 多个一场之间用逗号隔开

 - 表示**当前方法不知道如何处理这个异常**, 这个异常由该方法的调用者处理（如果 mn 方法也不知该怎么处理异常, 这个异常就会交给 JVM 处理, JVM 处理异常的方式是, 打印异常跟踪栈信息并终止程序运行, 这也就是为什么程序遇到异常会自动结束的的原因）

 - throws 表示出现异常的**一种可能性**, 并不一定会发生这些异常

**throw：**

 - 表示方法内抛出某种异常对象, throw 语句可以单独使用。

 - throw 语句**抛出的是一个异常实例**, 不是一个异常类, 而且**每次只能抛出一个异常实例**

 - 执行 throw **一定抛出**了某种异常

**避免在 finally 块中使用 return 或 throw**

当 Java 程序执行 try 块、catch 块时遇到了 **return 或 throw 语句**, 这两个语句都**会导致该方法立即结束**, 但是系统执行这两个语句并不会结束该方法, 而是去寻找该异常处理流程中**是否包含 finally 块**, 如果没有 finally 块, 程序立即执行 return 或 throw 语句, 方法终止；如果有 finally 块, 系统立即开始执行 finally 块。只有**当 finally 块执行完成后**, 系统才会再次**跳回来执行** try 块、catch 块里的 **return 或 throw 语句**；

如果 finally 块里也使用了 return 或 throw 等语句, finally 块会终止方法, 系统将不会跳回去执行 try 块、catch 块里的任何代码。这将会导致 try 块、catch 块中的 return、throw 语句失效, 所以, 我们应该**尽量避免在 finally 块中使用 return 或 throw。**

**finally 代码块不执行的几种情况：**

 - 如果当一个线程在执行 try 语句块或者 catch 语句块时被打断 interrupted 或者被终止 killed, 与其相对应的 finally 语句块可能不会执行。

 - 如果在 try 块或 catch 块中使用 `System.exit(1);` 来退出虚拟机, 则 finally 块将失去执行的机会。

## 八、反射 

### 8.1. 请说说你对反射的了解

**得分点**

反射概念、通过反射机制可以实现、获取 Class 对象的三种方式、优缺点、应用场景

**反射：**在程序运行期间**动态地获取类的信息并对类进行操作**的机制。

**通过反射机制可以实现：**

*   **获取类或对象的 Class 对象：**程序运行时, 可以通过反射获得任意一个类的 Class 对象, 并通过这个对象查看这个类的所有方法和属性（包括私有，私有需要给该字段调用 setAccessible(true) 方法开启私有权限）。注意类的 class 对象是运行时生成的，类的 class 字节码文件是编译时生成的。
*   **创建实例：**程序运行时, 可以利用反射先创建类的 Class 对象再创建该类的实例, 并访问该实例的成员；Xxx.class.newInstance() ; 例如在 Spring 容器类源码里，Bean 实例化就是通过 Bean 类的 Class 对象。Bean 类的 Class 对象是从 BeanDefinition 对象的 type 成员变量取的。BeanDefinition 对象存储一些 Bean 的类型、名称、作用域等声明信息。
*   **生成动态代理类或对象：**程序运行时, 可以通过反射机制生成一个类的动态代理类或动态代理对象。例如 JDK 中 Proxy 类的 newProxyInstance 静态方法，可以通过它创建基于接口的动态代理对象。

**获取类 Class 对象的 JVM 底层：**如果该类没有被加载过，会首先通过 JVM 实现类的加载过程，即加载、链接（验证、准备、解析）、初始化，加载阶段会生成类的 Class 对象。

**获取类 Class 对象的方法：**dog.getClass();Dog.class;Class.forName("package1.Dog");

**反射的优缺点：**

*   **优点：**
    *   **运行时获取属性：**运行期间能够动态的获取类，提高代码的灵活性。
    *   **访问私有成员：**构造方法、成员变量、方法对象取消访问检查可以访问私有成员；public void setAccessible(boolean flag): 值为 true，取消访问检查
    *   **越过泛型检查：**反射可以越过泛型检查，例如在 ArrayList<Integer> 中添加字符串
*   **缺点：性能差。**性能比直接的 Java 代码要差很多。  

**应用场景：**

*   **JDBC 加载数据库的驱动：**使用 JDBC 时，如果要创建数据库的连接，则需要先通过反射机制加载数据库的驱动程序；
*   **Bean 的生命周期：**
    *   **实例化 xml 解析出的类：**多数框架都支持注解或 XML 配置来定义应用程序中的类，从 xml 配置中解析出来的类是字符串，需要利用反射机制实例化；例如 Spring 通过 <bean> 和 < property name="按名称注入" ref="被注入 Bean 的 id"> 定义 bean，然后通过 Class.forName("xxx.Xxx") 获取类的 class 对象，然后创建实例。
    *   **注解容器类加载 Bean、实例化 Bean：**Bean 的生命周期中，注解容器类的构造方法会遍历 @ComponentScan("扫描路径") 下的. class 文件，通过类加载器. load("类名") 方式获得类的 class 对象，存入 beanDefinitionMap。然后遍历 beanDefinitionMap，通过 class 对象实例化等。
*   **AOP 创建动态代理对象：**面向切面编程（AOP）的实现方案，是在程序运行时创建目标对象的代理对象，这必须由反射机制来实现。 

**类加载过程：**加载、链接（验证、准备、解析）、初始化。这个过程是在类加载子系统完成的。

**加载：**生成类的 Class 对象。

1.  通过一个类的全限定名获取定义此类的二进制字节流
2.  将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构。包括创建运行时常量池，将类常量池的部分符号引用放入运行时常量池。
3.  在内存中生成一个代表这个类的 java.lang.Class 对象，作为方法区这个类的各种数据的访问入口。注意类的 class 对象是运行时生成的，类的 class 字节码文件是编译时生成的。

**链接：**将类的二进制数据合并到 JRE 中。该过程分为以下 3 个阶段：

*   **验证：**确保代码符合 JAVA 虚拟机规范和安全约束。包括文件格式验证、元数据验证、字节码验证、符号引用验证。
    *   **文件格式验证：**验证字节流是否符合 Class 文件格式规范。例如版本号是否在 JVM 兼容范围。
    *   **元数据验证：**元数据是类的全名、方法信息、字段信息、继承关系等。例如验证类名接口名标识符有没有符合规范、有没有实现接口的所有方法、有没有实现抽象类的所有抽象方法、是不是继承了 final 类等。
    *   **字节码验证：**对字节码进行验证, 保证校验的类在运行时不会做出对 JVM 危险的行为。例如强制把父类对象强转为子类对象（只有父类引用指向子类对象时才能向下转型）。
    *   **符号引用验证：**验证引用的对象是否存在，是否有权引用等。
*   **准备：**为类变量（即 static 变量）分配内存并赋零值。
*   **解析：**将方法区 - 运行时常量池内的符号引用（类的名字、成员名、标识符）转为直接引用（实际内存地址，不包含任何抽象信息，因此可以直接使用）。

**初始化：**类变量赋初值、执行静态语句块。

**aop：**

**一种编程思想，在不改原有代码的前提下对代码进行增强。**

*   **目标对象 (Target)：**原始功能去掉共性功能对应的类产生的对象，这种对象是无法直接完成最终工作的
*   **代理 (Proxy)：**目标对象无法直接完成工作，需要对其进行功能回填，通过原始对象的代理对象实现

SpringAOP 是在不改变原有设计 (代码) 的前提下对其进行增强的，它的底层采用的是代理模式实现的，所以要对原始对象进行增强，就需要**对原始对象创建代理对象，在代理对象中的方法把通知内容** [如: MyAdvice 中的 method 方法] **加进去**，就实现了增强, 这就是我们所说的代理 (Proxy)。

**反射**

Java 程序中, 许多对象在运行时都会有编译时异常和运行时异常两种, 例如多态情况下 Car c = new Audi(); 这行代码运行时会生成一个 c 变量, 在编译时该变量的类型是 Car, 运行时该变量类型为 Audi；另外还有更极端的情况, 例如程序在运行时接收到了外部传入的一个对象, 这个对象的编译时类型是 Object, 但程序又需要调用这个对象运行时类型的方法, 这种情况下, 有两种解决方法：

第一种做法是假设在编译时和运行时都完全知道类型的具体信息, 在这种情况下, 可以先使用 instanceof 运算符进行判断, 再利用强制类型转换将其转换成其运行时类型的变量。第二种做法是编译时根本无法预知该对象和类可能属于哪些类, 程序只依靠运行时信息来发现该对象和类的真实信息, 这就必须使用反射。

具体来说, 通过反射机制, 我们可以实现如下的操作：

- 程序运行时, 可以通过反射获得任意一个类的 Class 对象, 并通过这个对象查看这个类的信息；

- 程序运行时, 可以通过反射创建任意一个类的实例, 并访问该实例的成员； - 程序运行时, 可以通过反射机制生成一个类的动态代理类或动态代理对象。

**加分回答 - 反射应用场景**

Java 的反射机制在实际项目中应用广泛, 常见的应用场景有：

- 使用 JDBC 时, 如果要创建数据库的连接, 则需要先通过反射机制加载数据库的驱动程序；

- 多数框架都支持注解 / XML 配置, 从配置中解析出来的类是字符串, 需要利用反射机制实例化；

- 面向切面编程（AOP）的实现方案, 是在程序运行时创建目标对象的代理类, 这必须由反射机制来实现。 

## 九、多线程

[【2023Java 面试八股文】Java 多线程篇_vincewm 的博客 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/129446871 "【2023Java面试八股文】Java多线程篇_vincewm的博客-CSDN博客")