---
url: https://blog.csdn.net/qq_40991313/article/details/130440147
title: Java 代理模式详解：静态、JDK 动态与 Cglib 代理 - CSDN 博客
date: 2025-02-20 13:54:43
tag: 
summary: 
---
 **导航：** 

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?spm=1001.2014.3001.5501 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1、代理模式的基本介绍](#t0)

[2、静态代理](#t1)

[3、JDK 动态代理](#t2)

[4、Cglib 代理](#t3)

[5、代理模式 的变体（应用场景）](#t4)

## 1、代理模式的基本介绍

代理模式：为一个对象提供一个**替身**，以控制对这个对象的访问。即**通过代理对象访问目标对象**

代理模式是结构型设计模式（用于描述对象之间的组合关系）。

**好处：**可以在目标对象实现的基础上，增强额外的功能操作，即**扩展目标对象的功能**

被代理的对象可以是**远程对象**、创建开销大的对象或需要**安全控制**的对象（代理时对目标对象进行安全控制）

我们并不希望客户端直接调用目标对象，而是通过代理对象对目标对象实现安全控制或增强功能。所以我们客户端直接依赖代理对象，代理对象依赖目标对象：

![](<assets/1740030883606.png>)

代理模式有不同的形式，主要有三种：**静态代理、JDK 动态代理、Cglib 代理**

**静态代理：**

目标对象与代理对象实现租同的接口或继承相同的父类，在编译时生成代理对象。

目标对象实现代理接口，代理对象实现并聚合代理接口，重写方法编写增强后逻辑。

**JDK 动态代理：**

通过 Java **反射机制**在运行时动态地在内存中生成代理对象。 目标对象需要实现代理接口。

目标对象实现代理接口，**代理工厂**通过 Proxy 类的静态方法 newProxyInstance()，利用反射机制返回代理对象实例。  

newProxyInstance() 三个参数：目标对象的类加载器、目标对象的接口、实现 InvocationHandler 接口并重写 invoke() 方法，编写代理对象逻辑。

应用：**Spring AOP** 采用了动态代理的方式，在运行时动态的创建代理对象来实现增强。

**Cglib 代理：**

在**内存**中构建一个**子类对象**从而实现对目标对象功能扩展。目标对象不需要实现代理接口。底层是通过使用 ASM 框架转换字节码并生成新的类。

代理工厂类实现 MethodInterceptor 接口并重写 intercept() 方法编写代理逻辑，通过 cglib 包的 Enhancer 类设置父类字节码文件和创建子类对象来返回代理对象实例。

ASM 框架是一个强大的 Java 字节码操作框架，可以让程序员通过代码生成和转换现有字节码来操作 Java 类。ASM 可以直接生成字节码，也可以通过访问现有字节码来修改它。

## 2、静态代理

静态代理在使里时，需要定义接口或者父类，**目标对象与代理对象实现租同的接口**或继承相同的**父类**。

**实现方式：**目标对象实现代理接口，代理对象实现并聚合代理接口，重写方法编写增强后逻辑。

**优缺点**

*   **优点**：在不修改目标对象的功能前提下，能通过代理对象对目标功能扩展

*   **缺点**：因为代理对象需要与目标对象实现一样的接口，所以会有很多代理类。**耦合性较高**，一旦**接口增加方法，目标对象与代理对象都要维护**

**案例**

*   1）定义一个**接口：ITeacherDao**

*   2）**目标对象 TeacherDao** 实现接口 ITeacherDao

*   3）使用静态代理方式，就需要在代理对象 TeacherDaoProxy 中也实现 ITeacherDao

*   4）调用的时候通过调用代理对象的方法来调用目标对象

*   5）**特别提醒**：代理对象与目标对象要实现相同的接口，然后通过调用相同的方法来调用目标对象的方法

**UML 类图**

![](<assets/1740030883806.png>)

  **核心代码**

```
/**
 * 代理接口
 */
public interface ITeacherDao {
    void teach();    //代理对象和原始对象重写这个方法
}
/**
 * 目标对象，即被代理对象。实现代理借口
 */
public class TeacherDao implements ITeacherDao {
    @Override
    public void teach() {
        System.out.println("老师授课中...");
    }
}
/**
 * 代理对象。实现代理借口
 */
public class TeacherDaoProxy implements ITeacherDao {
 
    private ITeacherDao iTeacherDao;    //接口引用
 
    public TeacherDaoProxy(ITeacherDao iTeacherDao) {
        this.iTeacherDao = iTeacherDao;
    }
 
    @Override
    public void teach() {
        System.out.println("准备授课...");
        iTeacherDao.teach();
        System.out.println("结束授课...");
    }
}
```

**客户端调用代理**

```
//创建被代理对象
TeacherDao teacherDao = new TeacherDao();
//创建代理对象，聚合被代理对象
TeacherDaoProxy teacherDaoProxy = new TeacherDaoProxy(teacherDao);
//通过代理对象，调用被代理对象的方法
teacherDaoProxy.teach();
```

## 3、JDK 动态代理

Java 中的动态代理是一种机制，它通过**在程序运行时动态地生成代理对象**，并在代理对象上进行方法调用，实现对目标对象方法的拦截与控制。动态代理是代理设计模式的一种实现方式，与静态代理不同的是，它**不需要显示地编写代理类**来代理被代理对象，而是通过 **Java 反射机制**在运行时动态生成代理类和代理对象。 

**实现方法：**

目标对象实现代理接口，**代理工厂**通过 Proxy 类的静态方法 newProxyInstance()，利用反射机制返回代理对象实例。  

newProxyInstance() 三个参数：目标对象的类加载器、目标对象的接口、实现 InvocationHandler 接口并重写 invoke() 方法，编写代理对象逻辑。

*   1）**代理对象不需要实现接口**，但是**目标对象要实现接口**，否则不能用动态代理

*   2）代理对象的生成，是利用 JDK 的 APl，动态的在**内存中构建代理对象**

*   3）动态代理也叫做：JDK 代理、接口代理

 JDK 提供了 java.lang.reflect.Proxy 类，可以通过它创建基于接口的动态代理对象。使用 Proxy 类的 newProxyInstance 静态方法，该方法需要接收三个参数：

```
//代理接口和目标对象同上，目标对象需要实现代理接口
// ITeacherDao与TeacherDao同上
/**
 * 代理工厂
 */
public class TeacherFactory {
    /**
     * 目标对象
     */
    private Object target;
 
    public TeacherFactory(Object target) {
        this.target = target;
    }
 
    public Object newProxyInstance() {
//1。ClassLoader loader : 指定当前目标对象使用的类加载器，获取加载器的方法固定
//2，Class<?>[] interfaces: 目标对象实现的接口类型，使用泛型方法确认类型
//3。InvocationHandler h : 事情处理，执行目标对象的方法时，会触发事情处理器方法，会把当前执行的目标对象方法作为参数传入
        return Proxy.newProxyInstance(target.getClass().getClassLoader(), 
target.getClass().getInterfaces(), new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println("JDK代理授课开始...");
                Object returnVal = method.invoke(target, args);
                System.out.println("JDK代理授课结束...");
                return returnVal;
            }
        });
    }
}
```

案例： 

![](<assets/1740030884108.png>)

 **核心代码**

```
//创建目标对象
ITeacherDao target = new TeacherDao();
//给目标对象，创建代理对象，可以转成 ITeacherDao
ITeacherDao proxyInstance = (ITeacherDao)new ProxyFactory(target).getProxyInstance();
// proxyInstance=class com.sun.proxy.$Proxy0 内存中动态生成了代理对象
System.out.println("proxyInstance=" + proxyInstance.getClass());
//通过代理对象，调用目标对象的方法
proxyInstance.teach();
```

创建代理对象：

```
/**
 * 被代理对象
 */
public class TeacherDao {
    public String teach() {
        System.out.println("老师授课中...");
        return "Good";
    }
}
 
/**
 * 代理工厂类，实现MethodInterceptor 接口
 */
public class ProxyFactory implements MethodInterceptor {
    /**
     * 目标对象
     */
    private Object target;
 
    /**
     * 构造函数
     *
     * @param target
     */
    public ProxyFactory(Object target) {
        this.target = target;
    }
 
    /**
     * 返回代理对象实例。不是静态方法
     *
     * @return
     */
    public Object getProxyInstance() {
        // 1、创建工具类
        Enhancer enhancer = new Enhancer();
        // 2、设置父类
        enhancer.setSuperclass(target.getClass());
        // 3、设置回调函数
        enhancer.setCallback(this);
        // 4、创建子类对象，即代理对象
        return enhancer.create();
    }
 
    @Override
    public Object intercept(Object o, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
        System.out.println("cglib代理开始...");
        Object retVal = method.invoke(target, args);
        System.out.println("cglib代理结束...");
        return retVal;
    }
}
```

其中几个参数

*   1）`ClassLoader loader`：指定当前目标对象使用的类加载器，获取加载器的方法固定

*   2）`Class<?>[] interfaces`：目标对象实现的接口类型，使用泛型方法确认类型

*   3）`InvocationHandler h`：事情处理，执行目标对象的方法时触发事情处理器方法，把当前执行的目标对象方法作为参数传入

## 4、Cglib 代理

**Cglib 代理**

静态 代理和 JDK 代理模式都要求目标对象是实现一个接口，但是有时候目标对象只是一个单独的对象，并没有实现任何的接口，这个时候可**使用目标对象子类来实现代理**——这就是 Cglib 代理

Cglib 代理也叫作**子类代理**，它是**在内存中构建一个子类对象从而实现对目标对象功能扩展**，有些书也将 Cglib 代理归属到动态代理。

**使用方法：**

代理工厂类实现 MethodInterceptor 接口并重写 intercept() 方法编写代理逻辑，通过 cglib 包的 Enhancer 类设置父类字节码文件和创建子类对象来返回代理对象实例。

**Cglib 包**

是一个强大的高性能的**代码生成包**，它可以在**运行期扩展 java 类与实现 java 接口**。它广泛的被许多 AOP 的框架使用，例如 Spring AOP，实现方法拦截。

Cglib 包的**底层**是通过使用 **ASM 框架**来**转换字节码并生成新的类**

**ASM 框架**

一个强大的 Java 字节码操作框架，可以让程序员通过代码生成和转换现有字节码来操作 Java 类。ASM 可以直接生成字节码，也可以通过访问现有字节码来修改它。我们可以使用 ASM 来生成新的类、新的方法、字段、注解等。同时，ASM 还允许我们在运行时改变现有的 Java 类的字节码，从而实现动态的 Java 类修改，例如添加方法、添加字段等。

在 AOP 编程中如何选择代理模式：

*   目标对象需要实现接口，用 JDK 代理
*    目标对象不需要实现接口，用 Cglib 代理

**案例：**

*   1）引入`cglib`的 jar 文件
    
    ![](<assets/1740030884609.png>)
    

*   2）在内存中动态构建子类，注意**代理的类不能为`final`**，否则报错`java.lang.IllegalArgumentException`

*   3）目标对象的方法如果为`final`/`static`，那么就不会被拦截，即不会执行目标对象额外的业务方法

uml 图，不再有代理接口  
 

![](<assets/1740030884830.png>)

 

  
**核心代码**  
 

```
//创建目标对象
TeacherDao teacherDao = new TeacherDao();
//通过代理工厂创建代理对象
TeacherDao proxyInstance = (TeacherDao) new ProxyFactory(teacherDao).getProxyInstance();
//通过代理对象调用目标对象方法
String retVal = proxyInstance.teach();
System.out.println("retVal=" + retVal);
```

 **调用代理**

```
//创建目标对象
TeacherDao teacherDao = new TeacherDao();
//通过代理工厂创建代理对象
TeacherDao proxyInstance = (TeacherDao) new ProxyFactory(teacherDao).getProxyInstance();
//通过代理对象调用目标对象方法
String retVal = proxyInstance.teach();
System.out.println("retVal=" + retVal);
```

## 5、代理模式 的变体（应用场景）

几种常见的代 理模式介绍一几种变体

*   **防火墙代理**：内网通过代理穿透防火墙，实现对公网的访问
*   **缓存代理：**比如：当请求图片文件等资源时，先到缓存代理取，如果取到资源则 ok；如果取不到资源，再到公网或者数据库取，然后缓存
*   **远程代理：**远程对象的本地代表，通过它可以把远程对象当本地对象来调用。远程代理通过网络和真正的远程对象沟通信息

![](<assets/1740030885116.png>)

 

*   **同步代理：**主要使用在多线程编程中，完成多线程间同步工作