---
url: https://blog.csdn.net/qq_40991313/article/details/141138049
title: 什么是线程池？从底层源码入手，深度解析线程池的工作原理_java 线程池源码底层原理 - CSDN 博客
date: 2025-02-20 13:59:27
tag: 
summary: 
---
**导航：** 

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[一、什么是线程池？](#t0)

[1.1 基本介绍](#t1)

[1.2 创建线程的两种方式](#t2)

[1.2.1 方式 1：自定义线程池（推荐）](#t3)

[1.2.2 方式 2：线程池工具类](#t4)

[二、ThreadPoolExecutor 源码解析](#t5)

[2.1 基本介绍](#t6) 

[2.2 扩展：ExecutorService 接口](#t7)

[2.3 线程池参数](#t8)

[2.3.1 ThreadPoolExecutor 的七个参数](#t9)

[2.3.2 如何为线程池设置合适的线程数](#t10)

[2.3.3 代码示例](#t11)

[2.3.4 线程池的初始化](#t12)

[2.3.4.1 不指定线程工厂的构造方法](#2.3.4.1%C2%A0%E4%B8%8D%E6%8C%87%E5%AE%9A%E7%BA%BF%E7%A8%8B%E5%B7%A5%E5%8E%82%E7%9A%84%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95)

[2.3.4.2 全参数核心构造方法](#2.3.4.2%20%E5%85%A8%E5%8F%82%E6%95%B0%E6%A0%B8%E5%BF%83%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95)

[2.3.5 拒绝策略详解](#t13)

[2.3.5.0 概述](#2.3.5.0%C2%A0%E6%A6%82%E8%BF%B0)

[2.3.5.1 AbortPolicy：抛异常](#2.3.5.1%C2%A0AbortPolicy%EF%BC%9A%E6%8A%9B%E5%BC%82%E5%B8%B8)

[2.3.5.2 CallerRunsPolicy：调用线程 run() 方法](#2.3.5.2%20CallerRunsPolicy%EF%BC%9A%E8%B0%83%E7%94%A8%E7%BA%BF%E7%A8%8Brun%28%29%E6%96%B9%E6%B3%95)

[2.3.5.3 DiscardOldestPolicy：丢弃最老任务](#2.3.5.3%20DiscardOldestPolicy%EF%BC%9A%E4%B8%A2%E5%BC%83%E6%9C%80%E8%80%81%E4%BB%BB%E5%8A%A1)

[2.3.5.4 DiscardPolicy：丢弃新任务](#2.3.5.4%20DiscardPolicy%EF%BC%9A%E4%B8%A2%E5%BC%83%E6%96%B0%E4%BB%BB%E5%8A%A1)

[2.3.5.5 自定义拒绝策略](#2.3.5.5%20%E8%87%AA%E5%AE%9A%E4%B9%89%E6%8B%92%E7%BB%9D%E7%AD%96%E7%95%A5)

[2.4 核心字段和内部类](#t14)

[2.4.1 基本介绍](#t15)

[2.4.2 ctl：线程池当前状态和数量](#t16)

[2.4.3 COUNT_BITS 和 CAPACITY：线程数上限](#t17)

[2.4.4 五个状态字段：线程池的生命周期](#t18) 

[2.4.5 扩展：线程的生命周期  ​](#t19)

[2.4.6 workers 和 mainLock：维护所有工作线程](#t20)

[2.4.7 Worker 类：控制中断线程、执行 runWorker() 方法](#t21)

[2.4.8 allowCoreThreadTimeOut：是否允许核心线程超时](#t22)

[2.5 execute()：启动线程](#t23)

[2.5.1 工作原理](#t24)

[2.5.2 源码解析](#t25)

[2.6 addWorker()：添加工作线程](#t26)

[2.7 runWorker ()：执行线程](#t27)

[2.8 getTask()：从队列获取任务](#t28)

[2.9 关闭](#t29)

[2.9.1 shutdown()：关闭线程](#t30)

[2.9.1.1 源码解析](#2.9.1.1%20%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90)

[2.9.1.2 校验关闭权限：checkShutdownAccess()](#2.9.1.2%20%E6%A0%A1%E9%AA%8C%E5%85%B3%E9%97%AD%E6%9D%83%E9%99%90%EF%BC%9AcheckShutdownAccess%28%29)

[2.9.1.3 修改线程池状态：advanceRunState()](#2.9.1.3%C2%A0%E4%BF%AE%E6%94%B9%E7%BA%BF%E7%A8%8B%E6%B1%A0%E7%8A%B6%E6%80%81%EF%BC%9AadvanceRunState%28%29)

[2.9.1.4 中断空闲线程：interruptIdleWorkers()](#2.9.1.4%C2%A0%E4%B8%AD%E6%96%AD%E7%A9%BA%E9%97%B2%E7%BA%BF%E7%A8%8B%EF%BC%9AinterruptIdleWorkers%28%29)

[2.9.1.5 尝试终止线程池：tryTerminate()](#2.9.1.5%20%E5%B0%9D%E8%AF%95%E7%BB%88%E6%AD%A2%E7%BA%BF%E7%A8%8B%E6%B1%A0%EF%BC%9AtryTerminate%28%29)

[2.9.2 shutdownNow()：停止线程](#t31)

[2.9.2.1 源码解析](#2.9.2.1%C2%A0%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90)

[2.9.2.2 清空阻塞队列：drainQueue()](#2.9.2.2%C2%A0%E6%B8%85%E7%A9%BA%E9%98%BB%E5%A1%9E%E9%98%9F%E5%88%97%EF%BC%9AdrainQueue%28%29)

[三、Executors 源码解析](#t32)

[3.1 Executors 工具类](#t33)

[3.2 newFixedThreadPool：固定大小的线程池](#t34)

[3.3 newCachedThreadPool：缓存线程池（无限大）](#t35)

[3.4 newScheduledThreadPool：定时任务线程池](#t36)

[3.5 newSingleThreadExecutor：单线程化的线程池](#t37)

## 一、什么是线程池？

### 1.1 基本介绍

为了对多线程进行统一的管理，Java 引入了线程池，它通过限制并发线程的数量、将待执行的线程放入队列、销毁空闲线程，来控制资源消耗，使线程更合理地运行，避免系统因为创建过多线程而崩溃。

**线程池作用：** 

*   **管理线程数量：**它可以管理线程的数量, 可以避免无节制的销毁、创建线程，导致额外的性格损耗、或者线程数超出系统负荷直至崩溃。
*   **提高性能：**当有新的任务到来时，可以直接从线程池中取出一个空闲线程来执行任务，而不需要等待创建新线程，从而减少了响应时间。
*   **让线程复用：**它还可以让线程复用, 可以大大地减少创建和销毁线程所带来的开销。
*   **合理的拒绝策略**：线程池提供了多种拒绝策略，当线程池队列满了时，可以采用不同的策略进行处理，如抛出异常、丢弃任务或调用者运行等。

### 1.2 创建线程的两种方式

线程池一共有两种，分别是：

*   自定义线程池 ThreadPoolExecutor**（推荐）**；
*   线程池工具类 Executors.newXxxThreadPool

#### 1.2.1 方式 1：**自定义线程池（推荐）**

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
        // 源码
        // FixedThreadPool: 固定大小线程池
        // CachedTheadPool: 缓存线程池
        new ThreadExcutor(0, max_valuem, 60L, s, new SynchronousQueue<Runnable>());
        new ThreadExcutor(n, n, 0L, ms, new LinkedBlockingQueue<Runable>()
         
        // ScheduledThreadPoolExcutor: 定时任务线程池
        ScheduledThreadPool, SingleThreadScheduledExecutor
        // SingleThreadExecutor: 单线程化线程池
        new ThreadExcutor(1, 1, 0L, ms, new LinkedBlockingQueue<Runable>())
        ```
        
*   **threadFactory：线程的创建工厂**。可以使用默认的线程工厂 Executors.defaultThreadFactory()，也可以自定义线程工厂（实现 ThreadFactory 接口）
*   **RejectedExecutionHandler handler：拒绝策略。**如果任务队列和最大线程数量满了，按照指定的拒绝策略执行任务。
    *   **Abort（默认）：**直接抛异常（拒绝执行异常 RejectedExecutionException）
    *   **CallerRuns：**直接同步调用线程 run() 方法，不创建线程了
    *   **DiscardOldest：**丢弃最老任务
    *   **Discard：**直接丢弃新任务
    *   实现拒绝执行处理器接口（RejectedExecutionHandler），自定义拒绝策略。

#### 1.2.2 方式 2：线程池工具类

**执行器工具类** **Executors 创建线程池：** 底层都是 return new ThreadPoolExecutor(...)。一般不使用这种方式，参数配置死了不可控。

*   **newFixedThreadPool：固定大小的线程池。**
    *   **核心线程数：**所有线程都是核心线程（通过构造参数指定），最大线程数 = 核心线程数。
    *   **存活时间 0s：**因为所有线程都是核心线程，所以用不到存活时间，线程都会一直存活。keepAliveTime 为 0S。
    *   **链表阻塞队列：**超出的线程会在 LinkedBlockingQueue 队列中等待。
*   **newCachedThreadPool：缓存线程池（无限大）。** 
    *   **核心线程数是 0，最大线程数无限大：**最大线程数 Integer.MAX_VALUE。线程数量可以无限扩大，所有线程都是非核心线程。
    *   **空闲线程存活时间 60s：**keepAliveTime 为 60S，空闲线程超过 60s 会被杀死。
    *   **同步队列：**因为最大线程数无限大，所以也用不到阻塞队列，所以设为没有存储空间的 SynchronousQueue 同步队列。这意味着只要有请求到来，就必须要找到一条工作线程处理他，如果当前没有空闲的线程，那么就会再创建一条新的线程。
*   **newScheduledThreadPool：定时任务线程池。**创建一个定长线程池， 支持定时及周期性任务执行。可指定核心线程数，最大线程数。
*   **newSingleThreadExecutor：单线程化的线程池。**核心线程数与最大线程数都只有一个，不回收。后台从 LinkedBlockingQueue 队列中获取任务​。创建一个单线程化的线程池， 它只会用唯一的工作线程来执行任务， 保证所有任务按照指定顺序 (FIFO, LIFO, 优先级) 执行。 

```
/**
 * @Author: vince
 * @CreateTime: 2024/09/10
 * @Description: 测试类
 * @Version: 1.0
 */
public class Test {
    public static void main(String[] args) {
        ExecutorService pool = Executors.newFixedThreadPool(5);
        for (int i = 0; i < 3; i++) {
            pool.execute(() -> System.out.println("Hello World"));
        }
    }
}
```

**代码示例：**

```
package java.util.concurrent;
/**
 * 自定义线程池
 * @since 1.5
 * @author Doug Lea
 */
public class ThreadPoolExecutor extends AbstractExecutorService {
    // ...
}
 
public abstract class AbstractExecutorService implements ExecutorService {
    //  ...
}
```

![](<assets/1740031168098.png>)

## 二、**ThreadPoolExecutor** 源码解析

### **2.1 基本介绍** 

ThreadPoolExecutor 是线程池最核心的类，在 JDK1.5 引入，位于 java.util.concurrent 包下，是 ExecutorService 接口的实现类。 

```
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
 
public class ThreadPoolExample {
 
    public static void main(String[] args) {
        // 创建一个具有固定线程数的线程池
        ExecutorService executorService = Executors.newFixedThreadPool(5);
 
        // 提交任务给线程池执行
        for (int i = 0; i < 10; i++) {
            final int taskId = i;
            executorService.execute(() -> {
                System.out.println("Task ID : " + taskId + " 执行中...");
 
                // 模拟任务执行耗时
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
 
                System.out.println("Task ID : " + taskId + " 完成");
            });
        }
 
        // 关闭线程池
        executorService.shutdown();
    }
}
```

### 2.2 扩展：ExecutorService 接口

ExecutorService 是 Java 提供的线程池接口，所有线程池都直接或间接实现 ExecutorService 接口。它有两个最核心的方法，即 submit 或 execute 方法，用于线程池提交任务。

**submit 或 execute 方法：**

*   **execute：**参数只能是 Runnable，没有返回值
*   **submit：**参数可以是 Runnable、Callable，返回值是 FutureTask 

**Executor 接口：**

只有一个 execute 方法，用来替代通常创建或启动线程的方法。 

**代码示例：**

**execute：**

```
public class ThreadPoolExample {
 
    public static void main(String[] args) {
        // 创建一个具有固定线程数的线程池
        ExecutorService executorService = Executors.newFixedThreadPool(5);
 
        // 提交任务给线程池执行
        for (int i = 0; i < 10; i++) {
            int taskId = i;
            Future<String> future = executorService.submit(() -> {
                System.out.println("Task ID : " + taskId + " 执行中...");
 
                // 模拟任务执行耗时
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
 
                return "Task ID : " + taskId + " 完成";
            });
 
            // 获取任务的执行结果
            try {
                String result = future.get();
                System.out.println(result);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
 
        // 关闭线程池
        executorService.shutdown();
    }
}
```

**submit：**

```
// 创建线程池
        ThreadPoolExecutor threadPoolExecutor = new ThreadPoolExecutor(
                5, // 核心线程数
                200, // 最大线程数量
                10, // 空闲线程存活时间
                TimeUnit.SECONDS, // 时间单位
                new LinkedBlockingDeque<>(100000), // 任务队列，大小100000
                Executors.defaultThreadFactory(), // 线程工厂
                new ThreadPoolExecutor.AbortPolicy() // 拒绝策略
        );
```

### 2.3 线程池参数

#### **2.3.1 ThreadPoolExecutor** 的七个参数

```
import java.util.concurrent.LinkedBlockingDeque;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;
 
public class ThreadPoolExample {
    public static void main(String[] args) {
        // 创建线程池
        ThreadPoolExecutor threadPoolExecutor = new ThreadPoolExecutor(
                5, // 核心线程数
                200, // 最大线程数量
                10, // 空闲线程存活时间
                TimeUnit.SECONDS, // 时间单位
                new LinkedBlockingDeque<>(100000), // 任务队列，大小100000
                Executors.defaultThreadFactory(), // 线程工厂
                new ThreadPoolExecutor.AbortPolicy() // 拒绝策略
        );
 
        // 提交任务到线程池
        for (int i = 0; i < 10; i++) {
            threadPoolExecutor.execute(() -> {
                System.out.println(Thread.currentThread().getName() + " 正在执行任务");
            });
        }
 
        // 关闭线程池
        threadPoolExecutor.shutdown();
    }
}
```

 ThreadPoolExecutor 的构造方法有 7 个重要的参数，分别是下面几个：

*   **corePoolSize：核心线程数**。创建以后，会一直存活到线程池销毁，空闲时也不销毁。
*   **maximumPoolSize：最大线程数量**。阻塞队列满了
*   **keepAliveTime： 存活时间**。释放空闲时间超过 “存活时间” 的线程，仅留核心线程数量的线程。
*   **TimeUnitunit：时间单位**
*   **workQueue： 任务队列。**如果线程数超过核心数量，就把剩余的任务放到队列里。只要有线程空闲，就会去队列取出新的任务执行。new LinkedBlockingDeque() 队列大小默认是 Integer 的最大值，内存不够，所以建议指定队列大小。
    
    *   **LinkedBlockingQueue：链表阻塞队列。**是一个无界队列，可以缓存无限多的任务。由于其无界特性，因此需要合理地处理好任务的生产速率和线程池中线程的数量，以避免内存溢出等异常问题。无限缓存，拒绝策略就能随意了。
    *   **ArrayBlockingQueue：数组阻塞队列。**是一个有界（容量固定）队列，只能缓存固定数量的任务。通过固定队列容量，可以避免任务过多导致线程阻塞，保证线程池资源的可控性和稳定性。**推荐**，有界队列能增加系统的稳定性和预警能力。可以根据需要设大一点，比如几千，新任务丢弃后未来重新入队。
    *   **SynchronousQueue：同步队列。**这个阻塞队列没有存储空间，这意味着只要有请求到来，就必须要找到一条工作线程处理他，如果当前没有空闲的线程，那么就会再创建一条新的线程。
    *   **PriorityBlockingQueue：优先级队列。**能够对任务按照优先级进行排序，当任务数量超过队列容量时，会根据元素的 Comparable 或 Comparator 排序规则进行丢弃或抛异常。
        
        ```
        /**
             * 不指定线程工厂的构造方法
             * @param corePoolSize 核心线程数
             * @param maximumPoolSize 最大线程数
             * @param keepAliveTime 线程存活时间
             * @param unit 线程存活时间单位
             * @param workQueue 任务队列
             * @param handler 拒绝策略
             */
            public ThreadPoolExecutor(int corePoolSize,
                                      int maximumPoolSize,
                                      long keepAliveTime,
                                      TimeUnit unit,
                                      BlockingQueue<Runnable> workQueue,
                                      RejectedExecutionHandler handler) {
                // 不指定线程工厂，将使用默认工厂，调用7参数的构造方法
                this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
                        Executors.defaultThreadFactory(), handler);
            }
        ```
        
    *   **DelayedWorkQueue：延迟阻塞队列。**无界的延迟 + 优先级队列，每个任务有个延时时间，按时长逆序排列在队列中。延时队列越短，则优先级最高，在队列中越靠前，优先被消费。
        
*   **threadFactory：线程的创建工厂**。可以使用默认的线程工厂 Executors.defaultThreadFactory()，也可以自定义线程工厂（实现 ThreadFactory 接口）
*   **RejectedExecutionHandler handler：拒绝策略。**如果任务队列和最大线程数量满了，按照指定的拒绝策略执行任务。
    *   **Abort（默认）：**直接抛异常（拒绝执行异常 RejectedExecutionException）
    *   **CallerRuns：**直接同步调用线程 run() 方法，不创建线程了
    *   **DiscardOldest：**丢弃最老任务
    *   **Discard：**直接丢弃新任务
    *   实现拒绝执行处理器接口（RejectedExecutionHandler），自定义拒绝策略。

#### **2.3.2** 如何为线程池设置合适的线程数

下面的参数只是一个预估值，适合初步设置，具体的线程数需要经过**压测**确定，压榨（更好的利用）CPU 的性能。

**CPU 核心数为 N；**

**核心线程数：**

*   **CPU 密集型：**N+1。数量与 CPU 核数相近是为了不浪费 CPU，并防止频繁的上下文切换，加 1 是为了有线程被阻塞后还能不浪费 CPU 的算力。
*   **I/O 密集型：**2N，或 N/(1 - 阻塞系数)。I/O 密集型任务 CPU 使用率并不是很高，可以让 CPU 在等待 I/O 操作的时去处理别的任务，充分利用 CPU，所以数量就比 CPU 核心数高一倍。有些公司会考虑阻塞系数，阻塞系数是任务线程被阻塞的比例，一般是 0.8~0.9。
*   **实际开发中更适合的公式：**N*((线程等待时间 + 线程计算时间)/ 线程计算时间)

**最大线程数：**设成核心线程数的 2-4 倍。数量主要由 CPU 和 IO 的密集性、处理的数据量等因素决定。

**需要增加线程的情况：**jstack 打印线程快照，如果发现线程池中大部分线程都等待获取任务、则说明线程够用。如果大部分线程都处于运行状态，可以继续适当调高线程数量。

**jstack：**打印指定进程此刻的线程快照。定位线程长时间停顿的原因，例如死锁、等待资源、阻塞。如果有死锁会打印线程的互相占用资源情况。线程快照：该进程内每条线程正在执行的方法堆栈的集合。

#### **2.3.3 代码示例**

创建一个线程池，再创建 10 个线程打印文字：

```
/**
     * 构造方法初始化
     * @param corePoolSize 核心线程数
     * @param maximumPoolSize 最大线程数
     * @param keepAliveTime 非核心空闲线程存活时间
     * @param unit 非核心空闲线程存活时间单位
     * @param workQueue 任务队列
     * @param threadFactory 线程工厂(生产线程的地方)
     * @param handler 拒绝策略
     */
    public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler) {
        // 1.参数校验：
        // 核心线程数不能为负
        // 最大线程数不能为0且必须大于核心线程数
        // 非核心空闲线程存活时间不能为负
        if (corePoolSize < 0 || maximumPoolSize <= 0 || maximumPoolSize < corePoolSize || keepAliveTime < 0)
            throw new IllegalArgumentException();
 
        // 任务队列、线程工厂和拒绝策略不能为空
        if (workQueue == null || threadFactory == null || handler == null)
            throw new NullPointerException();
 
        // 2.将传入的参数赋值给线程池的成员变量
        // 核心线程数
        this.corePoolSize = corePoolSize;
        // 最大线程数
        this.maximumPoolSize = maximumPoolSize;
        // 任务队列
        this.workQueue = workQueue;
        // 将存活时间转换为纳秒存储
        this.keepAliveTime = unit.toNanos(keepAliveTime);
        // 线程工厂，用于创建新线程
        this.threadFactory = threadFactory;
        // 拒绝策略，当线程池无法执行任务时使用
        this.handler = handler; 
    }
```

可以看见只有 5 个核心线程执行任务，原因阻塞队列有 100000 的大小，所以用不到新增非核心线程：

![](<assets/1740031168268.png>)

#### **2.3.4** 线程池的初始化

##### **2.3.4.1** 不指定线程工厂的构造方法

ThreadPoolExecutor 有多个构造方法，这些构造方法内部实际都调用了**全参数的构造方法**。

例如不指定线程的创建工厂，将使用默认的线程创建工厂：

```
/**
     * 直接抛异常拒绝策略
     * {@code RejectedExecutionException}.
     */
    public static class AbortPolicy implements RejectedExecutionHandler {
        /**
         * 构造方法
         */
        public AbortPolicy() {
        }
 
        /**
         * 拒绝执行方法：总是抛出RejectedExecutionException异常
         *
         * @param r 线程
         * @param e 线程池
         * @throws RejectedExecutionException 总是抛出该异常
         */
        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
            // 代码只有一行，抛出异常
            throw new RejectedExecutionException("Task " + r.toString() +
                    " rejected from " +
                    e.toString());
        }
    }
```

![](<assets/1740031168514.png>)

##### **2.3.4.2 全参数核心构造方法**

**核心流程：**

1.  参数校验；非空校验、大于 0 校验、最大线程数大于核心线程数；
2.  参数赋值：7 个参数分别赋值给对应的成员变量

**具体代码：** 

```
/**
     * 同步调用拒绝策略
     */
    public static class CallerRunsPolicy implements RejectedExecutionHandler {
 
        /**
         * 创建一个 {@code CallerRunsPolicy} 实例。
         */
        public CallerRunsPolicy() {
        }
 
        /**
         * 拒绝执行方法
         * 此时任务将被丢弃。
         *
         * @param r 线程
         * @param e 线程池
         */
        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
            // 如果线程池还活着，直接同步调用线程的run()方法
            if (!e.isShutdown()) {
                r.run();
            }
        }
    }
```

#### **2.3.5** 拒绝策略详解

##### **2.3.5.0** 概述

**四个拒绝策略：**

*   **Abort（默认）：**直接抛异常（拒绝执行异常 RejectedExecutionException）
*   **CallerRuns：**直接同步调用线程 run() 方法，不创建线程了
*   **DiscardOldest：**丢弃最老任务
*   **Discard：**直接丢弃新任务

线程池共有以上 4 个拒绝策略，他们都属于 ThreadPoolExecutor 的内部类：

![](<assets/1740031168691.png>)

##### **2.3.5.1** AbortPolicy：抛异常

**核心流程：**

*   直接抛出拒绝执行异常（RejectedExecutionException）

**具体代码：**

```
/**
     * 丢弃最老任务策略
     */
    public static class DiscardOldestPolicy implements RejectedExecutionHandler {
 
        /**
         * 为给定的执行器创建一个 {@code DiscardOldestPolicy} 实例。
         */
        public DiscardOldestPolicy() {
        }
 
        /**
         * 丢弃最老任务
         *
         * @param r 线程
         * @param e 线程池
         */
        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
            // 如果线程池还活着，就让最老任务出队，执行最新任务
            if (!e.isShutdown()) {
                e.getQueue().poll();
                e.execute(r);
            }
        }
    }
```

##### **2.3.5.**2 CallerRunsPolicy：调用线程 run() 方法

**核心流程：**

*   如果线程池还活着，直接同步调用线程的 run() 方法

**具体代码：**

```
/**
     * 丢弃新任务策略
     */
    public static class DiscardPolicy implements RejectedExecutionHandler {
 
        /**
         * 创建一个 {@code DiscardPolicy} 实例。
         */
        public DiscardPolicy() {
        }
 
        /**
         * 不执行任何操作，相当于丢弃任务 r
         *
         * @param r 线程
         * @param e 线程池
         */
        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
        }
    }
```

##### **2.3.5.**3 DiscardOldestPolicy：丢弃最老任务

**核心流程：**

*   如果当前的阻塞队列满了，弹出时间最久的

**具体代码：**

```
/**
 * @Author: vince
 * @CreateTime: 2024/08/20
 * @Description: 丢最老和新任务拒绝策略
 * @Version: 1.0
 */
public class DiscardOldestAndNewestPolicy implements RejectedExecutionHandler {
 
    /**
     * 自定义拒绝策略，丢弃最老的和最新的任务，并打印日志。
     */
    @Override
    public void rejectedExecution(Runnable r, ThreadPoolExecutor executor) {
        BlockingQueue<Runnable> queue = executor.getQueue();
 
        // 检查队列是否为空，不为空则移除最老的任务
        if (!queue.isEmpty()) {
            Runnable oldestTask = queue.poll();
            System.out.println("丢弃最老的任务: " + oldestTask.toString());
        }
 
        // 打印日志并丢弃最新的任务：不对线程对象执行操作，就相当于丢弃了它
        System.out.println("丢弃最新的任务: " + r.toString());
    }
}
```

##### **2.3.5.**4 DiscardPolicy：丢弃新任务

**核心流程：**

*   丢弃新任务：方法内一行代码也没有，不执行任何操作，相当于丢弃任务

**具体代码：**

```
/*
     * 状态和线程数：用来标记线程池状态（高3位），线程个数（低29位，所以线程个数最多是2^29-1）
     * 默认是RUNNING状态，线程个数为0
     */
    private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
 
    /*
     * 线程个数掩码位数：线程池中最大可以容纳的线程数量。
     *  int：4字节（32位）,数据范围是 `-2^31 ~ 2^31-1`。
     * 所以准确说是具体平台下Integer的二进制位数-3后的剩余位数才是线程的个数，
     * 32-3=29
     * 为什么COUNT_BITS是29而不是32？因为int类型是32位，我们需要将高3位保留用于表示线程池的状态。剩下的29位则用来表示线程个数。
     */
    private static final int COUNT_BITS = Integer.SIZE - 3;
 
    /*
     * 线程最大个数(低29位)00011111111111111111111111111111
     * 默认容量：2^29-1
     * 位运算1左移32位，相当于2的32次方
     * （左移<<：是将二进制数的位左移若干位，低位补0，相当于乘以2的n次方。）
     */
    private static final int CAPACITY = (1 << COUNT_BITS) - 1;
 
    /*
     * 线程池有5种状态，按大小排序如下：
     * RUNNING < SHUTDOWN < STOP < TIDYING < TERMINATED
     * RUNNING：运行。线程池处于正常状态，可以接受新的任务，同时会按照预设的策略来处理已有任务的执行。
     * SHUTDOWN：关闭。线程池处于关闭状态，不再接受新的任务，但是会继续执行已有任务直到执行完成。执行线程池对象的shutdown()时进入该状态。
     * STOP：停止。线程池处于关闭状态，不再接受新的任务，同时会中断正在执行的任务，清空线程队列。执行shutdownNow()时进入该状态。
     * TIDYING：整理。所有任务已经执行完毕，线程池进入该状态会开始进行一些结尾工作，比如及时清理线程池的一些资源。
     * TERMINATED：终止。线程池已经完全停止，所有的状态都已经结束了，线程池处于最终的状态。
     */
    private static final int RUNNING = -1 << COUNT_BITS;
    private static final int SHUTDOWN = 0 << COUNT_BITS;
    private static final int STOP = 1 << COUNT_BITS;
    private static final int TIDYING = 2 << COUNT_BITS;
    private static final int TERMINATED = 3 << COUNT_BITS;
    /**
     * 可重入锁，用于加锁往workers里加线程
     */
    private final ReentrantLock mainLock = new ReentrantLock();
    /**
     * 一个HashSet，存有所有工作线程
     */
    private final HashSet<Worker> workers = new HashSet<Worker>();
 
    /**
     * 是否允许核心线程超时：默认是false，即核心线程永不超时
     * 如果是true，核心线程将根据参数的超时时间存活
     */
    private volatile boolean allowCoreThreadTimeOut;
```

##### **2.3.5.**5 自定义拒绝策略

通过实现拒绝执行处理器接口（RejectedExecutionHandler），重写 rejectedExection() 方法，可以创建一个自定义的拒绝策略。

**例如：**创建一个丢弃策略，丢弃最老任务，并且把让新任务优先执行

```
/*
     * 状态和线程数：用来标记线程池状态（高3位），线程个数（低29位，所以线程个数最多是2^29-1）
     * 默认是RUNNING状态，线程个数为0
     */
    private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
```

### **2.4** 核心字段和内部类

#### **2.4.1** 基本介绍

ThreadPoolExecutor 有以下一些核心字段和内部类：

```
private static final int COUNT_BITS = Integer.SIZE - 3;
 
    /*
     * 线程最大个数(低29位)00011111111111111111111111111111
     * 默认容量：2^29-1
     * 位运算1左移32位，相当于2的32次方
     * （左移<<：是将二进制数的位左移若干位，低位补0，相当于乘以2的n次方。）
     */
    private static final int CAPACITY = (1 << COUNT_BITS) - 1;
```

#### **2.4.2 ctl：**线程池当前状态和数量

*   **ctl：**标记线程池状态（高 3 位）和数量（低 29 位，所以线程个数最多是 2^29-1）。

```
/*
     * 线程池有5种状态，按大小排序如下：
     * RUNNING < SHUTDOWN < STOP < TIDYING < TERMINATED
     * RUNNING：运行。线程池处于正常状态，可以接受新的任务，同时会按照预设的策略来处理已有任务的执行。
     * SHUTDOWN：关闭。线程池处于关闭状态，不再接受新的任务，但是会继续执行已有任务直到执行完成。执行线程池对象的shutdown()时进入该状态。
     * STOP：停止。线程池处于关闭状态，不再接受新的任务，同时会中断正在执行的任务，清空线程队列。执行shutdownNow()时进入该状态。
     * TIDYING：整理。所有任务已经执行完毕，线程池进入该状态会开始进行一些结尾工作，比如及时清理线程池的一些资源。
     * TERMINATED：终止。线程池已经完全停止，所有的状态都已经结束了，线程池处于最终的状态。
     */
    private static final int RUNNING = -1 << COUNT_BITS;
    private static final int SHUTDOWN = 0 << COUNT_BITS;
    private static final int STOP = 1 << COUNT_BITS;
    private static final int TIDYING = 2 << COUNT_BITS;
    private static final int TERMINATED = 3 << COUNT_BITS;
```

#### **2.4.3** COUNT_BITS 和 CAPACITY：线程数上限

*   COUNT_BITS：线程数上限的掩码，值是 29
*   CAPACITY：线程数上限，值是 2^29-1

**为什么 COUNT_BITS 是 29 而不是 32？**

因为 int 类型是 32 位，并且我们需要将高 3 位保留用于表示线程池的状态。剩下的 29 位则用来表示线程个数。 

**源码：** 

```
/**
     * 可重入锁，用于加锁往workers里加线程
     */
    private final ReentrantLock mainLock = new ReentrantLock();
    /**
     * mainLock锁的Condition对象，用于线程通信
     * Condition对象：用于线程间通信，通过await()和signal(),signalAll()让线程等待或唤醒。通常用lock锁创建Condition对象，即lock.newCondition();
     */
    private final Condition termination = mainLock.newCondition();
    /**
     * 一个HashSet，存有所有工作线程
     */
    private final HashSet<Worker> workers = new HashSet<Worker>();
```

#### **2.4.4** 五个状态字段：线程池的生命周期 

通常线程池的生命周期包含 5 个状态，对应状态值分别是：-1、0、1、2、3，这些状态只能由小到大迁移，不可逆。

1.  **RUNNING：运行。**线程池处于正常状态，可以接受新的任务，同时会按照预设的策略来处理已有任务的执行。
2.  **SHUTDOWN：关闭。**线程池处于关闭状态，不再接受新的任务，但是会**继续执行**已有任务直到执行完成。执行线程池对象的 **shutdown()** 时进入该状态。
3.  **STOP：停止。**线程池处于关闭状态，不再接受新的任务，同时会**中断**正在执行的任务，**清空**线程队列。执行 **shutdownNow()** 时进入该状态。
4.  **TIDYING：整理。**所有任务已经执行完毕，线程池进入该状态会开始进行一些结尾工作，比如及时清理线程池的一些资源。
5.  **TERMINATED：终止。**线程池已经完全停止，所有的状态都已经结束了，线程池处于最终的状态。 

```
/**
     * @Author: vince
     * @CreateTime: 2024/09/09
     * @Description: Worker继承了AQS，目的就是为了控制工作线程的中断。
     * Worker实现了Runnable，它的run()方法调用runWorker()启动线程
     * @Version: 1.0
     */
    private final class Worker extends AbstractQueuedSynchronizer implements Runnable{
 
        private static final long serialVersionUID = 6138294804551838833L;
        // 当前线程工厂创建的线程(也是执行任务使用的线程)
        final Thread thread;
 
        // 当前的第一个任务
        Runnable firstTask;
 
        // 记录执行了多少个任务
        volatile long completedTasks;
 
        // 构造方法
        Worker(Runnable firstTask) {
            // 将State设置为-1，代表当前不允许中断线程
            setState(-1);
            // 设置任务
            this.firstTask = firstTask;
            // 设置线程
            this.thread = getThreadFactory().newThread(this);
        }
 
        /**
         * 线程启动执行的方法，调用了runWorker()方法
         */
        public void run() {
            runWorker(this);
        }
 
        /**
         * 中断工作线程
         */
        void interruptIfStarted() {
            Thread t;
            // 只有Worker中的state >= 0的时候，可以中断工作线程
            if (getState() >= 0 && (t = thread) != null && !t.isInterrupted()) {
                try {
                    // 如果状态正常，并且线程未中断，这边就中断线程
                    t.interrupt();
                } catch (SecurityException ignore) {
                }
            }
        }
    }
```

#### **2.4.5** 扩展：线程的生命周期  ​

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

![](<assets/1740031168944.png>)

**等待状态如何被唤醒？**

Object 类：

*   wait() 方法让线程进入等待状态
*   notify() 唤醒该对象上的随机一个线程
*   notifyAll() 唤醒该对象上的所有线程。

这 3 个方法必须处于 **synchronized** 代码块或方法中，否则会抛出 IllegalMonitorStateException 异常。因为调用这三个方法之前必须拿要到当前锁对象的监视器（Monitor 对象），synchronized 基于对象头和 Monitor 对象。

也可以通过 **Condition 类的 await/signal/signalAll** 方法实现线程的等待和唤醒，从而实现线程的通信，令线程之间协作处理任务。这两个方法依赖于 Lock 对象。

#### **2.4.6** workers 和 mainLock：维护所有工作线程

这两个字段主要在 addWorker() 方法内用到，每次调用线程. start() 方法启动线程前，都往 wokers 集合里放进一个线程，并给 ctl 加 1

```
/**
     * 是否允许核心线程超时：默认是false，即核心线程永不超时
     * 如果是true，核心线程将根据参数的超时时间存活
     */
    private volatile boolean allowCoreThreadTimeOut;
```

#### **2.4.7** Worker 类：控制中断线程、执行 runWorker() 方法

Worker 类是线程池的内部类，它继承了 AQS， 实现了 Runnable 接口，它

```
import java.util.concurrent.LinkedBlockingDeque;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;
 
public class ThreadPoolExample {
    public static void main(String[] args) {
        // 创建线程池
        ThreadPoolExecutor threadPoolExecutor = new ThreadPoolExecutor(
                5, // 核心线程数
                200, // 最大线程数量
                10, // 空闲线程存活时间
                TimeUnit.SECONDS, // 时间单位
                new LinkedBlockingDeque<>(100000), // 任务队列，大小100000
                Executors.defaultThreadFactory(), // 线程工厂
                new ThreadPoolExecutor.AbortPolicy() // 拒绝策略
        );
 
        // 提交任务到线程池
        for (int i = 0; i < 10; i++) {
            threadPoolExecutor.execute(() -> {
                System.out.println(Thread.currentThread().getName() + " 正在执行任务");
            });
        }
 
        // 关闭线程池
        threadPoolExecutor.shutdown();
    }
}
```

#### **2.4.8** allowCoreThreadTimeOut：是否允许核心线程超时

allowCoreThreadTimeOut 用于指定核心线程是否能超时，它在通过 getTask() 获取线程时调用，通过它判断是否允许核心线程超时。

```
import java.util.concurrent.LinkedBlockingDeque;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;
 
public class ThreadPoolExample {
    public static void main(String[] args) {
        // 创建线程池
        ThreadPoolExecutor threadPoolExecutor = new ThreadPoolExecutor(
                5, // 核心线程数
                200, // 最大线程数量
                10, // 空闲线程存活时间
                TimeUnit.SECONDS, // 时间单位
                new LinkedBlockingDeque<>(3), // 任务队列，大小100000
                Executors.defaultThreadFactory(), // 线程工厂
                new ThreadPoolExecutor.AbortPolicy() // 拒绝策略
        );
 
        // 提交任务到线程池
        for (int i = 0; i < 10; i++) {
            threadPoolExecutor.execute(() -> {
                System.out.println(Thread.currentThread().getName() + " 正在执行任务");
            });
        }
 
        // 关闭线程池
        threadPoolExecutor.shutdown();
    }
}
```

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

### 2.5 execute()：启动线程

#### **2.**5.1 工作原理

**任务加入线程池时，判断的顺序：**

*   核心线程数
*   阻塞队列
*   最大线程数
*   拒绝策略

![](<assets/1740031169040.png>)

**线程池工作原理：** 

1.  **判断核心线程数：**新加入任务，判断 corePoolSize 是否到最大值；如果没到最大值就创建核心线程执行新任务，如果到最大值就判断是否有空闲的核心线程；
2.  **判断加入队列：**如果有空闲的核心线程，则空闲核心线程执行新任务，如果没空闲的核心线程，则尝试加入 FIFO 阻塞队列；
3.  **判断队列容量：**若加入成功，则等待空闲核心线程将队头任务取出并执行，若加入失败（例如队列满了），则判断是否到最大值；
4.  **判断丢弃策略：**如果没到最大值就创建非核心线程执行新任务，如果到了最大值就执行丢弃策略，默认丢弃新任务；
5.  **非核心线程自动回收：**线程数大于 corePoolSize 时，空闲线程将在 keepAliveTime 后回收，直到线程数等于核心线程数。这些核心线程也不会被回收。

**注意：**

实际上线程本身没有核心和非核心的概念，都是靠比较 corePoolSize 和当前线程数判断一个线程是不是能看作核心线程。

可能某个线程之前被看作是核心线程，等它空闲了，线程池又有 corePoolSize 个线程在执行任务，这个线程到 keepAliveTime 后还是会被回收。

**验证：**

 **1. 阻塞队列大时只有核心线程数的线程工作**

 创建一个线程池，再创建 10 个线程打印文字：

```
/**
     * 执行任务
     * @param command Runnable对象
     */
    public void execute(Runnable command) {
        // 0.判空：如果当前传过来的任务是null,直接抛出异常即可
        if (command == null)
            throw new NullPointerException();
 
        int c = ctl.get();
        // 1.启动核心线程数：如果corePoolSize没到最大值就创建核心线程直接执行新任务
        if (workerCountOf(c) < corePoolSize) {
            // Step2:添加一个核心线程
            if (addWorker(command, true)){
                return;
            }
            // 更新一下当前值
            c = ctl.get();
        }
        // 如果走到下面会有两种情况：
        // a、核心线程数满了,需要往阻塞队列里面扔任务
        // b、核心线程数满了,阻塞队列也满了,执行拒绝策略
 
        // 2.任务放至阻塞队列：判断当前的状态是不是Running的状态(RUNNING可以处理任务，并且处理阻塞队列中的任务)
        // 如果是Running的状态,则尝试将任务放至阻塞队列。队列的offer(E e)：用于非阻塞的插入操作，插入失败会返回false而不是等待
        if (isRunning(c) && workQueue.offer(command)) {
            // 再次更新数值
            int recheck = ctl.get();
            // 再次校验当前的线程池状态是不是Running
            // 如果线程池状态不是Running的话,需要删除掉刚刚放的任务
            if (!isRunning(recheck) && remove(command)){
                // 执行拒绝策略
                reject(command);
            }
            // 如果到这里,说明上面阻塞队列中已经有数据了
            // 如果线程池的个数为0的话,需要创建一个非核心工作线程去执行该任务
            // 不能让人家堵塞着
            else if (workerCountOf(recheck) == 0){
                addWorker(null, false);
            }
        }
        // 此时会有两种情况：
        // a、任务成功放进阻塞队列，正在等待被执行；
        // b、放进阻塞队列失败,等待添加非核心工作线程
 
        // 3.启动非核心线程数
        //    尝试添加非核心工作线程，若添加非核心工作线程失败,证明已经到达maximumPoolSize的限制,执行拒绝策略
        // 添加一个非核心工作线程
        else if (!addWorker(command, false))
            // 工作队列中添加任务失败,执行拒绝策略
            reject(command);
    }
```

可以看见只有 5 个核心线程执行任务，原因阻塞队列有 100000 的大小，所以用不到新增非核心线程：

![](<assets/1740031169362.png>)

2. 阻塞队列小时，会创建非核心线程工作：

```
/**
     * 添加工作线程
     * @param firstTask Runnable对象
     * @param core 是否是核心线程
     * @return boolean
     */
    private boolean addWorker(Runnable firstTask, boolean core) {
        // retry主要是为了结束整个循环
        // retry 是一个标签（label），用来标记循环的某个位置。
        // 这个标签与 for (;;) 循环结合使用，调用continue retry时重新开始整个循环，调用break retry时结束整个循环
        retry:
        for (;;) {
            // 获取当前线程池的ctl：包含线程池状态和数量信息
            int c = ctl.get();
 
            // 获取线程池状态：根据ctl，获取线程池状态
            // runStateOf:基于&运算的特点，保证只会拿到ctl高三位的值
            int rs = runStateOf(c);
 
            // 1.校验线程池状态和队列：如果不是运行时，直接退出循环
            // rs >= SHUTDOWN:表示此时不再接收新任务。代表当前线程池状态为：SHUTDOWN、STOP、TIDYING、TERMINATED,线程池状态异常
            // RUNNING < SHUTDOWN < STOP < TIDYING < TERMINATED
            // rs == SHUTDOWN，这时表示关闭状态，不再接受新提交的任务，但却可以继续处理阻塞队列中已保存的任务
            // 但这里SHUTDOWN状态稍许不同(不会接收新任务，正在处理的任务正常进行，阻塞队列的任务也会做完)
            if (rs >= SHUTDOWN && ! (rs == SHUTDOWN && firstTask == null && !workQueue.isEmpty())){
                return false;
            }
 
            for (;;) {
                // 2.校验工作线程数量：如果超过线程数上限直接失败
                // 获取线程数
                int wc = workerCountOf(c);
                 // CAPACITY：是ctl的低29位的最大值（二进制是29个1），返回false；
                 // core：addWorker()方法的第二个参数，如果为true表示根据corePoolSize来比较，
                 // 如果为false则根据maximumPoolSize来比较。
                if (wc >= CAPACITY ||
                        wc >= (core ? corePoolSize : maximumPoolSize))
                    return false;
                // 3.尝试给ctl加1：如果成功，则跳出第一个for循环；如果失败，则重新循环
                // 尝试CAS方式自增workCount  其实就是类似锁，同时只有一个线程能成功
                if (compareAndIncrementWorkerCount(c))
                    break retry;
                // 3.1 如果增加workerCount失败，则重新获取ctl的值
                c = ctl.get();
                // 3.2 如果当前的运行状态不等于第一个循环刚开始时的状态，说明状态已被改变，返回第一个for循环继续执行
                if (runStateOf(c) != rs)
                    continue retry;
                // else CAS failed due to workerCount change; retry inner loop
            }
        }
 
        // 4.线程放进workers：加锁把线程放进workers，workers是一个HashSet，用于保存所有工作线程
        // 代码执行到这里  只是通过不停的循环通过CAS获取到执行任务的“锁”
 
        boolean workerStarted = false;
        boolean workerAdded = false;
        Worker w = null;
        try {
            // 根据firstTask来创建Worker对象
            w = new Worker(firstTask);
            // 每一个Worker对象都会创建一个线程
            final Thread t = w.thread;
            if (t != null) {
                // 获取锁。mainLock是ThreadPoolExecutor的可重入锁字段，用于保证线程安全
                final ReentrantLock mainLock = this.mainLock;
                // 加锁
                mainLock.lock();
                try {
                    int rs = runStateOf(ctl.get());
                    /*
                     * rs < SHUTDOWN表示是RUNNING状态；
                     * 如果rs是RUNNING状态或者rs是SHUTDOWN状态并且firstTask为null，向线程池中添加线程。
                     * 因为在SHUTDOWN时不会在添加新的任务，但还是会执行workQueue中的任务
                     */
                    if (rs < SHUTDOWN ||
                            (rs == SHUTDOWN && firstTask == null)) {
                        // 校验线程是否存活，不存活直接抛出异常
                        if (t.isAlive())
                            throw new IllegalThreadStateException();
                        // workers是一个HashSet，用于保存所有工作线程
                        workers.add(w);
                        int s = workers.size();
                        // largestPoolSize记录着线程池中出现过的最大线程数量
                        if (s > largestPoolSize)
                            largestPoolSize = s;
                        workerAdded = true;
                    }
                } finally {
                    // 解锁
                    mainLock.unlock();
                }
                // 5.启动线程：如果成功加进workers，调用线程的.start()方法，启动线程
                if (workerAdded) {
                    // 启动线程  这个t.start()实际是调用的Worker类中的run方法，Worker本身实现了Runnable接口，所以一个Worker类型的对象也是一个线程。
                    t.start();
                    workerStarted = true;
                }
            }
        } finally {
            if (! workerStarted)
                addWorkerFailed(w);
        }
        return workerStarted;
    }
```

可以看到，有 7 个线程在工作。流程如下：

*   5 个任务创建核心线程工作；
*   3 个任务因为核心线程数满了，所以进入阻塞队列，等待有空闲线程时执行；
*   2 个任务因为阻塞队列满了，所以创建非核心线程工作；

![](<assets/1740031169451.png>)

#### **2.**5.2 源码解析

**核心流程：**

1.  **启动核心线程数**：如果 corePoolSize 没到最大值就创建核心线程直接执行新任务
2.  **任务放至阻塞队列**：判断当前的状态是不是 Running 的状态 (RUNNING 可以处理任务，并且处理阻塞队列中的任务)
3.  **启动非核心线程数**：尝试添加非核心工作线程，若添加非核心工作线程失败, 证明已经到达 maximumPoolSize 的限制, 执行拒绝策略

**具体代码：**

```
/**
     * 执行任务：由Worker类的run()调用的唯一一行方法
     * @param w Worker对象
     */
    final void runWorker(Worker w) {
        Thread wt = Thread.currentThread();
        // 1.获取Worker中的runnable任务，拿到后Worker置空、解锁
        // 拿到当前Worker的第一个任务(如果携带的话)
        Runnable task = w.firstTask;
        // 置空
        w.firstTask = null;
        // 解锁
        w.unlock();
        boolean completedAbruptly = true;
        try {
            // 2.如果任务不等于空，则加锁、执行任务、解锁
            while (task != null || (task = getTask()) != null) {
                // 加锁
                w.lock();
                // 如果线程池状态 >= STOP，确保线程中断了
                if ((runStateAtLeast(ctl.get(), STOP) || (Thread.interrupted() && runStateAtLeast(ctl.get(), STOP))) &&
                        !wt.isInterrupted())
                    wt.interrupt();
                try {
                    // 在任务执行前做一些操作：这个方法内为空，需要我们继承ThreadPoolExecutor重写这个方法
                    beforeExecute(wt, task);
                    Throwable thrown = null;
                    try {
                        // 任务执行：调用线程的run方法
                        task.run();
                    } catch (RuntimeException x) {
                        thrown = x; throw x;
                    } catch (Error x) {
                        thrown = x; throw x;
                    } catch (Throwable x) {
                        thrown = x; throw new Error(x);
                    } finally {
                        // 任务执行后一些操作：这个方法内为空，需要我们继承ThreadPoolExecutor重写这个方法
                        afterExecute(task, thrown);
                    }
                } finally {
                    // 任务置空
                    task = null;
                    // 执行任务+1
                    w.completedTasks++;
                    // 解锁
                    w.unlock();
                }
            }
            completedAbruptly = false;
        } finally {
            // 删除线程的方法
            processWorkerExit(w, completedAbruptly);
        }
    }
```

### 2.6 addWorker()：添加工作线程

**调用关系：**

可以看到，上面 execute() 方法，启动核心线程、非核心线程时都要执行 addWorker（）方法，这个方法是往线程池添加工作线程。

![](<assets/1740031169692.png>)

**核心流程：**

1.  **ctl 自旋加 1：**在一个 retry 循环里自旋给 ctl 字段加 1。ctl 存放线程池当前状态和数量
    1.  **校验线程池状态和队列：**如果不是 RUNNING，直接退出循环
    2.  **校验线程数：**如果超过上限直接失败
    3.  **尝试给 ctl 加 1：**如果成功则结束循环，如果失败则重新循环
2.  **workers 存线程：**加锁把线程并放进 workers，workers 是一个 HashSet，用于保存所有工作线程
3.  **启动线程：**如果成功加进 workers，调用线程的. start() 方法，**启动线程。**
    1.  注意这里调用的是 Worker 对象的 start() 方法，而不是原线程的 start() 方法。Worker 对象封装了原线程。Worker 类是 Runnable 的子类，它的 run() 方法只有一行，调用了 **runWorker() 方法**。

**完整源码：** 

```
/**
     * 获取任务
     * @return {@link Runnable }
     */
    private Runnable getTask() {
        // 声明超时变量：表示上次从阻塞队列中取任务时是否超时，先标记为未超时
        boolean timedOut = false;
 
        // 1.死循环拿线程对象
        for (;;) {
            // 1.1 校验当前线程池状态
            // 拿到当前的ctl
            int c = ctl.get();
            // 获取当前的线程池状态
            int rs = runStateOf(c);
 
            // 如果线程池状态是STOP，没有必要处理阻塞队列任务，直接返回null
            // 如果线程池状态是SHUTDOWN，并且阻塞队列是空的，直接返回null
            if (rs >= SHUTDOWN && (rs >= STOP || workQueue.isEmpty())) {
                // 线程池中的线程个数减一
                decrementWorkerCount();
                return null;
            }
 
            // 当前线程池中线程个数
            int wc = workerCountOf(c);
 
            // 1.2 判断线程是否能超时：如果核心线程允许超时，或者当前线程池中的线程个数大于核心线程数，那么这个线程就有寿命
            // allowCoreThreadTimeOut:是否允许核心线程数超时（开启这个之后），核心线程数也会执行下面超时的逻辑
            // wc > corePoolSize:当前线程池中的线程个数大于核心线程数
            boolean timed = allowCoreThreadTimeOut || wc > corePoolSize;
 
            // 1.3 如果线程超时，则销毁线程
            // wc > maximumPoolSize:基本不存在
            // timed && timedOut:第一次肯定是失败的(超时标记为false)
            if ((wc > maximumPoolSize || (timed && timedOut))
                    // 1、线程个数为1
                    // 2、阻塞队列是空的
                    && (wc > 1 || workQueue.isEmpty())) {
                // 线程池的线程个数减一
                if (compareAndDecrementWorkerCount(c)){
                    return null;
                }
                continue;
            }
            // 运行到这里，说明线程没超时
            try {
                // 1.4 从阻塞队列取出线程：当队列没线程时，如果线程允许超时则返回空，如果不允许则阻塞获取
                // poll()：从队列中获取并移除一个任务。如果队列为空，它会返回 null
                // take()：从队列中获取并移除一个任务。如果队列为空，它会阻塞当前线程直到有任务可用为止。
                Runnable r = timed ? workQueue.poll(keepAliveTime, TimeUnit.NANOSECONDS) : workQueue.take();
                if (r != null){
                    return r;
                }
                // 到这里，说明没从阻塞队列拿出线程，线程超时了
                timedOut = true;
            } catch (InterruptedException retry) {
                timedOut = false;
            }
        }
    }
```

### 2.7 runWorker ()：执行线程

**调用关系：**

上面 addWorker() 开启线程时，调用的 Worker 对象的 start() 方法。Worker 对象封装了原线程。Worker 类是 Runnable 的子类，它的 run() 方法只有一行，调用了 **runWorker() 方法**。 

**addWorker() 调用 Worker 类的 start() 方法：**

![](<assets/1740031169942.png>)

**Worker 类的 run() 方法**

![](<assets/1740031170187.png>)

**核心流程：**

1.  **获取任务：**调用 getTask() 方法，获取 Worker 中的 Runnable 任务，拿到后 Worker 置空、解锁
2.  **执行任务：**如果任务不等于空，则加锁执行任务
    1.  加锁
    2.  beforeExecute()：这个方法为空，需要我们继承 ThreadPoolExecutor 重写这个方法
    3.  执行任务：调用 Runnable 的 run() 方法执行任务
    4.  afterExecute()：这个方法为空，需要我们继承 ThreadPoolExecutor 重写这个方法
    5.  解锁

**代码解析：**

```
/**
     * 关闭线程池
     */
    public void shutdown() {
        // 1.加锁
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            // 2.校验关闭权限：校验调用者是否有权限关闭线程池及其中的所有线程
            checkShutdownAccess();
            // 3.修改线程池状态：将线程池状态修改为SHUTDOWN
            advanceRunState(SHUTDOWN);
            // 4.中断空闲线程：遍历线程池workers中的工作线程，并尝试中断那些处于空闲状态的线程。
            interruptIdleWorkers();
            // 5.钩子函数，实际内容为空，用户继承重写可以编写关闭线程池后要执行的逻辑
            onShutdown(); // hook for ScheduledThreadPoolExecutor
        } finally {
            mainLock.unlock();
        }
        // 6.查看当前线程池是否可以变为TERMINATED状态
        // 从 SHUTDOWN 状态修改为 TIDYING，在修改为 TERMINATED
        tryTerminate();
    }
```

### 2.8 getTask()：从队列获取任务

**调用关系：**

上面 runWorker() 方法，调用了 getTask() 方法获取线程，返回值是 Runnable 对象

![](<assets/1740031170444.png>)

**核心流程：**

1.  **进入死循环**
2.  **校验当前线程池状态**：如果状态是停止，或关闭 + 阻塞队列是空，则返回空
3.  **校验超时**：如果是非核心线程，或者允许超时的核心线程，则尝试销毁线程；
4.  **从阻塞队列取出线程**：当队列没线程时，如果线程允许超时则返回空，如果不允许则阻塞获取

源码解析：

```
/**
     * 校验关闭权限：校验调用者是否有权限关闭线程池及其中的所有线程
     */
    private void checkShutdownAccess() {
        // 1. 获取系统的安全管理器
        SecurityManager security = System.getSecurityManager();
 
        // 2. 如果有安全管理器，需要进行权限检查
        if (security != null) {
            // 3. 首先检查调用者是否有权限关闭线程池（shutdownPerm 是一种特殊的权限）
            security.checkPermission(shutdownPerm);
 
            // 4. 获取线程池的锁，确保操作的线程安全
            final ReentrantLock mainLock = this.mainLock;
            mainLock.lock();
 
            try {
                // 5. 遍历线程池中的每一个工作线程，检查调用者是否有权限中断每个线程
                for (Worker w : workers)
                    security.checkAccess(w.thread);
            } finally {
                // 6. 最后解锁，确保锁的释放以防止死锁
                mainLock.unlock();
            }
        }
    }
```

### 2.9 关闭

1.  **SHUTDOWN：关闭。**线程池处于关闭状态，不再接受新的任务，但是会**继续执行**已有任务直到执行完成。执行线程池对象的 **shutdown()** 时进入该状态。
2.  **STOP：停止。**线程池处于关闭状态，不再接受新的任务，同时会**中断**正在执行的任务，**清空**线程队列。执行 **shutdownNow()** 时进入该状态。

#### 2.9.1 shutdown()：关闭线程

##### 2.9.1.1 源码解析

shutdown() 方法用于关闭线程，线程池关闭后，将不再接受新的任务，旧任务将**执行完成**。

**核心流程：**

1.  加锁
2.  **校验关闭权限：**校验调用者是否有权限关闭线程池及其中的所有线程
3.  **修改线程池状态：**将线程池状态修改为 SHUTDOWN
4.  **中断空闲线程：**遍历线程池 workers 中的工作线程，并尝试中断那些处于空闲状态的线程。
5.  **执行关闭时方法：**钩子函数，实际内容为空，用户继承重写可以编写关闭线程池后要执行的逻辑
6.  **尝试终止：**查看当前线程池是否可以变为 TERMINATED 状态

**代码解析：**

```
/**
     * 修改线程池状态
     * @param targetState 目标状态
     */
    private void advanceRunState(int targetState) {
        // 1.进入直接死循环
        for (;;) {
            // 2.拿到当前的ctl
            int c = ctl.get();
            // 3.修改状态：如果当前状态已经大于等于目标状态，则跳出循环，否则CAS修改线程池状态
            if (runStateAtLeast(c, targetState) || ctl.compareAndSet(c, ctlOf(targetState, workerCountOf(c))))
                break;
        }
    }
```

##### 2.9.1.2 校验关闭权限：checkShutdownAccess()

```
/**
     * 中断空闲线程：遍历线程池workers中的工作线程，并尝试中断那些处于空闲状态的线程。
     * 不传参数时，默认参数是false，即中断所有线程，而不是只中断完一个线程就完事
     */
    private void interruptIdleWorkers() {
        interruptIdleWorkers(false);
    }
    /**
     * 中断空闲线程：遍历线程池workers中的工作线程，并尝试中断那些处于空闲状态的线程。
     * @param onlyOne 是否只中断一个：如果参数 onlyOne 为 true，中断一个线程后就直接跳出循环。如果为 false，则继续尝试中断所有空闲线程。
     */
    private void interruptIdleWorkers(boolean onlyOne) {
        // 1.加锁
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            // 2.遍历线程池workers中的工作线程，并尝试中断那些处于空闲状态的线程。
            for (java.util.concurrent.ThreadPoolExecutor.Worker w : workers) {
                Thread t = w.thread;
                if (!t.isInterrupted() && w.tryLock()) {
                    try {
                        t.interrupt();
                    } catch (SecurityException ignore) {
                    } finally {
                        w.unlock();
                    }
                }
                // 如果只中断一个，则中断后直接退出循环
                if (onlyOne)
                    break;
            }
        } finally {
            mainLock.unlock();
        }
    }
```

##### 2.9.1.3 修改线程池状态：advanceRunState()

```
/**
     * 尝试设置线程池为TERMINATED状态
     */
    final void tryTerminate() {
        // 1.开始死循环；
        for (;;) {
            // 拿到ctl
            int c = ctl.get();
            // 加锁
            final ReentrantLock mainLock = this.mainLock;
            mainLock.lock();
            try {
                // 2.尝试CAS将线程池状态设为TIDYING，如果失败则解锁再次循环，如果成功则继续执行：
                if (ctl.compareAndSet(c, ctlOf(TIDYING, 0))) {
                    try {
                        // 2.1 执行销毁前方法：根据用户自定义的逻辑，在线程池销毁之前做最后的处理
                        // 该方法是一个钩子函数，是个空方法，允许我们通过继承重写的方式，自己定义该方法逻辑
                        terminated();
                    } finally {
                        // 2.2 线程池状态设为TERMINATED
                        ctl.set(ctlOf(TERMINATED, 0));
                        // 2.3 唤醒所有等待线程池终止的线程
                        // termination是mainLock的 Condition 对象 ，这是与 ReentrantLock 相关联的条件变量，用于线程间的通信。
                        // Condition对象：用于线程间通信，通过await()和signal(),signalAll()让线程等待或唤醒。通常用lock锁创建Condition对象，即lock.newCondition();
                        termination.signalAll();
                    }
                    return;
                }
            } finally {
                // 解锁
                mainLock.unlock();
            }
        }
    }
```

##### 2.9.1.4 中断空闲线程：interruptIdleWorkers()

```
/**
     * 终止线程池
     * @return {@link List }<{@link Runnable }> 被删除的所有线程
     */
    public List<Runnable> shutdownNow() {
        // 声明tasks，用于收集所有被删除的线程
        List<Runnable> tasks;
        // 1.加锁
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            // 2.校验关闭权限：校验调用者是否有权限关闭线程池及其中的所有线程
            checkShutdownAccess();
            // 3.修改线程池状态：将线程池状态修改为STOP
            advanceRunState(STOP);
            // 4.中断所有线程：将线程池中的线程全部中断
            interruptWorkers();
            // 5.删除所有线程：删除阻塞队列中所有的工作线程，并将这些删除线程收集到tasks
            // drain以为排空
            tasks = drainQueue();
        } finally {
            // 6.解锁
            mainLock.unlock();
        }
        // 7.尝试设置线程池为TERMINATED状态
        // 从 Stop 状态修改为 TIDYING，在修改为 TERMINATED
        tryTerminate();
        return tasks;
    }
```

##### 2.9.1.5 尝试终止线程池：tryTerminate()

```
/**
     * 清空阻塞队列：删除阻塞队列中所有线程，并返回这些线程
     * @return {@link List }<{@link Runnable }>
     */
    private List<Runnable> drainQueue() {
        // 1.获取阻塞队列
        BlockingQueue<Runnable> q = workQueue;
        // 返回的结果
        ArrayList<Runnable> taskList = new ArrayList<Runnable>();
        // 2.清空阻塞队列并将数据放入taskList中。
        // drainTo 方法是 BlockingQueue 的批量删除方法，它可以将队列中的所有元素批量转移到另一个集合中。
        q.drainTo(taskList);
        // 3.再次遍历清空：因为没加锁，所以需要再次遍历，防止依赖
        if (!q.isEmpty()) {
            for (Runnable r : q.toArray(new Runnable[0])) {
                if (q.remove(r))
                    taskList.add(r);
            }
        }
        // 最终返回即可
        return taskList;
    }
```

#### 2.9.2 shutdownNow()：停止线程

##### 2.9.2.1 源码解析

shutdownNow() 方法用于关闭线程，线程池关闭后，将不再接受新的任务，旧任务将**直接中断**。

**核心流程：**

1.  **加锁**
2.  **校验关闭权限**：校验调用者是否有权限关闭线程池及其中的所有线程
3.  **修改线程池状态**：将线程池状态修改为 STOP
4.  **中断所有线程**：将线程池中的线程全部中断
5.  **删除所有线程**：
    1.  删除当前所有的工作线程
    2.  删除阻塞队列中所有的工作线程
    3.  并将这些删除线程收集到 tasks
6.  **解锁**
7.  **尝试设置线程池为 TERMINATED 状态：**
    1.  CAS 自旋修改状态
    2.  执行销毁前自定义方法
    3.  唤醒所有等待线程池终止的线程

**代码解析：** 

```
ExecutorService executorService = Executors.newFixedThreadPool(10);
 
// 源码
// 固定大小线程池: 
new ThreadExcutor(n, n, 0L, ms, new LinkedBlockingQueue<Runable>()
// 缓存线程池: 
new ThreadExcutor(0, max_valuem, 60L, s, new SynchronousQueue<Runnable>());
// 定时任务线程池: 
new ScheduledThreadPoolExecutor(corePoolSize);
// super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
              new DelayedWorkQueue());
// 单线程线程池: 
new ThreadExcutor(1, 1, 0L, ms, new LinkedBlockingQueue<Runable>())
```

##### 2.9.2.2 清空阻塞队列：drainQueue()

```
public class Executors {
 
    /**
     * 创建固定大小的线程池
     *
     * @param nThreads the number of threads in the pool
     * @return the newly created thread pool
     * @throws IllegalArgumentException if {@code nThreads <= 0}
     */
    public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
    }
...
}
```

## 三、**Executors 源码解析**

### **3.1 Executors 工具类**

执行器工具类 Executors 主要用于创建线程池，它主要有 4 个创建线程池的方法：

*   **newFixedThreadPool：**固定大小的线程池
    
*   **newCachedThreadPool：**缓存线程池（无限大）
    
*   **newScheduledThreadPool：**定时任务线程池
    
*   **newSingleThreadExecutor：**单线程化的线程池
    

一般不使用这种方式创建线程池，因为虽然它简单方便，但是它的**参数配置写死了**，不可控。

**参数配置包括：**

核心线程数、最大线程数、线程存活时间、阻塞队列、拒绝策略。详细看上文的 “ThreadPoolExecutor”

这四个线程池底层都是 return new ThreadPoolExecutor(...)，返回自定义线程池，并根据各个线程池的特性，传入线程数等参数。

```
public class Executors {
 
    /**
     * 创建固定大小的线程池
     *
     * @param nThreads the number of threads in the pool
     * @return the newly created thread pool
     * @throws IllegalArgumentException if {@code nThreads <= 0}
     */
    public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
    }
    // ...
}
```

**以固定大小的线程池为例：**

源码如下

![](<assets/1740031170686.png>)

可以看到：

*   核心线程数和最大线程数都等于传入的参数
    
*   空闲线程存活时间是 0L。这里其实设置多少都一个效果，因为这个线程池最大线程数 = 核心线程数，也就是说不会有非核心线程，而核心线程是不会被回收的。
    
*   阻塞队列是链表。无界，可以缓存无限多的任务，所以也用不到拒绝策略。
    

```
​
public class Executors {
 
    /**
     * 缓存线程池
     * @return the newly created thread pool
     */
    public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
    }
    // ...
}
 
​
```

### **3.2 newFixedThreadPool：固定大小的线程池**

**特点：**

*   **核心线程数：**所有线程都是核心线程（通过构造参数指定），最大线程数 = 核心线程数。
*   **存活时间 0s：**因为所有线程都是核心线程，所以用不到存活时间，线程都会一直存活。keepAliveTime 为 0S。
*   **链表阻塞队列：**超出的线程会在 LinkedBlockingQueue 队列中等待。

**源码：**

```
public class Executors {
 
    /**
     * 定时任务线程池：可以定期或设定多久后执行任务
     */
    public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize) {
        return new ScheduledThreadPoolExecutor(corePoolSize);
    }
    // ...
}
```

可以看到，底层是创建自定义线程池对象，并且传入参数进行了设置：

*   **核心线程数和最大线程数：**都等于传入的参数
*   **空闲线程存活时间：**是 0L。这里其实设置多少都一个效果，因为这个线程池最大线程数 = 核心线程数，也就是说不会有非核心线程，而核心线程是不会被回收的。
*   **阻塞队列：**是链表。无界，可以缓存无限多的任务，所以也用不到拒绝策略。

![](<assets/1740031170973.png>)

### **3.3 newCachedThreadPool：缓存线程池（无限大）**

**特点：** 

*   **所有线程都是非核心线程：**核心线程数是 0，最大线程数无限大
*   **空闲线程一分钟后杀死：**keepAliveTime 为 60S，空闲线程超过 60s 会被杀死。
*   **阻塞队列没有空间：**因为最大线程数无限大，所以也用不到阻塞队列，所以设为没有存储空间的 SynchronousQueue 同步队列。

源码：

```
public class ScheduledThreadPoolExecutor
        extends ThreadPoolExecutor
        implements ScheduledExecutorService {
    /**
     * Creates a new {@code ScheduledThreadPoolExecutor} with the
     * given core pool size.
     *
     * @param corePoolSize the number of threads to keep in the pool, even
     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set
     * @throws IllegalArgumentException if {@code corePoolSize < 0}
     */
    public ScheduledThreadPoolExecutor(int corePoolSize) {
        super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
              new DelayedWorkQueue());
    }
    // ...
}
```

可以看到，底层是创建自定义线程池对象，并且传入参数进行了设置：

*   **核心线程数是 0，最大线程数无限大：**最大线程数 Integer.MAX_VALUE，即 2^31-1。线程数量可以无限扩大，所有线程都是非核心线程。
*   **空闲线程存活时间 60s：**keepAliveTime 为 60S，空闲线程超过 60s 会被杀死。
*   **同步队列：**因为最大线程数无限大，所以也用不到阻塞队列，所以设为没有存储空间的 SynchronousQueue 同步队列。这意味着只要有请求到来，就必须要找到一条工作线程处理他，如果当前没有空闲的线程，那么就会再创建一条新的线程。

![](<assets/1740031171064.png>)

### **3.4 newScheduledThreadPool：定时任务线程池**

创建一个定长线程池， 支持定时及周期性任务执行。可指定核心线程数。

特点：

*   **核心线程数：**由传入参数决定；
*   **最大线程数：**无限大
*   **存活时间：**0
*   **阻塞队列：**延迟队列。无界的延迟 + 优先级队列，延迟时间短的任务排在队头，优先被执行。

**源码：** 

```
public class Executors {
 
    /**
     * 单线程线程池：仅有一个核心线程和无界队列
     */
    public static ExecutorService newSingleThreadExecutor() {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
                                    new LinkedBlockingQueue<Runnable>()));
    }
    // ...
}
```

```
public class ScheduledThreadPoolExecutor
        extends ThreadPoolExecutor
        implements ScheduledExecutorService {
    /**
     * Creates a new {@code ScheduledThreadPoolExecutor} with the
     * given core pool size.
     *
     * @param corePoolSize the number of threads to keep in the pool, even
     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set
     * @throws IllegalArgumentException if {@code corePoolSize < 0}
     */
    public ScheduledThreadPoolExecutor(int corePoolSize) {
        super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
              new DelayedWorkQueue());
    }
    // ...
}
```

可以看到：

*   **核心线程数：**由传入参数 corePoolSize 决定；
*   **最大线程数：**无限大，2^31-1。这里其实设置无限大和设置 corePoolSize 是一个效果，因为它的阻塞队列是无界的，内存溢出前永远不会满，所以也不会有创建非核心线程的机会。至于为什么设置成无限大呢，可能是为了保证内存溢出等极端情况下，任务可以不被丢弃，也可能是心理安慰。
*   **存活时间：**0。因为阻塞队列无界，所以不会有非核心线程，所以存活时间也没意义，内存溢出时创建了非核心线程，空闲后直接销毁。
*   **阻塞队列：**延迟队列。无界的延迟 + 优先级队列，延迟时间短的任务排在队头，优先被执行。

![](<assets/1740031171371.png>)

### **3.5 newSingleThreadExecutor：单线程化的线程池**

*   **核心线程数和最大线程数：**核心线程数与最大线程数都只有一个，不回收。创建一个单线程化的线程池， 它只会用唯一的工作线程来执行任务， 保证所有任务按照指定顺序 (FIFO, LIFO, 优先级) 执行。 
*   **阻塞队列：**无界的链表队列。
*   **使用场景：**需要稳定串行化执行一个个线程的场景；

**思考：**单线程化的线程池存在的意义是什么？直接创建一个线程不就好了？

主要是为了利用线程池的优势。

*   **让线程复用：**如果是直接创建一个线程，这个线程创建后会直接销毁，而线程池的核心线程会一直存活，这就减少了创建、销毁的时间损耗。
*   **管理线程数量：**它可以管理线程的数量，保证只有一个线程，减少了创建、销毁的时间损耗。

源码：

```
public class Executors {
 
    /**
     * 单线程线程池：仅有一个核心线程和无界队列
     */
    public static ExecutorService newSingleThreadExecutor() {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
                                    new LinkedBlockingQueue<Runnable>()));
    }
    // ...
}
```