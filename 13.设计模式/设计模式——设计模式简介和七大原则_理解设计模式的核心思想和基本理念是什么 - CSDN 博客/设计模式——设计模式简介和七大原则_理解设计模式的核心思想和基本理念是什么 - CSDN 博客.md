---
url: https://blog.csdn.net/qq_40991313/article/details/130403757?spm=1001.2014.3001.5502
title: 设计模式——设计模式简介和七大原则_理解设计模式的核心思想和基本理念是什么 - CSDN 博客
date: 2025-02-20 13:54:37
tag: 
summary: 
---
 **导航：** 

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?spm=1001.2014.3001.5501 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[一、通过经典面试题掌握重点](#t0)

[二、设计模式的目的和核心原则](#t1)

[三、设计模式七大原则](#t2)

[3.1 单一职责原则（Single Responsibility Principle）](#t3)

[3.2 接口隔离原则（Interface Segregation Principle）](#t4)

[3.3 依赖倒转原则（Dependence Inversion Principle）](#t5)

[3.3.1 介绍](#t6)

[3.3.2 依赖关系传递的三种方式](#t7)

[3.4 里氏替换原则（Liskov Substitution Principle）](#t8)

[3.5 开闭原则（Open Closed Principle）](#t9)

[3.6 迪米特法则（Demeter Principle）](#t10)

[3.7 合成复用原则（Composite Reuse Principle）](#t11)

[3.8 其他原则](#t12)

[3.8.1 DRY 原则](#t13)

[四、UML 类图：统一建模语言](#t14)

[4.1、类图——依赖（dependence）](#t15)

[4.2、类图——泛化（Generalization）](#t16)

[4.3、类图——实现（Implementation）](#t17)

[4.4、类图——关联（Association）](#t18)

[4.5、类图——聚合（Aggregation）](#t19)

[4.6、类图——组合（Composition）](#t20)

## 一、通过经典面试题掌握重点

**原型设计模式** 

*   1）有请使用 UML 类图画出原型模式核心角色

*   2）原型设计模式的**深拷贝和浅拷贝**是什么，并写出深拷贝的两种方式的源码（重写 clone 方法实现深拷贝、使用序列化来实现深拷贝）

*   3）在 Spring 框架中哪里使用到原型模式，并对源码进行分析

```
public class SingleResponsibility1 {
    public static void main(String[] args) {
        Vehicle vehicle = new Vehicle();
        vehicle.run("汽车");
        vehicle.run("轮船");
        vehicle.run("飞机");
    }
}
 
/**
 * 方式1的分析
 * 1.在方式1的run方法中，违反了单一职责原则
 * 2.解决的方案非常的简单，根据交通工具运行方法不同，分解成不同类即可
 */
class Vehicle{
    public void run(String type){
        if ("汽车".equals(type)) {
            System.out.println(type + "在公路上运行...");
        } else if ("轮船".equals(type)) {
            System.out.println(type + "在水面上运行...");
        } else if ("飞机".equals(type)) {
            System.out.println(type + "在天空上运行...");
        }
    }
}
```

*   4）Spring 中原型 bean 的创建，就是原型模式的应用

*   5）代码分析 + Debug 源码

**设计模式七大原则**

*   1）单一职责原则

*   2）接口隔离原则

*   3）依赖倒转原则

*   4）里氏替换原则

*   5）开闭原则（OCP）

*   6）迪米特法则

*   7）合成复用原则

**要求：**

*   1）七大设计原则核心思想

*   2）能够以类图的说明设计原则

*   3）在项目实际开发中，你在哪里使用到了 OCP 原则

**状态模式**

金融借贷平台项目：借贷平台的订单，有审核 - 发布 - 抢单等等步骤，随着操作的不同，会改变订单的状态，项目中的这个模块实现就会使用到状态模式，请你使用状态模式进行设计，并完成实际代码

问题分析：这类代码难以应对变化，在添加一种状态时，我们需要手动添加 if/else ，在添加一种功能时，要对所有的状态进行判断。因此代码会变得越来越臃肿，并且一旦没有处理某个状态，便会发生极其严重的 BUG，难以维护

**解释器设计模式**

*   1）介绍解释器设计模式是什么？

*   2）画出解释器设计模式的 UML 类图，分析设计模式中的各个角色是什么？

*   3）请说明 Spring 的框架中，哪里使用到了解释器设计模式，并做源码级别的分析

**单例设计模式**

单例设计模式一共有几种实现方式？请分别用代码实现，并说明各个实现方式的优点和实点？

*   1）饿汉式 两种

*   2）懒汉式 三种

*   3）双重检查

*   4）静态内部类

*   5）枚举

## 二、设计模式的目的和核心原则

**重要性：** 

1.  设计模式是软件设计中普遍存在问题的解决方案。
2.  便于项目开发完成后维护项目（可读性、规范性）、新增功能。
3.  你在实际项目中使用过什么设计模式，怎么使用的？解决了什么问题？
4.  项目的功能模块和框架里会使用设计模式。

**设计模式目的：**

编写软件过程中，程序员面临着来自耦合性，内聚性以及可维护性，可扩展性，重用性，灵活性等多方面的挑战。

设计模式是为了让程序（软件），具有更好的

*   **1）可复用性**（即：相同功能的代码，不用多次编写，也叫做代码重用性）

*   **2）可读性**（即：编程规范性，便于其他程序员的阅读和理解）

*   **3）可扩展性**（即：当需要增加新的功能时，非常的方便，也叫做可维护性）

*   **4）可靠性**（即：当我们增加新的功能后，对原来的功能没有影响）

*   5）使程序呈现**高内聚，低耦合**的特性

**设计原则核心思想**

*   1）找出应用中可能**需要变化之处，把它们独立出来**，不要和那些不需要变化的代码混在一起

*   2）针对**接口编程**，而不是针对实现编程

*   3）为了交互对象之间的**松耦合**设计而努力

设计模式包含了面向对象的精髓，“懂了设计模式，你就懂了面向对象分析和设计（OOA/D）的精要”

## 三、设计模式七大原则

常用的七大原则有：

*   1）单一职责原则

*   2）接口隔离原则

*   3）依赖倒转原则

*   4）里氏替换原则

*   5）开闭原则

*   6）迪米特法则

*   7）合成复用原则

### 3.1 单一职责原则（Single Responsibility Principle）

对类来说，即**一个类应该只负责一项职责**。

对接口来说，接口设计要符合单一职责原则，粒度越小通用性就越好。

例如 user 表只负责存储用户相关的信息。如类 A 负责两个不同职责：职责 1，职责 2。当职责 1 需求变更而改变 A 时，可能造成职责 2 执行错误，所以需要将类 A 的粒度分解为 A1，A2

**注意事项和细节**

*   1）降低类的复杂度，一个类只负责一项职责

*   2）提高类的可读性，可维护性

*   3）降低变更引起的风险

*   4）通常情况下，我们应当遵守单一职责原则，只有逻辑足够简单，才可以在代码级违反单一职责原则；**只有类中方法数量足够少**，**可以在方法级别保持单一职责原则**

**案例：**

方案 1（违反单一原则），方法内条件判断，区分情景：

```
public class SingleResponsibility2 {
    public static void main(String[] args) {
        RoadVehicle roadVehicle = new RoadVehicle();
        roadVehicle.run("汽车");
        WaterVehicle waterVehicle = new WaterVehicle();
        waterVehicle.run("轮船");
        AirVehicle airVehicle = new AirVehicle();
        airVehicle.run("飞机");
    }
}
 
/**
 * 方案2的分析
 * 1.遵守单一职责原则
 * 2.但是这样做的改动很大，即将类分解，同时修改客户端
 * 3.改进：直接修改Vehicle类，改动的代码会比较少=>方案3
 */
class RoadVehicle{
    public void run(String type){
        System.out.println(type + "在公路上运行...");
    }
}
class WaterVehicle{
    public void run(String type){
        System.out.println(type + "在水面上运行...");
    }
}
class AirVehicle{
    public void run(String type){
        System.out.println(type + "在天空上运行...");
    }
}
```

**方案 2（单一职责）：**不同类，区分情景：

```
public class SingleResponsibility3 {
    public static void main(String[] args) {
        Vehicle2 vehicle = new Vehicle2();
        vehicle.run("汽车");
        vehicle.runWater("轮船");
        vehicle.runAir("飞机");
    }
}
 
/**
 * 方式3的分析
 * 1.这种修改方法没有对原来的类做大的修改，只是增加方法
 * 2.这里虽然没有在类这个级别上遵守单一职责原则，但是在方法级别上，仍然是遵守单一职责
 */
class Vehicle2{
    public void run(String type){
        System.out.println(type + "在公路上运行...");
    }
    public void runWater(String type){
        System.out.println(type + "在水面上运行...");
    }
    public void runAir(String type){
        System.out.println(type + "在天空上运行...");
    }
}
```

**方案 3（方法级别单一职责）：不同方法，区分情景：**

```
interface Interface1 {
    void operation1();
 
    void operation2();
 
    void operation3();
 
    void operation4();
 
    void operation5();
}
 
class B implements Interface1 {
 
    @Override
    public void operation1() {
        System.out.println("B 实现了 operation1");
    }
 
    @Override
    public void operation2() {
        System.out.println("B 实现了 operation2");
    }
 
    @Override
    public void operation3() {
        System.out.println("B 实现了 operation3");
    }
 
    @Override
    public void operation4() {
        System.out.println("B 实现了 operation4");
    }
 
    @Override
    public void operation5() {
        System.out.println("B 实现了 operation5");
    }
}
 
class D implements Interface1 {
 
    @Override
    public void operation1() {
        System.out.println("D 实现了 operation1");
    }
 
    @Override
    public void operation2() {
        System.out.println("D 实现了 operation2");
    }
 
    @Override
    public void operation3() {
        System.out.println("D 实现了 operation3");
    }
 
    @Override
    public void operation4() {
        System.out.println("D 实现了 operation4");
    }
 
    @Override
    public void operation5() {
        System.out.println("D 实现了 operation5");
    }
}
 
/**
 * A类通过接口Interface1依赖（使用）B类，但是只会用到1，2，3方法
 */
class A {
    public void depend1(Interface1 i) {
        i.operation1();
    }
 
    public void depend2(Interface1 i) {
        i.operation2();
    }
 
    public void depend3(Interface1 i) {
        i.operation3();
    }
}
 
/**
 * C类通过接口Interface1依赖（使用）D类，但是只会用到1，4，5方法
 */
class C {
    public void depend1(Interface1 i) {
        i.operation1();
    }
 
    public void depend4(Interface1 i) {
        i.operation4();
    }
 
    public void depend5(Interface1 i) {
        i.operation5();
    }
}
```

### 3.2 接口隔离原则（Interface Segregation Principle）

**客户端不应该依赖它不需要的接口**，即一个类对另一个类的依赖应该建立在最小的接口上

接口隔离原则（Interface Segregation Principle，ISP）是 SOLID 中的一个设计原则，它定义为 “客户端应该不被迫依赖于它不使用的方法”，即一个类不应该强制依赖它不需要的接口。

接口隔离原则的主要目标是**将庞大而臃肿的接口拆分成更小、更具体的接口**，以方便客户端根据需求选择其所需的特定接口。这样可以大幅度减少客户端对于不必要的接口的依赖，使系统更加灵活、可维护和易于扩展。

一个典型的例子是，当我们需要使用一个接口时，通常是需要实现该**接口**的所有方法，但是实际上可能**只需要用到部分方法**。如果这个接口包含很多方法，就会造成实现类的代码冗余和依赖性过强。

通过**合理的接口拆分和组合**，可以使得接口更加精简，提高代码的**复用性和可拓展性**。同时，也有利于提高代码的**可维护性**，降低代码修改时的风险和维护成本。

需要注意的是，接口是一种描述行为的抽象，而隔离的目的是为了让接口更好地描述抽象行为，而不是让接口的数量变得多而复杂。因此，我们需要在接口隔离时保持适度，并根据具体情况进行选择和拆分。

不符合接口隔离原则的案例：

![](<assets/1740030878118.png>)

*   类 A 通过接口 Interface1 依赖类 B，类 C 通过接口 Interface1 依赖类 D，如果接口 Interface1 对于类 A 和类 C 来说不是最小接口（类只会用到接口里的部分方法，另一部分方法不需要还得实现），那么类 B 和类 D 必须去实现他们不需要的方法
*   **按隔离原则应当这样处理：**将接口 Interface1 **拆分**为独立的几个接口，类 A 和类 C 分别与他们需要的接口建立依赖关系。也就是采用接口隔离原则

![](<assets/1740030878354.png>)

**违法隔离的代码：** 

```
interface Interface1 {
    void operation1();
}
 
interface Interface2 {
    void operation2();
 
    void operation3();
}
 
interface Interface3 {
    void operation4();
 
    void operation5();
}
 
class B implements Interface1, Interface2 {
 
    @Override
    public void operation1() {
        System.out.println("B 实现了 operation1");
    }
 
    @Override
    public void operation2() {
        System.out.println("B 实现了 operation2");
    }
 
    @Override
    public void operation3() {
        System.out.println("B 实现了 operation3");
    }
}
 
class D implements Interface1, Interface3 {
 
    @Override
    public void operation1() {
        System.out.println("D 实现了 operation1");
    }
 
    @Override
    public void operation4() {
        System.out.println("D 实现了 operation4");
    }
 
    @Override
    public void operation5() {
        System.out.println("D 实现了 operation5");
    }
}
 
/**
 * A类通过接口Interface1,Interface2依赖（使用）B类，但是只会用到1，2，3方法
 */
class A {
    public void depend1(Interface1 i) {
        i.operation1();
    }
 
    public void depend2(Interface2 i) {
        i.operation2();
    }
 
    public void depend3(Interface2 i) {
        i.operation3();
    }
}
 
/**
 * C类通过接口Interface1,Interface3依赖（使用）D类，但是只会用到1，4，5方法
 */
class C {
    public void depend1(Interface1 i) {
        i.operation1();
    }
 
    public void depend4(Interface3 i) {
        i.operation4();
    }
 
    public void depend5(Interface3 i) {
        i.operation5();
    }
}
```

**拆分接口后的代码：**

```
/**
 * 方式1分析
 * 1.简单，比较容易想到
 * 2.如果我们获取的对象是微信，短信等等，则新增类，同时 Peron也要增加相应的接收方法
 * 3.解决思路：
 * 引入一个抽象的接口IReceiver，表示接收者，这样Person类与接口IReceiver发生依赖
 * 因为Email，Weixin等等属于接收的范围，他们各自实现IReceiver接口就ok，这样我们就符号依赖倒转原则
 */
class Email {
    public String getInfo() {
        return "电子邮件信息：Hello World！";
    }
}
 
class Person {
    public void receive(Email email) {
        System.out.println(email.getInfo());
    }
}
```

### 3.3 依赖倒转原则（Dependence Inversion Principle）

#### 3.3.1 介绍

*   **1）高层模块不应该依赖低层模块，二者都应该依赖其抽象**（接口或抽象类）

*   2）抽象不应该依赖细节，细节应该依赖抽象

*   3）依赖倒转（倒置）的中心思想是**面向接口编程**

*   4）依赖倒转原则是基于这样的设计理念：相对于细节的多变性，**抽象的东西要稳定的多**。以抽象为基础搭建的架构比以细节为基础的架构要稳定的多。在 java 中，抽象指的是接口或抽象类，细节就是具体的实现类

*   5）使用接口或抽象类的目的是制定好规范，而不涉及任何具体的操作，把展现细节的任务交给他们的实现类去完成
*   **多态**是实现依赖倒转原则的方法之一。

**案例：**用户类接收邮件、微信等信息。

违反依赖倒转：引用具体而非抽象：

```
interface IReceiver {
    String getInfo();
}
 
class Email implements IReceiver {
    @Override
    public String getInfo() {
        return "电子邮件信息：Hello World！";
    }
}
 
class Weixin implements IReceiver {
    @Override
    public String getInfo() {
        return "微信消息：Hello World！";
    }
}
 
class ShortMessage implements IReceiver {
    @Override
    public String getInfo() {
        return "短信信息：Hello World！";
    }
}
 
class Person {
    public void receive(IReceiver receiver) {
        System.out.println(receiver.getInfo());
    }
}
```

**改进：多态的方式引用抽象：**

```
//方式1：通过接口传递实现依赖
//开关的接口
interface IOpenAndClose {
    void open(ITV tv);//抽象方法，接收接口
}
//ITV接口
interface ITV {
    void play();
}
//实现接口
class OpenAndClose implements IOpenAndClose {
    public void open(ITV tv){
        tv.play();
    }
}
```

#### 3.3.2 依赖关系传递的三种方式

*   **1）低层模块尽量都要有抽象类或接口，**或者两者都有，程序稳定性更好

*   **2）变量的声明类型尽量是抽象类或接口**，这样我们的变量引用和实际对象间，就存在一个缓冲层，利于程序扩展和优化

*   3）继承时遵循里氏替换原则

**开关电视的案例：** 

**1）接口传递**

 ITV 接口是 IOpenAndClose 接口的普通方法参数

```
//方式2：通过构造函数实现依赖
//开关的接口
interface IOpenAndClose {
    void open();//抽象方法
}
//ITV接口
interface ITV {
    public void play();
}
//实现接口
class OpenAndClose implements IOpenAndClose {
    private ITV tv; // 成员
    
    public OpenAndClose(ITV tv){ // 构造器
        this.tv = tv;
    }
    
    public void open(){
        this.tv.play();
    }
}
```

**2）构造方法传递**

 ITV 接口是 OpenAndClose 类的成员变量和构造方法参数 

```
//方式3，通过setter方法传递
interface IOpenAndClose{
    void open();//抽象方法
    void setTv(ITV tv);
}
//ITV接口
interface ITV{
    void play();
}
//实现接口
class OpenAndClose implements IOpenAndClose{
    private ITV tv;
    public void setTv(ITV tv){
        this.tv=tv;
    }
    public void open(){
        this.tv.play();
    }
}
```

**3）setter 方式传递**

 ITV 接口是 OpenAndClose 类的成员变量和 setter 方法参数  

```
public void test() {
    A a = new A();
    System.out.println("11-3=" + a.func1(11, 3));
    System.out.println("1-8=" + a.func1(1, 8));
    System.out.println("---------------------");
 
    B b = new B();
    System.out.println("11-3=" + b.func1(11, 3));
    System.out.println("1-8=" + b.func1(1, 8));
    System.out.println("11+3+9=" + b.func2(11, 3));
}
 
class A {
    //返回两个数的差
    public int func1(int num1, int num2) {
        return num1 - num2;
    }
}
 
class B extends A {
    @Override
    public int func1(int num1, int num2) {
        return num1 + num2;
    }
 
    //增加了一个新功能：完成两个数相加，然后和9求和
    public int func2(int num1, int num2) {
        return func1(num1, num2) + 9;
    }
}
```

### 3.4 里氏替换原则（Liskov Substitution Principle）

**面对对象 OO 中继承性的思考和说明**

*   1）继承包含这样一层含义：父类中凡是已经实现好的方法，实际上是在设定规范和契约，虽然它不强制要求所有的子类必须遵循这些契约，但是如果**子类对这些已经实现的方法任意修改，就会对整个继承体系造成破坏**

*   2）继承在给程序设计带来便利的同时，也带来了弊端。比如使用继承会给程序带来**侵入性**，程序的可移植性降低，增加对象间的**耦合性**，如果一个类被其他的类所继承，则当这个类需要修改时，必须考虑到所有的子类，并且父类修改后，所有涉及到子类的功能都有可能产生故障

*   3）问题提出：在编程中，如何正确使用继承？=> 里氏替换原则

**基本介绍**

*   1）里氏替换原则在 1988 年，由麻省理工学院的以为姓里的女士提出的

*   2）**父类型对象替换成子类型对象后功能未变：**如果对每个类型为 T1 的对象 o1，都有类型为 T2 的对象 o2，使得以 T1 定义的所有程序 P 在所有的对象 o1 都代换成 o2 时，程序 P 的行为没有发生变化，那么类型 T2 是类型 T1 的子类型。换句话说，所有引用基类的地方必须能透明地使用其子类的对象

*   3）在使用继承时，遵循里氏替换原则，在**子类中尽量不要重写父类的方法**

*   4）里氏替换原则告诉我们，继承实际上让两个类耦合性增强了，在适当的情况下，可以通过**聚合、组合、依赖**来解决问题

**案例：**

**传统方案：**子类把父类的减方法重写为加方法：整个继承体系的复用性会比较差。

```
//创建一个更加基础的基类
class Base {
    //将更基础的成员和方法写到Base类中
}
 
class A extends Base {
    //返回两个数的差
    public int func1(int num1, int num2) {
        return num1 - num2;
    }
}
 
class B extends Base {
    //如果B需要使用A类的方法，使用组合关系
    private A a;
 
    public int func1(int num1, int num2) {
        return num1 + num2;
    }
 
    //增加了一个新功能：完成两个数相加，然后和9求和
    public int func2(int num1, int num2) {
        return func1(num1, num2) + 9;
    }
 
    public int func3(int num1, int num2) {
        return this.a.func1(num1, num2);
    }
}
```

**改进方案：**原来的父类和子类都继承一个更通俗的基类，原有的继承关系去掉，采用依赖、聚合、组合等关系代替

![](<assets/1740030878651.png>)

```
class GraphicEditor {
    public void drawShape(Shape s) {
        if (s.m_type == 1) {
            drawRectangle(s);
        } else if (s.m_type == 2) {
            drawCircle(s);
 
        } else if (s.m_type == 3) {
            drawTriangle(s);
        }
    }
 
    public void drawRectangle(Shape r) {
        System.out.println("矩形");
    }
 
    public void drawCircle(Shape r) {
        System.out.println("圆形");
    }
 
    public void drawTriangle(Shape r) {
        System.out.println("三角形");
    }
}
 
class Shape {
    public int m_type;
}
 
class RectangleShape extends Shape {
    RectangleShape() {
        m_type = 1;
    }
}
 
class CircleShape extends Shape {
    CircleShape() {
        m_type = 2;
    }
}
 
class TriangleShape extends Shape {
    TriangleShape() {
        m_type = 3;
    }
}
```

### 3.5 开闭原则（Open Closed Principle）

**开：**对扩展开放。

**闭：** 对修改关闭。

*   1）开闭原则是编程中**最基础、最重要**的设计原则

*   2）一个软件实体如类、模块和函数应该**对扩展开放**（对提供者而言），**对修改关闭**（对使用者而言）。**用抽象构建框架，用实现扩展细节**。增加了新功能后，原来使用的代码并没有做更改。

*   3）当软件需要变化时，尽量通过**扩展**软件实体的行为来实现变化，而**不是通过修改**已有的代码来实现变化。

*   4）编程中遵循其它原则，以及使用设计模式的目的就是遵循开闭原则

**案例：**

**方案一：传统方案**

一个画图形的功能，类图设计如下：

![](<assets/1740030878825.png>)

```
class GraphicEditor {
    public void drawShape(Shape s) {
        s.draw();
    }
}
 
abstract class Shape {
    int m_type;
 
    public abstract void draw();
}
 
class RectangleShape extends Shape {
    RectangleShape() {
        m_type = 1;
    }
 
    @Override
    public void draw() {
        System.out.println("矩形");
    }
}
 
class CircleShape extends Shape {
    CircleShape() {
        m_type = 2;
    }
 
    @Override
    public void draw() {
        System.out.println("圆形");
    }
}
 
class TriangleShape extends Shape {
    TriangleShape() {
        m_type = 3;
    }
 
    @Override
    public void draw() {
        System.out.println("三角形");
    }
}
```

**方式 1 的优缺点**

*   1）优点是比较好理解，简单易操作

*   2）缺点是**违反了**设计模式的 OCP 原则，即**对扩展开放**（提供方），**对修改关闭**（使用方）。即当我们给类增加新功能的时喉，尽量不修改代码，或者尽可能少修改代码

*   3）比如我们这时要新增加一个图形种类，我们需要做如下修改，修改的地方较多 4）代码演示

**方式 1 的改进的思路分析**

把创建 Shape 类做成**抽象类**，并提供一个**抽象的 draw 方法**，让子类去实现即可

这样我们有新的图形种类时，只需要让新的图形类继承 Shape，并**实现 draw 方法**即可

使用方的代码就不需要修改，满足了**开闭原则**

**方式 2 开闭原则：**画图功能设为基类的抽象方法，新增实现类只需要继承基类并实现画图的抽象方法即可。

```
//总部员工
class Employee {
    private String id;
 
    public String getId() {
        return id;
    }
 
    public void setId(String id) {
        this.id = id;
    }
}
 
//学院员工
class CollegeEmployee {
    private String id;
 
    public String getId() {
        return id;
    }
 
    public void setId(String id) {
        this.id = id;
    }
}
 
//学院员工管理 类
class CollegeManager {
    public List<CollegeEmployee> getAllEmployee() {
        List<CollegeEmployee> list = new ArrayList<>();    //是直接朋友
        CollegeEmployee collegeEmployee;    //是直接朋友
        for (int i = 0; i < 10; i++) {
            collegeEmployee = new CollegeEmployee();
            collegeEmployee.setId("学院员工id=" + i);
            list.add(collegeEmployee);
        }
        return list;
    }
}
 
//总部员工管理类
class SchoolManager {
    public List<Employee> getAllEmployee() {
//仅出现成员变量，方法参数，方法返回值中的类为直接的朋友
        List<Employee> list = new ArrayList<>();    //Employee 是直接朋友，出现在返回值
        Employee employee;    //是直接朋友
        for (int i = 0; i < 5; i++) {
            employee = new Employee();
            employee.setId("总部员工id=" + i);
            list.add(employee);
        }
        return list;
    }
 
    public void printAllEmployee(CollegeManager sub) {
        System.out.println("--------------学院员工--------------");
        List<CollegeEmployee> list1 = sub.getAllEmployee();    //不是直接朋友，出现在局部变量
        for (CollegeEmployee collegeEmployee : list1) {
            System.out.println(collegeEmployee.getId());
        }
        System.out.println("---------------总部员工-------------");
        List<Employee> list2 = this.getAllEmployee();
        for (Employee employee : list2) {
            System.out.println(employee.getId());
        }
    }
}
```

### 3.6 迪米特法则（Demeter Principle）

**基本介绍**

*   1）一个对象应该对其他对象保持最少的了解

*   2）类与类关系越密切，耦合度越大

*   3）迪米特法则又叫**最少知道原则**，即**一个类对自己依赖的类知道的越少越好。**也就是说，对于被依赖的类不管多么复杂，都尽量将逻辑封装在类的内部。对外除了提供的 public 方法，不对外泄露任何信息

*   4）迪米特法则还有个更简单的定义：**只与直接的朋友通信**

*   5）直接的朋友：每个对象都会与其他对象有耦合关系，只要两个对象之间有耦合关系，我们就说这两个对象之间是朋友关系。耦合的方式很多：依赖、关联、组合、聚合等。其中，我们称**出现成员变量，方法参数，方法返回值中的类为直接的朋友**，而**出现在局部变量中的类不是直接的朋友。**也就是说，陌生的类最好不要以局部变量的形式出现在类的内部。

**注意事项和细节**

*   主要 A 类里存在方法里 B 类是直接朋友，那么 A 类所有方法局部变量出现的 B 类都是直接朋友。
*   1）迪米特法则的核心是降低类之间的耦合

*   2）但是注意：由于每个类都减少了不必要的依赖，因此迪米特法则只是要求降低类间（对象间）耦合关系，并不是要求完全没有依赖关系

**案例**

**传统方案：**

1）有一个学校，下属有各个学院和总部，现要求打印出学校总部员工 ID 和学院员工的 id

```
//学院员工管理类
class CollegeManager {
    public List<CollegeEmployee> getAllEmployee() {
        List<CollegeEmployee> list = new ArrayList<>();    //是直接朋友
        CollegeEmployee collegeEmployee;    //是直接朋友
        for (int i = 0; i < 10; i++) {
            collegeEmployee = new CollegeEmployee();
            collegeEmployee.setId("学院员工id=" + i);
            list.add(collegeEmployee);
        }
        return list;
    }
//改进，新增方法，输出学院员工信息
    public void printEmployee(){
        System.out.println("--------------学院员工--------------");
 //CollegeEmployee是直接朋友，出现在上面getAllEmployee()方法的返回值
        List<CollegeEmployee> list1 = getAllEmployee();   
        for (CollegeEmployee collegeEmployee : list1) {
            System.out.println(collegeEmployee.getId());
        }        
    }    
}
//总部员工管理类
class SchoolManager {
    public List<Employee> getAllEmployee() {
//仅出现成员变量，方法参数，方法返回值中的类为直接的朋友
        List<Employee> list = new ArrayList<>();    //Employee 是直接朋友，出现在返回值
        Employee employee;    //是直接朋友
        for (int i = 0; i < 5; i++) {
            employee = new Employee();
            employee.setId("总部员工id=" + i);
            list.add(employee);
        }
        return list;
    }
 
    public void printAllEmployee(CollegeManager sub) {
        sub.printEmployee();    //改进，降低耦合，不用非直接朋友。
        System.out.println("---------------总部员工-------------");
        List<Employee> list2 = this.getAllEmployee();
        for (Employee employee : list2) {
            System.out.println(employee.getId());
        }
    }
}
```

**应用实例改进**

*   1）前面设计的问题在于 SchoolManager 中，CollegeEmployee 类并不是 SchoolManager 类的直接朋友（分析）

*   2）按照迪米特法则，应该避免类中出现这样非直接朋友关系的耦合

*   3）对代码按照迪米特法则进行改进，**将局部对象变量封装进参数里。**

```
// 逻辑重复：两个方法实现逻辑相似
public void addUserToDatabase(User user) {
    if (user != null && user.isValid()) {
        database.save(user);
    }
}
 
public void addAdminToDatabase(User admin) {
    if (admin != null && admin.isValid()) {
        database.save(admin);
    }
}
```

### 3.7 合成复用原则（Composite Reuse Principle）

**基本介绍**

原则是尽量使用**合成 / 聚合**的方式，而不是使用继承

也就是把需要用到的类作为本类的参数、成员变量、局部变量。

*   **依赖**（Dependency）**：**指的是**一个对象使用另一个对象**的情况。通常是在一个对象的方法中传入另一个对象作为参数，或者在方法中创建另一个对象的实例。依赖关系是一种 “短暂” 的引用关系，一旦不再需要依赖对象就可以释放掉。
    
*   **合成（**Composition**）：**指的是两个或多个对象之间一种包含与被包含的关系。其中**包含对象是整体，被包含对象是零部件**，它们的生命周期是一致的，无法单独存在。例如，一辆汽车是由发动机、车轮、底盘等组成的，这些组成部分与它们组合成的整体汽车具有相同的生命周期。当整体消亡时，所有零部件也随之消亡。
    
*   **聚合（**Aggregation**）：**指的是两个或多个对象之间一种包含与被包含的关系，**被包含对象可以存在于多个包含对象之间。**在聚合关系中，被包含对象可以独立于包含对象存在，生命周期也不一定相同。例如，大学是由系部、学院、图书馆等组成的，这些部分可以独立存在，而且它们也可以属于不同的大学。即使整个大学消亡，它们仍然可以存在。
    

总之，依赖、合成和聚合是面向对象编程中描述对象关系的重要概念，它们有助于设计和实现具有良好扩展性和可维护性的应用程序。

**案例**

B 类想用 A 类方法，如果直接继承，那么耦合性会提高，之后 A 类修改后 B 类也得跟着修改。

**解决办法：**

*   把 A 类作为 B 类普通方法的形参；
*   把 A 类作为 B 类成员变量，用 setter 方法
*   B 类的普通方法里创建 A 类的对象；

![](<assets/1740030879044.png>)

### 3.8 其他原则

#### 3.8.1 DRY 原则

**DRY 原则**（Don't Repeat Yourself）：即不要写重复的代码。

**代码重复的三种情况：**

*   **实现逻辑重复**：多段代码实现了相同的逻辑。例如有两个方法，虽然变量名和方法名不一样，但实际逻辑一模一样。
    
    ```
    // 使用两种方式计算同样的总和
    public int calculateTotal(int[] numbers) {
        int sum = 0;
        for (int number : numbers) {
            sum += number;
        }
        return sum;
    }
     
    public int calculateSum(int[] numbers) {
        return Arrays.stream(numbers).sum();
    }
    ```
    
*   **功能语意重复**：多段代码实现了相同的功能。例如有两个方法，一个是遍历集合，一个是 stream 流遍历集合，只是表现形式不一样，实际功能医院。
    
    ```
    public void processOrder() {
        log.info("Order started");
        // other order processing code
        log.info("Order started");
    }
    ```
    
*   **代码执行重复**：多处地方调用了相同的多段代码。例如完全不抽取方法，一个 service 方法几千行，里面很多重复的代码行没有抽取方法。 
    
    ```
    public class Person {
    	private IDCard card;
    }
    public class IDCard {}
    ```
    

**解决方案：**

*   **三层架构**：开发过程中，我们把后端服务器 Servlet 拆分成三层，分别是 web、service 和 dao，这也是程序员常提到的 **“Java 味”**
*   **模块化**：将项目按业务分成相互隔离的多模块，然后抽取出一个 common 模块让其他模块调用，降低耦合；
*   **满足单一职责**：即一个类应该只负责一项职责、一个接口只实现一个功能。
*   **封装继承多态**：抽取公用代码为新方法、使用继承的方式替代重复成员变量、方法的编写。
*   **模板等设计模式**：将通用逻辑抽取成新方法。

## 四、UML 类图：**统一建模语言**

*   1）UML—-Unified modeling language UML（**统一建模语言**），是一种用于软件系统分析和设计的语言工具，它用于帮助软件开发人员进行思考和记录思路的结果

*   2）UML 本身是**一套符号的规定**，就像数学符号和化学符号一样，这些符号用于描述软件模型中的各个元素和他们之间的关系，比如类、接口、实现、泛化、依赖、组合、聚合等
*   3）使用 UML 来建模，常用的工具有 Rational Rose，也可以使用一些插件来建模

**UML 类图**

*   1）用于描述系统中的类（对象）本身的组成和类（对象）之间的各种静态关系

*   2）类之间的关系：依赖、泛化（继承）、实现、关联、聚合与组合

![](<assets/1740030879324.png>)

### 4.1、类图——依赖（dependence）

只要是在类中用到了对方，那么他们之间就存在依赖关系。如果没有对方，连编译都通过不了

*   1）类中用到了对方

*   2）类的成员属性

*   3）方法的返回类型

*   4）方法接收的参数类型

*   5）方法中使用到 

A 用到 B，那么 A 依赖 B，即 A------->B：

![](<assets/1740030879588.png>)

### 4.2、类图——泛化（Generalization）

泛化关系实际上就是**继承**关系，它是依赖关系的特例

A 继承 B，那么 A➞B

![](<assets/1740030879826.png>)

### 4.3、类图——实现（Implementation）

实现关系实际上就是 A 类实现 B 类，它是依赖关系的特例 

A 实现 B，则 A➟ B

![](<assets/1740030880138.png>)

### 4.4、类图——关联（Association）

关联关系实际上就是**类与类之间的联系**，它是依赖关系的特例

*   关联具有导航性：即双向关系或单向关系

*   关系具有多重性：如 “1”（表示有且仅有一个），“0...”（表示 0 个或者多个），“0，1”（表示 0 个或者一个），“n...m”（表示 n 到 m 个都可以），“m...*”（表示至少 m 个）

![](<assets/1740030880428.png>)

单向一对一关系

```
public class Person {
	private IDCard card;
}
public class IDCard {
	private Person person;
}
```

**UML 类图**

![](<assets/1740030880638.png>)

双向一对一关系

```
public class Mouse {}
public class Monitor {}
public class Computer {
    private Mouse mouse;
    private Monitor monitor;
 
    public void setMouse(Mouse mouse) {
        this.mouse = mouse;
    }
 
    public void setMonitor(Monitor monitor) {
        this.monitor = monitor;
    }
}
```

**UML 类图**

![](<assets/1740030880865.png>)

### 4.5、类图——聚合（Aggregation）

聚合关系表示的是**整体和部分的关系**，**整体与部分可以分开**。聚合关系是关联关系的特例，所以它具有关联的导航性与多重性

B 是 A 的未实例化成员变量，则 B——◇A

如：一台电脑由键盘（keyboard）、显示器（monitor），鼠标等组成；组成电脑的各个配件是可以从电脑上分离出来的，使用带空心菱形的实线来表示：

![](<assets/1740030881156.png>)

```
public class Mouse {}
public class Monitor {}
public class Computer {
    private Mouse mouse = new Mouse();
    private Monitor monitor = new Monitor();
}
```

**UML 类图**

![](<assets/1740030881409.png>)

### 4.6、类图——组合（Composition）

组合关系也是**整体与部分的关系**，但是**整体与部分不可以分开**

B 是 A 的实例化成员变量，则 B<——◇A

如果我们认为 Mouse、Monitor 和 Computer 是不可分离的，则升级为组合关系

```
public class IDCard{}
public class Head{}
public class Person{
    private IDCard card;
    private Head head = new Head();
}
```

**UML 类图**

![](<assets/1740030881649.png>)

再看一个案例，在程序中我们定义实体：Person 与 IDCard、Head，那么 Head 和 Person 就是组合，IDCard 和 Person 就是聚合

```
public class IDCard{}
public class Head{}
public class Person{
    private IDCard card;
    private Head head = new Head();
}
```

**UML 类图**

![](<assets/1740030881807.png>)

但是如果在程序中，Person 实体中定义了对 IDCard 进行级联删除，即删除 Person 时连同 IDCard 一起删除，那么 IDCard 和 Person 就是组合了