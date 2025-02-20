---
url: https://blog.csdn.net/qq_40991313/article/details/126283925
title: Spring 基础 1——Spring（配置开发版）,IOC 和 DI_java spring 需要 java ee 吗 - CSDN 博客
date: 2025-02-20 09:40:02
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1，Spring 背景](#t0)

[1.1 Spring 地位](#t1)

[1.2 Spring 优势和学习内容](#t2)

[2，Spring 相关概念](#t3)

[2.1 初识 Spring](#t4)

[2.1.1 Spring 家族](#t5)

[2.1.2 Spring 发展史](#t6)

[2.2 Spring 系统架构](#t7)

[2.2.1 系统架构图](#t8)

[2.2.2 学习内容](#t9)

[2.3 Spring 核心概念](#t10)

[2.3.1 不使用 Spring 项目中的问题](#t11)

[2.3.2 IOC、IOC 容器、Bean、DI](#t12)

[2.3.3 核心概念小结](#t13)

[3，入门案例（配置开发版）](#t14)

[3.1 IOC 入门案例](#t15)

[3.1.1 入门案例思路分析](#t16)

[3.1.2 入门案例代码实现](#t17)

[3.2 DI 入门案例，通过配置除掉 service 里的 new](#t18)

[3.2.1 入门案例思路分析](#t19)

[3.2.2 入门案例代码实现](#t20)

[4，IOC 相关内容](#t21)

[4.1 bean 基础配置](#t22)

[4.1.0 概述](#t23)

[4.1.1 bean 基础配置 (id 与 class)](#t24)

[4.1.2 bean 的 name 属性](#t25)

[4.1.3 bean 作用范围 scope 配置，单例和非单例](#t26)

[4.2 bean 的三种创建方式](#t27)

[4.2.1 环境准备](#t28)

[4.2.2 bean 的创建方法: 1：无参构造方法实例化（常用）](#t29)

[4.2.3 Spring 的报错信息从下往上看](#t30)

[4.2.4 bean 的创建方式 2：静态工厂实例化（了解）](#t31)

[4.2.5 bean 的创建方式 3：实例工厂（了解）与 FactoryBean（常用）](#t32)

[4.2.6 bean 实例化小结](#t33)

[4.3 bean 的生命周期](#t34)

[4.3.1 环境准备](#t35)

[4.3.2 生命周期控制](#t36)

[4.3.3 关闭 IOC 容器方法：close 关闭容器、注册钩子关闭容器](#t37)

[4.3.4 InitializingBean， DisposableBean 控制生命周期](#t38)

[4.3.5 bean 生命周期小结](#t39)

[5，DI 相关内容](#t40)

[5.1 setter 注入](#t41)

[5.1.1 环境准备](#t42)

[5.1.2 注入多个引用数据类型（property 标签的 name 和 ref）](#t43)

[5.1.3 注入简单数据类型（property 标签的 name 和 value）](#t44)

[5.2 构造器注入](#t45)

[5.2.1 环境准备](#t46)

[5.2.2 构造器注入引用数据类型（constructor-arg 标签的 name 和 ref）](#t47)

[5.2.3 构造器注入多个引用数据类型](#t48)

[5.2.4 构造器注入多个简单数据类型（constructor-arg 标签的 name 和 value）](#t49)

[5.2.5 依赖注入方式选择](#t50)

[5.3 自动配置依赖注入](#t51)

[5.3.1 什么是依赖自动装配?](#t52)

[5.3.2 自动装配方式有哪些?](#t53)

[5.3.3 准备下案例环境](#t54)

[5.3.4 完成自动装配的配置](#t55)

[5.4 数组、集合注入](#t56)

[5.4.1 环境准备](#t57)

[5.4.2 注入数组类型数据](#t58)

[5.4.3 注入 List 类型数据](#t59)

[5.4.4 注入 Set 类型数据](#t60)

[5.4.5 注入 Map 类型数据](#t61)

[5.4.6 注入 Properties 类型数据](#t62)

## 1，Spring 背景

### 1.1 Spring 地位

*   从使用和占有率看
    
    *   Spring 在市场的占有率与使用率高，未使用 Spring 的项目一般都是些比较老的项目，大多都处于维护阶段。
        
    *   Spring 在企业的技术选型命中率高，Spring 是微服务架构基础，是传统开发主流技术。所以说, Spring 技术是 JavaEE 开发必备技能
        

![](<assets/1740015602092.png>)

### 1.2 Spring 优势和学习内容

Spring 框架主要的优势是在**`简化开发`**和**`框架整合`**上：

*   **简化开发:** Spring 两大核心技术 **IOC、****AOP。**
    
*   **事务处理**属于 Spring 中 AOP 的具体应用，可以简化项目中的事务管理，也是 Spring 技术中的一大亮点。

*   **框架整合:** Spring 可以整合市面上几乎所有主流框架，比如:
    
    *   **MyBatis**
    *   MyBatis-plus
    *   Struts
    *   Struts2
    *   Hibernate
    *   ……

**学习内容:**

*   **Spring 核心容器的 IOC/DI**
*   **Spring 的 AOP**
*   **AOP 的具体应用, 事务管理**
*   **IOC/DI 的具体应用, 整合 Mybatis**

## 2，Spring 相关概念

### 2.1 初识 Spring

#### 2.1.1 Spring 家族

*   官网：https://spring.io
    
    *   **Spring 能做什么:** 用以开发 web、微服务以及分布式系统等。光这三块就已经占了 JavaEE 开发的**九成多**。
    *   Spring 并不是单一的一个技术，而是一个**大家族**，可以从官网的`Projects`中查看其包含的所有技术。
*   Spring 发展到今天已经形成了一种开发的生态圈, Spring 提供了若干个项目, 每个项目用于完成特定的功能。
    
    *   **Spring 已形成了完整的生态圈**。我们可以使用 Spring 技术完成整个项目的构建、设计与开发。
        
    *   Spring **有若干个项目**。可以根据需要自行选择，把这些个项目组合起来，起了一个名称叫**全家桶**，如下图所示
        
        ![](<assets/1740015604514.png>)
        
        **说明:**
        
        图中的图标都代表什么含义，可以进入`https://spring.io/projects`网站进行对比查看。
        
        这些技术并不是所有的都需要学习，额外需要重点关注`Spring Framework`、`SpringBoot`和`SpringCloud`:
        
        除了上面的这三个技术外，还有很多其他的技术，也比较流行，如 SpringData,SpringSecurity 等，这些都可以被应用在我们的项目中。
        
        *   **Spring Framework:**Spring 框架，是 Spring 中最早最核心的技术，也是所有其他技术的基础。
        *   **SpringBoot:**Spring 是来简化开发，而 SpringBoot 是来帮助 Spring 在简化的基础上能**更快速进行开发**。
        *   **SpringCloud:** 这个是用来做**分布式之微服务架构的相关开发**。

我们今天所学习的 Spring 其实指的是 **Spring Framework**。

#### 2.1.2 Spring 发展史

接下来我们介绍下 Spring Framework 这个技术是如何来的呢?

![](<assets/1740015605937.png>)

Spring 发展史

*   IBM(IT 公司 - 国际商业机器公司) 在 1997 年提出了 EJB 思想, 早期的 JAVAEE 开发大都基于该思想。
*   Rod Johnson(Java 和 J2EE 开发领域的专家) 在 2002 年出版的`Expert One-on-One J2EE Design and Development`, 书中有阐述在开发中使用 EJB 该如何做。
*   Rod Johnson 在 2004 年出版的`Expert One-on-One J2EE Development without EJB`, 书中提出了比 EJB 思想更高效的实现方案，并且在同年将方案进行了具体的落地实现，这个实现就是 Spring1.0。
*   随着时间推移，版本不断更新维护，目前最新的是 Spring5
    *   **Spring1.0** 是纯配置文件开发
    *   **Spring2.0** 为了简化开发引入了注解开发，此时是配置文件加注解的开发方式
    *   **Spring3.0** 已经可以进行**纯注解开发**，使开发效率大幅提升，我们的课程会以注解开发为主
    *   **Spring4.0** 根据 JDK 的版本升级对个别 API 进行了调整
    *   **Spring5.0** 已经全面支持 JDK8，现在 Spring 最新的是 5 系列所以建议大家**把 JDK 安装成 1.8 版**

本节介绍了 Spring 家族与 Spring 的发展史，需要大家重点掌握的是:

*   今天所学的 Spring 其实是 Spring 家族中的 Spring Framework
*   Spring Framework 是 Spring 家族中其他框架的底层基础，**学好 Spring 可以为其他 Spring 框架的学习打好基础**

### 2.2 Spring 系统架构

spring 框架主要指的是 Spring Framework。

#### 2.2.1 系统架构图

*   Spring Framework 是 Spring 生态圈中最基础的项目，是其他项目的根基。Spring Framework 的发展也经历了很多版本的变更，每个版本都有相应的调整。
    
    ![](<assets/1740015607953.png>)
    
*   Spring Framework 的 5 版本目前没有最新的架构图，而最新的是 4 版本，所以接下来主要研究的是 **Spring4 的架构图**
    
    ![](<assets/1740015610239.png>)
    
     系统架构讲究上层依赖下层，所以越下层越核心，最下层的是核心容器。
    
    **(1) 核心层**
    
    *   Core Container: 核心容器，这个模块是 Spring 最核心的模块，其他的都需要依赖该模块
    
    **(2)AOP 层**
    
    *   **AOP**（Aspect Oriented Programming，aspect 译为切面，方面；oriented 译为面向，以... 为方向；）: 面向切面编程。它依赖核心层容器，目的是**在不改变原有代码的前提下对其进行功能增强**
    *   **Aspects**：AOP 是思想, Aspects 是对 AOP 思想的具体实现
    
    **(3) 数据层**
    
    *   Data Access: 数据访问，Spring 全家桶中有对数据访问的具体实现技术
    *   Data Integration: 数据集成，Spring 支持整合其他的数据层解决方案，比如 Mybatis。integration 译为集成，结合，融合。
    *   Transactions: 事务，Spring 中事务管理是 Spring AOP 的一个具体实现，也是后期学习的重点内容。Transactions 译为事务，业务，交易。
    
    **(4)Web 层**
    
    *   这一层的内容将在 SpringMVC 框架具体学习
    
    **(5)Test 层**
    
    *   Spring 主要整合了 Junit 来完成单元测试和集成测试

#### 2.2.2 学习内容

主要包含四部分内容:

*   **Spring 核心容器的 IOC/DI**
*   **Spring 的 AOP**
*   **AOP 的具体应用, 事务管理**
*   **IOC/DI 的具体应用, 整合 Mybatis**

![](<assets/1740015612747.png>)

### 2.3 Spring 核心概念

在 Spring 核心概念这部分内容中主要包含`IOC/DI`、`IOC容器`和`Bean`, 那么问题就来了，这些都是什么呢?

#### 2.3.1 不使用 Spring 项目中的问题

**回顾：** 

在上一节整合 [Javaweb](https://so.csdn.net/so/search?q=Javaweb&spm=1001.2101.3001.7020) 开发商品管理系统写业务层的时候，方案是写 BrandService 接口，然后 BrandServiceImp1 实现 BrandService 接口，在 Servlet 创建 service 对象时候是

```
BrandService service=new BrandServiceImp1;

```

[JavaWeb 基础 10——VUE&Element & 整合 Javaweb 的商品管理系统_vincewm 的博客 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/126186764?spm=1001.2014.3001.5501 "JavaWeb基础10——VUE&Element&整合Javaweb的商品管理系统_vincewm的博客-CSDN博客")

当时优化时说使用接口实现类方法比直接业务类更好，能降低耦合度。在更新业务层实现方法时候，Servlet 里就只需要修改 new 后的实现类名，甚至在使用 Spring 后可以不改 Servlet 代码。

**问题: 耦合度偏高**

![](<assets/1740015615183.png>)

(1) 业务层需要调用数据层的方法，就需要在业务层 new 数据层的对象

(2) 如果数据层的实现类发生变化，那么业务层的代码也需要跟着改变，发生变更后，都需要进行编译打包和重部署

(3) 所以，现在代码在编写的过程中存在的问题是：**耦合度偏高**

**解决方案：不要主动使用 new 产生对象，转换为由外部提供对象。即 Ioc 控制反转思想。**

![](<assets/1740015617762.png>)

把框中的内容给去掉，就可以降低依赖了，同时为了使程序可以运行，Spring 就提出了一个解决方案:

*   使用对象时，在程序中不要主动使用 new 产生对象，转换为由**外部**提供对象

#### 2.3.2 IOC、IOC 容器、Bean、DI

**1.IOC（Inversion of Control）控制反转**

inversion 译为反转，相反。 

**（1）控制反转：**

*   **对象创建控制权由程序转移到外部（IOC 容器）**，此思想称为控制反转。使用对象时，由主动 new 产生对象转换为**由外部提供对象。**
    *   业务层要用数据层的类对象，以前是自己`new`的
    *   现在自己不 new 了，交给`别人[外部]`来创建对象
    *   `别人[外部]`就**反转控制**了数据层对象的**创建权**
    *   这种思想就是控制反转，别人【外部】是 IOC 容器

**(2)Spring 和 IOC 之间的关系是什么呢?**

*   Spring **实现**了 IOC 思想
*   Spring 提供了一个容器，称为 **IOC 容器**，用来充当 IOC 思想中的`别人[外部]`

**(3)IOC 容器的作用以及内部存放的是什么?**

作用： 

*   IOC 容器负责**对象的创建、初始化**等一系列工作，其中包含了**数据层和业务层**的**类对象**
*   **被创建或被管理的对象在 IOC 容器中统称为 Bean**

内部存放：

*   IOC 容器中放的就是一个个的 **Bean 对象**

**(4) 当 IOC 容器中创建好 service 和 dao 对象后，还需要 DI 依赖注入**

Dao(Data Access Object)：数据访问对象

*   service 运行需要依赖 dao 对象
*   IOC 容器中虽然有 service 和 dao 对象，但是 service 对象和 dao 对象没有任何关系
*   所以**要绑定 service 和 dao 对象之间的关系**

像这种**在容器中建立对象与对象之间的绑定关系就要用到 DI 依赖注入。**

**2.DI（Dependency Injection）依赖注入**

![](<assets/1740015619988.png>)

**(1) 依赖注入**

*   在容器中**绑定 bean 与 bean 之间的依赖关系**的整个过程，称为依赖注入
    *   业务层要用数据层的类对象，以前是自己`new`的，现在**靠`IOC容器`注入进来**
    *   这种**思想**就是依赖注入

**(2)IOC 容器中哪些 bean 之间要建立依赖关系?**

*   这个需要程序员根据业务需求提前建立好关系，如业务层需要依赖数据层，**service 就要和 dao 建立依赖关系**

Spring 的 IOC 和 DI 这两个概念的最终目标就是: **充分解耦**，具体实现方式:

*   使用 **IOC 容器**管理 bean（IOC)
*   在 IOC 容器内将有依赖关系的 bean 进行**依赖关系绑定**（DI）
*   最终结果为: **使用对象时不仅可以直接从 IOC 容器中获取，并且获取到的 bean 已经绑定了所有的依赖关系.**

#### 2.3.3 核心概念小结

这节比较重要，重点要理解`什么是IOC/DI思想`、`什么是IOC容器`和`什么是Bean`：

**(1) 什么 IOC/DI 思想?**

*   **IOC: 控制反转，控制反转的是对象的创建权，反转给 IOC 容器**
*   **DI: 依赖注入，绑定对象与对象之间的依赖关系**

**(2) 什么是 IOC 容器?**

Spring 创建了一个容器用来存放所创建的对象，这个容器就叫 IOC 容器

**(3) 什么是 Bean?**

容器中所存放的一个个对象就叫 Bean 或 Bean 对象

## 3，入门案例（配置开发版）

了解即可，以后更多用的是注解开发。 

介绍完 Spring 的核心概念后，接下来我们得思考一个问题就是，**Spring 到底是如何来实现 IOC 和 DI 的**，那接下来就通过一些简单的入门案例，来演示下具体实现过程:

### 3.1 IOC 入门案例

#### 3.1.1 入门案例思路分析

**IOC 创建 bean 有四种方式：无参构造方法实例化（常用）**、静态工厂实例化（了解）、实例工厂（了解）、FactoryBean（常用）。

这里入门案例使用的是无参构造方法实例化（常用）。

**(1)Spring 是使用容器来管理 bean 对象的，bean 对象有哪些?**

*   答：主要管理项目中所使用到的类对象，比如 **Service 和 Dao**

**(2) 如何将被管理的对象告知 IOC 容器?**

*   答：使用配置文件 applicationContext.xml 的 bean 标签。
    
    ```
        <bean/>
    
    ```
    

**(3)** **如何获取到 IOC 容器?**

*   答：Spring 框架提供相应的接口 ApplicationContext，实现类 ClassPathXmlApplicationContext
    
    ```
    //        ClassPathXmlApplicationContext了解即可，有new后面会被优化
            ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
    ```
    

**(4) 如何从容器中获取 bean?**

*   答：调用 ApplicationContext 的 getBean() 方法，根据 id 或 name 获取 Bean：

```
// 根据名称获取bean。默认的bean名称为首字母小写的类名。缺点：书写时没有提示，可能会写错。
       BookDao bookDao = (BookDao) applicationContext.getBean("bookDao");
       bookDao.save();
 
// 根据类型获取bean。优点：书写时有提示。缺点：IOC容器存在多个同类型不同名的Bean时，无法确定获取哪个Bean，报错。
       BookDao bookDao = (BookDao) applicationContext.getBean(BookDao.class);
// 根据名称+类型获取bean。推荐，精准指定，有提示避免报错，可读性高
       BookDao bookDao = (BookDao) applicationContext.getBean(BookDao.class);
       BookDao bookDao = (BookDao) applicationContext.getBean("bookDao",BookDao.class);
```

**(5) 使用 Spring 需要导入哪些坐标?**

*   答：用别人的东西，就需要在 pom.xml 添加 **spring-context 依赖**
    
    ```
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-context</artifactId>
                <version>5.3.22</version>
            </dependency>
    ```
    

#### 3.1.2 入门案例代码实现

**需求分析:**

将 BookServiceImpl 和 BookDaoImpl 交给 Spring 管理，并从容器中获取对应的 bean 对象进行方法调用。

**步骤：**

1. 创建 Maven 的 java 项目

2.pom.xml 添加 Spring 的依赖 jar 包

3. 创建 BookService,BookServiceImpl，BookDao 和 BookDaoImpl 四个类

4.resources 下添加 spring 配置文件，并完成 bean 的配置

5. 使用 Spring 提供的接口完成 IOC 容器的创建

6. 从容器中获取对象进行方法调用

**步骤 1: 创建 Maven 项目**

![](<assets/1740015621870.png>)

**步骤 2: 添加 Spring 的依赖 jar 包**

pom.xml 导入 Spring 和 junit 依赖

```
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.2.10.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

**步骤 3: 创建 dao 和 service**

创建 BookService,BookServiceImpl，BookDao 和 BookDaoImpl 四个类

![](<assets/1740015623746.png>)

```
public interface BookDao {
    public void save();
}
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ...");
    }
}
public interface BookService {
    public void save();
}
public class BookServiceImpl implements BookService {
    private BookDao bookDao = new BookDaoImpl();
    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```

**步骤 4: 添加 spring 配置文件**

resources 下添加 spring 配置文件 **applicationContext.xml。**

applicationContext 译为配置文件、应用上下文，是约定俗成的 Spring 配置文件命名规范。

![](<assets/1740015625797.png>)

 创建完上面有提示让配置应用程序上下文，点击确定即可：

![](<assets/1740015628065.png>)

![](<assets/1740015629124.png>)

**步骤 5: 在配置文件中完成 bean 的配置**

**bean 标签：**配置 bean  
**id 属性：**给 bean 起名字，唯一标识  
**class 属性：**给 bean 定义类型 

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <!--bean标签表示配置bean
        id属性表示给bean起名字，唯一标识
        class属性表示给bean定义类型
    -->
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl"/>
 
</beans>
```

****注意事项：bean 定义时 id 是 bean 的未知标识，属性在同一个上下文中 (配置文件) 不能重复****

**步骤 6: 获取 IOC 容器**

使用 Spring 提供的 **ApplicationContext** **接口**和 ClassPathXmlApplicationContext 实现类完成 IOC 容器的创建，创建 App 类，编写 main 方法

ClassPathXmlApplicationContext 实现类**了解即可**，用到 new 耦合度高，以后会被优化。

```
public class App {
    public static void main(String[] args) {
        //获取IOC容器
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml"); 
    }
}
```

**步骤 7: 从容器中获取 bean 并调用 bean 的方法**

```
public class App {
    public static void main(String[] args) {
        //获取IOC容器
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml"); 
//        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
//        bookDao.save();
        BookService bookService = (BookService) ctx.getBean("bookService");    //bookService是前面定义的bean的id，这里是根据id获取ioc容器里的bean
        bookService.save();
    }
}
```

**步骤 8: 运行程序**

测试结果为：

![](<assets/1740015629353.png>)

Spring 的 IOC 入门案例已经完成。但是在`BookServiceImpl`的类中**依然存在**`BookDaoImpl`对象的 **new 操作**，它们之间的耦合度还是比较高，这就需要用到下面的`DI:依赖注入`。

### 3.2 DI 入门案例，通过配置除掉 service 里的 new

#### 3.2.1 入门案例思路分析

**Spring 提供了两种注入方式**，分别是:

*   **setter 注入**
    *   简单类型
    *   **引用类型**
*   构造器注入
    *   简单类型
    *   引用类型

这里入门案例使用的是引用类型 setter 注入。

**(1) 要想实现依赖注入，必须要基于 IOC 管理 Bean**

*   **在 IOC 容器中绑定 bean 与 bean 之间的依赖关系**的整个过程，称为依赖注入

**(2)Service 中使用 new 形式创建的 Dao 对象是否保留?**

*   答：需要删除掉，同时为 dao 引用设置 setter 方法。最终要使用 IOC 容器中的 bean 对象
    
    ```
     
    public class BookServiceImpl implements BookService {
        //删除业务层中使用new的方式创建的dao对象
        private BookDao bookDao;
     
        public void save() {
            System.out.println("book service save ...");
            bookDao.save();
        }
        //提供对应的set方法
        public void setBookDao(BookDao bookDao) {
            this.bookDao = bookDao;
        }
    }
    ```
    

**(3)Service 中需要的 Dao 对象如何进入到 Service 中?**

*   答：**在 Service 中提供方法**，让 Spring 的 IOC 容器可以通过该方法传入 bean 对象

**(4)Service 与 Dao 间的关系如何描述?**

*   答：使用配置文件，service 的 bean 标签内通过 **property 标签配置 name 和 ref**。
    
    ```
        <bean/>
        <bean>
            <property name="bookDao" ref="bookDao"/>
        </bean>
    ```
    

#### 3.2.2 入门案例代码实现

**需求:**

基于 IOC 入门案例，在 BookServiceImpl 类中删除 new 对象的方式，使用 Spring 的 DI 完成 Dao 层的注入。

**步骤：**

1. 删除业务层中使用 new 的方式创建的 dao 对象

2. 在业务层提供 BookDao 的 setter 方法

3. 在配置文件中添加依赖注入的配置

4. 运行程序调用方法

**步骤 1: 去除业务实现类中的 new**，仅留下定义数据访问对象 dao 引用

在业务层 BookServiceImpl 类中，删除业务层中使用 new 的方式创建的 dao 对象

```
public class BookServiceImpl implements BookService {
    //删除业务层中使用new的方式创建的dao对象
    private BookDao bookDao;
 
    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```

**步骤 2: 为业务实现类属性提供 setter 方法**

在 BookServiceImpl 类中, 为 BookDao 提供 setter 方法

```
public class BookServiceImpl implements BookService {
    //删除业务层中使用new的方式创建的dao对象
    private BookDao bookDao;
 
    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
    //提供对应的set方法
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
}
```

**步骤 3: 修改配置完成 property 属性注入**

是 service 里调用 dao，所以要在配置文件 service 的 bean 中配置 service 和 dao 的关系

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--bean标签标示配置bean
        id属性标示给bean起名字
        class属性表示给bean定义类型
    -->
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
 
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <!--配置server与dao的关系-->
        <!--property标签表示配置当前bean的属性
                name属性表示配置哪一个具体的属性，值是在service中创建的私有dao引用。
                ref属性表示参照哪一个id的bean
        -->
        <property name="bookDao" ref="bookDao"/>
    </bean>
 
</beans>
```

**注意: property 标签中的两个 bookDao 的含义是不一样的**

**name="bookDao" 中`bookDao`：表示配置哪一个具体的属性。**

**作用：**让 Spring 的 IOC 容器在获取到名称后，将首字母大写，前面加 set 找对应的`setBookDao()`方法进行对象注入

**ref="bookDao" 中`bookDao`：表示参照哪一个 id 的 bean。**

**作用：**让 Spring 能在 IOC 容器中找到 id 为`bookDao`的 Bean 对象给`bookService`进行注入

综上所述，对应关系如下:

![](<assets/1740015629558.png>)

**步骤 4: 运行程序**

运行，测试结果为：

![](<assets/1740015629914.png>)

## 4，IOC 相关内容

IOC 容器主要是创建和管理类对象 bean 的，这一章节主要讲 bean 的配置、实例化、生命周期。

### 4.1 bean 基础配置

#### 4.1.0 概述

关于 bean 的基础配置中，需要大家掌握以下属性:

![](<assets/1740015630095.png>)

#### 4.1.1 bean 基础配置 (id 与 class)

对于 bean 的基础配置，在前面的案例中已经使用过:

```
<bean id="" class=""/>


```

其中，bean 标签的功能、使用方式以及 id 和 class 属性的作用，我们通过一张图来描述下

![](<assets/1740015630275.png>)

需要重点掌握的是:**bean 标签的 id 和 class 属性的使用**。

**思考：**

*   class 属性能不能写接口如`BookDao`的类全名呢?

答案肯定是不行，因为**接口是没办法创建对象的**。

*   前面提过为 bean 设置 id 时，id 必须唯一，但是如果由于命名习惯而产生了分歧后，该如何解决?

通过 bean 的 name 属性起别名。

#### 4.1.2 bean 的 name 属性

首先来看下别名的配置说明:

![](<assets/1740015630544.png>)

 **步骤 0：重新搭建和前面一样的的案例：**

![](<assets/1740015630817.png>)

**步骤 1：配置别名**

打开 spring 的配置文件 applicationContext.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <!--name:为bean指定别名，别名可以有多个，使用逗号，分号，空格进行分隔-->
    <bean id="bookService" name="service service4 bookEbi" class="com.itheima.service.impl.BookServiceImpl">
        <property name="bookDao" ref="bookDao"/>
    </bean>
 
    <!--scope：为bean设置作用范围，可选值为单例singloton，非单例prototype-->
    <bean id="bookDao" name="dao" class="com.itheima.dao.impl.BookDaoImpl"/>
</beans>
```

**说明:**service 起名 bookEbi。**Ebi 全称 Enterprise Business Interface，翻译为企业业务接口**

**步骤 2: 根据 name 在容器中获取 bean 对象**

```
public class AppForName {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        //此处根据bean标签的id属性和name属性的任意一个值来获取bean对象
        BookService bookService = (BookService) ctx.getBean("service4");
        bookService.save();
    }
}
```

**步骤 3: 运行程序**

测试结果为：

![](<assets/1740015631034.png>)

**注意事项:**

*   bean 依赖注入的 ref 属性可以是指定 bean 的 id 或 name，建议使用 id 注入
    
    ![](<assets/1740015631222.png>)
    
*   如果不存在, 则会报错 **NoSuchBeanDefinitionException**，如下:
    
    ![](<assets/1740015631487.png>)
    
    ![](<assets/1740015631795.png>)
    
    获取 bean 无论是通过 id 还是 name 获取，如果无法获取到，将抛出异常 **NoSuchBeanDefinitionException**
    

#### 4.1.3 bean 作用范围 scope 配置，单例和非单例

![](<assets/1740015632069.png>)

singleton 译为单例，单独的人、物。

prototype 译为非单例，原型、雏形

**默认情况下，Spring 创建的 bean 对象都是单例的**。

**单例避免了对象的频繁创建与销毁**，**达到了 bean 对象的复用**。

**验证 IOC 容器中对象是否为单例**

**单例 singleton：** 下面两个引用指向同一个实例化的 bean 对象

```
        BookDao bookDao1 = (BookDao) ctx.getBean("bookDao");
        BookDao bookDao2 = (BookDao) ctx.getBean("bookDao");
```

**验证思路**

​ 同一个 bean 获取两次，将对象打印到控制台，看打印出的地址值是否一致。

**具体实现**

*   创建一个 AppForScope 的类，在其 main 方法中来验证
    
    ```
    public class AppForScope {
        public static void main(String[] args) {
            ApplicationContext ctx = new 
                ClassPathXmlApplicationContext("applicationContext.xml");
     
            BookDao bookDao1 = (BookDao) ctx.getBean("bookDao");
            BookDao bookDao2 = (BookDao) ctx.getBean("bookDao");
            System.out.println(bookDao1);
            System.out.println(bookDao2);
        }
    }
    ```
    
*   打印，观察控制台的打印结果
    
    ![](<assets/1740015632336.png>)
    
*   结论: **默认情况下，Spring 创建的 bean 对象都是单例的**
    

获取到结论后，问题就来了，那如果我想创建出来非单例的 bean 对象，该如何实现呢?

**配置 bean 为非单例**

*   **将 scope 设置为`prototype`**
    
    ```
    <bean name="dao" scope="prototype"/>
    
    
    ```
    
    运行 AppForScope，打印看结果
    
    ![](<assets/1740015632544.png>)
    
*   结论，使用 bean 的`scope`属性可以控制 bean 的创建是否为单例：
    
    *   `singleton`默认为单例
    *   `prototype`为非单例

**思考**

*   **为什么 bean 默认为单例?**
    *   bean 为单例的意思是在 Spring 的 IOC 容器中只会有该类的一个对象
    *   bean 对象只有一个就**避免了对象的频繁创建与销毁**，**达到了 bean 对象的复用**，性能高
*   **bean 在容器中是单例的，会不会产生线程安全问题?**
    *   如果对象是**有状态对象**（该对象有成员变量可以用来存储数据的）
    *   因为所有请求线程共用一个 bean 对象，所以会**存在线程安全问题**。
    *   如果对象是**无状态对象**（该对象没有成员变量没有进行数据存储的，不建议存在 IOC 容器中）
    *   因方法中的局部变量在方法调用完成后会被销毁，所以**不会存在线程安全问题**。
*   **哪些 bean 对象适合交给容器进行管理?**
    *   表现层对象
    *   业务层对象
    *   数据层对象
    *   工具对象
*   **哪些 bean 对象不适合交给容器进行管理?**
    *   **封装实例的域对象**，因为会引发线程安全问题，所以不适合。
*   **JSP 四大域对象**

1.page 域对象（只在当前页面中有效）

2.request 域对象（只在一次请求中有效，服务端跳转有效，客户端跳转无效）

3.session 域对象（在一次会话中有效，服务端客户端跳转都有效）

4.application 域对象（在整个应用程序中都有效）

**域对象：**可以在不同的 [Servlet](https://so.csdn.net/so/search?q=Servlet&spm=1001.2101.3001.7020 "Servlet") 中进行数据传递的对象就称为域对象。

**域对象必须有的方法：**

```
    setAttribute(name,value);存储数据的方法
    getAttribute(name);根据name获取对应数据值
    removeAttribute(name)；删除数据
```

### 4.2 bean 的三种创建方式

思考：

**容器是如何来创建对象的呢?**

bean 本质上就是对象，对象在 new 的时候会使用构造方法完成，那创建 bean 也是使用构造方法完成的。

在这块内容中主要解决两部分内容，分别是

*   bean 是如何创建的
*   实例化 bean 的三种方式，`构造方法`,`静态工厂`和`实例工厂`

#### 4.2.1 环境准备

重新准备个开发环境，

*   创建一个 Maven 项目
*   pom.xml 添加 Spring 依赖
*   resources 下添加 spring 的配置文件 applicationContext.xml

这些步骤和前面的都一致，大家可以快速的拷贝即可，最终项目的结构如下:

![](<assets/1740015632886.png>)

#### 4.2.2 bean 的创建方法: 1：无参构造方法实例化（常用）

**Spring 底层是通过反射的无参构造方法创建类的对象 bean。** 

**步骤 1: 准备需要被创建的类**

准备一个 BookDao 和 BookDaoImpl 类

```
 
public interface BookDao {
    public void save();
}
 
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ...");
    }
 
}
```

**步骤 2: 将类配置到 Spring 容器**

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
 
</beans>
```

**步骤 3: 编写运行程序**

```
public class AppForInstanceBook {
    public static void main(String[] args) {
        ApplicationContext ctx = new 
            ClassPathXmlApplicationContext("applicationContext.xml");
        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
        bookDao.save();
 
    }
}
```

**步骤 4:DAO 类中提供无参构造函数测试，证明 IOC 容器创建对象通过无参构造方法**

在 BookDaoImpl 类中添加一个无参构造函数，并打印一句话，方便观察结果。

```
public class BookDaoImpl implements BookDao {
    public BookDaoImpl() {
        System.out.println("book dao constructor is running ....");
    }
    public void save() {
        System.out.println("book dao save ...");
    }
 
}
```

运行程序，如果控制台有打印构造函数中的输出，说明 Spring 容器在创建对象的时候也走的是构造函数

![](<assets/1740015633151.png>)

**步骤 5: 将构造函数改成私有，证明 IOC 容器通过反射调用无参构造方法**

```
public class BookDaoImpl implements BookDao {
    private BookDaoImpl() {
        System.out.println("book dao constructor is running ....");
    }
    public void save() {
        System.out.println("book dao save ...");
    }
 
}
```

运行程序，能执行成功, 说明内部走的依然是构造函数, 能访问到类中的私有构造方法, 显而易见 **Spring 底层用的是反射**

![](<assets/1740015633377.png>)

**步骤 6: 构造函数中添加一个参数，证明 IOC 通过无参数的构造方法创建 bean**

```
public class BookDaoImpl implements BookDao {
    private BookDaoImpl(int i) {
        System.out.println("book dao constructor is running ....");
    }
    public void save() {
        System.out.println("book dao save ...");
    }
 
}
```

运行程序，

**程序会报错**，说明 **Spring 底层使用的是类的无参构造方法**。

![](<assets/1740015633650.png>)

#### 4.2.3 Spring 的报错信息从下往上看

![](<assets/1740015633853.png>)

*   **错误信息从下往上依次查看**，因为上面的错误大都是对下面错误的一个包装，**最核心错误是在最下面**
*   Caused by: java.lang.NoSuchMethodException: com.itheima.dao.impl.BookDaoImpl.`<init>`()
    *   Caused by 翻译为`引起`，即出现错误的原因
    *   java.lang.NoSuchMethodException: 抛出的异常为`没有这样的方法异常`
    *   com.itheima.dao.impl.BookDaoImpl.`<init>`(): 哪个类的哪个方法没有被找到导致的异常，**`<init>`() 指定是类的无参构造方法**

如果最后一行错误获取不到错误信息，接下来查看第二层:

Caused by: org.springframework.beans.BeanInstantiationException: Failed to instantiate [com.itheima.dao.impl.BookDaoImpl]: No default constructor found; nested exception is java.lang.NoSuchMethodException: com.itheima.dao.impl.BookDaoImpl.`<init>`()

*   nested: 嵌套的意思，后面的异常内容和最底层的异常是一致的
*   Caused by: org.springframework.beans.BeanInstantiationException: Failed to instantiate [com.itheima.dao.impl.BookDaoImpl]: No default constructor found;
    *   Caused by: `引发`
    *   BeanInstantiationException: 翻译为`bean实例化异常`
    *   No default constructor found: 没有一个默认的构造函数被发现

看到这其实错误已经比较明显，给大家个练习，把倒数第三层的错误分析下吧:

Exception in thread "main" org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'bookDao' defined in class path resource [applicationContext.xml]: Instantiation of bean failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [com.itheima.dao.impl.BookDaoImpl]: No default constructor found; nested exception is java.lang.NoSuchMethodException: com.itheima.dao.impl.BookDaoImpl.`<init>`()。

至此，关于 Spring 的构造方法实例化就已经学习完了，因为每一个类默认都会提供一个无参构造函数，所以其实真正在使用这种方式的时候，我们什么也不需要做。这也是我们以后比较常用的一种方式。

#### 4.2.4 bean 的创建方式 2：静态工厂实例化（了解）

静态工厂一般在 service 层里使用，用于获取 dao 对象。 

**了解为主，**这种方式一般是用来兼容早期的一些老系统，所以**了解为主**。

**回顾，工厂方式创建对象**

以前最常接触到的工厂类是封装 SqlSessionFactory 工具类，提高代码复用性、通过静态代码块避免工厂类资源浪费。

```
public class SqlSessionFactoryUtils {
 
    private static SqlSessionFactory sqlSessionFactory;
 
    static {
        //静态代码块会随着类的加载而自动执行，且只执行一次
        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
 
 
    public static SqlSessionFactory getSqlSessionFactory(){
        return sqlSessionFactory;
    }
}
```

```
SqlSessionFactory sqlSessionFactory=SqlSessionFactoryUtils.getSqlSessionFactory();

``` 

**工厂方式创建 bean** 

(1) 准备一个 OrderDao 和 OrderDaoImpl 类

```
public interface OrderDao {
    public void save();
}
 
public class OrderDaoImpl implements OrderDao {
    public void save() {
        System.out.println("order dao save ...");
    }
}
```

(2) 创建一个工厂类 **OrderDaoFactory** 并提供一个**静态方法创建对象**

```
//静态工厂创建对象
public class OrderDaoFactory {
    public static OrderDao getOrderDao(){
        return new OrderDaoImpl();
    }
}
```

(3) 编写 AppForInstanceOrder 运行类，在类中**通过工厂获取对象**

```
public class AppForInstanceOrder {
    public static void main(String[] args) {
        //通过静态工厂创建对象
        OrderDao orderDao = OrderDaoFactory.getOrderDao();
        orderDao.save();
    }
}
```

(4) 运行后，可以查看到结果

![](<assets/1740015633954.png>)

**Spring 静态工厂实例化**

具体实现步骤为:

**(1) 在 spring 的配置文件 application.properties 中配置工厂类的 bean**

```
<bean id="orderDao" class="com.itheima.factory.OrderDaoFactory" factory-method="getOrderDao"/>


```

class: 工厂类的类全名

**factory-mehod: 具体工厂类中创建对象的方法名**

对应关系如下图:

![](<assets/1740015634133.png>)

**(2) 在 AppForInstanceOrder 运行类，使用从 IOC 容器中获取 bean 的方法进行运行测试**

```
public class AppForInstanceOrder {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        //直接获取dao对象。获取方式是调用配置的factory里的静态方法getOrderDao
        OrderDao orderDao = (OrderDao) ctx.getBean("orderDao");
 
        orderDao.save();
 
    }
}
```

(3) 运行后，可以查看到结果

![](<assets/1740015634348.png>)

**出现问题：工厂类中也是直接 new 对象的，和自己直接 new 的区别**

**主要的原因是:**

*   在工厂的静态方法中，我们除了 new 对象还可以做其他的一些业务操作，这些操作必不可少, 如 SqlSessionFactoryUtil 类里有静态代码块创建 SqlSessionFactory 对象。又例如:

```
public class OrderDaoFactory {
    public static OrderDao getOrderDao(){
        System.out.println("factory setup....");//模拟必要的业务操作
        return new OrderDaoImpl();
    }
}
```

![](<assets/1740015634687.png>)

这种方式一般是用来兼容早期的一些老系统，所以**了解为主**。

#### 4.2.5 bean 的创建方式 3：实例工厂（了解）与 FactoryBean（常用）

**环境准备**

(1) 准备一个 UserDao 和 UserDaoImpl 类

```
public interface UserDao {
    public void save();
}
 
public class UserDaoImpl implements UserDao {
 
    public void save() {
        System.out.println("user dao save ...");
    }
}
```

(2) 创建一个工厂类 OrderDaoFactory 并提供一个普通方法，注意此处和静态工厂的工厂类不一样的地方是**方法不是静态方法**

```
public class UserDaoFactory {
    public UserDao getUserDao(){
        return new UserDaoImpl();
    }
}
```

(3) 编写 AppForInstanceUser 运行类，在类中通过工厂获取对象

```
public class AppForInstanceUser {
    public static void main(String[] args) {
        //创建实例工厂对象
        UserDaoFactory userDaoFactory = new UserDaoFactory();
        //通过实例工厂对象创建对象
        UserDao userDao = userDaoFactory.getUserDao();
        userDao.save();
}
```

(4) 运行后，可以查看到结果

![](<assets/1740015634976.png>)

对于上面这种实例工厂的方式如何交给 Spring 管理呢?

**实例工厂实例化方法 A**

**了解即可**，更多用下面方法 B，**FactoryBean**

**(1) 在 spring 的配置文件中添加以下内容:**

```
<!--   第一步，创建实例化工厂对象，实际无意义，主要为了第二步用它-->
<bean id="userFactory" class="com.itheima.factory.UserDaoFactory"/>
<!--   第二步，调用实例化工厂对象中的方法来创建bean，不用class属性是因为getUserDao返回值是UserDao，ioc容器是可以找到UserDao的-->
<bean id="userDao" factory-method="getUserDao" factory-bean="userFactory"/>
```

**实例化工厂运行的顺序是:**

*   **创建实例化工厂对象**, 对应的是第一行配置
    
*   **调用对象中的方法来创建 bean**，对应的是第二行配置
    
    *   **factory-bean:** 工厂的实例对象
        
    *   **factory-method:** 工厂 bean 的 id, 对应关系如下:
        
        ![](<assets/1740015635219.png>)
        

factory-mehod: 具体工厂类中创建对象的方法名

(2) 在 AppForInstanceUser 运行类，使用从 IOC 容器中获取 bean 的方法进行运行测试

```
public class AppForInstanceUser {
    public static void main(String[] args) {
        ApplicationContext ctx = new 
            ClassPathXmlApplicationContext("applicationContext.xml");
        UserDao userDao = (UserDao) ctx.getBean("userDao");
        userDao.save();
    }
}
```

(3) 运行后，可以查看到结果

![](<assets/1740015635479.png>)

实例工厂实例化的方式就已经介绍完了，配置的过程还是比较复杂，所以 Spring 为了简化这种配置方式就提供了一种叫`FactoryBean`的方式来简化开发。

**实例工厂实例化方法 B：FactoryBean 的使用**

(1) 创建一个 UserDaoFactoryBean 的类，**实现 FactoryBean 接口**，重写接口的方法。**这两个方法会在创建 ApplicationContext 对象时候就先后调用。**

```
public class UserDaoFactoryBean implements FactoryBean<UserDao> {
    //代替原始实例工厂中创建对象的方法
    public UserDao getObject() throws Exception {
        //返回dao实现类的实例
        return new UserDaoImpl();
    }
    //返回所创建类的Class对象
    public Class<?> getObjectType() {
        //返回dao接口的字节码
        return UserDao.class;
    }
}
```

(2) 在 Spring 的配置文件中进行配置，注意 class 里 UserDaoFactoryBean

```
<bean id="userDao" class="com.itheima.factory.UserDaoFactoryBean"/>


```

(3)AppForInstanceUser 运行类不用做任何修改，直接运行

![](<assets/1740015635820.png>)

 **注意：**

在 IOC 容器创建时会直接创建其内所有 bean 对象，像下面代码，即使没有调用任何方法，运行后会输出 getObject 和 getObjectType

```
public class BookDaoFactoryBean implements FactoryBean<BookDao> {
    @Override
    public BookDao getObject() throws Exception {
        System.out.println("getObject");
        return new BookDaoImpl();
    }
 
    @Override
    public Class<?> getObjectType() {
        System.out.println("getObjectType");
        return BookDao.class;
    }
}
```

```
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");

```

![](<assets/1740015636150.png>) 

这种方式在 Spring 去整合其他框架的时候会被用到，所以这种方式需要大家理解掌握。

**FactoryBean 接口的三个方法**

查看源码会发现，FactoryBean 接口其实会有三个方法，分别是:

```
T getObject() throws Exception;
 
Class<?> getObjectType();
 
default boolean isSingleton() {
        return true;
}
```

方法一: getObject()，被重写后，在方法中进行对象的创建并返回

方法二: getObjectType(), 被重写后，主要返回的是被创建类的 Class 对象

方法三: 没有被重写，因为它已经给了默认值，从方法名中可以看出其作用是设置对象是否为单例，默认 true，从意思上来看，我们猜想默认应该是单例，如何来验证呢?

思路很简单，就是从容器中获取该对象的多个值，打印到控制台，查看是否为同一个对象。

```
public class AppForInstanceUser {
    public static void main(String[] args) {
        ApplicationContext ctx = new 
            ClassPathXmlApplicationContext("applicationContext.xml");
        UserDao userDao1 = (UserDao) ctx.getBean("userDao");
        UserDao userDao2 = (UserDao) ctx.getBean("userDao");
        System.out.println(userDao1);
        System.out.println(userDao2);
    }
}
```

打印结果，如下:

![](<assets/1740015636307.png>)

通过验证，会发现默认是单例。

**FactoryBean 设置单例、非单例：**

只需要将 isSingleton() 方法进行重写，修改返回为 false，即可

```
//FactoryBean创建对象
public class UserDaoFactoryBean implements FactoryBean<UserDao> {
    //代替原始实例工厂中创建对象的方法
    public UserDao getObject() throws Exception {
        return new UserDaoImpl();
    }
 
    public Class<?> getObjectType() {
        return UserDao.class;
    }
 
    public boolean isSingleton() {
        return false;
    }
}
```

重新运行 AppForInstanceUser，查看结果

![](<assets/1740015636580.png>)

从结果中可以看出现在已经是非单例了，但是一般情况下我们都会采用单例，也就是采用默认即可。所以 isSingleton() 方法一般不需要进行重写。

#### 4.2.6 bean 实例化小结

通过这一节的学习，需要掌握:

(1)bean 是如何创建的呢?

```
构造方法


```

(2)Spring 的 IOC 实例化对象的三种方式分别是:

*   **构造方法 (常用)**
*   **静态工厂 (了解)**
*   **实例工厂 (了解)**
    *   **FactoryBean(实用)**

这些方式中，重点掌握`构造方法`和`FactoryBean`即可。

需要注意的一点是，构造方法在类中默认会提供，但是如果重写了构造方法，默认的就会消失，在使用的过程中需要注意，如果需要重写构造方法，最好把默认的构造方法也重写下。

### 4.3 bean 的生命周期

**思考：**

*   什么是生命周期?
    *   从创建到消亡的完整过程, 例如人从出生到死亡的整个过程就是一个生命周期。
*   **bean 生命周期是什么?**
    *   bean 对象从创建到销毁的整体过程。
*   **bean 生命周期控制是什么**?
    *   在 bean 创建后到销毁前做一些事情。

#### 4.3.1 环境准备

为了方便大家后期代码的阅读，我们重新搭建下环境:

*   创建一个 Maven 项目
*   pom.xml 添加依赖
*   resources 下添加 spring 的配置文件 applicationContext.xml

这些步骤和前面的都一致，大家可以快速的拷贝即可，最终项目的结构如下:

![](<assets/1740015636843.png>)

(1) 项目中添加 BookDao、BookDaoImpl、BookService 和 BookServiceImpl 类

```
public interface BookDao {
    public void save();
}
 
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ...");
    }
}
 
public interface BookService {
    public void save();
}
 
public class BookServiceImpl implements BookService{
    private BookDao bookDao;
 
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
 
    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```

(2)resources 下提供 spring 的配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
</beans>
```

(3) 编写 AppForLifeCycle 运行类，加载 Spring 的 IOC 容器，并从中获取对应的 bean 对象

```
public class AppForLifeCycle {
    public static void main( String[] args ) {
        ApplicationContext ctx = new 
            ClassPathXmlApplicationContext("applicationContext.xml");
        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
        bookDao.save();
    }
}
```

#### 4.3.2 生命周期控制

接下来，在上面这个环境中来为 BookDao 添加生命周期的控制方法，具体的控制有两个阶段:

*   bean **创建 init** 之后，想要添加内容，比如用来初始化需要用到资源
*   bean **销毁 destory** 之前，想要添加内容，比如用来释放用到的资源

**步骤 1: 添加初始化和销毁方法**

针对这两个阶段，我们在 BooDaoImpl 类中分别添加两个方法，**方法名任意**

```
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ...");
    }
    //表示bean初始化对应的操作
    public void init(){
        System.out.println("init...");
    }
    //表示bean销毁前对应的操作
    public void destory(){
        System.out.println("destory...");
    }
}
```

**步骤 2: 配置生命周期**

在配置文件添加配置，如下:

```
<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl" init-method="init" destroy-method="destory"/>


```

**步骤 3: 运行程序**

运行 AppForLifeCycle 打印结果为:

![](<assets/1740015637046.png>)

从结果中可以看出，**init 方法执行了，但是 destroy 方法却未执行**，这是为什么呢?

*   Spring 的 **IOC 容器是运行在 JVM 中**
*   运行 main 方法后, JVM 启动, Spring 加载配置文件生成 IOC 容器, 从容器获取 bean 对象，然后调方法执行
*   main 方法执行完后，**JVM 退出**，这个时候 IOC 容器中的 **bean 还没有来得及销毁就已经结束了**
*   所以没有调用对应的 destroy 方法

**解决办法 1：**使用 ClassPathXmlApplicationContext 的 **close 方法**。

**解决方法 2：注册钩子**关闭容器

另外还可以用 Spring 的两个接口**`InitializingBean`， `DisposableBean`**实现初始化和销毁，但想要销毁方法执行出来还得用上面两种解决办法。

#### 4.3.3 关闭 IOC 容器方法：close 关闭容器、**注册钩子关闭容器**

**了解即可**，主要用 4.3.4 中的**`InitializingBean`， `DisposableBean`控制生命周期** 

**close 关闭容器**

*   ApplicationContext 中没有 close 方法
    
*   需要将 ApplicationContext 更换成 ClassPathXmlApplicationContext
    
    ```
    ClassPathXmlApplicationContext ctx = new 
        ClassPathXmlApplicationContext("applicationContext.xml");
    ```
    
*   调用 ctx 的 close() 方法
    
    ```
    ctx.close();
    
    
    ```
    
*   运行程序，就能执行 destroy 方法的内容
    
    ![](<assets/1740015637255.png>)
    

**销毁方法二：注册钩子关闭容器**

*   在容器未关闭之前，提前设置好回调函数，让 JVM 在退出之前回调此函数来关闭容器
    
*   调用 ClassPathXmlApplicationContext 的 registerShutdownHook() 方法
    
    ```
    ctx.registerShutdownHook();
    
    
    ```
    
    **注意:**registerShutdownHook 在 ApplicationContext 中也没有
    
*   运行后，查询打印结果
    
    ![](<assets/1740015637531.png>)
    

两种方式介绍完后，close 和 registerShutdownHook 选哪个?

**相同点:** 这两种都能用来关闭容器

**不同点:**

close() 是在**调用的时候**强制关闭，如果后面还有 IOC 的相关代码会报错。

registerShutdownHook() 是在 **JVM 退出前调用**关闭。

#### 4.3.4 **`InitializingBean`， `DisposableBean`控制生命周期**

**`Disposable译为用完即丢弃的，一次性的`**

Spring 提供了两个接口来完成生命周期的控制，好处是可以不用再进行配置`init-method`和`destroy-method，坏处是`想要销毁方法执行出来还得用上面两种解决办法。

**代码示例：**

修改 service 实现类 BookServiceImpl，添加两个接口`InitializingBean`， `DisposableBean`并实现接口中的两个方法`afterPropertiesSet`和`destroy`

```
public class BookServiceImpl implements BookService, InitializingBean, DisposableBean {
    private BookDao bookDao;
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
    public void save() {
        System.out.println("book service save ...");
        bookDao.save(); 
    }
    public void destroy() throws Exception {
        System.out.println("service destroy");
    }
    public void afterPropertiesSet() throws Exception {
        System.out.println("service init");
    }
}
```

```
public class BookDaoImpl implements BookDao {
    @Override
    public void save() {
        System.out.println("BookDao save...");
    }
 
    private void init() {
        System.out.println("book dao init");
    }
 
    private void destroy() {
        System.out.println("book dao destroy");
    }
}
```

​​​​​​​ 

```
    <bean id="bookDao" class="package1.dao.impl.BookDaoImpl" init-method="init" destroy-method="destroy"/>
    <bean id="bookService" class="package1.service.impl.BookServiceImpl" >
        <property name="bookDao" ref="bookDao"/>
    </bean>
```

```
public class Demo {
    public static void main(String[] args) {
//        ClassPathXmlApplicationContext了解即可，有new后面会被优化
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        System.out.println("创建IOC容器后...");
        BookDao bookDao=(BookDao)applicationContext.getBean("bookDao");
        bookDao.save();
 
        applicationContext.close();
    }
}
```

重新运行 AppForLifeCycle 类，

![](<assets/1740015637805.png>)

思考：

**dao 先初始化、最后销毁的原因：**

配置的初始化和销毁方法，要更严格，实现接口的方法都是创建和销毁中间完成的。

**控制台出现 service 的原因：**

**创建 IOC 容器时会创建所有的 bean 对象，并加载这些对象的 init 和 destroy 方法。**

就像前面 4.2.5 FactoryBean 那里，创建 IOC 容器时候会调用 getObject 和 getObjectType 方法。

**小细节**

*   对于 InitializingBean 接口中的 afterPropertiesSet 方法，翻译过来为`属性设置之后`。
    
*   对于 BookServiceImpl 来说，bookDao 是它的一个属性，setBookDao 方法是 Spring 的 IOC 容器为其注入属性的方法
    
*   ​​​​​​​**思考: afterPropertiesSet 和 setBookDao 谁先执行?**
    
    *   从方法名分析，猜想应该是 setBookDao 方法先执行
        
    *   验证思路，在 setBookDao 方法中添加一句话
        
        ```
        public void setBookDao(BookDao bookDao) {
                System.out.println("set .....");
                this.bookDao = bookDao;
            }
        ```
        
    *   重新运行 AppForLifeCycle，打印结果如下:
        
        ![](<assets/1740015638017.png>)
        
        验证的结果和我们猜想的结果是一致的，所以初始化方法会在类中属性设置之后执行。
        

#### 4.3.5 bean 生命周期小结

(1) 关于 Spring 中对 bean 生命周期控制提供了两种方式:

*   在配置文件中的 bean 标签中添加`init-method`和`destroy-method`属性
*   类实现`InitializingBean`与`DisposableBean`接口，这种方式了解下即可。

(2) 对于 bean 的生命周期控制在 bean 的整个生命周期中所处的位置如下:

*   初始化容器
    *   1. 创建对象 (内存分配)
    *   2. 执行构造方法
    *   3. 执行属性注入 (set 操作)
    *   **4. 执行 bean 初始化方法**
*   使用 bean
    *   1. 执行业务操作
*   关闭 / 销毁容器
    *   **1. 执行 bean 销毁方法**

(3) 关闭容器的两种方式:

*   ConfigurableApplicationContext 是 ApplicationContext 的子类
    *   close() 方法
    *   registerShutdownHook() 方法

## 5，DI 相关内容

前面我们已经完成了 bean 相关操作的讲解，接下来就进入第二个大的模块`DI依赖注入`，首先来介绍下 Spring 中有哪些注入方式?

我们先来**思考**

*   向一个类中传递数据的方式有几种?
    *   普通方法 (set 方法)
    *   构造方法
*   依赖注入描述了在容器中建立 bean 与 bean 之间的依赖关系的过程，如果 bean 运行需要的是数字或字符串呢?
    *   引用类型（前面 3.2 快速入门方案，service 里注入 dao 的引用）
    *   简单类型 (基本数据类型与 String)

**Spring 提供了两种注入方式：**

*   **setter 注入**
    *   简单类型
    *   引用类型（前面 3.2 快速入门方案）
*   **构造器注入**
    *   简单类型
    *   引用类型

### 5.1 setter 注入

1.  对于 setter 方式注入引用类型的方式之前已经学习过，快速回顾下:
    
2.  在 bean 中定义引用类型属性，并提供可访问的 **set** 方法
    

```
public class BookServiceImpl implements BookService {
    private BookDao bookDao;
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
}
```

*   配置中使用 **property** 标签 **ref** 属性注入引用类型对象

```
<bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
    <property name="bookDao" ref="bookDao"/>
</bean>
 
<bean id="bookDao" class="com.itheima.dao.imipl.BookDaoImpl"/>
```

#### 5.1.1 环境准备

为了更好的学习下面内容，我们依旧准备一个新环境:

*   创建一个 Maven 项目
*   pom.xml 添加依赖
*   resources 下添加 spring 的配置文件

这些步骤和前面的都一致，大家可以快速的拷贝即可，最终项目的结构如下:

![](<assets/1740015638236.png>)

(1) 项目中添加 BookDao、BookDaoImpl、UserDao、UserDaoImpl、BookService 和 BookServiceImpl 类

```
public interface BookDao {
    public void save();
}
 
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ...");
    }
}
public interface UserDao {
    public void save();
}
public class UserDaoImpl implements UserDao {
    public void save() {
        System.out.println("user dao save ...");
    }
}
 
public interface BookService {
    public void save();
}
 
public class BookServiceImpl implements BookService{
    private BookDao bookDao;
 
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
 
    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```

(2)resources 下提供 spring 的配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <property name="bookDao" ref="bookDao"/>
    </bean>
</beans>
```

(3) 编写 AppForDISet 运行类，加载 Spring 的 IOC 容器，并从中获取对应的 bean 对象

```
public class AppForDISet {
    public static void main( String[] args ) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        BookService bookService = (BookService) ctx.getBean("bookService");
        bookService.save();
    }
}
```

接下来，在上面这个环境中来完成 setter 注入的学习:

#### 5.1.2 注入多个引用数据类型（property 标签的 name 和 ref）

因为有 ref，所以叫引用数据类型，引用值是被注入类里定义的私有注入类的对象。

需求: 在 bookServiceImpl 对象中注入 userDao

1. 在 BookServiceImpl 中声明 userDao 属性

2. 为 userDao 属性提供 setter 方法

3. 在配置文件中使用 property 标签注入

**步骤 1: 声明属性并提供 setter 方法**

在 BookServiceImpl 中声明 userDao 属性，并提供 setter 方法

```
public class BookServiceImpl implements BookService{
    private BookDao bookDao;
    private UserDao userDao;
 
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
 
    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
        userDao.save();
    }
}
```

**步骤 2: 配置文件中进行注入配置**

在 applicationContext.xml 配置文件中使用 property 标签注入

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
    <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <property name="bookDao" ref="bookDao"/>
        <property name="userDao" ref="userDao"/>
    </bean>
</beans>
```

![](<assets/1740015638467.png>)

**步骤 3: 运行程序**

运行 AppForDISet 类，查看结果，说明 userDao 已经成功注入。

![](<assets/1740015638559.png>)

#### 5.1.3 注入简单数据类型（property 标签的 name 和 value）

**需求：**

给 BookDaoImpl 注入一些简单数据类型的数据

参考引用数据类型的注入，我们可以推出具体的步骤为:

1. 在 BookDaoImpl 类中声明对应的简单数据类型的属性

2. 为这些属性提供对应的 setter 方法

3. 在 applicationContext.xml 中配置

**思考:**

**问题：**引用类型使用的是`<property name="" ref=""/>`, 简单数据类型还是使用 ref 么?

**答案：**不是 ref，二手 property 标签的 name 和 value。

问题：ref 是指向 Spring 的 IOC 容器中的另一个 bean 对象的，对于简单数据类型，没有对应的 bean 对象，该如何配置?

答案：

**步骤 1: 声明属性并提供 setter 方法**

在 BookDaoImpl 类中声明对应的简单数据类型的属性, 并提供对应的 setter 方法

```
public class BookDaoImpl implements BookDao {
 
    private String databaseName;
    private int connectionNum;
 
    public void setConnectionNum(int connectionNum) {
        this.connectionNum = connectionNum;
    }
 
    public void setDatabaseName(String databaseName) {
        this.databaseName = databaseName;
    }
 
    public void save() {
        System.out.println("book dao save ..."+databaseName+","+connectionNum);
    }
}
```

**步骤 2: 配置文件中进行注入配置**

在 applicationContext.xml 配置文件中使用 property 标签注入

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
        <property name="databaseName" value="mysql"/>
         <property name="connectionNum" value="10"/>
    </bean>
    <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <property name="bookDao" ref="bookDao"/>
        <property name="userDao" ref="userDao"/>
    </bean>
</beans>
```

property 标签里，name 填的是 bean 类里定义的私有变量名，value 填的是注入的值。

**注意:**

value 后面跟的是简单数据类型，注意数据类型格式。

对于参数类型，Spring 在注入的时候会自动转换，但是不能写成

```
<property name="connectionNum" value="abc"/>


```

这样的话，spring 在将`abc`转换成 int 类型的时候就会报错。

**步骤 3: 运行程序**

运行 AppForDISet 类，查看结果，说明 userDao 已经成功注入。

![](<assets/1740015638779.png>)

**注意:** 两个 property 注入标签的顺序可以任意。

对于 setter 注入方式的基本使用就已经介绍完了，

*   对于引用数据类型使用的是`<property name="" ref=""/>`
*   对于简单数据类型使用的是`<property name="" value=""/>`

### 5.2 构造器注入

#### 5.2.1 环境准备

构造器注入也就是构造方法注入，学习之前，还是先准备下环境，和前面的都一致:

*   创建一个 Maven 项目
*   pom.xml 添加依赖
*   resources 下添加 spring 的配置文件

这些步骤和前面的都一致，大家可以快速的拷贝即可，最终项目的结构如下:

![](<assets/1740015639160.png>)

(1) 项目中添加 BookDao、BookDaoImpl、UserDao、UserDaoImpl、BookService 和 BookServiceImpl 类

```
public interface BookDao {
    public void save();
}
 
public class BookDaoImpl implements BookDao {
 
    private String databaseName;
    private int connectionNum;
 
    public void save() {
        System.out.println("book dao save ...");
    }
}
public interface UserDao {
    public void save();
}
public class UserDaoImpl implements UserDao {
    public void save() {
        System.out.println("user dao save ...");
    }
}
 
public interface BookService {
    public void save();
}
 
public class BookServiceImpl implements BookService{
    private BookDao bookDao;
 
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
 
    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```

(2)resources 下提供 spring 的配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <property name="bookDao" ref="bookDao"/>
    </bean>
</beans>
```

(3) 编写 AppForDIConstructor 运行类，加载 Spring 的 IOC 容器，并从中获取对应的 bean 对象

```
public class AppForDIConstructor {
    public static void main( String[] args ) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        BookService bookService = (BookService) ctx.getBean("bookService");
        bookService.save();
    }
}
```

#### 5.2.2 构造器注入引用数据类型（constructor-arg 标签的 name 和 ref）

接下来，在上面这个环境中来完成构造器注入的学习:

需求：将 BookServiceImpl 类中的 bookDao 修改成使用构造器的方式注入。

1. 将 bookDao 的 setter 方法删除掉

2. 添加带有 bookDao 参数的构造方法

3. 在 applicationContext.xml 中配置

**步骤 1: 删除 setter 方法并提供构造方法**

在 BookServiceImpl 类中将 bookDao 的 setter 方法删除掉, 并添加带有 bookDao 参数的构造方法

```
public class BookServiceImpl implements BookService{
    private BookDao bookDao;
 
    public BookServiceImpl(BookDao bookDao) {
        this.bookDao = bookDao;
    }
 
    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```

**步骤 2: 配置文件中进行配置构造方式注入**

在 applicationContext.xml 中配置，bean 便签中的 constructor-arg 便签

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <constructor-arg name="bookDao" ref="bookDao"/>
    </bean>
</beans>
```

**说明:**

arg 是 argument 的缩写，译为参数、争论、理由。constructor-arg 译为构造参数。

**标签 constructor-arg** 的属性

*   **name 属性：**对应的值为**构造函数中方法形参的参数名**，不是私有成员变量名，必须要保持一致。
    
*   **ref 属性：**指向的是 spring 的 IOC 容器中其他 bean 对象的 id。
    

**步骤 3：运行程序**

运行 AppForDIConstructor 类，查看结果，说明 bookDao 已经成功注入。

![](<assets/1740015639347.png>)

#### 5.2.3 构造器注入多个引用数据类型

需求: 在 BookServiceImpl 使用构造函数注入多个引用数据类型，比如 userDao

1. 声明 userDao 属性

2. 生成一个带有 bookDao 和 userDao 参数的构造函数

3. 在 applicationContext.xml 中配置注入

**步骤 1: 提供多个属性的构造函数**

在 BookServiceImpl 声明 userDao 并提供多个参数的构造函数

```
public class BookServiceImpl implements BookService{
    private BookDao bookDao;
    private UserDao userDao;
 
    public BookServiceImpl(BookDao bookDao,UserDao userDao) {
        this.bookDao = bookDao;
        this.userDao = userDao;
    }
 
    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
        userDao.save();
    }
}
```

**步骤 2: 配置文件中配置多参数注入**

在 applicationContext.xml 中配置注入

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
    <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <constructor-arg name="bookDao" ref="bookDao"/>
        <constructor-arg name="userDao" ref="userDao"/>
    </bean>
</beans>
```

**说明:** 这两个`<contructor-arg>`的配置顺序可以任意

**步骤 3: 运行程序**

运行 AppForDIConstructor 类，查看结果，说明 userDao 已经成功注入。

![](<assets/1740015639612.png>)

#### 5.2.4 构造器注入多个简单数据类型（constructor-arg 标签的 name 和 value）

需求: 在 BookDaoImpl 中，使用构造函数注入 databaseName 和 connectionNum 两个参数。

参考引用数据类型的注入，我们可以推出具体的步骤为:

1. 提供一个包含这两个参数的构造方法

2. 在 applicationContext.xml 中进行注入配置

**步骤 1: 添加多个简单属性并提供构造方法**

修改 BookDaoImpl 类，添加构造方法

```
public class BookDaoImpl implements BookDao {
    private String databaseName;
    private int connectionNum;
 
    public BookDaoImpl(String databaseName, int connectionNum) {
        this.databaseName = databaseName;
        this.connectionNum = connectionNum;
    }
 
    public void save() {
        System.out.println("book dao save ..."+databaseName+","+connectionNum);
    }
}
```

**步骤 2: 配置完成多个属性构造器注入**

在 applicationContext.xml 中进行注入配置，bean 标签内 constructor-arg 标签的属性 name 和 value

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
        <constructor-arg name="databaseName" value="mysql"/>
        <constructor-arg name="connectionNum" value="666"/>
    </bean>
    <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <constructor-arg name="bookDao" ref="bookDao"/>
        <constructor-arg name="userDao" ref="userDao"/>
    </bean>
</beans>
```

**说明:** 这两个`<contructor-arg>`的配置顺序可以任意

**步骤 3: 运行程序**

运行 AppForDIConstructor 类，查看结果

![](<assets/1740015639970.png>)

**存在问题: name 属性的紧耦合**

![](<assets/1740015640290.png>)

*   当构造函数中方法的参数名发生变化后，配置文件中的 name 属性也需要跟着变
*   这两块存在**紧耦合**，具体该如何解决?

**解决方案（有缺点，了解即可）：**

**了解即可。**参数名发生变化的情况并不多，并且解决方案有缺点类型限制或顺序限制。

**方式一:** 删除 name 属性，添加 type 属性，**按照类型注入**

```
<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
    <constructor-arg type="int" value="10"/>
    <constructor-arg type="java.lang.String" value="mysql"/>
</bean>
```

*   这种方式可以解决构造函数形参名发生变化带来的耦合问题
*   但是如果构造方法参数中**有类型相同的参数**，这种方式就**不太好实现**了

**方式二:** 删除 type 属性，添加 index 属性，**按照索引下标注入**，下标从 0 开始

```
<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
    <constructor-arg index="1" value="100"/>
    <constructor-arg index="0" value="mysql"/>
</bean>
```

*   这种方式可以解决参数类型重复问题
*   但是如果构造方法参**数顺序发生变化后**，这种方式又带来了**耦合问题**

### **5.2.5 依赖注入方式选择**

1.  **强制依赖**使用**构造器**进行，使用 **setter 注入有概率不进行注入导致 null 对象**出现。**强制依赖**指对象在创建的过程中必须要注入指定的参数。
2.  **可选依赖**使用 **setter** 注入进行，灵活性强。**可选依赖**指对象在创建过程中注入的参数可有可无
3.  **Spring 框架倡导使用构造器**，第三方框架内部大多数采用构造器注入的形式进行数据初始化，相对严谨
4.  如果有必要**可以两者同时使用**，使用构造器注入完成强制依赖的注入，使用 setter 注入完成可选依赖的注入
5.  实际开发过程中还要根据实际情况分析，如果受控对象没有提供 setter 方法就必须使用构造器注入
6.  ****自己开发的模块推荐优先使用 setter 注入****

**总结：**建议用 setter 注入。 

这节中主要讲解的是 Spring 的依赖注入的实现方式:

*   setter 注入
    
    *   简单数据类型
        
        ```
        <bean ...>
            <property name="" value=""/>
        </bean>
        ```
        
    *   引用数据类型
        
        ```
        <bean ...>
            <property name="" ref=""/>
        </bean>
        ```
        
*   构造器注入
    
    *   简单数据类型
        
        ```
        <bean ...>
            <constructor-arg name="" index="" type="" value=""/>
        </bean>
        ```
        
    *   引用数据类型
        
        ```
        <bean ...>
            <constructor-arg name="" index="" type="" ref=""/>
        </bean>
        ```
        
*   依赖注入的方式选择上
    
    *   建议使用 setter 注入
    *   第三方技术根据情况选择

### 5.3 自动配置依赖注入

前面花了大量的时间把 Spring 的注入去学习了下，很**麻烦**，可以使用**自动配置**优化。

#### 5.3.1 什么是依赖自动装配?

*   **IoC 容器**根据 bean 所依赖的资源在容器中**自动查找并注入**到 bean 中的过程称为自动装配

#### 5.3.2 自动装配方式有哪些?

*   **按类型（常用）**
*   按名称
*   按构造方法
*   不启用自动装配

#### 5.3.3 准备下案例环境

*   创建一个 Maven 项目
*   pom.xml 添加依赖
*   resources 下添加 spring 的配置文件

这些步骤和前面的都一致，大家可以快速的拷贝即可，最终项目的结构如下:

![](<assets/1740015640514.png>)

(1) 项目中添加 BookDao、BookDaoImpl、BookService 和 BookServiceImpl 类

```
public interface BookDao {
    public void save();
}
 
public class BookDaoImpl implements BookDao {
 
    private String databaseName;
    private int connectionNum;
 
    public void save() {
        System.out.println("book dao save ...");
    }
}
public interface BookService {
    public void save();
}
 
public class BookServiceImpl implements BookService{
    private BookDao bookDao;
 
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
 
    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```

(2)resources 下提供 spring 的配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <property name="bookDao" ref="bookDao"/>
    </bean>
</beans>
```

(3) 编写 AppForAutoware 运行类，加载 Spring 的 IOC 容器，并从中获取对应的 bean 对象

```
public class AppForAutoware {
    public static void main( String[] args ) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        BookService bookService = (BookService) ctx.getBean("bookService");
        bookService.save();
    }
}
```

#### 5.3.4 完成自动装配的配置

**自动装配只需要修改 applicationContext.xml 配置文件即可:**

(1) 将 dao 的 id 删除，service 的 bean 里`<property>`标签删除

(2) 在 service 的`<bean>`标签中添加 autowire 属性，autowire 就译为自动装配

（3）要确保自动装配的类里的有 setter 方法。

**按照类型注入的配置**

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <bean class="com.itheima.dao.impl.BookDaoImpl"/>
    <!--autowire属性：开启自动装配，通常使用按类型装配-->
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl" autowire="byType"/>
 
</beans>
```

**注意事项:**

*   需要注入属性的类中对应属性的 **setter 方法不能省略**
*   被注入的对象必须要被 Spring 的 IOC 容器管理
*   按类型注入必须保障**容器中相同类型的 bean 唯一，否则会报`NoUniqueBeanDefinitionException`**

**容器中相同类型的 bean 不唯一的情况：**

![](<assets/1740015640721.png>)

**按照名称注入：** 

一个类型在 IOC 中有多个对象，还想要注入成功，这个时候就需要**按照名称注入**，配置方式为:

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <bean class="com.itheima.dao.impl.BookDaoImpl"/>
    <!--autowire属性：开启自动装配，通常使用按类型装配-->
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl" autowire="byName"/>
 
</beans>
```

**注意事项:**

*   **按照名称注入中的名称指的是什么?**
    
    ![](<assets/1740015640927.png>)
    
    *   bookDao 是 private 修饰的，外部类无法直接方法
    *   外部类只能通过属性的 **set 方法**进行访问
    *   对外部类来说，setBookDao 方法名，去掉 set 后首字母小写是其属性名
        *   为什么是去掉 set 首字母小写?
        *   这个规则是 set 方法生成的默认规则，set 方法的生成是把属性名首字母大写前面加 set 形成的方法名
    *   所以按**照名称注入，其实是和对应的 set 方法有关**，但是如果按照标准起名称，属性名和 set 对应的名是一致的
*   如果按照名称去找对应的 bean 对象，找不到则注入 Null
    
*   **当某一个类型在 IOC 容器中有多个对象，按照名称注入只找其指定名称对应的 bean 对象，不会报错**
    

两种方式介绍完后，以后用的更多的是**按照类型**注入。

自动配置依赖注入的**特点:**

1.  **自动装配**用于引用类型依赖注入，**不能对简单类型进行操作**
2.  使用**按类型装配**时（byType）必须保障**容器中相同类型的 bean 唯一**，推荐使用
3.  使用按名称装配时（byName）必须保障容器中具有指定名称的 bean，因变量名与配置耦合，不推荐使用
4.  自动装配优先级低于 setter 注入与构造器注入，同时出现时自动装配配置失效

### 5.4 数组、集合注入

集合中既可以装简单数据类型也可以装引用数据类型。先来回顾下，常见的集合类型有哪些?

*   List
*   Set
*   Map
*   Properties

针对不同的集合类型，该如何实现注入呢?

#### 5.4.1 环境准备

*   创建一个 Maven 项目
*   pom.xml 添加依赖
*   resources 下添加 spring 的配置文件 applicationContext.xml

这些步骤和**前面的都一致**，大家可以快速的拷贝即可，最终项目的结构如下:

![](<assets/1740015641175.png>)

(1) 项目中添加添加 BookDao、BookDaoImpl 类

```
public interface BookDao {
    public void save();
}
 
 
public class BookDaoImpl implements BookDao {
 
    private int[] array;
 
    private List<String> list;
 
    private Set<String> set;
 
    private Map<String,String> map;
 
    private Properties properties;
 
     public void save() {
        System.out.println("book dao save ...");
 
        System.out.println("遍历数组:" + Arrays.toString(array));
 
        System.out.println("遍历List" + list);
 
        System.out.println("遍历Set" + set);
 
        System.out.println("遍历Map" + map);
 
        System.out.println("遍历Properties" + properties);
    }
    //setter....方法省略，自己使用工具生成
}
```

(2)resources 下提供 spring 的配置文件，applicationContext.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
</beans>
```

(3) 编写 AppForDICollection 运行类，加载 Spring 的 IOC 容器，并从中获取对应的 bean 对象

```
public class AppForDICollection {
    public static void main( String[] args ) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
        bookDao.save();
    }
}
```

接下来，在上面这个环境中来完成`集合注入`的学习:

下面的所以配置方式，都是在 bookDao 的 bean 标签中使用进行注入

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
 
    </bean>
</beans>
```

#### 5.4.2 注入数组类型数据

```
<property name="array">
    <array>
        <value>100</value>
        <value>200</value>
        <value>300</value>
    </array>
</property>
```

#### 5.4.3 注入 List 类型数据

```
<property name="list">
    <list>
        <value>itcast</value>
        <value>itheima</value>
        <value>boxuegu</value>
        <value>chuanzhihui</value>
    </list>
</property>
```

#### 5.4.4 注入 Set 类型数据

```
<property name="set">
    <set>
        <value>itcast</value>
        <value>itheima</value>
        <value>boxuegu</value>
        <value>boxuegu</value>
    </set>
</property>
```

#### 5.4.5 注入 Map 类型数据

注意 map 最内标签不是 value，是 entry 键值对。 

```
<property name="map">
    <map>
        <entry key="country" value="china"/>
        <entry key="province" value="henan"/>
        <entry key="city" value="kaifeng"/>
    </map>
</property>
```

#### 5.4.6 注入 Properties 类型数据

```
<property name="properties">
    <props>
        <prop key="country">china</prop>
        <prop key="province">henan</prop>
        <prop key="city">kaifeng</prop>
    </props>
</property>
```

配置完成后，运行下看结果:

![](<assets/1740015641411.png>)

**说明：**

*   **property 标签表示 setter 方式注入**，**构造方式注入** constructor-arg 标签内部**也可以写**`<array>`、`<list>`、`<set>`、`<map>`、`<props>`标签
*   List 的底层也是通过数组实现的，所以`<list>`和`<array>`标签是可以混用
*   集合中要添加引用类型，只需要把`<value>`标签改成`<ref>`标签，这种方式用的比较少