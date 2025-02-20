---
url: https://blog.csdn.net/D_boj/article/details/132241235
title: 学习笔记：黑马程序员 Java - 高级篇（第三部分）_案例按关键字进行名称查询 x-CSDN 博客
date: 2025-02-20 09:33:59
tag: 
summary: 
---
**Java 语言入门到精通章节**

1.  [学习笔记：Java - 基础篇（第一部分）_ljtxy.love 的博客 - CSDN 博客](https://blog.csdn.net/D_boj/article/details/132192272)
2.  [学习笔记：Java - 中级篇（第二部分）_ljtxy.love 的博客 - CSDN 博客](https://blog.csdn.net/D_boj/article/details/132224349)
3.  [学习笔记：Java - 高级篇（第三部分）_ljtxy.love 的博客 - CSDN 博客](https://blog.csdn.net/D_boj/article/details/132241235)
4.  [学习笔记：Java - 进阶篇（一）（第四部分）_ljtxy.love 的博客 - CSDN 博客](https://blog.csdn.net/D_boj/article/details/132257791)
5.  [学习笔记：Java - 进阶篇（二）（第五部分）_ljtxy.love 的博客 - CSDN 博客](https://blog.csdn.net/D_boj/article/details/132257812)
6.  [学习笔记：Java8 新特性_ljtxy.love 的博客 - CSDN 博客](https://blog.csdn.net/D_boj/article/details/132257818)

#### 文章目录

*   [17. 集合类](#17_11)
*   *   [17.1 集合体系结构](#171_15)
    *   *   [17.1.1 概述](#1711_42)
        *   [17.1.2 单列集合](#1712_50)
        *   [17.1.3 双列集合](#1713_61)
    *   [17.2Collection 集合](#172Collection_72)
    *   *   [17.2.1 概述](#1721_139)
        *   [17.2.2 常用成员方法](#1722_147)
        *   *   [17.2.2.1add（）](#17221add_159)
            *   [17.2.2.2clear（）](#17222clear_172)
            *   [17.2.2.3remove（）](#17223remove_179)
            *   [17.2.2.4contains（）](#17224contains_191)
            *   [17.2.2.5isEmpty（）](#17225isEmpty_202)
            *   [17.2.2.6size（）](#17226size_210)
            *   [17.2.2.7 案例 -- 学生查询](#17227_219)
        *   [17.2.3 遍历方式](#1723_245)
        *   *   [17.2.3.1 迭代器](#17231_247)
            *   [17.2.3.2 增强 for 循环](#17232for_337)
            *   [17.2.3.3Lambda 表达式](#17233Lambda_380)
    *   [17.3List 集合](#173List_404)
    *   *   [17.3.1 概述](#1731_416)
        *   [17.3.2 常用成员方法](#1732_432)
        *   *   [17.3.2.1add（）](#17321add_441)
            *   [17.3.2.2remove（）](#17322remove_456)
            *   [17.3.2.3set（）](#17323set_464)
            *   [17.3.2.4get（）](#17324get_472)
            *   [17.3.2.5 案例 -- 基础操作](#17325_480)
        *   [17.3.3 遍历方式](#1733_528)
    *   [17.4ArrayList 集合](#174ArrayList_608)
    *   *   [17.4.1 概述](#1741_621)
        *   [17.4.2 常用成员方法](#1742_635)
        *   *   [17.4.2.1 构造方法](#17421_637)
            *   [17.4.2.2 成员方法](#17422_643)
        *   [17.4.3 底层原理](#1743_698)
    *   [17.5LinkedList 集合](#175LinkedList_728)
    *   *   [17.5.1 概述](#1751_736)
        *   [17.5.2 常用成员方法](#1752_740)
        *   [17.5.3 底层原理](#1753_805)
    *   [17.6Set 集合](#176Set_819)
    *   *   [17.6.1 概述](#1761_841)
        *   [17.6.2 常用成员方法](#1762_851)
        *   [17.6.3 遍历方式](#1763_867)
    *   [17.7HashSet 集合](#177HashSet_923)
    *   *   [17.7.1 概述](#1771_939)
        *   [17.7.2 底层原理](#1772_989)
        *   [17.7.3HashSet 的三个问题](#1773HashSet_1031)
    *   [17.8LinkedHashSet 集合](#178LinkedHashSet_1106)
    *   *   [17.8.1 概述](#1781_1114)
        *   [17.8.2 底层原理](#1782_1116)
    *   [17.9TreeSet 集合](#179TreeSet_1156)
    *   *   [17.9.1 概述](#1791_1165)
        *   [17.9.2 常用成员方法](#1792_1178)
        *   [17.9.3 两种排序比较方式](#1793_1217)
        *   *   [17.9.3.1 默认排序 / 自然排序](#17931_1219)
            *   [17.9.3.2 比较器排序](#17932_1342)
        *   [17.9.4 方法返回值特点](#1794_1385)
    *   [17.10 单列集合应用场景（重点）](#1710_1391)
    *   [17.11Map 集合](#1711Map_1395)
    *   *   [17.11.1 概述](#17111_1435)
        *   [17.11.2 常用成员方法](#17112_1445)
        *   *   [17.11.2.1put（）](#171121put_1457)
            *   [17.11.2.2clear（）](#171122clear_1469)
            *   [17.11.2.3remove（）](#171123remove_1476)
            *   [17.11.2.4containsKey（）](#171124containsKey_1486)
            *   [17.11.2.5containsValue（）](#171125containsValue_1494)
            *   [17.11.2.6isEmpty（）](#171126isEmpty_1502)
            *   [17.11.2.7size（）](#171127size_1510)
            *   [17.11.2.8 案例 -- 神话人物查询](#171128_1518)
        *   [17.11.3 遍历方式](#17113_1554)
        *   *   [17.11.3.1 键找值](#171131_1556)
            *   [17.11.3.2 键值对](#171132_1601)
            *   [17.11.3.3Lambda 表达式](#171133Lambda_1643)
    *   [17.12HashMap 集合](#1712HashMap_1692)
    *   *   [17.12.1 概述](#17121_1704)
        *   [17.12.2 底层原理](#17122_1748)
    *   [17.13LinkedHashMap 集合](#1713LinkedHashMap_1764)
    *   *   [17.13.1 概述](#17131_1772)
        *   [17.13.2 底层原理](#17132_1784)
    *   [17.14TreeMap 集合](#1714TreeMap_1788)
    *   *   [17.14.1 概述](#17141_1801)
        *   [17.14.2 底层原理](#17142_1854)
    *   [17.15 不可变集合](#1715_1858)
    *   *   [17.15.1 概述](#17151_1903)
        *   [17.15.2 使用场景](#17152_1907)
        *   [17.15.3 分类](#17153_1917)
        *   *   [17.15.3.1 不可变的 list 集合](#171531list_1927)
            *   [17.15.3.2** 不可变的 Set 集合 **](#171532Set_1974)
            *   [17.15.3.3** 不可变的 Map 集合 **](#171533Map_2009)
    *   [17.16Collections 集合工具类](#1716Collections_2098)
    *   *   [17.16.1 概述](#17161_2111)
        *   [17.16.2 常用成员方法](#17162_2117)
        *   [17.16.3 基本用例](#17163_2121)
*   [18. 泛型](#18_2243)
*   *   [18.1 概述](#181_2348)
    *   [18.2 分类](#182_2378)
    *   *   [18.2.1 泛型类](#1821_2380)
        *   [18.2.2 泛型方法](#1822_2404)
        *   [18.2.3 泛型接口](#1823_2428)
    *   [18.3 通配符](#183_2452)
*   [知识加油站](#_2542)
*   *   [1. 数据结构](#1_2544)
    *   *   [1.1 概述](#11_2555)
        *   [1.2 栈](#12_2561)
        *   [1.3 队列](#13_2575)
        *   [1.4 数组](#14_2585)
        *   [1.5 链表](#15_2595)
        *   [1.6 树](#16_2606)
        *   *   [1.6.1 二叉树](#161_2648)
            *   [1.6.2 二叉查找树](#162_2660)
            *   [1.6.3 平衡二叉树](#163_2670)
            *   [1.6.4 红黑树](#164_2725)
    *   [2. 可变参数](#2_2759)
    *   *   [2.1 定义](#21_2784)
        *   [2.2 特点](#22_2788)
        *   [2.3 作用](#23_2792)
    *   [3.Lambda 表达式](#3Lambda_2818)
    *   *   [3.1 概述](#31_2820)
        *   [3.2 作用](#32_2834)
        *   [3.3 语法](#33_2838)
        *   [3.4 函数式编程](#34_2846)
    *   [4. 函数式接口](#4_2867)
    *   [5. 迭代器源码分析](#5_2875)
    *   [6.Equals()` 方法和 `==` 运算符区别](#6Equals_2887)

## 17. 集合类

笔记小结：见各个小节

### 17.1 集合体系结构

笔记小结：

1.  概述：集合分为**单列**集合与**双列**集合
    
    ![](<assets/1740015239471.png>)
    
2.  特点：
    
    ![](<assets/1740015239796.png>)
    
3.  单列集合：
    
    *   List 系列集合：**有序**、**可重复**的集合
    *   Set 系列集合：**无序**、**不可重复**的集合
    *   详细：见各个小结
4.  双列集合：
    
    *   HashMap: 基于**哈希表**实现，可以存储 **null 键**和 **null 值**，不保证元素的顺序
    *   TreeMap: 基于**红黑树**实现，按照键的自然顺序或者自定义的比较器排序，不允许有 null 键，但可以有 null 值
5.  单列集合应用场景：
    
    ![](<assets/1740015240082.png>)
    

#### 17.1.1 概述

​ Java 中的集合类是用于存储、操作和管理一组对象的容器。其中集合可分类为单列集合、双列集合

![](<assets/1740015240677.png>)

​ 在 Java 中，**单列集**合指的是只能保存**一个元素序列**的集合，例如 List、Set 和 Queue 等。**双列集合**则指的是可以保存**键值对的集合**，例如 Map 等。单列集合和双列集合在保存和操作数据时的方式和目的有所不同

#### 17.1.2 单列集合

单列集合又分为：List 系列集合、Set 系列集合

![](<assets/1740015240972.png>)

补充：

*   List 系列集合：**有序**、**可重复**的集合。可以通过索引来访问其中的元素，可以存储重复的元素
*   Set 系列集合：**无序**、**不可重复**的集合。不能通过索引来访问其中的元素，不可以存储重复的元素

#### 17.1.3 双列集合

​ Java 中的双列集合是指可以存储[键值对](https://so.csdn.net/so/search?q=%E9%94%AE%E5%80%BC%E5%AF%B9&spm=1001.2101.3001.7020)数据结构的集合，也称为映射表或关联数组

![](<assets/1740015241301.png>)

补充：

*   双列集合, 一个键对应一个值
*   键不可以重复, 值可以重复

### 17.2Collection 集合

笔记小结：

1.  概述：Collection 是单列集合的祖宗接口，它的功能是全部**单列**集合**都可以继承**使用的
    
2.  常用成员方法;**add**、**clear**、**remove**、**contains**、**isEmpty**、**size**
    
3.  遍历方式
    
    *   迭代器：
        
        1.  概述：迭代器是**集合专用**的遍历方式，迭代器在遍历集合的时候是不依赖索引的
            
        2.  使用：迭代器需要掌握三个方法
            
            ```
            while (it.hasNext()) { 
                String s = it.next(); 
                System.out.println(s);
            }
            
            ```
            
        3.  细节：
            
            1.  当迭代器中**无元素**或元素遍历**完成**，再次调用 it.next() 方法，则报错 **NoSuchElementException**
        4.  迭代器遍历完毕，指针**不会复位**
            
            3.  循环中只能用**一次 next** 方法（因为 next 方法会做两件事，分别是获取元素和移动指针）
        5.  迭代器遍历时，不能用**集合的方法**进行增加或者删除，否则报错 **ConcurrentModificationException**
            
4.  原理：可参考知识加油站
    

*   增强 for 循环：
    
    1.  概述：其内部原理是一个 **Iterator 迭代器**
        
    2.  格式：
        
        ```
           for(集合/数组中元素的数据类型 变量名 :  集合/数组名) {
           // 已经将当前遍历到的元素封装到变量中了,直接使用变量即可
           }
           //例如
           for(string s : list){
               System.out.println(s);
           }
        
        ```
        
    3.  注意：** 修改增强 for 中的变量，不会改变集合中原本的数据，** 因为是通过第三方变量进行赋值
        
*   Lambda 表达式
    
    1.  概述：更简单、更直接的遍历集合的方式
        
    2.  格式：
        
        ```
           forEach(Consumer<? super T> action)
           //例如
           coll.forEach(s -> System.out.println(s));
        
        ```
        
    3.  底层原理：其实也会自己**遍历**集合，**依次得到每一个元素**。把得到的每一个元素，传递给下面的 **accept** 方法。s 依次表示集合中的每一个数据
        

#### 17.2.1 概述

Collection 是单列集合的祖宗接口，它的功能是全部单列集合都可以继承使用的

注意：

ollection 是一个接口, 我们不能直接创建他的对象。所以，现在我们学习他的方法时，只能创建他实现类的对象

#### 17.2.2 常用成员方法

<table><thead><tr><th align="left">方法名</th><th align="left">说明</th></tr></thead><tbody><tr><td align="left">boolean add(E e)</td><td align="left">添加元素</td></tr><tr><td align="left">boolean remove(Object o)</td><td align="left">从集合中移除指定的元素</td></tr><tr><td align="left">boolean removeIf(Object o)</td><td align="left">根据条件进行移除</td></tr><tr><td align="left">void clear()</td><td align="left">清空集合中的元素</td></tr><tr><td align="left">boolean contains(Object o)</td><td align="left">判断集合中是否存在指定的元素</td></tr><tr><td align="left">boolean isEmpty()</td><td align="left">判断集合是否为空</td></tr><tr><td align="left">int size()</td><td align="left">集合的长度，也就是集合中元素的个数</td></tr></tbody></table>

##### 17.2.2.1add（）

```
Collection<String> coll = new ArrayList<>();
coll.add("aaa");
coll.add("bbb");
coll.add("ccc");
System.out.println(coll);

```

*   如果我们要往 List 系列集合中添加数据，那么方法永远返回 true，因为 List 系列的是允许元素重复的。
*   如果我们要往 Set 系列集合中添加数据，如果当前要添加元素不存在，方法返回 true，表示添加成功。如果当前要添加的元素已经存在，方法返回 false，表示添加失败。 因为 Set 系列的集合不允许重复。

##### 17.2.2.2clear（）

```
Collection<String> coll = new ArrayList<>();
coll.clear();

```

##### 17.2.2.3remove（）

```
Collection<String> coll = new ArrayList<>();
System.out.println(coll.remove("aaa"));
System.out.println(coll);

```

*   因为 Collection 里面定义的是共性的方法，所以此时不能通过索引进行删除。只能通过元素的对象进行删除。
*   方法会有一个布尔类型的返回值，删除成功返回 true，删除失败返回 false
*   如果要删除的元素不存在，就会删除失败。

##### 17.2.2.4contains（）

```
Collection<String> coll = new ArrayList<>(); 
boolean result1 = coll.contains("bbb");
System.out.println(result1);

```

*   底层是依赖 equals 方法进行判断是否存在的
*   所以，如果集合中存储的是自定义对象，也想通过 contains 方法来判断是否包含，那么在 javabean 类中，一定要重写 equals 方法。

##### 17.2.2.5isEmpty（）

```
Collection<String> coll = new ArrayList<>(); 
boolean result2 = coll.isEmpty();
System.out.println(result2);//false

```

##### 17.2.2.6size（）

```
Collection<String> coll = new ArrayList<>(); 
coll.add("ddd");
int size = coll.size();
System.out.println(size);//3

```

##### 17.2.2.7 案例–学生查询

```
//1.创建集合的对象
Collection<Student> coll = new ArrayList<>();

//2.创建三个学生对象
Student s1 = new Student("zhangsan",23);
Student s2 = new Student("lisi",24);
Student s3 = new Student("wangwu",25);


//3.把学生对象添加到集合当中
coll.add(s1);
coll.add(s2);
coll.add(s3);

//4.判断集合中某一个学生对象是否包含
Student s4 = new Student("zhangsan",23);
//因为contains方法在底层依赖equals方法判断对象是否一致的。
//如果存的是自定义对象，没有重写equals方法，那么默认使用Object类中的equals方法进行判断，而Object类中equals方法，依赖地址值进行判断。
//需求：如果同姓名和同年龄，就认为是同一个学生。
//所以，需要在自定义的Javabean类中，重写equals方法就可以了。
System.out.println(coll.contains(s4));

```

#### 17.2.3 遍历方式

##### 17.2.3.1 迭代器

概述：迭代器在 Java 中的类是 Iterator，迭代器是集合专用的遍历方式

*   Collection 集合获取迭代器
    
    ![](<assets/1740015241533.png>)
    
*   Iterator 中的常用成员方法
    
    ![](<assets/1740015241827.png>)
    

代码示例：

```
public class IteratorDemo1 {
    public static void main(String[] args) {
        //创建集合对象
        Collection<String> c = new ArrayList<>();

        //添加元素
        c.add("hello");
        c.add("world");
        c.add("java");
        c.add("javaee");

        //Iterator<E> iterator()：返回此集合中元素的迭代器，通过集合的iterator()方法得到
        Iterator<String> it = c.iterator(); // 1.获取迭代器

        //用while循环改进元素的判断和获取
        while (it.hasNext()) { // 2.判断是否有元素
            String s = it.next(); // 3.获取元素 // 4.移动指针
            System.out.println(s);
        }
    }
}

```

说明：

迭代器运行流程：

1.  创建指针
2.  判断是否有元素
3.  获取元素
4.  移动指针

注意：

1，当迭代器中**无元素**或元素遍历**完成**，再次调用 it.next() 方法，则报错 **NoSuchElementException**

2，迭代器遍历完毕，指针**不会复位**

3，循环中只能用**一次 next** 方法（因为 next 方法会做两件事，分别是获取元素和移动指针）

4，迭代器遍历时，不能用**集合的方法**进行增加或者删除，否则报错 **ConcurrentModificationException**

**迭代器删除**

​ void remove(): 删除迭代器对象当前指向的元素

示例：

```
public class IteratorDemo2 {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("a");
        list.add("b");
        list.add("b");
        list.add("c");
        list.add("d");

        Iterator<String> it = list.iterator();
        while(it.hasNext()){
            String s = it.next();
            if("b".equals(s)){
                //指向谁,那么此时就删除谁.
                it.remove();
            }
        }
        System.out.println(list);
    }
}

```

注意：

迭代器遍历时，不能用**集合的方法**进行增加或者删除，否则报错 **ConcurrentModificationException**

##### 17.2.3.2 增强 for 循环

格式：

```
for(集合/数组中元素的数据类型 变量名 :  集合/数组名) {
// 已经将当前遍历到的元素封装到变量中了,直接使用变量即可
}
//例如
for(string s : list){
    System.out.println(s);
}

```

示例：

```
public class MyCollectonDemo1 {
    public static void main(String[] args) {
        ArrayList<String> list =  new ArrayList<>();
        list.add("a");
        list.add("b");
        list.add("c");
        list.add("d");
        list.add("e");
        list.add("f");

        //1,数据类型一定是集合或者数组中元素的类型
        //2,str仅仅是一个变量名而已,在循环的过程中,依次表示集合或者数组中的每一个元素
        //3,list就是要遍历的集合或者数组
        for(String str : list){
            System.out.println(str);
        }
    }
}

```

注意：

** 修改增强 for 中的变量，不会改变集合中原本的数据，** 因为是通过第三方变量进行赋值

![](<assets/1740015242166.png>)

##### 17.2.3.3Lambda 表达式

概述：得益于 JDK8 开始的新技术 Lambda 表达式，提供了一种更简单、更直接的遍历集合的方式。

常用成员方法：

![](<assets/1740015242454.png>)

示例：

```
Collection<String> coll = new ArrayList<>();
coll.add("zhangsan");
coll.add("lisi");
coll.add("wangwu");
coll.forEach(s -> System.out.println(s));

```

注意：

底层原理：其实也会自己遍历集合，依次得到每一个元素。把得到的每一个元素，传递给下面的 accept 方法。s 依次表示集合中的每一个数据

![](<assets/1740015242756.png>)

### 17.3List 集合

笔记小结：

1.  概述：List 是 Java 中常用的集合类型之一，用于存储**有序、可重复**的元素序列
    
2.  常用成员方法：**add**、**remove**、**set**、**get**
    
3.  遍历方式：参见 Collection 集合遍历方式
    
    ![](<assets/1740015243040.png>)
    

#### 17.3.1 概述

![](<assets/1740015243248.png>)

​ Collection 的方法 List 都继承了集合的方法列表都继承了，List 集合因为有索引，所以多了很多索引操作的方法。列出集合因为有索引，所以多了很多索引操作的方法。

*   有序集合, 这里的有序指的是**存取**顺序
*   用户可以精确**控制**列表中每个元素的插入位置, 用户可以通过整数索引访问元素, 并搜索列表中的元素
*   与 Set 集合不同, 列表通常**允许**重复的元素

**特点**

*   存取**有序**
*   可以**重复**
*   有**索引**

#### 17.3.2 常用成员方法

<table><thead><tr><th>方法名</th><th>描述</th></tr></thead><tbody><tr><td>E remove(int index)</td><td>删除指定<strong>索引</strong>处的元素，返回被删除的元素</td></tr><tr><td>void add(int index,E element)</td><td>在此集合中的<strong>指定位置</strong>插入指定的元素</td></tr><tr><td>E set(int index,E element)</td><td>修改指定索引处的元素，返回被修改的元素</td></tr><tr><td>E get(int index)</td><td>返回指定索引处的元素</td></tr></tbody></table>

##### 17.3.2.1add（）

```
List<String> list = new ArrayList<>();
list.add("aaa");
list.add("bbb");//1
list.add("ccc");
// 或者
list.add(1,"QQQ") // 在此集合中的指定位置插入指定的元素

```

注意：

原来索引上的元素会依次往后移

##### 17.3.2.2remove（）

```
List<String> list = new ArrayList<>();
String remove = list.remove(0); //删除指定索引处的元素，返回被删除的元素
System.out.println(remove);//aaa

```

##### 17.3.2.3set（）

```
List<String> list = new ArrayList<>();
String result = list.set(0, "QQQ"); //修改指定索引处的元素，返回被修改的元素
System.out.println(result);

```

##### 17.3.2.4get（）

```
List<String> list = new ArrayList<>();
String s = list.get(0); //返回指定索引处的元素
System.out.println(s);

```

##### 17.3.2.5 案例–基础操作

```
public class MyListDemo {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("aaa");
        list.add("bbb");
        list.add("ccc");
        //method1(list);
        //method2(list);
        //method3(list);
        //method4(list);
    }

    private static void method4(List<String> list) {
        //        E get(int index)		返回指定索引处的元素
        String s = list.get(0);
        System.out.println(s);
    }

    private static void method3(List<String> list) {
        //        E set(int index,E element)	修改指定索引处的元素，返回被修改的元素
        //被替换的那个元素,在集合中就不存在了.
        String result = list.set(0, "qqq");
        System.out.println(result);
        System.out.println(list);
    }

    private static void method2(List<String> list) {
        //        E remove(int index)		删除指定索引处的元素，返回被删除的元素
        //在List集合中有两个删除的方法
        //第一个 删除指定的元素,返回值表示当前元素是否删除成功
        //第二个 删除指定索引的元素,返回值表示实际删除的元素
        String s = list.remove(0);
        System.out.println(s);
        System.out.println(list);
    }

    private static void method1(List<String> list) {
        //        void add(int index,E element)	在此集合中的指定位置插入指定的元素
        //原来位置上的元素往后挪一个索引.
        list.add(0,"qqq");
        System.out.println(list);
    }
}

```

#### 17.3.3 遍历方式

说明：此遍历方式，参考 Collection 集合遍历方式

![](<assets/1740015243578.png>)

基本用例

```
package com.itheima.a02mylist;

import java.util.ArrayList;
import java.util.List;
import java.util.ListIterator;

public class A03_ListDemo3 {
    public static void main(String[] args) {
        /*
            List系列集合的五种遍历方式：
                1.迭代器
                2.列表迭代器
                3.增强for
                4.Lambda表达式
                5.普通for循环
         */


        //创建集合并添加元素
        List<String> list = new ArrayList<>();
        list.add("aaa");
        list.add("bbb");
        list.add("ccc");

        //1.迭代器
        Iterator<String> it = list.iterator();
        while(it.hasNext()){
            String str = it.next();
            System.out.println(str);
        }


        //2.增强for
        //下面的变量s，其实就是一个第三方的变量而已。
        //在循环的过程中，依次表示集合中的每一个元素
       for (String s : list) {
            System.out.println(s);
        }

        //3.Lambda表达式
        //forEach方法的底层其实就是一个循环遍历，依次得到集合中的每一个元素
        //并把每一个元素传递给下面的accept方法
        //accept方法的形参s，依次表示集合中的每一个元素
        list.forEach(s->System.out.println(s) );


        //4.普通for循环
        //size方法跟get方法还有循环结合的方式，利用索引获取到集合中的每一个元素
        for (int i = 0; i < list.size(); i++) {
            //i:依次表示集合中的每一个索引
            String s = list.get(i);
            System.out.println(s);
        }

        // 5.列表迭代器
        //获取一个列表迭代器的对象，里面的指针默认也是指向0索引的

        //额外添加了一个方法：在遍历的过程中，可以添加元素
        ListIterator<String> it = list.listIterator();
        while(it.hasNext()){
            String str = it.next();
            if("bbb".equals(str)){
                //qqq
                it.add("qqq");
            }
        }
        System.out.println(list);
    }
}

```

### 17.4ArrayList 集合

笔记小结：

1.  概述; ArrayList 是 Java 中的一个类，它实现了 List 接口，是一种基于**动态数组**实现的集合类
2.  常用成员方法：
    *   构造方法：**ArrayList()**
    *   成员方法：**add**、**remove**、**set**、**get**、**size**
3.  底层原理：
    *   利用**空参创建的集合**，在底层创建一个**默认长度为 0** 的数组
    *   添加第一个元素时，底层会创建一个新的**长度为 10** 的数组**存满时**，会**扩容 1.5 倍**
    *   如果**一次添加多个**元素，1.5 倍还放不下，则新创建数组的**长度以实际为准**

#### 17.4.1 概述

*   什么是集合
    
    ​ 提供一种存储空间可变的存储模型，存储的数据容量可以发生改变
    
*   ArrayList 集合的特点
    
    ​ 长度可以变化，只能存储引用数据类型。
    
*   泛型的使用
    
    ​ 用于约束集合中存储元素的数据类型
    

#### 17.4.2 常用成员方法

##### 17.4.2.1 构造方法

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>public ArrayList()</td><td>创建一个空的集合对象</td></tr></tbody></table>

##### 17.4.2.2 成员方法

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>public boolean add(要添加的元素)</td><td>将指定的元素追加到此集合的末尾</td></tr><tr><td>public boolean remove(要删除的元素)</td><td>删除指定元素, 返回值表示是否删除成功</td></tr><tr><td>public E remove(int index)</td><td>删除指定索引处的元素，返回被删除的元素</td></tr><tr><td>public E set(int index,E element)</td><td>修改指定索引处的元素，返回被修改的元素</td></tr><tr><td>public E get(int index)</td><td>返回指定索引处的元素</td></tr><tr><td>public int size()</td><td>返回集合中的元素的个数</td></tr></tbody></table>

示例：

```
public class ArrayListDemo02 {
    public static void main(String[] args) {
        //创建集合
        ArrayList<String> array = new ArrayList<String>();

        //添加元素
        array.add("hello");
        array.add("world");
        array.add("java");

        //public boolean remove(Object o)：删除指定的元素，返回删除是否成功
        //        System.out.println(array.remove("world"));
        //        System.out.println(array.remove("javaee"));

        //public E remove(int index)：删除指定索引处的元素，返回被删除的元素
        //        System.out.println(array.remove(1));

        //IndexOutOfBoundsException
        //        System.out.println(array.remove(3));

        //public E set(int index,E element)：修改指定索引处的元素，返回被修改的元素
        //        System.out.println(array.set(1,"javaee"));

        //IndexOutOfBoundsException
        //        System.out.println(array.set(3,"javaee"));

        //public E get(int index)：返回指定索引处的元素
        //        System.out.println(array.get(0));
        //        System.out.println(array.get(1));
        //        System.out.println(array.get(2));
        //System.out.println(array.get(3)); //？？？？？？ 自己测试

        //public int size()：返回集合中的元素的个数
        System.out.println(array.size());

        //输出集合
        System.out.println("array:" + array);
    }
}

```

#### 17.4.3 底层原理

![](<assets/1740015243864.png>)

说明：

*   当长度为 m(m<=10) 的数组，添加少于 n(n<10) 个元素时，底层实际数组长度为 10
    
*   当长度为 m(m<=10) 的数组，添加多余 n(n>10) 个元素时，底层实际长度为 n+10
    
*   当长度为 m(m>10) 的数组，添加少于 n(n<10) 个元素时，底层实际数组长度为 m*1.5
    
*   当长度为 m(m>10) 的数组，添加多余 n(n>10) 个元素时，底层实际数组长度为 m+n
    

源码分析：

```
transient Object[] elementData; // non-private to simplify nested class access

```

**ArrayList 底层是一个数组**

添加一个元素

![](<assets/1740015244044.png>)

添加十个元素

![](<assets/1740015244424.png>)

### 17.5LinkedList 集合

笔记小结：

1.  概述：LinkedList 是 Java 集合框架中的一个**双向链表**实现类，可以用来存储一组有序的元素。LinkedList 本身多了很多直接**操作首尾元素的特有 API**
2.  常用成员方法：**addFirst**、**addLast**、**getFirst**、**getLast**、**removeFirst**、**removeLast**
3.  底层原理：底层数据结构是**双链表**，**查询慢，增删快**，但是如果操作的是**首尾元素**，**速度**也是极**快**的。

#### 17.5.1 概述

​ LinkedList 是 Java 集合框架中的一个双向链表实现类，可以用来存储一组**有序的元素**。LinkedList 的元素是通过链表连接在一起的，每个元素都包含了一个指向前一个元素和后一个元素的引用。因此，**插入和删除**元素的**时间复杂度是 O(1**) 的，而随机**访问元素**的时 ** 间复杂度是 O(n)** 的，其中 n 是 LinkedList 中元素的个数。

#### 17.5.2 常用成员方法

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>public void <strong>addFirst</strong>(E e)</td><td>在该列表开头插入指定的元素</td></tr><tr><td>public void <strong>addLast</strong>(E e)</td><td>将指定的元素追加到此列表的末尾</td></tr><tr><td>public E <strong>getFirst</strong>()</td><td>返回此列表中的第一个元素</td></tr><tr><td>public E <strong>getLast</strong>()</td><td>返回此列表中的最后一个元素</td></tr><tr><td>public E <strong>removeFirst</strong>()</td><td>从此列表中删除并返回第一个元素</td></tr><tr><td>public E <strong>removeLast</strong>()</td><td>从此列表中删除并返回最后一个元素</td></tr></tbody></table>

示例：

```
public class MyLinkedListDemo4 {
    public static void main(String[] args) {
        LinkedList<String> list = new LinkedList<>();
        list.add("aaa");
        list.add("bbb");
        list.add("ccc");
//        public void addFirst(E e)	在该列表开头插入指定的元素
        //method1(list);

//        public void addLast(E e)	将指定的元素追加到此列表的末尾
        //method2(list);

//        public E getFirst()		返回此列表中的第一个元素
//        public E getLast()		返回此列表中的最后一个元素
        //method3(list);

//        public E removeFirst()		从此列表中删除并返回第一个元素
//        public E removeLast()		从此列表中删除并返回最后一个元素
        //method4(list);
      
    }

    private static void method4(LinkedList<String> list) {
        String first = list.removeFirst();
        System.out.println(first);

        String last = list.removeLast();
        System.out.println(last);

        System.out.println(list);
    }

    private static void method3(LinkedList<String> list) {
        String first = list.getFirst();
        String last = list.getLast();
        System.out.println(first);
        System.out.println(last);
    }

    private static void method2(LinkedList<String> list) {
        list.addLast("www");
        System.out.println(list);
    }

    private static void method1(LinkedList<String> list) {
        list.addFirst("qqq");
        System.out.println(list);
    }
}

```

#### 17.5.3 底层原理

![](<assets/1740015244932.png>)

说明：

当添加第 1 个元素时，会将头结点和尾结点指向第 1 个元素，并且第 1 元素的前索引和后索引都为空

![](<assets/1740015245111.png>)

说明：

当添加第 n 个元素时，会将尾结点指向第 n 个元素，并且第 n 个元素前索引为 n-1, 第 n 个元素后索引为空

### 17.6Set 集合

笔记小结：

1.  概述：它用于存储**不重复**的元素
    
2.  常用成员方法：
    
    <table><thead><tr><th align="left">方法名</th><th align="left">说明</th></tr></thead><tbody><tr><td align="left">boolean <strong>add</strong>(E e)</td><td align="left">添加元素</td></tr><tr><td align="left">boolean <strong>remove</strong>(Object o)</td><td align="left">从集合中移除指定的元素</td></tr><tr><td align="left">boolean <strong>removeIf</strong>(Object o)</td><td align="left">根据条件进行移除</td></tr><tr><td align="left">void <strong>clear</strong>()</td><td align="left">清空集合中的元素</td></tr><tr><td align="left">boolean <strong>contains</strong>(Object o)</td><td align="left">判断集合中是否存在指定的元素</td></tr><tr><td align="left">boolean <strong>isEmpty</strong>()</td><td align="left">判断集合是否为空</td></tr><tr><td align="left">int <strong>size</strong>()</td><td align="left">集合的长度，也就是集合中元素的个数</td></tr></tbody></table>
    
3.  遍历方式：
    
    ![](<assets/1740015245399.png>)
    

#### 17.6.1 概述

Set 集合是 Java 集合框架中的一种，它用于存储不重复的元素，具有无序性。

特点：

*   无序: 存取顺序不一致
*   不重复: 可以去除重复
*   无索引: 没有带索引的方法，所以不能使用普通 for 循环遍历，也不能通过索引来获取元素

#### 17.6.2 常用成员方法

<table><thead><tr><th align="left">方法名</th><th align="left">说明</th></tr></thead><tbody><tr><td align="left">boolean add(E e)</td><td align="left">添加元素</td></tr><tr><td align="left">boolean remove(Object o)</td><td align="left">从集合中移除指定的元素</td></tr><tr><td align="left">boolean removeIf(Object o)</td><td align="left">根据条件进行移除</td></tr><tr><td align="left">void clear()</td><td align="left">清空集合中的元素</td></tr><tr><td align="left">boolean contains(Object o)</td><td align="left">判断集合中是否存在指定的元素</td></tr><tr><td align="left">boolean isEmpty()</td><td align="left">判断集合是否为空</td></tr><tr><td align="left">int size()</td><td align="left">集合的长度，也就是集合中元素的个数</td></tr></tbody></table>

说明：

​ Set 集合的方法基本上雨 Collection 的 API 一致

#### 17.6.3 遍历方式

此遍历方式，参考 Collection 集合遍历方式

![](<assets/1740015245589.png>)

示例：

```
public class A01_SetDemo1 {
    public static void main(String[] args) {
       /*
           利用Set系列的集合，添加字符串，并使用多种方式遍历。
            迭代器
            增强for
            Lambda表达式

        */


        //1.创建一个Set集合的对象
        Set<String> s = new HashSet<>();

        //2,添加元素
        //如果当前元素是第一次添加，那么可以添加成功，返回true
        //如果当前元素是第二次添加，那么添加失败，返回false
        s.add("张三");
        s.add("张三");
        s.add("李四");
        s.add("王五");

        //3.打印集合
        //无序
        System.out.println(s);//[李四, 张三, 王五]

        //迭代器遍历
      Iterator<String> it = s.iterator();
        while (it.hasNext()){
            String str = it.next();
            System.out.println(str);
        }


        //增强for
       for (String str : s) {
            System.out.println(str);
        }

        // Lambda表达式
        s.forEach( str->System.out.println(str));


    }
}

```

### 17.7HashSet 集合

笔记小结：

1.  概述：它是**基于哈希表**实现的，可以存储**无序的**、**不重复的**元素
2.  底层原理：
    *   HashSet 集合底层采取**哈希表**存储数据
    *   哈希表是一种对于增删改查数据性能都较好的结构
3.  HashSet 的三个问题;
    1.  HashSet 为什么**存和取的顺序不一样**?
        *   答案：链表中添加的数据不是按照指定的顺序存储的，数据在**链表**中的**添加顺序不同**，因此存和取的顺序不一样。
    2.  HashSet 为什么**没有索引**?
        *   答案：HashSet 底层是由**数组、链表、红黑树**组成。此时，在 1 索引的位置下挂着一个**链表**，如此多的元素都为 1 索引，看起来不合适，因此就取消掉了索引的机制
    3.  HashSet 是利用什么机制保证**数据去重**的?
        *   答案：HashSet 是根据 **HashCode** 和 **equals** 方法，进行判断元素是否相同

#### 17.7.1 概述

哈希值

*   含义：对象的整数表现形式
    
    ![](<assets/1740015245825.png>)
    
*   概述：
    
    *   根据 **hashCode 方法算出来的 int 类型的整数**
    *   该方法定义在 Object 类中，所有对象都可以调用，**默认使用地址值**进行计算
    *   一般情况下，会**重写** hashCode 方法，利用对**象内部的属性值**计算哈希值

对象的哈希值特点：

*   如果没有重写 hashCode 方法，不同对象计算出的哈希值是不同的
    
    ![](<assets/1740015246130.png>)
    
*   如果已经重写 hashcode 方法，不同的对象只要属性值相同，计算出的哈希值就是一样的
    
    ![](<assets/1740015246462.png>)
    
*   在小部分情况下，不同的属性值或者不同的地址值计算出来的哈希值也有可能一样。(哈希碰撞)
    
    ![](<assets/1740015246735.png>)
    
    int 的取值范围用将近 42 亿中，如果此时创建 50 亿个对象，就有可能 8 亿 hash 值相同
    

示例：

```
        //1.创建对象
        Student s1 = new Student("zhangsan",23);
        Student s2 = new Student("zhangsan",23);

        //2.如果没有重写hashCode方法，不同对象计算出的哈希值是不同的
        //  如果已经重写hashcode方法，不同的对象只要属性值相同，计算出的哈希值就是一样的
        System.out.println(s1.hashCode());//-1461067292
        System.out.println(s2.hashCode());//-1461067292


        //在小部分情况下，不同的属性值或者不同的地址值计算出来的哈希值也有可能一样。
        //哈希碰撞
        System.out.println("abc".hashCode());//96354
        System.out.println("acD".hashCode());//96354

```

#### 17.7.2 底层原理

HashSet 底层原理

*   HashSet 集合底层采取**哈希表**存储数据
*   哈希表是一种对于**增删改查**数据性能都较好的结构

哈希表组成

*   JDK8 之前: 数组 + 链表
*   JDK8 开始: 数组 + 链表 + 红黑树

**JDK1.8 以前：**

![](<assets/1740015246979.png>)

**JDK1.8 以后：**

*   节点个数少于等于 8 个
    
    ​ 数组 + 链表
    
*   节点个数多于 8 个
    
    ​ 数组 + 红黑树
    

![](<assets/1740015247246.png>)

说明：

加载因子：又叫 HashSet 的扩容时机

当数组内存储了 16x0.75 = 12 个元素时，次数数组的长度会扩容到原来的两倍，也就是 32

当链表**长度大于 8** 而且数组**长度大于等于 64**，链表就会转换为红黑树

底层流程：

![](<assets/1740015247475.png>)

![](<assets/1740015247694.png>)

#### 17.7.3HashSet 的三个问题

![](<assets/1740015247990.png>)

说明：

​ 当遍历数组时，下标为 1 索引时，存储为链表。链表中添加的数据不是按照指定的顺序存储的，数据在链表中的添加顺序不同，因此存和取的顺序不一样。HashSet 的存储索引是 通过 hashCode 方法算出来的 int 类型的整数。

![](<assets/1740015248183.png>)

说明：

​ HashSet 底层是又数组、链表、红黑树组成。此时，在 1 索引的位置下挂着一个链表，如此多的元素都为 1 索引，看起来不合适，因此就取消掉了索引的机制

![](<assets/1740015248412.png>)

说明：

​ HashSet 是根据 HashCode 和 equals 方法，进行判断元素是否相同，若相同则不会添加到数组、链表或者红黑树当中。因此，在像 HashSet 中添加自定义对象是，一定要记得重写 HashCode 和 equals 方法，让底层的 Hash 值是根据对象的属性来生成

示例：

```
public class HashSetDemo02 {
    public static void main(String[] args) {
        //创建HashSet集合对象
        HashSet<Student> hs = new HashSet<Student>();

        //创建学生对象
        Student s1 = new Student("林青霞", 30);
        Student s2 = new Student("张曼玉", 35);
        Student s3 = new Student("王祖贤", 33);

        Student s4 = new Student("王祖贤", 33);

        //把学生添加到集合
        hs.add(s1);
        hs.add(s2);
        hs.add(s3);
        hs.add(s4);

        //遍历集合(增强for)
        for (Student s : hs) {
            System.out.println(s.getName() + "," + s.getAge());
        }
    }
}

```

注意：

​ HashSet 集合存储自定义类型元素, 要想实现元素的唯一, 要求必须重写 hashCode 方法和 equals 方法

```
@Override
 public boolean equals(Object o) {
     if (this == o) return true;
     if (o == null || getClass() != o.getClass()) return false;

     Student student = (Student) o;

     if (age != student.age) return false;
     return name != null ? name.equals(student.name) : student.name == null;
 }

 @Override
 public int hashCode() {
     int result = name != null ? name.hashCode() : 0;
     result = 31 * result + age;
     return result;
 }

```

Idea 中重写，可 Alt+Insert 进行快捷重写

### 17.8LinkedHashSet 集合

笔记小结：

1.  概述：`LinkedHashSet` 是 `HashSet` 的一个子类，它使用了一个**双向链表**来维护插入顺序，同时也保持了 `HashSet` 的元素唯一性
2.  特点：**有序**、不重复、无索引。
3.  底层原理：底层数据结构是依然哈希表，只是每个元素又额外的多了**一个双链表**的机制记录**存储**的**顺序**

#### 17.8.1 概述

#### 17.8.2 底层原理

*   **有序**、不重复、无索引。
*   这里的有序指的是保证存储和取出的元素顺序一致
*   **原理**∶底层数据结构是依然哈希表，只是每个元素又额外的多了一个双链表的机制记录存储的顺序。

![](<assets/1740015248625.png>)

示例：

```
public class A04_LinkedHashSetDemo {
    public static void main(String[] args) {
        //1.创建4个学生对象
        Student s1 = new Student("zhangsan",23);
        Student s2 = new Student("lisi",24);
        Student s3 = new Student("wangwu",25);
        Student s4 = new Student("zhangsan",23);


        //2.创建集合的对象
        LinkedHashSet<Student> lhs = new LinkedHashSet<>();


        //3.添加元素
        System.out.println(lhs.add(s3));
        System.out.println(lhs.add(s2));
        System.out.println(lhs.add(s1));
        System.out.println(lhs.add(s4));

        //4.打印集合
        System.out.println(lhs);
    }
}

```

说明：

​ LinkedHashSet 存入和取出的数据是有序的

### 17.9TreeSet 集合

笔记小节：

1.  概述：TreeSet 是 Java 集合框架中的一个集合类，它实现了 **SortedSet** 接口，可以对元素进行**排序**
2.  底层原理：TreeSet 集合底层是**基于红黑树的数据结构**实现排序的，**增删改查性能都较好**
3.  两种比较方式：默认排序 / 自然排序、比较器排序
4.  细节：比较方式使用原则: 默认使用第一种，如果第一种不能满足当前需求，就使用第二种，**自定义排序**

#### 17.9.1 概述

**特点：**

*   不重复、无索引、**可排序**
*   可排序: 按照元素的默认规则（有小到大）排序
*   TreeSet 集合底层是**基于红黑树的数据结构**实现排序的，增删改查性能都较好

**TreeSet 集合默认的规则：**

*   对于数值类型: Integer , Double，默认按照从小到大的顺序进行排序。
*   对于字符、字符串类型: 按照字符在 ASClI 码表中的数字升序进行排序。

#### 17.9.2 常用成员方法

类是于 Set

基础示例：

```
 //1.创建TreeSet集合对象
        TreeSet<Integer> ts = new TreeSet<>();

        //2.添加元素
        ts.add(4);
        ts.add(5);
        ts.add(1);
        ts.add(3);
        ts.add(2);

        //3.打印集合
        //System.out.println(ts);

        //4.遍历集合（三种遍历）
        //迭代器
        Iterator<Integer> it = ts.iterator();
        while(it.hasNext()){
            int i = it.next();
            System.out.println(i);
        }

        System.out.println("--------------------------");
        //增强for
        for (int t : ts) {
            System.out.println(t);
        }
        System.out.println("--------------------------");
        //lambda
        ts.forEach( i-> System.out.println(i));


```

#### 17.9.3 两种排序比较方式

##### 17.9.3.1 默认排序 / 自然排序

Javabean 类实现 Comparable 接口指定比较规则

```
public class Student implements Comparable<Student>{
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    /**
     * 获取
     * @return name
     */
    public String getName() {
        return name;
    }

    /**
     * 设置
     * @param name
     */
    public void setName(String name) {
        this.name = name;
    }

    /**
     * 获取
     * @return age
     */
    public int getAge() {
        return age;
    }

    /**
     * 设置
     * @param age
     */
    public void setAge(int age) {
        this.age = age;
    }

    public String toString() {
        return "Student{name = " + name + ", age = " + age + "}";
    }

    @Override
    //this：表示当前要添加的元素
    //o：表示已经在红黑树存在的元素

    //返回值：
    //负数：表示当前要添加的元素是小的，存左边
    //正数：表示当前要添加的元素是大的，存右边
    //0 :表示当前要添加的元素已经存在，舍弃
    public int compareTo(Student o) {
        System.out.println("--------------");
        System.out.println("this:" + this);
        System.out.println("o:" + o);
        //指定排序的规则
        //只看年龄，我想要按照年龄的升序进行排列
        return this.getAge() - o.getAge();
    }
}

```

说明：

若需要将此类用于 Treeset，需要实现 Comparable 接口，并在实现时指定泛型的类

```
public class A05_TreeSetDemo2 {
    public static void main(String[] args) {
        /*
            需求：创建TreeSet集合，并添加3个学生对象
            学生对象属性：
                姓名，年龄。
                要求按照学生的年龄进行排序
                同年龄按照姓名字母排列（暂不考虑中文）
                同姓名，同年龄认为是同一个人

            方式一：
                默认的排序规则/自然排序
                Student实现Comparable接口，重写里面的抽象方法，再指定比较规则
        */


        //1.创建三个学生对象
        Student s1 = new Student("zhangsan",23);
        Student s2 = new Student("lisi",24);
        Student s3 = new Student("wangwu",25);
        Student s4 = new Student("zhaoliu",26);


        //2.创建集合对象
        TreeSet<Student> ts = new TreeSet<>();

        //3.添加元素
        ts.add(s3);
        ts.add(s2);
        ts.add(s1);
        ts.add(s4);

        //4.打印集合
        System.out.println(ts);


        //TreeSet 底层是红黑树

    }
}

```

17.9.3 底层原理：

![](<assets/1740015248955.png>)

##### 17.9.3.2 比较器排序

创建 TreeSet 对象时候，传递比较器 Comparator 指定规则

```
public class A06_TreeSetDemo3 {
    public static void main(String[] args) {
       /*
            需求：请自行选择比较器排序和自然排序两种方式；
            要求：存入四个字符串， “c”, “ab”, “df”, “qwer”
            按照长度排序，如果一样长则按照首字母排序

            采取第二种排序方式：比较器排序
        */

        //1.创建集合
        //o1:表示当前要添加的元素
        //o2：表示已经在红黑树存在的元素
        //返回值规则跟之前是一样的
        TreeSet<String> ts = new TreeSet<>((o1, o2)->{
                // 按照长度排序
                int i = o1.length() - o2.length();
                //如果一样长则按照首字母排序
                i = i == 0 ? o1.compareTo(o2) : i;
                return i;
        });

        //2.添加元素
        ts.add("c");
        ts.add("ab");
        ts.add("df");
        ts.add("qwer");


        //3.打印集合
        System.out.println(ts);


    }
}


```

#### 17.9.4 方法返回值特点

*   负数: 表示当前要添加的元素是小的，存左边
*   正数: 表示当前要添加的元素是大的，存右边
*   0: 表示当前要添加的元素已经存在，舍弃

### 17.10 单列集合应用场景（重点）

![](<assets/1740015249342.png>)

### 17.11Map 集合

笔记小结：

1.  概述：Map 是一种用于存储**键值对**的集合，每个**键对应**一个唯一的**值**
    
2.  常用成员方法：**put**、**remove**、**clear**、**containsKey**、**containsValue**、**isEmpty**、**size**.
    
3.  遍历方式：
    
    *   键找值，API：`map.keySet()`
        
        ```
        Set<String> keys = map.keySet();
        for (String key : keys) {
            String value = map.get(key);
        }
        
        ```
        
    *   键值对，API：`map.entrySet()`
        
        ```
        Set<Map.Entry<String, String>> entries = map.entrySet();
        for (Map.Entry<String, String> entry : entries) {
            String key = entry.getKey();
            String value = entry.getValue();
        }
        
        ```
        
    *   Lambda 表达式，API：`map.forEach()`
        
        ```
        map.forEach(new BiConsumer<String, String>() {
            @Override
            public void accept(String key, String value) {
                System.out.println(key + "=" + value);
            }
        });
        
        ``` 

#### 17.11.1 概述

​ 在 Java 中，Map 是一种用于存储键值对的集合，每个键对应一个唯一的值。它是一种非常常用的集合，可用于快速查找和访问数据。

**格式：**

```
interface Map<K,V>  K：键的类型；V：值的类型

```

#### 17.11.2 常用成员方法

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>V put(K key,V value)</td><td>添加元素</td></tr><tr><td>V remove(Object key)</td><td>根据键删除键值对元素</td></tr><tr><td>void clear()</td><td>移除所有的键值对元素</td></tr><tr><td>boolean containsKey(Object key)</td><td>判断集合是否包含指定的键</td></tr><tr><td>boolean containsValue(Object value)</td><td>判断集合是否包含指定的值</td></tr><tr><td>boolean isEmpty()</td><td>判断集合是否为空</td></tr><tr><td>int size()</td><td>集合的长度，也就是集合中键值对的个数</td></tr></tbody></table>

##### 17.11.2.1put（）

```
Map<String,String> map = new HashMap<String,String>();
map.put("张无忌","赵敏");
map.put("郭靖","黄蓉");
map.put("杨过","小龙女");

```

*   在添加数据的时候，如果键**不存在**，那么直接把键值对对象添加到 map 集合当中, 方法返回 **null**
*   在添加数据的时候，如果键是**存在**的，那么会把原有的键值对对象覆盖，会把返回被**覆盖的值**

##### 17.11.2.2clear（）

```
Map<String,String> map = new HashMap<String,String>();
map.clear();

```

##### 17.11.2.3remove（）

```
Map<String,String> map = new HashMap<String,String>();    
String result = map.remove("郭靖"); 

```

*   在删除数据的时候，如果键**不存在**，那么直接把键值对对象添加到 map 集合当中, 方法返回 **null**
*   在删除数据的时候，如果键是**存在**的，那么会把原有的键值对对象覆盖，会把返回被**删除的值**

##### 17.11.2.4containsKey（）

```
Map<String,String> map = new HashMap<String,String>();    
boolean keyResult = m.containsKey("郭靖");
System.out.println(valueResult);

```

##### 17.11.2.5containsValue（）

```
Map<String,String> map = new HashMap<String,String>();    
boolean valueResult = map.containsValue("小龙女2");
System.out.println(valueResult);

```

##### 17.11.2.6isEmpty（）

```
Map<String,String> map = new HashMap<String,String>();    
boolean result = map.isEmpty();
System.out.println(result);

```

##### 17.11.2.7size（）

```
Map<String,String> map = new HashMap<String,String>();   
int size = map.size();
System.out.println(size);

```

##### 17.11.2.8 案例–神话人物查询

```
public class MapDemo02 {
    public static void main(String[] args) {
        //创建集合对象
        Map<String,String> map = new HashMap<String,String>();

        //V put(K key,V value)：添加元素
        map.put("张无忌","赵敏");
        map.put("郭靖","黄蓉");
        map.put("杨过","小龙女");

        //V remove(Object key)：根据键删除键值对元素
//        System.out.println(map.remove("郭靖"));
//        System.out.println(map.remove("郭襄"));

        //void clear()：移除所有的键值对元素
//        map.clear();

        //boolean containsKey(Object key)：判断集合是否包含指定的键
//        System.out.println(map.containsKey("郭靖"));
//        System.out.println(map.containsKey("郭襄"));

        //boolean isEmpty()：判断集合是否为空
//        System.out.println(map.isEmpty());

        //int size()：集合的长度，也就是集合中键值对的个数
        System.out.println(map.size());

        //输出集合对象
        System.out.println(map);
    }
}

```

#### 17.11.3 遍历方式

##### 17.11.3.1 键找值

概述：通过 Map 集合的键来获得值

方法介绍：

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>V get(Object key)</td><td>根据键获取值</td></tr><tr><td>Set <strong>keySet</strong>()</td><td>获取所有键的集合</td></tr><tr><td>Collection <strong>values</strong>()</td><td>获取所有值的集合</td></tr></tbody></table>

基本用例：

```
public class MapDemo01 {
    public static void main(String[] args) {
        //1.创建Map集合的对象
        Map<String,String> map = new HashMap<>();

        //2.添加元素
        map.put("尹志平","小龙女");
        map.put("郭靖","穆念慈");
        map.put("欧阳克","黄蓉");

        //3.通过键找值

        //3.1获取所有的键，把这些键放到一个单列集合当中
        Set<String> keys = map.keySet();
        //3.2遍历单列集合，得到每一个键
        for (String key : keys) {
            //System.out.println(key);
            //3.3 利用map集合中的键获取对应的值  get
            String value = map.get(key);
            System.out.println(key + " = " + value);
        }
        //4.直接找值
        Collection<String> values = map.values();
        for(String value : values) {
            System.out.println(value);
        }
    }
}

```

##### 17.11.3.2 键值对

概述：通过 Map 集合获取键值对

方法介绍：

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>Set&lt;Map.Entry&lt;K,V&gt;&gt; <strong>entrySet</strong>()</td><td>获取所有键值对对象的集合</td></tr></tbody></table>

基本用例：

```
public class MapDemo03 {
    public static void main(String[] args) {
        //Map集合的第二种遍历方式
        
        //1.创建Map集合的对象
        Map<String, String> map = new HashMap<>();

        //2.添加元素
        //键：人物的外号
        //值：人物的名字
        map.put("标枪选手", "马超");
        map.put("人物挂件", "明世隐");
        map.put("御龙骑士", "尹志平");

        //3.Map集合的第二种遍历方式
        //通过键值对对象进行遍历
        //3.1 通过一个方法获取所有的键值对对象，返回一个Set集合
        Set<Map.Entry<String, String>> entries = map.entrySet();
        //3.2 遍历entries这个集合，去得到里面的每一个键值对对象
        for (Map.Entry<String, String> entry : entries) {//entry  --->  "御龙骑士","尹志平"
            //3.3 利用entry调用get方法获取键和值
            String key = entry.getKey();
            String value = entry.getValue();
            System.out.println(key + "=" + value);
        }
    }
}

```

##### 17.11.3.3Lambda 表达式

概述：得益于 JDK8 开始的新技术 Lambda 表达式，提供了一种更简单、更直接的遍历集合的方式。

基本用例：

```
public class MapDemo03 {
    public static void main(String[] args) {
       //Map集合的第三种遍历方式


        //1.创建Map集合的对象
        Map<String,String> map = new HashMap<>();

        //2.添加元素
        //键：人物的名字
        //值：名人名言
        map.put("鲁迅","这句话是我说的");
        map.put("曹操","不可能绝对不可能");
        map.put("刘备","接着奏乐接着舞");
        map.put("柯镇恶","看我眼色行事");

        //3.利用lambda表达式进行遍历
        //底层：
        //forEach其实就是利用第二种方式进行遍历，依次得到每一个键和值
        //再调用accept方法
        map.forEach(new BiConsumer<String, String>() {
            @Override
            public void accept(String key, String value) {
                System.out.println(key + "=" + value);
            }
        });

        System.out.println("-----------------------------------");

        map.forEach((String key, String value)->{
                System.out.println(key + "=" + value);
            }
        );

        System.out.println("-----------------------------------");

        map.forEach((key, value)-> System.out.println(key + "=" + value));

    }
}

```

### 17.12HashMap 集合

笔记小结：

1.  概述：HashMap 是 Java 中的一个集合类，它实现了 Map 接口，可以用来**存储键值对**
2.  特点：**无序**的、 允许使用 **null 作为键**或值、键是唯一的，插入**键相同则覆盖**、**线程不安全**的、HashMap 的实现是基于哈希表的，使用哈希算法来确定键值对的存储位置，因此**插入和查询**的**时间复杂度为 O(1)**。
3.  底层原理：
    *   HashMap 底层是**哈希表结构**的
    *   依赖 hashCode 方法和 equals 方法保证**键的唯一**
    *   如果**键**要存储的是**自定义对象**，**需要重写 hashCode 和 equals 方法**（此处注意！！！！）。如果**值**要存储的是自定义对象，不需要重写 hashCode 和 equals 方法 0
    *   待完善，详细参见视频！！！

#### 17.12.1 概述

![](<assets/1740015249539.png>)

HashMap 是 Java 中的一个集合类，它实现了 Map 接口，可以用来存储键值对。

特点：

*   HashMap 中的键值对是无序的。
*   HashMap 允许使用 null 作为键或值。
*   HashMap 的键是唯一的，如果插入了两个相同的键，则后插入的值会覆盖先插入的值。
*   HashMap 是线程不安全的，如果多个线程同时对 HashMap 进行操作，可能会导致数据不一致。
*   HashMap 的实现是基于哈希表的，使用哈希算法来确定键值对的存储位置，因此插入和查询的时间复杂度为 O(1)。

基本用例：

```
public class HashMapDemo {
    public static void main(String[] args) {
        //创建HashMap集合对象
        HashMap<Student, String> hm = new HashMap<Student, String>();

        //创建学生对象
        Student s1 = new Student("林青霞", 30);
        Student s2 = new Student("张曼玉", 35);
        Student s3 = new Student("王祖贤", 33);
        Student s4 = new Student("王祖贤 ", 33);

        //把学生添加到集合
        hm.put(s1, "西安");
        hm.put(s2, "武汉");
        hm.put(s3, "郑州");
        hm.put(s4, "北京");

        //遍历集合
        Set<Student> keySet = hm.keySet();
        for (Student key : keySet) {
            String value = hm.get(key);
            System.out.println(key.getName() + "," + key.getAge() + "," + value);
        }
    }
}

```

#### 17.12.2 底层原理

![](<assets/1740015249835.png>)

说明：

此图为 HashMap 底层源码的改写版本，通俗易懂

![](<assets/1740015250045.png>)

*   HashMap 底层是哈希表结构的
*   依赖 hashCode 方法和 equals 方法保证**键的唯一**
*   如果**键**要存储的是自定义对象，需要重写 hashCode 和 equals 方法。如果**值**要存储的是自定义对象，不需要重写 hashCode 和 equals 方法 0

参考资料：[集合进阶 - 13-HashMap 源码超详细解析（一）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1yW4y1Y7Ms/?p=14&spm_id_from=pageDriver&vd_source=dddaa7c77aa38a5c24657d40a4f357c9)

### 17.13LinkedHashMap 集合

笔记小结：

1.  概述：LinkedHashMap 是 HashMap 的一个子类
2.  特点：**不重复**、**无索引**、**有序性**
3.  底层原理：底层数据结构是依然哈希表，只是每个键值对元素又额外的多了一个**双链表**的机制**记录存储的顺序**。跟 HashSet 一样

#### 17.13.1 概述

![](<assets/1740015250349.png>)

LinkedHashMap 是 HashMap 的一个子类

**特点：**

*   不重复
*   无索引
*   有序性：LinkedHashMap 中的元素按照插入顺序排序，也可以按照访问顺序排序（通过构造函数传入 accessOrder 参数为 true 实现）。

#### 17.13.2 底层原理

底层数据结构是依然哈希表，只是每个键值对元素又额外的多了一个双链表的机制记录存储的顺序。跟 HashSet 一样

### 17.14TreeMap 集合

笔记小结：

1.  概述：TreeMap 是 Java 中的一种双列集合，它基于**红黑树**实现。TreeMap 通过 key-value 存储数据，支持根据 key 值进行排序和查找，可以**保证元素有序**，因此在需要对集合中的元素进行排序时，可以使用 TreeMap。
    
2.  特点：
    
    *   TreeMap 底层是**红黑树结构**
    *   依赖**自然排序**或者**比较器排序**, 对键进行排序
    *   如果**键**存储的是**自定义对象**, 需要**实现 Comparable 接口**或者在创建 TreeMap 对象时候给出比较器排序规则
    *   **注意**: 默认按照键的从小到大进行排序，也可以自己规定键的排序规则
3.  底层原理：待完善，详细参见视频！！！
    

#### 17.14.1 概述

特点：

*   TreeMap 底层是红黑树结构
*   依赖自然排序或者比较器排序, 对键进行排序
*   如果键存储的是自定义对象, 需要实现 Comparable 接口或者在创建 TreeMap 对象时候给出比较器排序规则
*   **注意**: 默认按照键的从小到大进行排序，也可以自己规定键的排序规则

基本用例：

```
public class Test1 {
    public static void main(String[] args) {
      	// 创建TreeMap集合对象
        TreeMap<Student,String> tm = new TreeMap<>();
      
		// 创建学生对象
        Student s1 = new Student("xiaohei",23);
        Student s2 = new Student("dapang",22);
        Student s3 = new Student("xiaomei",22);
      
		// 将学生对象添加到TreeMap集合中
        tm.put(s1,"江苏");
        tm.put(s2,"北京");
        tm.put(s3,"天津");
      
		// 遍历TreeMap集合,打印每个学生的信息
        tm.forEach(
                (Student key, String value)->{
                    System.out.println(key + "---" + value);
                }
        );
    }
}

```

也需要在 student 类中进行接口实现

```
public class Student implements Comparable<Student>{
	……………………
    @Override
    public int compareTo(Student o) {
        //按照年龄进行排序
        int result = o.getAge() - this.getAge();
        //次要条件，按照姓名排序。
        result = result == 0 ? o.getName().compareTo(this.getName()) : result;
        return result;
    }
}

```

#### 17.14.2 底层原理

参考资料：[集合进阶 - 17-TreeMap 源码超详细解析（一）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1yW4y1Y7Ms/?p=18&spm_id_from=pageDriver&vd_source=dddaa7c77aa38a5c24657d40a4f357c9)

### 17.15 不可变集合

笔记小结：

1.  概述：是一个**长度不可变**，**内容**也**无法修改**的集合
    
2.  特点：定义完成后
    
    *   **不可修改**
    *   **不可添加**
    *   **不可删除**
3.  常用成员方法：List、Set、Map 接口中，都存在 **of 方法**可以创建不可变集合
    
4.  三种方式的细节:
    
    1.  List: 直接用
        
        ```
         List<String> list = List.of("张三", "李四", "王五", "赵六");
        
        ```
        
    2.  Set: 元素不能重复
        
        ```
         Set<String> set = Set.of("张三", "张三", "李四", "王五", "赵六");
        
        ```
        
    3.  Map: 元素不能重复、键值对数量最多是 10 个。
        
        *   少于 10 个用 of 方法
            
            ```
                Map<String, String> map = Map.of("张三", "南京", "张三",……);
            
            ```
            
        *   超过 10 个用 ofEntries 方法
            
            ```
               HashMap<String, String> hm = new HashMap<>();
            hm.put("张三", "南京");
            hm.put("李四", "北京");
            Map<String, String> map = Map.copyOf(hm); // 此方法为JDK10之后才拥有的
            
            ``` 

#### 17.15.1 概述

是一个长度不可变，内容也无法修改的集合

#### 17.15.2 使用场景

​ 如果某个数据不能被修改，把它防御性地**拷贝**到**不可变集合中**是个很好的实践。

​ 当集合对象被不可信的库调用时，**不可变形式是安全**的。

简单理解：

​ 不想让别人修改集合中的内容

#### 17.15.3 分类

*   不可变的 list 集合
*   不可变的 set 集合
*   不可变的 map 集合

常用成员方法

![](<assets/1740015250600.png>)

##### 17.15.3.1 不可变的 list 集合

```
public class ImmutableDemo1 {
    public static void main(String[] args) {
        /*
            创建不可变的List集合
            "张三", "李四", "王五", "赵六"
        */

        //一旦创建完毕之后，是无法进行修改的，在下面的代码中，只能进行查询操作
        List<String> list = List.of("张三", "李四", "王五", "赵六");

        System.out.println(list.get(0));
        System.out.println(list.get(1));
        System.out.println(list.get(2));
        System.out.println(list.get(3));

        System.out.println("---------------------------");

        for (String s : list) {
            System.out.println(s);
        }

        System.out.println("---------------------------");


        Iterator<String> it = list.iterator();
        while(it.hasNext()){
            String s = it.next();
            System.out.println(s);
        }
        System.out.println("---------------------------");

        for (int i = 0; i < list.size(); i++) {
            String s = list.get(i);
            System.out.println(s);
        }
        System.out.println("---------------------------");

        //list.remove("李四");
        //list.add("aaa");
        list.set(0,"aaa");
    }
}

```

##### 17.15.3.2 **不可变的 Set 集合**

```
public class ImmutableDemo2 {
    public static void main(String[] args) {
        /*
           创建不可变的Set集合
           "张三", "李四", "王五", "赵六"


           细节：
                当我们要获取一个不可变的Set集合时，里面的参数一定要保证唯一性
        */

        //一旦创建完毕之后，是无法进行修改的，在下面的代码中，只能进行查询操作
        Set<String> set = Set.of("张三", "张三", "李四", "王五", "赵六");

        for (String s : set) {
            System.out.println(s);
        }

        System.out.println("-----------------------");

        Iterator<String> it = set.iterator();
        while(it.hasNext()){
            String s = it.next();
            System.out.println(s);
        }

        System.out.println("-----------------------");
        //set.remove("王五");
    }
}

```

##### 17.15.3.3 **不可变的 Map 集合**

*   键值对个数小于等于 10

```
public class ImmutableDemo3 {
    public static void main(String[] args) {
       /*
        创建Map的不可变集合
            细节1：
                键是不能重复的
            细节2：
                Map里面的of方法，参数是有上限的，最多只能传递20个参数，10个键值对
            细节3：
                如果我们要传递多个键值对对象，数量大于10个，在Map接口中还有一个方法
        */

        //一旦创建完毕之后，是无法进行修改的，在下面的代码中，只能进行查询操作
        Map<String, String> map = Map.of("张三", "南京", "张三", "北京", "王五", "上海",
                "赵六", "广州", "孙七", "深圳", "周八", "杭州",
                "吴九", "宁波", "郑十", "苏州", "刘一", "无锡",
                "陈二", "嘉兴");

        Set<String> keys = map.keySet();
        for (String key : keys) {
            String value = map.get(key);
            System.out.println(key + "=" + value);
        }

        System.out.println("--------------------------");

        Set<Map.Entry<String, String>> entries = map.entrySet();
        for (Map.Entry<String, String> entry : entries) {
            String key = entry.getKey();
            String value = entry.getValue();
            System.out.println(key + "=" + value);
        }
        System.out.println("--------------------------");
    }
}

```

*   键值对个数大于 10

```
public class ImmutableDemo4 {
    public static void main(String[] args) {

        /*
            创建Map的不可变集合,键值对的数量超过10个
        */

        //1.创建一个普通的Map集合
        HashMap<String, String> hm = new HashMap<>();
        hm.put("张三", "南京");
        hm.put("李四", "北京");
        hm.put("王五", "上海");
        hm.put("赵六", "北京");
        hm.put("孙七", "深圳");
        hm.put("周八", "杭州");
        hm.put("吴九", "宁波");
        hm.put("郑十", "苏州");
        hm.put("刘一", "无锡");
        hm.put("陈二", "嘉兴");
        hm.put("aaa", "111");

        //2.利用上面的数据来获取一个不可变的集合
/*
        //获取到所有的键值对对象（Entry对象）
        Set<Map.Entry<String, String>> entries = hm.entrySet();
        //把entries变成一个数组
        Map.Entry[] arr1 = new Map.Entry[0];
        //toArray方法在底层会比较集合的长度跟数组的长度两者的大小
        //如果集合的长度 > 数组的长度 ：数据在数组中放不下，此时会根据实际数据的个数，重新创建数组
        //如果集合的长度 <= 数组的长度：数据在数组中放的下，此时不会创建新的数组，而是直接用
        Map.Entry[] arr2 = entries.toArray(arr1);
        //不可变的map集合
        Map map = Map.ofEntries(arr2);
        map.put("bbb","222");*/


        //Map<Object, Object> map = Map.ofEntries(hm.entrySet().toArray(new Map.Entry[0]));

        Map<String, String> map = Map.copyOf(hm);
        map.put("bbb","222");
    }
}

```

### 17.16Collections 集合工具类

笔记小结：

1.  概述：java.util.Collections: 是**集合工具类**
2.  常用成员方法：
    1.  新增：addAll
    2.  顺序：打乱 shuffle（）、排序 sort（）、指定排序 sort（）、交换 swap（）
    3.  拷贝：copy（）
    4.  填充：fill（）
    5.  最大最小：max/min（）
    6.  查询：binarySearch（）

#### 17.16.1 概述

java.util.Collections: 是集合工具类

作用：Collections 不是集合，而是集合的工具类

#### 17.16.2 常用成员方法

![](<assets/1740015250840.png>)

#### 17.16.3 基本用例

案例 1

```
public class CollectionsDemo1 {
    public static void main(String[] args) {

        //addAll  批量添加元素
        //1.创建集合对象
        ArrayList<String> list = new ArrayList<>();
        //2.批量添加元素
        Collections.addAll(list,"abc","bcd","qwer","df","asdf","zxcv","1234","qwer");
        //3.打印集合
        System.out.println(list);

        //shuffle 打乱
        Collections.shuffle(list);

        System.out.println(list);

    }
}

```

案例 2

```
public class CollectionsDemo2 {
    public static void main(String[] args) {
      /*
        public static <T> void sort(List<T> list)                       排序
        public static <T> void sort(List<T> list, Comparator<T> c)      根据指定的规则进行排序
        public static <T> int binarySearch (List<T> list,  T key)       以二分查找法查找元素
        public static <T> void copy(List<T> dest, List<T> src)          拷贝集合中的元素
        public static <T> int fill (List<T> list,  T obj)               使用指定的元素填充集合
        public static <T> void max/min(Collection<T> coll)              根据默认的自然排序获取最大/小值
        public static <T> void swap(List<?> list, int i, int j)         交换集合中指定位置的元素
     */


        System.out.println("-------------sort默认规则--------------------------");
        //默认规则，需要重写Comparable接口compareTo方法。Integer已经实现，按照从小打大的顺序排列
        //如果是自定义对象，需要自己指定规则
        ArrayList<Integer> list1 = new ArrayList<>();
        Collections.addAll(list1, 10, 1, 2, 4, 8, 5, 9, 6, 7, 3);
        Collections.sort(list1);
        System.out.println(list1);


        System.out.println("-------------sort自己指定规则规则--------------------------");
        Collections.sort(list1, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });
        System.out.println(list1);

        Collections.sort(list1, (o1, o2) -> o2 - o1);
        System.out.println(list1);

        System.out.println("-------------binarySearch--------------------------");
        //需要元素有序
        ArrayList<Integer> list2 = new ArrayList<>();
        Collections.addAll(list2, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        System.out.println(Collections.binarySearch(list2, 9));
        System.out.println(Collections.binarySearch(list2, 1));
        System.out.println(Collections.binarySearch(list2, 20));

        System.out.println("-------------copy--------------------------");
        //把list3中的元素拷贝到list4中
        //会覆盖原来的元素
        //注意点：如果list3的长度 > list4的长度，方法会报错
        ArrayList<Integer> list3 = new ArrayList<>();
        ArrayList<Integer> list4 = new ArrayList<>();
        Collections.addAll(list3, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        Collections.addAll(list4, 0,0,0,0,0,0,0,0,0,0,0,0,0,0,0);
        Collections.copy(list4, list3);
        System.out.println(list3);
        System.out.println(list4);

        System.out.println("-------------fill--------------------------");
        //把集合中现有的所有数据，都修改为指定数据
        ArrayList<Integer> list5 = new ArrayList<>();
        Collections.addAll(list5, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        Collections.fill(list5, 100);
        System.out.println(list5);

        System.out.println("-------------max/min--------------------------");
        //求最大值或者最小值
        ArrayList<Integer> list6 = new ArrayList<>();
        Collections.addAll(list6, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        System.out.println(Collections.max(list6));
        System.out.println(Collections.min(list6));

        System.out.println("-------------max/min指定规则--------------------------");
        // String中默认是按照字母的abcdefg顺序进行排列的
        // 现在我要求最长的字符串
        // 默认的规则无法满足，可以自己指定规则
        // 求指定规则的最大值或者最小值
        ArrayList<String> list7 = new ArrayList<>();
        Collections.addAll(list7, "a","aa","aaa","aaaa");
        System.out.println(Collections.max(list7, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o1.length() - o2.length();
            }
        }));

        System.out.println("-------------swap--------------------------");
        ArrayList<Integer> list8 = new ArrayList<>();
        Collections.addAll(list8, 1, 2, 3);
        Collections.swap(list8,0,2);
        System.out.println(list8);



    }
}

```

## 18. 泛型

笔记小结：

1.  概述：泛型是 Java 中的一种特性，它可以将类或方法中的数据类型作为参数进行传递和使用
    
2.  格式
    
    ```
    <E> <T>
    
    ```
    
3.  好处：
    
    *   把运行时期的问题提前到了编译期间
    *   避免了强制类型转换
4.  注意：在 java 中在编译为 class 文件时，是会擦除掉泛型的
    
5.  细节：
    
    *   泛型中不能写基本数据类型
    *   指定泛型的具体类型后，传递数据时，可以传入该类类型或者其子类类型
    *   如果不写泛型，类型默认是 Object
6.  分类：
    
    1.  泛型类
        
        *   概念：在类名后面定义泛型，创建该类对象的时候，确定类型
            
        *   格式：
            
            ```
            修饰符 class 类名<类型>{
            }
            // 例如
            public class Arraylist<T>{
            }
            
            ```
            
    2.  泛型方法
        
        *   概念：在修饰符后面定义方法，调用该方法的时候，确定类型
            
        *   格式：
            
            ```
            修饰符<类型> 返回值类型方法名(类型变量名){
            }
            // 例如
            public<T> void show (T t){
            }
            
            ```
            
    3.  泛型接口
        
        *   概念：在接口名后面定义泛型，实现类确定类型，实现类延续泛型
            
        *   格式：
            
            ```
            修饰符 interface 接口名<类型>{
            }
            // 例如
            public interface List<E>{
            }
            
            ```
            
7.  泛型通配符
    
    *   泛型不具备继承性，但是数据具备继承性
        
        ```
        public static void method( ArrayList<Animal> list) {
        }
        // 此处method方法所传入的任何参数，只能是Animal类的集合，其余任何类的集合都不行，包括子类继承的类集合也不行
        
        ```
        
        ```
        ArrayList<Ye> list1 = new ArrayList<>();
        list1.add(new Ye());
        list1.add(new Fu());
        list1.add(new zi());
        // 与泛型类形成对比，数据具备继承性
        
        ```
        
    *   泛型的通配符:?
        
        ```
        // 格式
        ? extend E // 传递子类
        ? super E //传递父类
        // 例如
        public static void keepPet(ArrayList<? extends Animal> list){
        }
        public static void keepPet(ArrayList<? super Animal> list){
        }
        
        ```
        
8.  应用场景：
    
    *   定义类、方法、接口的时候，如果类型不确定，就可以定义泛型
    *   如果类型不确定，但是能知道是哪个继承体系中的，可以使用泛型的通配符

### 18.1 概述

​ Java 泛型是 Java SE 5 中引入的一种编程机制，它通过参数化类型来实现代码的重用。使用泛型可以让代码更加通用和类型安全，避免了在编译时期因类型转换错误导致的运行时异常。

**格式：**

*   <类型>: 指定一种类型的格式. 尖括号里面可以任意书写, 一般只写一个字母. 例如:
*   <类型 1, 类型 2…>: 指定多种类型的格式, 多种类型之间用逗号隔开. 例如: <E,T> <K,V>

**好处:**

*   把运行时期的问题提前到了编译期间
*   避免了强制类型转换

![](<assets/1740015251100.png>)

说明：

在 java 中在编译为 class 文件时，是会擦除掉泛型的

**细节：**

*   泛型中不能写基本数据类型
*   指定泛型的具体类型后，传递数据时，可以传入该类类型或者其子类类型
*   如果不写泛型，类型默认是 Object

说明：

因为泛型的基本数据类型是 Object 类型，基本数据类型无法自动转换为 Object 类型

### 18.2 分类

#### 18.2.1 泛型类

概念：泛型类（Generic Class）指在类的定义中使用泛型类型参数的类，可以在类中使用泛型类型参数，从而增强代码的可重用性和类型安全性。

格式：

```
修饰符 class 类名<类型>{
    
}
// 例如
public class Arraylist<T>{
    
}

```

说明：

此处 E 可以理解为变量，但是不是用来记录数据的，而是记录数据的类型，可以写成 T、E、K、V 等

示例：

![](<assets/1740015251491.png>)

#### 18.2.2 泛型方法

概述：泛型方法（Generic Method）指在方法定义中使用泛型类型参数的方法，可以在方法中使用泛型类型参数，从而增强代码的可重用性和类型安全性。

格式

```
修饰符<类型> 返回值类型方法名(类型变量名){
}
// 例如
public<T> void show (T t){
}

```

说明:

此处 T 可以理解为变量，但是不是用来记录数据的，而是记录类型的，可以写成:.E、K、V 等

![](<assets/1740015251712.png>)

补充：

当使用 E … 作为参数时， … 表示可变参数，此时传入的参数个数也是不确定的

#### 18.2.3 泛型接口

概念：泛型接口（Generic Interface）指在接口的定义中使用泛型类型参数的接口，可以在接口中使用泛型类型参数，从而增强代码的可重用性和类型安全性。

格式：

```
修饰符 interface 接口名<类型>{
}
// 例如
public interface List<E>{
}

```

使用：

*   实现类给出具体类型

![](<assets/1740015252008.png>)

*   实现类延续泛型，创建对象时再确定

![](<assets/1740015252309.png>)

### 18.3 通配符

​ 概念：泛型的通配符指的是 Java 中的通配符类型（Wildcard Type），使用 `?` 表示。通配符类型是一种类型实参，可以用于表示某个泛型类型的类型参数可以是任何类型。通配符类型可以用于方法参数类型、变量类型、返回值类型等。

通配符：? 也表示不确定的类型

分类：

*   ? extends E: 表示可以传递 E 或者 E 所有的子类类型
*   ? super E: 表示可以传递 E 或者 E 所有的父类类型

特点：

*   泛型的继承和通配符
*   泛型不具备继承性，但是数据具备继承性

应用场景：

*   如果我们在定义类、方法、接口的时候，如果类型不确定，就可以定义泛型类、泛型方法、泛型接口。
*   如果类型不确定，但是能知道以后只能传递某个继承体系中的，就可以泛型的通配符

意义：限定类型的范围

代码：

```
public class Test1 {
    public static void main(String[] args) {
        /*
            需求：
                定义一个继承结构：
                                    动物
                         |                           |
                         猫                          狗
                      |      |                    |      |
                   波斯猫   狸花猫                泰迪   哈士奇


                 属性：名字，年龄
                 行为：吃东西
                       波斯猫方法体打印：一只叫做XXX的，X岁的波斯猫，正在吃小饼干
                       狸花猫方法体打印：一只叫做XXX的，X岁的狸花猫，正在吃鱼
                       泰迪方法体打印：一只叫做XXX的，X岁的泰迪，正在吃骨头，边吃边蹭
                       哈士奇方法体打印：一只叫做XXX的，X岁的哈士奇，正在吃骨头，边吃边拆家

            测试类中定义一个方法用于饲养动物
                public static void keepPet(ArrayList<???> list){
                    //遍历集合，调用动物的eat方法
                }
            要求1：该方法能养所有品种的猫，但是不能养狗
            要求2：该方法能养所有品种的狗，但是不能养猫
            要求3：该方法能养所有的动物，但是不能传递其他类型
         */


        ArrayList<PersianCat> list1 = new ArrayList<>();
        ArrayList<LiHuaCat> list2 = new ArrayList<>();
        ArrayList<TeddyDog> list3 = new ArrayList<>();
        ArrayList<HuskyDog> list4 = new ArrayList<>();

        keepPet(list1);
        keepPet(list2);
        keepPet(list3);
        keepPet(list4);
    }

    //该方法能养所有的动物，但是不能传递其他类型
    public static void keepPet(ArrayList<? extends Animal> list){
        //遍历集合，调用动物的eat方法
    }



  /*  //  要求2：该方法能养所有品种的狗，但是不能养猫
    public static void keepPet(ArrayList<? extends Dog> list){
        //遍历集合，调用动物的eat方法
    }*/


    /*//要求1：该方法能养所有品种的猫，但是不能养狗
    public static void keepPet(ArrayList<? extends Cat> list){
        //遍历集合，调用动物的eat方法
    }*/
}

```

## 知识加油站

### 1. 数据结构

笔记小结：

1.  概述：数据结构是计算机**存储、组织数据的方式**
2.  栈: 后进先出，先进后出。
3.  队列: 先进先出，后进后出。
4.  数组: 内存连续区域，查询快，增删慢。
5.  链表: 元素是游离的，查询慢，首尾操作极快。
6.  树：请查看各个小节

#### 1.1 概述

​ 数据结构是计算机存储、组织数据的方式，是指数据元素之间的相互关系，以及它们之间在计算机存储器中的组织方式。常见的数据结构包括数组、链表、栈、队列、树、图等，它们在实际的计算机程序设计中扮演着重要的角色

![](<assets/1740015252619.png>)

#### 1.2 栈

栈特点：先进后出，后进先出

![](<assets/1740015252823.png>)

java 中方法

![](<assets/1740015253051.png>)

说明：

方法存在栈内存中，当 function<-method<-main 方法入栈后，会以 function->methon->main 的方式出栈

#### 1.3 队列

队列特点：先进先出，后进后出

![](<assets/1740015253370.png>)

说明：

类似于生活中的排队买票

#### 1.4 数组

特点：数组是一种**查询快, 增删慢**的模型

![](<assets/1740015253679.png>)

*   ** 查询速度快:** 查询数据通过地址值和索引定位，查询任意数据耗时相同。(元素在内存中是连续存储的)
*   **删除效率低**: 要将原始数据删除，同时后面每个数据前移。
*   **添加效率极低**: 添加位置后的每个数据后移，再添加元素。

#### 1.5 链表

特点：链表中的结点是独立的对象，在内存中是不连续的，每个结点包含数据值和下一个结点的地址

![](<assets/1740015253920.png>)

*   链表查询慢，无论查询哪个数据都要从头开始找。
*   链表增删相对快

![](<assets/1740015254195.png>)

#### 1.6 树

笔记小节：

1.  概述：
    
    *   节点: 在树结构中, 每一个元素称之为节点
        
        ![](<assets/1740015254576.png>)
        
    *   度: 每一个节点的子节点数量称之为度
        
2.  二叉树：
    
    ​ 每个节点**最多**有**两个子节点**，分别称为左子节点和右子节点。左子节点的**值小于或等于**父节点的**值**，右子节点的**值大于**父节点的**值**，这种性质使得二叉树在查找、排序等方面有很好的应用。二叉树的子树也是二叉树，即每个节点都可以看作是根节点
    
    ![](<assets/1740015254762.png>)
    
3.  二叉查找数：
    
    ​ 每个节点包含一个**键值对**（key-value）、左子树的所有节点的**键值小于根节点的键值**、右子树的所有节点的**键值大于根节点的键值**、**左子树**和**右子树**也是二叉查找树。
    
    ![](<assets/1740015255157.png>)
    
4.  平衡二叉树：
    
    ​ 叉树左右两个**子树的高度**差不超过 1、任意节点的左右两个子树都是一颗**平衡二叉树**
    
    ![](<assets/1740015255437.png>)
    
5.  红黑数:
    
    1.  特点：平衡二叉 B 树、每一个节点可以是红或者黑、红黑树**不是高度平衡**的, **它的平衡**是通过 " 自己的**红黑规则** " 进行实现的
        
    2.  红黑规则：
        
        ​ 每一个**节点**或是**红色**的, 或者是**黑色**的。根节点必须是黑色。如果一个节点没有子节点或者父节点, 则该节点相应的指针属性值为 **Nil**, 这些 Nil 视为叶节点, 每个**叶节点 (Nil) 是黑色**的。如果某一个节点是红色, 那么它的**子节点**必须是黑色 (不**能出现两个红色节点相连** 的情况)。对每一个节点, 从该节点到其所有后代叶节点的简单路径上, 均包含相同**数目的黑色节点**
        
    
    ![](<assets/1740015255682.png>)
    
6.  树的演变
    
    ![](<assets/1740015255980.png>)
    

##### 1.6.1 二叉树

特点：二叉树中, 任意一个节点的度要小于等于 2

*   节点: 在树结构中, 每一个元素称之为节点
    
    ![](<assets/1740015256356.png>)
    
*   度: 每一个节点的子节点数量称之为度
    

![](<assets/1740015256609.png>)

##### 1.6.2 二叉查找树

特点：

*   每一个节点上最多有两个子节点
*   任意节点左子树上的值都小于当前节点
*   任意节点右子树上的值都大于当前节点

![](<assets/1740015256954.png>)

##### 1.6.3 平衡二叉树

![](<assets/1740015257255.png>)

特点：

*   二叉树左右两个子树的高度差不超过 1
*   任意节点的左右两个子树都是一颗平衡二叉树

旋转：

1.  旋转触发时机
    
    *   当添加一个节点之后, 该树不再是一颗平衡二叉树
2.  左旋：
    
    *   就是将根节点的右侧往左拉, 原先的右子节点变成新的父节点, 并把多余的左子节点出让, 给已经降级的根节点当右子节点
        *   ![](<assets/1740015257442.png>)
            
        *   ![](<assets/1740015257626.png>)
            
3.  右旋：
    
    *   就是将根节点的左侧往右拉, 左子节点变成了新的父节点, 并把多余的右子节点出让, 给已经降级根节点当左子节点
        *   ![](<assets/1740015257927.png>)
            
        *   ![](<assets/1740015258191.png>)
            
4.  平衡二叉树旋转的四种情况：
    
    1.  左左：当根节点左子树的左子树有节点插入, 导致二叉树不平衡
        
        *   如何旋转: 直接对整体进行右旋即可
            
            ![](<assets/1740015258484.png>)
            
    2.  左右: 当根节点左子树的右子树有节点插入, 导致二叉树不平衡
        
        *   如何旋转: 先在左子树对应的节点位置进行左旋, 在对整体进行右旋
            
            ![](<assets/1740015258688.png>)
            
    3.  右右: 当根节点右子树的右子树有节点插入, 导致二叉树不平衡
        
        *   如何旋转: 直接对整体进行左旋即可
    
    *   ![](<assets/1740015258920.png>)
        
5.  右左: 当根节点右子树的左子树有节点插入, 导致二叉树不平衡
    
    *   如何旋转: 先在右子树对应的节点位置进行右旋, 在对整体进行左旋
        
        ![](<assets/1740015259330.png>)
        

参考资料：[集合进阶 - 11 数据结构（平衡二叉树旋转机制）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV17F411T7Ao?p=195&spm_id_from=pageDriver&vd_source=dddaa7c77aa38a5c24657d40a4f357c9)

##### 1.6.4 红黑树

**特点：**

*   平衡二叉 B 树
*   每一个节点可以是红或者黑
*   红黑树不是高度平衡的, 它的平衡是通过 "自己的红黑规则" 进行实现的

**红黑规则：**

1.  每一个节点或是红色的, 或者是黑色的
    
2.  根节点必须是黑色
    
3.  如果一个节点没有子节点或者父节点, 则该节点相应的指针属性值为 Nil, 这些 Nil 视为叶节点, 每个叶节点 (Nil) 是黑色的
    
4.  如果某一个节点是红色, 那么它的子节点必须是黑色 (不能出现两个红色节点相连 的情况)
    
5.  对每一个节点, 从该节点到其所有后代叶节点的简单路径上, 均包含相同数目的黑色节点
    

![](<assets/1740015259519.png>)

*   红黑树添加节点的默认颜色：添加节点时, 默认为红色, 效率高
    
    ![](<assets/1740015259922.png>)
    
*   红黑树添加节点后如何保持红黑规则
    
    ![](<assets/1740015260256.png>)
    

### 2. 可变参数

笔记小结：

1.  概述：可变参数是一种 **Java 方法的参数类型**，允许方法接受任意数量的参数
    
2.  特点：可变参数本质上就是一个数组
    
3.  作用：在形参中接收多个数据
    
4.  格式：数据类型… 参数名称
    
    ```
    getSum(1,2,3,4,5,6,7,8,9,10);
    
    public static int getSum( int a,int...args) {
        return 0;
    }
    
    ```
    
5.  注意：
    
    *   形参列表中可变参数只能有一个可变参数
    *   形参列表中可变参数必须放在形参列表的最后面

#### 2.1 定义

​ 可变参数是一种 Java 方法的参数类型，允许方法接受任意数量的参数。在方法声明中，可变参数使用省略号 (…) 来表示，它可以接收 0 个或多个相同类型的参数。使用可变参数的方法可以接受任意数量的参数，而无需提前声明它们的数量。在方法中，可变参数实际上是作为数组来处理的

#### 2.2 特点

*   可变参数本质上就是一个数组

#### 2.3 作用

*   在形参中接收多个数据

格式

*   数据类型… 参数名称

基本用例：

```
public class ArgsDemo4 {
    public static void main(String[] args) {
        //可变参数的小细节：
        //1.在方法的形参中最多只能写一个可变参数
        //可变参数，理解为一个大胖子，有多少吃多少
        //2.在方法的形参当中，如果出了可变参数以外，还有其他的形参，那么可变参数要写在最后
        getSum(1,2,3,4,5,6,7,8,9,10);
    }

    public static int getSum( int a,int...args) {
        return 0;
    }
}

```

### 3.Lambda 表达式

#### 3.1 概述

​ Lambda 表达式是 Java 编程语言中引入的一种**语法特性**，用于**创建匿名函数或闭包**。它允许您以更简洁的方式定义**单个方法的接口实现**。Lambda 表达式在函数式编程和简化代码编写方面具有重要作用，特别是在集合处理和多线程编程中。

补充：

​ 此处的单个方法的接口实现，简称函数式接口。该接口有且仅有一个抽象方法。有且**仅有一个**抽象方法的接口叫做**函数式接口**，接口上方可以加 **@Functionallnterface** 注解。

![](<assets/1740015260593.png>)

说明：

​ 在本方法中，简化 new 出来的接口为箭头函数

#### 3.2 作用

​ 把 Lambda 表达式理解为是一段可以传递的代码，它可以写出**更简洁**、**更灵活**的代码，作为一种更紧凑的代码风格，使 [Java 语言](https://so.csdn.net/so/search?q=Java%E8%AF%AD%E8%A8%80&spm=1001.2101.3001.7020)表达能力得到了提升。换句话说，**简化**函数式接口的**匿名内部类的写法**。

#### 3.3 语法

**省略核心**: 可推导, 可省略

*   **参数类型**可以**省略**不写。
*   如果只有**一个参数**，参数**类型**可以**省略**，同时 **() **也可以**省略 **。
*   如果 Lambda 表达式的方法体只有**一行**，**大括号，分号，return 可以省略**不写，需要同时省略。

#### 3.4 函数式编程

​ 函数式编程 (Functional programming）是一种思想特点。面向对象∶先找对象，让对象做事情。

​ 函数式编程思想，忽略面向对象的复杂语法，**强调做什么，而不是谁去做。**

格式：

```
() ->{ //( )对应着方法的形参 ->固定格式
    
} //{ } 对应着方法的方法体

```

说明：

*   Lambda 表达式可以用来**简化匿名内部类**的书写
*   Lambda 表达式**只能简化函数式接口的匿名内部类**的写法

### 4. 函数式接口

​ 函数式接口是指只包含一个抽象方法的接口。在 Java 中，可以使用`@FunctionalInterface`注解来标记一个接口是函数式接口，这样做可以帮助编译器检查接口是否符合函数式接口的定义。

​ 函数式接口在 Java 8 中引入了 Lambda 表达式和方法引用等新特性。**Lambda 表达式**可以将函数式接口作为参数进行传递，从而简化代码实现。Java 标准库中的一些函数式接口包括`Consumer`、`Supplier`、`Predicate`、`Function`等等，这些接口可以用于在 Java 中实现函数式编程的思想。

​ 函数式接口，也就意味着，可以使用 **Lambad 表达式**或者**匿名内部类**进行传递，从而简化代码实现。

### 5. 迭代器源码分析

![](<assets/1740015260859.png>)

说明：

*   list.iterator()，每调用一次就会创建一个新的迭代器
    
*   hasNext()，判断当前指针位置是否和迭代对象的长度相同
    
*   next()，先判断是否使用了集合中的添加或者删除方法，然后返回当前索引的元素，然后将指针移动到下一个位置
    

### 6.Equals()`方法和`==` 运算符区别

`equals()`方法是用于比较两个对象的值是否相等，通常在自定义类中重写`equals()`方法来实现自定义的相等比较规则。默认情况下，`equals()`方法实际上等同于`==`运算符，即比较两个对象的引用是否相等。

而`==`运算符用于比较两个对象的引用是否指向同一内存地址。如果两个对象的引用指向同一个内存地址，那么它们是相等的，否则它们不相等。

```
String str1 = "hello";
String str2 = "hello";
String str3 = new String("hello");

System.out.println(str1 == str2); // true，因为它们指向同一字符串常量池中的对象
System.out.println(str1 == str3); // false，因为它们指向不同的内存地址
System.out.println(str1.equals(str3)); // true，因为它们的值相等

```

​ 需要注意的是，对于基本数据类型，`==`运算符可以用于比较它们的值是否相等，因为基本数据类型在内存中存储的是它们的值而不是引用。但对于对象类型，`==`运算符比较的是对象的引用，而不是值。因此，在比较对象相等性时，应该使用`equals()`方法而不是`==`运算符。