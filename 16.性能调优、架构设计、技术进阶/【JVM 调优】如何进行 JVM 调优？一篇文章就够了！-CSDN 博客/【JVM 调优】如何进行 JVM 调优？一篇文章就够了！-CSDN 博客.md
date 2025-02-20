---
url: https://blog.csdn.net/qq_40991313/article/details/132382094
title: 【JVM 调优】如何进行 JVM 调优？一篇文章就够了！-CSDN 博客
date: 2025-02-20 13:57:59
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[一、大厂面试提问方式](#t0)

[二、JVM 调优步骤](#t1)

[三、监控发现问题](#t2)

[四、工具定位问题](#t3) 

[4.1 调优依据](#t4)

[4.2 JDK 自带的命令行调优工具](#t5)

[4.2.3 常用命令总结](#t6)

[4.2.4 jps：查看正在运行的 Java 进程](#t7)

[4.2.5 jstat：查看 JVM 统计信息](#jstat-%E6%9F%A5%E7%9C%8B-jvm-%E7%BB%9F%E8%AE%A1%E4%BF%A1%E6%81%AF)

[概述](#4.2.5.1%C2%A0%E6%A6%82%E8%BF%B0)

[【OOM 案例】jstat 判断内存溢出（OOM）：比较 GC 时长占运行时长的比例](#4.2.5.2%20%E3%80%90OOM%E6%A1%88%E4%BE%8B%E3%80%91jstat%E5%88%A4%E6%96%AD%E5%86%85%E5%AD%98%E6%BA%A2%E5%87%BA%EF%BC%88OOM%EF%BC%89%EF%BC%9A%E6%AF%94%E8%BE%83GC%E6%97%B6%E9%95%BF%E5%8D%A0%E8%BF%90%E8%A1%8C%E6%97%B6%E9%95%BF%E7%9A%84%E6%AF%94%E4%BE%8B)

[【内存泄漏案例】比较老年代内存量上涨速度](#%E5%85%B6%E4%BB%96%E5%8A%9F%E8%83%BD)

[4.2.6 jstack：打印指定进程此刻的线程快照](#t8)

[4.3 JDK 自带的可视化监控工具](#t9)

[4.4 MAT 分析堆转储文件](#t10)

[4.4.1 简介](#t11)

[4.4.2 生成 dump 文件方式](#t12)

[方法一：jmap](#%E6%96%B9%E6%B3%95%E4%B8%80%EF%BC%9Ajmap)

[方法二：Visual VM](#%E6%96%B9%E6%B3%95%E4%BA%8C%EF%BC%9AVisual%20VM)

[方法三：MAT 直接从 Java 进程导出 dump 文件](#%E6%96%B9%E6%B3%95%E4%B8%89%EF%BC%9AMAT%E7%9B%B4%E6%8E%A5%E4%BB%8EJava%E8%BF%9B%E7%A8%8B%E5%AF%BC%E5%87%BAdump%E6%96%87%E4%BB%B6)

[五、JVM 性能调优](#t13)

[5.1 调优 JVM 参数](#t14)

[5.1.0 JVM 常用调优参数查看和汇总](#t15)

[5.1.1 减少停顿时间：MaxGCPauseMillis](#t16)

[5.1.2 提高吞吐量：GCTimeRatio](#t17)

[5.1.3 调整堆内存大小](#t18)

[5.1.4 调整堆内存比例](#t19)

[5.1.5 调整升老年代年龄](#t20)

[扩展：一次完整的 GC 流程](#t21)

[扩展：Full GC](#t22)

[5.1.6 调整大对象阈值](#t23)

[5.1.7 调整 GC 的触发条件](#t24)

[CMS 调整老年代触发回收比例](#5.1.7.1%20CMS%E8%B0%83%E6%95%B4%E8%80%81%E5%B9%B4%E4%BB%A3%E8%A7%A6%E5%8F%91%E5%9B%9E%E6%94%B6%E6%AF%94%E4%BE%8B)

[G1 调整存活阈值](#5.1.7.2%20G1%E8%B0%83%E6%95%B4%E5%AD%98%E6%B4%BB%E9%98%88%E5%80%BC)

[5.1.8 【最有效】选择合适的垃圾回收器](#t25)

[5.1 排查大对象](#t26)

[5.1.1 内存溢出](#t27)

[概念](#5.1.1.1%20%E6%A6%82%E5%BF%B5)

[OOM 的排查和解决](#5.1.1.2%C2%A0OOM%E7%9A%84%E6%8E%92%E6%9F%A5%E5%92%8C%E8%A7%A3%E5%86%B3)

[5.1.2 内存泄漏](#t28)

[概念](#5.1.2.1%20%E6%A6%82%E5%BF%B5)

[内存泄漏的排查和解决](#5.1.2.2%20%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E7%9A%84%E6%8E%92%E6%9F%A5%E5%92%8C%E8%A7%A3%E5%86%B3)

[5.2 CPU 飙升和 GC 频繁的调优方案](#t29)

[5.2.1 CPU 飙升](#t30) 

[原因](#5.2.1.1%20%E5%8E%9F%E5%9B%A0)

[定位步骤](#5.2.1.2%C2%A0%E5%AE%9A%E4%BD%8D%E6%AD%A5%E9%AA%A4)

[5.2.2 GC 调优](#t31)

[GC 频率的合理范围](#5.2.2.1%20GC%E9%A2%91%E7%8E%87%E7%9A%84%E5%90%88%E7%90%86%E8%8C%83%E5%9B%B4)

[监控发现问题](#5.2.2.2%20%E7%9B%91%E6%8E%A7%E5%8F%91%E7%8E%B0%E9%97%AE%E9%A2%98)

[命令行分析问题](#5.2.2.3%20%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%88%86%E6%9E%90%E9%97%AE%E9%A2%98)

[解决方案](#5.2.2.4%20%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88)

[调优效果](#5.2.2.5%C2%A0%E8%B0%83%E4%BC%98%E6%95%88%E6%9E%9C)

[5.3 其他优化方案](#t32)

[5.3.1 优化业务代码](#t33)

[5.3.2 增加机器](#t34)

[5.3.3 调整线程池参数](#t35)

[5.3.4 缓存、MQ 等中间件优化](#t36)

## **一、**大厂面试提问方式

**面试题汇总：**

[【Java 面试题汇总】JVM 篇（2023 版）_jvm 面试题 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/130232389 "【Java面试题汇总】JVM篇（2023版）_jvm面试题-CSDN博客") 

**诊断分析工具：**

*   JVM 诊断调优工具用过哪些?
*   JVM 相关的分析工具使用过的有哪些? 具体的性能调优步骤如何

**调优：** 

*   如何进行 JVM 调优? 有哪些方法?
*   JVM 性能调优都做了什么?
*   有做过 JVM 内存优化吗?
*   如何对垃圾回收器的性能进行调优?
*   从 SQL、JVM、架构、数据库四个方面讲讲优化思路
*   JVM 如何调优、参数怎么调?
*   堆内存、栈空间设置多少合适
*   每秒几十万并发的秒杀系统为什么会频繁发生 GC?
*   日均百万级交易系统如何优化 JVM?

**生产环境调优：**

*   线上生产系统 OOM 如何监控及定位与解决?
*   高并发系统如何基于 G1 垃圾回收器优化性能?
*   生产环境发生了内存溢出该如何处理
*   生产环境应该给服务器分配多少内存合适?
*   生产环境 CPU 负载飙高该如何处理?
*   生产环境应该给应用分配多少线程合适?
*   不加 log，如何确定请求是否执行了某一行代码?
*   不加 log，如何实时查看某个方法的入参与返回值?
*   线上出现 oom 错误，怎么处理？ 

**内存泄漏：**

*   如何理解内存泄漏问题? 有哪些情况会导致内存泄漏? 如何解决?

## 二、**JVM** 调优步骤

在项目开发过程中、生产环境中，任何问题的解决、性能的调优总结下来都是三个步骤，即发现问题、定位问题、解决问题，本文将从这个步骤入手，详细阐述内存溢出（OOM、OutOfMemeory）、CPU 飙高、GC 频繁等 JVM 问题的排查、定位，以及调优。 

1.  监控发现问题
2.  工具分析问题 
3.  性能调优 

## 三、监控发现问题

通过监控工具例如 Prometheus+Grafana，监控服务器有没有以下情况，有的话需要调优：

*   GC 频繁
*   CPU 负载过高
*   OOM
*   内存泄露
*   死锁
*   程序响应时间较长

## 四、工具定位问题 

使用分析工具定位 oom、内存泄漏等问题。

### **4.1 调优依据**

JVM 调优时，吞吐量和停顿时长无法兼顾，吞吐量提高的代价是停顿时间拉长。

所以，如果应用程序跟用户基本不交互，就优先提升吞吐量。如果应用程序和用户频繁交互，就优先缩短停顿时间。

### **4.2 JDK 自带的命令行调优工具**

#### **4.2.3 常用命令总结**

**jps：**查看正在运行的 Java 进程。jps -v 查看进程启动时的 JVM 参数；

**jstat：**查看指定进程的 JVM 统计信息。jstat -gc 查看堆各分区大小、YGC,FGC 次数和时长。如果服务器没有 GUI 图形界面，只提供了纯文本控制台环境，它是运行期定位虚拟机性能问题的首选工具。

**jinfo：**实时查看和修改指定进程的 JVM 配置参数。jinfo -flag 查看和修改具体参数。

**jstack：**打印指定进程此刻的线程快照。定位线程长时间停顿的原因，例如死锁、等待资源、阻塞。如果有死锁会打印线程的互相占用资源情况。

#### **4.2.4** jps：查看正在运行的 Java 进程

jps(Java Process Status)：显示指定系统内所有的 HotSpot 虚拟机进程（查看虚拟机进程信息），可用于查询正在运行的虚拟机进程。

说明：对于本地虚拟机进程来说，进程的本地虚拟机 ID 与操作系统的进程 ID 是一致的，是唯一的。

**基本使用语法为：**

```
public class ScannerTest {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String info = scanner.next();
    }
}
```

**代码示例**

一个阻塞状态的线程，等待用户输入：

```
jstat -<option> [-t] [-h<lines>] <vmid> [<interval> [<count>]]
#[-t]是程序开启到采样的运行时间 interval查询间隔 count查询次数 [-h<lines>]周期性输出，每隔多少行打印一次表头
```

运行后，在命令行输入 jps 查看进程：

![](<assets/1740031079805.png>)

我们还可以通过追加参数，来打印额外的信息。 

**options 参数：**

*   -q：仅仅显示 LVMID（local virtual machine id），即本地虚拟机唯一 id。不显示主类的名称等
    
*   -l：输出应用程序主类的全类名或如果进程执行的是 jar 包，则输出 jar 完整路径
    
*   -m：输出虚拟机进程启动时传递给主类 main() 的参数
    
*   **-v：列出虚拟机进程启动时的 JVM 参数。**比如：`-Xms20m -Xmx50m` 是启动程序指定的 jvm 参数
    

说明：以上参数可以综合使用。

**案例：**

![](<assets/1740031079896.png>)

![](<assets/1740031080000.png>)

![](<assets/1740031080099.png>)

 

![](<assets/1740031080188.png>)

![](<assets/1740031080285.png>)

**补充：**如果某 Java 进程关闭了默认开启的 UsePerfData 参数（即使用参数 -XX:-UsePerfData），那么 jps 命令（以及下面介绍的 jstat）将**无法探知**该 Java 进程。

![](<assets/1740031080372.png>)

![](<assets/1740031080466.png>) 

**hostid 参数：**

RMI 注册表中注册的主机名。如果想要远程监控主机上的 java 程序，需要安装 jstatd。

对于具有更严格的安全实践的网络场所而言，可能使用一个自定义的策略文件来显示对特定的可信主机或网络的访问，尽管这种技术容易受到 IP 地址欺诈攻击。

如果安全问题无法使用一个定制的策略文件来处理，那么最安全的操作是不运行 jstatd 服务器，而是在本地使用 jstat 和 jps 工具。

#### **4.2.5** jstat：查看 JVM 统计信息

##### 概述

jstat（JVM Statistics Monitoring Tool）：用于监视虚拟机各种运行状态信息的命令行工具。它可以显示本地或者远程虚拟机进程中的**类装载、内存、垃圾收集、JIT 编译等运行数据**。在没有 GUI 图形界面，**只提供了纯文本控制台环境的服务器上**，它将是**运行期定位虚拟机性能问题**的首选工具。常用于检测垃圾回收问题以及内存泄漏问题。

基本使用语法为：

```
jstat -class 86517
Loaded  Bytes  Unloaded  Bytes     Time
 18051 32345.3        0     0.0     112.14
```

查看命令相关参数：`jstat-h` 或 `jstat-help`。

其中 vmid 是进程 id 号，也就是 jps 之后看到的前面的号码，如下：

![](<assets/1740031080560.png>)

**option 参数：**

选项 option 可以由以下值构成：

**类装载相关的**：

*   **-class：**显示 ClassLoader 的相关信息：类的装载、卸载数量、总空间、类装载所消耗的时间等

**垃圾回收相关的**：

*   **-gc：显示堆各分区大小、YGC,FGC 次数和时长。**包括 Eden 区、两个 Survivor 区、老年代、永久代等的容量、已用空间、GC 时间合计等信息
*   -gccapacity：显示内容与 `-gc` 基本相同，但输出主要关注 Java 堆各个区域使用到的最大、最小空间
*   -gcutil：显示内容与 `-gc` 基本相同，但输出主要关注已使用空间占总空间的百分比
*   -gccause：与 `-gcutil` 功能一样，但是会额外输出导致最后一次或当前正在发生的 GC 产生的原因
*   -gcnew：显示新生代 GC 状况
*   -gcnewcapacity：显示内容与 `-gcnew` 基本相同，输出主要关注使用到的最大、最小空间
*   -geold：显示老年代 GC 状况
*   -gcoldcapacity：显示内容与 `-gcold` 基本相同，输出主要关注使用到的最大、最小空间
*   -gcpermcapacity：显示永久代使用到的最大、最小空间

**JIT 相关的**：

*   -compiler：显示 JIT 编译器编译过的方法、耗时等信息
    
*   -printcompilation：输出已经被 JIT 编译的方法
    

**jstat -class**

```
public class GCTest {
    public static void main(String[] args) {
        ArrayList<byte[]> list = new ArrayList<>();
 
        for (int i = 0; i < 1000; i++) {
            byte[] arr = new byte[1024 * 100];//100KB
            list.add(arr);
            try {
                Thread.sleep(120);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

<table><thead><tr><th>显示列名</th><th>具体描述</th></tr></thead><tbody><tr><td><strong>Loaded</strong></td><td>装载的类的数量</td></tr><tr><td><strong>Bytes</strong></td><td>装载的字节数</td></tr><tr><td><strong>Unloaded</strong></td><td>卸载的类的数量</td></tr><tr><td><strong>Bytes</strong></td><td>卸载的类数量</td></tr><tr><td><strong>Time</strong></td><td>装载和卸载的使用时间</td></tr></tbody></table>

**jstat -compiler**

显示 JIT 编译器编译过的方法、耗时等信息。

![](<assets/1740031080657.png>)

**jstat -printcompilation**

输出已经被 JIT 编译的方法。

![](<assets/1740031080747.png>)

**jstat -gc**

显示与 GC 相关的堆信息。包括 Eden 区、两个 Survivor 区、老年代、永久代等的容量、已用空间、GC 时间合计等信息。

执行代码：

```
2022-01-31 21:51:08
Full thread dump Java HotSpot(TM) 64-Bit Server VM (25.231-b11 mixed mode):
 
"DestroyJavaVM" #15 prio=5 os_prio=0 tid=0x0000000002b12800 nid=0x5b50 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE
 
"Thread-1" #13 prio=5 os_prio=0 tid=0x000000001e041800 nid=0x9bc waiting for monitor entry [0x000000001fd1f000]
   java.lang.Thread.State: BLOCKED (on object monitor)
        at com.youngkbt.jstack.ThreadDeadLock$2.run(ThreadDeadLock.java:63)
        - waiting to lock <0x000000076ba1e2f0> (a java.lang.StringBuilder)
        - locked <0x000000076ba1e338> (a java.lang.StringBuilder)
        at java.lang.Thread.run(Thread.java:748)
 
"Thread-0" #12 prio=5 os_prio=0 tid=0x000000001e03b800 nid=0x52f8 waiting for monitor entry [0x000000001fc1f000]
   java.lang.Thread.State: BLOCKED (on object monitor)
        at com.youngkbt.jstack.ThreadDeadLock$1.run(ThreadDeadLock.java:35)
        - waiting to lock <0x000000076ba1e338> (a java.lang.StringBuilder)
        - locked <0x000000076ba1e2f0> (a java.lang.StringBuilder)
 
"Service Thread" #11 daemon prio=9 os_prio=0 tid=0x000000001df8b000 nid=0x3408 runnable [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE
 
"C1 CompilerThread3" #10 daemon prio=9 os_prio=2 tid=0x000000001df47000 nid=0x533c waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE
 
"C2 CompilerThread2" #9 daemon prio=9 os_prio=2 tid=0x000000001df44800 nid=0x4ef8 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE
 
"C2 CompilerThread1" #8 daemon prio=9 os_prio=2 tid=0x000000001df42800 nid=0x22b8 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE
 
"C2 CompilerThread0" #7 daemon prio=9 os_prio=2 tid=0x000000001df40000 nid=0x3494 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE
 
"Monitor Ctrl-Break" #6 daemon prio=5 os_prio=0 tid=0x000000001df35800 nid=0x2da8 runnable [0x000000001f4fe000]
   java.lang.Thread.State: RUNNABLE
        at java.net.SocketInputStream.socketRead0(Native Method)
        at java.net.SocketInputStream.socketRead(SocketInputStream.java:116)
        at java.net.SocketInputStream.read(SocketInputStream.java:171)
        at java.net.SocketInputStream.read(SocketInputStream.java:141)
        at sun.nio.cs.StreamDecoder.readBytes(StreamDecoder.java:284)
        at sun.nio.cs.StreamDecoder.implRead(StreamDecoder.java:326)
        at sun.nio.cs.StreamDecoder.read(StreamDecoder.java:178)
        - locked <0x000000076b904d88> (a java.io.InputStreamReader)
        at java.io.InputStreamReader.read(InputStreamReader.java:184)
        at java.io.BufferedReader.fill(BufferedReader.java:161)
        at java.io.BufferedReader.readLine(BufferedReader.java:324)
        - locked <0x000000076b904d88> (a java.io.InputStreamReader)
        at java.io.BufferedReader.readLine(BufferedReader.java:389)
        at com.intellij.rt.execution.application.AppMainV2$1.run(AppMainV2.java:47)
 
"Attach Listener" #5 daemon prio=5 os_prio=2 tid=0x000000001de9e000 nid=0xac waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE
 
"Signal Dispatcher" #4 daemon prio=9 os_prio=2 tid=0x000000001def2000 nid=0x2908 runnable [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE
 
"Finalizer" #3 daemon prio=8 os_prio=1 tid=0x000000001c7c3800 nid=0x1610 in Object.wait() [0x000000001f1df000]
   java.lang.Thread.State: WAITING (on object monitor)
        at java.lang.Object.wait(Native Method)
        - waiting on <0x000000076b788ed8> (a java.lang.ref.ReferenceQueue$Lock)
        at java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:144)
        - locked <0x000000076b788ed8> (a java.lang.ref.ReferenceQueue$Lock)
        at java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:165)
        at java.lang.ref.Finalizer$FinalizerThread.run(Finalizer.java:216)
 
"Reference Handler" #2 daemon prio=10 os_prio=2 tid=0x000000001de83000 nid=0x31e4 in Object.wait() [0x000000001f0de000]
   java.lang.Thread.State: WAITING (on object monitor)
        at java.lang.Object.wait(Native Method)
        - waiting on <0x000000076b786c00> (a java.lang.ref.Reference$Lock)
        at java.lang.Object.wait(Object.java:502)
        at java.lang.ref.Reference.tryHandlePending(Reference.java:191)
        - locked <0x000000076b786c00> (a java.lang.ref.Reference$Lock)
        at java.lang.ref.Reference$ReferenceHandler.run(Reference.java:153)
 
"VM Thread" os_prio=2 tid=0x000000001de62800 nid=0x575c runnable
 
"GC task thread#0 (ParallelGC)" os_prio=0 tid=0x0000000002b28800 nid=0x1768 runnable
 
"GC task thread#1 (ParallelGC)" os_prio=0 tid=0x0000000002b2a000 nid=0x97c runnable
 
"GC task thread#2 (ParallelGC)" os_prio=0 tid=0x0000000002b2c000 nid=0x4364 runnable
 
"GC task thread#3 (ParallelGC)" os_prio=0 tid=0x0000000002b2d800 nid=0x4608 runnable
 
"GC task thread#4 (ParallelGC)" os_prio=0 tid=0x0000000002b2f800 nid=0x4f38 runnable
 
"GC task thread#5 (ParallelGC)" os_prio=0 tid=0x0000000002b32000 nid=0xb80 runnable
 
"GC task thread#6 (ParallelGC)" os_prio=0 tid=0x0000000002b35000 nid=0xce4 runnable
 
"GC task thread#7 (ParallelGC)" os_prio=0 tid=0x0000000002b36000 nid=0x5510 runnable
 
"GC task thread#8 (ParallelGC)" os_prio=0 tid=0x0000000002b37800 nid=0x193c runnable
 
"GC task thread#9 (ParallelGC)" os_prio=0 tid=0x0000000002b38800 nid=0x1010 runnable
 
"VM Periodic Task Thread" os_prio=2 tid=0x000000001e008000 nid=0x1144 waiting on condition
 
JNI global references: 12
 
 
Found one Java-level deadlock:
=============================
"Thread-1":
  waiting to lock monitor 0x000000001e044d58 (object 0x000000076ba1e2f0, a java.lang.StringBuilder),
  which is held by "Thread-0"
"Thread-0":
  waiting to lock monitor 0x000000001c7c2dc8 (object 0x000000076ba1e338, a java.lang.StringBuilder),
  which is held by "Thread-1"
 
Java stack information for the threads listed above:
===================================================
"Thread-1":
        at com.youngkbt.jstack.ThreadDeadLock$2.run(ThreadDeadLock.java:63)
        - waiting to lock <0x000000076ba1e2f0> (a java.lang.StringBuilder)
        - locked <0x000000076ba1e338> (a java.lang.StringBuilder)
        at java.lang.Thread.run(Thread.java:748)
"Thread-0":
        at com.youngkbt.jstack.ThreadDeadLock$1.run(ThreadDeadLock.java:35)
        - waiting to lock <0x000000076ba1e338> (a java.lang.StringBuilder)
        - locked <0x000000076ba1e2f0> (a java.lang.StringBuilder)
 
Found 1 deadlock.
```

JVM 参数：

```
public class TreadSleepTest {
    public static void main(String[] args) {
        System.out.println("hello - 1");
        try {
            Thread.sleep(1000 * 60 * 10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
 
        System.out.println("hello - 2");
    }
}
```

运行后利用命令查询：

![](<assets/1740031080843.png>)

<table><thead><tr><th>表头</th><th>含义（字节）</th></tr></thead><tbody><tr><td>S0C</td><td>幸存者 0 区的大小</td></tr><tr><td>S1C</td><td>幸存者 1 区的大小</td></tr><tr><td>S0U</td><td>幸存者 0 区已使用的大小</td></tr><tr><td>S1U</td><td>幸存者 1 区已使用的大小</td></tr><tr><td>EC</td><td>Eden 区的大小</td></tr><tr><td>EU</td><td>Eden 区已使用的大小</td></tr><tr><td>OC</td><td>老年代的大小</td></tr><tr><td>OU</td><td>老年代已使用的大小</td></tr><tr><td>MC</td><td>元空间的大小</td></tr><tr><td>MU</td><td>元空间已使用的大小</td></tr><tr><td>CCSC</td><td>压缩类空间的大小</td></tr><tr><td>CCSU</td><td>压缩类空间已使用的大小</td></tr><tr><td>YGC</td><td>从应用程序启动到采样时 Young GC 的次数</td></tr><tr><td>YGCT</td><td>从应用程序启动到采样时 Young GC 消耗时间（秒）</td></tr><tr><td>FGC</td><td>从应用程序启动到采样时 Full GC 的次数</td></tr><tr><td>FGCT</td><td>从应用程序启动到采样时的 Full GC 的消耗时间（秒）</td></tr><tr><td>GCT</td><td>从应用程序启动到采样时 GC 的总时间</td></tr></tbody></table>

后面的参数代表 1000 毫秒打印一次，一个打印 10 次。

**jstat -gccapacity**

显示内容与 `-gc` 基本相同，但输出主要关注 Java 堆各个区域使用到的最大、最小空间。

![](<assets/1740031080943.png>)

**jstat -gcutil**

显示内容与 `-gc` 基本相同，但输出主要关注已使用空间占总空间的百分比。

![](<assets/1740031081027.png>)

<table><thead><tr><th>表头</th><th>含义（字节）</th></tr></thead><tbody><tr><td>SO</td><td>Survivor 0 区空间百分比</td></tr><tr><td>S1</td><td>Survivor 1 区空间百分比</td></tr><tr><td>E</td><td>Eden 区空间百分比</td></tr><tr><td>O</td><td>Old 区空间百分比</td></tr><tr><td>N</td><td>方法区空间百分比</td></tr><tr><td>CCS</td><td>压缩空间百分比</td></tr><tr><td>YGC</td><td>从应用程序启动到采样时 Young GC 的次数</td></tr><tr><td>YGCT</td><td>从应用程序启动到采样时 Young GC 消耗时间（秒）</td></tr><tr><td>FGC</td><td>从应用程序启动到采样时 Full GC 的次数</td></tr><tr><td>FGCT</td><td>从应用程序启动到采样时的 Full GC 的消耗时间（秒）</td></tr><tr><td>GCT</td><td>从应用程序启动到采样时 GC 的总时间</td></tr><tr><td></td><td></td></tr></tbody></table>

**jstat -gccause**

与 `-gcutil` 功能一样，但是会额外输出导致最后一次或当前正在发生的 GC 产生的原因。

![](<assets/1740031081116.png>)

**jstat -gcnew**

显示新生代 GC 状况。

![](<assets/1740031081208.png>)

**jstat -gcnewcapacity**

显示内容与 `-gcnew` 基本相同，输出主要关注使用到的最大、最小空间。

![](<assets/1740031081298.png>)

**jstat -gcold**

显示老年代 GC 状况。

![](<assets/1740031081399.png>)

**jstat -gcoldcapacity**

显示内容与 `-gcold` 基本相同，输出主要关注使用到的最大、最小空间。

![](<assets/1740031081536.png>)

**其他参数：**

下面的参数都是配合 option 参数后面使用。

基本使用语法为：

```
public class ThreadSyncTest {
    public static void main(String[] args) {
        Number number = new Number();
        Thread t1 = new Thread(number);
        Thread t2 = new Thread(number);
 
        t1.setName("线程1");
        t2.setName("线程2");
 
        t1.start();
        t2.start();
    }
}
 
class Number implements Runnable {
    private int number = 1;
 
    @Override
    public void run() {
        while (true) {
            synchronized (this) {
 
                if (number <= 100) {
 
                    try {
                        Thread.sleep(500);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
 
                    System.out.println(Thread.currentThread().getName() + ":" + number);
                    number++;
 
                } else {
                    break;
                }
            }
        }
    }
 
}
```

*   **interval 参数**：用于指定输出统计数据的周期，单位为毫秒。即：查询间隔
    
*   **count 参数**：用于指定查询的总次数
    
*   **-t 参数**：可以在输出信息前加上一个 Timestamp 列，显示程序的运行时间。单位：秒
    
    我们可以比较 Java 进程的启动时间以及总 GC 时间（GCT 列），或者两次测量的间隔时间以及总 GC 时间的增量，来得出 GC 时间占运行时间的比例。
    
    如果该比例超过 20%，则说明目前堆的压力较大；如果该比例超过 90%，则说明堆里几乎没有可用空间，随时都可能抛出 OOM 异常。
    
*   **-h 参数**：可以在周期性数据输出时，输出多少行数据后输出一个表头信息
    

![](<assets/1740031081646.png>)

**jstat -t**

可以在输出信息前加上一个 Timestamp 列，显示程序的运行时间。单位：秒。

![](<assets/1740031081743.png>)

**jstat -t -h**

可以在周期性数据输出时，输出多少行数据后输出一个表头信息。

![](<assets/1740031081843.png>)

##### **【OOM 案例】**jstat 判断内存溢出（OOM）：比较 **GC 时长占运行时长的比例**

我们可以比较 Java 进程的启动时长以及总 GC 时长 (GCT 列)，或者两次测量的间隔时长以及总 GC 时长的增量，来得出 **GC 时长占运行时长的比例**。

如果该比例超过 **20%**，则说明目前堆的压力较大;

如果该比例超过 **98%**，则说明这段时期内几乎一直在 GC，堆里几乎没有可用空间，随时都可能抛出 **OOM 异常**。

**示例：**统计两次测量的时间间隔内，GC 时长占运行时长的比例： 

使用 jstat 统计 GC 信息，并显示进程启动时间、统计间隔 1000ms、统计 20 次

![](<assets/1740031081938.png>)

##### **【**内存泄漏案例**】**比较老年代内存量上涨速度

每隔一段较长的时间采样多组 OU（老年代内存量） 的最小值，如果这些最小值在上涨，说明无法回收对象在不断增加，可能是内存泄漏导致的。

*   在长时间运行的 Java 程序中，我们可以运行 jstat 命令连续获取多行性能数据，并取这几行数据中 OU 列（Old Used，已占用的老年代内存）的最小值
    
*   然后，我们每隔一段较长的时间重复一次上述操作，来**获得多组 OU 最小值**。如果这些值呈上涨趋势，则说明该 Java 程序的**老年代内存已使用量**在不断上涨，这意味着无法回收的对象在不断增加，因此很有**可能**存在内存泄漏（不再使用的对象仍然被引用，导致 GC 无法回收）。
    

#### **4.2.6** jstack：打印指定进程此刻的线程快照

官方帮助文档：`https://docs.oracle.com/en/java/javase/11/tools/jstack.html`

**jstack（JVM Stack Trace）：**用于生成虚拟机指定进程当前时刻的线程快照（虚拟机堆栈跟踪）。

**线程快照：**该进程内每条线程正在执行的方法堆栈的集合。

生成线程快照的作用：可用于定位线程出现长时间停顿的原因，如线程间死锁、死循环、请求外部资源导致的长时间等待等问题。这些都是导致线程长时间停顿的常见原因。当线程出现停顿时，就可以用 jstack 显示各个线程调用的堆栈情况。

在 thread dump 中，要留意下面几种状态

*   死锁，Deadlock（重点关注）
*   等待资源，Waiting on condition（重点关注）
*   等待获取监视器，Waiting on monitor entry（重点关注）
*   阻塞，Blocked（重点关注）
*   执行中，Runnable
*   暂停，Suspended
*   对象等待中，`Object.wait()` 或 `TIMED＿WAITING`
*   停止，Parked

<table><thead><tr><th>option 参数</th><th>作用</th></tr></thead><tbody><tr><td>-F</td><td>当正常输出的请求不被响应时，强制输出线程堆栈</td></tr><tr><td>-l</td><td>除堆栈外，显示关于锁的附加信息</td></tr><tr><td>-m</td><td>如果调用本地方法的话，可以显示 C/C++ 的堆栈</td></tr></tbody></table>

代码示例（死锁）

![](<assets/1740031082041.png>)

![](<assets/1740031082133.png>)

运行后，在命令行使用该命令：

输出的代码：

```
public class AllStackTrace {
    public static void main(String[] args) {
        Map<Thread, StackTraceElement[]> all = Thread.getAllStackTraces();
        Set<Map.Entry<Thread, StackTraceElement[]>> entries = all.entrySet();
        for(Map.Entry<Thread, StackTraceElement[]> en : entries){
            Thread t = en.getKey();
            StackTraceElement[] v = en.getValue();
            System.out.println("【Thread name is :" + t.getName() + "】");
            for(StackTraceElement s : v){
                System.out.println("\t" + s.toString());
            }
        }
    }
}
```

部分图：（可以看出 BLOCKED 进入死锁阻塞）

![](<assets/1740031082218.png>)

其他代码示例：

因为内容结果太长，所以只给代码，在命令行的输入可以自行练习查看（也可以使用 option 参数查看额外内容）。

线程睡眠代码：

```
// 开启在出现 OOM 错误时生成堆转储文件
-Xmx1024m
-XX:+HeapDumpOnOutOfMemoryError
// 将生成的堆转储文件保存到 /tmp 目录下，并以进程 ID 和时间戳作为文件名
-XX:HeapDumpPath=/tmp/java_%p_%t.hprof
 
// 在进行 Full GC 前生成堆转储文件
// 注：如果没有开启自动 GC，则此参数无效。JDK 9 之后该参数已被删除。
-XX:+HeapDumpBeforeFullGC
```

线程同步代码：

```
//调整内存大小
-XX:MetaspaceSize=128m（元空间默认大小）
-XX:MaxMetaspaceSize=128m（元空间最大大小）
-Xms1024m（堆最大大小）
-Xmx1024m（堆默认大小）
-Xmn256m（新生代大小）
-Xss256k（栈最大深度大小）
 
//调整内存比例
 //伊甸园:幸存区
-XX:SurvivorRatio=8（伊甸园:幸存区=8:2）
 //新生代和老年代的占比
 -XX:NewRatio=4  //表示新生代:老年代 = 1:4 即老年代占整个堆的4/5；默认值=2
 
//修改垃圾回收器
//设置Serial垃圾收集器（新生代）
//-XX:+UseSerialGC
 //设置PS+PO,新生代使用功能Parallel Scavenge 老年代将会使用Parallel Old收集器
//-XX:+UseParallelOldGC
 //CMS垃圾收集器（老年代）
//-XX:+UseConcMarkSweepGC
 //设置G1垃圾收集器
-XX:+UseG1GC
 
//GC停顿时间，垃圾收集器会尝试用各种手段达到这个时间
 -XX:MaxGCPauseMillis
 
 //进入老年代最小的GC年龄,年轻代对象转换为老年代对象最小年龄值，JDK8默认值15，JDK9默认值7
 -XX:InitialTenuringThreshold=7
 //新生代可容纳的最大对象,大于则直接会分配到老年代，0代表没有限制。
  -XX:PretenureSizeThreshold=1000000
 
 //使用多少比例的老年代后开始CMS收集，默认是68%，如果频繁发生SerialOld卡顿，应该调小
 -XX:CMSInitiatingOccupancyFraction 
 //G1混合垃圾回收周期中要包括的旧区域设置占用率阈值。默认占用率为 65%
 -XX:G1MixedGCLiveThresholdPercent=65
 
 //Heap Dump（堆转储）文件
 //当发生OutOfMemoryError错误时，自动生成堆转储文件。
-XX:+HeapDumpOnOutOfMemoryError 
 //错误输出地址
-XX:HeapDumpPath=/Users/a123/IdeaProjects/java-test/logs/dump.hprof
 
 //GC日志
-XX:+PrintGCDetails（打印详细GC日志）
-XX:+PrintGCTimeStamps：打印GC时间戳（以基准时间的形式）
-XX:+PrintGCDateStamps：打印GC时间戳（以日期格式）
-Xlog:gc:（打印gc日志地址）
```

在控制台输出结果的代码：

```
//GC停顿时间，垃圾收集器会尝试用各种手段达到这个时间
 -XX:MaxGCPauseMillis=10
```

### 4.3 JDK 自带的可视化监控工具

*   jconsole
*   Visual VM：Visual VM 可以监视应用程序的 CPU、GC、堆、方法区、线程快照，查看 JVM 进程、JVM 参数、系统属性。

### **4.4 MAT 分析堆转储文件**

#### **4.4.1 简介**

**MAT 简介：**MAT 可以解析 Heap Dump（堆转储）文件 dump.hprof，查看 GC Roots、引用链、对象信息、类信息、线程信息。可以快速生成内存泄漏报表。

MAT（Memory Analyzer Tool）工具是一款功能强大的 Java 堆内存分析器。可以用于查找内存泄漏以及查看内存消耗情况。

MAT 可以分析 heap dump 文件。在进行内存分析时，只要获得了反映当前设备内存映像的 hprof 文件，通过 MAT 打开就可以直观地看到当前的内存信息。一般说来，这些内存信息包含：

*   所有的对象信息，包括对象实例、成员变量、存储于栈中的基本类型值和存储于堆中的其他对象的引用值
*   所有的类信息，包括 classloader、类名称、父类、静态变量等
*   GCRoot 到所有的这些对象的引用路径
*   线程信息，包括线程的调用栈及此线程的线程局部变量（TLS）

#### **4.4.2 生成 dump 文件方式**

##### 方法一：jmap

jmap（JVM Memory Map）：作用一方面是获取 dump 文件（堆转储快照文件，二进制文件），它还可以获取目标 Java 进程的内存相关信息，包括 Java 堆各区域的使用情况、堆中对象的统计信息、类加载信息等。开发人员可以在控制台中输入命令 `jmap -help` 查阅 jmap 工具的具体使用方式和一些标准选项配置。

**基本语法**

基本使用语法为：

*   `jmap [option] <pid>`
*   `jmap [option] <executable &lt;core>`
*   `jmap [option] [server_id@] <remote server IP or hostname>`

<table><thead><tr><th>选项</th><th>作用</th></tr></thead><tbody><tr><td>-dump</td><td>生成 dump 文件（Java 堆转储快照），-dump:live 只保存堆中的存活对象</td></tr><tr><td>-heap</td><td>输出整个堆空间的详细信息，包括 GC 的使用、堆配置信息，以及内存的使用信息等</td></tr><tr><td>-histo</td><td>输出堆空间中对象的统计信息，包括类、实例数量和合计容量，-histo:live 只统计堆中的存活对象</td></tr><tr><td>-J &lt;flag&gt;</td><td>传递参数给 jmap 启动的 jvm</td></tr><tr><td>-finalizerinfo</td><td>显示在 F-Queue 中等待 Finalizer 线程执行 finalize 方法的对象，仅 linux/solaris 平台有效</td></tr><tr><td>-permstat</td><td>以 ClassLoader 为统计口径输出永久代的内存状态信息，仅 linux/solaris 平台有效</td></tr><tr><td>-F</td><td>当虚拟机进程对 - dump 选项没有任何响应时，强制执行生成 dump 文件，仅 linux/solaris 平台有效</td></tr><tr><td>-h | -help</td><td>jmap 工具使用的帮助命令</td></tr><tr><td>-j &lt;flag&gt;</td><td>传递参数给 jmap 启动的 JVM</td></tr></tbody></table>

说明：这些参数和 linux 下输入显示的命令多少会有不同，包括也受 JDK 版本的影响。

JVM 参数：OOM 后生成、FGC 前生成

##### 方法二：Visual VM

使用 Visual VM 可以导出堆 dump 文件。

**1. 首先启动程序 (需确保程序一直在运行中)**  
**2. 打开 JvisualVM 工具**  
**3. 打开对应的程序进程**  

![](<assets/1740031082315.png>)

  
**4. 点击线程 -> 线程 dump**  

![](<assets/1740031082407.png>)

**5. 右键快照 -> 另存为**  

![](<assets/1740031082502.png>)

  

![](<assets/1740031082593.png>)

##### 方法三：MAT 直接从 Java 进程导出 dump 文件

```
//调整内存大小
-XX:MetaspaceSize=128m（元空间默认大小）
-XX:MaxMetaspaceSize=128m（元空间最大大小）
-Xms1024m（堆最大大小）
-Xmx1024m（堆默认大小）
-Xmn256m（新生代大小）
-Xss256k（栈最大深度大小）
```

## **五、JVM 性能调优**

### 5.1 **调优 JVM 参数**

调优 JVM 参数主要关注停顿时间和吞吐量，两者不可兼得，提高吞吐量会拉长停顿时间。

#### 5.1.0 JVM 常用调优参数查看和汇总

**查看当前 JVM 参数配置：**

```
//调整内存比例
 //伊甸园:幸存区
-XX:SurvivorRatio=8（伊甸园:幸存区=8:2）
 //新生代和老年代的占比
 -XX:NewRatio=4  //表示新生代:老年代 = 1:4 即老年代占整个堆的4/5；默认值=2
```

![](<assets/1740031082684.png>)

 **常用参数：**

```
//进入老年代最小的GC年龄,年轻代对象转换为老年代对象最小年龄值，JDK8默认值15，JDK9默认值7
 -XX:InitialTenuringThreshold=7
```

#### **5.1.1 减少停顿时间：**MaxGCPauseMillis

**STW：**Stop The World，暂停其他所有工作线程直到收集结束。垃圾收集器做垃圾回收中断应用执行的时间。

可以通过 - XX:MaxGCPauseMillis 参数进行设置，以毫秒为单位，至少大于 1。

```
//新生代可容纳的最大对象,大于则直接会分配到老年代，0代表没有限制。
  -XX:PretenureSizeThreshold=1000000
```

G1 回收器默认 200ms 停顿时长。

#### **5.1.2 提高吞吐量：**GCTimeRatio

吞吐量 = 运行时长 /(运行时长 + GC 时长)。

通过 - XX:GCTimeRatio=n 参数可以设置吞吐量，99 代表吞吐量为 99%， 一般吞吐量不能低于 95%。

示例：

```
//使用多少比例的老年代后开始CMS收集，默认是68%，如果频繁发生SerialOld卡顿，应该调小
 -XX:CMSInitiatingOccupancyFraction
```

吞吐量太高会拉长停顿时间，造成用户体验下降。 

#### **5.1.3 调整堆内存大小**

根据程序运行时老年代存活对象大小（记为 x）进行调整，整个堆内存大小设置为 X 的 3~4 倍。年轻代占堆内存的 3/8。

*   **-Xms：**初始堆内存大小。**默认：**物理内存小于 192MB 时，默认为物理内存的 1/2；物理内存大 192MB 且小于 128GB 时，默认为物理内存的 1/4；物理内存大于等于 128GB 时，都为 32GB。
*   **-Xmx：**最大堆内存大小，建议保持和初始堆内存大小一样。因为从初始堆到最大堆的过程会有一定的性能开销，而且现在内存不是稀缺资源。
*   **-Xmn：**年轻代大小。JDK 官方建议年轻代占整个堆大小空间的 3/8 左右。

**示例：**

```
//G1混合垃圾回收周期中要包括的旧区域设置占用率阈值。默认占用率为 65%
 -XX:G1MixedGCLiveThresholdPercent=65
```

#### **5.1.4 调整堆内存比例**

调整伊甸园区和幸存区比例、新生代和老年代比例。

Young GC 频繁时，我们可以提高新生代在堆内存中的比例、提高伊甸园区在新生代的比例，令新生代不那么快被填满。

默认情况，伊甸园区: S0:S1=8:1:1，新生代: 老年代 = 1:2。

**示例：**

```
//修改垃圾回收器
//设置Serial垃圾收集器（新生代）
-XX:+UseSerialGC
```

#### **5.1.5 调整升老年代年龄**

JDK8 时 Young GC 默认把 15 岁的对象移动到老年代。JDK9 默认值改为 7。

当 Full GC 频繁时，我们提高升老年龄，让年轻代的对象多在年轻代待一会，从而降低 Full GC 频率。

```
//修改垃圾回收器
 //设置PS+PO,新生代使用功能Parallel Scavenge 老年代将会使用Parallel Old收集器
-XX:+UseParallelOldGC
```

#### **扩展：一次完整的 GC 流程**

1.  首先，任何新对象都分配到 eden 空间。两个幸存者空间开始时都是空的。
2.  当 eden 空间填满时，将触发一个 **Minor GC**(年轻代的垃圾回收，也称为 Young GC)，删除所有未引用的对象，**大对象**（需要大量连续内存空间的 Java 对象，如那种很长的字符串）直接进入老年代。
3.  所有被引用的对象作为存活对象，将移动到第一个幸存者空间 S0，并标记年龄为 1，即经历过一次 Minor GC。之后每经过一次 Minor GC，年龄 + 1。GC 分代年龄存储在对象头的 Mark Word 里。
4.  当 eden 空间再次被填满时，会执行第二次 Minor GC，将 Eden 和 S0 区中所有垃圾对象清除，并将存活对象复制到 S1 并年龄加 1，此时 S0 变为空。
5.  如此反复在 S0 和 S1 之间切换几次之后，还存活的**年龄等于 15 的对象**（JDK8 默认 15，JDK9 默认 7，-XX:InitialTenuringThreshold=7）在下一次 Minor GC 时将放到老年代中。 
6.  如果老年代内存不足够存储新对象，则会执行 Full GC（清空整个新生代和老年代）。
7.  当老年代满了时会触发 **Full GC 或者 Major GC**（老年代的垃圾回收，清理整个老年代空间）。具体是 Full GC 还是 Major GC 取决于用哪个垃圾回收器，传统垃圾回收器会 Full GC，G1（优先使用 **Mixed GC**，清空部分新生代和老年代）、ZGC（直接抛出内存溢出错误）等现代回收器会避免 Full GC。

![](<assets/1740031082777.png>)

#### **扩展：Full GC**

在触发一下条件时，JVM 会执行 Full GC， 将整个堆（包括整个新生代和老年代）内存清空。

**触发条件：**

*   **调用 System.gc()**：调用 System.gc() （用于请求 JVM 执行垃圾回收）时，JVM 会建议执行 Full GC（具体是否执行取决于垃圾回收器实现的 System.gc() 逻辑，例如 Parallel GC、CMS 等传统垃圾回收器该方法会 Full GC，而 G1、ZGC 等现代回收器不会 Full GC）。
*   **空间分配担保失败**：当对象从新生代晋升到老年代前，要检查老年代是否有足够空间容纳新对象，这叫做空间分配担保。如果空间不足，则空间分配担保失败，JVM 会执行 Full GC。
*   **老年代满了**：当老年代满了时会触发 **Full GC 或者 Major GC**（老年代的垃圾回收，清理整个老年代空间）。具体是 Full GC 还是 Major GC 取决于用哪个垃圾回收器，传统垃圾回收器会 Full GC，G1（优先使用 **Mixed GC**，清空部分新生代和老年代）、ZGC（直接抛出内存溢出错误）等现代回收器会避免 Full GC。
*   **方法区满了**：当方法区（Method Area）空间不足时，会触发 Full GC。

**避免 Full GC：**

Full GC 的执行时间最长，也是 JVM 调优的重点。

*   调大老年代比例
*   增加整个堆内存
*   使用 G1、ZGC 等现代回收器。

#### **5.1.6 调整大对象阈值**

Young GC 时大对象会不顾年龄直接移动到老年代。当 Full GC 频繁时，我们关闭或提高大对象阈值，让老年代更迟填满。

默认是 0，即大对象不会直接在 YGC 时移到老年代。

```
//CMS垃圾收集器（老年代）
-XX:+UseConcMarkSweepGC
```

#### **5.1.7 调整 GC 的触发条件**

##### **CMS 调整老年代触发回收比例**

CMS 的并发标记和并发清除阶段是用户线程和回收线程并发执行，如果老年代满了再回收会导致用户线程被强制暂停。所以我们修改回收条件为老年代的 60%，保证回收时预留足够空间放新对象。CMS 默认是老年代 68% 时触发回收机制。

```
//修改垃圾回收器
 //设置G1垃圾收集器
-XX:+UseG1GC
```

##### **G1 调整存活阈值**

超过存活阈值的 Region，其内对象会被混合回收到老年代。G1 回收时也要预留空间给新对象。存活阈值默认 85%，即当一个内存区块中存活对象所占比例超过 85% 时，这些对象就会通过 Mixed GC 内存整理并晋升至老年代内存区域。

```
//G1混合垃圾回收周期中要包括的旧区域设置占用率阈值。默认占用率为 65%
 -XX:G1MixedGCLiveThresholdPercent=65
```

#### **5.1.8 【最有效】选择合适的垃圾回收器**

**JVM 调优最实用、最有效的方式是升级垃圾回收器**，根据 CPU 核数，升级当前版本支持的最新回收器。

*   CPU 单核，那么毫无疑问 Serial 垃圾收集器是你唯一的选择。
*   CPU 多核，关注吞吐量 ，那么选择 Parallel Scavenge+Parallel Old 组合（JDK8 默认）。
*   CPU 多核，关注用户停顿时间，JDK 版本 1.6 或者 1.7，那么选择 ParNew+CMS，吞吐量降低但是低停顿。
*   CPU 多核，关注用户停顿时间，JDK1.8 及以上，JVM 可用内存 6G 以上，那么选择 G1。

**示例：**

设置 Serial 垃圾收集器（新生代）

```
//修改垃圾回收器
//设置Serial垃圾收集器（新生代）
-XX:+UseSerialGC
```

设置 PS+PO, 新生代使用功能 Parallel Scavenge 老年代将会使用 Parallel Old 收集器 

```
//修改垃圾回收器
 //设置PS+PO,新生代使用功能Parallel Scavenge 老年代将会使用Parallel Old收集器
-XX:+UseParallelOldGC
```

设置 CMS 垃圾收集器（老年代） 

```
//CMS垃圾收集器（老年代）
-XX:+UseConcMarkSweepGC
```

设置 G1 垃圾收集器 

```
//修改垃圾回收器
 //设置G1垃圾收集器
-XX:+UseG1GC
```

### **5.1** 排查大对象

使用 MAT 分析堆转储日志中的大对象，看是否合理。大对象会直接进入老年代，导致 Full GC 频繁。

#### 5.1.1 内存溢出

##### 概念

**内存溢出：** 申请的内存大于系统能提供的内存。

**溢出原因：**

*   **本地直接内存溢出：**本地直接内存设的太小导致溢出。设置直接内存最大值 - XX：MaxDirectMemorySize，若不指定则默认与 Java 堆最大值一致。
*   **虚拟机栈和本地方法栈溢出：**如果虚拟机的栈内存允许动态扩展，并且方法递归层数太深时，导致扩展栈容量时无法申请到足够内存。
*   **方法区溢出：**运行时生成大量动态类时会内存溢出。
    *   **CGlib 动态代理：**CGlib 动态代理产生大量类填满了整个方法区（方法区存常量池、类信息、方法信息），直到溢出。CGlib 动态代理是在内存中构建子类对象实现对目标对象功能扩展，如果 enhancer.setUseCache(false);，即关闭用户缓存，那么每次创建代理对象都是一个新的实例，创建过多就会导致方法区溢出。注意 JDK 动态代理不会导致方法区溢出。
    *   **JSP：**大量 JSP 或动态产生 JSP 文件的应用（JSP 第一次运行时需要编译为 Java 类）。
*   **堆溢出：**
    *   死循环创建过多对象；
    *   集合类中有对对象的引用，使用完后未清空，使得 JVM 不能回收；
    *   内存中加载的数据量过于庞大，如一次从数据库取出的数据集太大、第三方接口接口传输的大对象、接收的 MQ 消息太大；
    *   Tomcat 参数设置不当导致 OOM：Tomcat 会给每个线程创建两个默认 4M 大小的缓冲区，高并发情况下会导致缓冲区创建过多，导致 OOM。
*   程序计数器不会内存溢出。

##### OOM 的排查和解决

**使用 JDK 自带的命令行调优工具 ，判断是否有 OOM：**

1.  使用 jsp 命令查看当前 Java 进程；
2.  使用 jstat 命令多次统计 GC，比较 GC 时长占运行时长的比例；
3.  如果比例超过 20%，就代表堆压力已经很大了；
4.  如果比例超过 98%，说明这段时期内几乎一直在 GC，堆里几乎没有可用空间，随时都可能抛出 OOM 异常。

**MAT 定位导致 OOM：**示例代码：写死循环创建对象，不断添加到 list 里，导致堆内存溢出；

1.  JVM 参数设置，内存溢出后生成 dump 文件，设置路径；
    
    ```
    -XX:+HeapDumpOnOutOfMemoryError、-XX:HeapDumpPath
    ```
    
2.  MAT 解析 dump 文件；
3.  **定位大对象：**点击直方图图标（Histogram），对象会按内存大小排序，查看内存占用最大的对象；
    
    ![](<assets/1740031082884.png>)
    
4.  **这个对象被谁引用：**点击支配树（dominator tree），看大对象被哪个线程调用。这里可以看到是被主线程调用。
    
    ![](<assets/1740031082972.png>)
    
5.  **定位具体代码：**点击概述图标（thread_overview），看线程的方法调用链和堆栈信息，查看大对象所属类和第几行，定位到具体代码，解决问题。
    
    ![](<assets/1740031083086.png>)
    

**解决方案：**

1.  通过 jinfo 命令查看并修改 JVM 参数，直接增加内存。如 - Xmx256m
2.  检查错误日志，查看 “OutOfMemory” 错误前是否有其它异常或错误。
3.  对代码进行走查和分析，找出可能发生内存溢出的位置。
4.  使用内存查看工具动态查看内存使用情况。

#### 5.1.2 内存泄漏

##### 概念

**内存泄漏：** 不再使用的对象仍然被引用，导致 GC 无法回收；

**内存泄露的 9 种情况：**

*   **静态容器里的对象：**静态集合类的生命周期与 JVM 程序一致，容器里的对象引用也将一直被引用得不到 GC；Java 里不准静态方法引用非静态方法也是防止内存泄漏。
*   **单例对象引用的外部对象：**单例模式里，如果单例对象如果持有外部对象的引用，因为单例对象不会被回收，那么这个外部对象也不会被回收
*   **外部类跟随内部类被引用：**内部类持有外部类，这个内部类对象被长期引用了，即使那个外部类实例对象不再被使用，但由于内部类持有外部类的实例对象，这个外部类对象将不会被垃圾回收，这也会造成内存泄漏。
*   **数据库、网络、IO 等连接忘记关闭：**在对数据库进行操作的过程中，首先需要建立与数据库的连接，当不再使用时，需要调用 close 方法来释放与数据库的连接。如果对 Connection、Statement 或 ResultSet 不显性地关闭，将会造成大量的对象无法被回收，从而引起内存泄漏。
*   **变量作用域不合理：**例如一个变量只会在某个方法中使用，却声明为成员变量，并且被使用后没有被赋值为 null，将会导致这个变量明明已经没用了，生命周期却还跟对象一致。
*   **HashSet 中对象改变哈希值：**当一个对象被存储进 HashSet 集合中以后，就不能修改这个对象中的那些参与计算哈希值的字段了。否则对象哈希值改变，找不到对应的 value。
*   **缓存引用忘删除：**一旦你把对象引用放入到缓存中，他就很容易遗忘，缓存忘了删除，将导致引用一直存在。
*   **逻辑删除而不是真实删除：**监听器和其他回调：如果客户端在你实现的 API 中注册回调，却没有显示的取消，那么就会积聚。需要确保回调立即被当作垃圾回收的最佳方法是只保存它的弱引用，例如将他们保存成为 软 WeakHashMap 中的键。例如出栈只是移动了指针，而没有将出栈的位置赋值 null，导致已出栈的位置还存在引用。
*   **线程池时，ThreadLocal 忘记 remove()：**使用线程池的时候，ThreadLocal 需要在使用完线程中的线程变量手动 remove()，否则会内存泄漏。因为线程执行完后没有销毁而是被线程池回收，导致 ThreadLocal 中的对象不能被自动垃圾回收。 

##### 内存泄漏的排查和解决

**性能分析工具判断是否有内存泄漏：**

*   JDK 自带的命令行调优工具：
    *   每隔一段较长的时间通过 jstat 命令采样多组 OU（老年代内存量） 的最小值；
    *   如果这些最小值在上涨，说明无法回收对象在不断增加，可能是内存泄漏导致的。
*   **MAT 监视诊断内存泄漏：**
    *   **生成堆转储文件：**MAT 直接从 Java 进程导出 dump 文件
    *   **可疑点：**查看泄漏怀疑（Leak Suspects），找到内存泄漏可疑点
    *   **可疑线程：**可疑点查看详情（Details），找到可疑线程
    *   **定位代码：**查看线程调用栈（See stacktrace），找到问题代码的具体位置。
*   GC 详细日志：启动参数开启 GC 详细日志，设置日志地址；-XX:+PrintGCDetails；
*   编译器警告：查看 Eclipse 等编译器的内存泄漏警告；
*   Java 基准测试工具：分析代码性能； 

**解决办法：**

1.  牢记内存泄漏的场景，当一个对象不会被使用时，给它的所有引用赋值 null，堤防静态容器，记得关闭连接、别用逻辑删除，只要用到了引用，变量的作用域要合理。
2.  使用 java.lang.ref 包的弱引用 WeakReference，下次垃圾收集器工作时被回收。
3.  检查代码；

### **5.2 CPU 飙升和 GC 频繁的调优方案**

#### **5.2.1 CPU 飙升** 

##### 原因

CPU 利用率过高，大量线程并发执行任务导致 CPU 飙升。例如锁等待（例如 CAS 不断自旋）、**多线程都陷入死循环**、Redis 被攻击、网站被攻击、文件 IO、网络 IO。

##### **定位步骤**

1.  **定位进程 ID：**通过 top 命令查看当前服务 CPU 使用最高的进程，获取到对应的 pid（进程 ID）
2.  **定位线程 ID：**使用 top -Hp pid，显示指定进程下面的线程信息，找到消耗 CPU 最高的线程 id
3.  **线程 ID 转十六进制：**转十六进制是因为下一步 jstack 打印的线程快照（线程正在执行方法的堆栈集合）里线程 id 是十六进制。
4.  **定位代码：**使用 jstack pid | grep tid（十六进制），打印线程快照，找到线程执行的代码。一般如果有死锁的话就会显示线程互相占用情况。
5.  **解决问题：**优化代码、增加系统资源（增多服务器、增大内存）。

#### **5.2.2 GC 调优**

##### **GC 频率的合理范围**

jvm.gc.time：每分钟的 GC 耗时在 1s 以内，500ms 以内尤佳

jvm.gc.meantime：每次 YGC 耗时在 100ms 以内，50ms 以内尤佳

jvm.fullgc.count：最多几小时 FGC 一次，1 天不到 1 次尤佳

jvm.fullgc.time：每次 FGC 耗时在 1s 以内，500ms 以内尤佳

**最差情况下能接受的 GC 频率：**Young GC 频率 10s 一次，每次 500ms 以内。Full GC 频率 10min 一次，每次 1s 以内。

其实一小时一次 Full GC 已经算频繁了，一个不错的应用起码得控制一天一次 Full GC。

##### **监控发现问题**

上午 8 点是我们的业务高峰，一到高峰的时候，用户感觉到明显卡顿，监控工具（例如 Prometheus 和 Grafana）发现 TP99（99% 请求在多少 ms 内完成）时长明显变高，有明显的的毛刺；内存使用率也不稳定，会周期性增大再降低，于是怀疑是 GC 导致。

##### **命令行分析问题**

通过 jstat -gc 观察服务器的 GC 情况，发现 Young GC 频率提高成原来的 10 倍，Full GC 频率提高成原来的四倍。正常 YGC 10min 一次，FGC 10h 一次。异常 YGC 1min 一次，FGC 3h 一次；

所以主要问题是 Young GC 频繁，进而导致 Full GC 频繁。Full GC 频繁会触发 STW，导致 TP99 耗时上升。

##### **解决方案**

*   排查内存泄漏、大对象、BUG；
*   **增大堆内存：**服务器加 8G 内存条，同时提高初始堆内存、最大堆内存。-Xms、-Xmx。
*   **提高新生代比例：**新生代和老年代默认比例是 1:2。-XX:NewRatio = 由 4 改为默认的 2
*   **降低升老年龄：**让存活对象更快进入老年代。-XX:InitialTenuringThreshold=15（JDK8 默认）改成 7（JDK9 默认）
*   **设置大对象阈值：**让大于 1M 的大对象直接进入老年代。-XX:PretenureSizeThreshold=0（默认）改为 1000000（单位是字节）
*   **垃圾回收器升级为 G1：**因为是 JDK8，所以直接由默认的 Parallel Scavenge+Parallel Old 组合，升级为低延时的 G1 回收器。如果是 JDK7 版本，不支持 G1，可以修改成 ParNew+CMS 或 Parallel Scavenge+CMS，以降低吞吐量为代价降低停顿时间。-XX:CMSInitiatingOccupancyFraction
*   **降低 G1 的存活阈值：**超过存活阈值的 Region，其内对象会被混合回收到老年代。降低存活阈值，更早进入老年代。-XX:G1MixedGCLiveThresholdPercent=90 设为默认的 85

##### **调优效果**

调优后我们重新进行了一次压测，发现 TP99 耗时较之前降低 60%。FullGC 耗时降低 80%，YoungGC 次数减少 30%。TP99 耗时基本持平，完全符台预期。

### **5.3 其他优化方案**

#### **5.3.1 优化业务代码**

绝大部分问题都出自代码。

日常开发中，要尽量减少非必要对象的创建，防止死循环创建对象，注意内存泄漏的 12 个场景，防止内存泄漏。

在一些内存占用率高的场景下需要以时间换空间，控制内存使用。

#### **5.3.2 增加机器**

在集群下新增加几个服务器，分散节点压力，可以提高整体效率。

#### **5.3.3 调整线程池参数**

合理设置线程池的线程数量。

下面的参数只是一个预估值，适合初步设置，具体的线程数需要经过压测确定，压榨（更好的利用）CPU 的性能。

**记 CPU 核心数为 N；**

**核心线程数：**

*   CPU 密集型：N+1。数量与 CPU 核数相近是为了不浪费 CPU，并防止频繁的上下文切换，加 1 是为了有线程被阻塞后还能不浪费 CPU 的算力。
*   **I/O 密集型：**2N，或 N/(1 - 阻塞系数)。I/O 密集型任务 CPU 使用率并不是很高，可以让 CPU 在等待 I/O 操作的时去处理别的任务，充分利用 CPU，所以数量就比 CPU 核心数高一倍。有些公司会考虑阻塞系数，阻塞系数是任务线程被阻塞的比例，一般是 0.8~0.9。
*   **实际开发中更适合的公式：**N*((线程等待时间 + 线程计算时间)/ 线程计算时间)

**最大线程数：**设成核心线程数的 2-4 倍。数量主要由 CPU 和 IO 的密集性、处理的数据量等因素决定。

**需要增加线程的情况：**jstack 打印线程快照，如果发现线程池中大部分线程都等待获取任务、则说明线程够用。如果大部分线程都处于运行状态，可以继续适当调高线程数量。

**jstack：**打印指定进程此刻的线程快照。定位线程长时间停顿的原因，例如死锁、等待资源、阻塞。如果有死锁会打印线程的互相占用资源情况。线程快照：该进程内每条线程正在执行的方法堆栈的集合。

#### **5.3.4 缓存、MQ 等中间件优化**

使用中间件提高程序效率，比如缓存、消息队列等