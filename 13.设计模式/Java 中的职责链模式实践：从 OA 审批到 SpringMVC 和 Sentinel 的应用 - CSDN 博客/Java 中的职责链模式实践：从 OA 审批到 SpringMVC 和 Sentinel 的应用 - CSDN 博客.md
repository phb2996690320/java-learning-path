---
url: https://blog.csdn.net/qq_40991313/article/details/130412327?spm=1001.2014.3001.5502
title: Java 中的职责链模式实践：从 OA 审批到 SpringMVC 和 Sentinel 的应用 - CSDN 博客
date: 2025-02-20 13:54:45
tag: 
summary: 
---
 **导航：** 

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?spm=1001.2014.3001.5501 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1、传统方案，OA 系统的采购审批项目](#t0)

[2、职责链模式基本介绍](#t1)

[3、职责链模式解决 OA 系统采购审批项目](#t2)

[4、职责链模式在 SpringMVC 框架应用的源码分析](#t3)

[5、职责链模式在 Sentinel 中的应用](#t4)

责任链模式类似一个链表，每个具体处理人层层判断对请求的处理权限，没权限的话把请求交给下一个具体处理人。

**抽象处理人：**成员变量是资源和下一个抽象处理人，通过 setNext() 设置下一个抽象处理人（后面会多态形式传参具体处理人），通过 process() 方法处理资源。

**具体处理人：**层层判断处理权限，没权限的话把请求交给下一个具体处理人，有权限就 process() 方法处理资源。

**测试方法：**创建每个具体处理人对象，通过 setNext() 按处理人权限把每个对象串起来。

### 1、传统方案，OA 系统的采购审批项目

学校 OA 系统的采购审批项目，需求是

*   1）采购员采购教学器材

*   2）如果金额小于等于 5000，由教学主任审批（0 < x ≤ 5000）

*   3）如果金额小于等于 10000，由院长审批（5000 < x ≤ 10000）

*   4）如果金额小于等于 30000，由副校长审批（10000< x ≤ 30000）

*   5）如果金额超过 30000 以上，有校长审批（30000  < x）

请设计程序完成采购审批项目

**传统方案解决 OA 系统审批，传统的设计方案（类图）**

![](<assets/1740030885712.png>)

**传统方案解决 OA 系统审批问题分析**

**传统方式是：**接收到一个**采购请求**后，**根据采购金额来调用**对应的`Approver`（审批人）完成审批

**传统方式的问题分析：**客户端这里会使用到分支判断（比如`switch`）来对不同的采购请求处理，这样就存在如下问题

*   （1）如果**各个级别的人员审批金额发生变化**，在客户端的也需要变化
*   （2）客户端必须明确的知道有多少个审批级别和访问

这样对一个采购请求进行处理和`Approver`（审批人）就存在**强耦合**关系，不利于代码的扩展和维护

**解决方案：职责链模式**

### 2、职责链模式基本介绍

1）职责链模式（Chain of Responsibility Pattern），又叫责任链模式：**为请求创建了一个接收者对象的链**（简单示意图）。

![](<assets/1740030886005.png>)

  
这种模式对请求的发送者和接收者进行**解耦**

2）职责链模式通常**每个接收者都包含对另一个接收者的引用**。如果一个对象不能处理该请求，那么它会把相同的请求传给下一个接收者，依此类推。如果最后接收者也无法处理，就返回 “无法处理”，或者抛出异常。

3）这种类型的设计模式属于行为型模式

**原理类图**

![](<assets/1740030886286.png>)

职责链模式使**多个对象都有机会处理请求**，从而避免请求的发送者和接收者之间的耦合关系

将这个**对象连成一条链**，并沿着这条链传递该请求，直到有一个对象处理它为止。

*   **Handler** **抽象处理者：**是一个抽象类或接口，里面包含一个处理请求的抽象方法，和另外一个 Handler 作为成员变量。Handler 依赖请求，客户端把请求发给 Handler。上面案例里每个审批者抽象的接口是 Handler。

*   **ConcreteHandler** **具体处理者：**是 Handler 的实现类，处理自己负责的请求，同时可以访问它的**后继者**（即下一个处理者） ；如果可以处理请求，则进行处理，否则交给后继者去处理，从而形成一个职责链。上面案例里每个审批者都是一个具体处理者。

*   **Request 请求：**含有很多属性，表示一个请求。上面案例里采购员采购是一个请求。

**注意事项和细节**

*   1）将**请求和处理分开**，实现解耦，提高系统的灵活性

*   2）简化了对象，使对象不需要知道链的结构，**对象自己不知道下一个结点是谁**（迪米特法则 / 最少知道原则）

*   3）**性能会受到影响**，特别是在链比较长的时候，因此需控制链中最大节点数量，一般通过在`Handler`中设置一个最大节点数量，在`setNext()`方法中判断是否已经超过阀值，超过则不允许该链建立，避免出现超长链无意识地破坏系统性能

*   4）**调试不方便**。采用了类似递归的方式，调试时逻辑可能比较复杂

*   **5）最佳应用场景：**有多个对象可以处理同一个请求时，比如：**多级请求、请假 / 加薪等审批流程**、Java Web 中 Tomcat 对`Encoding`的处理、拦截器

### 3、职责链模式解决 OA 系统采购审批项目

**UML 类图**

![](<assets/1740030886585.png>)

**核心代码**

**购买请求类：**有 id 和价格两个成员变量。

```
/**
 * 采购申请类
 */
public class PurchaseRequest {
    private Integer id;
    private Float price;
 
    public PurchaseRequest(Integer id, Float price) {
        this.id = id;
        this.price = price;
    }
 
    public Integer getId() {
        return id;
    }
 
    public Float getPrice() {
        return price;
    }
}
```

**抽象审批人类：**成员变量有姓名和下一个抽象审批人，方法有处理请求。

```
/**
 * 抽象审批人对象
 */
public abstract class Approver {
    protected Approver nextApprover;
    protected String name;
 
    public Approver(String name) {
        this.name = name;
    }
 
    /**
     * 设置后继者
     *
     * @param nextApprover
     */
    public void setNextApprover(Approver nextApprover) {
        this.nextApprover = nextApprover;
    }
 
    /**
     * 处理请求的方法
     */
    public abstract void processRequest(PurchaseRequest purchaseRequest);
}
```

**具体审批人对象：**主任、院长等具体审批人，继承抽象审批人，实现处理请求的方法，如果请求中金额自己能审批则审批，如果金额不能审批则把请求交给下一个具体人。

```
/**
 * 教学主任审批人
 */
public class TeachDirectorApprover extends Approver {
    public TeachDirectorApprover(String name) {
        super(name);
    }
 
    @Override
    public void processRequest(PurchaseRequest purchaseRequest) {
        if (purchaseRequest.getPrice() <= 5000) {
            System.out.println("请求编号：" + purchaseRequest.getId() + "，处理人：" + this.name);
        } else {
            nextApprover.processRequest(purchaseRequest);
        }
    }
}
/**
 * 院长审批人
 */
public class DepartmentHeadApprover extends Approver {
    public DepartmentHeadApprover(String name) {
        super(name);
    }
 
    @Override
    public void processRequest(PurchaseRequest purchaseRequest) {
        if (purchaseRequest.getPrice() > 5000 && purchaseRequest.getPrice() <= 10000) {
            System.out.println("请求编号：" + purchaseRequest.getId() + "，处理人：" + this.name);
        } else {
            nextApprover.processRequest(purchaseRequest);
        }
    }
}
/**
 * 副校长审批人
 */
public class ViceChancellorApprover extends Approver {
    public ViceChancellorApprover(String name) {
        super(name);
    }
 
    @Override
    public void processRequest(PurchaseRequest purchaseRequest) {
        if (purchaseRequest.getPrice() > 10000 && purchaseRequest.getPrice() <= 30000) {
            System.out.println("请求编号：" + purchaseRequest.getId() + "，处理人：" + this.name);
        } else {
            nextApprover.processRequest(purchaseRequest);
        }
    }
}
/**
 * 副校长审批人
 */
public class ChancellorApprover extends Approver {
    public ChancellorApprover(String name) {
        super(name);
    }
 
    @Override
    public void processRequest(PurchaseRequest purchaseRequest) {
        if (purchaseRequest.getPrice() > 30000) {
            System.out.println("请求编号：" + purchaseRequest.getId() + "，处理人：" + this.name);
        } else {
            nextApprover.processRequest(purchaseRequest);
        }
    }
}
```

测试代码

```
//创建一个请求。id是1，价格是31000.0f
PurchaseRequest purchaseRequest = new PurchaseRequest(1, 31000.0f);
 
//创建相关的审批人
TeachDirectorApprover teachDirectorApprover = new TeachDirectorApprover("童主任");
DepartmentHeadApprover departmentHeadApprover = new DepartmentHeadApprover("王院长");
ViceChancellorApprover viceChancellorApprover = new ViceChancellorApprover("钱副校长");
ChancellorApprover chancellorApprover = new ChancellorApprover("郑校长");
 
//设置后继者（处理人形成环形）
teachDirectorApprover.setNextApprover(departmentHeadApprover);
departmentHeadApprover.setNextApprover(viceChancellorApprover);
viceChancellorApprover.setNextApprover(chancellorApprover);
chancellorApprover.setNextApprover(teachDirectorApprover);
 
//发起一个请求
teachDirectorApprover.processRequest(purchaseRequest); //请求编号：1，处理人：郑校长
```

### 4、职责链模式在 SpringMVC 框架应用的源码分析

`SpringMVC`中`HandlerExecutionChain`类就使用到了职责链模式

首先，需要回顾下`SpringMVC`基本的请求流程，如下图所示

![](<assets/1740030886869.png>)

 首先，当用户会发起一个`request`请求到后台，这个`request`请求首先会经过`DispatcherServlet`，`DispatcherServlet`对象首先会遍历接收到的`HandlerMapping`集合，然后再找到对应的`HandlerMapping`集合，并得到`HandlerExecutionChain`对象。这个`HandlerExecutionChain`对象内部包含了一些拦截器。拿到`HandlerInterceptor`拦截器过后，有以下几个操作

*   首先会调用`HandlerInterceptor`中的`preHandle()`方法

*   然后会调用`HandlerInterceptor`中的`postHandle()`方法

*   最后会调用`HandlerInterceptor`中的`afterCompletion()`方法

现在对`SpringMVC`进行源码分析，首先需要引入`SpringMVC`相关依赖

```
<properties>
    <maven.compiler.source>8</maven.compiler.source>
    <maven.compiler.target>8</maven.compiler.target>
    <org.springframework.version>4.3.7.RELEASE</org.springframework.version>
</properties>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>${org.springframework.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
    <version>${org.springframework.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>${org.springframework.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>${org.springframework.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>${org.springframework.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-expression</artifactId>
    <version>${org.springframework.version}</version>
</dependency>
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>servlet-api</artifactId>
    <version>2.5</version>
    <scope>provided</scope>
</dependency>
```

接下来，我们对其如何调用`HandlerInterceptor`拦截器中的这三个方法一探究竟吧

**1）在 DispatcherServlet 中找到 doDispatch() 方法，发现该方法中定义了一个 HandlerExecutionChain 对象**

![](<assets/1740030887161.png>)

在后续的代码逻辑中，调用了 getHandler() 方法，接收一个 processedRequest 请求对象作为参数，得到初始化的 HandlerExecutionChain 对象

![](<assets/1740030887608.png>)

直接对 mappedHandler 对象进行高亮，方便我们更加直观地看到 mappedHandler 是如何调用上述所说的 preHandle()、postHandle() 和 afterCompletion() 三个方法的  

![](<assets/1740030887905.png>)

 最终，我们找到这样两段代码，很像上述所说的 preHandle() 和 postHandle() 两个方法  
 

```
if (!mappedHandler.applyPreHandle(processedRequest, response)) {
    return;
}
 
mappedHandler.applyPostHandle(processedRequest, response, mv);
```

首先会执行 mappedHandler 的 applyPreHandle() 方法：如果返回为 false，则判断成立，后续代码不再执行；否则继续往下执行，调用 mappedHandler 的 applyPostHandle() 方法

我们对 applyPreHandle() 和 applyPostHandle() 方法进行源码追踪

**2）先看下`applyPreHandle()`方法的源码**

```
boolean applyPreHandle(HttpServletRequest request, HttpServletResponse response) throws Exception {
    HandlerInterceptor[] interceptors = getInterceptors();
    if (!ObjectUtils.isEmpty(interceptors)) {
        for (int i = 0; i < interceptors.length; i++) {
            HandlerInterceptor interceptor = interceptors[i];
            if (!interceptor.preHandle(request, response, this.handler)) {
                triggerAfterCompletion(request, response, null);
                return false;
            }
            this.interceptorIndex = i;
        }
    }
    return true;
}
```

可以发现，`applyPreHandle`方法内部首先会拿到一组`interceptors`拦截器，当拦截器数组不为空时，进行如下处理：

*   首先对`interceptors`拦截器进行了`for`循环遍历，拿到每一个具体的`interceptor`拦截器

*   接着调用了`interceptor`的`preHandle()`方法，如果返回`false`，则执行`triggerAfterCompletion()`方法并进行`return`，此方法结束；否则继续执行相关处理

**3）接着看下`triggerAfterCompletion()`方法的源码**

```
void triggerAfterCompletion(HttpServletRequest request, HttpServletResponse response, Exception ex)
    throws Exception {
 
    HandlerInterceptor[] interceptors = getInterceptors();
    if (!ObjectUtils.isEmpty(interceptors)) {
        for (int i = this.interceptorIndex; i >= 0; i--) {
            HandlerInterceptor interceptor = interceptors[i];
            try {
                interceptor.afterCompletion(request, response, this.handler, ex);
            }
            catch (Throwable ex2) {
                logger.error("HandlerInterceptor.afterCompletion threw exception", ex2);
            }
        }
    }
}
```

可以发现，其中逻辑跟 applyPreHandle() 方法很相似：先对一组 interceptors 拦截器进行遍历，再执行 interceptor 单个拦截器的 afterCompletion() 方法  
**4）最后看下 applyPostHandle() 方法的源码**  
 

```
void applyPostHandle(HttpServletRequest request, HttpServletResponse response, ModelAndView mv) throws Exception {
    HandlerInterceptor[] interceptors = getInterceptors();
    if (!ObjectUtils.isEmpty(interceptors)) {
        for (int i = interceptors.length - 1; i >= 0; i--) {
            HandlerInterceptor interceptor = interceptors[i];
            interceptor.postHandle(request, response, this.handler, mv);
        }
    }
}
```

同样可以发现，其中逻辑跟上述方法也基本一致：先对一组 interceptors 拦截器进行遍历，再执行 interceptor 单个拦截器的 postHandle() 方法。

**总结**

*   `SpringMVC`请求的流程图中，执行了拦截器相关方法，如`interceptor.preHandler()`

*   在处理`SpringMVC`请求时，使用到职责链模式和适配器模式

*   `HandlerExecutionChain`：主要负责请求拦截器的执行和请求处理，但是本身不处理请求，只是将请求分配给 _链上注册处理器_ 执行。这是职责链实现方式，减少职责链本身与处理逻辑之间的耦合，规范了处理流程

*   `HandlerExecutionChain`：维护了`Handlerlnterceptor`的集合，可以向其中注册相应的拦截器

### 5、职责链模式在 Sentinel 中的应用

**责任链模式：**sentinel 在内部创建了一个责任链，责任链是由一系列 ProcessorSlot 接口的实现类组成的，每个 ProcessorSlot 对象负责不同的功能，外部请求想要访问资源需要责任链层层校验和处理。每个具体处理人有权限（例如配置过降级规则 DegradeSlot 有权限）则校验，没权限则交给下一个具体处理人。只有校验通过才可以访问资源，如果校验失败，会抛出 BlockException 异常。

**校验顺序：**降级、黑白名单、构建 ClusterNode 对象（统计 QPS,RT 等）、校验 QPS,RT 等、流控、打印日志

**ProcessorSlot 接口（抽象处理人）：**是一个基于责任链模式的接口，定义了一个 entry() 方法，用于处理入口参数和出口参数的限流和降级逻辑；一个 exit() 方法，用于将权限交给下一个抽象处理人（实际会传参具体处理人）。

**ProcessorSlot 实现类（具体处理人）：**

*   **DegradeSlot：**用于服务降级。如果发现服务超时次数或者报错次数超过限制，DegradeSlot 将禁止再次访问服务，等待一段时间后，DegradeSlot 试探性的放过一个请求，然后根据该请求的处理情况，决定是否再次降级。
*   **AuthoritySlot：**黑白名单校验，按照字符串匹配，如果在黑名单，则禁止访问。
*   **ClusterBuilderSlot：**构建 ClusterNode 对象，该对象用于统计访问资源的 QPS、线程数、异常、响应时间等，每个资源对应一个 ClusterNode 对象。
*   **SystemSlot：**校验 QPS、并发线程数、系统负载、CPU 使用率、平均响应时间是否超过限制，使用滑动窗口算法统计上述这些数据。
*   **StatisticSlot：**用于从多个维度（入口流量、调用者、当前被访问资源）统计响应时间、并发线程数、处理失败个数、处理成功个数等。
*   **FlowSlot：**用于流控，可以根据 QPS 或者每秒并发线程数控制，当 QPS 或者并发线程数超过设定值，便会抛出 FlowException 异常。FlowSlot 依赖于 StatisticSlot 的统计数据。
*   **NodeSelectorSlot：**负责收集资源路径，并将这些资源的调用路径，以树状结构存储起来，用于根据调用路径来限流降级、数据统计。
*   **LogSlot：**打印日志。