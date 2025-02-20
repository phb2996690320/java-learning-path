---
url: https://blog.csdn.net/qq_40991313/article/details/130433828?spm=1001.2014.3001.5502
title: Java 工厂模式详解：从简单到抽象 - CSDN 博客
date: 2025-02-20 13:54:38
tag: 
summary: 
---
**导航：** 

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?spm=1001.2014.3001.5501 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1、工厂模式介绍](#t0)

[2、披萨项目需求](#t1)

[3、传统方式](#t2)

[4、非静态简单工厂模式](#t3)

[5、静态简单工厂模式](#t4)

[6、工厂方法模式](#t5)

[7、抽象工厂模式](#t6)

[8、JDK 源码分析](#t7)

## 1、工厂模式介绍

以提高复杂性为代价，提高可维护性和复用性。 

工厂模式是一种**创建型设计模式（跟创建对象有关）**，它主要解决了对象的创建过程中的灵活性和可维护性问题。工厂模式允许在**不暴露对象创建逻辑**的情况下，统一由**工厂类负责创建对象并返回**，从而降低了代码的耦合性。

**简单工厂模式** 

简单工厂模式是**通过工厂类**（抽象或非抽象）**的静态方法来创建对象实例**，并将实例作为方法的返回值。在使用时，只需要调用该静态方法来创建对象，而无需创建工厂类的实例。

**工厂方法模式**

工厂方法模式是在抽象工厂类里**定义创建抽象产品对象的抽象方法，由具体工厂类决定要实例化的产品类**。抽象工厂类的构造器里调用 “创建抽象产品对象” 的抽象方法，具体工厂类重写抽象方法，按情景实例化具体产品类。使用时直接创建具体工厂对象，将执行抽象工程类构造器调用重写后的创建方法，创建具体产品对象。

一个抽象工厂类能够派生出多个具体工厂类。每一个具体工厂类只能建立一个具体产品类的实例。一个抽象产品类能够派生出多个具体产品类。 

**抽象工厂模式**

抽象工厂模式是抽象工厂接口里定义创建抽象产品对象的抽象方法，各具体工厂实现类根据情景创建具体产品对象。

一个抽象工厂类能够派生出多个具体工厂类。每一个具体工厂类能够建立多个具体产品类的实例。多个抽象产品类，每一个抽象产品类能够派生出多个具体产品类。

**优点：**  
1. 可以避免直接使用 new 关键字创建对象带来的耦合性，提高了代码的**可维护性**；  
2. 可以将对象的创建逻辑封装到一个工厂类中，提高了代码的**复用性**；  
3. 可以对**对象的创建逻辑进行统一管理**，方便代码的维护和升级。

**缺点：**  
1. 增加了代码的复杂度，需要创建工厂类，会**增加代码规模**；  
2. 如果产品类发生变化，需要**修改工厂类，可能会影响到其他代码的功能**。

综上所述，工厂模式是一种常用的创建型设计模式，可以提高代码的可维护性、复用性和灵活性。但是，在使用时需要权衡利弊，避免过度使用，增加代码的复杂度。

**工厂模式的意义：**将**实例化对象的代码提取出来**，放到一个类中统一管理和维护，达到和主项目的依赖关系的解耦。从而提高项目的扩展和维护性

2）三种工厂模式：简单工厂模式（静态工厂方法也是简单工厂模式的一种）、工厂方法模式、抽象工厂模式

3）设计模式的依赖抽象原则

*   创建对象实例时，不要直接 new 类，而是把这个 new 类的动作放在一个工厂的方法中并返回。有的书上说，变量不要直接持有具体类的引用
*   不要让类继承具体类，而是继承抽象类或者是实现 interface（接口）
*   不要覆盖基类中已经实现的方法

## 2、披萨项目需求

**需求**

披萨项目：要便于披萨种类的扩展，要便于维护

*   1）披萨的种类很多（比如 GreekPizz、CheesePizz 等）

*   2）披萨的制作有 prepare、bake、cut、box

*   3）完成披萨店订购功能

## 3、传统方式

**UML 类图**

![](<assets/1740030878870.png>)

**核心代码**

```
public abstract class AbstractPizza {
    protected String name;
 
    public void setName(String name) {
        this.name = name;
    }
//准备原材料，不同具体披萨不一样，所以是抽象方法。
    public abstract void prepare();
 
    public void bake() {
        System.out.println(name + " baking...");
    }
 
    public void cut() {
        System.out.println(name + " cutting...");
    }
 
    public void box() {
        System.out.println(name + " boxing...");
    }
}
 
//希腊风味披萨
public class GreekPizza extends Pizza {
    @Override
    public void prepare() {
        setName("GreekPizza");
        System.out.println(name + " preparing...");
    }
}
 
// 奶酪披萨
public class CheesePizza extends Pizza {
    @Override
    public void prepare() {
        setName("CheesePizza");
        System.out.println(name + " preparing...");
    }
}
 
//订购披萨，不断输入披萨类型，输出披萨的生产包装过程。
//耦合度高，违反了设计模式的 OCP 原则，即对扩展开放，对修改关闭。
public class OrderPizza {
    public OrderPizza() {
        Pizza pizza = null;
        String orderType;
        do {
//每次新增披萨类型，都要改这个订购类，而订购类可能会有很多，都要改就太麻烦了
            orderType = getType();
            if ("cheese".equals(orderType)) {
                pizza = new CheesePizza();
            } else if ("greek".equals(orderType)) {
                pizza = new GreekPizza();
            } else {
                System.out.println("输入类型错误，程序退出");
                break;
            }
            pizza.prepare();
            pizza.bake();
            pizza.cut();
            pizza.box();
        } while (true);
    }
 
    private String getType() {
        System.out.println("请输入披萨类型：");
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
        try {
            return reader.readLine();
        } catch (IOException e) {
            e.printStackTrace();
            return "";
        }
    }
}
//披萨商店
//相当于一个客户端，发出订购
public class PizzaStore {
    public static void main(String[] args) {
        new stubOrderPizza();
}
```

**传统方式优缺点**

*   1）优点是比较好理解，简单易操作

*   2）缺点是**违反了设计模式的 OCP 原则，即对扩展开放，对修改关闭。**即当我们给类增加新能的时候，尽量不修改代码，或者尽可能少修改代码

*   3）比如我们这时要新增加一个 Pizza 的种类（Cheese 技萨），我们需要做如下修改订购类

```
// 胡椒披萨
public class PepperPizza extends Pizza {
    @Override
    public void prepare() {
        setName("PepperPizza");
        System.out.println(name + " preparing...");
    }
}
 
public class OrderPizza {
    public OrderPizza() {
        // ...
        else if ("pepper".equals(orderType)) {
            pizza = new PepperPizza();
        } 
        // ...
    }
    // ...
}
```

**改进的思路分析**

*   分析：修改代码可以接受，但是如果我们在其它的地方也有创建 Pizza 的代码，就意味着也需要修改。而创建 Pizza 的代码，往往有多处

*   思路：把**创建 Pizza 对象封装到一个类中**，这样我们有新的 Pizza 种类时，只需要修改该类就可，其它有创建到 Pizza 对象的代码就不需要修改了 ==> **简单工厂模式**

## 4、非静态简单工厂模式

*   1）简单工厂模式是属于创建型模式，是工厂模式的一种。简单工厂模式是**由一个工厂对象决定创建出哪一种产品类的实例**。简单工厂模式是工厂模式家族中最简单实用的模式

*   2）简单工厂模式：**定义了一个创建对象的类，由这个类来封装实例化对象的行为**（代码）

*   3）在软件开发中，当我们会用到大量的创建某种、某类或者某批对象时，就会使用到工厂模式

**简单工厂模式优化披萨项目**

创建一个披萨工厂类，用于根据披萨类别创建披萨对象。

**UML 类图**

![](<assets/1740030879138.png>)

**核心代码**

```
//创建一个披萨工厂类，用于根据披萨类别创建披萨对象。
public class PizzaFactory {
    public Pizza createPizza(String orderType) {
        Pizza pizza = null;
        switch (orderType) {
            case "cheese":
                pizza = new CheesePizza();
                break;
            case "greek":
                pizza = new GreekPizza();
                break;
            case "pepper":
                pizza = new PepperPizza();
                break;
            default:
                break;
        }
        return pizza;
    }
}
//修改订购披萨类，披萨对象从披萨工厂类获取
public class OrderPizza {
    private PizzaFactory pizzaFactory;
 
    public OrderPizza(PizzaFactory pizzaFactory) {
        this.pizzaFactory = pizzaFactory;
        orderPizza();
    }
 
    public void orderPizza() {
        Pizza pizza = null;
        do {
            pizza = pizzaFactory.createPizza(getType());
            if (pizza == null) {
                System.out.println("Failed to Order Pizza");
            } else {
                pizza.prepare();
                pizza.bake();
                pizza.cut();
                pizza.box();
            }
        } while (true);        //无限循环，不断输入披萨类型，进行加工、包装等操作；
    }
    // ...
}
```

## 5、静态简单工厂模式

静态工厂模式也是简单工厂模式的一种，只是**将工厂方法改为静态方法**

静态工厂模式**通过工厂类的静态方法来创建对象实例，并将实例作为方法的返回值**。在使用时，只需要调用该静态方法来创建对象，而无需创建工厂类的实例。

静态工厂模式的优点在于可以**简化代码实现**，无需创建工厂对象的实例，提高代码的简洁性；

**对比普通工厂模式**的优点在于**更方便扩展和修改**，如果需要新增一个产品线，只需要添加具体的产品类和对应的工厂类即可，无需修改已有的代码。

优化披萨项目

**核心代码**

```
//简单静态工厂
public class PizzaFactory {
    public static Pizza createPizza2(String orderType) {
        // ...
    }
}
//订购披萨类
public class OrderPizza {
    public OrderPizza() {
        Pizza pizza;
        do {
//直接通过静态方法创建工厂对象，不用再像之前通过“构造器参数赋值成员变量”方式创建对象。
            pizza = PizzaFactory.createPizza(getType());    
            // ...
        } while (true);
    }
}
```

## 6、工厂方法模式

工厂方法模式：**定义创建对象的抽象方法**，由**子类决定要实例化的类**。工厂方法模式将对象的实例化推迟到子类。

**实现方式：**抽象工厂类的构造器里调用 “创建抽象产品对象” 的抽象方法，具体工厂类重写抽象方法，按情景实例化具体产品类。

一个抽象产品能够派生出多个具体产品类。 一个抽象工厂类能够派生出多个具体工厂类。每一个具体工厂类只能建立一个具体产品类的实例。 

工厂方法模式包含以下几个角色：

1.  抽象产品类（Product）：定义了产品的抽象接口，具体产品将按照抽象产品类所定义的接口来实现。
    
2.  具体产品类（Concrete Product）：是抽象产品类的一个具体实现，定义了具体产品的实现方法。
    
3.  抽象工厂类（Factory）：是工厂方法模式的核心，定义了工厂类需要实现的接口，用于创建产品对象。
    
4.  具体工厂类（Concrete Factory）：是抽象工厂类的一个具体实现，实现了工厂方法创建具体对象实例的逻辑。
    

**看一个新的需求**

披萨项目新的需求：客户在点披萨时，可以点不同口味的披萨，比如**北京的奶酪 Pizza**、北京的胡椒 Pizza 或者是伦敦的奶酪 Pizza、伦敦的胡椒 Pizza

思路 1：使用简单工厂模式，创建不同的简单工厂类，比如 BJPizzaFactory、LDPizzaFactory 等等。从当前这个案例来说，也是可以的，但是考虑到项目的规模，以及软件的可维护性、可扩展性并不是特别好

**思路 2：**工厂方法模式设计方案：将披萨项目的**实例化功能抽象成抽象方法**，在不同的口味点餐子类中具体实现。

**UML 类图**

![](<assets/1740030879394.png>)

**核心代码**

抽象工厂类，订购披萨类：

```
//抽象工厂类，订购披萨类：
public abstract class AbstractOrderPizzaFactory{
//构造方法不断根据用户输入披萨类型，调用创建披萨对象的抽象方法，实现加工披萨。
    public void OrderPizza() {
        Pizza pizza = null;
        do {
            pizza = createPizza(getType());
            if (pizza == null) {
                System.out.println("Failed to Order Pizza");
            } else {
                pizza.prepare();
                pizza.bake();
                pizza.cut();
                pizza.box();
            }
        } while (true);
    }
 
    public abstract Pizza createPizza(String orderType);
     
    // ...
}
```

```
//具体工厂类，订购伦敦披萨类：
public class LDOrderPizza extends AbstractOrderPizzaFactory{
    @Override
    public Pizza createPizza(String orderType) {
        Pizza pizza = null;
        switch (orderType) {
            case "cheese":
                pizza = new LDCheesePizza();
                break;
            case "pepper":
                pizza = new LDPepperPizza();
                break;
            default:
                break;
        }
        return pizza;
    }
}
```

```
具体工厂类，订购北京披萨类：
public class BJOrderPizza extends AbstractOrderPizzaFactory{
    @Override
    public Pizza createPizza(String orderType) {
        Pizza pizza = null;
        switch (orderType) {
            case "cheese":
                pizza = new BJCheesePizza();
                break;
            case "pepper":
                pizza = new BJPepperPizza();
                break;
            default:
                break;
        }
        return pizza;
    }
}
```

```
public class PizzaStore{
    public static void main(String[] args) {
        String loc = "bj";
        if (loc.equals("bj")) {//创建北京口味的各种Rizza
            new BJOrderPizza();}
        else {
 
            //创建伦敦口味的各种Pizza
            new LDOrderPizza();
        }
    }
}
```

## 7、抽象工厂模式

 抽象工厂接口里有创建抽象产品的方法，各具体工厂实现类根据情景创建具体产品对象。

*   1）抽象工厂模式：定义了一个接口用于创建相关或有依赖关系的对象簇，而无需指明具体的类

*   2）抽象工厂模式可以**将简单工厂模式和工厂方法模式进行整合**

*   3）从设计层面看，抽象工厂模式就是对简单工厂模式的改进（或者称为进一步的抽象）

*   4）将工厂抽象成两层，抽象工厂接口和具体工厂类。程序员可以根据创建对象类型使用对应的工厂子类。这样将单个的简单工厂类变成了工厂簇，更利于代码的维护和扩展

**UML 类图**

![](<assets/1740030879647.png>)

 抽象工厂接口里有创建抽象产品的方法，各具体工厂实现类根据情景创建具体产品对象。

**核心代码**

```
public interface AbsPizzaFactory {
    Pizza createPizza(String orderType);
}
 
public class BJPizzaFactory implements AbsPizzaFactory {
    @Override
    public Pizza createPizza(String orderType) {
        Pizza pizza = null;
        switch (orderType) {
            case "cheese":
                pizza = new BJCheesePizza();
                break;
            case "pepper":
                pizza = new BJPepperPizza();
                break;
            default:
                break;
        }
        return pizza;
    }
}
 
public class LDPizzaFactory implements AbsPizzaFactory {
    @Override
    public Pizza createPizza(String orderType) {
        Pizza pizza = null;
        switch (orderType) {
            case "cheese":
                pizza = new LDCheesePizza();
                break;
            case "pepper":
                pizza = new LDPepperPizza();
                break;
            default:
                break;
        }
        return pizza;
    }
}
```

```
//创建订购单，由具体工厂类创建具体产品对象
new OrderPizza(new BJFactory());
```

OrderPizza 订购披萨：

![](<assets/1740030879959.png>)

## 8、JDK 源码分析

JDK 中的 Calendar 类中，就使用了**静态简单工厂模式：由一个工厂对象**（可以是抽象类，可以是非抽象类）**决定创建出哪一种产品类的实例**

Calendar 类是一个抽象的工厂类，用于根据时区和地区创建出具体的日期类对象。 

其中 getInstance() 静态方法用于返回具体日期类的实例，它调用 createCalendar(时区, 地区) 方法创建具体日期类，例如 JapaneseImperialCalendar 日本日期类

**测试代码：** 

```
public class Test {
    public static void main(String[] args) {
//Calendar类是工厂类，getInstance()是静态方法，用于创建对象
        Calendar cal = Calendar.getInstance();
        // 注意月份下标从O开始，所以取月份要+1
        System.out.println("年:" + cal.get(Calendar.YEAR));
        System.out.println("月:" + (cal.get(Calendar.MONTH) + 1));
        System.out.println("日:" + cal.get(Calendar.DAY_OF_MONTH));
        System.out.println("时:" + cal.get(Calendar. HOUR_OF_DAY)) ;
        System.out.println("分:" + cal.get(Calendar. MINUTE)) ;
        System.out.println("秒:" + cal. get(Calendar.SECOND));
    }
}
```

Calendar 工厂类，决定创建出哪一种产品类的实例

```
//Calendar类是工厂类，getInstance()是静态方法，用于创建对象
public abstract class Calendar implements Serializable, Cloneable, Comparable<Calendar> {
...
//根据时区地区返回Calendar 实例
    public static Calendar getInstance() {
        Locale aLocale = Locale.getDefault(Locale.Category.FORMAT);
        return createCalendar(defaultTimeZone(aLocale), aLocale);
    }
//根据时区地区创建Calendar 实例
    private static Calendar createCalendar(TimeZone zone,
                                           Locale aLocale) {
        CalendarProvider provider =
            LocaleProviderAdapter.getAdapter(CalendarProvider.class, aLocale)
                                 .getCalendarProvider();
        if (provider != null) {
            try {
                return provider.getInstance(zone, aLocale);
            } catch (IllegalArgumentException iae) {
                // fall back to the default instantiation
            }
        }
 
        Calendar cal = null;
 
        if (aLocale.hasExtensions()) {
            String caltype = aLocale.getUnicodeLocaleType("ca");
            if (caltype != null) {
                switch (caltype) {
                case "buddhist":
                cal = new BuddhistCalendar(zone, aLocale);
                    break;
                case "japanese":
                    cal = new JapaneseImperialCalendar(zone, aLocale);
                    break;
                case "gregory":
                    cal = new GregorianCalendar(zone, aLocale);
                    break;
                }
            }
        }
        if (cal == null) {
//根据不同地区，返回不同的具体的产品类
            // If no known calendar type is explicitly specified,
            // perform the traditional way to create a Calendar:
            // create a BuddhistCalendar for th_TH locale,
            // a JapaneseImperialCalendar for ja_JP_JP locale, or
            // a GregorianCalendar for any other locales.
            // NOTE: The language, country and variant strings are interned.
            if (aLocale.getLanguage() == "th" && aLocale.getCountry() == "TH") {
                cal = new BuddhistCalendar(zone, aLocale);
            } else if (aLocale.getVariant() == "JP" && aLocale.getLanguage() == "ja"
                       && aLocale.getCountry() == "JP") {
                cal = new JapaneseImperialCalendar(zone, aLocale);
            } else {
                cal = new GregorianCalendar(zone, aLocale);
            }
        }
        return cal;
    }
...
}
```