---
url: https://blog.csdn.net/qq_40991313/article/details/140324927
title: Java 修仙之路，十万字吐血整理全网最完整 Java 学习笔记（高级篇）_java 高级学习 - CSDN 博客
date: 2025-02-20 09:12:44
tag: 
summary: 
---
**导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**推荐视频：**

[黑马程序员全套 Java 教程_哔哩哔哩](https://www.bilibili.com/video/BV18J411W7cE/?spm_id_from=333.337.search-card.all.click "黑马程序员全套Java教程_哔哩哔哩")

[尚硅谷 Java 入门视频教程_哔哩哔哩](https://www.bilibili.com/video/BV1Kb411W75N/?spm_id_from=333.337.search-card.all.click "尚硅谷Java入门视频教程_哔哩哔哩")

**推荐书籍：**

《Java 编程思想 （第 4 版）》 

《Java 核心技术 · 卷 I（原书第 12 版） : 开发基础》

**目录**

[十一、多线程](#t0)

[11.1 基本介绍](#t1)

[11.1.1  线程和进程的关系](#t2)

[11.1.2 多线程](#t3)

[11.2 创建线程方法](#t4)

[11.2.0 简介](#t5)

[11.2.1 方法 1：继承 Thread 类](#t6)

[11.2.2 方法 2：实现 Runnable 接口](#t7) 

[11.2.3 方法 3：实现 Callable 接口](#t8)

[11.2.4 方法 4：线程池](#t9)

[11.3 知识加油站](#t10)

[11.3.1 线程生命周期](#t11)

[11.3.2 线程的通信方式](#t12)

[11.3.3 线程池](#t13)

[11.3.3.1 作用](#11.3.3.1%20%E4%BD%9C%E7%94%A8)

[11.3.3.2 生命周期](#11.3.3.2%20%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%C2%A0) 

[11.3.3.3 创建线程池的方式 1：线程池工具类](#11.3.3.3%20%E5%88%9B%E5%BB%BA%E7%BA%BF%E7%A8%8B%E6%B1%A0%E7%9A%84%E6%96%B9%E5%BC%8F1%EF%BC%9A%E7%BA%BF%E7%A8%8B%E6%B1%A0%E5%B7%A5%E5%85%B7%E7%B1%BB)

[11.3.3.4 创建线程池的方式 2：自定义线程池（推荐）](#11.3.3.4%20%E5%88%9B%E5%BB%BA%E7%BA%BF%E7%A8%8B%E6%B1%A0%E7%9A%84%E6%96%B9%E5%BC%8F2%EF%BC%9A%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BA%BF%E7%A8%8B%E6%B1%A0%EF%BC%88%E6%8E%A8%E8%8D%90%EF%BC%89)

[11.3.3.5 如何为线程池设置合适的线程数](#11.3.3.5%20%E5%A6%82%E4%BD%95%E4%B8%BA%E7%BA%BF%E7%A8%8B%E6%B1%A0%E8%AE%BE%E7%BD%AE%E5%90%88%E9%80%82%E7%9A%84%E7%BA%BF%E7%A8%8B%E6%95%B0)

[11.3.3.6 线程池的原理](#11.3.3.6%20%E7%BA%BF%E7%A8%8B%E6%B1%A0%E7%9A%84%E5%8E%9F%E7%90%86)

[11.3.4 练习：多线程交替打印 A/B/C，每个打印 3 次](#t14)

[11.4 线程安全](#t15)

[11.4.1 基本介绍](#t16) 

[11.4.2 原子类](#t17)

[11.4.3 volatile 关键字](#t18)

[11.4.4 锁](#t19)

[11.4.5 线程安全的集合](#t20)

[11.5  线程同步](#t21)

[11.5.1 基本介绍](#t22)

[11.5.2 synchronized 锁](#t23)

[11.5.2.1 基本介绍](#11.5.2.1%20%E5%9F%BA%E6%9C%AC%E4%BB%8B%E7%BB%8D)

[11.5.2.2 同步代码块](#11.5.2.2%20%E5%90%8C%E6%AD%A5%E4%BB%A3%E7%A0%81%E5%9D%97)

[11.5.2.3 同步方法](#%E5%90%8C%E6%AD%A5%E6%96%B9%E6%B3%95)

[11.5.2.4 知识加油站：synchronized 锁的原理](#11.5.2.4%20%E7%9F%A5%E8%AF%86%E5%8A%A0%E6%B2%B9%E7%AB%99%EF%BC%9Asynchronized%E9%94%81%E7%9A%84%E5%8E%9F%E7%90%86)

[11.5.3 Lock 锁](#t24)

[11.5.4 synchronized 和 Lock 的区别](#t25)

[十二、 反射](#t26)

[12.1 基本介绍](#t27)

[12.2 反射获取 Class 对象](#t28)

[12.2.1 基本介绍](#t29)

[12.2.2 全限定名和规范名](#t30)

[12.3 反射获取成员](#t31)

[12.3.1 反射获取构造方法](#t32)

[12.3.2 反射获取字段](#t33)

[12.3.3 反射获取普通方法](#t34)

## 十一、多线程

### 11.1 基本介绍

#### 11.1.1  线程和进程的关系

**进程：**是操作系统分配资源的基本单位，有独立的地址空间（内存空间的一部分，用于存储进程中的代码、数据和堆栈等信息）和内存空间，进程之间不能共享资源，上下文切换慢，并发低，能独立执行（有程序入口、执行序列、出口），更健壮（因为进程崩溃后不会影响其他进程）。

**线程：**是操作系统调度的基本单位，没有独立的地址空间和内存空间（只有自己的堆栈和局部变量，只能共享所在进程的内存空间），线程之间可以共享进程内的资源，上下文切换快，并发高，不能独立执行（应用程序控制多线程执行，进程通过管理线程优先级间接控制线程执行），不健壮（因为一个线程崩溃会导致整个进程崩溃）。

**关系：**一个程序运行后至少包括一个进程，一个进程至少有一个线程。 

运行时数据区包括本地方法栈、虚拟机栈、方法区、堆、程序计数器。每个线程都有独自的本地方法栈、虚拟机栈、程序计数器。各线程共享进程的方法区和堆。

**JVM 运行时数据区参考：**

[什么是 JVM 的内存模型？详细阐述 Java 中局部变量、常量、类名等信息在 JVM 中的存储位置_jvm 中主要用于存储类的元数据 (类型信息 (类的描述信息 类的元数据))、静态变量、常 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/134742377?spm=1001.2014.3001.5501 "什么是JVM的内存模型？详细阐述Java中局部变量、常量、类名等信息在JVM中的存储位置_jvm中主要用于存储类的元数据(类型信息(类的描述信息 类的元数据))、静态变量、常-CSDN博客")

#### 11.1.2 多线程

一个程序运行后至少包括一个进程，一个进程至少有一个线程，一个进程下有多个线程并发地处理任务，称为多线程。

**多线程的好处：**当一个线程进入阻塞或者等待状态时，其他的线程可以获取 CPU 的执行权，提高了 CPU 的利用率。

**多线程的缺点：**

*   **死锁：**多个进程或线程相互等待对方释放所持有的资源，从而无法继续执行的情况。若无外力作用，它们都将无法推进下去。死锁用占用 CPU、内存等系统资源，导致资源浪费，死锁会导致程序无法正常退出，导致系统性能差。
*   **上下文频繁切换：**频繁的上下文切换可能会造成资源的浪费；
*   **串行：**如果因为资源的限制，多线程串行执行，可能速度会比单线程更慢。

 **线程的优先级：**java 是抢占式调度模型，每一个 Java 线程都有一个优先级，优先级是一个整数，其取值范围是 1 （Thread.MIN_PRIORITY ） - 10 （Thread.MAX_PRIORITY ）。

默认情况下，每一个线程都会分配一个优先级 NORM_PRIORITY（5）。

**注意：**优先级高的线程只是获取 CPU 时间片的几率高，但并不能保证先执行。

### **11.2 创建线程方法**

#### **11.2.0 简介**

创建线程有 4 种方式：

*    **继承 Thread 类：**继承 Thread 类，重写 run() 方法；然后创建线程对象调用 start() 方法开启线程。start() 方法里包括了 run() 方法，用于开启线程。注意如果直接调用 run() 方法的话，将是普通方法调用，无法起到开启线程的效果
*   **实现 Runnable 接口：**实现 Runnable 接口并重写 run() 方法，将实现类作为构造参数创建 Thread 对象。推荐，因为 Java 是单继承，线程类实现接口的同时，还可以继承其他类实现其他接口。
*   **实现 Callable：**实现 Callable<T> 接口，重写带返回值的 call() 方法；将实现类对象作为构造参数创建 FutureTask<T > 对象；将 FutureTask 对象作为构造参数创建 Thread 对象。所以此方法可以获取线程执行完后的返回值，而前两种方式不能。
*   **ExecutorService 的 submit 或 execute 方法：**execute 和 submit 都是 ExecutorService 接口的方法，用于线程池提交任务。所有线程池都直接或间接实现 ExecutorService 接口。
    *   **execute：**参数只能是 Runnable，没有返回值
    *   **submit：**参数可以是 Runnable、Callable，返回值是 FutureTask  

#### **11.2.1** 方法 1：继承 Thread 类

**创建并启动线程的步骤：** 

1.  创建一个继承了 Thread 类的线程类，重写的 run() 方法是线程执行体。
2.  创建这个类的对象。
3.  调用线程对象的 start() 方法来启动该线程（之后 Java 虚拟机会调用该线程 run 方法）。

**run() 和 start() 区别：**

*   **run()：**封装线程执行的代码，直接调用相当于普通方法的调用。
*   **start()：**启动线程，虚拟机**调用**该线程的 **run()** 方法。

**构造方法：**

*   **Thread():** 创建一个新的线程对象。
*   **Thread(String name):** 创建一个新的线程对象并将其名称设置为指定的名称。
*   **Thread(Runnable target):** 创建一个新的线程对象并将其目标设置为指定的 Runnable 对象。主要用于后面通过 Runable 接口创建线程。
*   **Thread(Runnable target, String name):** 创建一个新的线程对象，将其目标设置为指定的 Runnable 对象，并将其名称设置为指定的名称。

**常用方法：**

*   **void start():** 使线程开始执行；Java 虚拟机调用此线程的 run 方法。
*   **void run():** 如果此线程是使用独立的 Runnable 运行对象构造的，则调用该 Runnable 对象的 run 方法；否则，此方法不执行任何操作并返回。
*   **void join():** 等待该线程执行完成。A 线程调用 B 线程的 join() 方法，A 线程将被阻塞，直到 B 线程执行完。可以用于线程之间的通信。
*   void join(long millis): 等待该线程终止的时间最长为 millis 毫秒。
*   void join(long millis, int nanos): 等待该线程终止的时间最长为 millis 毫秒 + nanos 纳秒。
*   **void interrupt():** 中断该线程。
*   boolean isInterrupted(): 测试当前线程是否已中断。
*   **boolean isAlive():** 测试线程是否处于活动状态。
*   **static void sleep(long millis):** 使当前正在执行的线程休眠（暂停执行）指定的毫秒数。
*   static void sleep(long millis, int nanos): 使当前正在执行的线程休眠（暂停执行）指定的毫秒数加指定的纳秒数。

**属性方法：**

*   **void setName(String name):** 改变线程名称，使之与参数 name 相同。
*   **String getName():** 返回该线程的名称。
*   **void setPriority(int newPriority):** 更改该线程的优先级。
*   **int getPriority():** 返回该线程的优先级。
*   **Thread.State getState():** 返回该线程的状态。
*   **void setDaemon(boolean on):** 将该线程标记为守护线程或用户线程。
*   **boolean isDaemon():** 测试该线程是否为守护线程。用户线程是普通的线程，它们通常是应用程序执行任务的主要线程。守护线程为其他线程提供后台支持。当所有用户线程结束时，JVM 会自动退出，无论守护线程是否仍在运行。

**代码示例 1：**主线程设置名字并查看：

```
    public static void main(String[] args) {
        Thread.currentThread().setName("主线程");
        System.out.println(Thread.currentThread().getName());
    }
```

![](<assets/1740013965317.png>)

**代码示例 2：创建并启动线程**

线程类： 

```
/**
 * @Author: vince
 * @CreateTime: 2024/07/16
 * @Description: 打印数字线程类
 * @Version: 1.0
 */
public class PrintNumberThread extends Thread{
    /**
     * 打印1-100
     */
    @Override
    public void run(){
        for(int i=0;i<100;i++) {
            System.out.println(getName()+":"+i);
        }
    }
 
    /**
     * 构造方法
     * @param name 线程名
     */
    public PrintNumberThread(String name) {
        super(name);
    }
}
```

创建并启动线程 ：

```
public class Test {
    public static void main(String[] args) {
        PrintNumberThread a = new PrintNumberThread ("a"), b = new PrintNumberThread ("b");
        a.start();
        b.start();
    }
}
```

**运行结果：**

可以看到两个线程是随机交替打印的，因为它们获取 CPU 的调度是随机的：

![](<assets/1740013965542.png>)

​

#### **11.2.**2 方法 2：实现 Runnable 接口 

Runnable 翻译：可运行的

**步骤：** 

1.  定义 Runnable 接口的实现类, 并实现该接口的 run() 方法, 该方法将作为线程执行体。
2.  创建 Runnable 实现类的实例, 并将其作为参数来创建 Thread 对象, Thread 对象为线程对象。
3.  调用线程对象的 start() 方法来启动该线程。

**这种办法更好，优点：**

*   **避免 Java 单继承局限性：**Java 是单继承，使用这种方法，线程类实现接口的同时，还可以继承其他类、实现其他接口。
*   **逻辑和数据更好分离：**通过实现 Runnable 接口的方法创建多线程更加适合**同一个资源被多段业务逻辑并行处理**的场景。在同一个资源被多个线程逻辑异步、并行处理的场景中，通过实现 Runnable 接口的方式设计多个 target 执行目标类可以更加方便、清晰地将执行逻辑和数据存储分离，更好地体现了面向对象的设计思想。

**示例：**

```
/**
 * @Author: vince
 * @CreateTime: 2024/07/16
 * @Description: 打印数字Runnable
 * @Version: 1.0
 */
public class PrintNumberRunnable implements Runnable{
    @Override
    public void run(){
        for(int i=0;i<100;i++){
            System.out.println(Thread.currentThread().getName()+":"+i);
        }
    }
}
```

```
public class Test {
    public static void main(String[] args) {
        // 方法1：使用普通方式实现Runnable接口
        PrintNumberRunnable runnable = new PrintNumberRunnable();
        Thread a = new Thread(runnable, "a"), b = new Thread(runnable, "b");
        // 方法2：使用Lambda表达式实现Runnable接口，无需再创建PrintNumberRunnable类
        Thread d = new Thread(() -> {
            for (int i = 0; i < 100; i++) {
                System.out.println(Thread.currentThread().getName() + ":" + i);
            }
        },"d");
        a.start();
        b.start();
        d.start();
    }
}
```

 运行结果： 

![](<assets/1740013965801.png>)

​

#### **11.2.3** 方法 3：**实现 Callable 接口**

通过**实现 Callable 接口**来创建线程的步骤如下

*   实现 Callable<T> 接口，重写带返回值的 call() 方法；
*   将实现类对象作为构造参数创建 FutureTask<T> 对象；
*   将 FutureTask 对象作为构造参数创建 Thread 对象。

相比于前两种方法，此方法可以**获取线程执行完后的返回值**，而前两种方式不能，因为 call() 方法是有返回值的。 

**代码示例：**

```
class MyCallable implements Callable {
    @Override
    public Object call() throws Exception {
        return null;
    }
}
```

```
	FutureTask task = new FutureTask(new MyCallable());
    new Thread(task).start();
```

#### **11.2.**4 方法 4：**线程池**

线程池（Thread Pool）是一种多线程处理方式，用于减少创建和销毁线程的开销，提高系统资源利用率和处理效率。

**线程池作用：** 

*   **管理线程数量：**它可以管理线程的数量, 可以避免无节制的创建线程, 导致超出系统负荷直至崩溃。
*   **让线程复用：**它还可以让线程复用, 可以大大地减少创建和销毁线程所带来的开销。

**线程池的两种创建方法：**

*   执行器工具类 Executors；
*   自定义线程池 ThreadPoolExecutor 

**线程池两种提交任务的方法：**

execute 和 submit 都是 ExecutorService 接口的方法，用于线程池提交任务。所有线程池都直接或间接实现 ExecutorService 接口。

*   **execute：**参数只能是 Runnable，没有返回值
*   **submit：**参数可以是 Runnable、Callable，返回值是 FutureTask 

代码示例：

**两种创建线程池的方法：**

线程池工具类，创建固定大小的线程池：

```
        ExecutorService executorService = Executors.newFixedThreadPool(10);
        executorService.execute(new Runnable() {
            @Override
            public void run() {
                System.out.println("当前线程"+Thread.currentThread());
            }
        });
```

自定义线程池（腿姐）：

```
ThreadPoolExecutor executor = new ThreadPoolExecutor( 
    			 5,    //核心线程数
                 200,    //最大线程数量，控制资源并发
                 10,    //存活时间
                TimeUnit.SECONDS,    //时间单位
                new LinkedBlockingDeque<>(  100000),    //任务队列，大小100000个
        Executors.defaultThreadFactory(),    //线程的创建工厂
        new ThreadPoolExecutor.AbortPolicy());    //拒绝策略
        // 任务1
        executor.execute(() -> {
            try {
                Thread.sleep(3 * 1000);
                System.out.println("--helloWorld_001--" + Thread.currentThread().getName());
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
```

### 11.3 知识加油站

#### 11.3.1 线程生命周期

Java 线程在运行的生命周期中, 在任意给定的时刻, 只能处于下列 6 种状态之一：

*   **NEW ：初始状态**, 线程被创建, 但是还没有调用 start 方法。
*   **RUNNABLE：可运行状态**, 等待调度或运行。线程正在 JVM 中执行, 但是有可能在等待操作系统的调度。
*   **BLOCKED ：阻塞状态**, 线程正在等待获取监视器锁。
*   **WAITING ：等待状态**, 线程正在等待其他线程的通知或中断。线程等待状态不占用 CPU 资源，被唤醒后进入可运行状态（等待调度或运行）。
*   **TIMED_WAITING：超时等待状态**, 在 WAITING 的基础上增加了超时时间, 即超出时间自动返回。Thread.sleep(1000); 让线程超时等待 1s。
*   **TERMINATED：终止状态**, 线程已经执行完毕。

**线程的运行过程：**

线程在创建之后默认为 NEW（初始状态），在调用 start 方法之后进入 RUNNABLE（可运行状态）。

**注意：**

可运行状态不代表线程正在运行，它有可能正在等待操作系统的调度。

WAITING （等待状态）的线程需要其他线程的通知才能返回到可运行状态，而 TIMED_WAITING（超时等待状态）相当于在等待状态的基础上增加了超时限制，除了他线程的唤醒，在超时时间到达时也会返回运行状态。

此外，线程在执行同步方法时，在没有获取到锁的情况下，会进入到 BLOCKED（阻塞状态）。线程在执行完 run 方法之后，会进入到 TERMINATED（终止状态）。

![](<assets/1740013966062.png>)

**等待状态如何被唤醒？**

Object 类：

*   wait() 方法让线程进入等待状态
*   notify() 唤醒该对象上的随机一个线程
*   notifyAll() 唤醒该对象上的所有线程。

这 3 个方法必须处于 **synchronized** 代码块或方法中，否则会抛出 IllegalMonitorStateException 异常。因为调用这三个方法之前必须拿要到当前锁对象的监视器（Monitor 对象），synchronized 基于对象头和 Monitor 对象。

另外，也可以通过 **Condition 类的 await/signal/signalAll** 方法实现线程的等待和唤醒，从而实现线程的通信，令线程之间协作处理任务。这两个方法依赖于 Lock 对象。

#### 11.3.2 线程的通信方式

**线程通信：**用于多个线程之间协作工作，共同完成某个任务**。**多个线程在并发执行的时候，他们在 CPU 中是随机切换执行的，这个时候我们想多个线程一起来完成一件任务，这个时候我们就需要线程之间的通信了，多个线程一起来完成一个任务。

![](<assets/1740013966156.png>)

**线程通信方式：** 

*   **通过 volatile 关键字：**多个线程同时监听一个 volatile 变量，当这个变量发生变化的时候 ，线程能够感知并执行相应的业务。利用了 volatile 可见性，即一旦修改变量则立即刷新到共享内存中。
*   **通过 Object 类的 wait/notify/notifyAll 方法：**当我们使用 synchronized 同步时就会使用 Monitor 来实现线程通信，这里的 Monitor 其实就是锁对象，其利用 Object 类的 wait，notify，notifyAll 等方法来实现线程通信。Monitor 是 Java 虚拟机实现锁的一种底层机制，用于控制线程对共享资源的访问。（Object 类的 wait() 方法让线程进入等待状态，notify() 唤醒该对象上的随机一个线程，notifyAll() 唤醒该对象上的所有线程。这 3 个方法必须处于 synchronized 代码块或方法中，否则会抛出 IllegalMonitorStateException 异常。因为调用这三个方法之前必须拿要到当前锁对象的监视器（Monitor 对象），synchronized 基于对象头和 Monitor 对象。）
*   **通过 Condition 类的 await/signal 方法：**而使用 Lock 进行同步时就是使用 Condition 对象来实现线程通信，Condition 对象通过 Lock 的 lock.newCondition() 方法创建，使用其 await，sign 或 signAll 方法实现线程通信。 Condition 是一个与锁 Lock 相关联的条件对象，可以让等待线程在某个条件被满足时被唤醒，从而达到线程协作的目的。
*   **通过 Semaphore 的 acquire/release 方法：** Semaphore 是一个计数信号量，用于控制同时访问某个资源的线程数量。线程可以通过 acquire() 方法获取许可，release() 方法释放许可。
*   **通过 Thread 类的 join() 方法：**join() 方法是等待该线程执行完成。A 线程调用 B 线程的 join() 方法，A 线程将被阻塞，直到 B 线程执行完。

**应用场景：**

*   **线程交替打印：**在多线程交替打印 A/B、或者交替打印 1 到 100 时，需要在锁中使用线程通信。如果不使用 lock.notify() 和 lock.wait()，可能导致当前线程释放锁后立刻又拿回锁（因为多线程是 CPU 随机切换的），从而达不到交替打印的效果
    
    ```
            //第一个线程，例如打印A
            new Thread(
                    () -> {
                        while (true) {
                            synchronized (lock) {
                                // 1.临界值校验：到临界值唤醒其他线程，防止其他线程永远等待；
                                // 2.打印判断：如果需要打印，则打印、操作原子类。 如果用的当前行值原子类，则加1；如果用的总行数原子类，则减1
                                // 4.线程通信：唤醒、等待。
                                // 如果删除下面两行代码，可能导致当前线程释放锁后立刻又拿到锁了，从而达不到交替打印的效果
                                lock.notifyAll();
                                try-catch{lock.wait();}
     
                            }
                        }
     
                    }
            ).start();
            //另一个线程，例如打印B...
    ```
    

#### 11.3.3 线程池

##### 11.3.3.1 作用

为了对多线程进行统一的管理，Java 引入了线程池，它通过限制并发线程的数量、将待执行的线程放入队列、销毁空闲线程，来控制资源消耗，使线程更合理地运行，避免系统因为创建过多线程而崩溃。

**线程池作用：** 

*   **管理线程数量：**它可以管理线程的数量, 可以避免无节制的销毁、创建线程，导致额外的性格损耗、或者线程数超出系统负荷直至崩溃。
*   **提高性能：**当有新的任务到来时，可以直接从线程池中取出一个空闲线程来执行任务，而不需要等待创建新线程，从而减少了响应时间。
*   **让线程复用：**它还可以让线程复用, 可以大大地减少创建和销毁线程所带来的开销。
*   **合理的拒绝策略**：线程池提供了多种拒绝策略，当线程池队列满了时，可以采用不同的策略进行处理，如抛出异常、丢弃任务或调用者运行等。

##### 11.3.3.2 生命周期 

**生命周期：**

通常线程池的生命周期包含 5 个状态，对应状态值分别是：-1、0、1、2、3，这些状态只能由小到大迁移，不可逆。

1.  **RUNNING：运行。**线程池处于正常状态，可以接受新的任务，同时会按照预设的策略来处理已有任务的执行。
2.  **SHUTDOWN：关闭。**线程池处于关闭状态，不再接受新的任务，但是会**继续执行**已有任务直到执行完成。执行线程池对象的 shutdown() 时进入该状态。
3.  **STOP：停止。**线程池处于关闭状态，不再接受新的任务，同时会**中断**正在执行的任务，**清空**线程队列。执行 shutdownNow() 时进入该状态。
4.  **TIDYING：整理。**所有任务已经执行完毕，线程池进入该状态会开始进行一些结尾工作，比如及时清理线程池的一些资源。
5.  **TERMINATED：终止。**线程池已经完全停止，所有的状态都已经结束了，线程池处于最终的状态。

##### 11.3.3.3 创建线程池的方式 1：线程池工具类

**执行器工具类** **Executors 创建线程池：** 底层都是 return new ThreadPoolExecutor(...)。一般不使用这种方式，参数配置死了不可控。

*   **newCachedThreadPool：缓存线程池（无限大）。** 
    *   **核心线程数是 0，最大线程数无限大：**最大线程数 Integer.MAX_VALUE。线程数量可以无限扩大，所有线程都是非核心线程。
    *   **空闲线程存活时间 60s：**keepAliveTime 为 60S，空闲线程超过 60s 会被杀死。
    *   **同步队列：**因为最大线程数无限大，所以也用不到阻塞队列，所以设为没有存储空间的 SynchronousQueue 同步队列。这意味着只要有请求到来，就必须要找到一条工作线程处理他，如果当前没有空闲的线程，那么就会再创建一条新的线程。
*   **newFixedThreadPool：固定大小的线程池。**
    *   **核心线程数：**所有线程都是核心线程（通过构造参数指定），最大线程数 = 核心线程数。
    *   **存活时间 0s：**因为所有线程都是核心线程，所以用不到存活时间，线程都会一直存活。keepAliveTime 为 0S。
    *   **链表阻塞队列：**超出的线程会在 LinkedBlockingQueue 队列中等待。
*   **newScheduledThreadPool：定时任务线程池。**创建一个定长线程池， 支持定时及周期性任务执行。可指定核心线程数，最大线程数。
*   **newSingleThreadExecutor：单线程化的线程池。**核心线程数与最大线程数都只有一个，不回收。后台从 LinkedBlockingQueue 队列中获取任务​。创建一个单线程化的线程池， 它只会用唯一的工作线程来执行任务， 保证所有任务按照指定顺序 (FIFO, LIFO, 优先级) 执行。 

```
ExecutorService executorService = Executors.newFixedThreadPool(10);
 
//源码
FixedThredPool: new ThreadExcutor(n, n, 0L, ms, new LinkedBlockingQueue<Runable>()
SingleThreadExecutor: new ThreadExcutor(1, 1, 0L, ms, new LinkedBlockingQueue<Runable>())
CachedTheadPool: new ThreadExcutor(0, max_valuem, 60L, s, new SynchronousQueue<Runnable>());
ScheduledThreadPoolExcutor: ScheduledThreadPool, SingleThreadScheduledExecutor.
```

一般要搭配计数器 CountDownLatch，await(时间) 让主线程等待，直到任务线程都执行完（计数器减为零），或者到达超时时间，防止无线等待。

##### 11.3.3.4 创建线程池的方式 2：**自定义线程池（推荐）**

**线程池执行器 ThreadPoolExecutor 创建自定义线程池：**

```
        ThreadPoolExecutor threadPoolExecutor= new ThreadPoolExecutor( 
    			 5,    //核心线程数
                 200,    //最大线程数量，控制资源并发
                 10,    //存活时间
                TimeUnit.SECONDS,    //时间单位
                new LinkedBlockingDeque<>(  100000),    //任务队列，大小100000个
        Executors.defaultThreadFactory(),    //线程的创建工厂
        new ThreadPoolExecutor.AbortPolicy());    //拒绝策略
 
        CompletableFuture<Integer> future1 = CompletableFuture.supplyAsync(() -> {    //开启异步编排，有返回值
            return 1;
        }, threadPoolExecutor).thenApplyAsync(res -> {    //串行化，接收参数并有返回值
            return res+1;
        }, threadPoolExecutor);
        Integer integer = future.get();    //获取返回值
```

 **七个参数：**

*   **corePoolSize：核心线程数**。创建以后，会一直存活到线程池销毁，空闲时也不销毁。
*   **maximumPoolSize：最大线程数量**。阻塞队列满了
*   **keepAliveTime： 存活时间**。释放空闲时间超过 “存活时间” 的线程，仅留核心线程数量的线程。
*   **TimeUnitunit：时间单位**
*   **workQueue： 任务队列。**如果线程数超过核心数量，就把剩余的任务放到队列里。只要有线程空闲，就会去队列取出新的任务执行。new LinkedBlockingDeque() 队列大小默认是 Integer 的最大值，内存不够，所以建议指定队列大小。
    *   SynchronousQueue 是一个同步队列，这个阻塞队列没有存储空间，这意味着只要有请求到来，就必须要找到一条工作线程处理他，如果当前没有空闲的线程，那么就会再创建一条新的线程。
    *   **LinkedBlockingQueue** 是一个无界队列，可以缓存无限多的任务。由于其无界特性，因此需要合理地处理好任务的生产速率和线程池中线程的数量，以避免内存溢出等异常问题。无限缓存，拒绝策略就能随意了。
    *   **ArrayBlockingQueue** 是一个有界（容量固定）队列，只能缓存固定数量的任务。通过固定队列容量，可以避免任务过多导致线程阻塞，保证线程池资源的可控性和稳定性。**推荐**，有界队列能增加系统的稳定性和预警能力。可以根据需要设大一点，比如几千，新任务丢弃后未来重新入队。
    *   PriorityBlockingQueue 是一个优先级队列，能够对任务按照优先级进行排序，当任务数量超过队列容量时，会根据元素的 Comparable 或 Comparator 排序规则进行丢弃或抛异常。
        
        ```
        new PriorityBlockingQueue<>((o1, o2) -> o1.length() - o2.length());
        
        ```
        
*   **threadFactory：线程的创建工厂**。可以使用默认的线程工厂 Executors.defaultThreadFactory()，也可以自定义线程工厂（实现 ThreadFactory 接口）
*   **RejectedExecutionHandler handler：拒绝策略。**如果任务队列和最大线程数量满了，按照指定的拒绝策略执行任务。
    *   **Abort（默认）：**直接抛异常（拒绝执行异常 RejectedExecutionException）
    *   **CallerRuns：**直接同步调用线程 run() 方法，不创建线程了
    *   **DiscardOldest：**丢弃最老任务
    *   **Discard：**直接丢弃新任务
    *   实现拒绝执行处理器接口（RejectedExecutionHandler），自定义拒绝策略。

##### 11.3.3.5 如何为线程池设置合适的线程数

下面的参数只是一个预估值，适合初步设置，具体的线程数需要经过压测确定，压榨（更好的利用）CPU 的性能。

**CPU 核心数为 N；**

**核心线程数：**

*   CPU 密集型：N+1。数量与 CPU 核数相近是为了不浪费 CPU，并防止频繁的上下文切换，加 1 是为了有线程被阻塞后还能不浪费 CPU 的算力。
*   **I/O 密集型：**2N，或 N/(1 - 阻塞系数)。I/O 密集型任务 CPU 使用率并不是很高，可以让 CPU 在等待 I/O 操作的时去处理别的任务，充分利用 CPU，所以数量就比 CPU 核心数高一倍。有些公司会考虑阻塞系数，阻塞系数是任务线程被阻塞的比例，一般是 0.8~0.9。
*   **实际开发中更适合的公式：**N*((线程等待时间 + 线程计算时间)/ 线程计算时间)

**最大线程数：**设成核心线程数的 2-4 倍。数量主要由 CPU 和 IO 的密集性、处理的数据量等因素决定。

**需要增加线程的情况：**jstack 打印线程快照，如果发现线程池中大部分线程都等待获取任务、则说明线程够用。如果大部分线程都处于运行状态，可以继续适当调高线程数量。

**jstack：**打印指定进程此刻的线程快照。定位线程长时间停顿的原因，例如死锁、等待资源、阻塞。如果有死锁会打印线程的互相占用资源情况。线程快照：该进程内每条线程正在执行的方法堆栈的集合。

##### 11.3.3.6 线程池的原理

**任务加入时判断的顺序：**核心线程数 、阻塞队列、最大线程数、拒绝策略。

**线程池执原理：** 

1.  新加入任务，判断 corePoolSize 是否到最大值；如果没到最大值就创建核心线程执行新任务，如果到最大值就判断是否有空闲的核心线程；
2.  如果有空闲的核心线程，则空闲核心线程执行新任务，如果没空闲的核心线程，则尝试加入 FIFO 阻塞队列；
3.  若加入成功，则等待空闲核心线程将队头任务取出并执行，若加入失败（例如队列满了），则判断 maximumPoolSize 是否到最大值；
4.  如果没到最大值就创建非核心线程执行新任务，如果到了最大值就执行丢弃策略，默认丢弃新任务；
5.  线程数大于 corePoolSize 时，空闲线程将在 keepAliveTime 后回收，直到线程数等于核心线程数。这些核心线程也不会被回收。

实际上线程本身没有核心和非核心的概念，都是靠比较 corePoolSize 和当前线程数判断一个线程是不是能看作核心线程。

可能某个线程之前被看作是核心线程，等它空闲了，线程池又有 corePoolSize 个线程在执行任务，这个线程到 keepAliveTime 后还是会被回收。

#### 11.3.4 练习：多线程交替打印 A/B/C，每个打印 3 次

 **核心逻辑：**创建线程，循环加锁，执行以下逻辑：

1.  **临界值判断：**到达临界值后唤醒其他线程并结束锁；
2.  **打印判断：**如果需要打印，则打印、操作原子类（只有打印后才操作原子类，否则就是不满足条件，需要下一步的唤醒等待后，进入下一轮的循环）；
3.  **线程通信：**唤醒、等待。

**坑点：**

*   **临界值判断不能放到 while 里：**防止最后一个线程无法唤醒其他线程，从而导致死锁（其他线程没人唤醒了）。
*   **必须用线程通信：**防止当前线程释放锁后立刻又拿回锁（因为多线程是 CPU 随机切换的），从而达不到交替打印的效果

**具体代码（Object 类的 wait() 和 notifyAll() 方案、不抽取方法）：** 

```
/**
 * @Author: vince
 * @CreateTime: 2024/05/13
 * @Description: 多线交替打印A/B/C
 * @Version: 1.0
 */
public class Test2 {
    /**
     * 当前行值
     */
    private static AtomicInteger index = new AtomicInteger(0);
    /**
     * 总打印行数
     */
    private static final int count = 9;
 
    public static void main(String[] args) {
        Object lock = new Object();
        // 下面创建三个线程可以抽取成一个方法，这里方便理解所以拆开
        new Thread(() -> {
            // tip：这里条件没必要index.get()<count，因为where不在锁里。
            // 如果临界值判断加到这里，会导致最后一个线程无法唤醒其他线程，从而导致死锁（其他线程没人唤醒了）。
            while (true) {
                synchronized (lock) {
                    // 1.临界值判断：到达临界值后唤醒其他线程并结束锁；
                    if(index.get()>=count){
                        lock.notifyAll();
                        break;
                    }
                    // 2.打印判断：如果需要打印，则打印、操作原子类
                    if (index.get() % 3 == 0) {
                        System.out.println("A");
                        // 只有打印后才操作原子类，否则就是不满足条件，需要下一步的唤醒等待后，进入下一轮的循环
                        index.getAndIncrement();
 
                    }
                    // 3.线程通信：唤醒、等待
                    // 3.1 唤醒其他线程：不管能不能整除，结束后都唤醒其他线程
                    // notifyAll()唤醒该对象上的所有线程
                    lock.notifyAll();
                    // 3.2 当前线程等待：Object类的wait()方法让线程进入等待状态，直到其他线程调用notify()或notifyAll()方法唤醒
                    try {
                        lock.wait();
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                }
            }
        }, "线程1打印A").start();
        new Thread(() -> {
            while (true) {
                synchronized (lock) {
                    if(index.get()>=count){
                        lock.notifyAll();
                        break;
                    }
                    if (index.get() % 3 == 1) {
                        System.out.println("B");
                        index.getAndIncrement();
                    }
                    lock.notifyAll();
                    try {
                        lock.wait();
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                }
            }
        }, "线程2打印B").start();
        new Thread(() -> {
            while (true) {
                synchronized (lock) {
                    if(index.get()>=count){
                        lock.notifyAll();
                        break;
                    }
                    if (index.get() % 3 == 2) {
                        System.out.println("C");
                        index.getAndIncrement();
                    }
                    lock.notifyAll();
                    try {
                        lock.wait();
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                }
            }
        }, "线程3打印C").start();
    }
}
```

 **结果：**

![](<assets/1740013966442.png>)

**具体代码（创建和启动线程抽取方法）：**

```
import java.util.concurrent.atomic.AtomicInteger;
 
/**
 * @Author: vince
 * @CreateTime: 2024/05/13
 * @Description: 多线交替打印A/B/C
 * @Version: 1.0
 */
public class Test2 {
    /**
     * 当前行值
     */
    private static AtomicInteger index = new AtomicInteger(0);
    /**
     * 总打印行数
     */
    private static final int count = 9;
 
    public static void main(String[] args) {
        Object lock = new Object();
 
        createAndStartThread("线程1打印A", lock, 0, "A");
        createAndStartThread("线程2打印B", lock, 1, "B");
        createAndStartThread("线程3打印C", lock, 2, "C");
    }
 
    private static void createAndStartThread(String threadName, Object lock, int remainder, String output) {
        new Thread(() -> {
            while (true) {
                synchronized (lock) {
                    if (index.get() >= count) {
                        lock.notifyAll();
                        break;
                    }
                    if (index.get() % 3 == remainder) {
                        System.out.println(output);
                        index.getAndIncrement();
                    }
                    lock.notifyAll();
                    try {
                        lock.wait();
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                }
            }
        }, threadName).start();
    }
}
```

**其他线程通信方式：**

*   Object 类的 wait() 和 notifyAll()（采用）
*   **Conditon 的 await，sign 或 signAll 方法：**创建三个 Conditon 对象 A/B/C，A.await() 就是让 A 线程等待；
*   **Semaphore 的 acquire 和 release 方法：**使用三个 Semaphore 对象，分别初始化为 1、0、0，表示 A、B、C 三个线程的初始许可数。每个线程在打印字母之前，需要调用对应的 Semaphore 对象的 acquire 方法，获取许可。每个线程在打印字母之后，需要调用下一个 Semaphore 对象的 release 方法，释放许可。

### 11.4 线程安全

#### 11.4.1 基本介绍 

**线程安全：**程序在多线程环境下可以持续进行正确的处理，不会产生数据竞争（例如死锁）和不一致的问题。解决方案：原子类、volatile、锁、线程安全的集合 

**线程安全的解决方案：**按照资源占用情况由轻到重排列：

*   **原子类：**具有原子操作特征（化学中原子是最小单位、不可分割）的类，只能保证单个共享变量的线程安全
*   **volatile：**只能保证单个共享变量的线程安全
*   **锁：**可以保证临界区内的多个共享变量线程安全。

#### 11.4.2 **原子类**

原子类是具有原子操作特征（化学中原子是最小单位、不可分割）的类，原子是指一个操作是不可中断的。即使是在多个线程一起执行的时候，一个操作一旦开始，就不会被其他线程干扰。

在 java.util.concurrent.atomic 包下，有一系列 “Atomic” 开头的类，统称为原子类。例如 AtomicInteger 替代 int ，底层采用 CAS 原子指令实现，内部的存储值使用 volatile 修饰，因此多线程之间是修改可见的。

以 AtomicInteger 为例，某线程调用该对象的 incrementAndGet() 方式自增时采用 CAS 尝试修改它的值，若此时没有其他线程操作该值便修改成功否则反复执行 CAS 操作直到修改成功。

**CAS：**不断对变量进行原子性比较和交换，从而解决单个变量的线程安全问题。。比较内存中值和预期值，如果相等则交换，如果不相等就代表被其他线程改了则重试。 

**AtomicInteger 常用方法：**   

*   **构造方法：**
    *   AtomicInteger (): 创建一个初始值为 0 的 AtomicInteger。
    *   AtomicInteger(int initialValue): 创建一个初始值为 initialValue 的 AtomicInteger。
    *   获取和设置：
    *   int get(): 获取当前的值。
    *   void set(int newValue): 设置为 newValue。
    *   int getAndSet(int newValue): 获取当前值，并设置为 newValue。 
*   **原子更新：**
    *    boolean compareAndSet(int expect, int update): 如果当前值等于 expect，则更新为 update。
    *   int getAndIncrement(): 以原子方式将当前值加 1，返回的是旧值。
    *   int incrementAndGet(): 以原子方式将当前值加 1，返回的是新值。
    *   int getAndDecrement(): 以原子方式将当前值减 1，返回的是旧值。
    *   int decrementAndGet(): 以原子方式将当前值减 1，返回的是新值。
    *   int getAndAdd(int delta): 以原子方式将当前值加上 delta，返回的是旧值。
    *   int addAndGet(int delta): 以原子方式将当前值加上 delta，返回的是新值。
*   **其他方法：**  
    *    int getAndUpdate(IntUnaryOperator updateFunction): 获取当前值，并按更新函数计算新值设置。
    *   int updateAndGet(IntUnaryOperator updateFunction): 按更新函数计算新值设置，并返回新值。
    *   int getAndAccumulate(int x, IntBinaryOperator accumulatorFunction): 获取当前值，并按累加函数计算新值设置。
    *   int accumulateAndGet(int x, IntBinaryOperator accumulatorFunction): 按累加函数计算新值设置，并返回新值。 

**验证原子类的线程安全：**

```
/**
 * @Author: vince
 * @CreateTime: 2024/06/27
 * @Description: 测试类
 * @Version: 1.0
 */
 
public class Test {
 
    public static int num=0;
    public static void main(String[] args) throws InterruptedException {
        AtomicInteger atomicInteger = new AtomicInteger(0);
 
        // 创建10个线程，分别对atomicInteger进行操作
        for (int i = 0; i < 10; i++) {
            new Thread(() -> {
                for (int j = 0; j < 10000; j++) {
                    atomicInteger.incrementAndGet();
                    num++;
                }
            }).start();
        }
        // 阻塞主线程1s，保证10个线程执行完毕
        Thread.sleep(1000);
        System.out.println(atomicInteger);
        System.out.println(num);
    }
}
```

可以看到原子类正常加到 100000，而 num 没有：

![](<assets/1740013966749.png>) 

#### 11.4.3 **volatile 关键字**

volatile 是一个关键字，被 volatile 声明的变量存在共享内存中，所有线程要读取、修改这个变量，都是从内存中读取、修改，并且修改操作是原子性的，所以它能保证线程安全。

**volatile** **特性：**

*   **有序性：**被 volatile 声明的变量之前的代码一定会比它先执行，而之后的代码一定会比它慢执行。底层是在生成字节码文件时，在指令序列中插入内存屏障防止指令重排序。
*   **可见性：**一旦修改变量则立即刷新到共享内存中，当其他线程要读取这个变量的时候，最终会去内存中读取，而不是从自己的工作空间中读取。每个线程自己的工作空间用于存放堆栈（存方法的参数和返回地址）和局部变量。
*   **原子性：**volatile 变量不能保证完全的原子性，只能保证单次的读 / 写操作具有原子性（在同一时刻只能被一个线程访问和修改），自增减、复合操作（+=,/= 等）则不具有原子性。这也是和 synchronized 的区别。

**读写内存语义：**

*   **写内存语义：**当写一个 volatile 变量时, JMM（Java 内存模型）会把该线程本地内存中的共享变量的值刷新到主内存中。
*   **读内存语义：**当读一个 volatile 变量时, JMM 会把该线程本地内存置为无效, 使其从主内存中读取共享变量。

**有序性实现机制：**

volatile 有序性是通过内存屏障来实现的。内存屏障就是在编译器生成字节码时，会在指令序列中插入内存屏障来禁止特定类型的处理器重排序。

**机器指令：**JVM 包括类加载子系统、运行时数据区、执行引擎。 执行引擎负责将字节码指令转为操作系统能识别的本地机器指令。

**指令重排序：**处理器为了提高运算速度会对指令重排序，重排序分三种类型：编译器优化重排序、处理器指令级并行重排序、内存系统重排序。 

*   **编译器优化的重排序：**编译器在不改变单线程程序的语义前提下，可以重新安排语句的执行顺序。
*   **指令级并行的重排序：**现在处理器采用了指令集并行技术，将多条指令重叠执行。如果不存在依赖性，处理器可以改变语句对应的机器指令的执行顺序。
*   **内存系统的重排序：**由于处理器使用缓存和读写缓冲区，这使得加载和存储操作看上去可能是在乱序执行。

#### 11.4.4 **锁**

加锁的方式有两种, 分别是 synchronized 关键字和 Lock 接口（在 JUC 包下）。

**synchronized 锁**是互斥锁，可以作用于实例方法、静态方法、代码块，能够保证同一个时刻只有一个线程执行该段代码，保证线程安全。 在执行完或者出现异常时自动释放锁。synchronized 锁基于对象头和 Monitor 对象，在 1.6 之后引入轻量级锁、偏向锁等优化。  

**lock 锁**接口可以通过 lock、unlock 方法锁住一段代码，Lock 实现类都是基于 AQS 实现的。Lock 可以让等待锁的线程响应中断，而 synchronized 却不行，使用 synchronized 时，等待的线程会一直等待下去，不能够响应中断。

#### 11.4.5 **线程安全的集合**

1.  **Collections 工具类：**Collections 工具类的 synchronizedXxx() 方法**，**将 ArrayList 等集合类包装成线程安全的集合类。
2.  **古老 api：**java.util 包下性能差的古老 api，如 Vector、Hashtable
3.  **降低锁粒度的并发容器：**JUC 包下 Concurrent 开头的、以降低锁粒度来提高并发性能的容器，如 ConcurrentHashMap。
4.  **复制技术实现的并发容器：**JUC 包下以 CopyOnWrite 开头的、采用写时复制技术实现的并发容器，如 CopyOnWriteArrayList。 

### 11.5  线程同步

#### 11.5.1 基本介绍

多条语句共享数据时，多线程程序会出现**数据安全问题**。

**线程同步：**即当有一个线程在对内存进行操作时，其他线程都不可以对这个内存地址进行操作，直到该线程完成操作， 其他线程才能对该内存地址进行操作，而其他线程又处于等待状态。

Java 主要通过加锁的方式实现线程同步, 而锁有两类, 分别是 synchronized 关键字和 Lock 接口（在 JUC 包下）。

**synchronized 锁**是互斥锁，可以作用于实例方法、静态方法、代码块，能够保证同一个时刻只有一个线程执行该段代码，保证线程安全。 在执行完或者出现异常时自动释放锁。synchronized 锁基于对象头和 Monitor 对象，在 1.6 之后引入轻量级锁、偏向锁等优化。  

**lock 锁**接口可以通过 lock、unlock 方法锁住一段代码，Lock 实现类都是基于 AQS 实现的。**Lock 可以让等待锁的线程响应中断，而 synchronized 却不行**，使用 synchronized 时，等待的线程会一直等待下去，不能够响应中断。

**对比线程安全和线程同步：**线程同步是实现线程安全的一种手段

*   **线程安全：**程序在多线程环境下可以持续进行正确的处理，不会产生数据竞争（例如死锁）和不一致的问题。解决方案：原子类、volatile、锁、线程安全的集合
*   **线程同步：**确保多个线程正确、有序地访问共享资源。解决方案：锁

#### 11.5.2 **synchronized 锁**

##### 11.5.2.1 基本介绍

**synchronized 锁：** 

synchronized 锁是互斥锁，可以作用于实例方法、静态方法、代码块，能够保证同一个时刻只有一个线程执行该段代码，保证线程安全。 在执行完或者出现异常时自动释放锁。synchronized 锁基于对象头、CAS、Monitor 对象，在 1.6 之后引入轻量级锁、偏向锁等优化。   

**作用于三个位置：**

 1. 作用在静态方法上, 则锁是当前类的 Class 对象。

 2. 作用在普通方法上, 则锁是当前的实例（this）。

 3. 作用在代码块上, 则需要在关键字后面的小括号里, 显式指定锁对象，例如 this、Xxx.class。

##### 11.5.2.2 同步代码块

同步代码块作用在代码块上, 则需要在关键字后面的小括号里, 显式指定锁对象，例如 this、Xxx.class。

同步代码块简单来说就是将一段代码用一把锁给锁起来, 只有获得了这把锁的线程才访问, 并且同一时刻, 只有一个线程能持有这把锁, 这样就保证了同一时刻只有一个线程能执行被锁住的代码.

```
    synchronized(同步对象) {
        //多条语句操作共享数据的代码 
    }
```

​

**同步代码块的好处：**解决了多线程的数据安全问题

**弊端：**线程很多时，每个线程都会去判断锁，这是很耗费资源和时间的。

**代码示例：**共有 100 张票，三个窗口卖票，通过加锁防止超卖

```
public class SellTicket implements Runnable {
    private int tickets = 100;
    private final Object obj = new Object();
 
    @Override
    public void run() {
        while (true) {
            synchronized (obj) {
                if (tickets > 0) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + " 正在出售第 " + tickets + " 张票");
                    tickets--;
                } else {
                    break;
                }
            }
        }
    }
 
    public static void main(String[] args) {
        SellTicket sellTicket = new SellTicket();
 
        Thread t1 = new Thread(sellTicket, "窗口1");
        Thread t2 = new Thread(sellTicket, "窗口2");
        Thread t3 = new Thread(sellTicket, "窗口3");
 
        t1.start();
        t2.start();
        t3.start();
    }
}
```

##### 11.5.2.3 同步方法

 1. 作用在静态方法上, 则锁是当前类的 Class 对象。

 2. 作用在普通方法上, 则锁是当前的实例（this）。

**非静态同步方法的锁对象为 this**。下面代码是相同功能的同步方法和同步代码块：

**代码示例：**

锁的粒度是当前对象： 

```
    // 方法1：实例方法，使用this对象锁
    private void sellTicket1() {
        synchronized (this) {
            if (tickets > 0) {
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + " 正在出售第 " + tickets + " 张票");
                tickets--;
            }
        }
    }
 
    // 方法2：实例方法，使用this对象锁
    private void sellTicket2() {
        synchronized (this) {
            if (tickets > 0) {
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + " 正在出售第 " + tickets + " 张票");
                tickets--;
            }
        }
    }
 
 
```

锁的粒度是真个类：

静态同步方法的锁对象为：类名. class。下面代码是相同功能的同步方法和同步代码块 

```
    // 方法3：静态方法，使用类对象锁
    private static synchronized void sellTicket3() {
        if (tickets > 0) {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + " 正在出售第 " + tickets + " 张票");
            tickets--;
        }
    }
 
    // 方法4：静态方法，使用类对象锁
    private static void sellTicket4() {
        synchronized (SellTicket.class) {
            if (tickets > 0) {
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + " 正在出售第 " + tickets + " 张票");
                tickets--;
            }
        }
    }
```

##### 11.5.2.4 知识加油站：**synchronized 锁的**原理

**synchronized 锁：** 

synchronized 锁是互斥锁，可以作用于实例方法、静态方法、代码块，能够保证同一个时刻只有一个线程执行该段代码，保证线程安全。 在执行完或者出现异常时自动释放锁。synchronized 锁基于对象头、CAS、Monitor 对象，在 1.6 之后引入轻量级锁、偏向锁等优化。   

**作用于三个位置：**

 1. 作用在静态方法上, 则锁是当前类的 Class 对象。

 2. 作用在普通方法上, 则锁是当前的实例（this）。

 3. 作用在代码块上, 则需要在关键字后面的小括号里, 显式指定锁对象，例如 this、Xxx.class。

**对象头存储锁信息：** synchronized 的底层是采用对象头的 Mark Word 来存储锁信息的。Hotspot 虚拟机（JVM 默认虚拟机）中每个对象都有一个对象头 (Object Header)，包含 Mark Word（标记字段） 和 Class Pointer（类型指针）。

*   **Mark Word（标记字段）：**存储哈希码、GC 分代年龄、锁信息、GC 标记（标志位，标记可达对象或垃圾对象）等。**锁信息包括：**
    *   **锁标志位：**64 位的二进制数，通过末尾能判断锁状态。01 未锁定、01 可偏向、00 轻量级锁、10 重量级锁、11 垃圾回收标记
    *   **偏向锁线程 ID、时间戳等；**
    *   **轻量级锁的指针：**指向锁记录的指针
    *   **重量级锁的指针：**指向 Monitor 锁的指针
*   **类型指针：**指向它的类元数据的指针，用于找到对象所在的类。  

不考虑共享资源是类变量等特殊情况的话，有共享资源的多个线程通常都属于同一个对象。  

![](<assets/1740013966951.png>)

**Monitor 对象：**每个 Java 对象都可以关联一个 Monitor 对象，也称为监视器锁或 Monitor 锁。Monitor 锁用于控制线程对共享资源的访问，开发人员不能直接访问 Monitor 对象。当一个线程获取了 Monitor 的锁后，其他试图获取该锁的线程就会被阻塞，直到当前线程释放锁为止。

当一个线程执行 synchronized 方法或代码块并升级成重量级锁时，当前对象会关联一个 Monitor 对象，线程必须先获得该对象的 Monitor 锁才能执行。Monitor 有 Owner、EntryList、WaitSet 三个字段，分别表示 Monitor 的持有者线程 (获得锁的线程)、阻塞队列、和等待队列。

**线程通信：**synchronized 通过 Monitor 对象，利用 Object 的 wait，notify，notifyAll 等方法来实现线程通信。

**锁升级：**JDK6 之前 synchronized 只有无锁和重量级锁两个状态，JDK6 引入偏向锁、轻量级锁两个状态，锁可以根据竞争程度从无锁状态慢慢升级到重量级锁。当竞争小的时候，只需以较小的代价加锁，直到竞争加剧，才使用重量级锁，从而减小了加锁带来的开销。

*   **锁升级顺序：**无锁状态、偏向锁状态、轻量级锁状态、重量级锁状态。
*   **无锁：**没有线程访问同步代码块时。没有对资源进行锁定，所有的线程都能访问并不断修改同一个资源，但同时只有一个线程能修改成功，失败线程会不断重试。
*   **偏向锁：**当有一个线程访问同步代码块时升级为偏向锁。一段同步代码块一直被一个线程所访问，那么该线程 id 会 CAS 写入对象头，下次再访问同步代码块时对象头检查到该线程 id，就不用加锁解锁了，降低获取锁的代价。
*   **轻量级锁（自旋锁）：**有锁竞争时升级为轻量级锁。其他线程会通过自旋的形式尝试通过 CAS 将对象头中 Mark Word 替换为指向线程栈帧里锁记录的指针，从而获得锁；同时线程锁记录里存放 Mark Word 信息。竞争的线程不会阻塞但会一直自旋，消耗 CPU 性能，但速度快。
*   **重量级锁：**锁膨胀（自旋失败 10 次）时升级为重量级锁。Mark Word 中存储的是指向 Monitor 锁的指针，对象 Mark Word 信息也会保存在 Monitor 锁里，当一个线程获取了 Monitor 锁后，竞争线程会被阻塞，不再自旋，不消耗 CPU，速度慢。

![](<assets/1740013967203.png>)

#### 11.5.3 Lock 锁

Lock 提供比同步方法和代码块更广泛的锁定操作。

*   **lock()：**获取锁。如果锁不可用，则当前线程将被禁用，直到获取锁为止。
*   **tryLock()：**尝试获取锁，如果锁可用，则获取并立即返回 true；如果锁不可用，则立即返回 false，不会等待。
*   **tryLock(long time, TimeUnit unit)：**尝试在指定的时间内获取锁。如果锁可用，则获取并立即返回 true；如果在指定时间内锁不可用，则等待直到超时，然后返回 false。
*   **unlock()：**释放锁。
*   **newCondition()：**返回一个绑定到此 Lock 实例的新 Condition 实例，可以用于线程之间的协调等待。

**代码示例：** 

```
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
 
public class SellTicket implements Runnable {
    private int tickets = 100;
    private Lock lock = new ReentrantLock();
 
    @Override
    public void run() {
        while (true) {
            try {
                lock.lock();
                if (tickets > 0) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + " 正在出售第 " + tickets + " 张票");
                    tickets--;
                } else {
                    break;
                }
            } finally {
                lock.unlock();
            }
        }
    }
 
    public static void main(String[] args) {
        SellTicket sellTicket = new SellTicket();
        Thread t1 = new Thread(sellTicket);
        Thread t2 = new Thread(sellTicket);
        Thread t3 = new Thread(sellTicket);
 
        t1.start();
        t2.start();
        t3.start();
    }
}
```

​

#### 11.5.4 synchronized 和 Lock 的区别

Lock 和 synchronized 有以下几点不同：

*   **接口和关键字。**Lock 是一个接口，而 synchronized 是 Java 中的关键字，synchronized 是内置的语言实现；
*   **死锁问题。**synchronized 在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而 Lock 在发生异常时，如果没有主动通过 unLock() 去释放锁，则很可能造成死锁现象，因此使用 Lock 时需要在 finally 块中释放锁；
*   **让等待锁的线程响应中断。**Lock 可以可以通过 lockInterruptibly() 获取锁的方法让等待锁的线程响应中断。而 synchronized 却不行，使用 synchronized 时，等待的线程会一直等待下去，不能够响应中断；
*   **得知是否成功获取锁。**通过 Lock 可以通过 tryLock() 知道有没有成功获取锁，而 synchronized 却无法办到。
*   **性能对比。**两者性能差不多。JDK6 之前 synchronized 没有锁升级，线程竞争非常激烈时 Lock 的性能要远远优于 synchronized；而 JDK6 引入锁升级后，线程竞争激烈时候两者性能也相差无几。

**lock 锁中断线程：**若有线程已拿到锁，其他线程使用 lock() 获取锁时会阻塞，使用 lockInterruptibly() 获取锁时会直接中断抛出 InterruptedException 异常

**lock 锁编码习惯：**加锁代码要放到 try 外面。如果放在 try 里面的话，加锁失败抛异常或者加锁前的代码抛异常后，执行 finally 里的解锁代码，而其实加锁都没成功，最终解锁就也不合适了。

```
lock.lock(); // 加锁
try{
    // do something
}finally{
    lock.unlock(); // 解锁
}
 
//不推荐
try{
int a=3/0;//这里抛异常会直接进入finally
lock.lock(); // 加锁
    // do something
}finally{
    lock.unlock(); // 解锁
}
```

**分布式锁：**SETNX、Redisson。Redisson 基于 Redis 协议，可以实现可重入锁、公平锁、读写锁、信号量、闭锁（计数器），支持看门狗自动续期。

## 十二、 反射

### 12.1 基本介绍

**反射：**在程序运行期间**动态地获取类的信息并对类进行操作**的机制。

**通过反射机制可以实现：**

*   **获取类或对象的 Class 对象：**程序运行时, 可以通过反射获得任意一个类的 Class 对象, 并通过这个对象查看这个类的所有方法和属性（包括私有，私有需要给该字段调用 setAccessible(true) 方法开启私有权限）。注意类的 class 对象是运行时生成的，类的 class 字节码文件是编译时生成的。
*   **创建实例：**程序运行时, 可以利用反射先创建类的 Class 对象再创建该类的实例, 并访问该实例的成员；Xxx.class.newInstance() ; 例如在 Spring 容器类源码里，Bean 实例化就是通过 Bean 类的 Class 对象。Bean 类的 Class 对象是从 BeanDefinition 对象的 type 成员变量取的。BeanDefinition 对象存储一些 Bean 的类型、名称、作用域等声明信息。
*   **生成动态代理类或对象：**程序运行时, 可以通过反射机制生成一个类的动态代理类或动态代理对象。例如 JDK 中 Proxy 类的 newProxyInstance 静态方法，可以通过它创建基于接口的动态代理对象。

**类的字节码文件和 Class 对象的区别：**

*   类的 class 字节码文件是编译时生成的，类的 class 对象是运行时生成的。
*   类的字节码文件是一个存储在电脑硬盘中的文件，例如 Test.class；类的 Class 对象是存放在内存中的数据，可以快速获取其中的信息；
*   两者都存储类的各种信息；

**获取类 Class 对象的 JVM 底层：**如果该类没有被加载过，会首先通过 JVM 实现类的加载过程，即加载、链接（验证、准备、解析）、初始化，加载阶段会生成类的 Class 对象。

**获取类 Class 对象的方法：**dog.getClass();Dog.class;Class.forName("package1.Dog");

**特点：**

*   **访问私有成员：**构造方法、成员变量、方法对象取消访问检查可以访问私有成员；public void setAccessible(boolean flag): 值为 true，取消访问检查
*   **越过泛型检查：**反射可以越过泛型检查，例如在 ArrayList<Integer> 中添加字符串

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

**验证反射可以绕过泛型检查：**

基于反射，我们可以给 ArrayList<Integer> 对象中，加入字符串

```
public class Test {
    /**
     * 测试方法，实际场景建议try-catch,而不是throws
     * @param args
     * @throws NoSuchMethodException
     * @throws InvocationTargetException
     * @throws IllegalAccessException
     */
    public static void main(String[] args) throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {
        ArrayList<Integer> integers = new ArrayList<>();
        Class<? extends ArrayList> aClass = integers.getClass();
        Method add = aClass.getMethod("add", Object.class);
        add.invoke(integers, 1);
        add.invoke(integers, 2);
        add.invoke(integers, 3);
        add.invoke(integers, 4);
        add.invoke(integers, "hello");
        System.out.println(integers);
    }
}
```

![](<assets/1740013967523.png>)

### 12.2 反射获取 Class 对象

#### 12.2.1 基本介绍

**Class 类的对象：**程序运行时, 可以通过反射获得任意一个类的 Class 对象, 并通过这个对象查看这个类的所有方法和属性（包括私有，私有需要给该字段调用 setAccessible(true) 方法开启私有权限）。

注意类的 class 对象是运行时生成的，类的 class 字节码文件是编译时生成的。

**获取类 Class 对象：**

*   **对象. getClass()：**Object 是一切类的根类，Object 类有个 getClass() 方法可以获取类的 Class 对象。例如 dog.getClass();
*   **类名. class（推荐）：**例如 Dog.class;
*   **Class.forName("类名")：**例如 Class.forName("package1.Dog");

**Class 对象的常用方法：**

*   **获取类的信息：**
    *   **String getName()：**返回类的全限定名。**全限定名**包含包名和类名，用于唯一标识类或接口。例如 package1.Dog、java.lang.String、java.util.Map$Entry
    *   **String getSimpleName()：**返回类的简单名。例如 Dog
    *   **String getCanonicalName()：**返回类的规范名。**规范名**是类的规范字符串形式，常用于打印和日志记录。例如 package1.Dog、java.lang.String、java.util.Map.Entry
    *   **Package getPackage()：**返回此类所属的包。
    *   ClassLoader getClassLoader()：返回该类的类加载器。
    *   Class<? super T> getSuperclass()：返回表示类的超类的 Class 对象。
    *   Class<?>[] getInterfaces()：返回类实现的所有接口。
    *   **boolean isInterface()：**判断是否是接口。
    *   boolean isAnnotation()：判断是否是注解类型。
    *   boolean isEnum()：判断是否是枚举类型。
    *   Annotation[] getAnnotations()：返回此元素上存在的所有注解。
    *   Annotation[] getDeclaredAnnotations()：返回直接存在于此元素上的所有注解。
    *   T getAnnotation(Class<T> annotationClass)：返回指定类型的注解，如果该注解存在于此元素上，否则返回 null。例如 Spring 源码中，ApplicaitonContext 构造器判断一个类是不是 Bean，是通过这个方法判断类有没有 @Comonent 等注解，从而判断它是不是 Bean。
*   **获取成员：**
    *   **Field[] getFields()：**返回类的所有公共字段，包括从父类继承的字段。
    *   **Field[] getDeclaredFields()：**返回类声明的所有字段，不包括继承的字段。
    *   **Method[] getMethods()：**返回类的所有公共方法，包括从父类继承的方法。
    *   **Method[] getDeclaredMethods()：**返回类声明的所有方法，不包括继承的方法。
    *   **Constructor<?>[] getConstructors()：**返回类的所有公共构造方法。
    *   **Constructor<?>[] getDeclaredConstructors()：**返回类声明的所有构造方法。
*   **其他方法：**
    *   **T newInstance()：**创建此 Class 对象所表示的类的一个新实例（使用默认构造方法）。

**Spring 源码：**Bean 初始化时判断类是否 Bean、判断属性是否需要填充都用到了反射

[Spring 框架中 Bean 是如何加载的？从底层源码入手，详细解读 Bean 的创建流程 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/137231482 "Spring框架中Bean是如何加载的？从底层源码入手，详细解读Bean的创建流程-CSDN博客")

**代码示例：**

准备 Dog 类

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
    private int weight;
    /**
     * 名字
     */
    public String name;
 
    public Dog() {
    }
 
    public int getWeight() {
        return weight;
    }
 
    public void setWeight(int weight) {
        this.weight = weight;
    }
 
    public String getName() {
        return name;
    }
 
    public void setName(String name) {
        this.name = name;
    }
 
    public Dog(int weight, String name) {
        this.weight = weight;
        this.name = name;
    }
 
    @Override
    public String toString() {
        return "Dog{" +
                "weight=" + weight +
                ", name='" + name + '\'' +
                '}';
    }
}
```

**获取类的 Class 对象：** 

```
public class Test {
    /**
     * 测试方法，实际场景建议try-catch,而不是throws
     * @param args
     */
    public static void main(String[] args) throws ClassNotFoundException {
        //方法1：类的class属性
        Class<Dog> c1=Dog.class;
        //方法2：对象的getClass方法
        Dog wangCaiDog = new Dog(23, "旺财");
        Class<? extends Dog> c2= wangCaiDog.getClass();
        //方法3：Class类的静态方法forName
        Class<?> c3= Class.forName("package1.Dog");
        // 方法4：使用类加载器
        ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
        Class<?> c5 = systemClassLoader.loadClass("package1.Dog");
        // 输出：class package1.Dog
        System.out.println(c1);
        //三种方式获取到Class对象地址是完全一致的
        // 输出：true
        System.out.println(c1==c2&&c1==c3);
    }
}
```

![](<assets/1740013967767.png>)

获取类的信息：

```
/**
 * @Author: vince
 * @CreateTime: 2024/06/27
 * @Description: 测试类
 * @Version: 1.0
 */
 
public class Test {
    /**
     * 测试方法，实际场景建议try-catch,而不是throws
     *
     * @param args
     */
    public static void main(String[] args) throws Exceptionn {
        Class<? extends Dog> dogClass = Dog.class;
        System.out.println(dogClass.getName());
        System.out.println(dogClass.getSimpleName());
        System.out.println(dogClass.getCanonicalName());
    }
}
```

![](<assets/1740013968225.png>)

#### 12.2.2 全限定名和规范名

**全限定名和规范名：**

外部类的全限定名和规范名是一样的，都是 “xxx. 类名”。区别主要在内部类，内部类的全限定名是 “xxx. 外部类名 $ 内部类名”，规范名是 “xxx. 外部类名. 内部类名”。

*   **简单名：**只包含类名。例如 Dog、String、Entry
*   **全限定名：**包含包名和类名，用于唯一标识类或接口，通过全限定名能找到唯一一个类。例如 package1.Dog、java.lang.String、java.util.Map$Entry
*   **规范名：**类的规范字符串形式，常用于打印和日志记录。例如 package1.Dog、java.lang.String、java.util.Map.Entry

**代码示例：**

```
    public static void main(String[] args) throws Exception {
        // java.util.Map
        System.out.println(Map.class.getName());
        // java.util.Map
        System.out.println(Map.class.getCanonicalName());
        // 输出 "java.util.Map$Entry"
        System.out.println(Map.Entry.class.getName());
        // 输出 "java.util.Map.Entry"
        System.out.println(Map.Entry.class.getCanonicalName());
 
    }
```

![](<assets/1740013968490.png>)

### 12.3 反射获取成员

#### 12.3.1 反射获取构造方法

**Class 对象获取构造器：**

*   **getConstructor(Class<?>... parameterTypes)：**获取指定参数类型的公共构造方法。返回值是 **Constructor 类**。
*   **getDeclaredConstructor(Class<?>... parameterTypes)：**获取指定参数类型的构造方法（包括私有构造方法）。
*   **getConstructors()：**获取所有公共构造方法。
*   **getDeclaredConstructors()：**获取所有构造方法（包括私有构造方法）。
*   **newInstance()：**创建类的新实例。
    *   **Class 类的 newInstance()：**只能够调用无参构造函数；
    *   **Constructor 类的 newInstance()：**可以根据传入的参数，调用任意构造函数。

Constructor 译作构造方法，构造器。

**代码示例：**

**获取所有构造器对象：**

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
    private int weight;
    /**
     * 名字
     */
    public String name;
 
    public Dog() {
    }
 
    public int getWeight() {
        return weight;
    }
 
    public void setWeight(int weight) {
        this.weight = weight;
    }
 
    public String getName() {
        return name;
    }
 
    public void setName(String name) {
        this.name = name;
    }
 
    public Dog(int weight, String name) {
        this.weight = weight;
        this.name = name;
    }
 
    @Override
    public String toString() {
        return "Dog{" +
                "weight=" + weight +
                ", name='" + name + '\'' +
                '}';
    }
}
```

```
    public static void main(String[] args) {
        Class<Dog> dogClass=Dog.class;
        Constructor<?>[] cons = dogClass.getDeclaredConstructors();
        for(Constructor<?> con:cons){
            System.out.println(con);
        }
    }
```

![](<assets/1740013968644.png>)

​​

 **获取单个构造器并实例化：**

无参：

```
/**
 * @Author: vince
 * @CreateTime: 2024/06/27
 * @Description: 测试类
 * @Version: 1.0
 */
 
public class Test {
    /**
     * 测试方法，实际场景建议try-catch,而不是throws
     * @param args
     */
    public static void main(String[] args) throws InvocationTargetException, InstantiationException, IllegalAccessException, NoSuchMethodException {
        Class<Dog> dogClass=Dog.class;
        //获取单个构造方法对象
        Constructor<?> con=dogClass.getDeclaredConstructor();
        //构造方法对象实例化，会调用无参构造方法
        Object dogObject = con.newInstance();
        // 无参构造器实例化，也可以直接用Class对象的newInstance方法，带参就不行了
        Dog dog = dogClass.newInstance();
        //重写了Dog类的to_String，所以输出：Dog{weight=0, name='null'}
        System.out.println(dogObject);
        System.out.println(dog);
    }
}
```

![](<assets/1740013968834.png>)

带参：

```
/**
 * @Author: vince
 * @CreateTime: 2024/06/27
 * @Description: 测试类
 * @Version: 1.0
 */
 
public class Test {
    /**
     * 测试方法，实际场景建议try-catch,而不是throws
     * @param args
     */
    public static void main(String[] args) throws InvocationTargetException, InstantiationException, IllegalAccessException, NoSuchMethodException {
        Class<Dog> dogClass=Dog.class;
        Constructor<?> con=dogClass.getConstructor(int.class,String.class);
        Object obj = con.newInstance(32,"旺财");
        System.out.println(obj);
    }
}
```

![](<assets/1740013969004.png>)

#### 12.3.2 反射获取字段

**Class 对象获取字段：**

*   getField(String name)：返回指定名称的公共字段。返回类型是字段类 Field。
*   **getDeclaredField(String name)：**返回指定名称的字段**（包括私有字段）**。
*   getFields()：返回所有公共字段。
*   **getDeclaredFields()：**返回所有字段**（包括私有字段）**。

**字段类 Field 常用方法：**

**获取字段信息**：

*   **getName()：**返回字段的名称。
*   **getType()：**返回字段的类型。
*   **getModifiers()：**返回字段的修饰符。
*   **getDeclaringClass()：**返回声明该字段的类的 Class 对象。

**获取和设置字段值**：

*   **get(Object obj)：**返回指定对象上此字段的值。
*   **getBoolean(Object obj)：**返回指定对象上此字段的值（如果字段类型是 boolean）。
*   getByte(Object obj)：返回指定对象上此字段的值（如果字段类型是 byte）。
*   getChar(Object obj)：返回指定对象上此字段的值（如果字段类型是 char）。
*   getDouble(Object obj)：返回指定对象上此字段的值（如果字段类型是 double）。
*   getFloat(Object obj)：返回指定对象上此字段的值（如果字段类型是 float）。
*   getInt(Object obj)：返回指定对象上此字段的值（如果字段类型是 int）。
*   getLong(Object obj)：返回指定对象上此字段的值（如果字段类型是 long）。
*   getShort(Object obj)：返回指定对象上此字段的值（如果字段类型是 short）。
*   **set(Object obj, Object value)：**设置指定对象上此字段的值。注意私有字段默认不允许赋值，要赋值必须给私有字段 setAccessible(true)
*   **setBoolean(Object obj, boolean value)：**设置指定对象上此字段的值（如果字段类型是 boolean）。
*   setByte(Object obj, byte value)：设置指定对象上此字段的值（如果字段类型是 byte）。
*   setChar(Object obj, char value)：设置指定对象上此字段的值（如果字段类型是 char）。
*   setDouble(Object obj, double value)：设置指定对象上此字段的值（如果字段类型是 double）。
*   setFloat(Object obj, float value)：设置指定对象上此字段的值（如果字段类型是 float）。
*   setInt(Object obj, int value)：设置指定对象上此字段的值（如果字段类型是 int）。
*   setLong(Object obj, long value)：设置指定对象上此字段的值（如果字段类型是 long）。
*   setShort(Object obj, short value)：设置指定对象上此字段的值（如果字段类型是 short）。

**其他方法**：

*   **isAccessible()：**返回字段是否可访问。
*   **setAccessible(boolean flag)：**设置字段的可访问性。通过这个方法可以让私有字段也可以赋值。
*   toGenericString()：返回字段的描述，包括泛型信息。
*   getAnnotatedType()：返回此字段的带注释的类型。
*   getAnnotations()：返回字段的所有注解。
*   **getAnnotation(Class<T> annotationClass)：**返回字段的指定类型的注解，如果该注解不存在，则返回 null。例如 Spring 源码中依赖注入这一块，就是基于反射获取类中字段有没有 @Resource、@Component 等注解，有的话就是要注入 Bean.
*   getDeclaredAnnotations()：返回直接存在于此字段上的所有注解。

**Spring 源码：**Bean 初始化时判断类是否 Bean、判断属性是否需要填充都用到了反射

[Spring 框架中 Bean 是如何加载的？从底层源码入手，详细解读 Bean 的创建流程 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/137231482 "Spring框架中Bean是如何加载的？从底层源码入手，详细解读Bean的创建流程-CSDN博客")

**代码示例：**

准备 Dog 类

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
    private int weight;
    /**
     * 名字
     */
    public String name;
 
    public Dog() {
    }
 
    public int getWeight() {
        return weight;
    }
 
    public void setWeight(int weight) {
        this.weight = weight;
    }
 
    public String getName() {
        return name;
    }
 
    public void setName(String name) {
        this.name = name;
    }
 
    public Dog(int weight, String name) {
        this.weight = weight;
        this.name = name;
    }
 
    @Override
    public String toString() {
        return "Dog{" +
                "weight=" + weight +
                ", name='" + name + '\'' +
                '}';
    }
}
```

**获取成员变量对象并赋值：** 

```
/**
 * @Author: vince
 * @CreateTime: 2024/06/27
 * @Description: 测试类
 * @Version: 1.0
 */
 
public class Test {
    /**
     * 测试方法，实际场景建议try-catch,而不是throws
     *
     * @param args
     */
    public static void main(String[] args) throws NoSuchFieldException, InstantiationException, IllegalAccessException {
        // 1.获取Class对象，并实例化
        Class<? extends Dog> dogClass = Dog.class;
        Dog dog = dogClass.newInstance();
        // 2.获取字段对象
        Field nameField = dogClass.getDeclaredField("name");
        Field weightField = dogClass.getDeclaredField("weight");
        // 3.给字段对象赋值
        nameField.set(dog, "旺财");
        // 注意私有字段默认不允许赋值，要赋值必须给私有字段设置可访问
        weightField.setAccessible(true);
        weightField.set(dog, 10);
        System.out.println(dog);
    }
}
```

可以看到私有、公有字段都赋值成功： 

![](<assets/1740013969362.png>)

**通过 Field 获取的 Class 类：**

```
/**
 * @Author: vince
 * @CreateTime: 2024/06/27
 * @Description: 测试类
 * @Version: 1.0
 */
 
public class Test {
    /**
     * 测试方法，实际场景建议try-catch,而不是throws
     * @param args
     */
    public static void main(String[] args) throws NoSuchFieldException {
        Class<Dog> dogClass=Dog.class;
        Field weightField = dogClass.getDeclaredField("name");
        Class<?> dogClassByField = weightField.getDeclaringClass();
        // 通过字段获取到的class对象和源class对象是地址是一样的，事实上一个类的所有Class对象都是一个实例
        // true
        System.out.println(dogClassByField==dogClass);
    }
}
```

![](<assets/1740013969575.png>)

#### 12.3.3 反射获取普通方法

Class 对象获取成员方法的方法： 

*   **getMethod(String name, Class<?>... parameterTypes)：**返回指定名称和参数类型的公共方法。返回值是**方法类 Method。**
*   **getDeclaredMethod(String name, Class<?>... parameterTypes)：**返回指定名称和参数类型的方法（包括私有方法）。
*   **getMethods()：**返回所有公共方法（包括从父类**继承的方法**）。
*   **getDeclaredMethods()：**返回所有方法（包括**私有方法**）。

**Method 类的方法：**

*   **获取方法信息**：
    *   getName()：返回方法的名称。
    *   getReturnType()：返回方法的返回类型。
    *   getParameterTypes()：返回方法参数类型的数组。
    *   getModifiers()：返回方法的修饰符。
    *   getDeclaringClass()：返回声明此方法的类的 Class 对象。
*   **调用方法**：
    *   **Object invoke(Object obj, Object... args)：**调用指定对象上此 Method 对象表示的基础方法。
*   **其他方法**：
    *   **isAccessible()：**返回方法是否可访问。
    *   **setAccessible(boolean flag)：**设置方法的可访问性。
    *   **getAnnotations()：**返回此方法的所有注解。例如 Spring 源码中通过此方法判断一个类中
    *   isAnnotationPresent(Class<? extends Annotation> annotationClass)：判断此方法是否被指定的注解类型注释。
    *   getAnnotation(Class<T> annotationClass)：返回该方法的指定类型的注解。
    *   getExceptionTypes()：返回此方法抛出的异常类型的数组。
    *   toGenericString()：返回方法的描述，包括泛型信息。

**获取成员变量对象并调用：**

```
//        1.获取构造方法对象并实例化
        Class<?> c= Class.forName("train.Dog");
        Constructor<?> con=c.getConstructor();
        Object obj = con.newInstance();
//        2.获取成员方法对象
        Method eat = c.getMethod("eat");
//        3.通过成员方法对象的invoke方法，调用构造方法对象的成员方法
//无参无返回值方法
        eat.invoke(obj);
//带参有返回值方法
        Object sucess= eat.invoke(obj,"food");
        System.out.println((boolean)sucess);
```