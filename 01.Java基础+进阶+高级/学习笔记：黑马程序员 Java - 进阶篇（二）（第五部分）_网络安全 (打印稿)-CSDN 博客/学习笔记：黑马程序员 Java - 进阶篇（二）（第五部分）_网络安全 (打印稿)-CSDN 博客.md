---
url: https://blog.csdn.net/D_boj/article/details/132257812
title: 学习笔记：黑马程序员 Java - 进阶篇（二）（第五部分）_网络安全 (打印稿)-CSDN 博客
date: 2025-02-20 09:34:10
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

*   [25. 多线程](#25_13)
*   *   [25.1 概述](#251_17)
    *   *   [25.1.1 进程和线程](#2511_29)
        *   [25.1.2 并发和并行](#2512_47)
        *   [25.1.3 线程调度](#2513_57)
        *   [25.1.4 守护线程](#2514_75)
    *   [25.2 实现方式](#252_85)
    *   *   [25.2.1 概述](#2521_92)
        *   [25.2.2 继承 Thread 类](#2522Thread_103)
        *   *   [25.2.2.1 构造方法](#25221_105)
            *   [25.2.2.2 常用成员方法](#25222_112)
            *   [25.2.2.3 案例 - 打印线程](#25223_119)
        *   [25.2.3 实现 Runnable 接口](#2523Runnable_159)
        *   *   [25.2.3.1 常用成员方法](#25231_161)
            *   [25.2.3.3 案例 - 打印线程](#25233_163)
        *   [25.2.4 实现 Callable 接口](#2524Callable_203)
        *   *   [25.2.4.1 常用成员方法](#25241_205)
            *   [25.2.4.3 案例 - 打印线程](#25243_213)
    *   [25.3 常用成员方法](#253_259)
    *   *   [25.3.1 概述](#2531_269)
        *   [25.3.2setName(String name)](#2532setNameString_name_279)
        *   [25.3.3getName()](#2533getName_298)
        *   [25.3.4currentThread()](#2534currentThread_307)
        *   [25.3.5sleep(long millis)](#2535sleeplong_millis_323)
        *   [25.3.6getPriority()](#2536getPriority_332)
        *   [25.3.7setPriority(int newPriority)](#2537setPriorityint_newPriority_346)
        *   [25.3.8setDaemon(boolean on)](#2538setDaemonboolean_on_359)
        *   [25.3.9yield()](#2539yield_368)
        *   [25.3.10jion()](#25310jion_381)
    *   [25.4 生命周期](#254_394)
    *   [25.5 线程的安全问题](#255_408)
    *   *   [25.5.1 同步代码块](#2551_439)
        *   *   [25.5.1.1 概述](#25511_441)
            *   *   [25.5.1.1.1 定义](#255111_443)
                *   [25.5.1.1.2 特点](#255112_447)
            *   [25.5.1.2 基本用例](#25512_452)
            *   [25.5.1.3 常用方式](#25513_486)
            *   [25.5.1.4 应用场景](#25514_505)
        *   [25.5.2 同步方法](#2552_513)
        *   *   [25.5.2.1 概述](#25521_515)
            *   *   [25.5.2.1.1 定义](#255211_517)
                *   [25.5.2.1.2 特点](#255212_521)
            *   [25.5.2.4 基本用例](#25524_529)
            *   [25.5.2.5 常用方式](#25525_565)
            *   [25.5.2.6 案例 - 订票](#25526_583)
        *   [25.5.3Lock 锁](#2553Lock_639)
        *   *   [25.5.3.1 概述](#25531_641)
            *   *   [25.5.3.1.1 定义](#255311_643)
                *   [25.5.3.1.2 特点](#255312_647)
            *   [25.5.3.4 基本用例](#25534_655)
            *   [25.5.3.5 常用方式](#25535_696)
        *   [25.5.4 死锁](#2554_713)
    *   [25.6 生产者和消费者](#256_763)
    *   *   [25.6.1 概述](#2561_777)
        *   *   [25.6.1.1 定义](#25611_779)
            *   [25.6.1.2 生产者消费者模型](#25612_783)
            *   [25.6.1.3 等待唤醒机制](#25613_788)
        *   [25.6.2 常用的成员方法](#2562_792)
        *   [25.6.3 案例 - 吃货与厨师](#2563_800)
    *   [25.7 阻塞队列](#257_945)
    *   *   [25.7.1 概述](#2571_949)
        *   *   [25.7.1.1 定义](#25711_951)
            *   [25.7.1.2 继承结构](#25712_955)
        *   [25.7.2 案例 - 吃货与厨师](#2572_965)
    *   [25.8 线程的状态](#258_1062)
    *   [25.9 线程栈](#259_1084)
    *   *   [25.9.1 概述](#2591_1088)
        *   [25.9.2 内存图](#2592_1092)
        *   [25.9.3 案例 - 抽奖池](#2593_1100)
    *   [25.10 案例 - 抽奖池](#2510_1176)
    *   [25.11 线程池](#2511_1291)
    *   *   [25.11.1 概述](#25111_1308)
        *   *   [25.11.1.1 含义](#251111_1310)
            *   [25.11.1.2 主要核心原理](#251112_1314)
            *   [25.11.1.3 优点](#251113_1318)
        *   [25.11.2 基本用例](#25112_1325)
    *   [25.12 自定义线程池](#2512_1369)
    *   *   [25.12.1 概述](#25121_1387)
        *   [25.12.2 构造方法](#25122_1391)
        *   [25.12.3 基本用例](#25123_1407)
        *   [25.12.4 拒绝策略](#25124_1421)
    *   [25.13 线程常见问题](#2513_1425)
    *   *   [25.13.1 最大并行数](#25131_1433)
        *   *   [25.13.1.1 概述](#251311_1435)
            *   [25.13.1.2 基本用例](#251312_1439)
        *   [25.13.2 线程池大小](#25132_1455)
        *   [25.13.3 线程池额外扩展知识](#25133_1459)
    *   [25.13 工具类](#2513_1467)
    *   *   [25.13.1 概述](#25131_1473)
        *   [25.13.2 构造方法](#25132_1479)
*   [26. 网络编程](#26_1483)
*   *   [26.1 概述](#261_1504)
    *   *   [26.1.1 定义](#2611_1506)
        *   [26.1.2 软件架构](#2612_1512)
        *   [26.1.3 网络编程三要素](#2613_1522)
    *   [26.2InetAddress 类](#262InetAddress_1526)
    *   *   [26.2.1 概述](#2621_1528)
        *   [26.2.2 常用成员方法](#2622_1532)
        *   [26.2.3 案例 - 获取基本信息](#2623_1540)
    *   [26.3UDP 通信程序](#263UDP_1559)
    *   *   [26.3.1 概述](#2631_1561)
        *   *   [26.3.1.1 定义](#26311_1563)
            *   [26.3.1.2UDP 三种通讯方式](#26312UDP_1569)
        *   [26.3.2 构造方法](#2632_1585)
        *   [26.3.3 常用成员方法](#2633_1598)
        *   [26.3.4 基本用例 - 发送数据](#2634_1608)
        *   [26.3.5 基本用例 - 接收数据](#2635_1651)
        *   [26.3.6 案例 - UDP 组播实现](#2636UDP_1703)
        *   *   [26.3.6.1 基本用例 - 发送数据](#26361_1705)
            *   [26.3.6.2 基本用例 - 接收数据](#26362_1731)
        *   [26.3.7 案例 UDP 广播实现](#2637UDP_1759)
        *   *   [26.3.7.1 基本用例 - 发送数据](#26371_1761)
            *   [26.3.7.2 基本用例 - 接收数据](#26372_1787)
    *   [26.4TCP 通信程序](#264TCP_1809)
    *   *   [26.4.1 概述](#2641_1811)
        *   *   [26.4.1.1 定义](#26411_1813)
            *   [26.4.1.2 三次握手](#26412_1821)
            *   [26.4.1.3 四次挥手](#26413_1825)
        *   [26.4.2 基本用例 - 发送数据](#2642_1829)
        *   [26.4.3 基本用例 - 接收数据](#2643_1854)
*   [27. 反射](#27_1889)
*   *   [27.1 概述](#271_1899)
    *   *   [27.1.1 定义](#2711_1901)
        *   [27.1.2 作用](#2712_1907)
    *   [27.2 获取字节码方式](#272_1912)
    *   *   [27.2.1 概述](#2721_1914)
        *   [27.2.2 基本用例 - 获取字节码](#2722_1924)
    *   [27.3 获取构造方法](#273_1947)
    *   *   [27.3.1 概述](#2731_1949)
        *   [27.3.2 基本用例 - 获取构造方法](#2732_1970)
    *   [27.4 获取成员变量](#274_2012)
    *   *   [27.4.1 概述](#2741_2014)
        *   [27.4.2 基本用例 - 获取成员变量](#2742_2034)
    *   [27.5 获取成员方法](#275_2075)
    *   *   [27.5.1 概述](#2751_2077)
        *   [27.5.2 基本用例 - 获取成员方法](#2752_2100)
*   [28. 动态代理](#28_2147)
*   *   [28.1 概述](#281_2155)
    *   *   [28.1.1 定义](#2811_2157)
        *   [28.1.2 作用](#2812_2161)
    *   [28.2 基本用例](#282_2171)
*   [29. 日志](#29_2345)
*   *   [29.1 概述](#291_2353)
    *   *   [29.1.1 定义](#2911_2355)
        *   [29.1.2 作用](#2912_2359)
        *   [29.1.3 日志级别](#2913_2366)
        *   [29.1.4 体系结构](#2914_2376)
    *   [29.2Logback 日志框架](#292Logback_2383)
    *   *   [29.2.1 概述](#2921_2385)
        *   [29.2.2 模块](#2922_2393)
    *   [29.3Logback 基本用例](#293Logback_2403)
    *   [29.4Logback 配置文件](#294Logback_2466)
*   [30. 注解](#30_2485)
*   *   [30.1 概述](#301_2511)
    *   *   [30.1.1 定义](#3011_2513)
        *   [30.1.2 作用](#3012_2530)
        *   [30.1.3 注释和注解的区别](#3013_2534)
    *   [30.2 基本用例](#302_2546)
    *   [30.3 常用注解](#303_2579)
    *   [30.4 自定义注解](#304_2596)
    *   *   [30.4.1 概述](#3041_2598)
        *   [30.4.2 基本用例](#3042_2606)
    *   [30.5 元注解](#305_2676)
    *   *   [30.5.1 概述](#3051_2678)
        *   [30.5.2 分类](#3052_2684)
        *   [30.5.3@Target](#3053Target_2691)
        *   [30.5.4@Retention](#3054Retention_2704)
*   [30. 算法（目前了解，日后补充）](#30_2714)
*   *   [30.1 查找算法](#301_2718)
    *   *   [30.1.1. 基本查找](#3011__2720)
        *   [30.1.2. 二分查找](#3012__2765)
        *   [30.1.3. 插值查找](#3013__2837)
        *   [30.1.4. 斐波那契查找](#3014__2863)
        *   [30.1.5. 分块查找](#3015__2954)
        *   [30.1.6. 哈希查找](#3016__3123)
        *   [30.1.7. 树表查找](#3017__3146)
    *   [30.2 排序算法](#302_3172)
    *   *   [30.2.1. 冒泡排序](#3021__3174)
        *   *   [30.2.1.1 算法步骤](#30211__3184)
            *   [30.2.1.2 动图演示](#30212__3190)
            *   [30.2.1.3 代码示例](#30213__3195)
        *   [30.2.2. 选择排序](#3022__3248)
        *   *   [30.2.2.1 算法步骤](#30221__3250)
            *   [30.2.2.2 动图演示](#30222__3259)
        *   [30.2.3. 插入排序](#3023__3328)
        *   *   [30.2.3.1 算法步骤](#30231__3334)
            *   [30.2.3.2 动图演示](#30232__3342)
        *   [30.2.4. 快速排序](#3024__3405)
        *   *   [30.2.4.1 算法步骤](#30241__3415)
            *   [30.2.4.2 动图演示](#30242__3427)
*   [30. 补充（目前了解，日后补充）](#30_3542)
*   *   [30.1 类加载器](#301_3544)
    *   *   [30.1.1 概述](#3011_3546)
        *   [30.1.2 类加载的时机](#3012_3552)
        *   [30.1.3 类加载的过程](#3013_3563)
        *   [30.1.4 类加载的分类](#3014_3598)
        *   [30.1.5 双亲委派模型](#3015_3606)
        *   [30.1.6 类加载器常用方法](#3016_3612)
    *   [30.2XML](#302XML_3649)
    *   *   [30.2.1 三种配置文件的优缺点](#3021_3651)
        *   [30.2.2 概述](#3022_3655)
        *   [30.2.3 使用](#3023_3675)
        *   [30.2.4 文档约束](#3024_3717)
        *   *   [30.2.4.1 概述](#30241_3721)
            *   [30.2.4.2 分类](#30242_3725)
    *   [30.3 单元测试](#303_3727)
    *   *   [30.3.1 概述](#3031_3729)
        *   [30.3.2 快速入门](#3032_3731)

## 25. 多线程

笔记小结：见各个小节

### 25.1 概述

笔记小结：

1.  进程：是正在**运行的程序**
2.  线程：是进程中的单个**顺序控制流**，是一条执行路径
3.  并行：在同一时刻，有多个指令在多个 CPU 上**同时**执行
4.  并发：在同一时刻，有多个指令在单个 CPU 上**交替**执行
5.  抢占式调度模型：**优先让优先级高的线程使用 CPU**
6.  分时调度模型：**所有线程轮流使用 CPU** 的使用权，平均分配每个线程占用 CPU 的时间片
7.  守护线程（Daemon Thread）是一种特殊的线程，它在**后台提供**一种**通用的服务**

#### 25.1.1 进程和线程

进程：是正在运行的程序

*   独立性：进程是一个能独立运行的基本单位，同时也是系统分配资源和调度的独立单位
*   动态性：进程的实质是程序的一次执行过程，进程是动态产生，动态消亡的
*   并发性：任何进程都可以同其他进程一起并发执行

线程：是进程中的单个顺序控制流，是一条执行路径

*   单线程：一个进程如果只有一条执行路径，则称为单线程程序
    
*   多线程：一个进程如果有多条执行路径，则称为多线程程序
    

​ 线程是操作系统能够进行运算调度的最小单位。它被包含在**进程**之中，是进程中的实际运作单位。

![](<assets/1740015251006.png>)

#### 25.1.2 并发和并行

*   并行：在同一时刻，有多个指令在多个 CPU 上**同时**执行。
    
    ![](<assets/1740015251244.png>)
    
*   并发：在同一时刻，有多个指令在单个 CPU 上**交替**执行。
    
    ![](<assets/1740015251638.png>)
    

#### 25.1.3 线程调度

​ 抢占式调度模型：优先让优先级高的线程使用 CPU，如果线程的优先级相同，那么会随机选择一个，优先级高的线程获取的 CPU 时间片相对多一些

![](<assets/1740015251825.png>)

说明：

​ CPU 调用线程的几率是随机的，调用时长也不确定

​ 分时调度模型：所有线程轮流使用 CPU 的使用权，平均分配每个线程占用 CPU 的时间片

![](<assets/1740015252126.png>)

说明：

CPU 调用线程是依次的，调用时长也比较固定

#### 25.1.4 守护线程

​ 在 Java 中，守护线程（Daemon Thread）是一种特殊的线程，它在**后台提供**一种**通用的服务**。当 Java 虚拟机（JVM）中只剩下守护线程时，JVM 会退出。与用户线程不同，守护线程并不会阻止 JVM 的退出

![](<assets/1740015252447.png>)

说明：

​ 当聊天时，还可以传输文件

### 25.2 实现方式

笔记小结：

*   实现方式：**继承** Thread 类、**实现** Runnable、Callable 接口
*   使用方式：通过**线程对象. start() **或者 `FutureTask<String> ft = new FutureTask<>(mc);`来接收**线程返回结果**。详细请查看各个小节

#### 25.2.1 概述

![](<assets/1740015252689.png>)

*   实现 Runnable、Callable 接口
    *   好处: 扩展性强，实现该接口的同时还可以继承其他的类
    *   缺点: 编程相对复杂，不能直接使用 Thread 类中的方法
*   继承 Thread 类
    *   好处: 编程比较简单，可以直接使用 Thread 类中的方法
    *   缺点: 可以扩展性较差，不能再继承其他的类

#### 25.2.2 继承 Thread 类

##### 25.2.2.1 构造方法

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>Thread(Runnable target)</td><td>分配一个新的 Thread 对象</td></tr><tr><td>Thread(Runnable target, String name)</td><td>分配一个新的 Thread 对象</td></tr></tbody></table>

##### 25.2.2.2 常用成员方法

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>void run( )</td><td>在线程开启后，此方法将被调用执行</td></tr><tr><td>void start( )</td><td>使此线程开始执行，Java 虚拟机会调用 run 方法 ()</td></tr></tbody></table>

##### 25.2.2.3 案例 - 打印线程

实现步骤：

1.  定义一个类 MyThread 继承 Thread 类
2.  在 MyThread 类中重写 run() 方法
3.  创建 MyThread 类的对象
4.  启动线程

```
public class MyThread extends Thread {
    @Override
    public void run() {
        for(int i=0; i<100; i++) {
            System.out.println(i);
        }
    }
}
public class MyThreadDemo {
    public static void main(String[] args) {
        MyThread my1 = new MyThread();
        MyThread my2 = new MyThread();

//        my1.run();
//        my2.run();

        //void start() 导致此线程开始执行; Java虚拟机调用此线程的run方法
        my1.start();
        my2.start();
    }
}

```

注意：

1.  在类中重写 run 方法，因为 run() 是用来封装被线程执行的代码
2.  run() 和 start() 方法的区别是
    *   run()：封装线程执行的代码，直接调用，相当于普通方法的调用
    *   start()：启动线程；然后由 JVM 调用此线程的 run() 方法

#### 25.2.3 实现 Runnable 接口

##### 25.2.3.1 常用成员方法

##### 25.2.3.3 案例 - 打印线程

实现步骤:

1.  定义一个类 MyRunnable 实现 Runnable 接口
2.  在 MyRunnable 类中重写 run() 方法
3.  创建 MyRunnable 类的对象
4.  创建 Thread 类的对象，把 MyRunnable 对象作为构造方法的参数
5.  启动线程

```
public class MyRunnable implements Runnable {
    @Override
    public void run() {
        for(int i=0; i<100; i++) {
            // 此处通过线程来进行获取名称，不能直接使用getName()方法
            System.out.println(Thread.currentThread().getName()+":"+i);
        }
    }
}
public class MyRunnableDemo {
    public static void main(String[] args) {
        //创建MyRunnable类的对象
        MyRunnable my = new MyRunnable();

        //创建Thread类的对象，把MyRunnable对象作为构造方法的参数
        //Thread(Runnable target)
//        Thread t1 = new Thread(my);
//        Thread t2 = new Thread(my);
        //Thread(Runnable target, String name)
        Thread t1 = new Thread(my,"坦克");
        Thread t2 = new Thread(my,"飞机");

        //启动线程
        t1.start();
        t2.start();
    }
}

```

#### 25.2.4 实现 Callable 接口

##### 25.2.4.1 常用成员方法

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>V call()</td><td>计算结果，如果无法计算结果，则抛出一个异常</td></tr><tr><td>FutureTask(Callable callable)</td><td>创建一个 FutureTask，一旦运行就执行给定的 Callable</td></tr><tr><td>V get()</td><td>如有必要，等待计算完成，然后获取其结果</td></tr></tbody></table>

##### 25.2.4.3 案例 - 打印线程

实现步骤：

1.  定义一个类 MyCallable 实现 Callable 接口
2.  在 MyCallable 类中重写 call() 方法
3.  创建多线程要运行的参数对象 MyCallable 类
4.  创建多线程运行结果的管理者对象 **Future 的实现类 FutureTask 对象**，把 MyCallable 对象作为构造方法的参数
5.  创建线程对象 Thread 类，把 FutureTask 对象作为构造方法的参数
6.  启动线程
7.  再调用 get 方法，就可以获取线程结束之后的结果

```
public class MyCallable implements Callable<String> {
    @Override
    public String call() throws Exception {
        for (int i = 0; i < 100; i++) {
            System.out.println("跟女孩表白" + i);
        }
        //返回值就表示线程运行完毕之后的结果
        return "答应";
    }
}
public class Demo {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        //线程开启之后需要执行里面的call方法
        MyCallable mc = new MyCallable();

        //Thread t1 = new Thread(mc);

        //可以获取线程执行完毕之后的结果.也可以作为参数传递给Thread对象
        FutureTask<String> ft = new FutureTask<>(mc);

        //创建线程对象
        Thread t1 = new Thread(ft);

        String s = ft.get();
        //开启线程
        t1.start();

        //String s = ft.get();
        System.out.println(s);
    }
}

```

### 25.3 常用成员方法

笔记小结：

*   Thread 对象。多线程常用的成员方法，通常有`getName（）`、`currentEhread()`

![](<assets/1740015252972.png>)

#### 25.3.1 概述

​ Thread 对象。多线程常用的成员方法

![](<assets/1740015253339.png>)

补充：

设置线程的优先级范围为 1-10

#### 25.3.2setName(String name)

```
/* 格式：
	Thread对象.setName(String name)
	作用：为线程起名字*/
thred.setName("玥玥");

```

细节：

*   如果我们没有给线程设置名字，线程也是有默认的名字的
    
    ```
    格式：Thread-X（X序号，从0开始的）
    
    ```
    
*   如果我们要给线程设置名字，可以用 set 方法进行设置，也可以构造方法设置
    

#### 25.3.3getName()

```
/* 格式：
	Thread对象.getName()
	作用：获取线程的名字*/
thred.getName()

```

#### 25.3.4currentThread()

```
/* 格式：
	Thread对象.currentThread()
	作用：获取对当前正在执行的线程对象的引用*/
thred.currentThread()

```

细节：

*   ​ 当 JVM 虚拟机启动之后，会自动的启动多条线程。
*   ​ 其中有一条线程就叫做 main 线程
*   ​ 他的作用就是去调用 main 方法，并执行里面的代码
*   ​ 在以前，我们写的所有的代码，其实都是运行在 main 线程当中

#### 25.3.5sleep(long millis)

```
/6 格式：
	Thread对象.sleep(long millis)
	作用：使当前正在执行的线程停留（暂停执行）指定的毫秒数*/
thred.sleep(1000)

```

#### 25.3.6getPriority()

```
/* 格式：
	Thread对象.getPriority()
	作用：返回此线程的优先级*/
thred.getPriority() // 5

```

细节：

*   Java 的最小优先级为 1，默认为 5，最大为 10
*   ![](<assets/1740015253639.png>)
    

#### 25.3.7setPriority(int newPriority)

```
/* 格式：
	Thread对象.setPriority(int newPriority)
	作用：更改此线程的优先级，范围1-10*/
thred.setPriority(10) // 5

```

说明：

优先级越高，并不代表就一定先执行，只是抢占的概率更高

#### 25.3.8setDaemon(boolean on)

```
/* 格式：
	Thread对象.setDaemon(boolean on)
	作用：将此线程标记为守护线程，当运行的线程都是守护线程时，Java虚拟机将退出*/
thred.setDaemon(True) 

```

#### 25.3.9yield()

```
/* 格式：
	Thread对象.yield()
	作用：让线程输出更均衡一点*/
thred.yield()

```

说明：

礼让线程，在线程中加入此方法，可以让线程输出更均衡一点。不常用，了解即可

#### 25.3.10jion()

```
/* 格式：
	Thread对象.jion()
	作用：将此线程在某线程之前插队执行完成*/
thred.jion()

```

说明：

插入线程，在线程中加入此方法，可以将此线程在某线程之前插队执行完成。不常用，了解即可

### 25.4 生命周期

笔记小结：请查看本小节

![](<assets/1740015253882.png>)

说明：

​ 创建线程对象，调用`start()`方法。此时线程会不断的抢中 CPU 直到同时拥有执行资格和执行权。此时该线程会执行该任务，该任务执行结束后，该线程会变为无执行资格和执行权的线程。往复循环。若总任务执行完成，则线程消亡

补充：

​ sleep 方法会让线程睡眠，睡眠时间到了之后，立马就会执行下面的代码吗? 答案是不会的，此时线程进入就绪状态

### 25.5 线程的安全问题

笔记小结：

1.  synchronized 关键字，实现代码加锁。
    
    ```
    static Object object = new Object(); // 注意：此时锁的对象需要确保唯一，否则加锁失效
    synchronized (object) { 
    	……
    }
    
    ```
    
2.  同步方法:
    
    ```
    public synchronized boolean method() { // 此时锁对象不能够自定义
    	……
    }
    
    ```
    
3.  Lock 锁：
    
    ```
    ReentrantLock lock = new ReentrantLock(); //创建锁对象
    lock.lock(); // 加锁
    lock.unlock(); // 释放锁
    
    ```
    
4.  死锁：线程和线程之间**相互等待**，造成了**相互阻塞**的状态，使得程序无法继续执行
    

#### 25.5.1 同步代码块

##### 25.5.1.1 概述

###### 25.5.1.1.1 定义

​ 把操作共享数据的代码锁起来

###### 25.5.1.1.2 特点

*   锁默认打开，有一个线程进去了，锁自动关闭
*   里面的代码全部执行完毕, 线程出来, 锁自动打开

##### 25.5.1.2 基本用例

```
public class MyThread extends Thread {

    //表示这个类所有的对象，都共享ticket数据
    static int ticket = 0;//0 ~ 99

    @Override
    public void run() {
            while (true) {
                synchronized (MyThread.class) {
                //同步代码块
                if (ticket < 100) {
                    try {
                        Thread.sleep(10);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    ticket++;
                    System.out.println(getName() + "正在卖第" + ticket + "张票！！！");
                } else {
                    break;
                }
            }
        }
    }
}

```

细节：

通常来说一个类对象的字节码文件，也就是 MyThread.class，是唯一的。因此，符合锁对象是唯一的条件

##### 25.5.1.3 常用方式

```
/* 格式：
 synchronized (锁) {
	操作共享数据的代码} */
// 锁对象，一定需要是唯一的
static Object object = new Object();
synchronized (object) {
	……
}

```

细节：

​ synchronized 中的锁对象，一定是需要唯一的，若此时是不同的锁对象，那么此为此代码加上的锁就毫无意义

![](<assets/1740015254085.png>)

##### 25.5.1.4 应用场景

![](<assets/1740015254359.png>)

说明：

​ 若多个线程同时进行买票，那么当执行到 ticket++ 此代码时，cpu 的执行权被抢走，而没有执行到 sout 方法，就会导致逻辑错误

#### 25.5.2 同步方法

##### 25.5.2.1 概述

###### 25.5.2.1.1 定义

​ Java 中的同步方法是指一种能够保证在**多线程访问时数据的正确性**的机制。

###### 25.5.2.1.2 特点

1.  同步方法在执行时会自动**获取当前对象的锁**
2.  如果一个线程已经获取了当前对象的锁，其他线程就必须**等待**该**线程执行完毕**释放锁之后才能获得锁并执行同步方法
3.  同步方法可以**保证线程安全**，避免多个线程同时对同一个对象进行修改的问题
4.  同步方法也可能会**导致线程阻塞**和**性能下降**，因此需要谨慎使用
5.  **锁对象不能自己指定**

##### 25.5.2.4 基本用例

```
public class MyRunnable implements Runnable {

    int ticket = 0;

    @Override
    public void run() {
        //1.循环
        while (true) {
            //2.同步代码块（同步方法）
            if (method()) break;
        }
    }

    //this
    private synchronized boolean method() {
        //3.判断共享数据是否到了末尾，如果到了末尾
        if (ticket == 100) {
            return true;
        } else {
            //4.判断共享数据是否到了末尾，如果没有到末尾
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            ticket++;
            System.out.println(Thread.currentThread().getName() + "在卖第" + ticket + "张票！！！");
        }
        return false;
    }
}

```

##### 25.5.2.5 常用方式

```
/* 格式：
修饰符 synchronized 返回值类型 方法名(方法参数) { 
	方法体；	} */

public synchronized boolean method() {
	……
}

```

细节：

*   锁对象，不能自己指定
    *   静态方法：锁对象为当前对象的字节码文件
    *   非静态方法：锁对象为 this

##### 25.5.2.6 案例 - 订票

```
public class MyRunnable implements Runnable {

    int ticket = 0;

    @Override
    public void run() {
        //1.循环
        while (true) {
            //2.同步代码块（同步方法）
            if (method()) break;
        }
    }

    //this
    private synchronized boolean method() {
        //3.判断共享数据是否到了末尾，如果到了末尾
        if (ticket == 100) {
            return true;
        } else {
            //4.判断共享数据是否到了末尾，如果没有到末尾
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            ticket++;
            System.out.println(Thread.currentThread().getName() + "在卖第" + ticket + "张票！！！");
        }
        return false;
    }
}

```

```
MyRunnable mr = new MyRunnable();

Thread t1 = new Thread(mr);
Thread t2 = new Thread(mr);
Thread t3 = new Thread(mr);

t1.setName("窗口1");
t2.setName("窗口2");
t3.setName("窗口3");

t1.start();
t2.start();
t3.start();

```

说明：

此时每个线程所传入的锁对象是唯一的，都为 mr。因此，保证了同步方法的静态方法中的锁对象唯一

#### 25.5.3Lock 锁

##### 25.5.3.1 概述

###### 25.5.3.1.1 定义

​ Lock 锁是 Java **并发编程中的一种同步机制**，可以用来控制对共享资源的访问

###### 25.5.3.1.2 特点

1.  显式获取和释放锁：synchronized 关键字是隐式获取和释放锁的，而 Lock 锁需要手动获取和释放锁。
2.  可以实现公平锁和非公平锁：synchronized 关键字是非公平锁，即获取锁的顺序是不确定的。而 Lock 锁可以实现公平锁和非公平锁，即获取锁的顺序是可以预测的。
3.  可以中断等待锁的线程：在获取锁的过程中，如果线程等待时间过长，可以中断等待锁的线程。
4.  支持多个条件变量：Lock 锁可以支持多个条件变量，即不同条件下可以对线程进行不同的唤醒操作。
5.  可以提高吞吐量：在高并发场景下，使用 Lock 锁可以提高系统的吞吐量，比 synchronized 关键字更加高效。

##### 25.5.3.4 基本用例

```
public class MyThread extends Thread{

    static int ticket = 0;
	// 此处需要将Lock锁变为静态，以保证全局唯一，而不会创建多个Lock锁
    static Lock lock = new ReentrantLock();

    @Override
    public void run() {
        //1.循环
        while(true){
            //2.同步代码块
            //synchronized (MyThread.class){
            lock.lock(); //2 //3
            try {
                //3.判断
                if(ticket == 100){
                    break;
                    //4.判断
                }else{
                    Thread.sleep(10);
                    ticket++;
                    System.out.println(getName() + "在卖第" + ticket + "张票！！！");
                }
                //  }
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                lock.unlock();
            }
        }
    }
}

```

细节：

​ 当 Lock 锁即将终止时，建议使用 try…catch…finally 的方式进行。因为，finally 的方法始终会执行，因此放在此比较合适

##### 25.5.3.5 常用方式

```
/* 格式：
构造方法：ReentrantLock() 
加锁解锁方法：获得锁 void lock()、释放锁 void unlock()*/
ReentrantLock lock = new ReentrantLock();
lock.lock();
lock.unlock();

```

步骤：

1.  创建 Lock 锁对象
2.  获得锁
3.  释放锁

#### 25.5.4 死锁

​ 死锁是指两个或多个线程在执行过程中，因争夺资源而造成的一种**互相等待的现象**，若无外力作用，它们都将无法继续执行下去。简单来说，就是两个或多个线程都在等待对方先释放锁，造成了相互阻塞的状态，使得程序无法继续执行。

![](<assets/1740015254567.png>)

说明：

当男孩和女孩同时拿到了一根筷子，男孩在等待女孩放下筷子，女孩在等待男孩放下筷子

示例：

```
public class MyThread extends Thread {

    static Object objA = new Object();
    static Object objB = new Object();

    @Override
    public void run() {
        //1.循环
        while (true) {
            if ("线程A".equals(getName())) {
                synchronized (objA) {
                    System.out.println("线程A拿到了A锁，准备拿B锁");//A
                    synchronized (objB) {
                        System.out.println("线程A拿到了B锁，顺利执行完一轮");
                    }
                }
            } else if ("线程B".equals(getName())) {
                if ("线程B".equals(getName())) {
                    synchronized (objB) {
                        System.out.println("线程B拿到了B锁，准备拿A锁");//B
                        synchronized (objA) {
                            System.out.println("线程B拿到了A锁，顺利执行完一轮");
                        }
                    }
                }
            }
        }
    }
}

```

说明：

​ 当线程 A 拿到了 A 锁，线程 B 拿到了 B 锁。于此同时，线程 A 在等待获得 B 锁，线程 B 在等待获得 A 锁，所以就会造成程序卡死，称之为死锁

想要避免死锁，不要使用锁嵌套锁即可

### 25.6 生产者和消费者

笔记小结：

1.  概述：生产者消费者是一种常见的并发模型，用于解决**生产者和消费者之间的数据交换**问题
    
2.  常用成员方法：
    
    <table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>void wait()</td><td>导致当前线程等待，直到另一个线程调用该对象的 notify() 方法或 notifyAll() 方法</td></tr><tr><td>void notify()</td><td>唤醒正在等待对象监视器的单个线程</td></tr><tr><td>void notifyAll()</td><td>唤醒正在等待对象监视器的所有线程</td></tr></tbody></table>
    

#### 25.6.1 概述

##### 25.6.1.1 定义

​ 生产者消费者是一种常见的并发模型，用于解决**生产者和消费者之间的数据交换**问题。

##### 25.6.1.2 生产者消费者模型

*   生产者：负责**生产数据**并将其**存储在共享的数据结构中**
*   消费者：负责从共享数据结构中**获取数据并进行消费**

##### 25.6.1.3 等待唤醒机制

![](<assets/1740015254882.png>)

#### 25.6.2 常用的成员方法

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>void wait()</td><td>导致当前线程等待，直到另一个线程调用该对象的 notify() 方法或 notifyAll() 方法</td></tr><tr><td>void notify()</td><td>唤醒正在等待对象监视器的单个线程</td></tr><tr><td>void notifyAll()</td><td>唤醒正在等待对象监视器的所有线程</td></tr></tbody></table>

#### 25.6.3 案例 - 吃货与厨师

1. 创建桌子类

```
public class Desk {

    /*
    * 作用：控制生产者和消费者的执行
    *
    * */

    //是否有面条  0：没有面条  1：有面条
    public static int foodFlag = 0;

    //总个数
    public static int count = 10;

    //锁对象
    public static Object lock = new Object();

}

```

2. 创建消费者（吃货）

```
public class Foodie extends Thread{

    @Override
    public void run() {
        /*
        * 1. 循环
        * 2. 同步代码块
        * 3. 判断共享数据是否到了末尾（到了末尾）
        * 4. 判断共享数据是否到了末尾（没有到末尾，执行核心逻辑）
        * */

        while(true){
            synchronized (Desk.lock){
                if(Desk.count == 0){
                    break;
                }else{
                    //先判断桌子上是否有面条
                    if(Desk.foodFlag == 0){
                        //如果没有，就等待
                        try {
                            Desk.lock.wait();//让当前线程跟锁进行绑定
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }else{
                        //把吃的总数-1
                        Desk.count--;
                        //如果有，就开吃
                        System.out.println("吃货在吃面条，还能再吃" + Desk.count + "碗！！！");
                        //吃完之后，唤醒厨师继续做
                        Desk.lock.notifyAll();
                        //修改桌子的状态
                        Desk.foodFlag = 0;
                    }
                }
            }
        }
    }
}

```

细节：

​ 当消费者，进行等待的时候，需要调用中间者 `Desk.lock.wait();`这个锁对象的方法，目的是将此当前线程跟锁进行绑定，便于`Desk.lock.notifyAll();`此线程的唤醒操作

3. 创建生产者（厨师）

```
public class Cook extends Thread{

    @Override
    public void run() {
        /*
         * 1. 循环
         * 2. 同步代码块
         * 3. 判断共享数据是否到了末尾（到了末尾）
         * 4. 判断共享数据是否到了末尾（没有到末尾，执行核心逻辑）
         * */

        while (true){
            synchronized (Desk.lock){
                if(Desk.count == 0){
                    break;
                }else{
                    //判断桌子上是否有食物
                    if(Desk.foodFlag == 1){
                        //如果有，就等待
                        try {
                            Desk.lock.wait();
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }else{
                        //如果没有，就制作食物
                        System.out.println("厨师做了一碗面条");
                        //修改桌子上的食物状态
                        Desk.foodFlag = 1;
                        //叫醒等待的消费者开吃
                        Desk.lock.notifyAll();
                    }
                }
            }
        }
    }
}

```

细节：

同上

4. 使用

```
public class ThreadDemo {
    public static void main(String[] args) {
       /*
       *
       *    需求：完成生产者和消费者（等待唤醒机制）的代码
       *         实现线程轮流交替执行的效果
       *
       * */

        //创建线程的对象
        Cook c = new Cook();
        Foodie f = new Foodie();

        //给线程设置名字
        c.setName("厨师");
        f.setName("吃货");

        //开启线程
        c.start();
        f.start();
    }
}

```

### 25.7 阻塞队列

笔记小结：详细查看本小结**概述**

#### 25.7.1 概述

##### 25.7.1.1 定义

​ 阻塞队列是一种特殊的队列，它具有在**插入和删除元素时阻塞等待**的特性。阻塞队列常用于多线程编程中，作为不同线程间的数据交换通道

##### 25.7.1.2 继承结构

![](<assets/1740015255188.png>)

说明：

*   ArrayBlockingQueue: 底层是数组, 有界
    
*   LinkedBlockingQueue: 底层是链表, 无界. 但不是真正的无界, 最大为 int 的最大值
    

#### 25.7.2 案例 - 吃货与厨师

步骤一：创建消费者（吃货）

```
public class Foodie extends Thread{

    ArrayBlockingQueue<String> queue;

    public Foodie(ArrayBlockingQueue<String> queue) {
        this.queue = queue;
    }


    @Override
    public void run() {
        while(true){
                //不断从阻塞队列中获取面条
                try {
                    String food = queue.take();
                    System.out.println(food);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
        }
    }
}

```

说明：

当使用阻塞队列时使用对象的 take() 方法即可从阻塞队列中获得数据

步骤二：创建生产者（厨师）

```
public class Cook extends Thread{

    ArrayBlockingQueue<String> queue;

    public Cook(ArrayBlockingQueue<String> queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        while(true){
            //不断的把面条放到阻塞队列当中
            try {
                queue.put("面条");
                System.out.println("厨师放了一碗面条");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}


```

说明：

​ 当使用阻塞队列时使用对象的 put() 方法即可从阻塞队列中塞入数据

步骤三：演示

```
ublic class ThreadDemo {
    public static void main(String[] args) {
       /*
       *
       *    需求：利用阻塞队列完成生产者和消费者（等待唤醒机制）的代码
       *    细节：
       *           生产者和消费者必须使用同一个阻塞队列
       *
       * */

        //1.创建阻塞队列的对象
        ArrayBlockingQueue<String> queue = new ArrayBlockingQueue<>(1);


        //2.创建线程的对象，并把阻塞队列传递过去
        Cook c = new Cook(queue);
        Foodie f = new Foodie(queue);

        //3.开启线程
        c.start();
        f.start();

    }
}

```

说明：

​ 创建阻塞对象，传入生产者与消费者，目的是将厨师（生产者）与吃货（消费者）使用的阻塞队列统一

### 25.8 线程的状态

笔记小结：详细查看本小节

在 Java 中，线程的状态指的是线程对象在其生命周期中的不同状态。Java 线程状态包括：

![](<assets/1740015255390.png>)

1.  NEW：新创建的线程，尚未开始执行。
2.  RUNNABLE：运行状态，可以运行，也可能在等待 CPU 的时间片。
3.  BLOCKED：线程被阻塞，等待某个锁或者其他同步资源。
4.  WAITING：线程处于等待状态，等待其他线程执行某个操作或等待超时。
5.  TIMED_WAITING：线程处于有时限的等待状态，例如 Thread.sleep()。
6.  TERMINATED：线程已经执行完毕，退出线程生命周期。

![](<assets/1740015255691.png>)

说明：

​ 此图中的有执行资格和有执行权，是便于理解而加上的。在 Java 中，只有其余六种状态会被 Java 进行协调，当有执行资格和有执行权时，Java 将不在接管

### 25.9 线程栈

笔记小结：详细请查看本小节**概述**

#### 25.9.1 概述

​ 线程栈（Thread Stack）是操作系统分配给线程的一块内存区域，用于**存储线程的方法调用**和**局部变量**等**信息**。**每个线程**都**有自己的线程栈**，线程栈是**独立**的，**互**相之间**不**会**影响**。

#### 25.9.2 内存图

![](<assets/1740015255876.png>)

说明：

​ 每当创建一个线程时，都会在内存中创建各种的栈

#### 25.9.3 案例 - 抽奖池

1. 创建线程

```
public class MyThread extends Thread {

    ArrayList<Integer> list;

    public MyThread(ArrayList<Integer> list) {
        this.list = list;
    }

    @Override
    public void run() {
        ArrayList<Integer> boxList = new ArrayList<>();//1 //2
        while (true) {
            synchronized (MyThread.class) {
                if (list.size() == 0) {
                    System.out.println(getName() + boxList);
                    break;
                } else {
                    //继续抽奖
                    Collections.shuffle(list);
                    int prize = list.remove(0);
                    boxList.add(prize);
                }
            }
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        }
    }
}

```

2. 演示

```
public class Test {
    public static void main(String[] args) {
        /*
            有一个抽奖池,该抽奖池中存放了奖励的金额,该抽奖池中的奖项为 {10,5,20,50,100,200,500,800,2,80,300,700};
            创建两个抽奖箱(线程)设置线程名称分别为“抽奖箱1”，“抽奖箱2”
            随机从抽奖池中获取奖项元素并打印在控制台上,格式如下:
            每次抽的过程中，不打印，抽完时一次性打印(随机)    在此次抽奖过程中，抽奖箱1总共产生了6个奖项。
                分别为：10,20,100,500,2,300最高奖项为300元，总计额为932元
            在此次抽奖过程中，抽奖箱2总共产生了6个奖项。
                分别为：5,50,200,800,80,700最高奖项为800元，总计额为1835元
        */

        //创建奖池
        ArrayList<Integer> list = new ArrayList<>();
        Collections.addAll(list,10,5,20,50,100,200,500,800,2,80,300,700);

        //创建线程
        MyThread t1 = new MyThread(list);
        MyThread t2 = new MyThread(list);


        //设置名字
        t1.setName("抽奖箱1");
        t2.setName("抽奖箱2");


        //启动线程
        t1.start();
        t2.start();

    }
}

```

### 25.10 案例 - 抽奖池

1. 创建线程

```
public class MyCallable implements Callable<Integer> {

    ArrayList<Integer> list;

    public MyCallable(ArrayList<Integer> list) {
        this.list = list;
    }

    @Override
    public Integer call() throws Exception {
        ArrayList<Integer> boxList = new ArrayList<>();//1 //2
        while (true) {
            synchronized (MyCallable.class) {
                if (list.size() == 0) {
                    System.out.println(Thread.currentThread().getName() + boxList);
                    break;
                } else {
                    //继续抽奖
                    Collections.shuffle(list);
                    int prize = list.remove(0);
                    boxList.add(prize);
                }
            }
            Thread.sleep(10);
        }
        //把集合中的最大值返回
        if(boxList.size() == 0){
            return null;
        }else{
            return Collections.max(boxList);
        }
    }
}

```

说明：

*   此处使用了 Collections 工具类中的 shuffle(); 方法，来打乱集合中的数据
*   此处使用了 Collections 工具类中的 max 方法，来获取集合中的最大值

2. 演示

```
public class Test {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        /*
            有一个抽奖池,该抽奖池中存放了奖励的金额,该抽奖池中的奖项为 {10,5,20,50,100,200,500,800,2,80,300,700};
            创建两个抽奖箱(线程)设置线程名称分别为    "抽奖箱1", "抽奖箱2"
            随机从抽奖池中获取奖项元素并打印在控制台上,格式如下:

            在此次抽奖过程中，抽奖箱1总共产生了6个奖项，分别为：10,20,100,500,2,300
        	    最高奖项为300元，总计额为932元

            在此次抽奖过程中，抽奖箱2总共产生了6个奖项，分别为：5,50,200,800,80,700
            	最高奖项为800元，总计额为1835元

            在此次抽奖过程中,抽奖箱2中产生了最大奖项,该奖项金额为800元
            核心逻辑：获取线程抽奖的最大值（看成是线程运行的结果）


            以上打印效果只是数据模拟,实际代码运行的效果会有差异
        */

        //创建奖池
        ArrayList<Integer> list = new ArrayList<>();
        Collections.addAll(list,10,5,20,50,100,200,500,800,2,80,300,700);

        //创建多线程要运行的参数对象
        MyCallable mc = new MyCallable(list);

        //创建多线程运行结果的管理者对象
        //线程一
        FutureTask<Integer> ft1 = new FutureTask<>(mc);
        //线程二
        FutureTask<Integer> ft2 = new FutureTask<>(mc);

        //创建线程对象
        Thread t1 = new Thread(ft1);
        Thread t2 = new Thread(ft2);

        //设置名字
        t1.setName("抽奖箱1");
        t2.setName("抽奖箱2");

        //开启线程
        t1.start();
        t2.start();

        Integer max1 = ft1.get();
        Integer max2 = ft2.get();

        System.out.println(max1);
        System.out.println(max2);

        //在此次抽奖过程中,抽奖箱2中产生了最大奖项,该奖项金额为800元
        if(max1 == null){
            System.out.println("在此次抽奖过程中,抽奖箱2中产生了最大奖项,该奖项金额为"+max2+"元");
        }else if(max2 == null){
            System.out.println("在此次抽奖过程中,抽奖箱1中产生了最大奖项,该奖项金额为"+max1+"元");
        }else if(max1 > max2){
            System.out.println("在此次抽奖过程中,抽奖箱1中产生了最大奖项,该奖项金额为"+max1+"元");
        }else if(max1 < max2){
            System.out.println("在此次抽奖过程中,抽奖箱2中产生了最大奖项,该奖项金额为"+max2+"元");
        }else{
            System.out.println("两者的最大奖项是一样的");
        }
    }
}

```

### 25.11 线程池

笔记小结：

​ 线程池是一种实现**多线程的机制**。它可以在**应用程序启动时创建一定数量的线程**，等到任务处理完毕后，线程并**不被销毁**

```
	   // 1.创建一个默认的线程池对象.池子中默认是空的.默认最多可以容纳int类型的最大值.
     ExecutorService executorService = Executors.newCachedThreadPool();//Executors --- 可以帮助我们创建线程池对象
     // 2.ExecutorService --- 可以帮助我们控制线程池
     executorService.submit(()->{
         System.out.println(Thread.currentThread().getName() + "在执行了");
     });
     // 3.销毁线程池
     executorService.shutdown();

``` 

#### 25.11.1 概述

##### 25.11.1.1 含义

​ 在 Java 中，线程池是一种实现**多线程的机制**。它可以在**应用程序启动时创建一定数量的线程**，并将它们保存在线程池中，然后在需要处理任务时，从线程池中取出一个空闲线程来处理任务，任务处理完毕后，线程并不被销毁，而是返回线程池，等待下一个任务的到来

##### 25.11.1.2 主要核心原理

![](<assets/1740015256179.png>)

##### 25.11.1.3 优点

1.  提高系统性能：线程池可以根据系统资源的实际情况，自动控制线程的数量，能够更好地利用系统的 CPU 和内存资源，从而提高系统的性能。
2.  提高线程的复用性：线程池中的线程可以被重复使用，可以避免线程的创建和销毁带来的开销，从而提高线程的复用性。
3.  提高线程的可管理性：线程池中的线程可以统一管理，可以通过配置参数来控制线程的数量，防止线程数量过多而导致系统资源不足。
4.  提高程序的可靠性：线程池可以根据需要创建新线程，线程池中的线程可以自动恢复，避免因线程挂掉而导致整个程序崩溃的情况。

#### 25.11.2 基本用例

```
package com.itheima.mythreadpool;


//static ExecutorService newCachedThreadPool()   创建一个默认的线程池
//static newFixedThreadPool(int nThreads)	    创建一个指定最多线程数量的线程池

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class MyThreadPoolDemo {
    public static void main(String[] args) throws InterruptedException {

        //1,创建一个默认的线程池对象.池子中默认是空的.默认最多可以容纳int类型的最大值.
        ExecutorService executorService = Executors.newCachedThreadPool();
        //Executors --- 可以帮助我们创建线程池对象
        //ExecutorService --- 可以帮助我们控制线程池

        executorService.submit(()->{
            System.out.println(Thread.currentThread().getName() + "在执行了");
        });

        //Thread.sleep(2000);

        executorService.submit(()->{
            System.out.println(Thread.currentThread().getName() + "在执行了");
        });
		
        //销毁线程池
        executorService.shutdown();
    }
}

```

说明：

此处使用工具类 Executors 类的方法

*   获取对象的静态方法 newCachedThreadPool()
*   提交任务的成员方法 submit()
*   关闭线程池的方法 shudown()

### 25.12 自定义线程池

笔记小结：

自定义线程池对象：new `ThreadPoolExecutor`对象

```
ThreadPoolExecutor pool = new ThreadPoolExecutor(
 3,  //核心线程数量，能小于0
 6,  //最大线程数，不能小于0，最大数量 >= 核心线程数量
 60,//空闲线程最大存活时间
 TimeUnit.SECONDS,//时间单位
 new ArrayBlockingQueue<>(3),//任务队列
 Executors.defaultThreadFactory(),//创建线程工厂
 new ThreadPoolExecutor.AbortPolicy()//任务的拒绝策略
);

``` 

#### 25.12.1 概述

​ 在 Java 中，可以通过**自定义线程池对象**来管理线程池的线程数量、线程的执行方式等。自定义线程池对象的创建过程需要使用 `ThreadPoolExecutor` 类来实现

#### 25.12.2 构造方法

```
ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue, ThreadFactory threadFactory, RejectedExecutionHandler handler)

```

*   `corePoolSize`: 线程池中**维护的**线程数量，即使线程处于空闲状态也不会被销毁。
*   `maximumPoolSize`: 线程池中允许的**最大**线程数量，当线程池中的线程数量达到该值时，后续的任务会被阻塞。
*   `keepAliveTime`: 线程池中空闲线程的**存活时间**。
*   `unit`: `keepAliveTime` 参数的**时间单位**。
*   `workQueue`: 线程池中的**任务队列**。
*   `threadFactory`: 创建线程的**工厂对象**。
*   `handler`: **拒绝策略**，当任务队列已满且无法再接受新的任务时，如何处理新任务。

![](<assets/1740015256431.png>)

#### 25.12.3 基本用例

```
ThreadPoolExecutor pool = new ThreadPoolExecutor(
    3,  //核心线程数量，能小于0
    6,  //最大线程数，不能小于0，最大数量 >= 核心线程数量
    60,//空闲线程最大存活时间
    TimeUnit.SECONDS,//时间单位
    new ArrayBlockingQueue<>(3),//任务队列
    Executors.defaultThreadFactory(),//创建线程工厂
    new ThreadPoolExecutor.AbortPolicy()//任务的拒绝策略
);

```

#### 25.12.4 拒绝策略

![](<assets/1740015256668.png>)

### 25.13 线程常见问题

笔记小结：

*   最大并行数：最大并行数指的是**同时运行的线程**或**任务的最大数量**
*   线程池大小：根据部署机器的**硬件性能**，例如 CPU、内存等调整相应线程池的初始大小
*   扩展知识：本章不再拓展展开本小姐内容，日后补充！

#### 25.13.1 最大并行数

##### 25.13.1.1 概述

​ 最大并行数指的是**同时运行的线程**或**任务的最大数量**

##### 25.13.1.2 基本用例

```
public class MyThreadPoolDemo2 {
    public static void main(String[] args) {
        //向Java虚拟机返回可用处理器的数目
        int count = Runtime.getRuntime().availableProcessors();
        System.out.println(count);
    }
}

```

说明：

​ 获取当前允许环境的处理器的数目

#### 25.13.2 线程池大小

![](<assets/1740015256931.png>)

#### 25.13.3 线程池额外扩展知识

![](<assets/1740015257146.png>)

说明：

​ 本章不再拓展展开本小姐内容，日后补充！

### 25.13 工具类

笔记小结：

概述：多线程通常使用 **Executors 工具类**进行线程池的创建与使用

#### 25.13.1 概述

**Executors 类**

Executors: 线程池的工具类通过调用方法返回不同类型的线程池对象。

#### 25.13.2 构造方法

![](<assets/1740015257386.png>)

## 26. 网络编程

笔记小结：

1.  概述：
    *   定义：网络编程允许不**同的计算机之间**通过**网络**进行**数据传输和通信**，使得远程计算机之间能够相互交互和**共享信息**
    *   软件架构：B/S、C/S
    *   编程三要素：IP、端口、协议
2.  `InetAddress`类：
    *   概述：表示**网络地址**的**类**。它提供了一组方法，用于获取主机名和 IP 地址，并将域名解析为 IP 地址
    *   常用成员方法：getByName、getHostName、getHostAddress
3.  UDP 通信程序：
    *   概述：UDP 协议是一种**不可靠**的网络协议
    *   构造方法：`new DatagramSocket()`
    *   常用成员方法：send、receive、close、getData
    *   组播、广播实现：了解即可，当用到时再深入
4.  TCP 通信程序：
    *   概述：TCP 协议是一种**可靠的**网络协议，有**三次握手**和**四次挥手**的通信保障
    *   构造方法：`new Socket()`
    *   常用成员方法：accept、getInputStream、close

### 26.1 概述

#### 26.1.1 定义

​ 在 Java 中，网络编程指的是使用 Java 编程语言和相关的类库来实现网络通信的过程。网络编程允许不**同的计算机之间**通过**网络**进行**数据传输和通信**，使得远程计算机之间能够相互交互和共享信息。

​ 在网络通信协议下，不同计算机上运行的程序，可以进行数据传输

#### 26.1.2 软件架构

软件架构分为 B/S 和 C/S

![](<assets/1740015257775.png>)

BS/CS 架构的优缺点

![](<assets/1740015258166.png>)

#### 26.1.3 网络编程三要素

![](<assets/1740015258578.png>)

### 26.2InetAddress 类

#### 26.2.1 概述

​ InetAddress 是 Java 中用于表示网络地址的类。它提供了一组方法，用于获取主机名和 IP 地址，并将域名解析为 IP 地址

#### 26.2.2 常用成员方法

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>static InetAddress getByName(String host)</td><td>确定主机名称的 IP 地址。主机名称可以是机器名称，也可以是 IP 地址</td></tr><tr><td>String getHostName()</td><td>获取此 IP 地址的主机名</td></tr><tr><td>String getHostAddress()</td><td>返回文本显示中的 IP 地址字符串</td></tr></tbody></table>

#### 26.2.3 案例 - 获取基本信息

```
public class InetAddressDemo {
    public static void main(String[] args) throws UnknownHostException {
		//InetAddress address = InetAddress.getByName("itheima");
        InetAddress address = InetAddress.getByName("192.168.1.66");

        //public String getHostName()：获取此IP地址的主机名
        String name = address.getHostName();
        //public String getHostAddress()：返回文本显示中的IP地址字符串
        String ip = address.getHostAddress();

        System.out.println("主机名：" + name);
        System.out.println("IP地址：" + ip);
    }
}

```

### 26.3UDP 通信程序

#### 26.3.1 概述

##### 26.3.1.1 定义

​ UDP 协议是一种不可靠的网络协议，它在通信的两端各建立一个 Socket 对象，但是这两个 Socket 只是发送，接收数据的对象，因此对于基于 UDP 协议的通信双方而言，没有所谓的客户端和服务器的概念

​ Java 提供了 DatagramSocket 类作为基于 UDP 协议的 Socket

##### 26.3.1.2UDP 三种通讯方式

*   单播
    
    单播用于两个主机之间的端对端通信
    
*   组播
    
    组播用于对一组特定的主机进行通信
    
*   广播
    
    广播用于一个主机对整个局域网上所有主机上的数据通信
    

![](<assets/1740015258777.png>)

#### 26.3.2 构造方法

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>DatagramSocket()</td><td>创建数据报套接字并将其绑定到本机地址上的任何可用端口</td></tr><tr><td>DatagramPacket(byte[] buf, int len)</td><td>创建一个 DatagramPacket 用于接收长度为 len 的数据包</td></tr><tr><td>DatagramPacket(byte[] buf,int len,InetAddress add,int port)</td><td>创建数据包, 发送长度为 len 的数据包到指定主机的指定端口</td></tr></tbody></table>

细节：

*   空参构造：所有可用的端口中随机一个进行使用
*   有参构造：指定端口号进行绑定

#### 26.3.3 常用成员方法

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>void send(DatagramPacket p)</td><td>发送数据报包</td></tr><tr><td>void close()</td><td>关闭数据报套接字</td></tr><tr><td>void receive(DatagramPacket p)</td><td>从此套接字接受数据报包</td></tr><tr><td>byte[] getData()</td><td>返回数据缓冲区</td></tr><tr><td>int getLength()</td><td>返回要发送的数据的长度或接收的数据的长度</td></tr></tbody></table>

#### 26.3.4 基本用例 - 发送数据

步骤：

1.  创建发送端的 Socket 对象 (DatagramSocket)–创建快递公司
2.  创建数据，并把数据打包–创建包裹
3.  调用 DatagramSocket 对象的 send 方法发送数据–快递公司发送数据
4.  调用 DatagramSocket 对象的 close 方法释放资源–摧毁快递公司

```
public class SendMessageDemo {
    public static void main(String[] args) throws IOException {
        //发送数据

        //1.创建DatagramSocket对象(快递公司)
        //细节：
        //绑定端口，以后我们就是通过这个端口往外发送
        //空参：所有可用的端口中随机一个进行使用
        //有参：指定端口号进行绑定
        DatagramSocket ds = new DatagramSocket();

        //2.打包数据
        String str = "你好威啊！！！";
        byte[] bytes = str.getBytes();
        InetAddress address = InetAddress.getByName("127.0.0.1");
        int port = 10086;

        DatagramPacket dp = new DatagramPacket(bytes,bytes.length,address,port);

        //3.发送数据
        ds.send(dp);

        //4.释放资源
        ds.close();
    }
}

```

说明：

*   在创建 DatagramSocket 时，指定的端口号是发送方端口号
*   在创建 DatagramPacket 时，指定的端口号是接收方端口号

#### 26.3.5 基本用例 - 接收数据

步骤：

1.  创建接收端的 Socket 对象 (DatagramSocket)–创建快递公司
2.  创建一个数据包，用于接收数据–创建空包裹
3.  调用 DatagramSocket 对象的 receive 方法接收数据–快递公司接收数据
4.  解析数据包，并把数据在控制台显示–拆包裹
5.  调用 DatagramSocket 对象的 close 方法释放资源–摧毁快递公司

```
public class ReceiveMessageDemo {
    public static void main(String[] args) throws IOException {
        //接收数据

        //1.创建DatagramSocket对象（快递公司）
        //细节：
        //在接收的时候，一定要绑定端口
        //而且绑定的端口一定要跟发送的端口保持一致
        DatagramSocket ds = new DatagramSocket(10086);

        //2.接收数据包
        byte[] bytes = new byte[1024];
        DatagramPacket dp = new DatagramPacket(bytes,bytes.length);

        //该方法是阻塞的
        //程序执行到这一步的时候，会在这里死等
        //等发送端发送消息
        System.out.println(11111);
        ds.receive(dp);
        System.out.println(2222);

        //3.解析数据包
        byte[] data = dp.getData();
        int len = dp.getLength();
        InetAddress address = dp.getAddress();
        int port = dp.getPort();

        System.out.println("接收到数据" + new String(data,0,len));
        System.out.println("该数据是从" + address + "这台电脑中的" + port + "这个端口发出的");

        //4.释放资源
        ds.close();
    }
}

```

注意：

*   在创建 DatagramSocket 时，指定的端口号是接收方端口号，注意此端口号需要跟发送的端口号保持一致
*   在创建 DatagramPacket 时，无需指定 IP 地址或者端口号，只需要创建空包裹即可

#### 26.3.6 案例 - UDP 组播实现

##### 26.3.6.1 基本用例 - 发送数据

```
// 发送端
public class ClinetDemo {
    public static void main(String[] args) throws IOException {
        // 1. 创建发送端的Socket对象(DatagramSocket)
        DatagramSocket ds = new DatagramSocket();
        String s = "hello 组播";
        byte[] bytes = s.getBytes();
        InetAddress address = InetAddress.getByName("224.0.1.0");
        int port = 10000;
        // 2. 创建数据，并把数据打包(DatagramPacket)
        DatagramPacket dp = new DatagramPacket(bytes,bytes.length,address,port);
        // 3. 调用DatagramSocket对象的方法发送数据(在单播中,这里是发给指定IP的电脑但是在组播当中,这里是发给组播地址)
        ds.send(dp);
        // 4. 释放资源
        ds.close();
    }
}

```

细节：

​ 在发送数据时，创建的包裹指定 IP，需要填写指定组播的地址

##### 26.3.6.2 基本用例 - 接收数据

```
// 接收端
public class ServerDemo {
    public static void main(String[] args) throws IOException {
        // 1. 创建接收端Socket对象(MulticastSocket)
        MulticastSocket ms = new MulticastSocket(10000);
        // 2. 创建一个箱子,用于接收数据
        DatagramPacket dp = new DatagramPacket(new byte[1024],1024);
        // 3. 把当前计算机绑定一个组播地址,表示添加到这一组中.
        ms.joinGroup(InetAddress.getByName("224.0.1.0"));
        // 4. 将数据接收到箱子中
        ms.receive(dp);
        // 5. 解析数据包,并打印数据
        byte[] data = dp.getData();
        int length = dp.getLength();
        System.out.println(new String(data,0,length));
        // 6. 释放资源
        ms.close();
    }
}

```

细节：

​ 需要把当前计算机绑定一个组播地址，表示添加到这一组中

#### 26.3.7 案例 UDP 广播实现

##### 26.3.7.1 基本用例 - 发送数据

```
// 发送端
public class ClientDemo {
    public static void main(String[] args) throws IOException {
      	// 1. 创建发送端Socket对象(DatagramSocket)
        DatagramSocket ds = new DatagramSocket();
		// 2. 创建存储数据的箱子,将广播地址封装进去
        String s = "广播 hello";
        byte[] bytes = s.getBytes();
        InetAddress address = InetAddress.getByName("255.255.255.255");
        int port = 10000;
        DatagramPacket dp = new DatagramPacket(bytes,bytes.length,address,port);
		// 3. 发送数据
        ds.send(dp);
		// 4. 释放资源
        ds.close();
    }
}

```

细节：

​ 发送数据，发送 IP 指定为 255.255.255.255

##### 26.3.7.2 基本用例 - 接收数据

```
// 接收端
public class ServerDemo {
    public static void main(String[] args) throws IOException {
        // 1. 创建接收端的Socket对象(DatagramSocket)
        DatagramSocket ds = new DatagramSocket(10000);
        // 2. 创建一个数据包，用于接收数据
        DatagramPacket dp = new DatagramPacket(new byte[1024],1024);
        // 3. 调用DatagramSocket对象的方法接收数据
        ds.receive(dp);
        // 4. 解析数据包，并把数据在控制台显示
        byte[] data = dp.getData();
        int length = dp.getLength();
        System.out.println(new String(data,0,length));
        // 5. 关闭接收端
        ds.close();
    }
}

```

### 26.4TCP 通信程序

#### 26.4.1 概述

##### 26.4.1.1 定义

​ Java 中的 TCP 通信程序是一种基于 Socket 套接字的网络通信方式，其中客户端和服务器通过 TCP 协议进行数据的传输和交互

![](<assets/1740015259058.png>)

​ 在 Socket 进行发送与读取数据时，在底层会进行三次握手与四次挥手的协议，确保所传输的数据是能够被正常接收到

##### 26.4.1.2 三次握手

![](<assets/1740015259349.png>)

##### 26.4.1.3 四次挥手

![](<assets/1740015259601.png>)

#### 26.4.2 基本用例 - 发送数据

```
public class Client {
    public static void main(String[] args) throws IOException {
        //TCP协议，发送数据

        //1.创建Socket对象
        //细节：在创建对象的同时会连接服务端
        //      如果连接不上，代码会报错
        Socket socket = new Socket("127.0.0.1",10000);


        //2.可以从连接通道中获取输出流
        OutputStream os = socket.getOutputStream();
        //写出数据
        os.write("aaa".getBytes());

        //3.释放资源
        os.close();
        socket.close();
    }
}

```

#### 26.4.3 基本用例 - 接收数据

```
public class Server {
    public static void main(String[] args) throws IOException {
        //TCP协议，接收数据

        //1.创建对象ServerSocker
        ServerSocket ss = new ServerSocket(10000);

        //2.监听客户端的链接
        Socket socket = ss.accept();

        //3.从连接通道中获取输入流读取数据
        InputStream is = socket.getInputStream();
        // InputStreamReader isr = new InputStreamReader(is);
        // BufferedReader br = new BufferedReader(isr); // 将字符输入流用缓冲字符输入流进行读取，可加快读取的速度
        int b;
        while ((b = is.read()) != -1){
            System.out.println((char) b);
        }

        //4.释放资源
        socket.close();
        ss.close();

    }
}


```

细节：

​ 在接收中文数据时，会产生中文乱码，是由于 IDEA 平台默认使用 UTF-8 的编码方式。InputStream 在读取字节流时默认是用一个字节一个字节的读，而不能根据编码表的方式进行读取。因此，使用字符流可以根据编码进行读取，解决中文乱码。

## 27. 反射

笔记小结：

1.  概述：反射允许**对**封装**类**的**字段**，**方法**和**构造函数**的信息**进行编程访问**。
2.  作用：可以结合配置文件，**动态的创建对象**并调用方法，就像 Java 设计模式中的工程模式创建就会运用
3.  获取构造方法：getDeclaredConstructors（）、getConstructors（）
4.  获取成员变量：getDeclaredFields（）、getFields（）
5.  获取成员方法：getDeclaredMethods（）、getMethods（）

### 27.1 概述

#### 27.1.1 定义

​ 反射允许对封装类的字段，方法和构造函数的信息进行编程访问

![](<assets/1740015259841.png>)

#### 27.1.2 作用

*   获取一个类里面所有的信息，获取到了之后，再执行其他的业务逻辑
*   结合配置文件，动态的创建对象并调用方法

### 27.2 获取字节码方式

#### 27.2.1 概述

三种方式：

*   Class 这个类里面的静态方法 forName（“全类名”）**（最常用）**
*   通过 class 属性获取
*   通过对象获取字节码文件对象

![](<assets/1740015260160.png>)

#### 27.2.2 基本用例 - 获取字节码

```
//1. 第一种方式
//全类名 ： 包名 + 类名
//最为常用的
Class clazz1 = Class.forName("com.itheima.myreflect1.Student");

//2. 第二种方式
//一般更多的是当做参数进行传递
Class clazz2 = Student.class;


//3.第三种方式
//当我们已经有了这个类的对象时，才可以使用。
Student s = new Student();
Class clazz3 = s.getClass();

```

说明：

第一种方式 Class.forName(“xxx”) 的方式是常用方式

### 27.3 获取构造方法

#### 27.3.1 概述

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>Constructor&lt;?&gt;[] getConstructors()</td><td>获得所有的构造（只能 public 修饰）</td></tr><tr><td>Constructor&lt;?&gt;[] getDeclaredConstructors()</td><td>获得所有的构造（包含 private 修饰）</td></tr><tr><td>Constructor getConstructor(Class&lt;?&gt;… parameterTypes)</td><td>获取指定构造（只能 public 修饰）</td></tr><tr><td>Constructor getDeclaredConstructor(Class&lt;?&gt;… parameterTypes)</td><td>获取指定构造（包含 private 修饰</td></tr></tbody></table>

说明：

​ 获取构造方法名称解析，get 获取，Constructors 表示构造方法，Declared 表示所有权限，s 表示多个

常用成员方法

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td><code onclick="mdcp.copyCode(event)">getModifiers</code></td><td>获取修饰符。返回表示该成员或构造函数的 Java 语言修饰符的整数。可以使用 <code onclick="mdcp.copyCode(event)">Modifier</code> 类来解码这个整数中包含的修饰符。</td></tr><tr><td><code onclick="mdcp.copyCode(event)">getParameters</code></td><td>获取此方法的形式参数。返回一个包含 <code onclick="mdcp.copyCode(event)">Parameter</code> 对象的数组，每个 <code onclick="mdcp.copyCode(event)">Parameter</code> 对象描述一个参数。如果该方法没有参数，则返回长度为 0 的数组。</td></tr><tr><td><code onclick="mdcp.copyCode(event)">setAccessible</code></td><td>将此对象的可访问标志设置为指示的布尔值。值为 true 表示反射对象在使用时应该取消 Java 语言访问检查。</td></tr></tbody></table>

#### 27.3.2 基本用例 - 获取构造方法

```
//1.获得整体（class字节码文件对象）
Class clazz = Class.forName("com.itheima.reflectdemo.Student");

//2.获取构造方法对象

//2.1获取所有构造方法（public）
Constructor[] constructors1 = clazz.getConstructors();

System.out.println("=======================");

//2.2获取所有构造（带私有的）
Constructor[] constructors2 = clazz.getDeclaredConstructors();

System.out.println("=======================");

//2.3获取指定的空参构造
Constructor con1 = clazz.getConstructor();
Constructor con2 = clazz.getConstructor(String.class,int.class);

System.out.println("=======================");

//2.4获取指定的构造(所有构造都可以获取到，包括public包括private)
Constructor con3 = clazz.getDeclaredConstructor();

//3.获取构造方法的权限修饰符
int modifiers = con3.getModifiers();

//4.获取构造方法的参数信息
Parameter[] parameters = con4.getParameters();

//5.暴力反射
con4.setAccessible(true);
Student stu = (Student) con4.newInstance("张三", 23);

```

说明：

​ 当通过字节码文件，获取到 Java 中的构造方法后，可以更深入的获取构造方法的详细信息

### 27.4 获取成员变量

#### 27.4.1 概述

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>Field[] getFields()</td><td>返回所有成员变量对象的数组（只能拿 public 的）</td></tr><tr><td>Field[] getDeclaredFields()</td><td>返回所有成员变量对象的数组，存在就能拿到</td></tr><tr><td>Field getField(String name)</td><td>返回单个成员变量对象（只能拿 public 的）</td></tr><tr><td>Field getDeclaredField(String name)</td><td>返回单个成员变量对象，存在就能拿到</td></tr></tbody></table>

说明：

​ 获取成员变量解析，get 获取，Constructors 表示构造方法，Declared 表示所有权限，s 表示多个

<table><thead><tr><th>方法名称</th><th>说明</th></tr></thead><tbody><tr><td>getModifiers</td><td>返回该方法的修饰符，如 public、static、final 等</td></tr><tr><td>getName</td><td>返回该方法的名称</td></tr><tr><td>getType</td><td>返回该方法参数类型的 Class 对象的数组</td></tr><tr><td>setAccessible</td><td>设置是否允许访问该方法，若设置为 true，则可访问 private 修饰符的方法。</td></tr></tbody></table>

#### 27.4.2 基本用例 - 获取成员变量

```
//1.获取class字节码文件的对象
Class clazz = Class.forName("com.itheima.myreflect3.Student");

//2.获取成员变量
//2.1获取所有的成员变量
Field[] fields = clazz.getDeclaredFields();

//2.2获取单个的成员变量
Field name = clazz.getDeclaredField("name");
System.out.println(name);

//2.3获取权限修饰符
int modifiers = name.getModifiers();
System.out.println(modifiers);

//2.4获取成员变量的名字
String n = name.getName();
System.out.println(n);

//2.5获取成员变量的数据类型
Class<?> type = name.getType();
System.out.println(type);

//3.获取成员变量记录的值
Student s = new Student("zhangsan",23,"男");
name.setAccessible(true);
String value = (String) name.get(s);
System.out.println(value);

//4.修改对象里面记录的值
name.set(s,"lisi");
System.out.println(s);

```

说明：

​ 可对获取到的成员变量进行更细一步的操作，例如获取变量修饰符、变量名、数据类型、变量值，修改变量值等

### 27.5 获取成员方法

#### 27.5.1 概述

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>Method[] getMethods()</td><td>返回所有成员方法对象的数组（只能拿 public 的）</td></tr><tr><td>Method[] getDeclaredMethods()</td><td>返回所有成员方法对象的数组，存在就能拿到</td></tr><tr><td>Method getMethod(String name, Class&lt;?&gt;… parameterTypes)</td><td>返回单个成员方法对象（只能拿 public 的）</td></tr><tr><td>Method getDeclaredMethod(String name, Class&lt;?&gt;… parameterTypes)</td><td>返回单个成员方法对象，存在就能拿到</td></tr></tbody></table>

```
说明：

```

​ 获取成员方法解析，get 获取，Constructors 表示构造方法，Declared 表示所有权限，s 表示多个

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>getModifiers</td><td>返回当前方法的修饰符的整数表示，修饰符包括 public、private、protected、static、final 等等</td></tr><tr><td>getName</td><td>返回当前方法的名称</td></tr><tr><td>getParameters</td><td>返回当前方法的参数列表，以 Parameter 数组形式返回</td></tr><tr><td>getExceptionTypes</td><td>返回当前方法抛出的异常类型数组</td></tr><tr><td>invoke</td><td>通过反射调用当前方法</td></tr></tbody></table>

#### 27.5.2 基本用例 - 获取成员方法

```
//1. 获取class字节码文件对象
Class clazz = Class.forName("com.itheima.myreflect4.Student");

//2.获取方法对象
//2.1获取里面所有的方法对象(包含父类中所有的公共方法)
Method[] methods = clazz.getMethods();

//2.2获取里面所有的方法对象(不能获取父类的，但是可以获取本类中私有的方法)
Method[] methods = clazz.getDeclaredMethods();

//2.3获取指定的单一方法
Method m = clazz.getDeclaredMethod("eat", String.class);
System.out.println(m);

//3.获取方法的修饰符
int modifiers = m.getModifiers();
System.out.println(modifiers);

//4.获取方法的名字
String name = m.getName();
System.out.println(name);

//5.获取方法的形参
Parameter[] parameters = m.getParameters();

//6.获取方法的抛出的异常
Class[] exceptionTypes = m.getExceptionTypes();


//7.方法运行
/*Method类中用于创建对象的方法
        Object invoke(Object obj, Object... args)：运行方法
        参数一：用obj对象调用该方法
        参数二：调用方法的传递的参数（如果没有就不写）
        返回值：方法的返回值（如果没有就不写）*/

Student s = new Student();
m.setAccessible(true);
//参数一s：表示方法的调用者
//参数二"汉堡包"：表示在调用方法的时候传递的实际参数
String result = (String) m.invoke(s, "汉堡包");
System.out.println(result);

```

## 28. 动态代理

笔记小结：

1.  概述：动态代理是一种机制，它允许在运行时创建一个代理对象，**代理对象能够拦截并处理被代理对象的方法调用**。跟 IP 的代理不一样，动态代理是用于增强被代理对象的功能
2.  作用：代理可以**无侵入式**的给对象**增强**其他的**功能**
3.  基本用例：使用`Proxy`类提供的`newProxyInstance`方法，进行动态的代理

### 28.1 概述

#### 28.1.1 定义

​ 在 Java 中，动态代理是一种机制，它允许在运行时创建一个代理对象，代理对象能够拦截并处理被代理对象的方法调用

#### 28.1.2 作用

​ 代理可以无侵入式的给对象增强其他的功能

![](<assets/1740015260360.png>)

补充：

​ 通过接口保证，后面的对象和代理需要实现同一个接口，接口中就是被代理的所有方法

### 28.2 基本用例

步骤一：创建接口

说明：

​ 我们可以把所有想要被代理的方法定义在接口当中

```
public interface Star {

    //唱歌
    public abstract String sing(String name);

    //跳舞
    public abstract void dance();

}

```

说明：

​ 此接口，需要各个被**代理对象**与**代理**，**共同实现**。目的是让代理能够调用被代理对象内的方法

步骤二：创建被代理对象

*   创建实体类

```
public class BigStar implements Star {
    private String name;


    public BigStar() {
    }

    public BigStar(String name) {
        this.name = name;
    }

    //唱歌
    @Override
    public String sing(String name){
        System.out.println(this.name + "正在唱" + name);
        return "谢谢";
    }

    //跳舞
    @Override
    public void dance(){
        System.out.println(this.name + "正在跳舞");
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

    public String toString() {
        return "BigStar{name = " + name + "}";
    }
}

```

步骤三：创建代理

说明：

​ 给一个明星的对象，创建一个代理

```
/*
* 类的作用：
*       创建一个代理
* */
public class ProxyUtil {

    /*
    * 方法的作用：
    *       给一个明星的对象，创建一个代理
    *  形参：
    *       被代理的明星对象
    *  返回值：
    *       给明星创建的代理
    * 需求：
    *   外面的人想要大明星唱一首歌
    *   1. 获取代理的对象
    *      代理对象 = ProxyUtil.createProxy(大明星的对象);
    *   2. 再调用代理的唱歌方法
    *      代理对象.唱歌的方法("只因你太美");
    * */
    public static Star createProxy(BigStar bigStar){
        /* java.lang.reflect.Proxy类：提供了为对象产生代理对象的方法：

        public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)
        参数一：用于指定用哪个类加载器，去加载生成的代理类
        参数二：指定接口，这些接口用于指定生成的代理长什么，也就是有哪些方法
        参数三：用来指定生成的代理对象要干什么事情*/
        Star star = (Star) Proxy.newProxyInstance(
            ProxyUtil.class.getClassLoader(),//参数一：用于指定用哪个类加载器，去加载生成的代理类
            new Class[]{Star.class},//参数二：指定接口，这些接口用于指定生成的代理长什么，也就是有哪些方法
            //参数三：用来指定生成的代理对象要干什么事情
            new InvocationHandler() {
                @Override
                public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                    /*
                        * 参数一：代理的对象
                        * 参数二：要运行的方法 sing
                        * 参数三：调用sing方法时，传递的实参
                        * */
                    if("sing".equals(method.getName())){
                        System.out.println("准备话筒，收钱");
                    }else if("dance".equals(method.getName())){
                        System.out.println("准备场地，收钱");
                    }
                    //去找大明星开始唱歌或者跳舞
                    //代码的表现形式：调用大明星里面唱歌或者跳舞的方法
                    return method.invoke(bigStar,args);
                }
            }
        );
        return star;
    }
}


```

说明：

*   此代理对象，需要用到 Proxy 类中的 newProxyInstance 方法

```
public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)

``` 

步骤四：演示

```
public class Test {
    public static void main(String[] args) {
        
    /*
        需求：
            外面的人想要大明星唱一首歌
             1. 获取代理的对象
                代理对象 = ProxyUtil.createProxy(大明星的对象);
             2. 再调用代理的唱歌方法
                代理对象.唱歌的方法("只因你太美");
     */


        //1. 获取代理的对象
        BigStar bigStar = new BigStar("鸡哥");
        Star proxy = ProxyUtil.createProxy(bigStar);

        //2. 调用唱歌的方法
        String result = proxy.sing("只因你太美");
        System.out.println(result);

    }
}

```

## 29. 日志

笔记小结：

1.  概述：日志技术，可以便于记录重要的信息，将这些详细信息保存到文件和数据库中
2.  日志级别：TRACE < DEBUG < INFO < WARN < ERROR
3.  补充：@Sf4j 注解也可以输出到本地，详细请参考 [springboot 项目使用 slf4j 控制台输出日志 + 输出日志文件到本地_org.slf4j.loggerfactory 日志路径_余额一个亿的博客 - CSDN 博客](https://blog.csdn.net/MengDiL_yl/article/details/86648197)

### 29.1 概述

#### 29.1.1 定义

​ 在 Java 中，日志在合适的时候加入，用于记录重要的信息。便于运维进行管理

#### 29.1.2 作用

*   跟输出语句一样，可以把程序在运行过程中的详细信息都打印在控制台上
*   利用 log 日志还可以把这些详细信息保存到文件和数据库中

![](<assets/1740015260674.png>)

#### 29.1.3 日志级别

```
TRACE, DEBUG, INFO, WARN, ERROR

```

日志级别从小到大的关系：

​ TRACE < DEBUG < INFO < WARN < ERROR

#### 29.1.4 体系结构

![](<assets/1740015260919.png>)

*   日志规范: 一些接口，提供给日志的实现框架设计的标准
*   日志框架: 牛人或者第三方公司已经做好的日志记录实现代码，后来者直接可以拿去使用

### 29.2Logback 日志框架

#### 29.2.1 概述

Logback 是基于 slf4j 的日志规范实现的框架，性能比之前使用的 log4j 要好

说明：

​ 官方网站: https://logback.qos.ch/index.html

#### 29.2.2 模块

Logback 主要分为三个技术模块:

*   logback-core: 该模块为其他两个模块提供基础代码，必须有。
*   logback-classic: 完整实现了 slf4j API 的模块。
*   logback-access 模块与 Tomcat 和 Jetty 等 Servlet 容器集成，以提供 HTTP 访问日志功能

### 29.3Logback 基本用例

步骤一：导入 jar 包，并添加依赖库

![](<assets/1740015261190.png>)

步骤二：将 Logback 的核心配置文件 logback.xml 直接拷贝到 src 目录下（必须是 src 下)

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!--
        CONSOLE ：表示当前的日志信息是可以输出到控制台的。
    -->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <!--输出流对象 默认 System.out 改为 System.err-->
        <target>System.out</target>
        <encoder>
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度
                %msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%-5level]  %c [%thread] : %msg%n</pattern>
        </encoder>
    </appender>

    <!-- File是输出的方向通向文件的 -->
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
            <charset>utf-8</charset>
        </encoder>
        <!--日志输出路径-->
        <file>C:/code/itheima-data.log</file>
        <!--指定日志文件拆分和压缩规则-->
        <rollingPolicy
                       class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <!--通过指定压缩文件名称，来确定分割文件方式-->
            <fileNamePattern>C:/code/itheima-data2-%d{yyyy-MMdd}.log%i.gz</fileNamePattern>
            <!--文件拆分大小-->
            <maxFileSize>1MB</maxFileSize>
        </rollingPolicy>
    </appender>

    <!--

    level:用来设置打印级别，大小写无关：TRACE, DEBUG, INFO, WARN, ERROR, ALL 和 OFF
   ， 默认debug
    <root>可以包含零个或多个<appender-ref>元素，标识这个输出位置将会被本日志级别控制。
    -->
    <root level="info">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE" />
    </root>
</configuration>

```

步骤三：在代码中获取日志的对象

```
public static final Logger LOGGER= LoggerFactory.getLdgger("类对象");

```

步骤四：使用日志对象的方法记录系统的日志信息

### 29.4Logback 配置文件

**Logback 日志输出位置、格式设置:**

*   通过 logback.xml 中的标签可以设置输出位置和日志信息的详细格式。
*   通常可以设置 2 个日志输定位置: 一个是控制台、一个是系统文件中

输出到控制台的配置标志：

```
<appender name="CONSOLE" class="ch.qos.logback.core.consoleAppender">

```

输出到系统文件的配置标志：

```
<appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender"">

```

## 30. 注解

笔记小结：

1.  概述：在 Java 中，注解（Annotation）是一种**元数据机制**，它允许在源代码、编译时以及运行时为程序元素（类、方法、字段等）添加信息、**标记和说明**
    
2.  常用注解：@Override：表示**方法的重写**、@Deprecated：表示修饰的**方法已过时**、@SuppressWarnings(“all”)：压**制警告**
    
3.  自定义注解：
    
    说明：自己编写注解，通常来说与反射一并使用。
    
    ```
    // 定义格式
    public @interface 注解名称{
    	public 属性类型 属性名() default 默认值;
    }
    
    ```
    
4.  元注解：
    
    *   @Retention：用来标识**注解的生命周期** (有效范围)
    *   @Target：用来标识**注解使用的位置**

### 30.1 概述

#### 30.1.1 定义

​ 在 Java 中，注解（Annotation）是一种**元数据机制**，它允许在源代码、编译时以及运行时为程序元素（类、方法、字段等）添加信息、**标记和说明**。注解可以帮助开发人员在代码中添加关键信息，以便在后续的编译、构建、部署和运行过程中使用

```
@RestController
@RequestMapping("order")
public @interface MyAnnotation {
    String value();
    int priority() default 0;
}

```

说明：

​ 以`@`开头的代码行即为注解

#### 30.1.2 作用

​ Java 的注解通过 **`@`符号 ** 来表示，紧接着是注解的名称。注解可以附加到类、方法、字段、参数等程序元素上，以提供额外的元数据。注解的元数据信息可以被编译器、工具和运行时框架使用，用于自动生成代码、配置应用程序行为，以及实现一些特定的功能

#### 30.1.3 注释和注解的区别

*   共同点：都可以对程序进行解释说明
    
*   不同点：注释，是给程序员看的。只在 Java 中有效。在 **class 文件中不存在注释**的
    

说明：

*   当编译之后，会进行注释擦除
*   注解，是给虚拟机看的。当虚拟机看到注解之后，就知道要做什么事情了

### 30.2 基本用例

说明：

​ 注解的一般使用

步骤一：认识注解

```
@Override

```

说明：

​ 明白这个注解的作用。在以前看过注解 @Override，当子类重写父类方法的时候，在重写的方法上面写 @Override。当虚拟机看到 @Override 的时候，就知道下面的方法是重写的父类的。检查语法，如果语法正确编译正常，如果语法错误，就会报错。

步骤二：添加注解

```
@Override
public PageResult search(GlobalSearchDTO requestParams) {
    ……
}

```

说明：

注解通常添加在类中的方法上

补充：

​ 也有添加在`@interface`注解之上的，这个叫源注解

### 30.3 常用注解

JVM 提供的注解：

*   @Override：表示方法的重写
    
*   @Deprecated：表示修饰的方法已过时
    
*   @SuppressWarnings(“all”)：压制警告
    

第三方框架注解

*   Junit:
    *   @Test 表示运行测试方法
    *   @Before 表示在 Test 之前运行，进行数据的初始化
    *   @After 表示在 Test 之后运行，进行数据的还原

### 30.4 自定义注解

#### 30.4.1 概述

​ 自定义注解就是自己做一个注解来使用。自定义注解单独存在是没有什么意义的，一般会跟反射**结合**起来**使用**，会用发射去解析注解。

注意：

​ JVM 不会对自定义的注解添加**任何逻辑**，怎样处理完全由 Java 代码决定。也就是说光定义了注解是不足以使用的，需要通过反射等手法来实现实现注解的逻辑

#### 30.4.2 基本用例

说明：

*   定义注解格式

```
public @interface 注解名称{
	public 属性类型 属性名() default 默认值;
}

```

*   属性类型可以为：基本数据类型、String、Class、注解等
*   使用注解格式

```
@注解名称(属性名1 = 值1，属性名2 = 值2)

```

*   使用自定义注解时要保证注解每个属性都有值。注解可以使用默认值

步骤一：定义注解

```
public @interface Annotation {
    public abstract String value();
}

```

步骤二：使用注解

```
@Annotation(value = "")
private int myField;

```

说明：

*   value 属性，如果只有一个 value 属性的情况下，使用 value 属性的时候**可以省略** value 名称不写
*   但是加里右多个属性日多个属性沿右默认值，那么 value 名称是**不能省略**的

步骤三：获取注解

```
public class MyClass implements MyInterface {
    @Annotation("My Class")
    private int myField;

    @Override
    public void myMethod() {}

    public static void main(String[] args) throws NoSuchFieldException, NoSuchMethodException {
		// 1.获取成员变量
        Field field = MyClass.class.getDeclaredField("myField");
        // 2.获取成员变量上的注解
        Annotation annotation = field.getAnnotation(Annotation.class);
        // 3.获取该成员变量注解上的值
        System.out.println(annotation.value()); 
    }
}

```

补充：获取方法上的注解

```
Method method = MyClass.class.getMethod("myMethod");
MyAnnotation annotation = method.getAnnotation(MyAnnotation.class);
System.out.println(annotation.value()); 

``` 

### 30.5 元注解

#### 30.5.1 概述

​ 在 Java 中，元注解（Meta-annotation）是用于**注解其他注解的注解**，也就是说，元注解用于对其他注解进行注解。元注解为开发者提供了**定义和控制注解行为**的能力，可以影响注解在代码中的使用和解释方式

​ 简单的说，就是写在注解上的注解

#### 30.5.2 分类

1.  **@Retention**：用于指定注解的保留策略，即注解在什么**级别**保存（源代码、类文件、运行时）。
2.  **@Target**：用于指定注解可以应用的**目标元素类型**，如类、方法、字段等。
3.  **@Documented**：用于指定注解是否**包含**在 Java 文档中。
4.  **@Inherited**：用于指定注解是否被子类**继承**。

#### 30.5.3@Target

​ 用来标识**注解使用的位置**，如果没有使用该注解标识，则自定义的注解可以使用在任意位置。

​ 可使用的值定义在 ElementType 枚举类中，常用值如下

*   TYPE，类，接口
*   FIELD, 成员变量
*   METHOD, 成员方法
*   PARAMETER, 方法参数
*   CONSTRUCTOR, 构造方法
*   LOCAL_VARIABLE, 局部变量

#### 30.5.4@Retention

​ 用来标识**注解的生命周期** (有效范围)

​ 可使用的值定义在 RetentionPolicy 枚举类中，常用值如下

*   SOURCE：注解只作用在源码阶段，生成的字节码文件中不存在
*   CLASS：注解作用在源码阶段，字节码文件阶段，运行阶段不存在，默认值
*   RUNTIME：注解作用在源码阶段，字节码文件阶段，运行阶段

## 30. 算法（目前了解，日后补充）

​ 数据结构是数据存储的方式，算法是数据计算的方式。所以在开发中，算法和数据结构息息相关。今天的讲义中会涉及部分数据结构的专业名词，如果各位铁粉有疑惑，可以先看一下哥们后面录制的数据结构，再回头看算法。

### 30.1 查找算法

#### 30.1.1. 基本查找

​ 也叫做顺序查找

​ 说明：顺序查找适合于存储结构为数组或者链表。

**基本思想**：顺序查找也称为线形查找，属于无序查找算法。从数据结构线的一端开始，顺序扫描，依次将遍历到的结点与要查找的值相比较，若相等则表示查找成功；若遍历结束仍没有找到相同的，表示查找失败。

示例代码：

```
public class A01_BasicSearchDemo1 {
    public static void main(String[] args) {
        //基本查找/顺序查找
        //核心：
        //从0索引开始挨个往后查找

        //需求：定义一个方法利用基本查找，查询某个元素是否存在
        //数据如下：{131, 127, 147, 81, 103, 23, 7, 79}


        int[] arr = {131, 127, 147, 81, 103, 23, 7, 79};
        int number = 82;
        System.out.println(basicSearch(arr, number));

    }

    //参数：
    //一：数组
    //二：要查找的元素

    //返回值：
    //元素是否存在
    public static boolean basicSearch(int[] arr, int number){
        //利用基本查找来查找number在数组中是否存在
        for (int i = 0; i < arr.length; i++) {
            if(arr[i] == number){
                return true;
            }
        }
        return false;
    }
}

```

#### 30.1.2. 二分查找

​ 也叫做折半查找

说明：元素必须是有序的，从小到大，或者从大到小都是可以的。

如果是无序的，也可以先进行排序。但是排序之后，会改变原有数据的顺序，查找出来元素位置跟原来的元素可能是不一样的，所以排序之后再查找只能判断当前数据是否在容器当中，返回的索引无实际的意义。

**基本思想**：也称为是折半查找，属于有序查找算法。用给定值先与中间结点比较。比较完之后有三种情况：

*   相等
    
    说明找到了
    
*   要查找的数据比中间节点小
    
    说明要查找的数字在中间节点左边
    
*   要查找的数据比中间节点大
    
    说明要查找的数字在中间节点右边
    

代码示例：

```
package com.itheima.search;

public class A02_BinarySearchDemo1 {
    public static void main(String[] args) {
        //二分查找/折半查找
        //核心：
        //每次排除一半的查找范围

        //需求：定义一个方法利用二分查找，查询某个元素在数组中的索引
        //数据如下：{7, 23, 79, 81, 103, 127, 131, 147}

        int[] arr = {7, 23, 79, 81, 103, 127, 131, 147};
        System.out.println(binarySearch(arr, 150));
    }

    public static int binarySearch(int[] arr, int number){
        //1.定义两个变量记录要查找的范围
        int min = 0;
        int max = arr.length - 1;

        //2.利用循环不断的去找要查找的数据
        while(true){
            if(min > max){
                return -1;
            }
            //3.找到min和max的中间位置
            int mid = (min + max) / 2;
            //4.拿着mid指向的元素跟要查找的元素进行比较
            if(arr[mid] > number){
                //4.1 number在mid的左边
                //min不变，max = mid - 1；
                max = mid - 1;
            }else if(arr[mid] < number){
                //4.2 number在mid的右边
                //max不变，min = mid + 1;
                min = mid + 1;
            }else{
                //4.3 number跟mid指向的元素一样
                //找到了
                return mid;
            }

        }
    }
}

```

#### 30.1.3. 插值查找

在介绍插值查找之前，先考虑一个问题：

​ 为什么二分查找算法一定要是折半，而不是折四分之一或者折更多呢？

其实就是因为方便，简单，但是如果我能在二分查找的基础上，让中间的 mid 点，尽可能靠近想要查找的元素，那不就能提高查找的效率了吗？

二分查找中查找点计算如下：

mid=(low+high)/2, 即 mid=low+1/2*(high-low);

我们可以将查找的点改进为如下：

mid=low+(key-a[low])/(a[high]-a[low])*(high-low)，

这样，让 mid 值的变化更靠近关键字 key，这样也就间接地减少了比较次数。

基本思想：基于二分查找算法，将查找点的选择改进为自适应选择，可以提高查找效率。当然，差值查找也属于有序查找。

** 细节：** 对于表长较大，而关键字分布又比较均匀的查找表来说，插值查找算法的平均性能比折半查找要好的多。反之，数组中如果分布非常不均匀，那么插值查找未必是很合适的选择。

代码跟二分查找类似，只要修改一下 mid 的计算方式即可。

#### 30.1.4. 斐波那契查找

在介绍斐波那契查找算法之前，我们先介绍一下很它紧密相连并且大家都熟知的一个概念——黄金分割。

黄金比例又称黄金分割，是指事物各部分间一定的数学比例关系，即将整体一分为二，较大部分与较小部分之比等于整体与较大部分之比，其比值约为 1:0.619 或 1.619:1。

0.619 被公认为最具有审美意义的比例数字，这个数值的作用不仅仅体现在诸如绘画、雕塑、音乐、建筑等艺术领域，而且在管理、工程设计等方面也有着不可忽视的作用。因此被称为黄金分割。

在数学中有一个非常有名的数学规律：斐波那契数列：1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89…….

（从第三个数开始，后边每一个数都是前两个数的和）。

然后我们会发现，随着斐波那契数列的递增，前后两个数的比值会越来越接近 0.619，利用这个特性，我们就可以将黄金比例运用到查找技术中。

![](<assets/1740015261475.png>)

基本思想：也是二分查找的一种提升算法，通过运用黄金比例的概念在数列中选择查找点进行查找，提高查找效率。同样地，斐波那契查找也属于一种有序查找算法。

斐波那契查找也是在二分查找的基础上进行了优化，优化中间点 mid 的计算方式即可

代码示例：

```
public class FeiBoSearchDemo {
    public static int maxSize = 20;

    public static void main(String[] args) {
        int[] arr = {1, 8, 10, 89, 1000, 1234};
        System.out.println(search(arr, 1234));
    }

    public static int[] getFeiBo() {
        int[] arr = new int[maxSize];
        arr[0] = 1;
        arr[1] = 1;
        for (int i = 2; i < maxSize; i++) {
            arr[i] = arr[i - 1] + arr[i - 2];
        }
        return arr;
    }

    public static int search(int[] arr, int key) {
        int low = 0;
        int high = arr.length - 1;
        //表示斐波那契数分割数的下标值
        int index = 0;
        int mid = 0;
        //调用斐波那契数列
        int[] f = getFeiBo();
        //获取斐波那契分割数值的下标
        while (high > (f[index] - 1)) {
            index++;
        }
        //因为f[k]值可能大于a的长度，因此需要使用Arrays工具类，构造一个新法数组，并指向temp[],不足的部分会使用0补齐
        int[] temp = Arrays.copyOf(arr, f[index]);
        //实际需要使用arr数组的最后一个数来填充不足的部分
        for (int i = high + 1; i < temp.length; i++) {
            temp[i] = arr[high];
        }
        //使用while循环处理，找到key值
        while (low <= high) {
            mid = low + f[index - 1] - 1;
            if (key < temp[mid]) {//向数组的前面部分进行查找
                high = mid - 1;
                /*
                  对k--进行理解
                  1.全部元素=前面的元素+后面的元素
                  2.f[k]=k[k-1]+f[k-2]
                  因为前面有k-1个元素没所以可以继续分为f[k-1]=f[k-2]+f[k-3]
                  即在f[k-1]的前面继续查找k--
                  即下次循环,mid=f[k-1-1]-1
                 */
                index--;
            } else if (key > temp[mid]) {//向数组的后面的部分进行查找
                low = mid + 1;
                index -= 2;
            } else {//找到了
                //需要确定返回的是哪个下标
                if (mid <= high) {
                    return mid;
                } else {
                    return high;
                }
            }
        }
        return -1;
    }
}


```

#### 30.1.5. 分块查找

当数据表中的数据元素很多时，可以采用分块查找。

汲取了顺序查找和折半查找各自的优点，既有动态结构，又适于快速查找

分块查找适用于数据较多，但是数据不会发生变化的情况，如果需要一边添加一边查找，建议使用哈希查找

分块查找的过程：

1.  需要把数据分成 N 多小块，块与块之间不能有数据重复的交集。
2.  给每一块创建对象单独存储到数组当中
3.  查找数据的时候，先在数组查，当前数据属于哪一块
4.  再到这一块中顺序查找

代码示例：

```
package com.itheima.search;

public class A03_BlockSearchDemo {
    public static void main(String[] args) {
        /*
            分块查找
            核心思想：
                块内无序，块间有序
            实现步骤：
                1.创建数组blockArr存放每一个块对象的信息
                2.先查找blockArr确定要查找的数据属于哪一块
                3.再单独遍历这一块数据即可
        */
        int[] arr = {16, 5, 9, 12,21, 19,
                     32, 23, 37, 26, 45, 34,
                     50, 48, 61, 52, 73, 66};

        //创建三个块的对象
        Block b1 = new Block(21,0,5);
        Block b2 = new Block(45,6,11);
        Block b3 = new Block(73,12,19);

        //定义数组用来管理三个块的对象（索引表）
        Block[] blockArr = {b1,b2,b3};

        //定义一个变量用来记录要查找的元素
        int number = 37;

        //调用方法，传递索引表，数组，要查找的元素
        int index = getIndex(blockArr,arr,number);

        //打印一下
        System.out.println(index);



    }

    //利用分块查找的原理，查询number的索引
    private static int getIndex(Block[] blockArr, int[] arr, int number) {
        //1.确定number是在那一块当中
        int indexBlock = findIndexBlock(blockArr, number);

        if(indexBlock == -1){
            //表示number不在数组当中
            return -1;
        }

        //2.获取这一块的起始索引和结束索引   --- 30
        // Block b1 = new Block(21,0,5);   ----  0
        // Block b2 = new Block(45,6,11);  ----  1
        // Block b3 = new Block(73,12,19); ----  2
        int startIndex = blockArr[indexBlock].getStartIndex();
        int endIndex = blockArr[indexBlock].getEndIndex();

        //3.遍历
        for (int i = startIndex; i <= endIndex; i++) {
            if(arr[i] == number){
                return i;
            }
        }
        return -1;
    }


    //定义一个方法，用来确定number在哪一块当中
    public static int findIndexBlock(Block[] blockArr,int number){ //100


        //从0索引开始遍历blockArr，如果number小于max，那么就表示number是在这一块当中的
        for (int i = 0; i < blockArr.length; i++) {
            if(number <= blockArr[i].getMax()){
                return i;
            }
        }
        return -1;
    }



}

class Block{
    private int max;//最大值
    private int startIndex;//起始索引
    private int endIndex;//结束索引


    public Block() {
    }

    public Block(int max, int startIndex, int endIndex) {
        this.max = max;
        this.startIndex = startIndex;
        this.endIndex = endIndex;
    }

    /**
     * 获取
     * @return max
     */
    public int getMax() {
        return max;
    }

    /**
     * 设置
     * @param max
     */
    public void setMax(int max) {
        this.max = max;
    }

    /**
     * 获取
     * @return startIndex
     */
    public int getStartIndex() {
        return startIndex;
    }

    /**
     * 设置
     * @param startIndex
     */
    public void setStartIndex(int startIndex) {
        this.startIndex = startIndex;
    }

    /**
     * 获取
     * @return endIndex
     */
    public int getEndIndex() {
        return endIndex;
    }

    /**
     * 设置
     * @param endIndex
     */
    public void setEndIndex(int endIndex) {
        this.endIndex = endIndex;
    }

    public String toString() {
        return "Block{max = " + max + ", startIndex = " + startIndex + ", endIndex = " + endIndex + "}";
    }
}

```

#### 30.1.6. 哈希查找

哈希查找是分块查找的进阶版，适用于数据一边添加一边查找的情况。

一般是数组 + 链表的结合体或者是数组 + 链表 + 红黑树的结合体

在课程中，为了让大家方便理解，所以规定：

*   数组的 0 索引处存储 1~100
*   数组的 1 索引处存储 101~200
*   数组的 2 索引处存储 201~300
*   以此类推

但是实际上，我们一般不会采取这种方式，因为这种方式容易导致一块区域添加的元素过多，导致效率偏低。

更多的是先计算出当前数据的哈希值，用哈希值跟数组的长度进行计算，计算出应存入的位置，再挂在数组的后面形成链表，如果挂的元素太多而且数组长度过长，我们也会把链表转化为红黑树，进一步提高效率。

具体的过程，大家可以参见 B 站阿玮讲解课程：从入门到起飞。在集合章节详细讲解了哈希表的数据结构。全程采取动画形式讲解，让大家一目了然。

在此不多做阐述。

#### 30.1.7. 树表查找

本知识点涉及到数据结构：树。

建议先看一下后面阿玮讲解的数据结构，再回头理解。

基本思想：二叉查找树是先对待查找的数据进行生成树，确保树的左分支的值小于右分支的值，然后在就行和每个节点的父节点比较大小，查找最适合的范围。 这个算法的查找效率很高，但是如果使用这种查找方法要首先创建树。

二叉查找树（BinarySearch Tree，也叫二叉搜索树，或称二叉排序树 Binary Sort Tree），具有下列性质的二叉树：

1）若任意节点左子树上所有的数据，均小于本身；

2）若任意节点右子树上所有的数据，均大于本身；

二叉查找树性质：对二叉查找树进行中序遍历，即可得到有序的数列。

​

基于二叉查找树进行优化，进而可以得到其他的树表查找算法，如平衡树、红黑树等高效算法。

具体细节大家可以参见 B 站阿玮讲解课程：从入门到起飞。在集合章节详细讲解了树数据结构。全程采取动画形式讲解，让大家一目了然。

在此不多做阐述。

​ 不管是二叉查找树，还是平衡二叉树，还是红黑树，查找的性能都比较高

### 30.2 排序算法

#### 30.2.1. 冒泡排序

冒泡排序（Bubble Sort）也是一种简单直观的排序算法。

它重复的遍历过要排序的数列，一次比较相邻的两个元素，如果他们的顺序错误就把他们交换过来。

这个算法的名字由来是因为越大的元素会经由交换慢慢 "浮" 到最后面。

当然，大家可以按照从大到小的方式进行排列。

##### 30.2.1.1 算法步骤

1.  相邻的元素两两比较，大的放右边，小的放左边
2.  第一轮比较完毕之后，最大值就已经确定，第二轮可以少循环一次，后面以此类推
3.  如果数组中有 n 个数据，总共我们只要执行 n-1 轮的代码就可以

##### 30.2.1.2 动图演示

![](<assets/1740015261668.png>)

##### 30.2.1.3 代码示例

```
public class A01_BubbleDemo {
    public static void main(String[] args) {
        /*
            冒泡排序：
            核心思想：
            1，相邻的元素两两比较，大的放右边，小的放左边。
            2，第一轮比较完毕之后，最大值就已经确定，第二轮可以少循环一次，后面以此类推。
            3，如果数组中有n个数据，总共我们只要执行n-1轮的代码就可以。
        */


        //1.定义数组
        int[] arr = {2, 4, 5, 3, 1};

        //2.利用冒泡排序将数组中的数据变成 1 2 3 4 5

        //外循环：表示我要执行多少轮。 如果有n个数据，那么执行n - 1 轮
        for (int i = 0; i < arr.length - 1; i++) {
            //内循环：每一轮中我如何比较数据并找到当前的最大值
            //-1：为了防止索引越界
            //-i：提高效率，每一轮执行的次数应该比上一轮少一次。
            for (int j = 0; j < arr.length - 1 - i; j++) {
                //i 依次表示数组中的每一个索引：0 1 2 3 4
                if(arr[j] > arr[j + 1]){
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }

        printArr(arr);




    }

    private static void printArr(int[] arr) {
        //3.遍历数组
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
    }
}

```

#### 30.2.2. 选择排序

##### 30.2.2.1 算法步骤

1.  从 0 索引开始，跟后面的元素一一比较
2.  小的放前面，大的放后面
3.  第一次循环结束后，最小的数据已经确定
4.  第二次循环从 1 索引开始以此类推
5.  第三轮循环从 2 索引开始以此类推
6.  第四轮循环从 3 索引开始以此类推。

##### 30.2.2.2 动图演示

![](<assets/1740015261980.png>)

```
public class A02_SelectionDemo {
    public static void main(String[] args) {

        /*
            选择排序：
                1，从0索引开始，跟后面的元素一一比较。
                2，小的放前面，大的放后面。
                3，第一次循环结束后，最小的数据已经确定。
                4，第二次循环从1索引开始以此类推。

         */


        //1.定义数组
        int[] arr = {2, 4, 5, 3, 1};


        //2.利用选择排序让数组变成 1 2 3 4 5
       /* //第一轮：
        //从0索引开始，跟后面的元素一一比较。
        for (int i = 0 + 1; i < arr.length; i++) {
            //拿着0索引跟后面的数据进行比较
            if(arr[0] > arr[i]){
                int temp = arr[0];
                arr[0] = arr[i];
                arr[i] = temp;
            }
        }*/

        //最终代码：
        //外循环：几轮
        //i:表示这一轮中，我拿着哪个索引上的数据跟后面的数据进行比较并交换
        for (int i = 0; i < arr.length -1; i++) {
            //内循环：每一轮我要干什么事情？
            //拿着i跟i后面的数据进行比较交换
            for (int j = i + 1; j < arr.length; j++) {
                if(arr[i] > arr[j]){
                    int temp = arr[i];
                    arr[i] = arr[j];
                    arr[j] = temp;
                }
            }
        }


        printArr(arr);


    }
    private static void printArr(int[] arr) {
        //3.遍历数组
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
    }

}


```

#### 30.2.3. 插入排序

插入排序的代码实现虽然没有冒泡排序和选择排序那么简单粗暴，但它的原理应该是最容易理解的了，因为只要打过扑克牌的人都应该能够秒懂。插入排序是一种最简单直观的排序算法，它的工作原理是通过创建有序序列和无序序列，然后再遍历无序序列得到里面每一个数字，把每一个数字插入到有序序列中正确的位置。

插入排序在插入的时候，有优化算法，在遍历有序序列找正确位置时，可以采取二分查找

##### 30.2.3.1 算法步骤

将 0 索引的元素到 N 索引的元素看做是有序的，把 N+1 索引的元素到最后一个当成是无序的。

遍历无序的数据，将遍历到的元素插入有序序列中适当的位置，如遇到相同数据，插在后面。

N 的范围：0~ 最大索引

##### 30.2.3.2 动图演示

![](<assets/1740015262318.png>)

```
package com.itheima.mysort;


public class A03_InsertDemo {
    public static void main(String[] args) {
        /*
            插入排序：
                将0索引的元素到N索引的元素看做是有序的，把N+1索引的元素到最后一个当成是无序的。
                遍历无序的数据，将遍历到的元素插入有序序列中适当的位置，如遇到相同数据，插在后面。
                N的范围：0~最大索引

        */
        int[] arr = {3, 44, 38, 5, 47, 15, 36, 26, 27, 2, 46, 4, 19, 50, 48};

        //1.找到无序的哪一组数组是从哪个索引开始的。  2
        int startIndex = -1;
        for (int i = 0; i < arr.length; i++) {
            if(arr[i] > arr[i + 1]){
                startIndex = i + 1;
                break;
            }
        }

        //2.遍历从startIndex开始到最后一个元素，依次得到无序的哪一组数据中的每一个元素
        for (int i = startIndex; i < arr.length; i++) {
            //问题：如何把遍历到的数据，插入到前面有序的这一组当中

            //记录当前要插入数据的索引
            int j = i;

            while(j > 0 && arr[j] < arr[j - 1]){
                //交换位置
                int temp = arr[j];
                arr[j] = arr[j - 1];
                arr[j - 1] = temp;
                j--;
            }

        }
        printArr(arr);
    }

    private static void printArr(int[] arr) {
        //3.遍历数组
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
    }

}


```

#### 30.2.4. 快速排序

快速排序是由东尼 · 霍尔所发展的一种排序算法。

快速排序又是一种分而治之思想在排序算法上的典型应用。

快速排序的名字起的是简单粗暴，因为一听到这个名字你就知道它存在的意义，就是快，而且效率高！

它是处理大数据最快的排序算法之一了。

##### 30.2.4.1 算法步骤

1.  从数列中挑出一个元素，一般都是左边第一个数字，称为 “基准数”;
2.  创建两个指针，一个从前往后走，一个从后往前走。
3.  先执行后面的指针，找出第一个比基准数小的数字
4.  再执行前面的指针，找出第一个比基准数大的数字
5.  交换两个指针指向的数字
6.  直到两个指针相遇
7.  将基准数跟指针指向位置的数字交换位置，称之为：基准数归位。
8.  第一轮结束之后，基准数左边的数字都是比基准数小的，基准数右边的数字都是比基准数大的。
9.  把基准数左边看做一个序列，把基准数右边看做一个序列，按照刚刚的规则递归排序

##### 30.2.4.2 动图演示

![](<assets/1740015262513.png>)

```
package com.itheima.mysort;

import java.util.Arrays;

public class A05_QuickSortDemo {
   public static void main(String[] args) {
       System.out.println(Integer.MAX_VALUE);
       System.out.println(Integer.MIN_VALUE);
     /*
       快速排序：
           第一轮：以0索引的数字为基准数，确定基准数在数组中正确的位置。
           比基准数小的全部在左边，比基准数大的全部在右边。
           后面以此类推。
     */

       int[] arr = {1,1, 6, 2, 7, 9, 3, 4, 5, 1,10, 8};


       //int[] arr = new int[1000000];

      /* Random r = new Random();
       for (int i = 0; i < arr.length; i++) {
           arr[i] = r.nextInt();
       }*/


       long start = System.currentTimeMillis();
       quickSort(arr, 0, arr.length - 1);
       long end = System.currentTimeMillis();

       System.out.println(end - start);//149

       System.out.println(Arrays.toString(arr));
       //课堂练习：
       //我们可以利用相同的办法去测试一下，选择排序，冒泡排序以及插入排序运行的效率
       //得到一个结论：快速排序真的非常快。

      /* for (int i = 0; i < arr.length; i++) {
           System.out.print(arr[i] + " ");
       }*/

   }


   /*
    *   参数一：我们要排序的数组
    *   参数二：要排序数组的起始索引
    *   参数三：要排序数组的结束索引
    * */
   public static void quickSort(int[] arr, int i, int j) {
       //定义两个变量记录要查找的范围
       int start = i;
       int end = j;

       if(start > end){
           //递归的出口
           return;
       }



       //记录基准数
       int baseNumber = arr[i];
       //利用循环找到要交换的数字
       while(start != end){
           //利用end，从后往前开始找，找比基准数小的数字
           //int[] arr = {1, 6, 2, 7, 9, 3, 4, 5, 10, 8};
           while(true){
               if(end <= start || arr[end] < baseNumber){
                   break;
               }
               end--;
           }
           System.out.println(end);
           //利用start，从前往后找，找比基准数大的数字
           while(true){
               if(end <= start || arr[start] > baseNumber){
                   break;
               }
               start++;
           }



           //把end和start指向的元素进行交换
           int temp = arr[start];
           arr[start] = arr[end];
           arr[end] = temp;
       }

       //当start和end指向了同一个元素的时候，那么上面的循环就会结束
       //表示已经找到了基准数在数组中应存入的位置
       //基准数归位
       //就是拿着这个范围中的第一个数字，跟start指向的元素进行交换
       int temp = arr[i];
       arr[i] = arr[start];
       arr[start] = temp;

       //确定6左边的范围，重复刚刚所做的事情
       quickSort(arr,i,start - 1);
       //确定6右边的范围，重复刚刚所做的事情
       quickSort(arr,start + 1,j);

   }
}

```

## 30. 补充（目前了解，日后补充）

### 30.1 类加载器

#### 30.1.1 概述

类加载器∶负责将. class 文件 (存储的物理文件）加载在到内存中

![](<assets/1740015262693.png>)

#### 30.1.2 类加载的时机

1.  创建类的实例（对象）
2.  调用类的类方法
3.  访问类或者接口的类变量，或者为该类变量赋值
4.  使用反射方式来强制创建某个类或接口对应的 java.lang.Class 对象
5.  初始化某个类的子类
6.  直接使用 java.exe 命令来运行某个主类

总结：用到就加载，不用就不加载

#### 30.1.3 类加载的过程

1.  加载
    
    *   通过包名 + 类名，获取这个类，准备用流进行传输
    *   在这个类加载到内存中
    *   加载完毕创建一个 class 对象
    
    ![](<assets/1740015262993.png>)
    
2.  链接
    
    *   验证：确保 Class 文件字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身安全 (文件中的信息是否符合虚拟机规范有没有安全隐患)
    
    ![](<assets/1740015263305.png>)
    
    *   准备：负责为类的类变量（被 static 修饰的变量）分配内存，并设置默认初始化值 (初始化静态变量)
    
    ![](<assets/1740015263673.png>)
    
    *   解析：将类的二进制数据流中的符号引用替换为直接引用 (本类中如果用到了其他类，此时就需要找到对应的类)
    
    ![](<assets/1740015263908.png>)
    
3.  初始化：根据程序员通过程序制定的主观计划去初始化类变量和其他资源 (静态变量赋值以及初始化其他资源)
    
    ![](<assets/1740015264111.png>)
    

小结：

*   当一个类被使用的时候，才会加载到内存
*   类加载的过程: 加载、验证、准备、解析、初始化

#### 30.1.4 类加载的分类

分类

*   Bootstrap class loader：虚拟机的内置类加载器，通常表示为 null ，并且没有父 null
*   Platform class loader：平台类加载器, 负责加载 JDK 中一些特殊的模块
*   System class loader：系统类加载器, 负责加载用户类路径上所指定的类库

#### 30.1.5 双亲委派模型

​ 如果一个类加载器收到了类加载请求，它并不会自己先去加载，而是把这个请求委托给父类的加载器去执行，如果父类加载器还存在其父类加载器，则进一步向上委托，依次递归，请求最终将到达顶层的启动类加载器，如果父类加载器可以完成类加载任务，就成功返回，倘若父类加载器无法完成此加载任务，子加载器才会尝试自己去加载，这就是双亲委派模式

![](<assets/1740015264365.png>)

#### 30.1.6 类加载器常用方法

<table><thead><tr><th>方法名</th><th>说明</th></tr></thead><tbody><tr><td>public static ClassLoader getSystemClassLoader()</td><td>获取系统类加载器</td></tr><tr><td>public InputStream getResourceAsStream(String name)</td><td>加载某一个资源文件</td></tr></tbody></table>

示例：

```
public class ClassLoaderDemo2 {
    public static void main(String[] args) throws IOException {
        //static ClassLoader getSystemClassLoader() 获取系统类加载器
        //InputStream getResourceAsStream(String name)  加载某一个资源文件

        //获取系统类加载器
        ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();

        //利用加载器去加载一个指定的文件
        //参数：文件的路径（放在src的根目录下，默认去那里加载）
        //返回值：字节流。
        InputStream is = systemClassLoader.getResourceAsStream("prop.properties");

        Properties prop = new Properties();
        prop.load(is);

        System.out.println(prop);

        is.close();
    }
}

```

注意:

prop.properties 文件，需要放在 src 下，才能被读取并加载

### 30.2XML

#### 30.2.1 三种配置文件的优缺点

![](<assets/1740015264692.png>)

#### 30.2.2 概述

*   XML 的全称为 (EXtensible Markup Language)，是一种**可扩展**的**标记语言**
*   标记语言: 通过标签来描述数据的一门语言 (标签有时我们也将其称之为元素)
*   可扩展：标签的名字是可以自定义的, XML 文件是由很多标签组成的, 而标签名是可以自定义的

例如：

![](<assets/1740015264991.png>)

*   作用
    
    *   用于进行存储数据和传输数据
    *   作为软件的配置文件
*   作为配置文件的优势
    
    *   可读性好
    *   可维护性高

#### 30.2.3 使用

**XML 的创建**

*   就是创建一个 XML 类型的文件，要求文件的后缀必须使用 xml，如 hello_world.xml

![](<assets/1740015265332.png>)

**XML 的基本语法**

*   标签由一对尖括号和合法标识符组成
    
    ```
    <student>
    
    ```
    
*   标签必须成对出现
    
    ```
    <student> </student>
    前边的是开始标签，后边的是结束标签
    
    ```
    
*   特殊的标签可以不成对, 但是必须有结束标记
    
    ```
    <address/>
    
    ```
    
*   标签中可以定义属性, 属性和标签名空格隔开, 属性值必须用引号引起来
    
    ```
    <student> </student>
    
    ```
    
*   标签需要正确的嵌套
    
    ```
    这是正确的: <student> <name>张三</name> </student>
    这是错误的: <student><name>张三</student></name>
    
    ```
    

#### 30.2.4 文档约束

##### 30.2.4.1 概述

##### 30.2.4.2 分类

### 30.3 单元测试

#### 30.3.1 概述

#### 30.3.2 快速入门

![](<assets/1740015265646.png>)