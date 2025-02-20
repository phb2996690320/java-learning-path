---
url: https://blog.csdn.net/qq_40991313/article/details/134162179
title: 【设计模式】结合 StringBuilder 源码，探析建造者模式的特性和应用场景_stringbuilder 建造者模式 - CSDN 博客
date: 2025-02-20 13:54:42
tag: 
summary: 
---
**导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 黑马旅游 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/126646289 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/黑马旅游/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码-CSDN博客")

**目录**

[一、经典的盖房子问题](#t0)

[二、传统方案盖房子](#t1)

[2.1 实现方案：产品和创建产品过程耦合](#t2)

[2.2 优缺点和改进思路](#t3)

[三、建造者模式 / 生成器模式](#t4)

[3.1 基本介绍](#t5)

[3.2 四个角色](#t6)

[3.3 优缺点和使用场景](#t7)

[3.4 建造者模式和抽象工厂模式的区别](#t8)

[四、建造者模式盖房子](#t9)

[4.1 UML 类图](#t10)

[4.2 具体代码](#t11)

[4.2.1 产品类](#t12) 

[4.2.2 抽象建造者](#t13) 

[4.2.3 具体建造者](#t14)

[4.2.4 指挥者](#t15)

[4.2.5 测试类](#t16)

[五、StringBuilder 中的建造者模式](#t17)

[5.1 JDK 源码中的建造者模式](#t18)

[5.2 角色分析](#t19)

[5.2.1 产品：char 数组](#t20)

[5.2.2 抽象建造者：AbstractStringBuilder](#t21)

[5.2.3 具体建造者：重写 append() 返回类型](#t22)

[5.2.3.1 StringBuilder](#StringBuilder)

[5.2.3.2 StringBuffer](#StringBuffer)

[5.2.3.3 扩展：String、StringBuffer、Stringbuilder 有什么区别](#%E6%89%A9%E5%B1%95%EF%BC%9AString%E3%80%81StringBuffer%E3%80%81Stringbuilder%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB)

[5.2.3.4 扩展：为什么 StringBuffer 是线程安全的](#%E6%89%A9%E5%B1%95%EF%BC%9A%E4%B8%BA%E4%BB%80%E4%B9%88StringBuffer%E6%98%AF%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8%E7%9A%84)

[5.2.4 指挥者：StringBuilder](#t23) 

## 一、经典的盖房子问题

**问题描述：**

*   **建房子过程：**打桩、砌墙、封顶。虽然建造过程一样，但实际造的房子有差别，因为房子有各种各样的，比如普通房，高楼，别墅。
*   **需求：**写代码能建造各类房子

## 二、传统方案盖房子

### 2.1 实现方案：产品和创建产品过程耦合

创建以下几个类：

*   AbsHouse（抽象盖房类）：包含打桩、砌墙、封顶三个方法；
*   NormalRoom（普通房间类）、Villa（别墅类）：继承抽象盖房类，根据情况重写三个方法；

![](<assets/1740030882634.png>)

抽象盖房类：包含打桩、砌墙、封顶三个方法； 

```
/**
 * @Author: vince
 * @CreateTime: 2024/09/10
 * @Description: 抽象房间类
 * 包含打桩、砌墙、封顶
 * @Version: 1.0
 */
public abstract class AbsHouse {
    /**
     * 打桩
     */
    protected abstract void piling();
 
    /**
     * 砌墙
     */
    protected abstract void walling();
 
    /**
     * 封顶
     */
    protected abstract void capping();
 
    /**
     * 盖房子
     */
    public void build() {
        piling();
        walling();
        capping();
    }
}
```

NormalRoom（普通房间类）、Villa（别墅类）：继承抽象盖房类，根据情况重写三个方法；

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/12
     * @Description: 普通房 
     * @Version: 1.0
     */
    public class NormalRoom extends AbsHouse {
        @Override
        protected void piling() {
            System.out.println("普通房打桩...");
        }
 
        @Override
        protected void walling() {
            System.out.println("普通房砌墙...");
        }
 
        @Override
        protected void capping() {
            System.out.println("普通房封顶...");
        }
    }
 
    /**
     * @Author: vince
     * @CreateTime: 2024/10/12
     * @Description: 高楼
     * @Version: 1.0
     */
    public class HighRise extends AbsHouse {
        @Override
        protected void piling() {
            System.out.println("高楼打桩...");
        }
 
        @Override
        protected void walling() {
            System.out.println("高楼砌墙...");
        }
 
        @Override
        protected void capping() {
            System.out.println("高楼封顶...");
        }
    }
 
    /**
     * @Author: vince
     * @CreateTime: 2024/10/12
     * @Description: 别墅
     * @Version: 1.0
     */
    public class Villa extends AbsHouse {
        @Override
        protected void piling() {
            System.out.println("别墅打桩...");
        }
 
        @Override
        protected void walling() {
            System.out.println("别墅砌墙...");
        }
 
        @Override
        protected void capping() {
            System.out.println("别墅封顶...");
        }
    }
```

### 2.2 优缺点和改进思路

**优点：**

*   **简单：**比较好理解，简单易操作

**缺点：**

*   **产品和创建产品过程耦合：**没有设计**缓存层对象**，程序的扩展和维护不好。也就是说，这种设计方案把产品（即：房子）和创建产品的过程（即：建房子流程）封装在一起，耦合性太高。

**改进思路分析：** 

*   **解耦：**使用建造者模式，将产品和产品建造过程解耦。

## 三、建造者模式 / 生成器模式

### 3.1 基本介绍

**建造者模式（Builder Pattern）**：使用多个步骤来创建一个复杂对象，而不是在一个构造函数或工厂方法中直接返回该对象。它将产品和产品建造过程进行了解耦。

建造者模式又叫生成器模式，是一种创建型设计模式。

**特点：**

*   分步骤创建对象：对象的构建是分多个步骤进行的，而不是直接使用构造方法 new 对象，适用于需要分阶段构造的复杂对象。
*   同一个建造过程：同样的构建过程可以创建不同的对象表示（不同的配置组合）。

### 3.2 四个角色

建造者模式有四个角色： 

*   **Product（产品）**：一个包含多个部件的类。
    *   每个部件是一个成员变量。例如房间包括地基、墙和屋顶等组件，又例如电脑包括 CPU、内存条、主板等组件。
*   **Builder（抽象建造者）**：一个包含产品变量及其所有部件建造方法的抽象类或接口
    *   包含每个部件的建造方法，和一个返回产品的 build() 方法。例如房间的打桩、砌墙、封顶过程。
*   **ConcreteBuilder（具体建造者）**：实现抽象建造者所有抽象建造方法
*   **Director（指挥者）**：一个以抽象建造者为变量的、包含建造方法的类
    *   包含一个抽象建造者变量，和一个建造并返回产品的 build() 方法。
    *   实际建造产品时，先创建一个指挥者对象，然后设置它的建造者变量，最后调用它的 build() 方法返回产品。

### 3.3 优缺点和使用场景

**优点：**

*   **耦合性降低**：
    *   **产品与建造解耦**：客户端（使用程序）不必知道产品内部组成的细节，将产品本身与产品的创建过程解耦，使得相同的创建过程可以创建不同的产品对象
    *   **具体建造者之间解耦**：每一个具体建造者都相对独立，而与其他的具体建造者无关，因此可以很方便地替换具体建造者或增加新的具体建造者，用户使用不同的具体建造者即可得到不同的产品对象
*   **代码可读性高**：可以更加精细地控制产品的创建过程。将复杂产品的创建步骤分解在不同的方法中，使得创建过程更加清晰，也更方便使用程序来控制创建过程
*   **开闭原则**：增加新的具体建造者无须修改原有类库的代码，指挥者类针对抽象建造者类编程，系统扩展方便，符合 “开闭原则”，即代码对修改不开放，而对扩展开放。

**缺点：**

*   **代码复杂性**：没了解过建造者模式的人阅读代码更困难，这也是设计模式的通用缺点。
*   **过度设计风险**：建造者模式只适合复杂产品对象，太简单的产品对象则没必要使用，或者建造方式只有一种的产品，都没必要使用。这也是设计模式的通用缺点。

**使用场景**：

*   **复杂产品对象**：部件比较多的产品，例如房屋有墙、屋顶、地基、横梁等等部件。
*   **建造方式多样**：例如房屋对于同一些材料，可以建成普通房屋、高楼等等。
*   **建造产品共同点**：建造者模式所创建的产品一般具有较多的共同点，其组成部分相似，如果产品之间的差异性很大，则不适合使用建造者模式，因此其使用范围受到一定的限制

**不适用场景**：

*   简单产品对象；
*   建造方式单一；

### 3.4 建造者模式和抽象工厂模式的区别

**区别：** 

*   **抽象工厂模式**：适用于**不同产品**的情况。用来创建不同但是相关类型的对象（继承同一父类或者接口的一组子类）。
*   **建造者模式**：适用于**一个产品有多个部件**的情况。用来创建一种类型的复杂对象，通过设置不同的可选参数，“定制化” 地创建不同的对象。

**示例**：顾客走进一家餐馆点餐

*   **工厂模式**：根据用户不同的选择，来制作**不同的食物**，比如披萨、汉堡、沙拉。
*   **建造者模式**：对于披萨来说，用户又有各种配料可以定制，我们通过建造者模式根据用户选择的**不同配料**来制作披萨。

## 四、建造者模式盖房子

### **4.1 UML 类图**

![](<assets/1740030882999.png>)

### **4.2 具体代码**

#### 4.2.1 产品类 

产品类包含多个部件的对象。  
每个部件是一个成员变量。例如房间包括地基、墙和屋顶等组件，又例如电脑包括 CPU、内存条、主板等组件。

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/12
     * @Description: 产品：房间类，包含多个部件
     * @Version: 1.0
     */
    @Data
    public class House {
        /**
         * 地基
         */
        private String pile;
        /**
         * 墙
         */
        private String wall;
        /**
         * 屋顶
         */
        private String roof;
 
        // getter和setter方法...
    }
```

#### 4.2.2 抽象建造者 

抽象建造者是个抽象类，包含每个部件的建造方法，和一个返回产品的 build() 方法  
例如房间的打桩、砌墙、封顶过程。

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/12
     * @Description: 抽象建造者：房间建造抽象类
     * 包含每个部件的抽象建造方法，和一个build()方法返回产品
     * @Version: 1.0
     */
    public abstract class HouseBuilder {
        /**
         * 产品对象
         */
        private House house = new House();
 
        /**
         * 打地基
         */
        public abstract void piling();
 
        /**
         * 砌墙
         */
        public abstract void walling();
        /**
         * 封顶
         */
        public abstract void capping();
 
        /**
         * build()方法返回产品
         * @return {@link House }
         */
        public House build() {
            return house;
        }
    }
```

#### 4.2.3 具体建造者

具体建造者是抽象建造者的实现类，实现各个部件具体构建逻辑。

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/14
     * @Description: 具体建造者：普通房屋建造者
     * @Version: 1.0
     */
    public class NormalRoomBuilder extends HouseBuilder {
        @Override
        public void piling() {
            System.out.println("普通房打桩...");
        }
 
        @Override
        public void walling() {
            System.out.println("普通房砌墙...");
        }
 
        @Override
        public void capping() {
            System.out.println("普通房封顶...");
        }
    }
    public class HighRiseBuilder extends HouseBuilder {
        @Override
        public void piling() {
            System.out.println("高楼打桩...");
        }
 
        @Override
        public void walling() {
            System.out.println("高楼砌墙...");
        }
 
        @Override
        public void capping() {
            System.out.println("高楼封顶...");
        }
    }
```

#### 4.2.4 指挥者

指挥类包含一个抽象建造者变量，和一个建造并返回产品的 build() 方法。

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/14
     * @Description: 指挥者：负责建造并返回产品
     * @Version: 1.0
     */
    public class HouseDirector {
        /**
         * 抽象建造者变量
         */
        private HouseBuilder houseBuilder;
 
        public HouseDirector() {
        }
 
        public void setHouseBuilder(HouseBuilder houseBuilder) {
            this.houseBuilder = houseBuilder;
        }
 
        /**
         * 建房子方法
         * @return {@link House }
         */
        public House buildHouse() {
            // 调用建造者变量的各部门建造方法，然后建造返回
            houseBuilder.piling();
            houseBuilder.walling();
            houseBuilder.capping();
            return houseBuilder.build();
        }
    }
```

#### 4.2.5 测试类

**步骤：**

1.  **创建指挥者**：创建一个指挥者对象
2.  **设置建造者**：设置它的建造者变量为具体建造者对象
3.  **返回产品**：调用它的 build() 方法返回产品。

**代码**： 

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/14
     * @Description: 建造者测试类
     * @Version: 1.0
     */
    public class BuilderTest {
        public static void main(String[] args) {
            // 1.创建指挥者对象
            HouseDirector houseDirector = new HouseDirector();
            House house;
            // 2.用指挥者对象的setter方法设置建造者变量
            houseDirector.setHouseBuilder(new NormalRoomBuilder());
            // 3.指挥者对象的build()方法获取产品
            house = houseDirector.buildHouse();
 
            // 4.建造高楼
            houseDirector.setHouseBuilder(new HighRiseBuilder());
            house = houseDirector.buildHouse();
        }
    }
```

## 五、StringBuilder 中的建造者模式

### 5.1 JDK 源码中的建造者模式

StringBuilder 不是严格的建造者模式，但是使用了建造者模式思想。

JDK 中，不止 StringBuilder、StringBuffer 使用了建造者模式，还有：

stream 流

```
// 1.过滤只保留长度为3的字符串，收集成List<String>类型
        List<String> ansList = list.stream().filter(item -> item.length() == 3).collect(Collectors.toList());
```

时间 API：

```
LocalDate date = LocalDate.of(2023, Month.JULY, 4);
LocalDateTime dateTime = LocalDateTime.of(2023, 7, 4, 10, 30);
```

### 5.2 角色分析

#### 5.2.1 产品：char 数组

char 数组，数组可以包含多个元素，每个元素是一个部件。

![](<assets/1740030883305.png>)

#### 5.2.2 抽象建造者：AbstractStringBuilder

AbstractStringBuilder 包含 append()、delete() 等方法，用来对产品（字符数组）里的部件（数组内每个元素）进行追加和删除。

**部件组装方法**：append() 、delete()

以 append() 为例：

**核心流程**：

1.  **判空**：如果传入的字符串为 null，则不追加
2.  **校验扩容**：若新容量大于当前数组长度，则进行扩容
3.  **拷贝数组**：校验数组越界后，调用 System.arraycopy() 拷贝数组

**具体代码**： 

```
/**
 * @Author: vince
 * @CreateTime: 2024/10/15
 * @Description: 抽象建造者：简化版AbstractStringBuilder
 * @Version: 1.0
 */
abstract class AbstractStringBuilder implements Appendable, CharSequence {
    /**
     * 产品：一个字符数组
     */
    char[] value;
 
    /**
     * 实际字符串长度
     */
    int count;
 
    /**
     * 产品部件建造方法：追加字符串
     * 这个方法不一定必须是抽象方法，子类可以重写        
     * @param str
     * @return {@link AbstractStringBuilder }
     */
    public AbstractStringBuilder append(String str) {
        // 1.判空：如果传入的字符串为null，则不追加
        if (str == null)
            return appendNull();
        int len = str.length();
        // 2.校验扩容：若新容量大于当前数组长度，则进行扩容
        ensureCapacityInternal(count + len);
        // 3.拷贝数组
        str.getChars(0, len, value, count);
        // 调整字符串长度：给count变量加上追加字符串长度
        count += len;
        return this;
    }
 
    /**
     * 扩容底层数组
     * @param minimumCapacity 新容量
     */
    private void ensureCapacityInternal(int minimumCapacity) {
        // 检查是否需要扩容，若新容量大于当前数组长度，则进行扩容
        if (minimumCapacity - value.length > 0) {
            value = Arrays.copyOf(value,
                    newCapacity(minimumCapacity));
        }
    }
    public void getChars(int srcBegin, int srcEnd, char dst[], int dstBegin) {
        // 检查源开始索引是否合法
        if (srcBegin < 0) {
            throw new StringIndexOutOfBoundsException(srcBegin);
        }
        // 检查源结束索引是否合法
        if (srcEnd > value.length) {
            throw new StringIndexOutOfBoundsException(srcEnd);
        }
        // 检查开始索引是否大于结束索引
        if (srcBegin > srcEnd) {
            throw new StringIndexOutOfBoundsException(srcEnd - srcBegin);
        }
        // 使用System.arraycopy复制字符
        System.arraycopy(value, srcBegin, dst, dstBegin, srcEnd - srcBegin);
    }
        // ...
}
```

#### 5.2.3 具体建造者：重写 append() 返回类型

##### 5.2.3.1 StringBuilder

具体建造者 StringBuilder 继承了 AbstractStringBuilder，并重写了 append() 方法。

append() 逻辑都是用父类的 append() 方法，主要重写的地方在于**返回类型由父类 AbstractStringBuilder 改成了子类 StringBuilder**

**重写规则：**重写时

*   **返回类可以是原返回类的子类。**例如工厂方法设计模式里，抽象工厂类的 createObject() 方法返回值是抽象产品类，具体工厂类的 createObject() 方法返回类是具体产品类
*   访问权限不能比其父类更为严格
*   抛出异常不能比父类更广泛

![](<assets/1740030883528.png>)

 

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/15
     * @Description: 具体建造者：简化版StringBuilder
     * @Version: 1.0
     */
    public final class StringBuilder
            extends AbstractStringBuilder
            implements Serializable, CharSequence {
        /**
         * 追加字符串
         * @param str
         * @return {@link java.lang.StringBuilder }
         */
        @Override
        public java.lang.StringBuilder append(String str) {
            super.append(str);
            return this;
        }
        // ...
    }
```

##### 5.2.3.2 StringBuffer

 相比 StringBuilder 的 append() 方法，主要修改了返回值，方法内第一步清空 toStringCache 变量

![](<assets/1740030883798.png>)

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/15
     * @Description: 具体建造者：简化版StringBuffer
     * @Version: 1.0
     */
    public final class StringBuffer
            extends AbstractStringBuilder
            implements Serializable, CharSequence {
        /**
         * 缓存上一次调用toString()方法返回的值。
         * 每当StringBuffer被修改时，这个缓存会被清除。
         * transient关键字表明此字段不会被序列化。
         */
        private transient char[] toStringCache;
 
        /**
         * 追加字符串
         * 特点：
         *  1.方法加了synchronized锁，保证线程安全
         *  2.返回值是StringBuffer
         *  3.方法内第一步清空toStringCache
         * @param str
         * @return {@link java.lang.StringBuffer }
         */
        @Override
        public synchronized java.lang.StringBuffer append(String str) {
            // 缓存上一次调用toString()方法返回的值，所以每次更新底层字符数组时，都需要清空；
            toStringCache = null;
            super.append(str);
            return this;
        }
 
        /**
         * 获取实际字符串长度
         * StringBuffer线程安全的原因：有线程同步风险的方法都加了synchronized锁，锁的粒度是当前实例
         * @return int
         */
        @Override
        public synchronized int length() {
            return count;
        }
 
        @Override
        public synchronized int capacity() {
            return value.length;
        }
    }
```

##### 5.2.3.3 扩展：String、StringBuffer、Stringbuilder 有什么区别

**得分点**

是否可变、复用率、效率、线程安全问题

**标准回答**

**String:** 不可变字符序列，效率低，但是复用率高、线程安全。

*   **不可变**：指 String 对象创建之后, 直到这个对象销毁为止, 对象中的字符序列都不能被改变。
*   **复用率高**：指 String 类型对象创建出来后归常量池管，可以随时从常量池调用同一个 String 对象。StringBuffer 和 StringBuider 在创建对象后一般要转化成 String 对象才调用。

**StringBuffer 和 StringBuilder**StringBuffer 和 Stringbuilder 都是字符序列可变的字符串，方法也一样，有共同的父类 AbstractStringBuilder。 

*   **StringBuilder:** 可变字符序列、**效率最高、线程不安全**
*   **StringBuffer:** 可变字符序列、效率较高 (增删)、**线程安全**

##### 5.2.3.​​​​​​​4 **扩展：为什么 StringBuffer 是线程安全的**

因为有线程同步风险的方法（例如 length()、append()、delete() 等）都加了 synchronized 锁，锁的粒度是当前实例。

![](<assets/1740030884176.png>)

**synchronized 关键字作用于三个位置：**

 1. 作用在静态方法上, 则锁是当前类的 Class 对象。

 2. 作用在普通方法上, 则锁是当前的实例（this）。

 3. 作用在代码块上, 则需要在关键字后面的小括号里, 显式指定锁对象，例如 this、Xxx.class。

**为什么 StringBuffer 效率低？**

1.  **锁本身效率低**：在存在线程竞争情况下，加锁的代码块本身就比不加锁要慢很多，因为锁内的共享资源同一时刻只能一个线程访问。
2.  **锁粒度大**：锁的粒度是当前实例，直接给整个方法加 synchronized，而不是具体需要在有同步风险的代码块上加锁，性能低。这个方法内部分共享资源可能是线程安全的，并不需要加锁。

#### 5.2.4 指挥者：StringBuilder 

StringBuilder 和 StringBuffer 自身既是具体建造者，也是指挥者。

实际开发中，也不是一定需要套范例，而是应该更多地理解范例中的设计模式思想，运用到代码中。