---
url: https://blog.csdn.net/qq_40991313/article/details/126339270
title: Spring 基础 3——AOP，事务管理_filter 和 aop 区别 - CSDN 博客
date: 2025-02-20 09:40:09
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1，AOP 简介](#t0)

[1.1 AOP 概念、作用、方式、应用场景](#t1)

[1.2 AOP 作用](#t2)

[1.3 AOP 核心概念](#t3)

[1.4 Filter 跟 AOP 区别](#t4)

[2，AOP 入门案例](#t5)

[2.1 需求分析](#t6)

[2.2 思路分析](#t7)

[2.3 环境准备](#t8)

[2.4 AOP 代码实现](#t9)

[2.5 知识点 @EnableAspectJAutoProxy,@Aspect,@Pointcut,@Before](#t10)

[3，AOP 工作流程](#t11)

[3.1 AOP 工作流程](#t12)

[3.1.1 AOP 工作流程概述](#t13) 

[3.1.2 验证容器中是否为代理对象](#t14)

[3.2 目标对象和代理对象总结](#t15)

[4，AOP 配置管理](#t16)

[4.1 AOP 切入点表达式](#t17)

[4.1.1 语法格式](#t18)

[4.1.2 通配符](#t19)

[4.1.3 书写技巧](#t20)

[4.2 AOP 通知类型](#t21)

[4.2.1 类型介绍](#t22)

[4.2.2 环境准备](#t23)

[4.2.3 前置通知、后置通知](#t24)

[4.2.4 环绕通知（重要，可以做所有通知类型做的事）](#t25)

[4.2.5 返回后通知、异常后通知（了解）](#t26)

[知识点 @After,@Around@AfterReturning,@AfterThrowing](#t27)

[4.3 案例，业务层接口执行效率](#t28)

[4.3.1 需求分析](#t29)

[4.3.2 环境准备](#t30)

[4.3.3 功能开发](#t31)

[4.4 AOP 通知获取数据](#t32)

[4.0 概述](#t33) 

[4.4.1 环境准备](#t34)

[4.4.2 获取参数](#t35)

[4.4.3 获取返回值](#t36)

[4.4.4 获取异常](#t37)

[4.5 案例，百度网盘密码数据兼容处理](#t38)

[4.5.1 需求分析](#t39)

[4.5.2 环境准备](#t40)

[4.5.3 具体实现](#t41)

[5，AOP 小结](#t42)

[6，AOP 事务管理](#t43)

[6.1 Spring 事务简介](#t44)

[6.2 入门案例，转账](#t45)

[6.2.1 需求分析](#t46)

[6.2.2 环境搭建](#t47)

[6.2.3 事务管理 - 代码实现](#t48)

[知识点 @EnableTransactionManagement，@Transactional](#t49)

[6.3 Spring 事务管理员和事务协调员](#t50)

[6.4 Spring 事务属性](#t51)

[6.4.1 所有事务配置](#t52)

[6.4.2 事务配置 - 事务传播行为的需求案例，转账业务追加日志](#t53)

[6.4.3 事务配置 - 事务传播行为，propagation 属性](#t54)

[6.4.4 非事务方法调用事务方法必须用代理对象调用](#t55)

## 1，AOP 简介

前面我们在介绍 Spring 的时候说过，Spring 有两个**核心**的概念**，一个是`IOC/DI`，一个是`AOP`**。

前面已经对`IOC/DI`进行了系统的学习，接下来要学习它的另一个核心内容，就是 **AOP**。

对于 AOP, 我们前面提过一句话是:**AOP 是在不改原有代码的前提下对代码进行增强。**

对于下面的内容，我们主要就是围绕着这一句话进行展开学习，主要学习两方面内容`AOP核心概念`,`AOP作用。`

### 1.1 AOP 概念、作用、方式、应用场景

**AOP 面向切面编程：**抽取共性代码（即通知），植入到待增强的方法（即切入点）。

**作用：**在不改原有代码的前提下对代码进行增强。

切面：通知和切入点的关系。

**AOP 有两种实现方式：** 

JDK 动态代理（默认）：运行时创建接口的代理实例。

CGLib 代码生成库动态代理：采用底层的字节码技术，当目标对象不存在接口时，创建子类代理的实例。

**应用场景：**日志和事务。

AOP 是一种编程思想，是通过预编译方式和运行期动态代理的方式实现**不修改源代码**的情况下**给程序动态统一添加功能**的技术。面向对象编程将程序抽象成各个层次的对象，而面向切面编程是将程序抽象成各个切面。

所谓切面，相当于应用对象间的横切点，我们可以将其单独抽象为单独的模块。AOP 技术利用一种称为 “横切” 的技术，剖解开封装对象的内部，将影响多个类的公共行为封装到一个可重用的模块中，并将其命名为切面。所谓的**切面**，简单来说就是**与业务无关，却为业务模块所共同调用的逻辑**，将其封装起来便于减少系统的重复代码，降低模块的耦合度，有利用未来的可操作性和可维护性。

利用 AOP 可以对业务逻辑各个部分进行隔离，从而使业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高开发效率。

**AOP** 可以有多种**实现方式**，而 Spring AOP 支持如下两种实现方式。

**- JDK 动态代理：**这是 Java 提供的动态代理技术，可以在**运行时创建接口的代理实例**。Spring AOP **默认**采用这种方式，在接口的代理实例中织入代码。

**- CGLib**(Code Generation Library) **动态代理：**采用底层的字节码技术，在运行时**创建子类代理的实例**。当目标对象**不存在接口时**，Spring AOP 就会采用这种方式，在子类实例中织入代码。

**加分回答 - 应用场景**

在应用场景方面，Spring AOP 为 IoC 的使用提供了更多的便利，一方面，应用可以直接使用 AOP 的功能，设计应用的横切关注点，把跨越应用程序多个模块的功能抽象出来，并通过简单的 AOP 的使用，灵活地编制到模块中，比如可以**通过 AOP 实现应用程序中的日志功能**。另一方面，在 Spring 内部，例如**事务处理**之类的一些支持模块**也是通过 Spring AOP 来实现**的。

AOP 不能增强的类：

1. Spring AOP **只能对 IoC 容器中的 Bean 进行增强**，对于不受容器管理的对象不能增强。

2. 由于 CGLib 采用动态创建子类的方式生成代理对象，所以不能对 final 修饰的类进行代理。

*   **AOP(Aspect Oriented Programming) 面向切面编程**：**一种编程思想，在不改原有代码的前提下对代码进行增强。**

aspect 切面，方面，朝向；oriented 面向的，以... 为方向的；切面 Aspect 面向的 Oriented 编程 Programming

**编程思想：** 

我们都知道 OOP(Object Oriented Programming) 面向对象编程是一种编程思想，那么 AOP 也是一种编程思想。

编程思想主要的内容就是指导程序员该如何编写程序，所以它们两个是不同的`编程范式`。

### 1.2 AOP 作用

*   在**不惊动原始设计的基础上**为其进行**功能增强**。

前面咱们有技术就可以实现这样的功能即**`代理模式`**。

**Spring 的理念：无入侵式 / 无侵入式。**例如依赖注入，没有改变代码，但代码的功能改变了。这里 AOP 也是无侵入式的理念。

### 1.3 AOP 核心概念

为了能更好的理解 AOP 的相关概念，我们准备了一个环境，整个环境的内容我们暂时可以不用关注，最主要的类为:`BookDaoImpl`

```
@Repository
public class BookDaoImpl implements BookDao {
    public void save() {
        //记录程序当前执行执行（开始时间）
        Long startTime = System.currentTimeMillis();
        //业务执行万次
        for (int i = 0;i<10000;i++) {
            System.out.println("book dao save ...");
        }
        //记录程序当前执行时间（结束时间）
        Long endTime = System.currentTimeMillis();
        //计算时间差
        Long totalTime = endTime-startTime;
        //输出信息
        System.out.println("执行万次消耗时间：" + totalTime + "ms");
    }
    public void update(){
        System.out.println("book dao update ...");
    }
    public void delete(){
        System.out.println("book dao delete ...");
    }
    public void select(){
        System.out.println("book dao select ...");
    }
}
```

代码的内容相信大家都能够读懂，对于`save`方法中有计算万次执行消耗的时间。

当在 App 类中从容器中获取 bookDao 对象后，**分别执行**其`save`,`delete`,`update`和`select`方法后会有如下的**打印结果**:

![](<assets/1740015609198.png>)

这个时候，我们就应该有些**疑问**?

*   对于计算万次执行消耗的时间只有 save 方法有，为什么 delete 和 update 方法也会有呢?
*   delete 和 update 方法有，那 select 方法为什么又没有呢?

这个案例中其实就**使用了 Spring 的 AOP**，在不惊动 (改动) 原有设计 (代码) 的前提下，想给谁添加功能就给谁添加。这个也就是 **Spring 的理念：无入侵式 / 无侵入式。**

**Spring 实现方式：**

![](<assets/1740015611589.png>)

(1) 前面一直在强调，Spring 的 AOP 是对一个类的方法在不进行任何修改的前提下实现增强。对于上面的案例中 BookServiceImpl 中有`save`,`update`,`delete`和`select`方法, **这些方法**我们起名**连接点**

(2) 在 BookServiceImpl 的四个方法中，`update`和`delete`只有打印没有计算万次执行消耗时间，但是**在运行的时候已经有该功能**，那也就是说**`update`和`delete`方法都已经被增强**，所以对于**需要增强的方法**我们**起名**叫**切入点**

(3) 执行 BookServiceImpl 的 update 和 delete 方法的时候都被添加了一个计算万次执行消耗时间的功能，**将这个功能抽取到一个方法中**，换句话说就是**存放共性功能的方法**，我们**起名**叫**通知**

(4) **通知是要增强的内容**，会有多个，**切入点是需要被增强的方法**，也会有多个，那哪个切入点需要添加哪个通知，就需要提前将它们之间的关系描述清楚，那么对于**通知和切入点之间的关系**描述，我们**起名**叫**切面**

(5) 通知是一个方法，方法不能独立存在需要被写在一个类中，这个类我们也给起了个名字叫**通知类**

**Spring 的 AOP 核心概念总结:**

![](<assets/1740015614167.png>)

*   **连接点 (JoinPoint)：bean 类的原始方法**
    *   程序执行过程中的任意位置，**粒度为执行方法**、抛出异常、设置变量等
    *   在 SpringAOP 中，理解为**方法的执行**
*   **切入点 (Pointcut): 需要被增强的方法。匹配连接点的式子**
    *   在 SpringAOP 中，一个切入点可以描述一个具体方法，也可也匹配多个方法
        *   一个具体的方法: 如 com.itheima.dao 包下的 BookDao 接口中的无形参无返回值的 save 方法
        *   匹配多个方法: 所有的 save 方法，所有的 get 开头的方法，所有以 Dao 结尾的接口中的任意方法，所有带有一个参数的方法
    *   连接点是全部方法，切入点匹配某些需要被增强的方法。连接点范围要比切入点范围大，**是切入点的方法也一定是连接点**，但是是连接点的方法就不一定要被增强，所以可能不是切入点。
*   **通知 (Advice): 要增强的内容，共性的方法。**在切入点处执行的操作，也就是共性功能
    *   在 SpringAOP 中，功能最终以方法的形式呈现
*   **通知类：**定义通知的 bean 类
*   **切面 (Aspect):** 描述**通知与切入点的对应关系。把通知和切入点绑定到一块，一个通知对应一个切入点**。例如切面是 Before，即通知在切入点之前运行。

### 1.4 Filter 跟 AOP 区别

从思想上来说，Filter 跟 AOP 极其接近  
但是区别明显：  
1、**Filter 只能拦截 request 的请求**  
2、Spring 中的 **AOP**，一般而言，是在 **Service 层，拦截 Bean（实例）的访问。**  
例如使用 @Transcational 来拦截 dao 的应用，让它实现事务的管理。从思想上来说，Filter 跟 AOP 极其接近

**小结**

这一节中主要讲解了 AOP 的概念与作用，以及 AOP 中的核心概念，学完以后大家需要能说出:

*   什么是 AOP?
*   AOP 的作用是什么?
*   AOP 中核心概念分别指的是什么?
    *   连接点
    *   切入点
    *   通知
    *   通知类
    *   切面

## 2，AOP 入门案例

### 2.1 需求分析

**需求:** 使用 SpringAOP 的注解方式完成在方法执行的前打印出当前系统时间。

对于 SpringAOP 的开发有两种方式，XML 和 **注解**，我们使用注解。

因为现在注解使用的比较多，所以本次课程就采用注解完成 AOP 的开发。

### 2.2 思路分析

1. 导入坐标 (pom.xml)

2. 制作连接点 (原始操作，Dao 接口与实现类)

3. 制作共性功能 (通知类与通知)

4. 定义切入点

5. 绑定切入点与通知关系 (切面)

### 2.3 环境准备

*   创建一个 Maven 项目
    
*   **pom.xml 添加 Spring-context 依赖**（目前学了的关于 spring 依赖暂不导入，spring-jdbc,mybatis-spring,spring-test）
    
    ```
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.10.RELEASE</version>
        </dependency>
    </dependencies>
    ```
    
*   添加 **BookDao 和 BookDaoImpl** 类
    
    ```
    public interface BookDao {
        public void save();
        public void update();
    }
     
    @Repository
    public class BookDaoImpl implements BookDao {
     
        public void save() {
            System.out.println(System.currentTimeMillis());
            System.out.println("book dao save ...");
        }
     
        public void update(){
            System.out.println("book dao update ...");
        }
    }
    ```
    
*   创建 Spring 的**配置类**
    
    ```
    @Configuration
    @ComponentScan("com.itheima")
    public class SpringConfig {
    }
    ```
    
*   编写 App 运行类
    
    ```
    public class App {
        public static void main(String[] args) {
            ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
            BookDao bookDao = ctx.getBean(BookDao.class);
            bookDao.save();
        }
    }
    ```
    

最终创建好的项目结构如下:

![](<assets/1740015616549.png>)

**说明:**

*   目前打印 save 方法的时候，因为方法中有打印系统时间，所以运行的时候是可以看到系统时间
*   对于 update 方法来说，就没有该功能
*   我们要使用 SpringAOP 的方式在不改变 update 方法的前提下让其具有打印系统时间的功能。

### 2.4 AOP 代码实现

**步骤 1: 添加 aspectj 依赖 aspectjweaver**

pom.xml

```
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```

*   导入 AspectJ 的 jar 包,**AspectJ** 是 **AOP** 思想的一个**具体实现**，Spring 有自己的 AOP 实现，但是相比于 AspectJ 来说比较麻烦，所以我们直接采用 **Spring 整合 ApsectJ** 的方式进行 AOP 开发。
*   因为`spring-context`中已经导入了`spring-aop`, 所以不需要再单独导入`spring-aop`
    
    ![](<assets/1740015619307.png>)
    

**步骤 2: 定义接口与实现类**

```
环境准备的时候，BookDaoImpl已经准备好，不需要做任何修改


```

**步骤 3:aop 包下定义通知类和通知**

通知就是将共性功能抽取出来后形成的方法，共性功能指的就是当前系统时间的打印。

```
public class MyAdvice {
    //通知，共性功能
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
}
```

类名和方法名没有要求，可以任意。

**步骤 4: 定义切入点 @Pointcut，说明该方法待增强**

BookDaoImpl 中有两个方法，分别是 save 和 update，我们要增强的是 update 方法，该如何定义呢?

通过 **@Pointcut** 指定切入点方法格式和位置

```
public class MyAdvice {
    //注解描述切入点的方法格式和位置，执行到update时候运行通知。切入点是要被增强的方法
    //切入点表达式的public和异常名可以省略，下面已省略
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    //一个私有空壳方法，切入点依托一个不具有实际意义（无参无返空方法）的方法进行。
    private void pt(){}
    //通知，共性功能
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
}
```

**说明:**

*   切入点定义依托一个不具有实际意义的方法进行，即无参数、无返回值、方法体无实际逻辑。
*   execution 及后面编写的内容，后面会有章节专门去学习。

**步骤 5: 制作切面 @Before 或其他通知**

@Before 是前置通知，通知在原始方法前通知。

还有其他四种通知，最常用的是 @Around，具体通知类型讲解在 4.2 通知类型中。

*   后置通知 @After
*   **环绕通知 (重点)@Around**
*   返回后通知 (了解)@AfterReturning
*   抛出异常后通知 (了解)@AfterThrowing

切面是用来描述通知和切入点之间的关系，如何进行关系的绑定?

```
public class MyAdvice {
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
    //制作切面，这里通知和切入点的关系是before，通知在切入点之前运行
    @Before("pt()")
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
}
```

绑定切入点与通知关系，并指定通知添加到原始连接点的具体执行**位置**

![](<assets/1740015620183.png>)

**说明:**@Before 翻译过来是之前，也就是说通知会在切入点方法执行之前执行，除此之前还有其他四种类型，后面会讲。

**步骤 6: 注解指定该类为 bean 和切面类 @Aspect**

```
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
 
    @Before("pt()")
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
}
```

**步骤 7: 配置类注解开启 AOP 功能 @EnableAspectJAutoProxy**

 enable 译为开启，使能够；EnableAspectJAutoProxy 译为开启 aspectj 自动代理。

```
@Configuration
@ComponentScan("com.itheima")
//注解开启AOP
@EnableAspectJAutoProxy
public class SpringConfig {
}
```

**步骤 8: 运行程序**

```
public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookDao bookDao = ctx.getBean(BookDao.class);
        bookDao.update();
    }
}
```

看到在执行 update 方法之前打印了系统时间戳，说明对原始方法进行了增强，AOP 编程成功。

![](<assets/1740015621986.png>)

### 2.5 知识点 @EnableAspectJAutoProxy,@Aspect,@Pointcut,@Before

**知识点 1：@EnableAspectJAutoProxy**

<table><thead><tr><th>名称</th><th>@EnableAspectJAutoProxy</th></tr></thead><tbody><tr><td>类型</td><td>配置类注解</td></tr><tr><td>位置</td><td>配置类定义上方</td></tr><tr><td>作用</td><td>开启注解格式 AOP 功能</td></tr></tbody></table>

**知识点 2：@Aspect**

<table><thead><tr><th>名称</th><th>@Aspect</th></tr></thead><tbody><tr><td>类型</td><td>类注解</td></tr><tr><td>位置</td><td>切面类定义上方</td></tr><tr><td>作用</td><td>设置当前类为 AOP 切面类</td></tr></tbody></table>

**知识点 3：@Pointcut**

<table><thead><tr><th>名称</th><th>@Pointcut</th></tr></thead><tbody><tr><td>类型</td><td>方法注解</td></tr><tr><td>位置</td><td>切入点方法定义上方</td></tr><tr><td>作用</td><td>设置切入点方法</td></tr><tr><td>属性</td><td>value（默认）：切入点表达式</td></tr></tbody></table>

**知识点 4：@Before**

<table><thead><tr><th>名称</th><th>@Before</th></tr></thead><tbody><tr><td>类型</td><td>方法注解</td></tr><tr><td>位置</td><td>通知方法定义上方</td></tr><tr><td>作用</td><td>设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法前运行</td></tr></tbody></table>

## 3，AOP 工作流程

这一节我们主要讲解两个知识点:**`AOP工作流程`和`AOP核心概念`**。其中核心概念是对前面核心概念的补充。

### 3.1 AOP 工作流程

#### 3.1.1 AOP 工作流程概述 

由于 AOP 是基于 Spring 容器管理的 bean 做的增强，所以整个工作过程需要**从 Spring 加载 bean 说起**:

**流程 1:Spring 容器启动**

容器启动，在创建 bean 对象前需要去加载 bean, **哪些类需要被 IOC 容器加载呢?**

*   需要被增强的类，如: BookServiceImpl
*   通知类，如: MyAdvice

**流程 2: 读取被切面绑定的切入点**

不读取没被切面绑定的切入点。 

![](<assets/1740015623851.png>)

*   上面这个例子中有两个切入点的配置，但是第一个`ptx()`并没有被使用，所以不会被读取。

**流程 3: 初始化 bean**

**判定 bean 对应的通知类中的方法是否匹配到任意切入点**

*   注意第 1 步在容器启动的时候，bean 对象还没有被创建成功。
    
*   所有要被实例化 bean 对象的类中的方法和切入点进行匹配
    
    ![](<assets/1740015625868.png>)
    
    *   **匹配失败**，IOC 容器创建并管理**原始对象（目标对象）**, 如`UserDao`
        *   匹配失败说明不需要增强，直接调用原始对象的方法即可。
        *   没有被增强（加入共性代码即通知）的对象就叫做**目标对象**
    *   **匹配成功**，IOC 容器创建原始对象（目标对象）的**代理对象**, 如:`BookDao`
        *   匹配成功说明需要对其进行增强
        *   因为要对目标对象进行功能增强，并且采用的技术是**动态代理**，所以会为其**创建一个代理对象，容器中管理的就是这个代理对象，不是目标对象。**
        *   **最终运行的是代理对象的方法**，在该方法中会对原始方法进行功能增强

**流程 4: 获取 bean 执行方法**

*   获取的 bean 是原始对象（匹配失败）时，调用方法并执行，完成操作
*   获取的 bean 是代理对象（匹配成功）时，根据代理对象的运行模式**运行原始方法与增强的内容**，完成操作

#### **3.1.2 验证容器中是否为代理对象**

**结论**:

*   如果目标对象中的方法**会被增强**，那么容器中将存入的是目标对象的**代理对象**
*   如果目标对象中的方法**不被增强**，那么容器中将存入的是目标**对象本身**。
*   要**通过字节码文件验证已有对象是否是代理对象**，不能直接输出对象，因为代理对象重写了原始对象的 toString 方法

**验证：**

**验证思路**

1. 要执行的方法，不被定义的切入点包含，即不要增强，打印当前类的 getClass() 方法

2. 要执行的方法，被定义的切入点包含，即要增强，打印出当前类的 getClass() 方法

3. 观察两次打印的结果

**步骤 1: 修改 App 类, 获取类的类型**

```
public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookDao bookDao = ctx.getBean(BookDao.class);
        System.out.println(bookDao);
        System.out.println(bookDao.getClass());
    }
}
```

**步骤 2: 修改 MyAdvice 类，不增强**

因为定义的切入点中，被修改成`update1`, 所以 BookDao 中的 update 方法在执行的时候，就不会被增强，

所以容器中的对象应该是目标对象本身。

```
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(void com.itheima.dao.BookDao.update1())")
    private void pt(){}
 
    @Before("pt()")
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
}
```

**步骤 3: 运行程序**

![](<assets/1740015627985.png>)

**步骤 4: 修改 MyAdvice 类，增强**

因为定义的切入点中，被修改成`update`, 所以 BookDao 中的 update 方法在执行的时候，就会被增强，

所以容器中的对象应该是目标对象的代理对象

```
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
 
    @Before("pt()")
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
}
```

**步骤 5: 运行程序**

![](<assets/1740015629066.png>)

至此对于刚才的结论，我们就得到了验证，这块大家需要注意的是:

不能直接打印对象，从上面两次结果中可以看出，**直接打印对象走的是对象的 toString 方法**，代理对象重写了目标对象的 toString 方法，不管是不是代理对象打印的结果都是一样的，原因是内部对 toString 方法进行了重写。**要通过字节码文件判断是否是代理对象。**

### 3.2 目标对象和代理对象总结

在上面介绍 AOP 的工作流程中，我们提到了两个核心概念，分别是:

*   **目标对象 (Target)：原始功能去掉共性功能对应的类产生的对象，这种对象是无法直接完成最终工作的**
*   **代理 (Proxy)：目标对象无法直接完成工作，需要对其进行功能回填，通过原始对象的代理对象实现**

上面这两个概念比较抽象，简单来说，

**目标对象就是还没有加入共性方法**的类 [如: BookServiceImpl 类] 对应的对象，也叫原始对象。

不能说它不能运行，只能说它在运行的过程中对于要增强的内容是缺失的。

**SpringAOP** 是在**不改变原有设计 (代码) 的前提下对其进行增强的**，它的**底层采用的是代理模式**实现的，所以要对原始对象进行增强，就需要**对原始对象创建代理对象**，在代理对象中的方法把**通知** [如: MyAdvice 中的 method 方法] 内容**加进去**，就实现了增强, 这就是我们所说的**代理 (Proxy)**。

**小结**

通过这一节中，我们需要掌握的内容有：

*   能说出 AOP 的工作流程
*   AOP 的核心概念
    *   目标对象、连接点、切入点
    *   通知类、通知
    *   切面
    *   代理
*   SpringAOP 的本质或者可以说底层实现是通过代理模式。

## 4，AOP 配置管理

### 4.1 AOP 切入点表达式

前面的案例中，有涉及到如下内容:

![](<assets/1740015629363.png>)

对于 AOP 中切入点表达式，我们总共会学习三个内容，分别是**`语法格式`、`通配符`和`书写技巧`。**

#### 4.1.1 语法格式

首先我们先要明确两个概念:

*   **切入点: 要进行增强的方法**
*   **切入点表达式:** 切入点的**描述方式**

对于切入点的描述，我们其实是有两中方式的，先来看下前面的例子

![](<assets/1740015629646.png>)

**描述方式一：**执行 com.itheima.dao 包下的 BookDao **接口**中的无参数 update 方法

```
execution(void com.itheima.dao.BookDao.update())


```

**描述方式二：不建议，紧耦合。**执行 com.itheima.dao.impl 包下的 BookDaoImpl **实现类**中的无参数 update 方法

```
execution(void com.itheima.dao.impl.BookDaoImpl.update())


```

因为调用接口方法的时候最终运行的还是其实现类的方法，所以上面两种描述方式都是可以的。

**切入点表达式标准格式**

*   动作关键字 (访问修饰符 返回值 包名. 类 / 接口名. 方法名 (参数) 异常名）

对于这个格式，我们不需要硬记，通过一个例子，理解它:

```
execution(public User com.itheima.service.UserService.findById(int))


```

*   execution：动作关键字，描述切入点的行为动作，例如 execution 表示执行到指定切入点
*   **public:** 访问修饰符, 还可以是 public，private 等，**可以省略**
*   User：返回值，写返回值类型
*   com.itheima.service：包名，多级包使用点连接
*   UserService: 类 / 接口名称
*   findById：方法名
*   int: 参数，直接写参数的类型，多个类型用逗号隔开
*   **异常名：**方法定义中抛出指定异常，**可以省略**

**切入点表达式**就是要找到需要增强的方法，所以它就是**对一个具体方法的描述**，但是方法的定义会有很多，所以如果每一个方法对应一个切入点表达式，想想这块就会觉得将来编写起来会比较麻烦，这就需要用到下面所学习的通配符。

#### 4.1.2 通配符

**使用通配符描述切入点**，主要的目的就是简化之前的配置，具体都有哪些通配符可以使用?

*   **`*   ：`单个独立的任意符号**，可以独立出现，也可以作为前缀或者后缀的匹配符出现
    
    ```
    execution（public * com.itheima.*.UserService.find*(*))    //这里参数通配符限定一个以上参数
    
    
    ```
    
    匹配 com.itheima 包下的任意包中的 UserService 类或接口中所有 find 开头的**带有一个参数**的方法。注意一般包名尽量不用通配符，影响效率。
    

通配返回值，是**可以匹配 void** 的。* 通配符通配**路径不能匹配空值**，通配**一半方法名**时**可以匹配空值，**通配参数**不能匹配无参**。

```
    @Pointcut("execution(* package1.dao.*Dao.*.select*())")
    private void pt(){}
```

*   `**..**` ：**0 个或多个连续的任意符号**，可以独立出现，常用于**简化包名与参数的书写**
    
    ```
    execution（public User com..UserService.findById(..))
    
    
    ```
    
    匹配 com 包下的任意包中的 UserService 类或接口中所有名称为 findById 的方法
    
*   `**+ **` ：慎用。专用于**匹配子类类型**
    
    ```
    execution(* *..*Service+.*(..))
    
    
    ```
    
    这个**使用率较低**，描述子类的，咱们做 JavaEE 开发，**继承机会就一次，使用都很慎重**，所以很少用它。*Service+，表示**所有以 Service 结尾的接口的子类**。
    

**最常用切入点表达式：**

可以通配所有返回类型（void，int 等）、所有业务层接口、所有 select 方法（select，selectXxx）、所有带参、无参类型。

```
    @Pointcut("execution(* package1.service.*Service.select*(..))")
    private void pt(){}
```

**案例使用切入点表达式:**

![](<assets/1740015629909.png>)

```
execution(void com.itheima.dao.BookDao.update())
匹配接口，能匹配到
execution(void com.itheima.dao.impl.BookDaoImpl.update())
匹配实现类，能匹配到
execution(* com.itheima.dao.impl.BookDaoImpl.update())
返回值任意，能匹配到
execution(* com.itheima.dao.impl.BookDaoImpl.update(*))
返回值任意，但是update方法必须要有一个参数，无法匹配，要想匹配需要在update接口和实现类添加参数
execution(void com.*.*.*.*.update())
返回值为void,com包下的任意包三层包下的任意类的update方法，匹配到的是实现类，能匹配
execution(void com.*.*.*.update())
返回值为void,com包下的任意两层包下的任意类的update方法，匹配到的是接口，能匹配
execution(void *..update())
返回值为void，方法名是update的任意包下的任意类，能匹配
execution(* *..*(..))
匹配项目中任意类的任意方法，能匹配，但是不建议使用这种方式，影响范围广
execution(* *..u*(..))
匹配项目中任意包任意类下只要以u开头的方法，update方法能满足，能匹配
execution(* *..*e(..))
匹配项目中任意包任意类下只要以e结尾的方法，update和save方法能满足，能匹配
execution(void com..*())
返回值为void，com包下的任意包任意类任意方法，能匹配，*代表的是方法
execution(* com.itheima.*.*Service.find*(..))
将项目中所有业务层方法的以find开头的方法匹配
execution(* com.itheima.*.*Service.save*(..))
将项目中所有业务层方法的以save开头的方法匹配
```

后面两种更符合我们平常切入点表达式的编写规则

#### 4.1.3 书写技巧

对于切入点表达式的编写其实是很灵活的，那么在编写的时候，有没有什么好的技巧让我们用用:

*   所有代码按照标准规范开发，否则以下技巧全部失效
*   描述切入点通**常描述接口**，**而不描述实现类**, 如果描述到实现类，就出现紧耦合了
*   访问控制修饰符针对接口开发均采用 public 描述（**可省略访问控制修饰符描述**）
*   **返回值**类型对于**增删改**类使用**精准类型**加速匹配，对于**查询类**使用 *** 通配**快速描述**返回值**
*   **包名**书写**尽量不使用.. 匹配**，**效率**过低，常用 * 做单个包描述匹配，或精准匹配
*   **接口名 / 类名**书写名称与模块相关的**采用 * 匹配**，例如 UserService 书写成 ***Service**，绑定业务层接口名
*   **方法名**书写以**动词**进行**精准匹配**，名词采用_匹配，例如 getById 书写成 getBy*_,selectAll 书写成 select*
*   参数规则较为复杂，根据业务方法灵活调整
*   通常**不使用异常**作为**匹配**规则

### 4.2 AOP 通知类型

前面的案例中，有涉及到如下内容:

![](<assets/1740015630240.png>)

它所代表的含义是将`通知`添加到`切入点`方法执行的**前面**。

算上前置通知注解外，共有 5 种通知类型:

*   前置通知 @Before
*   后置通知 @After
*   **环绕通知 (重点)@Around**
*   返回后通知 (了解)@AfterReturning
*   抛出异常后通知 (了解)@AfterThrowing

#### 4.2.1 类型介绍

我们先来回顾下 AOP 通知:

*   AOP 通知描述了抽取的共性功能，根据共性功能抽取的位置不同，最终运行代码时要将其加入到合理的位置

通知具体要添加到切入点的哪里?

共提供了 5 种通知类型:

*   前置通知 @Before
*   后置通知 @After
*   **环绕通知 (重点)@Around**
*   返回后通知 (了解)@AfterReturning
*   抛出异常后通知 (了解)@AfterThrowing

为了更好的理解这几种通知类型，我们来看一张图

![](<assets/1740015630460.png>)

(1) **前置通知**，追加功能到**方法执行前**, 类似于在代码 1 或者代码 2 添加内容

(2) **后置通知**, 追加功能到方法执行后, 不管方法执行的过程中有没有抛出异常都会执行，类似于在代码 5 添加内容

(3) **环绕通知**, 环绕通知功能比较强大，它可以追加功能到方法执行的前后，这也是比较常用的方式，它可以实现其他四种通知类型的功能。

(4) **返回后通知**, 追加功能到方法执行后，只有方法正常执行结束后才进行, 类似于在代码 3 添加内容，如果方法执行抛出异常，返回后通知将不会被添加

(5) **抛出异常后通知**, 追加功能到方法抛出异常后，只有方法执行出异常才进行, 类似于在代码 4 添加内容，只有方法抛出异常后才会被添加

#### 4.2.2 环境准备

*   创建一个 Maven 项目
    
*   pom.xml 添加 Spring 依赖
    
    ```
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.10.RELEASE</version>
        </dependency>
        <dependency>
          <groupId>org.aspectj</groupId>
          <artifactId>aspectjweaver</artifactId>
          <version>1.9.4</version>
        </dependency>
    </dependencies>
    ```
    
*   添加 BookDao 和 BookDaoImpl 类
    
    ```
    public interface BookDao {
        public void update();
        public int select();
    }
     
    @Repository
    public class BookDaoImpl implements BookDao {
        public void update(){
            System.out.println("book dao update ...");
        }
        public int select() {
            System.out.println("book dao select is running ...");
            return 100;
        }
    }
    ```
    
*   创建 Spring 的配置类
    
    ```
    @Configuration
    @ComponentScan("com.itheima")
    @EnableAspectJAutoProxy
    public class SpringConfig {
    }
    ```
    
*   创建通知类
    
    ```
    @Component
    @Aspect
    public class MyAdvice {
        @Pointcut("execution(void com.itheima.dao.BookDao.update())")
        private void pt(){}
     
        public void before() {
            System.out.println("before advice ...");
        }
     
        public void after() {
            System.out.println("after advice ...");
        }
     
        public void around(){
            System.out.println("around before advice ...");
            System.out.println("around after advice ...");
        }
     
        public void afterReturning() {
            System.out.println("afterReturning advice ...");
        }
     
        public void afterThrowing() {
            System.out.println("afterThrowing advice ...");
        }
    }
    ```
    
*   编写 App 运行类
    
    ```
    public class App {
        public static void main(String[] args) {
            ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
            BookDao bookDao = ctx.getBean(BookDao.class);
            bookDao.update();
        }
    }
    ```
    

最终创建好的项目结构如下:

![](<assets/1740015630665.png>)

#### 4.2.3 **前置通知、后置通知**

**前置通知**

修改 MyAdvice, 在 before 方法上添加`@Before注解`

```
@Component
@Aspect
public class MyAdvice {
 
    //指定切入点
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
 
    //制作切面，即绑定切入点与通知间的关系，Before说明是在切入点方法运行之前通知。
    @Before("pt()")
    //此处也可以写成 @Before("MyAdvice.pt()"),不建议
    public void before() {
        System.out.println("before advice ...");
    }
}
```

![](<assets/1740015630995.png>)

**后置通知**

```
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
 
    @Before("pt()")
    public void before() {
        System.out.println("before advice ...");
    }
    @After("pt()")
    public void after() {
        System.out.println("after advice ...");
    }
}
```

![](<assets/1740015631208.png>)

#### **4.2.4 环绕通知（重要，可以做所有通知类型做的事）**

**1. 无返回值通知（适配无返回值的切入点）：** 

环绕通知必须传参 ProceedingJoinPoint ，通过其 proceed 方法，调用原始方法：

```
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
 
 
    @Around("pt()")
    //必须传参ProceedingJoinPoint ，调用该对象的proceed方法运行原始方法，起到环绕的效果。否则仅运行通知，不运行原始方法
    public void around(ProceedingJoinPoint pjp) throws Throwable{
        System.out.println("around before advice ...");
        //proceed方法对原始操作调用
        pjp.proceed();
        System.out.println("around after advice ...");
 
 
        //getSignature方法获取执行签名信息
        Signature signature = pjp.getSignature();
        //通过签名获取执行操作名称(接口名)
        String className = signature.getDeclaringTypeName();
        //通过签名获取执行操作名称(方法名)
        String methodName = signature.getName();
        System.out.println("当前接口名和方法名"+className +methodName );
    }
}
```

 **ProceedingJoinPoint** 译为：程序连接点。Proceeding 译为程序，过程，诉讼。

**proceed() 为什么要抛出异常?**

因为源码里强制要抛出异常，让原始方法的异常在原始方法里处理好。

![](<assets/1740015631482.png>)

运行，可以看到原始方法已经被执行了

![](<assets/1740015631728.png>)

**测试：不特意调用原始方法，原始方法不执行**

```
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
 
    @Around("pt()")
    public void around(){
        System.out.println("around before advice ...");
        System.out.println("around after advice ...");
    }
}
```

![](<assets/1740015632034.png>)

运行结果中，通知的内容打印出来，但是**原始方法的内容却没有被执行**。

因为**环绕通知**需要在**原始方法的前后**进行增强，所以环绕通知就必须要能对原始操作进行调用。 

**2. 有返回值通知（常用，适配有返回值的切入点）：**

原始方法有返回值，要根据原始方法的返回值来设置环绕通知的返回值：

```
@Component
@Aspect
public class MyAdvice {
 
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
 
    @Pointcut("execution(int com.itheima.dao.BookDao.select())")
    //*：通配符返回值，是可以匹配void的。*通配符通配路径不能匹配空值，通配一半方法名时可以匹配空值
    //..：可以通配0个或多个参数
    //@Pointcut("execution(* com.itheima.dao.*Dao.select*(..))")
    private void pt2(){}
 
    @Around("pt2()")
    //因为切入点有返回值，所以通知必须有返回值，值为proceed方法的返回值，类型Object
    public Object aroundSelect(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("around before advice ...");
        //表示对原始操作的调用
        Object ret = pjp.proceed();
        System.out.println("around after advice ...");
        return ret;
    }
}
```

**说明:**

*   ​ **返回值类型是 Object 而不是 int**，主要原因是 Object 类型更通用。
*   ​ 在环绕通知中是可以对原始方法返回值进行修改的。

**不加返回值报错演示：**

*   修改 MyAdvice, 对 BookDao 中的 select 方法添加环绕通知，

```
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
 
    @Pointcut("execution(int com.itheima.dao.BookDao.select())")
    private void pt2(){}
 
    @Around("pt2()")
    public void aroundSelect(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("around before advice ...");
        //表示对原始操作的调用
        pjp.proceed();
        System.out.println("around after advice ...");
    }
}
```

*   修改 App 类，调用 select 方法

```
public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookDao bookDao = ctx.getBean(BookDao.class);
        int num = bookDao.select();
        System.out.println(num);
    }
}
```

运行后会报错，错误内容为:

Exception in thread "main" org.springframework.aop.AopInvocationException: **Null return value from advice does not match primitive return type for: public abstract int com.itheima.dao.BookDao.select()** at org.springframework.aop.framework.JdkDynamicAopProxy.invoke(JdkDynamicAopProxy.java:226) at com.sun.proxy.$Proxy19.select(Unknown Source) at com.itheima.App.main(App.java:12)

错误大概的意思是:`空的返回不匹配原始方法的int返回`

*   void 就是返回 Null
*   原始方法就是 BookDao 下的 select 方法

**环绕通知注意事项**

1.  环绕通知**必须依赖形参 ProceedingJoinPoint** 才能实现**对原始方法的调用**，进而实现原始方法调用前后同时添加通知
2.  通知中如果**未使用 ProceedingJoinPoint** 对原始方法进行调用将**跳过原始方法**的执行
3.  对原始方法的调用可以不接收返回值，通知方法设置成 void 即可，如果接收返回值，最好设定为 Object 类型
4.  **原始方法的返回值**如果是 **void** 类型，**通知方法的返回值**类型可以设置成 void, **也可以**设置成 **Object**
5.  由于无法预知原始方法运行后是否会抛出异常，因此**环绕通知方法必须要处理 Throwable 异常**

#### 4.2.5 返回后通知、异常后通知（了解）

**返回后通知（了解）**

```
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
 
    @Pointcut("execution(int com.itheima.dao.BookDao.select())")
    private void pt2(){}
 
    @AfterReturning("pt2()")
    public void afterReturning() {
        System.out.println("afterReturning advice ...");
    }
}
```

![](<assets/1740015632340.png>)

**注意：返回后通知**是需要在**原始方法`select`正常执行后才会被执**行，如果`select()`方法执行的过程中出现了异常，那么返回后通知是不会被执行。后置通知是不管原始方法有没有抛出异常都会被执行。这个案例大家下去可以自己练习验证下。

**异常后通知**

```
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
 
    @Pointcut("execution(int com.itheima.dao.BookDao.select())")
    private void pt2(){}
 
    @AfterThrowing("pt2()")
    public void afterThrowing() {
        System.out.println("afterThrowing advice ...");
    }
}
```

![](<assets/1740015632603.png>)

**注意：**异常后通知是需要原始方法抛出异常，可以在`select()`方法中添加一行代码`int i = 1/0`即可。如果没有抛异常，异常后通知将不会被执行。

学习完这 5 种通知类型，我们来思考下环绕通知是如何实现其他通知类型的功能的?

因为环绕通知是可以控制原始方法执行的，所以我们把增强的代码写在调用原始方法的不同位置就可以实现不同的通知类型的功能，如:

![](<assets/1740015632830.png>)

#### 知识点 @After,@Around@AfterReturning,@AfterThrowing

知识点 1：@After

<table><thead><tr><th>名称</th><th>@After</th></tr></thead><tbody><tr><td>类型</td><td>方法注解</td></tr><tr><td>位置</td><td>通知方法定义上方</td></tr><tr><td>作用</td><td>设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法后运行</td></tr></tbody></table>

知识点 2：@AfterReturning

<table><thead><tr><th>名称</th><th>@AfterReturning</th></tr></thead><tbody><tr><td>类型</td><td>方法注解</td></tr><tr><td>位置</td><td>通知方法定义上方</td></tr><tr><td>作用</td><td>设置当前通知方法与切入点之间绑定关系，当前通知方法在原始切入点方法正常执行完毕后执行</td></tr></tbody></table>

知识点 3：@AfterThrowing

<table><thead><tr><th>名称</th><th>@AfterThrowing</th></tr></thead><tbody><tr><td>类型</td><td>方法注解</td></tr><tr><td>位置</td><td>通知方法定义上方</td></tr><tr><td>作用</td><td>设置当前通知方法与切入点之间绑定关系，当前通知方法在原始切入点方法运行抛出异常后执行</td></tr></tbody></table>

知识点 4：@Around

<table><thead><tr><th>名称</th><th>@Around</th></tr></thead><tbody><tr><td>类型</td><td>方法注解</td></tr><tr><td>位置</td><td>通知方法定义上方</td></tr><tr><td>作用</td><td>设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法前后运行</td></tr></tbody></table>

### 4.3 案例，业务层接口执行效率

#### 4.3.1 需求分析

*   **需求:** 任意业务层接口执行均可显示其执行效率（执行时长）

**目的** 

这个案例的目的是查看每个业务层执行的时间，这样就可以监控出哪个业务比较耗时，将其查找出来方便优化。

**具体实现的思路:**

(1) 开始执行方法之前记录一个时间

(2) 执行方法

(3) 执行完方法之后记录一个时间

(4) 用后一个时间减去前一个时间的差值，就是我们需要的结果。

所以要在方法执行的**前后添加业务**，经过分析我们将采用**`环绕通知`**。

**说明:** 原始方法如果只执行一次，时间太快，两个时间差可能为 0，所以我们要执行万次来计算时间差。

#### 4.3.2 环境准备

*   pom.xml 添加 Spring 依赖 spring 相关 spring-context，spring-jdbc，测试相关 spring-test、junit，aspectj 依赖 aspectjweaver、数据库相关 mysql、druid、mybatis、mybatis-spring
    
    ```
    <dependencies>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>5.2.10.RELEASE</version>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-jdbc</artifactId>
          <version>5.2.10.RELEASE</version>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-test</artifactId>
          <version>5.2.10.RELEASE</version>
        </dependency>
        <dependency>
          <groupId>org.aspectj</groupId>
          <artifactId>aspectjweaver</artifactId>
          <version>1.9.4</version>
        </dependency>
        <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>5.1.47</version>
        </dependency>
        <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>druid</artifactId>
          <version>1.1.16</version>
        </dependency>
        <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis</artifactId>
          <version>3.5.6</version>
        </dependency>
        <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis-spring</artifactId>
          <version>1.3.0</version>
        </dependency>
        <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.12</version>
          <scope>test</scope>
        </dependency>
      </dependencies>
    ```
    
*   添加 AccountService、AccountServiceImpl、AccountDao 与 Account 类
    
    ```
    public interface AccountService {
        void save(Account account);
        void delete(Integer id);
        void update(Account account);
        List<Account> findAll();
        Account findById(Integer id);
    }
     
    @Service
    public class AccountServiceImpl implements AccountService {
     
        @Autowired
        private AccountDao accountDao;
     
        public void save(Account account) {
            accountDao.save(account);
        }
     
        public void update(Account account){
            accountDao.update(account);
        }
     
        public void delete(Integer id) {
            accountDao.delete(id);
        }
     
        public Account findById(Integer id) {
            return accountDao.findById(id);
        }
     
        public List<Account> findAll() {
            return accountDao.findAll();
        }
    }
    public interface AccountDao {
     
        @Insert("insert into tbl_account(name,money)values(#{name},#{money})")
        void save(Account account);
     
        @Delete("delete from tbl_account where id = #{id} ")
        void delete(Integer id);
     
        @Update("update tbl_account set name = #{name} , money = #{money} where id = #{id} ")
        void update(Account account);
     
        @Select("select * from tbl_account")
        List<Account> findAll();
     
        @Select("select * from tbl_account where id = #{id} ")
        Account findById(Integer id);
    }
     
    public class Account implements Serializable {
     
        private Integer id;
        private String name;
        private Double money;
        //setter..getter..toString方法省略
    }
    ```
    
*   resources 下提供一个 jdbc.properties
    
    ```
    jdbc.driver=com.mysql.jdbc.Driver
    jdbc.url=jdbc:mysql://localhost:3306/spring_db?useSSL=false
    jdbc.username=root
    jdbc.password=root
    ```
    
*   创建相关配置类
    
    ```
    //Spring配置类:SpringConfig
    @Configuration
    @ComponentScan("com.itheima")
    @PropertySource("classpath:jdbc.properties")
    @Import({JdbcConfig.class,MybatisConfig.class})
    public class SpringConfig {
    }
    //JdbcConfig配置类
    public class JdbcConfig {
        @Value("${jdbc.driver}")
        private String driver;
        @Value("${jdbc.url}")
        private String url;
        @Value("${jdbc.username}")
        private String userName;
        @Value("${jdbc.password}")
        private String password;
     
        @Bean
        public DataSource dataSource(){
            DruidDataSource ds = new DruidDataSource();
            ds.setDriverClassName(driver);
            ds.setUrl(url);
            ds.setUsername(userName);
            ds.setPassword(password);
            return ds;
        }
    }
    //MybatisConfig配置类
    public class MybatisConfig {
     
        @Bean
        public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource){
            SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();
            ssfb.setTypeAliasesPackage("com.itheima.domain");
            ssfb.setDataSource(dataSource);
            return ssfb;
        }
     
        @Bean
        public MapperScannerConfigurer mapperScannerConfigurer(){
            MapperScannerConfigurer msc = new MapperScannerConfigurer();
            msc.setBasePackage("com.itheima.dao");
            return msc;
        }
    }
    ```
    
*   编写 Spring 整合 Junit 的测试类
    
    ```
    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration(classes = SpringConfig.class)
    public class AccountServiceTestCase {
        //要调用service方法，所以自动注入service
     
        @Autowired
        private AccountService accountService;
     
        @Test
        public void testFindById(){
            Account ac = accountService.findById(2);
        }
     
        @Test
        public void testFindAll(){
            List<Account> all = accountService.findAll();
        }
     
    }
    ```
    

最终创建好的项目结构如下:

![](<assets/1740015633083.png>)

#### 4.3.3 功能开发

**步骤 1: 开启 SpringAOP 的注解功能**

在 Spring 的主配置文件 SpringConfig 类中添加使用 aop 注解 EnableAspectJAutoProxy

```
@EnableAspectJAutoProxy


```

**步骤 2: 创建 AOP 的通知类**

*   该类要被 Spring 管理，需要添加 **@Component**
    
*   要标识该类是一个 AOP 的切面类，需要添加 **@Aspect**
    
*   配置切入点表达式，需要添加一个方法，并添加 **@Pointcut**

```
@Component
@Aspect
public class ProjectAdvice {
    //配置业务层的所有方法
    @Pointcut("execution(* com.itheima.service.*Service.*(..))")
    private void servicePt(){}
 
    public void runSpeed(){
 
    } 
}
```

**步骤 3: 添加环绕通知**

在 runSpeed() 方法上添加 **@Around**

```
@Component
@Aspect
public class ProjectAdvice {
    //配置业务层的所有方法
    @Pointcut("execution(* com.itheima.service.*Service.*(..))")
    private void servicePt(){}
    //@Around("ProjectAdvice.servicePt()") 可以简写为下面的方式
    @Around("servicePt()")
    public Object runSpeed(ProceedingJoinPoint pjp){
        Object ret = pjp.proceed();
        return ret;
    } 
}
```

**注意:** 目前并没有做任何增强

**步骤 4: 完成核心业务，记录万次执行的时间**

```
@Component
@Aspect
public class ProjectAdvice {
    //配置业务层的所有方法
    @Pointcut("execution(* com.itheima.service.*Service.*(..))")
    private void servicePt(){}
    //@Around("ProjectAdvice.servicePt()") 可以简写为下面的方式
    @Around("servicePt()")
    public void runSpeed(ProceedingJoinPoint pjp){
 
        long start = System.currentTimeMillis();
        for (int i = 0; i < 10000; i++) {
           pjp.proceed();
        }
        long end = System.currentTimeMillis();
        System.out.println("业务层接口万次执行时间: "+(end-start)+"ms");
    } 
}
```

**步骤 5: 运行单元测试类**

![](<assets/1740015633272.png>)

**注意:** 因为程序每次执行的时长是不一样的，所以运行多次最终的结果是不一样的。

**步骤 6: 程序优化，通过签名获取接口方法名，从而区分不同业务层执行时间**

**面临问题：**

多个方法一起执行测试的时候，控制台都打印的是:

```
业务层接口万次执行时间:xxxms

```

我们没有办法区分到底是哪个接口的哪个方法执行的具体时间。

**优化方法：**

```
@Component
@Aspect
public class ProjectAdvice {
    //配置业务层的所有方法
    @Pointcut("execution(* com.itheima.service.*Service.*(..))")
    private void servicePt(){}
    //@Around("ProjectAdvice.servicePt()") 可以简写为下面的方式
    @Around("servicePt()")
    public void runSpeed(ProceedingJoinPoint pjp){
        //获取执行签名信息
        Signature signature = pjp.getSignature();
        //通过签名获取执行操作名称(接口名)
        String className = signature.getDeclaringTypeName();
        //通过签名获取执行操作名称(方法名)
        String methodName = signature.getName();
 
        long start = System.currentTimeMillis();
        for (int i = 0; i < 10000; i++) {
           pjp.proceed();
        }
        long end = System.currentTimeMillis();
        System.out.println("万次执行："+ className+"."+methodName+"---->" +(end-start) + "ms");
    } 
}
```

**步骤 7: 运行单元测试类**

![](<assets/1740015633477.png>)

**补充说明**

当前测试的接口执行效率仅仅是一个**理论值**，并不是一次完整的执行过程，例如其它层加载速度之类的误差。

这块只是通过该案例把 AOP 的使用进行了学习，具体的实际值是有很多因素共同决定的。

### 4.4 AOP 通知获取数据

#### 4.0 概述 

目前我们写 AOP 仅仅是在原始方法前后追加一些操作，接下来我们要说说 **AOP 中数据相关的内容**，我们将从**`获取参数`、`获取返回值`**和**`获取异常`**三个方面来研究**切入点的相关信息**。

**思考：**前面我们介绍通知类型的时候总共讲了五种，那么对于这五种类型都会有参数，返回值和异常吗?

**答案：**

*   **获取**切入点方法的**参数：所有**的通知类型都可以获取切入点参数
    *   JoinPoint：适用于前置、后置、返回后、抛出异常后通知
    *   **ProceedingJoinPoint：**适用于环绕通知
*   **获取**切入点方法**返回值：**
    *   **能：返回后通知**和**环绕通知，可以获取**切入点方法返回值
    *   不能：前置和抛出异常后通知是没有返回值，后置通知可有可无，所以不做研究
*   **获取**切入点方法运行**异常信息：**
    *   **能：抛出异常后通知**和**环绕通知**，**可以获取**切入点方法异常信息
    *   不能：前置和返回后通知是不会有，后置通知可有可无，所以不做研究

#### 4.4.1 环境准备

*   创建一个 Maven 项目
    
*   pom.xml 添加 Spring 和 aspectj 依赖
    
    ```
    <dependencies>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>5.2.10.RELEASE</version>
        </dependency>
        <dependency>
          <groupId>org.aspectj</groupId>
          <artifactId>aspectjweaver</artifactId>
          <version>1.9.4</version>
        </dependency>
      </dependencies>
    ```
    
*   添加 BookDao 和 BookDaoImpl 类
    
    ```
    public interface BookDao {
        public String findName(int id);
    }
    @Repository
    public class BookDaoImpl implements BookDao {
     
        public String findName(int id) {
            System.out.println("id:"+id);
            return "itcast";
        }
    }
    ```
    
*   创建 Spring 的配置类
    
    ```
    @Configuration
    @ComponentScan("com.itheima")
    @EnableAspectJAutoProxy
    public class SpringConfig {
    }
    ```
    
*   编写通知类
    
    ```
    @Component
    @Aspect
    public class MyAdvice {
        @Pointcut("execution(* com.itheima.dao.BookDao.findName(..))")
        private void pt() {
        }
     
        @Before("pt()")
        public void before() {
            System.out.println("before advice ...");
        }
     
        @After("pt()")
        public void after() {
            System.out.println("after advice ...");
        }
     
        @Around("pt()")
        public Object around() throws Throwable {
            Object ret = pjp.proceed();
            return ret;
        }
     
        @AfterReturning("pt()")
        public void afterReturning() {
            System.out.println("afterReturning advice ...");
        }
     
        @AfterThrowing("pt()")
        public void afterThrowing() {
            System.out.println("afterThrowing advice ...");
        }
    }
    ```
    

编写 App 运行类

```
 
  public class App {
      public static void main(String[] args) {
          ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
          BookDao bookDao = ctx.getBean(BookDao.class);
          String name = bookDao.findName(100);
          System.out.println(name);
      }
  }
```

最终创建好的项目结构如下:

![](<assets/1740015633782.png>)

#### 4.4.2 获取参数

**非环绕通知获取方式，JoinPoint 的 getArgs 方法**

JoinPoint 是 ProceedingJoinPoint 的父类。

**①获取单个参数** 

通知方法参数设为 JoinPoint 对象, 通过 JoinPoint 对象的 getArgs 方法来获取参数

```
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(* com.itheima.dao.BookDao.findName(..))")
    private void pt(){}
 
    @Before("pt()")
    public void before(JoinPoint jp) 
        Object[] args = jp.getArgs();
        System.out.println(Arrays.toString(args));
        System.out.println("before advice ..." );
    }
    //...其他的略
}
```

运行 App 类，可以获取如下内容，说明参数 100 已经被获取

![](<assets/1740015634007.png>)

**思考: 方法的参数只有一个，为什么获取的是一个数组?**

因为**参数的个数是不固定的**，所以使用数组更通配些。

**②获取两个参数**

(1) 修改 BookDao 接口和 BookDaoImpl 实现类

```
public interface BookDao {
    public String findName(int id,String password);
}
@Repository
public class BookDaoImpl implements BookDao {
 
    public String findName(int id,String password) {
        System.out.println("id:"+id);
        return "itcast";
    }
}
```

(2) 修改 App 类，调用方法传入多个参数

```
public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookDao bookDao = ctx.getBean(BookDao.class);
        String name = bookDao.findName(100,"itheima");
        System.out.println(name);
    }
}
```

(3) 运行 App，查看结果, 说明两个参数都已经被获取到

![](<assets/1740015634235.png>)

**说明:**

使用 JoinPoint 的方式获取参数适用于`前置`、`后置`、`返回后`、`抛出异常后`通知。

**环绕通知获取方式，ProceedingJoinPoint 类的`getArgs方法`**

环绕通知使用的是程序连接点 ProceedingJoinPoint，前面将环绕通知时已经用过他的 proceed 方法，调用原始方法。

因为 ProceedingJoinPoint **是 JoinPoint 类的子类**，获取参数使用的也是**`getArgs()`方法：**

```
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(* com.itheima.dao.BookDao.findName(..))")
    private void pt(){}
 
    @Around("pt()")
    public Object around(ProceedingJoinPoint pjp)throws Throwable {
        Object[] args = pjp.getArgs();
        System.out.println(Arrays.toString(args));
        Object ret = pjp.proceed();
        //也可以用带参的proceed方法，可以修改参数
        //args[0] = 666;
        //Object ret = pjp.proceed(args);
        return ret;
    }
    //其他的略
}
```

运行 App 后查看运行结果，说明 ProceedingJoinPoint 也是可以通过 getArgs() 获取参数

![](<assets/1740015634430.png>)

**注意:**

*   **pjp.proceed()** 方法是有**两个构造方法**，分别是:
    
    ![](<assets/1740015634769.png>)
    
    *   调用**无参数的 proceed 仅能获取参数无法修改参数**，当原始方法有参数，会在调用的过程中自动传入参数
        
    *   所以调用这两个方法的任意一个都可以完成功能
        
    *   **带参数的 proceed 能获取并修改参数**, 如下:
        
        ```
        @Component
        @Aspect
        public class MyAdvice {
            @Pointcut("execution(* com.itheima.dao.BookDao.findName(..))")
            private void pt(){}
         
            @Around("pt()")
            public Object around(ProceedingJoinPoint pjp) throws Throwable{
                Object[] args = pjp.getArgs();
                System.out.println(Arrays.toString(args));
                args[0] = 666;
                Object ret = pjp.proceed(args);
                return ret;
            }
            //其他的略
        }
        ```
        
        有了这个特性后，我们就可以在环绕通知中对原始方法的参数进行拦截过滤，避免由于参数的问题导致程序无法正确运行，保证代码的健壮性。
        

#### 4.4.3 获取返回值

对于返回值，**只有返回后`AfterReturing`和环绕`Around`**这两个通知类型**可以获取**，具体如何获取?

**@Around 环绕通知获取返回值**

ProceedingJoinPoint 对象 proceed 方法的返回值 ret，就是原始方法的返回值

```
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(* com.itheima.dao.BookDao.findName(..))")
    private void pt(){}
 
    @Around("pt()")
    public Object around(ProceedingJoinPoint pjp) throws Throwable{
        Object[] args = pjp.getArgs();
        System.out.println(Arrays.toString(args));
        args[0] = 666;
        Object ret = pjp.proceed(args);
 
        //proceed方法的返回值ret，就是原始方法的返回值
        System.out.println(ret);
        return ret;
    }
    //其他的略
}
```

上述代码中，**`ret`就是方法的返回值**，我们是**可以直接获取**，不但可以获取，如果需要还可以进行**修改**。

**@AfterReturning 返回后通知获取返回值**

```
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(* com.itheima.dao.BookDao.findName(..))")
    private void pt(){}
 
    //注解里的returning值要设成通知的形参
    @AfterReturning(value = "pt()",returning = "ret")
    public void afterReturning(Object ret) {
        System.out.println("afterReturning advice ..."+ret);
    }
    //其他的略
}
```

 AfterReturning 跟进后的属性：默认的 value 设置切入点，**returning 设置返回值的参数名**

![](<assets/1740015634986.png>)

 运行 App 后查看运行结果，说明返回值已经被获取到

![](<assets/1740015635236.png>)

**注意:**

**(1) 参数名的问题**

![](<assets/1740015635515.png>)

**(2)**afterReturning 方法形参**类型建议为 Object**

参数类型可以写成 String，但是为了能匹配更多的参数类型，**建议写成 Object 类型**

**(3）如果**参数有 **JoinPoint** 获取原始方法参数，**必须放第一个**

![](<assets/1740015635819.png>)

#### 4.4.4 获取异常

对于获取抛出的异常，**只有抛出异常后`AfterThrowing`和环绕`Around`这两个通知类型可以获取**。

**@Around 环绕通知获取异常**

这块比较简单，以前我们是抛出异常，现在只需要将异常捕获，就可以获取到原始方法的异常信息了

```
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(* com.itheima.dao.BookDao.findName(..))")
    private void pt(){}
 
    @Around("pt()")
    public Object around(ProceedingJoinPoint pjp){
        Object[] args = pjp.getArgs();
        System.out.println(Arrays.toString(args));
        args[0] = 666;
        Object ret = null;
        try{
            ret = pjp.proceed(args);
        }catch(Throwable throwable){
            t.printStackTrace();
        }
        return ret;
    }
    //其他的略
}
```

在 catch 方法中就可以获取到异常，至于获取到异常以后该如何处理，这个就和你的业务需求有关了。

**抛出异常后通知获取异常**

```
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(* com.itheima.dao.BookDao.findName(..))")
    private void pt(){}
 
    @AfterThrowing(value = "pt()",throwing = "t")
    public void afterThrowing(Throwable t) {
        System.out.println("afterThrowing advice ..."+t);
    }
    //其他的略
}
```

如何**让原始方法抛出异常**，方式有很多，

```
@Repository
public class BookDaoImpl implements BookDao {
 
    public String findName(int id,String password) {
        System.out.println("id:"+id);
        if(true){
            throw new NullPointerException();
        }
        return "itcast";
    }
}
```

**注意:**

![](<assets/1740015636047.png>)

运行 App 后，查看控制台，就能看的异常信息被打印到控制台

![](<assets/1740015636270.png>)

至此，AOP 通知如何获取数据就已经讲解完了，数据中包含`参数`、`返回值`、`异常(了解)`。

### 4.5 案例，百度网盘密码数据兼容处理

#### 4.5.1 需求分析

**需求:** 对百度网盘分享链接输入密码时尾部**多输入的空格做兼容处理**。

![](<assets/1740015636517.png>)

**问题描述:**

*   点击链接，会提示，请输入提取码，如下图所示
    
    ![](<assets/1740015636788.png>)
    
*   当我们从别人发给我们的内容中复制提取码的时候，有时候会多复制到一些空格，直接粘贴到百度的提取码输入框，从而导致提取码输入错误
    

**通过 aop 解决：** 

①：在**业务方法执行之前**对所有的输入参数进行格式处理——**trim()**

②：使用处理后的参数调用原始方法——环绕通知中存在对原始方法的调用

**思考：**

*   以后涉及到需要去除前后空格的业务可能会有很多，这个去空格的代码是每个业务都写么?

可以考虑使用 **AOP 来统一处理**。

*   AOP 有五种**通知类型**，该使用哪种呢?

我们的需求是将原始方法的参数处理后在参与原始方法的调用，能做这件事的就只有环绕通知。

#### 4.5.2 环境准备

*   创建一个 Maven 项目
    
*   pom.xml 添加 Spring 和 aspectj 依赖
    
    ```
    <dependencies>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>5.2.10.RELEASE</version>
        </dependency>
        <dependency>
          <groupId>org.aspectj</groupId>
          <artifactId>aspectjweaver</artifactId>
          <version>1.9.4</version>
        </dependency>
      </dependencies>
    ```
    
*   添加 ResourcesService，ResourcesServiceImpl,ResourcesDao 和 ResourcesDaoImpl 类
    
    ```
    public interface ResourcesDao {
        boolean readResources(String url, String password);
    }
    @Repository
    public class ResourcesDaoImpl implements ResourcesDao {
        public boolean readResources(String url, String password) {
            System.out.println(password.length());
            //模拟校验
            return password.equals("root");
        }
    }
    public interface ResourcesService {
        public boolean openURL(String url ,String password);
    }
    @Service
    public class ResourcesServiceImpl implements ResourcesService {
        @Autowired
        private ResourcesDao resourcesDao;
     
        public boolean openURL(String url, String password) {
            return resourcesDao.readResources(url,password);
        }
    }
    ```
    
*   创建 Spring 的配置类
    
    ```
    @Configuration
    @ComponentScan("com.itheima")
    public class SpringConfig {
    }
    ```
    
*   编写 App 运行类
    
    ```
    public class App {
        public static void main(String[] args) {
            ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
            ResourcesService resourcesService = ctx.getBean(ResourcesService.class);
            boolean flag = resourcesService.openURL("http://pan.baidu.com/haha", "root");
            System.out.println(flag);
        }
    }
    ```
    

最终创建好的项目结构如下:

![](<assets/1740015637048.png>)

现在项目的效果是，当输入密码为 "root" 控制台打印为 true, 如果密码改为 "root" 控制台打印的是 false

需求是使用 AOP 将参数进行统一处理，不管输入的密码`root`前后包含多少个空格，最终控制台打印的都是 true。

#### 4.5.3 具体实现

**步骤 1: 开启 SpringAOP 的注解功能 @EnableAspectJAutoProxy**

```
@Configuration
@ComponentScan("com.itheima")
@EnableAspectJAutoProxy
public class SpringConfig {
}
```

**步骤 2: 编写通知类，****添加环绕通知，****完成核心业务，处理参数中的空格**

```
@Component
@Aspect
public class DataAdvice {
    @Pointcut("execution(boolean com.itheima.service.*Service.*(*,*))")
    private void servicePt(){}
 
    @Around("DataAdvice.servicePt()")
    // @Around("servicePt()")这两种写法都对
    public Object trimStr(ProceedingJoinPoint pjp) throws Throwable {
        //获取原始方法的参数
        Object[] args = pjp.getArgs();
        for (int i = 0; i < args.length; i++) {
            //判断参数是不是字符串
            if(args[i].getClass().equals(String.class)){
                args[i] = args[i].toString().trim();
            }
        }
        //将修改后的参数传入到原始方法的执行中
        Object ret = pjp.proceed(args);
        return ret;
    }
 
}
```

**步骤 3: 运行程序**

根据最终打印的长度来看看，字符串的空格有没有被去除掉。

**注意：**

![](<assets/1740015637287.png>)

## 5，AOP 小结

AOP 的知识就已经讲解完了，接下来对于 AOP 的知识进行一个总结:

**5.1 AOP 的核心概念**

*   概念：AOP(Aspect Oriented Programming) 面向切面编程，一种编程范式
*   作用：在不惊动原始设计的基础上为方法进行功能**增强**
*   核心概念
    *   代理（Proxy）：SpringAOP 的核心本质是采用代理模式实现的
    *   连接点（JoinPoint）：在 SpringAOP 中，理解为任意方法的执行
    *   切入点（Pointcut）：匹配连接点的式子，也是具有共性功能的方法描述
    *   通知（Advice）：若干个方法的共性功能，在切入点处执行，最终体现为一个方法
    *   切面（Aspect）：描述通知与切入点的对应关系
    *   目标对象（Target）：被代理的原始对象成为目标对象

**5.2 切入点表达式**

*   切入点表达式标准格式：动作关键字 (访问修饰符 返回值 包名. 类 / 接口名. 方法名（参数）异常名)
    
    ```
    execution(* com.itheima.service.*Service.*(..))
    
    
    ```
    
*   切入点表达式描述通配符：
    
    *   作用：用于快速描述，范围描述
    *   `*`：匹配任意符号（常用）
    *   `..` ：匹配多个连续的任意符号（常用）
    *   `+`：匹配子类类型
*   切入点表达式书写技巧
    
    1. 按**标准规范**开发 2. 查询操作的返回值建议使用 * 匹配 3. 减少使用.. 的形式描述包 4. **对接口进行描述**，使用 * 表示模块名，例如 UserService 的匹配描述为 _Service 5. 方法名书写保留动词，例如 get，使用 \_ 表示名词，例如 getById 匹配描述为 getBy* 6. 参数根据实际情况灵活调整
    

**5.3 五种通知类型**

*   前置通知
*   后置通知
*   环绕通知（重点）
    *   环绕通知依赖形参 ProceedingJoinPoint 才能实现对原始方法的调用
    *   环绕通知可以隔离原始方法的调用执行
    *   环绕通知返回值设置为 Object 类型
    *   环绕通知中可以对原始方法调用过程中出现的异常进行处理
*   返回后通知
*   抛出异常后通知

**5.4 通知中获取参数**

*   获取切入点方法的参数，所有的通知类型都可以获取参数
    *   **JoinPoint：**适用于前置、后置、返回后、抛出异常后通知，**设置为第一个参数**
    *   **ProceedingJoinPoint：**适用于**环绕通知**
*   获取切入点方法返回值，前置和抛出异常后通知是没有返回值，后置通知可有可无，所以不做研究
    *   返回后通知
    *   环绕通知
*   获取切入点方法运行异常信息，前置和返回后通知是不会有，后置通知可有可无，所以不做研究
    *   抛出异常后通知
    *   环绕通知

## 6，AOP 事务管理

### 6.1 Spring 事务简介

*   **事务作用：**在**数据层**保障一系列的数据库操作**同成功同失败**
*   **Spring 事务作用：**在**数据层或业务层**保障一系列的数据库操作**同成功同失败**

**两种事务编程模型** 

Spring 支持编程式事务（注入 TransactionTemplate 对象，execute 方法里编写事务）和声明式事务（方法或类注解 @Transactional）。

**@Transactional 原理** 

原理是 AOP，生成代理对象，在业务方法执行前后自动开启和提交 / 回滚事务。

@Transactional 的属性可以设置事务的超时时间、隔离级别、传播机制、回滚异常、不会滚异常。

**事务隔离级别**

多个事务同时操作同一个数据库时，一个事务读取到其他事务已经提交但未持久化到数据库中的数据所带来的影响。 

1.  读取未提交内容：隔离级别最低，事务可以读取到其他未提交事务的数据；
2.  读取已提交内容：保证其他事务提交后才能读取数据。解决脏读。
3.  可重复读：保证在一个事务内多次查询相同数据时，结果是一致的。解决脏读、不可重复读
4.  序列化：隔离级别最高，在整个事务过程中将所有操作串行化执行。解决脏读、不可重复读和幻读，但性能差
5.  与数据库默认设置保持一致（默认）：数据库的默认隔离级别

脏读：因为延迟等原因，第一个事务在更新数据库前，第二个事务已经更新了数据库，导致最终数据库内容是第一个事务改的情况。

不可重复读：同一次事务前后查询结果不一致；

幻读：同义词事务前后数据量发生变化，像幻觉一样。

**事务的传播行为：**多个事务方法互相调用时，事务如何在这些方法之间传播。

默认传播行为：如果当前没有事务，则开启一个新的事务；如果已存在事务，就加入到该事务中。

**非事务方法调用事务方法**

非事务方法调用事务方法必须用代理对象调用，也就是注入自己再调用事务方法。

**思考：业务层事务处理的必要性**

数据层有事务我们可以理解，**为什么业务层也需要处理事务呢?**

举个简单的例子，

*   转账业务会有两次数据层的调用，一次是加钱一次是减钱
*   假如只在数据层处理事务，加钱和减钱就有两个事务，没办法保证加钱和减钱同时成功或者同时失败
*   这个时候就需要将事务放在业务层进行处理。

**平台事务管理器`PlatformTransactionManager接口`**

Spring 为了管理事务，提供了一个平台事务管理器`PlatformTransactionManager`

![](<assets/1740015637531.png>)

commit 是用来提交事务，rollback 是用来回滚事务。

**事务管理器 DataSourceTransactionManager 实现类** 

PlatformTransactionManager 只是一个接口，DataSourceTransactionManager 是实现类。

从名称上可以看出，我们只需要**给它一个 DataSource 对象**，它就**可以**帮你去**在业务层管理事务**。其**内部采用的是 JDBC 的事务**。所以说如果你**持久层采用的是 JDBC 相关的技术**，就可以采用这个事务管理器来管理你的事务。而 **Mybatis 内部采用的就是 JDBC 的事务**，所以后期我们 Spring 整合 Mybatis 就采用的这个 DataSourceTransactionManager 事务管理器。

![](<assets/1740015637831.png>)

### 6.2 入门案例，转账

#### 6.2.1 需求分析

接下来通过一个案例来学习下 Spring 是如何来管理事务的。

**需求:**

A 账户减钱，B 账户加钱

**步骤：** 

①：**数据层**提供基础操作，指定账户减钱（outMoney），指定账户加钱（inMoney）

②：**业务层**提供转账操作（transfer），调用减钱与加钱的操作

③：提供 2 个账号和操作金额执行转账操作

④：基于 **Spring 整合 MyBatis** 环境搭建上述操作

#### 6.2.2 环境搭建

**步骤 1: 准备数据库表**

之前我们在整合 Mybatis 的时候已经创建了这个表, 可以直接使用

```
create database spring_db character set utf8;
use spring_db;
create table tbl_account(
    id int primary key auto_increment,
    name varchar(35),
    money double
);
insert into tbl_account values(1,'Tom',1000);
insert into tbl_account values(2,'Jerry',1000);
```

**步骤 2: 创建项目导入 jar 包**

项目的 pom.xml 添加相关依赖 spring 相关 spring-context,spring-jdbc,spring-test，测试 junit，数据库相关 mysql-connector-java,druid,mybatis,mybatis-spring

```
<dependencies>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.10.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.16</version>
    </dependency>
 
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.6</version>
    </dependency>
 
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.47</version>
    </dependency>
 
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.2.10.RELEASE</version>
    </dependency>
 
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.0</version>
    </dependency>
 
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
 
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>5.2.10.RELEASE</version>
    </dependency>
 
  </dependencies>
```

**步骤 3: 根据表创建模型类（实体类）**

```
public class Account implements Serializable {
 
    private Integer id;
    private String name;
    private Double money;
    //setter...getter...toString...方法略    
}
```

**步骤 4: 创建 Dao 接口**

```
public interface AccountDao {
 
    @Update("update tbl_account set money = money + #{money} where name = #{name}")
    void inMoney(@Param("name") String name, @Param("money") Double money);
 
    @Update("update tbl_account set money = money - #{money} where name = #{name}")
    void outMoney(@Param("name") String name, @Param("money") Double money);
}
```

**步骤 5: 创建 Service 接口和实现类**

```
public interface AccountService {
    /**
     * 转账操作
     * @param out 传出方
     * @param in 转入方
     * @param money 金额
     */
    public void transfer(String out,String in ,Double money) ;
}
 
@Service
public class AccountServiceImpl implements AccountService {
 
    @Autowired
    private AccountDao accountDao;
 
    public void transfer(String out,String in ,Double money) {
        accountDao.outMoney(out,money);
        accountDao.inMoney(in,money);
    }
 
}
```

**步骤 6: 添加 jdbc.properties 文件**

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/spring_db?useSSL=false
jdbc.username=root
jdbc.password=root
```

**步骤 7: 创建 JdbcConfig 配置类**

```
public class JdbcConfig {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String userName;
    @Value("${jdbc.password}")
    private String password;
 
    @Bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(userName);
        ds.setPassword(password);
        return ds;
    }
}
```

**步骤 8: 创建 MybatisConfig 配置类**

```
public class MybatisConfig {
 
    @Bean
    public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource){
        SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();
        ssfb.setTypeAliasesPackage("com.itheima.domain");
        ssfb.setDataSource(dataSource);
        return ssfb;
    }
 
    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
        MapperScannerConfigurer msc = new MapperScannerConfigurer();
        msc.setBasePackage("com.itheima.dao");
        return msc;
    }
}
```

**步骤 9: 创建 SpringConfig 配置类**

```
@Configuration
@ComponentScan("com.itheima")
@PropertySource("classpath:jdbc.properties")
@Import({JdbcConfig.class,MybatisConfig.class})
public class SpringConfig {
}
```

**步骤 10: 编写测试类**

```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class AccountServiceTest {
 
    @Autowired
    private AccountService accountService;
 
    @Test
    public void testTransfer() throws IOException {
        accountService.transfer("Tom","Jerry",100D);
    }
 
}
```

最终创建好的项目结构如下:

![](<assets/1740015638160.png>)

#### 6.2.3 事务管理 - 代码实现

上述环境，运行单元测试类，会执行转账操作，`Tom`的账户会减少 100，`Jerry`的账户会加 100。

**不使用业务层事务处理，异常问题模拟** 

这是正常情况下的运行结果，但是如果在转账的过程中出现了异常，如:

```
@Service
public class AccountServiceImpl implements AccountService {
 
    @Autowired
    private AccountDao accountDao;
 
    public void transfer(String out,String in ,Double money) {
        accountDao.outMoney(out,money);
        int i = 1/0;
        accountDao.inMoney(in,money);
    }
 
}
```

这个时候就模拟了转账过程中出现异常的情况，正确的操作应该是转账出问题了，`Tom`应该还是 900，`Jerry`应该还是 1100，但是真正运行后会发现，并没有像我们想象的那样，`Tom`账户为 800 而`Jerry`还是 1100,**100 块钱凭空消息了**，银行乐疯了。如果把转账换个顺序，银行就该哭了。

**分析问题原因:**

①：程序正常执行时，账户金额 A 减 B 加，没有问题

②：程序出现异常后，**转账失败，但是异常之前操作成功**，异常之后操作失败，整体业务失败

当程序出问题后，我们**需要让事务进行回滚**，而且这个事务应该是加在业务层上，而 Spring 的事务管理就是用来解决这类问题的。

**业务层事务管理的具体步骤:**

**步骤 1: 在需要被事务管理的方法上添加注解 @Transactional**

```
public interface AccountService {
    /**
     * 转账操作
     * @param out 传出方
     * @param in 转入方
     * @param money 金额
     */
    //配置当前接口方法具有事务
    public void transfer(String out,String in ,Double money) ;
}
 
@Service
public class AccountServiceImpl implements AccountService {
 
    @Autowired
    private AccountDao accountDao;
    @Transactional
    public void transfer(String out,String in ,Double money) {
        accountDao.outMoney(out,money);
        int i = 1/0;
        accountDao.inMoney(in,money);
    }
 
}
```

**注意:**

**@Transactional 可以写在接口类上、接口方法上、实现类上和实现类方法上**

*   写在**接口类**上，该接口的所有实现类的所有方法都会有事务
*   写在**接口方法**上，该接口的所有实现类的该方法都会有事务
*   写在实现类上，该类中的所有方法都会有事务
*   写在实现类方法上，该方法上有事务
*   **建议写在接口或接口方法上**，降低耦合

**步骤 2: 在 JdbcConfig 类中配置事务管理器方法，PlatformTransactionManager** 

```
public class JdbcConfig {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String userName;
    @Value("${jdbc.password}")
    private String password;
 
    @Bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(userName);
        ds.setPassword(password);
        return ds;
    }
 
    //配置事务管理器，mybatis使用的是jdbc事务
    @Bean
    public PlatformTransactionManager transactionManager(DataSource dataSource){
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(dataSource);
        return transactionManager;
    }
}
```

**注意：**事务管理器要根据使用技术进行选择，Mybatis 框架使用的是 JDBC 事务，可以直接使用`DataSourceTransactionManager`

**步骤 3：开启事务注解**

在 SpringConfig 的配置类中开启**事务注解 @EnableTransactionManagement**

```
@Configuration
@ComponentScan("com.itheima")
@PropertySource("classpath:jdbc.properties")
@Import({JdbcConfig.class,MybatisConfig.class})
//开启注解式事务驱动
@EnableTransactionManagement
public class SpringConfig {
}
```

**步骤 4: 运行测试类**

会发现在转换的业务出现错误后，事务就可以控制回顾，保证数据的正确性。

#### 知识点 @EnableTransactionManagement，@Transactional

知识点 1：@EnableTransactionManagement

<table><thead><tr><th>名称</th><th>@EnableTransactionManagement</th></tr></thead><tbody><tr><td>类型</td><td>配置类注解</td></tr><tr><td>位置</td><td>配置类定义上方</td></tr><tr><td>作用</td><td>设置当前 Spring 环境中开启注解式事务支持</td></tr></tbody></table>

知识点 2：@Transactional

<table><thead><tr><th>名称</th><th>@Transactional</th></tr></thead><tbody><tr><td>类型</td><td>接口注解 类注解 方法注解</td></tr><tr><td>位置</td><td>业务层接口上方 业务层实现类上方 业务方法上方</td></tr><tr><td>作用</td><td>为当前业务层方法添加事务（如果设置在类或接口上方则类或接口中所有方法均添加事务）</td></tr></tbody></table>

### 6.3 Spring 事务管理员和事务协调员

*   **未开启 Spring 事务之前工作的流程:**

![](<assets/1740015638495.png>)

*   AccountDao 的 outMoney 因为是**修改操作**，会**开启一个事务** T1
*   AccountDao 的 inMoney 因为是**修改操作**，会**开启一个事务** T2
*   AccountService 的 transfer 没有事务，
    
    *   运行过程中如果没有抛出异常，则 T1 和 T2 都正常提交，数据正确
    *   如果在两个方法**中间抛出异常**，T1 因为执行成功提交事务，T2 因为**抛异常**不会被执行
    *   就会导致数据出现错误
*   **开启 Spring 的事务管理后的工作流程**
    

![](<assets/1740015638765.png>)

*   transfer 上添加了 **@Transactional** 注解，在该**方法上**就会有一个**事务** T
*   AccountDao 的 outMoney 方法的**事务 T1 加入到** transfer 的**事务 T** 中
*   AccountDao 的 inMoney 方法的**事务 T2 加入到 t**ransfer 的**事务 T** 中
*   这样就保证他们在**同一个事务**中，当业务层中出现异常，整个事务就会回滚，保证数据的准确性。

通过上面例子的分析，我们就可以得到如下概念:

*   **事务管理员：**案例中的业务层事务。**发起事务方**，在 Spring 中通常指代**业务层开启事务**的方法
*   **事务协调员：**案例中的数据层事务。**加入事务方**，在 Spring 中通常指代**数据层方法**，**也可以**是**业务层方法**

**注意:**

目前的事务管理是基于**`DataSourceTransactionManager`**和**`SqlSessionFactoryBean`**使用的是同一个数据源。

### 6.4 Spring 事务属性

在这一节中，我们主要学习三部分内容**`事务配置`、`转账业务追加日志`、`事务传播行为`**。

#### 6.4.1 所有事务配置

![](<assets/1740015638949.png>)

上面**这些属性**都可以在**`@Transactional`注解的参数上进行设置**。

*   **readOnly：**设置只读事务。true 只读事务，false 读写事务，增删改要设为 false, 查询设为 true。
    
*   **timeout: 设置事务超时时间。**单位秒，在多长时间之内事务没有提交成功就自动回滚，-1 表示不设置超时时间。
    
*   **rollbackFor:** 当出现**指定异常**进行事务回滚
    
*   **noRollbackFor:** 当出现**指定异常**不进行事务回滚
    

**思考:**

出现异常事务会自动回滚，这个是我们之前就已经知道的。**noRollbackFor** 是设定对于指定的异常不回滚，这个好理解。

**问题：rollbackFor** 是指定回滚异常，**对于异常事务不应该都回滚么，为什么还要指定?**

**答案：**Spring 的事务只会对**`Error异常`和`RuntimeException异常`及其子类**进行事务**回滚**，其他的异常类型是不会回滚的。

**异常体系回顾：**

![](<assets/1740015639220.png>)

比如 **IOException 异常不会回滚：**

```
public interface AccountService {
    /**
     * 转账操作
     * @param out 传出方
     * @param in 转入方
     * @param money 金额
     */
    //配置当前接口方法具有事务
    public void transfer(String out,String in ,Double money) throws IOException;
}
 
@Service
public class AccountServiceImpl implements AccountService {
 
    @Autowired
    private AccountDao accountDao;
    @Transactional
    public void transfer(String out,String in ,Double money) throws IOException{
        accountDao.outMoney(out,money);
        //int i = 1/0; //这个异常事务会回滚
        if(true){
            throw new IOException(); //这个异常事务就不会回滚
        }
        accountDao.inMoney(in,money);
    }
 
}
```

出现这个问题的原因是，对应 IOException 不符合上述条件所以不回滚

此时就可以使用 **rollbackFor** 属性来**设置出现 IOException 异常不回滚**

```
@Service
public class AccountServiceImpl implements AccountService {
 
    @Autowired
    private AccountDao accountDao;
    //设置IOException回滚异常
     @Transactional(rollbackFor = {IOException.class})
    public void transfer(String out,String in ,Double money) throws IOException{
        accountDao.outMoney(out,money);
        //int i = 1/0; //这个异常事务会回滚
        if(true){
            throw new IOException(); //这个异常事务就不会回滚
        }
        accountDao.inMoney(in,money);
    }
 
}
```

*   **rollbackForClassName** 等同于 rollbackFor, 只不过属性为异常的**类全名字符串**
    
*   **noRollbackForClassName** 等同于 noRollbackFor，只不过属性为异常的**类全名字符串**
    
*   **isolation 设置事务的隔离级别**
    
    *   DEFAULT : 默认隔离级别, 会采用数据库的隔离级别
    *   READ_UNCOMMITTED : 读未提交
    *   READ_COMMITTED : 读已提交
    *   REPEATABLE_READ : 重复读取
    *   SERIALIZABLE: 串行化

ctrl+b 跟进 Transaction 注解，Ctrl+f12 查看属性：

![](<assets/1740015639489.png>)

介绍完上述属性后，还有最后一个事务的传播行为，为了讲解该属性的设置，我们需要完成下面的案例。

#### 6.4.2 事务配置 - 事务传播行为的需求案例，转账业务追加日志

**需求分析**

在前面的转案例的基础上，**完成转账后记录日志**。

*   **需求：**实现任意两个账户间转账操作，并对每次转账操作在数据库进行留痕
*   **需求微缩：**A 账户减钱，B 账户加钱，数据库记录日志

**实现思路:**

①：基于转账操作案例添加日志模块，实现数据库中记录日志

②：业务层转账操作（transfer），调用减钱、加钱与记录日志功能

需要注意一点就是，我们这个案例的预期效果为:

**无论转账操作是否成功，均进行转账操作的日志留痕**

**环境准备**

该环境是基于转账环境来完成的，所以环境的准备可以参考`6.1.3的环境搭建步骤`，在其基础上，我们继续往下写

**代码实现** 

**步骤 1: 创建日志表**

```
create table tbl_log(
   id int primary key auto_increment,
   info varchar(255),
   createDate datetime
)
```

**步骤 2: 添加 LogDao 接口**

```
public interface LogDao {
    @Insert("insert into tbl_log (info,createDate) values(#{info},now())")
    void log(String info);
    //传单个参数不用@Param起别名，没歧义，会自动识别
    //void log(@Param("info") String info);
}
```

**步骤 3: 添加 LogService 接口与实现类**

```
public interface LogService {
    void log(String out, String in, Double money);
}
@Service
public class LogServiceImpl implements LogService {
 
    @Autowired
    private LogDao logDao;
    @Transactional
    public void log(String out,String in,Double money ) {
        logDao.log("转账操作由"+out+"到"+in+",金额："+money);
    }
}
```

**步骤 4: 在转账的业务中添加记录日志**

```
public interface AccountService {
    /**
     * 转账操作
     * @param out 传出方
     * @param in 转入方
     * @param money 金额
     */
    //配置当前接口方法具有事务
    public void transfer(String out,String in ,Double money)throws IOException ;
}
@Service
public class AccountServiceImpl implements AccountService {
 
    @Autowired
    private AccountDao accountDao;
    @Autowired
    private LogService logService;
    @Transactional
    public void transfer(String out,String in ,Double money) {
        try{
            accountDao.outMoney(out,money);
            accountDao.inMoney(in,money);
            //将是否异常都要提交的日志代码，放在finally里
        }finally {
            logService.log(out,in,money);
        }
    }
 
}
```

**步骤 5: 运行程序**

程序正常运行，tbl_account 表中转账成功，tbl_log 表中日志记录成功

**步骤 6：设置异常使事务回滚，运行程序**

当转账业务之间出现异常 **(int i =1/0),** 转账失败，tbl_account 成功回滚，但是 **tbl_log 表未添加数据**

**为什么回滚后日志没加上？**

*   **失败原因:** 日志的记录与转账操作隶属同一个事务，同成功同失败
*   **解决办法:** 无论转账操作是否成功，**日志必须保留**。这就需要**设置事务传播行为 propagation**

#### 6.4.3 事务配置 - 事务传播行为，propagation 属性

![](<assets/1740015639662.png>)

**对于上述案例的分析**:

*   log 方法、inMoney 方法和 outMoney 方法都属于增删改，分别有事务 T1,T2,T3
*   transfer 因为加了 @Transactional 注解，也开启了事务 T
*   前面我们讲过 Spring 事务会把 T1,T2,T3 都加入到事务 T 中
*   所以当转账失败后，所有的事务都回滚，导致日志没有记录下来

问题：如何**让 log 方法单独是一个事务**呢?

**答案：事务传播行为**。

**事务传播行为**`**propagation**`**：**

**事务协调员**对事务管理员所携带事务的**处理态度**。例如默认情况，管理员新建事务 T，协调员加入 T 事务。

`**propagation**`译为传播。

**具体解决转账日志案例**

修改 **logService** 改变事务的传播行为`**propagation**为**Propagation.REQUIRES_NEW**。`

`REQUIRES_NEW**译为需要新建**，也就是给日志业务方法另外新建事务。`

```
@Service
public class LogServiceImpl implements LogService {
 
    @Autowired
    private LogDao logDao;
    //propagation设置事务属性：传播行为设置为当前操作需要新事务
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void log(String out,String in,Double money ) {
        logDao.log("转账操作由"+out+"到"+in+",金额："+money);
    }
}
```

运行后，就能实现我们想要的结果，不管转账是否成功，都会记录日志。

**2. 事务传播行为的可选值**

![](<assets/1740015639901.png>)

对于我们开发实际中使用的话，因为默认值需要事务是常态的。根据开发过程选择其他的就可以了，例如案例中需要新事务就需要手工配置。其实入账和出账操作上也有事务，采用的就是默认值。

#### 6.4.4 **非事务方法调用事务方法必须用代理对象调用**

```
 
@Service
public class AccountServiceImpl implements AccountService {
 
    @Autowired
    private AccountDao accountDao;
    @Autowired
    private AccountService currentProxy;
    @Transactional
    public void transfer(String out,String in ,Double money) {
        accountDao.outMoney(out,money);
        int i = 1/0;
        accountDao.inMoney(in,money);
    }
 
    public void fun() {
        //非事务方法调用事务方法必须用代理对象调用。
        currentProxy.transfer(xx,xx,xx);
    } 
}
```