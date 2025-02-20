---
url: https://blog.csdn.net/qq_40991313/article/details/134039365
title: 【阿里规约】阿里开发手册解读——命名规范篇_阿里命名规范 - CSDN 博客
date: 2025-02-20 13:56:46
tag: 
summary: 
---
**导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**阿里规约 PDF：**

[阿里巴巴开发手册. pdf - 蓝奏云](https://wwmg.lanzouj.com/iBGjO1ctw3sb "阿里巴巴开发手册.pdf - 蓝奏云")

**目录**

[一、包名](#t0)

[二、类名](#t1)

[2.1 普通类和方法](#t2)

[2.2 测试类](#t3)

[2.3 抽象类](#t4)

[2.4 异常类](#t5)

[2.5 枚举类](#t6)

[2.6 接口和实现类](#t7)

[2.7 PO、DTO、VO](#t8)

[2.8 用到设计模式的类](#t9)

[2.8.1 概述](#t10)

[2.8.2 案例一：工厂设计模式采购披萨](#t11)

[2.8.3 案例二： 观察者设计模式订阅天气](#t12)

[三、变量](#t13)

[3.0 普通变量](#t14)

[3.1 数组](#t15)

[3.2 布尔型变量](#t16)

[3.3 父子类变量](#t17)

[3.4 局部变量](#t18)

[四、常量](#t19)

[4.1 普通常量](#t20)

[4.2 Long 常量](#t21)

## 一、包名

*   **统一小写：**例如商品库存包 com.example.product.stock，而不是 com.example.productStock
*   **分隔符间单词必须单语义：**例如商品库存包 com.example.product.stock，而不是 com.example.product_stock、com.example.product-stock

**参考：**

【强制】 包名统一使用小写，点分隔符之间有且仅有一个自然语义的英语单词。包名统一使

用 单数 形式，但是类名如果有复数含义，类名可以使用复数形式。

正例： 应用工具类包名为 com.alibaba.ai.util、类名为 MessageUtils（此规则参考 spring 的框架结构）

## 二、类名

### 2.1 普通类和方法

*   驼峰命名：例如 UserService，而不是 User_service。
*   禁止拼音英文混用；
*   禁止下划线、美元符号起始。
*   **可读性比长度重要：**命名尽可能短，但 “望文生义” 更重要
    *   **可读性高（推荐）：**下面这些类既保证了望文生义，又保证了缩写
        *   商品描述类：DescriptionOfProduct 缩写为 ProductDesc
        *   应用程序配置类：ApplicationConfiguration 缩写为 AppConfig
        *   客户信息类：CustomerInformation 缩写为 CustInfo。
    *   **可读性低（不推荐）：**
        *   AbstractClass“缩写” 命名成 AbsClass；
        *   PurchaseProduct 类缩写成 PurProduct;

**参考：**

【强制】 代码中的命名均不能以 下划线或美元符号 开始，也不能以 下划线或美元符号 结束。

反例： _name / __name / $name / name_ / name$ / name__

【强制】 代码中的命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式。

说明： 正确的英文拼写和语法可以让阅读者易于理解，避免歧义。注意，纯拼音命名方式更要避免采用。

正例： renminbi / alibaba / taobao / youku / hangzhou 等国际通用的名称，可视同英文。

反例： DaZhePromotion [打折] / getPingfenByName() [评分] / int 某变量 = 3

【强制】 方法名、参数名、成员变量、局部变量都统一使用 lowerCamelCase 风格，必须遵

从驼峰形式。

正例： localValue / getHttpMessage() / inputUserId

【强制】 杜绝完全不规范的缩写，避免望文不知义。

反例： AbstractClass“缩写” 命名成 AbsClass；condition“缩写” 命名成 condi，此类随意缩写严重

降低了代码的可阅读性。

【推荐】 为了达到代码自解释的目标，任何自定义编程元素在命名时，使用尽量完整的单词

组合来表达其意。

正例： 在 JDK 中，表达原子更新的类名为：AtomicReferenceFieldUpdater。

反例： int a 的随意命名方式。

【推荐】 在常量与变量的命名时，表示类型的名词放在词尾，以提升辨识度。

正例： startTime / workQueue / nameList / TERMINATED_THREAD_COUNT

反例： startedAt / QueueOfWork / listName / COUNT_TERMINATED_THREAD

### **2.2 测试类**

**命名：**待测类名 + Test 。

**举例：**用户测试类 UserServiceTest，而不是 TestUserService。

**参考：**

【强制】 抽象类命名使用 Abstract 或 Base 开头；异常类命名使用 Exception 结尾；测试类

命名以它要测试的类的名称开始，以 Test 结尾

### 2.3 抽象类

**命名：** Abstract + 自定义类名。

**举例：**抽象支付方式类 AbstractPaymentMethod/BasePaymentMethod，而不是 AbstractPayment。

**参考：**

【强制】 抽象类命名使用 Abstract 或 Base 开头；异常类命名使用 Exception 结尾；测试类

命名以它要测试的类的名称开始，以 Test 结尾

### 2.4 异常类

**命名：**自定义异常名 +Exception。

例如商品找不到类 ProductNotFoundException，而不是 ProductNotFound。

**参考：**

【强制】 抽象类命名使用 Abstract 或 Base 开头；异常类命名使用 Exception 结尾；测试类

命名以它要测试的类的名称开始，以 Test 结尾

### 2.5 枚举类

*   **类名：**自定义类名 +Enum。 例如 ProcessStatusEnum
    
*   **成员名：**跟常量一样
    
    *   **纯大写 + 下划线：**例如最大库存量 MAX_STOCK_COUNT。
    *   **可读性比长度重要：**常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长。例如最大库存量 MAX_STOCK_COUNT 而不是 MAX_COUNT。

**枚举类和常量类区别：**

**相同点：**在 Java 中，枚举和常量都用于表示一组固定的值，但它们适用的场景有所不同。

**不同点：**

*   枚举（Enum）适用于表示**一组有限的可能取值**，这些取值在程序中某个上下文中具有特殊意义。例如，表示一周的星期几或者表示某个颜色的枚举类型。枚举类型可以通过限定的值范围提供更好的类型安全性，并且可以使用 switch 语句进行更清晰的代码编写。
*   常量（Constant）适用于表示在程序中经常使用的不可变的值。常量一般定义为 final 类型，并且通常用于表示某个固定的数值或者字符串常量。常量的值在程序中是不可修改的，可以通过常量名直接使用，提高代码的可读性和可维护性，但是可能会降低类型安全性和表达能力。

总结来说，枚举适用于表示一组有限的、具有特殊意义的取值，常量适用于表示程序中经常使用的不可变的值。

**参考：**

【参考】 枚举类名带上 Enum 后缀，枚举成员名称需要全大写，单词间用下划线隔开。

说明： 枚举其实就是特殊的类，域成员均为常量，且构造方法被默认强制是私有。

正例： 枚举名字为 ProcessStatusEnum 的成员名称：SUCCESS / UNKNOWN_REASON

### 2.6 接口和实现类

*   service 和 dao 实现类以 Impl 结尾。注意不要用 Imp 结尾。例如 UserServiceImpl 而不是 UserServiceImp
*   能力型接口以 - able 结尾。例如异常根类 Throwable 是可抛出的，实现类 Exception 和 Error 都可抛出。又例如 Drawable 接口和 Circle 实现类。

**参考：**

接口和实现类的命名有两套规则：

1） 【强制】 对于 Service 和 DAO 类，基于 SOA 的理念，暴露出来的服务一定是接口，内部的实现类用

Impl 的后缀与接口区别。

正例： CacheServiceImpl 实现 CacheService 接口。

2） 【推荐】 如果是形容能力的接口名称，取对应的形容词为接口名（通常是–able 的形容词）。

正例： AbstractTranslator 实现 Translatable 接口。

### **2.7 PO、DTO、VO**

*   **DO / BO / DTO / VO / AO / PO / UID 要大写，而非驼峰：**例如 UserDO，而不是 UserDo。例如 serialVersionUID，而不是 serialVersionUid。
*   **禁止 POJO 后缀：**POJO 是 DO/DTO/BO/VO 的统称，禁止命名成 xxxPOJO。

**扩展：PO、DTO、VO**

1） 数据对象：xxxDO，xxx 即为数据表名。

2） **DTO 数据传输对象：**xxxDTO，xxx 为业务领域相关的名称。

3） **VO 展示对象：**xxxVO，xxx 一般为网页名称。

4） POJO 是 DO/DTO/BO/VO 的统称，禁止命名成 xxxPOJO。

**参考：**

【强制】 类名使用 UpperCamelCase 风格，但以下情形例外：DO / BO / DTO / VO / AO

/ PO / UID 等。

正例： JavaServerlessPlatform / UserDO / XmlService / TcpUdpDeal / TaPromotion

反例： javaserverlessplatform / UserDo / XMLService / TCPUDPDeal / TAPromotion

【参考】 各层命名规约：

A) Service/DAO 层方法命名规约

1） 获取单个对象的方法用 get 做前缀。

2） 获取多个对象的方法用 list 做前缀，复数形式结尾如：listObjects。

3） 获取统计值的方法用 count 做前缀。

4） 插入的方法用 save/insert 做前缀。

5） 删除的方法用 remove/delete 做前缀。

6） 修改的方法用 update 做前缀。

### 2.8 用到设计模式的类

#### 2.8.1 概述

*   抽象类名以 Abstarct/Base 为前缀
*   用到设计模式时要在命名中进行体现。

**参考：**

【强制】 抽象类命名使用 Abstract 或 Base 开头；异常类命名使用 Exception 结尾；测试类

命名以它要测试的类的名称开始，以 Test 结尾

【推荐】 如果模块、接口、类、方法使用了设计模式，在命名时需体现出具体模式。

说明： 将设计模式体现在名字中，有利于阅读者快速理解架构设计理念。

正例： public class OrderFactory;

public class LoginProxy;

public class ResourceObserver;

#### 2.8.2 **案例一：**工厂设计模式采购披萨

采购披萨工厂类：利用工厂方法模式创建披萨时，可以把抽象工厂类命名为 AbstractOrderPizzaFactory 类。

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

**参考：**

[设计模式——工厂模式_工厂设计模式_程序员小海绵【vincewm】的博客 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/130433828?spm=1001.2014.3001.5502 "设计模式——工厂模式_工厂设计模式_程序员小海绵【vincewm】的博客-CSDN博客")

#### 2.8.3 案例二： 观察者设计模式订阅天气

天气局接口和观察者类：WeatherSubject 和天气观察者接口 WeatherObserver

**参考：**

[设计模式——观察者模式_程序员小海绵【vincewm】的博客 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/130451520 "设计模式——观察者模式_程序员小海绵【vincewm】的博客-CSDN博客")

## 三、变量

### 3.0 普通变量

*   驼峰命名：例如商品价格命名 pruductPrice，而不是 pruduct_price。
*   禁止拼音英文混用；
*   禁止下划线、美元符号起始。
*   **可读性比长度重要：**命名尽可能短，但 “望文生义” 更重要
    *   **可读性高（推荐）：**下面这些类
        *   最大值：maximumValue 缩写为 maxValue
        *   项目数量：numberOfItems 缩写为 numItems
        *   初始化计数器：initializeCounter 缩写为 initCounter
    *   **可读性低（不推荐）：**
        *   数量：count 缩写为 cnt
        *   值：value 缩写成 val
        *   序号：index 缩写成 i
*   **类型名词放词尾：**词尾是类型，让人能一眼看出这个变量是干什么的。例如：
    *   开始时间：startTime 而不是 startedAt。
    *   工作队列：workQueue 而不是 QueueOfWork。
    *   名字列表：nameList 而不是 listName。因为它是个列表。
    *   最大线程数：MAX_THREAD_COUNT 而不是 COUNT_MAX_THREAD 。它是数量而不是线程。
        

**参考：**

【强制】 代码中的命名均不能以 下划线或美元符号 开始，也不能以 下划线或美元符号 结束。

反例： _name / __name / $name / name_ / name$ / name__

【强制】 代码中的命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式。

说明： 正确的英文拼写和语法可以让阅读者易于理解，避免歧义。注意，纯拼音命名方式更要避免采用。

正例： renminbi / alibaba / taobao / youku / hangzhou 等国际通用的名称，可视同英文。

反例： DaZhePromotion [打折] / getPingfenByName() [评分] / int 某变量 = 3

【强制】 方法名、参数名、成员变量、局部变量都统一使用 lowerCamelCase 风格，必须遵

从驼峰形式。

正例： localValue / getHttpMessage() / inputUserId

. 【强制】 杜绝完全不规范的缩写，避免望文不知义。

反例： AbstractClass“缩写” 命名成 AbsClass；condition“缩写” 命名成 condi，此类随意缩写严重

降低了代码的可阅读性。

【推荐】 为了达到代码自解释的目标，任何自定义编程元素在命名时，使用尽量完整的单词

组合来表达其意。

正例： 在 JDK 中，表达原子更新的类名为：AtomicReferenceFieldUpdater。

反例： int a 的随意命名方式。

【推荐】 在常量与变量的命名时，表示类型的名词放在词尾，以提升辨识度。

正例： startTime / workQueue / nameList / TERMINATED_THREAD_COUNT

反例： startedAt / QueueOfWork / listName / COUNT_TERMINATED_THREAD

### **3.1 数组**

中括号要直接跟在类型后面。例如 String[] args;，而不是 String args[];

```
// 错误示例：中括号放在变量名后面，注释放在右边
String args[]; 
 
// 正确示例：中括号直接跟在类型后面，注释放在代码上面
String[] args;
```

**参考：**

【强制】 类型与中括号紧挨相连来表示数组。

正例： 定义整形数组 int[] arrayDemo;

反例： 在 main 参数中，使用 String args[] 来定义

### **3.2 布尔型变量**

实体类（PO 类）里布尔型变量不能加 is 前缀，否则部分框架解析会引起序列化错误。同时 MySQL 表达 “是否” 的字段可以用 is_xxx，只需 < resultMap > 设置从 is_xxx 到 xxx 的映射关系即可。

**示例：** 

```
public class User {
    private Long id;
    private String username;
//逻辑删除字段
    private boolean deleted;
 
    // 其他属性、构造函数和方法
 
    public boolean isDeleted() {
        return deleted;
    }
 
    public void setDeleted(boolean deleted) {
        this.deleted = deleted;
    }
}
```

**思考 - 为什么强制 boolean 类型变量不能使用 is 开头？**

为了防止序列号失败。

*   lombok 序列号失败：javaBeans 规范 boolean 变量的 getter 方法是 isXXX()，其他变量的 getter 方法是 getXXX()。lombok 遵循 javaBeans 规范，如果一个变量是 boolean isSuccess; 在注解 @Data 或 @Getter 生成 getter 方法的时候，它会生成 isSuccess() 方法，而不是 isIsSucess() 方法。这也是 lombok 的一个大坑。
*   rpc 框架序列号失败：在一些 rpc 框架里面，当反向解析读取到 isSuccess()方法的时候，rpc 框架会 “以为” 其对应的属性值是 success，而实际上其对应的属性值是 isSuccess，导致属性值获取不到，从而抛出异常。

**参考：**

【强制】 POJO 类中布尔类型变量都不要加 is 前缀，否则部分框架解析会引起序列化错误。

说明： 在本文 MySQL 规约中的建表约定第一条，表达是与否的值采用 is_xxx 的命名方式，所以，需要在

<resultMap> 设置从 is_xxx 到 xxx 的映射关系。

反例： 定义为基本数据类型 Boolean isDeleted 的属性，它的方法也是 isDeleted()，RPC 框架在反向解

析的时候，“误以为” 对应的属性名称是 deleted，导致属性获取不到，进而抛出异常。

### 3.3 父子类变量

**父子类变量命名：**避免同名变量。例如父类有个 name 变量，子类想声明一个昵称变量，应该另外声明 nickname，而非再声明一次 name。

**参考：**

【强制】 避免在子父类的成员变量之间、或者不同代码块的局部变量之间采用完全相同的命

名，使可读性降低。

说明： 子类、父类成员变量名相同，即使是 public 类型的变量也是能够通过编译，而局部变量在同一方法

内的不同代码块中同名也是合法的，但是要避免使用。对于非 setter/getter 的参数名称也要避免与成员

变量名称相同。

反例：

```
public class ConfusingName {
    private int age;
 
    // 非 setter/getter 的参数名称，不允许与本类成员变量同名
    public void getData(String alibaba) {
        if (condition) {
            final int money = 531;
            // ...
        }
        
        for (int i = 0; i < 10; i++) {
            // 在同一方法体中，不允许与其它代码块中的 money 命名相同
            final int money = 615;
            // ...
        }
    }
}
 
class Son extends ConfusingName {
    // 不允许与父类的成员变量名称相同
    private int sonAge;
}
```

### 3.4 局部变量

避免同一个方法的多个代码块中声明同名局部变量。

例如下面方法的 money 变量不符合规范：

```
public void getData(String alibaba) {
        if (condition) {
            final int money = 531;
            // ...
        }
        
        for (int i = 0; i < 10; i++) {
            // 在同一方法体中，不允许与其它代码块中的 money 命名相同
            final int money = 615;
            // ...
        }
    }
```

**参考：**

 【强制】 避免在子父类的成员变量之间、或者不同代码块的局部变量之间采用完全相同的命

名，使可读性降低。

说明： 子类、父类成员变量名相同，即使是 public 类型的变量也是能够通过编译，而局部变量在同一方法

内的不同代码块中同名也是合法的，但是要避免使用。对于非 setter/getter 的参数名称也要避免与成员

变量名称相同。

反例：

```
public class ConfusingName {
    private int age;
 
    // 非 setter/getter 的参数名称，不允许与本类成员变量同名
    public void getData(String alibaba) {
        if (condition) {
            final int money = 531;
            // ...
        }
        
        for (int i = 0; i < 10; i++) {
            // 在同一方法体中，不允许与其它代码块中的 money 命名相同
            final int money = 615;
            // ...
        }
    }
}
 
class Son extends ConfusingName {
    // 不允许与父类的成员变量名称相同
    private int sonAge;
}
```

## **四、常量**

### **4.1 普通常量**

*   **纯大写 + 下划线：**例如最大库存量 MAX_STOCK_COUNT。
*   **可读性比长度重要：**常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长。例如最大库存量 MAX_STOCK_COUNT 而不是 MAX_COUNT。
*   **常量类细化：**不要一个总的常量类维护所有常量，要按功能创建粒度小的常量类。例如电商平台商品模块不应该一个 ProductStatusConstants 维护所有商品常量，而应该细化成商品类型常量类 ProductTypeConstants、商品价格范围常量类 PriceRangeConstants、商品属性相关常量类 ProductAttributeConstants 等。
*   **枚举类：**变量值只在固定几个值内变化时用枚举类。例如季节 Season 变量只会有春夏秋冬四种类型，可以定义成季节枚举类。
    
    ```
    public enum SeasonEnum {
        SPRING(1), SUMMER(2), AUTUMN(3), WINTER(4);
        
        private int seq;
     
        SeasonEnum(int seq) {
            this.seq = seq;
        }
     
        public int getSeq() {
            return seq;
        }
    }
    ```
    

**参考：**

【强制】 常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字

长。

正例： MAX_STOCK_COUNT / CACHE_EXPIRED_TIME

反例： MAX_COUNT / EXPIRED_TIME

【推荐】 不要使用一个常量类维护所有常量，要按常量功能进行归类，分开维护。

说明： 大而全的常量类，杂乱无章，使用查找功能才能定位到修改的常量，不利于理解和维护。

正例： 缓存相关常量放在类 CacheConsts 下；系统配置相关常量放在类 ConfigConsts 下。

【推荐】 如果变量值仅在一个固定范围内变化用 enum 类型来定义。

说明： 如果存在名称之外的延伸属性应使用 enum 类型，下面正例中的数字就是延伸信息，表示一年中的

第几个季节。

正例：

```
public enum SeasonEnum {
    SPRING(1), SUMMER(2), AUTUMN(3), WINTER(4);
    
    private int seq;
 
    SeasonEnum(int seq) {
        this.seq = seq;
    }
 
    public int getSeq() {
        return seq;
    }
}
```

### 4.2 Long 常量

*   **大写 L：**以便于区分 “L” 和数字“1”。例如 Long a=1L，而不是 Long a=1l。

```
//错误示例：
// a实际上表示 Long 型的 2，但容易混淆为数字 21。
Long a = 2l; 
//正确示例：
// 使用大写 "L" 来明确表示 Long 型的 2。
Long a = 2L; 
// 使用大写 "L" 来明确表示 long 型的 12345。
long b = 12345L;
```

**参考：**

【强制】 在 long 或者 Long 赋值时，数值后使用大写的 L，不能是小写的 l，小写容易跟数

字 1 混淆，造成误解。

说明： Long a = 2l; 写的是数字的 21，还是 Long 型的 2。