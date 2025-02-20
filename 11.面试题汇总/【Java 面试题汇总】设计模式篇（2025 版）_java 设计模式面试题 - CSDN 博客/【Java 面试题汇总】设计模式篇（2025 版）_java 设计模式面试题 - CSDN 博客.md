---
url: https://blog.csdn.net/qq_40991313/article/details/130435077
title: 【Java 面试题汇总】设计模式篇（2025 版）_java 设计模式面试题 - CSDN 博客
date: 2025-02-20 13:45:55
tag: 
summary: 
---
 **导航：** 

[【黑马 Java 笔记 + 踩坑汇总】JavaSE+JavaWeb+SSM+SpringBoot + 瑞吉外卖 + SpringCloud + 黑马旅游 + 谷粒商城 + 学成在线 + 设计模式 + 牛客面试题](https://blog.csdn.net/qq_40991313/article/details/126646289?spm=1001.2014.3001.5501 "【黑马Java笔记+踩坑汇总】JavaSE+JavaWeb+SSM+SpringBoot+瑞吉外卖+SpringCloud+黑马旅游+谷粒商城+学成在线+设计模式+牛客面试题")

**目录**

[谈谈你对设计模式的理解？](#t0)

[谈谈你对单例模式的理解？](#t1)

[手写一下单例模式](#t2)

[谈谈你对工厂模式的理解？](#t3)

[谈谈你对代理模式的理解？](#t4)

[谈谈你对模板模式的理解？](#t5)

[谈谈你对观察者模式的理解？](#t6)

[职责链模式](#t7)

[谈谈 JDK 中用到的设计模式？](#t8)

[谈谈 Spring 中用到的设计模式？](#t9)

### 谈谈你对设计模式的理解？

**关键字：**总结的设计经验、通用解决方案、维护性通用性扩展性、降低复杂度、具体问题具体分析；七大原则、三种类型。

**设计模式：**

设计模式是程序员在面对同类软件工程设计问题**所总结出来的有用的经验**。模式不是代码，而是某类问题的**通用解决方案**，设计模式（Design pattern）代表了最佳实践。这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的。

设计模式的本质提高软件的**维护性、通用性和扩展性**，并降低软件的复杂度。

过度地使用设计模式可能会导致代码的过度抽象，增加了代码的维护难度和学习成本。因此，在使用设计模式时需要根据具体的项目需要和**实际情况**进行选择。

**七大原则：**

*   **单一职责原则：**一个类应该只负责一项职责。提高可读性维护性、降低耦合。
*   **接口隔离原则：**类之间的依赖关系应该建立在最小的接口上。例如实现类只能用到接口的部分方法，为了降低冗余提高可维护性，对原接口进行合理的拆分和组合。
*   **依赖倒转原则：**高层模块不应该直接依赖于低层模块，两者都应该依赖于抽象。即详细应该依赖于抽象。在调用链上，调用者属于高层，被调用者属于低层。例如 controller 层不能直接依赖于 service 层，他们都应该依赖于抽象，即 service 接口。
*   **里氏代换原则：**子类引用指向父类对象后功能未变，子类中尽量不要重写父类的方法。
*   **开闭原则：**软件对扩展开放，对修改关闭。提高扩展性和可维护性。
*   **迪米特法则：**最少知道原则，一个类对自己依赖的类知道的越少越好，只与直接的朋友（成员变量，方法参数，方法返回值的类）通信。
*   **合成复用原则：**尽量使用聚合或组合的方式，而不是使用继承。也就是把需要用到的类作为本类的参数、成员变量、局部变量。

**其他原则：**

*   **KISS 原则：**即 "保持简单原则"，它强调在设计和编写代码时，应该尽量保持简单，避免过度复杂化。产品的设计越简单越好, 任何没有必要的复杂都是需要避免的。

**类之间关系：**

*   **依赖：**我用到了你，我就依赖你。例如你是我的成员变量、成员方法的参数返回值中间变量等。
*   **泛化：**泛化就是继承。
*   **实现：**实现类实现了接口。
*   **关联：**类与类之间的关系，可以一对一、零对一、一对多等。例如学生和学号是关联的。
*   **聚合：**整体和部分可以分割。例如电脑 USB 接口聚合了鼠标，鼠标是电脑的成员方法。
*   **组合：**整体和部分不能分割。例如把鼠标焊死在电脑上，他们就是组合关系了。

**设计模式类型：**

*   **创建型模式：**用于对象的创建，包括单例、工厂等模式。
*   **结构型模式：**用于描述对象之间的组合关系，包括代理、享元等模式
*   **行为型模式：**用于描述对象之间的通信和责任分配。包括模版方法、观察者、责任链等模式

### 谈谈你对单例模式的理解？

**关键字：**概念（一个实例、一个静态方法）、实现方案、优缺点（节省资源、避免重复创建对象、管理全局变量、注意线程安全）、场景（重量级对象）

保证一个类只有一个实例，并且只提供一个取得对象实例的静态方法。

**实现方式：** 

*   **饿汉：**构造器私有化、类内部创建私有静态常对象、静态公共方法返回这个实例。线程安全（类加载器加载类是线程安全的），没用到会浪费内存（类加载过程会给类变量赋初值）。
*   **懒汉：**类内部创建私有静态对象引用、静态公共方法判空再实例化再返回。懒加载，线程不安全，可以方法加 synchronized 锁实现同步但效率低。
*   **双重检查：**优化懒汉，类内部引用 volatile 修饰，双层判空中间 synchronized 类级别锁。线程安全，延时加载，效率高，推荐。
*   **静态内部类：**在静态内部类里面创建实例。线程安全，延时加载，效率高，推荐。
*   **枚举：**提倡。枚举类的常量是单例的。

**优点：**

*   **节省资源：**单例模式实例只有一个，可以**避免重复创建对象**，从而节省了资源，提高了系统性能。
*   管理全局变量：单例模式可以用于管理全局状态和变量，方便在整个系统中**共享数据**。
*   简化系统架构：使用单例模式可以简化系统架构，减少类的数量和接口的复杂度。

**缺点：**

1.  **可能引发并发问题：**单例模式在多线程中使用时，需要保证线程安全，否则可能会引发并发问题。
2.  **内存泄漏问题：**单例对象是类变量，属于 GC Roots，它和它引用的对象即使不再被引用，也不会被 JVM 垃圾回收。
3.  可能增加系统复杂性：过度使用单例模式可能会增加系统复杂性，导致代码难以维护。
4.  难以调试：由于单例模式全局共享状态，可能会导致调试过程中的问题难以定位和测试。

**使用场景：**

*   需要**频繁的进行创建和销毁的对象**、创建对象时耗时过多或耗费资源过多但又经常用到的对象（即：重量级对象）、工具类对象、频繁访问数据库或文件的对象（比如数据源、session 工厂等）
*   JDK 中 **java.lang.Runtime 类**就是饿汉式单例模式。

### 手写一下单例模式

实现方式： 饿汉懒汉、双重检查、静态内部类、枚举。

**饿汉（静态常量版）：**线程安全，没用到会浪费内存。

```
public class Singleton {
    // 1、构造器私有化
    private Singleton() {
    }
 
    // 2、类的内部创建对象
    private static final Singleton instance = new Singleton();
 
    // 3、向外暴露一个静态的公共方法
    public static Singleton getInstance() {
        return instance;
    }
}
```

**懒汉（同步方法版）：**懒加载，线程安全，但效率低（每次获取实例都要加锁），不推荐。

```
public class Singleton {
    // 1、构造器私有化
    private Singleton() {
    }
 
    // 2、类的内部声明对象
    private static Singleton instance;
 
    // 3、向外暴露一个静态的公共方法，加入同步处理的代码，解决线程安全问题
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

**双重检查：**线程安全、懒加载

```
public class Singleton {
    // 1、构造器私有化
    private Singleton() {
    }
 
    // 2、类的内部声明对象，同时用`volatile`关键字修饰，为了保证可见性。
//原子性、可见性（修改立即更新到内存）、有序性
    private static volatile Singleton instance;
 
    // 3、向外暴露一个静态的公共方法，加入同步处理的代码块，并进行双重判断，解决线程安全问题
    public static Singleton getInstance() {
        if (instance == null) {    //第一次检查，可能有多个线程同时通过检查
//类级别的锁对象，锁对象是全局的，对该类的所有实例均有效。回顾锁对象是this时仅限于锁当前实例
            synchronized (Singleton.class) {    
                if (instance == null) {   //第二次检查，只会有1个线程通过检查并创建实例
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

**静态内部类：**线程安全、延迟加载、效率高。

```
public class Singleton {
    // 1、构造器私有化
    private Singleton() {
    }
 
    // 2、定义一个静态内部类，内部定义当前类的静态属性
    private static class SingletonInstance {
        private static final Singleton instance = new Singleton();
    }
 
    // 3、向外暴露一个静态的公共方法
    public static Singleton getInstance() {
        return SingletonInstance.instance;
    }
}
```

**枚举：**线程安全，延迟加载。

```
public enum Singleton {
    INSTANCE;
}
```

### 谈谈你对工厂模式的理解？

工厂类创建对象、工厂、抽象工厂 、复用性可维护性、具体问题具体分析。

以提高复杂性为代价，提高可维护性和复用性。 

工厂模式是一种创建型设计模式，它主要解决了对象的创建过程中的灵活性和可维护性问题。

工厂模式允许在不暴露对象创建逻辑的情况下，统一由**工厂类负责创建对象并返回**，从而降低了代码的耦合性。

**简单工厂模式：**工厂类的静态方法根据情景创建并返回不同实例

```
public class PizzaFactory {
    public static Pizza createPizza(String orderType) {
        Pizza pizza = null;
        if("芝士披萨".equals(orderType)) pizza = new CheesePizza();
        //else if()... else....
        return pizza;
    }
}
```

**工厂方法模式：**抽象工厂**类**里定义返回抽象产品实例的抽象方法，各具体工厂类决定要实例化的具体产品类实例。

```
public abstract class OrderPizza {
    public abstract Pizza createPizza(String orderType);
    // ...
}
```

**抽象工厂模式：**抽象工厂**接口**定义返回产品实例的方法，各具体工厂类决定要实例化的具体产品类实例。

**重写回顾：**重写时返回值类可以是原返回值类的子类、访问权限不能比其父类更为严格、抛出异常不能比父类更广泛。

```
public interface AbsPizzaFactory {
    Pizza createPizza(String orderType);
}
```

**优点：**

*   **可维护性：**可以避免直接使用 new 关键字创建对象带来的耦合性，提高了代码的可维护性；
*   **复用性：**可以将对象的创建逻辑封装到一个工厂类中，提高了代码的复用性；
*   **方便维护和升级：**可以对对象的创建逻辑进行统一管理，方便代码的维护和升级。

**缺点：**

*   **代码规模：**增加了代码的复杂度，需要创建工厂类，会增加代码规模；
*   **耦合性：**如果产品类发生变化，需要修改工厂类，可能会影响到其他代码的功能。

综上所述，工厂模式是一种常用的创建型设计模式，可以提高代码的可维护性、复用性和灵活性。但是，在使用时需要权衡利弊，避免过度使用，增加代码的复杂度。

### 谈谈你对代理模式的理解？

关键字：结构性、代理对象、增强、控制远程对象、安全控制；**静态代理**、相同代理接口；**JDK 动态代理**、反射、内存、代理工厂、Proxy.newProxyInstance()、Spring AOP；**Cglib 代理**、子类对象、代理工厂、ASM 框架、MethodInterceptor 接口的 intercept() 方法、cglib 包的 Enhancer 工具类

代理模式是结构型设计模式（用于描述对象之间的组合关系）。

**代理模式：**在目标对象实现的基础上，增强额外的功能操作，即扩展目标对象的功能。主要包括静态代理、JDK 动态代理、Cglib 代理。

**静态代理：**目标对象与代理对象实现租同的接口或抽象父类，通过实现方法进行增强。在**编译时**生成代理对象。

**JDK 动态代理：**目标对象实现代理接口，简单工厂类返回 Proxy.newProxyInstance(类加载, 类 class 对象, 实现了 invoke() 方法的 InvocationHandler 对象) 方法创建的代理对象。利用 Java 反射机制在运行时动态地在内存中生成代理对象。Spring AOP 采用了 JDK 动态代理的方式，在运行时动态的创建代理对象来实现增强。

```
//代理接口 ITeacherDao，目标对象TeacherDao，目标对象实现了代理接口
/**
 * 代理工厂
 */
public class TeacherFactory {
    /**
     * 目标对象
     */
    private Object target;
 //构造参数是目标对象
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

```
//创建目标对象
ITeacherDao target = new TeacherDao();
//给目标对象，创建代理对象，可以转成 ITeacherDao
ITeacherDao proxyInstance = (ITeacherDao)new ProxyFactory(target).getProxyInstance();
```

**Cglib 代理：**简单工厂类实现方法拦截器接口（MethodInterceptor），重写 intercept() 方法编写增强逻辑，并通过增强器类（**Enhancer**）基于反射和 ASM 框架操作原始对象这个成员变量，创建并返回代理对象实例。在内存中构建一个**子类对象**从而实现对目标对象功能扩展。ASM 框架主要用来操作字节码文件，可以直接生成字节码，也可以通过访问现有字节码来修改它。

```
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
//或者
//      enhancer.setCallback(new MethodInterceptor() { 
//        @Override 
//        public Object intercept(Object obj, Method method, 
//            Object[] args, MethodProxy proxy) throws Throwable { 
//          return method.invoke(obj, args); 
//        } 
//      }); 
 
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

### 谈谈你对模板模式的理解？

**行为型模式：**用于描述对象之间的通信和责任分配。

**实现方式：**抽象类有一个模板方法和其他行为方法，模板方法按流程调用各行为方法（抽象或非抽象）；具体子类重写抽象的行为方法。 

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
```

**优点：**

1.  **可重用性和可维护性：**由于算法的骨架在父类或者抽象类中实现，因此可以避免在每个子类中重复编写相同的代码，提高了代码的可重用性和可维护性；
2.  可扩展性：由于子类只需要实现特定的部分而不需要修改算法的整体结构，因此可以提高代码的安全性和可扩展性；
3.  一致性和稳定性：可以在父类或者抽象类中控制算法的结构和执行流程，从而保证算法的一致性和稳定性。

**应用场景：**

AQS 是基于模板方法模式进行设计的。锁的实现类需要继承 AQS 并重写它指定的方法。

AQS 的模板方法将 “**管理同步状态的逻辑**” 提炼出来形成标准流程，这些方法主要包括：独占式获取同步状态、独占式释放同步状态、共享式获取同步状态、共享式释放同步状态。 

### 谈谈你对观察者模式的理解？

**关键字：** 行为型、一对多、两个接口、主题、注册、移除、通知观察者更新；观察者、更新；耦合、开闭原则、复杂度；JDK、Observable、Observer

**观察者模式：**当主题对象状态发生改变时，它的观察者对象都能得到通知并自动更新。行为型设计模式（描述对象之间的通信和责任分配）。

**实现方法：**

*   **主题对象**实现主题接口的注册、移除、通知方法，并**管理资源和观察者列表**；
    *   注册：将参数传来的观察者对象添加到观察者列表里。
    *   **通知：**循环调用所有观察者更新方法。
*   **观察者对象**实现观察者接口的更新方法，并管理资源；

```
/**
 * 主体对象接口，有注册、移除和通知功能；
 */
public interface Subject {
//注册某观察者
    void registerObserver(Observer o);
//移除某观察者
    void removeObserver(Observer o);
//通知
    void notifyObservers();
}
/**
 * 主体对象实现，聚合观察者列表，管理天气信息和观察者列表；
 */
public class WeatherData implements Subject {
//资源：温度、气压、湿度
    private Float temperature;
    private Float pressure;
    private Float humidity;
    private List<Observer> observerList;
 
    public WeatherData() {
        observerList = new ArrayList<>();
    }
 
    public Float getTemperature() {
        return temperature;
    }
 
    public Float getPressure() {
        return pressure;
    }
 
    public Float getHumidity() {
        return humidity;
    }
 
    /**
     * 推送天气数据到网站
     */
    public void dataChange() {
        notifyObservers();
    }
 
    /**
     * 当天气数据发生变化时进行更新
     *
     * @param temperature
     * @param pressure
     * @param humidity
     */
    public void setData(Float temperature, Float pressure, Float humidity) {
        this.temperature = temperature;
        this.pressure = pressure;
        this.humidity = humidity;
        dataChange();
    }
//注册某观察者
    @Override
    public void registerObserver(Observer o) {
        observerList.add(o);
    }
//移除某观察者
    @Override
    public void removeObserver(Observer o) {
        if(o!= null && observerList.contains(o)) {
            observerList.remove(o);
        }
    }
//通知
    @Override
    public void notifyObservers() {
        for (Observer observer : observerList) {
            observer.update(temperature, pressure, humidity);
        }
    }
}
```

```
/**
 * 观察者接口，发起更新天气信息请求；
 */
public interface Observer {
    void update(Float temperature, Float pressure, Float humidity);
}
/**
 * 观察者实现，发起更新天气信息请求和使用天气
 */
public class CurrentConditions implements Observer {
    private Float temperature;
    private Float pressure;
    private Float humidity;
 
    @Override
    public void update(Float temperature, Float pressure, Float humidity) {
        // 更新最新天气数据
        this.temperature = temperature;
        this.pressure = pressure;
        this.humidity = humidity;
        // 展示最新天气数据
        display();
    }
 
    /**
     * 公告板展示天气情况
     */
    public void display() {
        System.out.println("============最新天气============");
        System.out.println("*** 当前温度：" + this.temperature + " ℃ ***");
        System.out.println("*** 当前气压：" + this.pressure + " kPa ***");
        System.out.println("*** 当前湿度：" + this.humidity + " %RH ***");
    }
}
```

**优点：**

*   **降低耦合：**降低了对象之间的耦合度，因为主题对象不需要知道观察者的具体实现，只需要知道观察者实现了一个特定接口即可。
*   **动态扩展：**可以动态扩展观察者列表，方便灵活。
*   **开闭原则：**实现了对象之间的一对多依赖关系，提高了系统的可维护性和可重用性。遵守了 ocp 原则（开闭原则：对扩展开放，对修改关闭）。

**缺点**：

*   **通知耗时：**当观察者过多时，通知过程需要花费较多的时间，会影响系统的性能。
*   **循环依赖导致死循环：**如果观察者与主题对象之间存在**循环依赖**，可能会出现**死循环**。

**JDK：**提供观察者模式基础功能的主题抽象类和观察者接口：

Observable 抽象类简单实现了主题对象基础功能（注册、移除、通知）；

Observer 即观察者接口，具有 update() 方法  。

### 职责链模式

类似一个链表。

抽象处理人：成员变量是资源和下一个抽象处理人，通过 setNext() 设置下一个抽象处理人（后面会多态形式传参具体处理人）。

具体处理人：层层判断处理权限，没权限的话把请求交给下一个具体处理人，有权限就处理。

测试方法：创建每个具体处理人对象，通过 setNext() 按处理人权限把每个对象串起来。

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
```

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

### 谈谈 JDK 中用到的设计模式？

**单例模式：**

JDK 中 **java.lang.Runtime 类**就是饿汉式单例模式。

**静态简单工厂模式** 

Calendar 类中，使用了静态简单工厂模式：由一个工厂对象（可以是抽象类，可以是非抽象类）决定创建出哪一种产品类的实例

Calendar 类是一个抽象的工厂类，用于根据时区和地区创建出具体的日期类对象。 

其中 getInstance() 静态方法用于返回具体日期类的实例，它调用 createCalendar(时区, 地区) 方法创建具体日期类，例如 JapaneseImperialCalendar 日本国日期类

**观察者模式**

JDK 提供观察者模式基础功能的主题抽象类和观察者接口：

Observable 抽象类简单实现了主题对象基础功能（注册、移除、通知）；

Observer 即观察者接口，具有 update() 方法  。

### 谈谈 Spring 中用到的设计模式？

Spring AOP 采用了 JDK 动态代理的方式，在运行时动态的创建代理对象来实现增强。