---
url: https://blog.csdn.net/qq_40991313/article/details/130448194
title: 设计模式——模板模式_aqs 设计模式 - CSDN 博客
date: 2025-02-20 13:54:44
tag: 
summary: 
---
 **导航：**   

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?spm=1001.2014.3001.5501 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[模板模式](#t0)

[1、基本介绍](#t1)

[2、模板模式解决豆浆制作问题](#t2)

[3、钩子方法](#t3)

[4、Spring 框架 AbstractApplicationContext 抽象类](#t4)

[5、JUC 包下的 AQS 抽象队列同步器](#t5)

[6、应用场景：AQS](#t6) 

## 模板模式

### 1、基本介绍

*   1）模板方法模式（Template Method Pattern），又叫模板模式（Template Pattern），在一个**抽象类**公开**定义了**执行它的方法的**模板**。它的**子类可以按需要重写方法实现**，但调用将以抽象类中定义的方式进行

*   2）简单说，模板方法模式定义一个操作中的算法的骨架，而**将一些步骤延迟到子类中**，使得子类可以不改变一个算法的结构，就可以重定义该算法的某些特定步骤

*   3）这种类型的设计模式属于**行为型模式（**用于描述对象之间的通信和责任分配**）。**

**实现方式：**抽象类有一个模板方法和其他行为方法，模板方法按流程调用各行为方法（抽象或非抽象）；具体子类重写抽象的行为方法。

![](<assets/1740030884604.png>)

**对原理类图的说明——即模板方法模式的角色和职责**

*   `AbstractClass`抽象类中实现了模板方法，定义了算法的骨架，具体子类需要去实现其抽象方法或重写其中方法

*   `ConcreteClass`实现了抽象方法，已完成算法中特定子类的步骤

**注意事项和细节**

*   1）**基本思想**：算法只存在于一个地方，也就是在父类中，容易修改。需要修改算法时，只要修改父类的模板方法或者已经实现的某些步骤，子类就会继承这些修改

*   2）实现了最大化**代码复用**。父类的模板方法和已实现的某些步骤会被子类继承而直接使用

*   3）既统一了算法，也提供了很大的灵活性。父类的模板方法确保了算法的结构保持不变，同时由子类提供部分步骤的实现

*   4）**不足之处**：每一个不同的实现都需要一个子类实现，导致类的个数增加，使得系统更加庞大

*   5）一般**模板方法都加上 final 关键字**，防止子类重写模板方法

*   6）**使用场景**：当要完成在某个过程，该过程要执行一系列步骤，这一系列的步骤基本相同，但其个别步骤在实现时可能不同，通常考虑用模板方法模式来处理

### 2、模板模式解决豆浆制作问题

编写制作豆浆的程序，说明如下：

*   1）制作豆浆的流程选材 ----> 添加配料 ----> 浸泡 ----> 放到豆浆机打碎

*   2）通过添加不同的配料，可以制作出不同口味的豆浆

*   3）选材、浸泡和放到豆浆机打碎**这几个步骤是一个模板方法，**对于制作每种口味的豆浆都是一样的

*   4）请使用模板方法模式完成

说明：因为模板方法模式比较简单，很容易就想到这个方案，因此就直接使用，不再使用传统的方案来引出模板方法模式

![](<assets/1740030884842.png>)

**核心代码**

```
/**
 * 抽象方法
 */
public abstract class SoyaMilk {
    /**
     * 模板方法，定义为final禁止覆写
     */
    public final void make() {
        System.out.println(">>>>>>豆浆制作开始<<<<<<");
        useSoyBean();
        addIngredients();
        soak();
        mash();
        System.out.println(">>>>>>豆浆制作结束<<<<<<");
    }
// 同包可见、对其他包下的子类可见。
    protected void useSoyBean() {
        System.out.println("Step1. 选用上好的黄豆.");
    }
//添加原材料是抽象方法，因为不同豆浆原材料不一样
    protected abstract void addIngredients();
 
    protected void soak() {
        System.out.println("Step3. 对黄豆和配料进行水洗浸泡.");
    }
 
    protected void mash() {
        System.out.println("Step4. 将充分浸泡过的黄豆和配料放入豆浆机中，开始打豆浆.");
    }
}
/**
 * 花生豆浆
 */
public class PeanutSoyaMilk extends SoyaMilk {
    public PeanutSoyaMilk() {
        System.out.println("============花生豆浆============");
    }
    @Override
    protected void addIngredients() {
        System.out.println("Step2. 加入上好的花生.");
    }
}
/**
 * 红豆豆浆
 */
public class RedBeanSoyaMilk extends SoyaMilk {
    public RedBeanSoyaMilk() {
        System.out.println("============红豆豆浆============");
    }
    @Override
    protected void addIngredients() {
        System.out.println("Step2. 加入上好的红豆.");
    }
}
/**
 * 芝麻豆浆
 */
public class SesameSoyaMilk extends SoyaMilk {
        public SesameSoyaMilk() {
        System.out.println("============芝麻豆浆============");
    }
    @Override
    protected void addIngredients() {
        System.out.println("Step2. 加入上好的芝麻.");
    }
}
```

**客户端调用模板方法**

```
SoyaMilk peanutSoyaMilk = new PeanutSoyaMilk();
peanutSoyaMilk.make();
SoyaMilk redBeanSoyaMilk = new RedBeanSoyaMilk();
redBeanSoyaMilk.make();
SoyaMilk sesameSoyaMilk = new SesameSoyaMilk();
sesameSoyaMilk.make();
/*
============花生豆浆============
>>>>>>豆浆制作开始<<<<<<
Step1. 选用上好的黄豆.
Step2. 加入上好的花生.
Step3. 对黄豆和配料进行水洗浸泡.
Step4. 将充分浸泡过的黄豆和配料放入豆浆机中，开始打豆浆.
>>>>>>豆浆制作结束<<<<<<
============红豆豆浆============
>>>>>>豆浆制作开始<<<<<<
Step1. 选用上好的黄豆.
Step2. 加入上好的红豆.
Step3. 对黄豆和配料进行水洗浸泡.
Step4. 将充分浸泡过的黄豆和配料放入豆浆机中，开始打豆浆.
>>>>>>豆浆制作结束<<<<<<
============芝麻豆浆============
>>>>>>豆浆制作开始<<<<<<
Step1. 选用上好的黄豆.
Step2. 加入上好的芝麻.
Step3. 对黄豆和配料进行水洗浸泡.
Step4. 将充分浸泡过的黄豆和配料放入豆浆机中，开始打豆浆.
>>>>>>豆浆制作结束<<<<<<
*/
```

### 3、钩子方法

在模板方法模式的父类中，我们可以定义一个**方法，它默认不做任何事，子类可以视情况要不要覆盖它**，该方法称为 “钩子”

还是用上面做豆浆的例子来讲解，比如，我们还希望制作纯豆浆，不添加任何的配料，请使用钩子方法对前面的模板方法进行改造

**抽象类和具体类**

```
//抽象模板类
public abstract class SoyaMilk {
//模板方法
    public final void make() {
        // ...
//如果钩子方法决定加配料，就加配料；否则不执行加配料操作。
        if (customAddIngredients()) {
            addIngredients();
        }
        // ...
    }
//钩子方法，决定是否需要添加配料。默认情况是加配料。
    boolean customAddIngredients() {
        return true;
    }
    // ...
}
 
/**
 * 纯豆浆
 */
public class PureSoyaMilk extends SoyaMilk {
    public PureSoyaMilk() {
        System.out.println("============纯豆浆============");
    }
 
    @Override
    protected void addIngredients() {
        // 空实现即可
    }
 
    @Override
    protected Boolean customAddIngredients() {
        return false;
    }
}
```

**客户端，测试钩子方法**

```
SoyaMilk pureSoyaMilk = new PureSoyaMilk();
pureSoyaMilk.make();
/*
============纯豆浆============
>>>>>>豆浆制作开始<<<<<<
Step1. 选用上好的黄豆.
Step3. 对黄豆和配料进行水洗浸泡.
Step4. 将充分浸泡过的黄豆和配料放入豆浆机中，开始打豆浆.
>>>>>>豆浆制作结束<<<<<<
*/
```

### 4、Spring 框架 AbstractApplicationContext 抽象类

AbstractApplicationContext.java 中有一个 refresh() 方法就是模板方法，它用于根据流程调用 aop 代理创建、bean 生命周期初始化、属性注入等启动并初始化 Spring 应用上下文的方法。

**AbstractApplicationContext** 抽象类是 ApplicationContext 接口的一种默认实现，提供了一些**通用的应用上下文功能**，同时也为其他具体的应用上下文实现类提供了一些可扩展的方法。

**应用上下文：**负责管理各种 bean 以及它们之间的关系，并对它们进行生命周期的管理。 

![](<assets/1740030885020.png>)

```
// 模板方法
public void refresh() throws BeansException, IllegalStateException {
    synchronized (this.startupShutdownMonitor) {
        prepareRefresh();
        ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();
        prepareBeanFactory(beanFactory);
        try {
            postProcessBeanFactory(beanFactory); // 钩子方法
            invokeBeanFactoryPostProcessors(beanFactory);
            registerBeanPostProcessors(beanFactory);
            initMessageSource();
            initApplicationEventMulticaster();
            onRefresh(); // 钩子方法
            registerListeners();
            finishBeanFactoryInitialization(beanFactory);
            finishRefresh();
        }
        catch (BeansException ex) {
            if (logger.isWarnEnabled()) {
                logger.warn("Exception encountered during context initialization - " +
                            "cancelling refresh attempt: " + ex);
            }
            destroyBeans();
            cancelRefresh(ex);
            throw ex;
        }
        finally {
            resetCommonCaches();
        }
    }
}
protected ConfigurableListableBeanFactory obtainFreshBeanFactory() {
    refreshBeanFactory(); // 抽象方法
    ConfigurableListableBeanFactory beanFactory = getBeanFactory(); // 抽象方法
    if (logger.isDebugEnabled()) {
        logger.debug("Bean factory for " + getDisplayName() + ": " + beanFactory);
    }
    return beanFactory;
}
protected void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) {
}
protected void onRefresh() throws BeansException {
    // For subclasses: do nothing by default.
}
```

### 5、JUC 包下的 AQS 抽象队列同步器

AQS 是基于模板方法模式进行设计的。锁的实现类需要继承 AQS 并重写它指定的方法。

AQS 的模板方法将 “**管理同步状态的逻辑**” 提炼出来形成标准流程，这些方法主要包括：独占式获取同步状态、独占式释放同步状态、共享式获取同步状态、共享式释放同步状态。

**AQS（AbstractQueuedSynchronizer 抽象队列同步器）：**

AQS 是实现锁或者其他同步器（用于协调线程之间对共享资源的访问）的核心框架，通过内部的状态变量和队列来实现线程的同步。ReentrantLock，ThreadPoolExecutor，CountDownLatch 等都是基于 AQS 实现。

*   **同步状态：**在 AQS 中，volatile 类型的 state 变量表示锁的状态，通过 CAS 原子操作这个状态变量来保证线程安全。
*   **同步队列：**在 AQS 中，FIFO（先入先出）队列用来管理等待锁的线程，队列每个节点记录等待线程的状态以及等待锁的条件。先入先出确保同步器的公平性，也就是先等待的线程先获得锁。

**实现线程同步的原理：**线程通过 CAS 原子性修改 state 变量，修改成功则获得锁，失败则插入队尾等待。 

**基于模板方法：**AQS 是基于模板方法模式进行设计的。锁的实现类需要继承 AQS 并重写它指定的方法。

AQS 的模板方法将 “**管理同步状态的逻辑**” 提炼出来形成标准流程，这些方法主要包括：独占式获取同步状态、独占式释放同步状态、共享式获取同步状态、共享式释放同步状态。

## 6、应用场景：AQS 

AQS 是基于模板方法模式进行设计的。锁的实现类需要继承 AQS 并重写它指定的方法。

AQS 的模板方法将 “**管理同步状态的逻辑**” 提炼出来形成标准流程，这些方法主要包括：独占式获取同步状态、独占式释放同步状态、共享式获取同步状态、共享式释放同步状态。 

**AQS（AbstractQueuedSynchronizer 抽象队列同步器）：**

AQS 是一个抽象类，在 JUC.locks 包下，它通过内部的状态变量和同步队列来实现线程的同步（允许多个线程协作共享访问共享资源）。很多锁和同步器都是基于 AQS 实现的。ReentrantLock，ThreadPoolExecutor，CountDownLatch 等都是基于 AQS 实现。

*   **ReentrantLock：**可重入锁，同一个线程在持有锁的情况下，可以重复地获取该锁，无需等待，只需记录重入次数。能防止死锁，因为不用线程自己等待自己释放锁。是 Lock 接口的实现类。可以通过构造参数 true 或 false 指定公平锁或非公平锁，可以通过 newCondition() 方法创建多个 Condition 对象分组唤醒等待线程。
    *   **公平锁：**按加锁顺序获取锁。线程竞争锁时判断 AQS 队列里有没有等待线程，有就加入队尾。
    *   **非公平锁（默认）：**可能某个线程会不断获取锁，牺牲公平的情况下提高了效率。不管 AQS 队列里有没有等待线程，都会先尝试获取锁；如果抢占不到，再加入队尾。如果线程刚好在上个线程释放时拿到锁，就不用像公平锁那样还要阻塞等待、放队尾、唤醒，这些操作涉及到对内核的切换，对性能有影响。
    *   **Condition 对象：**用于线程间通信，通过 await() 和 signal(),signalAll() 让线程等待或唤醒。通常用 lock 锁创建 Condition 对象，即 lock.newCondition();
*   **CountDownLatch：**计数器，它允许一个或多个线程等待其他线程完成操作后再执行。countDown() 方法让计数器减一，await() 方法阻塞当前线程直到计数器减为 0。
*   **ThreadPoolExecutor。**

**state 变量和等待队列：**

*   **state 变量：**在 AQS 中，volatile 类型的 state 变量表示锁的状态，通过 CAS 原子操作这个状态变量来保证线程安全。初始是 0，代表没拿到锁，1 代表拿到锁。
*   **同步队列：**在 AQS 中，FIFO（先入先出）队列用来管理等待锁的线程，队列每个节点记录等待锁的线程的地址、状态、等待锁的条件。先入先出确保同步器的公平性，也就是先等待的线程先获得锁。队列底层是双向链表。

**实现线程同步的原理：**线程通过 CAS 原子性修改 state 变量，修改成功则获得锁，失败则插入队尾等待。 

**基于模板方法：**AQS 是基于模板方法模式进行设计的。锁的实现类需要继承 AQS 并重写它指定的方法。

AQS 的模板方法将 “**管理同步状态的逻辑**” 提炼出来形成标准流程，这些方法主要包括：独占式获取同步状态、独占式释放同步状态、共享式获取同步状态、共享式释放同步状态。

**模板方法：**抽象类有一个模板方法和其他行为方法，模板方法按流程调用各行为方法（抽象或非抽象）；具体子类重写抽象的行为方法。