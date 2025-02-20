---
url: https://blog.csdn.net/qq_40991313/article/details/130451520
title: Java 中的观察者模式：实现与优化天气预报系统,-CSDN 博客
date: 2025-02-20 13:54:44
tag: 
summary: 
---
 **导航：** 

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?spm=1001.2014.3001.5501 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[观察者模式](#t0)

[1、天气预报需求](#t1)

[2、天气预报需求方案之普通方案](#t2)

[3、观察者模式介绍](#t3)

[4、观察者模式优化天气预报案例](#t4)

[5、JDK 的 Observable 类和 Observer 类](#t5)

## 观察者模式

### 1、天气预报需求

具体要求如下：

*   1）气象站可以将每天测量到的温度，湿度，气压等等以公告的形式发布出去（比如发布到自己的网站或第三方）

*   2）需要**设计开放型 API**，便于其他**第三方也能接入气象站获取数据**

*   3）提供温度、气压和湿度的接口

*   4）**测量数据更新时，要能实时的通知给第三方**

![](<assets/1740030884935.png>)

​

### 2、天气预报需求方案之普通方案

**WeatherData 类**

通过对气象站项目的分析，我们可以初步设计出一个天气数据类 WeatherData 类

![](<assets/1740030885273.png>)

​

*   1）通过`getXxx`方法，可以让第三方接入，并得到相关信息

*   2）当数据有更新时，气象站通过调用 **dataChange() 去更新数据**，当第三方再次获取时，就能得到最新数据，当然也可以推送

![](<assets/1740030885457.png>)

​

CurrentConditions（当前的天气情况）可以理解成是我们气象局的网站

**核心代码**

当前天气状况类，即气象网，用于展示天气数据

```
/**
 * 当前的天气情况：可以理解成是气象局的网站
 */
public class CurrentConditions {
    private Float temperature;
    private Float pressure;
    private Float humidity;
 
    /**
     * 更新天气情况，通过推送的方式，由 WeatherData 调用
     *
     * @param temperature
     * @param pressure
     * @param humidity
     */
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

天气数据类

```
/**
 * 核心类
 * 1、包含最新的天气信息情况
 * 2、含有 CurrentConditions 对象
 * 3、当数据更新时，主动调用 CurrentConditions 的 update() 方法
 */
public class WeatherData {
    private Float temperature;
    private Float pressure;
    private Float humidity;
    private CurrentConditions conditions;
 
    /**
     * 传入 CurrentConditions 对象
     *
     * @param conditions
     */
    public WeatherData(CurrentConditions conditions) {
        this.conditions = conditions;
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
        conditions.update(getTemperature(), getPressure(), getHumidity());
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
}
```

客户端调用

```
// 创建气象网站对象
CurrentConditions currentConditions = new CurrentConditions();
// 创建气象数据对象，并传入气象网站对象
WeatherData weatherData = new WeatherData(currentConditions);
// 天气发生变化时，更新最新的气象数据
weatherData.setData(10f, 150f, 40f);
//weatherData.setData(15f, 130f, 60f);
//weatherData.setData(13f, 160f, 20f);
```

测试结果

```
============最新天气============
*** 当前温度：10.0 ℃ ***
*** 当前气压：150.0 kPa ***
*** 当前湿度：40.0 %RH ***
============最新天气============
*** 当前温度：15.0 ℃ ***
*** 当前气压：130.0 kPa ***
*** 当前湿度：60.0 %RH ***
============最新天气============
*** 当前温度：13.0 ℃ ***
*** 当前气压：160.0 kPa ***
*** 当前湿度：20.0 %RH ***
```

**问题分析**

*   1）其他第三方接入气象站获取数据的问题

*   2）无法在运行时动态的添加第三方（如 xx 网站）

*   3）违反`OCP`原则 => 观察者模式

在`WeatherData`中增加第三方时，都需要创建对应的第三方公台板对象并加入到`dataChange()`方法中，既不是动态加入，也不利于维护

### 3、观察者模式介绍

观察者模式是一种**行为型设计模式（**用于描述对象之间的通信和责任分配**）**，它定义了对象之间**一对多**的依赖关系，使得当主题对象状态发生改变时，所有依赖于它的观察者对象都能够**得到通知并自动更新**。该模式的核心是抽象对象与观察者之间的耦合度达到了最小化，从而使系统更加灵活且易于扩展。

**实现方法：**

主题对象实现主题接口的注册、移除、通知方法，并管理资源和观察者列表；

观察者对象实现观察者接口的更新方法，并管理资源；

主题对象通知方法：遍历观察者列表执行更新方法。

在观察者模式中，**主题对象**（也称为被观察者）**维护一个观察者列表**，并提供方法用于**添加、删除和通知观察者**。当主题状态发生改变时，它会遍历观察者列表并调用每个观察者的更新方法，从而通知它们状态已经改变。

主题接口：有注册、移除和通知功能；

主题实现类：实现主题接口，管理资源和观察者列表；

观察者接口：发起更新资源请求；

观察者实现类：发起更新资源请求、使用资源

**优点：**

1. **降低了对象之间的耦合度**，因为主题对象不需要知道观察者的具体实现，只需要知道观察者实现了一个特定接口即可。

2. 可以**动态扩展观察者列表**，方便灵活。

3. 实现了对象之间的**一对多**依赖关系，提高了系统的**可维护性和可重用性**。遵守了 **ocp 原则（开闭原则**：对扩展开放，对修改关闭）。

**缺点**：

1. 当观察者过多时，通知过程需要花费较多的时间，会影响系统的性能。

2. 如果观察者与主题对象之间存在**循环依赖**，可能会出现**死循环**。

观察者模式在 Java 中的应用非常广泛，例如 Swing 中的 Listener、Servlet 中的 Listener、Spring 中的事件监听、JDK 的 Observable 等等。

### 4、观察者模式优化天气预报案例

Subject 接口：主体接口，有注册、移除和通知功能；

WeatherData 类：主体实现类，实现 Subject 接口，聚合观察者列表，管理天气信息和观察者列表；

Observer 接口：观察者接口，发起更新天气信息请求；

CurrentCondition 类：观察者实现类，发起更新天气信息请求和使用天气

![](<assets/1740030885752.png>)

`**主题Subject**`

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

 **观察者对象 Observer**

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

**调用测试**

```
// 创建气象网站对象
CurrentConditions currentConditions = new CurrentConditions();
// 创建气象数据对象
WeatherData weatherData = new WeatherData();
// 注册气象网站对象
weatherData.registerObserver(currentConditions);
// 天气发生变化时，更新最新的气象数据
weatherData.setData(10f, 150f, 40f);
//============最新天气============
//*** 当前温度：10.0 ℃ ***
//*** 当前气压：150.0 kPa ***
//*** 当前湿度：40.0 %RH ***
```

**观察者模式的好处**

*   1）观察者模式设计后，会以集合的方式来管理用户`Observer`，包括注册、移除和通知

*   2）这样，我们**增加观察者**（这里可以理解成一个新的公告板），就不需要去修改核心类`WeatherData`不会修改代码，遵守了 ocp 原则（开闭原则）

例如，我们新增`SinaWebSite`和`BaiDuWebSite`两个三方网站，接口气象局。此时三方只需实现相应接口即可，`WeatherData`不需要有任何的改变

```
/**
 * 新增的三方观察者对象——新浪网
 */
public class SinaWebSite implements Observer {
    private Float temperature;
    private Float pressure;
    private Float humidity;
 
    /**
     * 更新天气情况，通过推送的方式，由 WeatherData 调用
     *
     * @param temperature
     * @param pressure
     * @param humidity
     */
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
        System.out.println("============新浪网-最新天气============");
        System.out.println("*** 新浪网-当前温度：" + this.temperature + " ℃ ***");
        System.out.println("*** 新浪网-当前气压：" + this.pressure + " kPa ***");
        System.out.println("*** 新浪网-当前湿度：" + this.humidity + " %RH ***");
    }
}
/**
 * 新增的三方观察者对象——百度网
 */
public class BaiDuWebSite implements Observer {
    private Float temperature;
    private Float pressure;
    private Float humidity;
 
    /**
     * 更新天气情况，通过推送的方式，由 WeatherData 调用
     *
     * @param temperature
     * @param pressure
     * @param humidity
     */
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
        System.out.println("============百度网-最新天气============");
        System.out.println("*** 百度网-当前温度：" + this.temperature + " ℃ ***");
        System.out.println("*** 百度网-当前气压：" + this.pressure + " kPa ***");
        System.out.println("*** 百度网-当前湿度：" + this.humidity + " %RH ***");
    }
}
```

**调用测试**

```
// 新增三方气象网站，只需注册即可
weatherData.registerObserver(new SinaWebSite());
weatherData.registerObserver(new BaiDuWebSite());
// 天气发生变化时，更新最新的气象数据
weatherData.setData(15f, 120f, 80f);
//============最新天气============
//*** 当前温度：15.0 ℃ ***
//*** 当前气压：120.0 kPa ***
//*** 当前湿度：80.0 %RH ***
//============新浪网-最新天气============
//*** 新浪网-当前温度：15.0 ℃ ***
//*** 新浪网-当前气压：120.0 kPa ***
//*** 新浪网-当前湿度：80.0 %RH ***
//============百度网-最新天气============
//*** 百度网-当前温度：15.0 ℃ ***
//*** 百度网-当前气压：120.0 kPa ***
//*** 百度网-当前湿度：80.0 %RH ***
```

当三方网站不再需要时，只要做相应的移除即可

```
// 移除气象网站
weatherData.removeObserver(currentConditions);
weatherData.setData(20f, 160f, 30f);
//============新浪网-最新天气============
//*** 新浪网-当前温度：20.0 ℃ ***
//*** 新浪网-当前气压：160.0 kPa ***
//*** 新浪网-当前湿度：30.0 %RH ***
//============百度网-最新天气============
//*** 百度网-当前温度：20.0 ℃ ***
//*** 百度网-当前气压：160.0 kPa ***
//*** 百度网-当前湿度：30.0 %RH ***
```

### 5、JDK 的 Observable 类和 Observer 类

JDK 提供观察者模式基础功能的主题抽象类和观察者接口：

**Observable 抽象类** 

JDK 中的 Observable 抽象类可以**作为实现观察者模式的一种工具**，**用于构建主题**（被观察者）**对象**，并且可以将多个观察者对象添加到主题中。当主题发生变化时，通过调用 Observable 类的 notifyObservers() 方法，可以通知所有的观察者对象进行更新，从而实现一对多依赖关系。

Observable 类的主要作用是**简化**观察者模式的实现过程，**将观察者模式的基础部分已经实现**，程序员只需要编写具体的业务逻辑即可。addObserver()、deleteObserver() 和 notifyObservers()。

但是需要注意的是，JDK 中的 Observable 类并**不是非常灵活和易于扩展**，它**只提供了简单的实现方式**。因此，在实际的项目中，我们有时会采用其他的方案来实现观察者模式，例如使用事件模型、Spring 框架中的 ApplicationEvent 等。

总之，JDK 中的 Observable 类可以作为一种工具来支持观察者模式的实现，它简化了观察者模式的编写，提高了代码的可读性和可维护性。但是在实际应用中，我们需要根据实际情况选择最适合的实现方案。

**Observer 类**

Observer 即观察者接口，具有 update() 方法  

```
public class Observable {
    private boolean changed = false;
    private Vector<Observer> obs = new Vector();
 
    public Observable() {
    }
 
    public synchronized void addObserver(Observer var1) {
        if (var1 == null) {
            throw new NullPointerException();
        } else {
            if (!this.obs.contains(var1)) {
                this.obs.addElement(var1);
            }
 
        }
    }
 
    public synchronized void deleteObserver(Observer var1) {
        this.obs.removeElement(var1);
    }
 
    public void notifyObservers() {
        this.notifyObservers((Object)null);
    }
    //...
}
 
public interface Observer {
    void update(Observable o, Object arg);
}
```