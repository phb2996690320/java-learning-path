---
url: https://blog.csdn.net/qq_40991313/article/details/126312538
title: 【Java 笔记 + 踩坑】Spring 基础 2——IOC,DI 注解开发、整合 Mybatis,Junit_java ioc 注解 - CSDN 博客
date: 2025-02-20 09:40:05
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1，IOC/DI 配置管理第三方 bean](#t0)

[1.1 案例: 数据源对象 dataSource 管理](#t1)

[1.1.1 环境准备](#t2)

[1.1.2 思路分析](#t3)

[1.1.3 实现 Druid 管理](#t4)

[1.1.4 实现 C3P0 管理](#t5)

[1.2 加载 properties 文件](#t6)

[1.2.1 抽取数据库信息到 properties 文件，回顾 sql、jsp、xml 参数占位符](#t7)

[1.2.2 读取 properties 配置文件里的属性（setter 依赖注入基本数据类型）](#t8)

[1.2.3 设置系统属性模式和加载多个 properties](#t9)

[1.2.4 小结](#t10)

[2，核心容器](#t11)

[2.1 环境准备](#t12)

[2.2 容器](#t13)

[2.2.1 容器的两种创建方式（主要用过去方法）](#t14)

[2.2.2 Bean 的三种获取方式](#t15)

[2.2.3 容器类 BeanFactory 继承实现关系](#t16)

[2.2.4 BeanFactory 创建容器（了解）](#t17)

[2.2.5 小结](#t18)

[2.3 核心容器总结](#t19)

[2.3.1 IOC 容器的类和接口](#t20)

[2.3.2 bean 的属性](#t21)

[2.3.3 两种依赖注入方法](#t22)

[3，IOC/DI 注解开发](#t23)

[3.-1 注解关键字汇总](#t24)

[3.0 环境准备](#t25)

[3.1 注解开发案例（xml 里扫描组件）](#t26)

[3.1.1 代码实现](#t27)

[3.1.2 @Component 内可省略 id](#t28)

[3.1.3 知识点 @Component/@Controller/@Service/@Repository](#t29)

[3.2 纯注解开发 @Configuration 和 @ComponentScan](#t30)

[3.2.1 思路分析](#t31)

[3.2.2 代码实现](#t32)

[3.3 注解开发 bean 作用范围与生命周期管理](#t33)

[3.3.1 环境准备](#t34)

[3.3.2 Bean 的作用范围，@Scope](#t35)

[3.3.3 Bean 的生命周期，@PostConstruct 和 @PreDestroy](#t36)

[3.3.4 bean 标签属性对应的注解总结，知识点 @PostConstruct 和 @PreDestroy](#t37)

[3.4 注解开发依赖注入 @Autowired](#t38)

[3.4.1 环境准备](#t39)

[3.4.2 按照类型注入（默认）](#t40)

[3.4.3 按照名称注入 @Qualifier](#t41)

[3.4.4 简单数据类型注入 @Value](#t42)

[3.4.5 注解读取 properties 配置文件 @PropertySource](#t43)

[3.4.6 知识点 @Autowired,@Qualifier,@Value,@PropertySource](#t44)

[4，IOC/DI 注解开发管理第三方 bean，Druid](#t45)

[4.1 环境准备](#t46)

[4.2 不使用外部配置类管理第三方 bean](#t47)

[4.3 创建并引用外部配置类管理第三方 bean](#t48)

[4.3.1 方法一：使用包扫描引入，@ComponentScan](#t49)

[4.3.2 方法二：使用 @Import 精准引入（不推荐）](#t50)

[4.3.3 知识点 @Bean 和 @Import](#t51)

[4.4 为外部配置类注入资源](#t52)

[4.4.1 注入简单数据类型 @Value](#t53)

[4.4.2 注入引用数据类型](#t54)

[5，xml 配置和注解开发对比](#t55)

[6，Spring 整合](#t56)

[6.1 Spring 整合 Mybatis](#t57)

[6.1.1 环境准备（回顾 Mybatis）](#t58)

[6.1.2 整合思路分析](#t59)

[6.1.2 代码实现](#t60)

[6.3 Spring 整合 Junit](#t61)

[6.3.1 环境准备](#t62)

[6.3.2 整合 Junit 步骤](#t63)

[知识点 1：@RunWith](#t64)

[知识点 2：@ContextConfiguration](#t65)

## 1，IOC/DI 配置管理第三方 bean

了解即可，开发中更多用注解开发。

### 1.1 案例: 数据源对象 dataSource 管理

**常见的数据库连接池**

我们现在使用更多的是 Druid，它的性能比其他两个会好一些。

*   DBCP
    
*   C3P0
    
*   Druid
    
*   **Druid（德鲁伊）**
    
    *   Druid 连接池是阿里巴巴开源的数据库连接池项目
        
    *   功能强大，性能优秀，是 Java 语言最好的数据库连接池之一
        

#### 1.1.1 环境准备

学习之前，先来准备下案例环境:

*   创建一个 Maven 项目
    
    ![](<assets/1740015605906.png>)
    
*   pom.xml 添加依赖 Spring
    
    ```
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.10.RELEASE</version>
        </dependency>
    </dependencies>
    ```
    
*   resources 下添加 spring 的配置文件 applicationContext.xml
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="
                http://www.springframework.org/schema/beans
                http://www.springframework.org/schema/beans/spring-beans.xsd">
     
    </beans>
    ```
    
*   编写一个运行类 App
    
    ```
    public class App {
        public static void main(String[] args) {
            ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        }
    }
    ```
    

#### 1.1.2 思路分析

在上述环境下，我们来对数据源进行配置管理，先来分析下思路:

需求: 使用 Spring 的 IOC 容器来管理 Druid 连接池对象

1. 使用第三方的技术，需要在 pom.xml 添加依赖

2. 在配置文件中将【第三方的类】制作成一个 bean，让 IOC 容器进行管理

3. 数据库连接需要基础的四要素`驱动`、`连接`、`用户名`和`密码`，【如何注入】到对应的 bean 中

4. 从 IOC 容器中获取对应的 bean 对象，将其打印到控制台查看结果

**思考:**

*   第三方的类指的是什么?
*   如何注入数据库连接四要素?

#### 1.1.3 实现 Druid 管理

*   Druid 连接池是阿里巴巴开源的数据库连接池项目
    

带着这两个问题，把下面的案例实现下:

**步骤 1: 导入`druid`的依赖**

pom.xml 中添加依赖 druid

```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.16</version>
</dependency>
```

**步骤 2: 配置第三方 bean**

在 applicationContext.xml 配置文件中添加`DruidDataSource`的配置，`DruidDataSource`类里使用 setter 方法注入数据库配置信息。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
            http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--管理DruidDataSource对象-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <!--注意name是driverClassName，不是driver-->
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/spring_db"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>
</beans>
```

 Ctrl+b 跟进到 DruidDataSource 类，按 Ctrl+f12 可以知道适合用 setter 方法注入

![](<assets/1740015607897.png>)

 回顾一下以前 Mybatis 核心配置文件里配置数据库信息片段：

```
 
    <environments default="development">
        <environment>
            <dataSource type="POOLED">
                <!--数据库连接信息-->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///mybatis?useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="1234"/>
            </dataSource>
        </environment>
    </environments>
```

**说明:**

*   **driverClassName: 数据库驱动**
*   url: 数据库连接地址
*   username: 数据库连接用户名
*   password: 数据库连接密码
*   数据库连接的四要素要和自己使用的数据库信息一致。

**步骤 3: 从 IOC 容器中获取对应的 bean 对象**

```
public class App {
    public static void main(String[] args) {
       ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
       DataSource dataSource = (DataSource) ctx.getBean("dataSource");
       System.out.println(dataSource);
    }
}
```

**步骤 4: 运行程序**

打印如下结果: 说明第三方 bean 对象已经被 spring 的 IOC 容器进行管理

![](<assets/1740015610182.png>)

做完案例后，我们可以将刚才思考的两个问题答案说下:

*   第三方的类指的是什么?
    
    ```
    DruidDataSource
    
    
    ```
    
*   如何注入数据库连接四要素?
    
    ```
    setter注入
    
    
    ```
    

#### 1.1.4 实现 C3P0 管理

需求: 使用 Spring 的 IOC 容器来管理 C3P0 连接池对象

实现方案和上面基本一致，重点要关注管理的是哪个 bean 对象 `?

**步骤 1: 导入`C3P0`的依赖**

pom.xml 中添加 c3p0 和 mysql 依赖

```
<dependency>
    <groupId>c3p0</groupId>
    <artifactId>c3p0</artifactId>
    <version>0.9.1.2</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>
```

**对于新的技术，不知道具体的坐标该如何查找?**

*   直接百度搜索
    
*   从 mvn 的仓库`https://mvnrepository.com/（网站打开需要科技）`中进行搜索
    
    ![](<assets/1740015612601.png>)
    

**步骤 2: 配置第三方 bean**

在 applicationContext.xml 配置文件中配置 c3p0

```
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
<!--注意是driverClass-->
    <property name="driverClass" value="com.mysql.jdbc.Driver"/>
<!--注意是jdbcUrl-->
    <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/spring_db"/>
    <property name="user" value="root"/>
    <property name="password" value="root"/>
    <property name="maxPoolSize" value="1000"/>
</bean>
```

****注意:****

*   ComboPooledDataSource 的属性是通过 setter 方式进行注入
*   想注入属性就需要在 ComboPooledDataSource 类或其上层类中有提供属性对应的 setter 方法
*   C3P0 的四个属性和 Druid 的四个属性是不一样的

**步骤 3: 运行程序**

程序会报错，错误如下

![](<assets/1740015615183.png>)

报的错为 **ClassNotFoundException**, 翻译出来是`类没有发现的异常`，具体的类为`com.mysql.jdbc.Driver`。错误的原因是缺少 mysql 的驱动包。

分析出错误的原因，具体的解决方案就比较简单，只需要在 pom.xml 把驱动包引入即可。

```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>
```

添加完 mysql 的驱动包以后，再次运行 App, 就可以打印出结果:

![](<assets/1740015617778.png>)

**注意：**

*   数据连接池在配置属性的时候，除了可以注入数据库连接四要素外还可以配置很多其他的属性，具体都有哪些属性用到的时候再去查，一般配置基础的四个，其他都有自己的默认值
*   Druid 和 C3P0 在没有导入 mysql 驱动包的前提下，一个没报错一个报错，说明 Druid 在初始化的时候没有去加载驱动，而 C3P0 刚好相反
*   Druid 程序运行虽然没有报错，但是当调用 DruidDataSource 的 getConnection() 方法获取连接的时候，也会报找不到驱动类的错误

### 1.2 加载 properties 文件

上节中我们已经完成两个数据源`druid`和`C3P0`的配置，但是其中包含了一些问题，我们来分析下:

*   这两个数据源中都使用到了一些固定的常量如**数据库连接四要素**，把这些值**写在 Spring 的配置文件中不利于后期维护**
*   需要将这些值提**取到一个外部的 properties 配置文**件中
*   Spring 框架如何从配置文件中读取属性值来配置就是接下来要解决的问题。

问题提出来后，具体该如何实现?

#### 1.2.1 抽取数据库信息到 properties 文件，回顾 sql、jsp、xml 参数占位符

**实现思路**

**需求:**

将数据库连接四要素提取到 properties 配置文件，spring 来加载配置信息并使用这些信息来完成属性注入。

1. 在 resources 下创建一个 jdbc.properties(文件的名称可以任意)

2. 将数据库连接四要素配置到配置文件中

3. 在 Spring 的配置文件中加载 properties 文件

4. 使用加载到的值实现属性注入

其中第 3，4 步骤是需要大家重点关注，具体是如何实现。

**实现步骤**

**步骤 1: 准备 properties 配置文件**

resources 下创建一个 jdbc.properties 文件, 并添加对应的属性键值对

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql:///spring_db?ssl=false
jdbc.username=root
jdbc.password=root
```

**注意：别忘了加前缀 “jdbc.”**

**回顾：**

1.url 里如果有 **127.0.0.1:3306 或 localhost:3306**，那么他们都能**省略**。下面两行都可以

```
jdbc.url=jdbc:mysql:///spring_db?ssl=false

```

```
jdbc.url=jdbc:mysql://localhost:3306/spring_db?ssl=false

```

2.url 后缀加 **ssl=false** 可以防止外面获取 properties 属性时候显红，虽然不影响运行，但显红看起来不舒服。

**步骤 2: 开启 xmlns:context 命名空间**

在 applicationContext.xml 中开`context`命名空间。这个不用记，输入 context:property-placeholder 标签后，**会自动导入上面的 xmlns:context 等信息**。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
            http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
 
    <context:property-placeholder location="jdbc.properties"/>
</beans>
```

一共改了这 5 处地方：

![](<assets/1740015620148.png>)

**说明：**

1.  **xmlns** 代表 `xml namespace`， 就是 XML 标签的命名空间。
2.  **xmlns:context** 代表使用 context 作为前缀的命名空间。比如在 XML 中，我们如果用到 这样的配置，就需要加上对应的前缀命名空间。
3.  xmlns:context=”http://www.springframework.org/schema/context”。比如 同理。
4.  **xmlns:xsi** 代表是指 XML **文件**遵守的 XML 规范。
5.  **xsi:schemaLocation** 代表 XML **标签**遵守的 XML 规范。其中 namespace 和对应的 xsd 文件的路径，必须成对出现。比如用到的 xmlns:context 等命名空间，都需要在这里声明其 xsd 文件地址。

**步骤 3: 加载 properties 配置文件**

在配置文件中使用**`context:property-placeholder`标签的 location 属性**来**加载 properties 配置文件**。placeholder 译为占位符。

```
<context:property-placeholder location="jdbc.properties"/>


```

**步骤 4: 完成属性注入**

使用`${key}`来读取 properties 配置文件中的内容并完成属性注入

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
            http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
 
    <context:property-placeholder location="jdbc.properties"/>
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
</beans>
```

**回顾一下参数占位符**

xml 里的参数占位符 ${}

SQL 里的参数占位符 #{}、${}，前者没 sql 注入问题

jsp 里 EL 表达式参数占位符：${}

```
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<!--brands是request请求域转发来的-->
    ${brands}
</body>
</html>
```

至此，读取外部 properties 配置文件中的内容就已经完成。

#### 1.2.2 读取 properties 配置文件里的属性（setter 依赖注入基本数据类型）

**实现思路**

对于上面的案例，效果不是很明显，我们可以换个案例来演示下:

**需求:**

从 properties 配置文件中读取 key 为 name 的值，并将其注入到 BookDao 中并在 save 方法中进行打印。

1. 在项目中添加 BookDao 和 BookDaoImpl 类

2. 为 BookDaoImpl 添加一个 name 属性并提供 setter 方法

3. 在 jdbc.properties 中添加数据注入到 bookDao 中打印方便查询结果

4. 在 applicationContext.xml 添加配置完成配置文件加载、属性注入 (${key})

**实现步骤**

**步骤 1: 在项目中添对应的类**

BookDao 和 BookDaoImpl 类，并在 BookDaoImpl 类中添加`name`属性与 setter 方法

```
public interface BookDao {
    public void save();
}
 
public class BookDaoImpl implements BookDao {
    private String name;
 
    public void setName(String name) {
        this.name = name;
    }
 
    public void save() {
        System.out.println("book dao save ..." + name);
    }
}
```

**步骤 2: 完成配置文件的读取与注入**

在 applicationContext.xml 添加配置 bookDao 的 bean 对象，注入 properties 的键。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
            http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
 
    <context:property-placeholder location="jdbc.properties"/>
 
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
        <property name="name" value="${jdbc.driver}"/>
    </bean>
</beans>
```

**步骤 3: 运行程序**

在 App 类中，从 IOC 容器中获取 bookDao 对象，调用方法，查看值是否已经被获取到并打印控制台

```
public class App {
    public static void main(String[] args) throws Exception{
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
        bookDao.save();
 
    }
}
```

![](<assets/1740015621874.png>)

#### **1.2.3 设置系统属性模式和加载多个 properties**

至此，读取 properties 配置文件中的内容就已经完成，但是在使用的时候，有些注意事项:

*   **问题一: 键值对的 key 为`username`引发的问题**
    
    1. 在 properties 中配置键值对的时候，如果 **key 设置为`username`**`，不是jdbc.username`
    
    ```
    username=root666
    
    
    ```
    
    2. 在 applicationContext.xml 注入该属性
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="
                http://www.springframework.org/schema/beans
                http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context
                http://www.springframework.org/schema/context/spring-context.xsd">
     
        <context:property-placeholder location="jdbc.properties"/>
     
        <bean>
            <property name="name" value="${username}"/>
        </bean>
    </beans>
    ```
    
    3. 运行后，在**控制台打印的**却不是`root666`，而**是自己电脑的用户名**
    
    ![](<assets/1740015623772.png>)
    
    4. 出现问题的**原因**是**`<context:property-placeholder/>`标签会加载系统的环境变量**，而且环境变量的值**优先度更高**
    
*   **查看系统的环境变量**
    
    ```
    public static void main(String[] args) throws Exception{
        Map<String, String> env = System.getenv();
        System.out.println(env);
    }
    ```
    
    大家可以自行运行，在打印出来的结果中会有一个 USERNAME=XXX[自己电脑的用户名称]
    
    **5. 解决方案，设置系统属性模式为** NEVER
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="
                http://www.springframework.org/schema/beans
                http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context
                http://www.springframework.org/schema/context/spring-context.xsd">
     
        <context:property-placeholder location="jdbc.properties" system-properties-mode="NEVER"/>
    </beans>
    ```
    
    **system-properties-mode: 设置为 NEVER, 表示不加载系统属性**，就可以解决上述问题。
    
    **解决方案二：避免使用`username`作为属性的`key`。**
    
*   **问题二: 加载多个 properties 配置文件**
    
    1. 调整下配置文件的内容，在 resources 下添加`jdbc.properties`,`jdbc2.properties`, 内容如下:
    
    jdbc.properties
    
    ```
    jdbc.driver=com.mysql.jdbc.Driver
    jdbc.url=jdbc:mysql://127.0.0.1:3306/spring_db
    jdbc.username=root
    jdbc.password=root
    ```
    
    jdbc2.properties
    
    ```
    username=root666
    
    
    ```
    
    2. 修改 applicationContext.xml
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="
                http://www.springframework.org/schema/beans
                http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context
                http://www.springframework.org/schema/context/spring-context.xsd">
        <!--方式一：使用逗号分别配置-->
        <context:property-placeholder location="jdbc.properties,jdbc2.properties" system-properties-mode="NEVER"/>
        <!--方式二：当前路径下通配符配置-->
        <context:property-placeholder location="*.properties" system-properties-mode="NEVER"/>
        <!--方式三：根路径（仅当前工程）下通配符配置-->
        <context:property-placeholder location="classpath:*.properties" system-properties-mode="NEVER"/>
        <!--方式四：根路径（当前工程、依赖的jar包）通配符配置-->
        <context:property-placeholder location="classpath*:*.properties" system-properties-mode="NEVER"/>
    </beans>
    ```
    
    **说明:**
    
    *   方式一: 可以实现，如果配置文件多的话，每个都需要配置
    *   方式二:`*.properties`代表所有以 properties 结尾的文件都会被加载，可以解决方式一的问题，但是不标准
    *   方式三: 标准的写法，`classpath:`代表的是从根路径下开始查找，但是只能查询当前项目的根路径
    *   **方式四: 不仅可以加载当前项目还可以加载当前项目所依赖的所有项目的根路径下的 properties 配置文件**
    
    **classpath 和 classpath * 区别：** 
    
    **classpath：**只会到你的 class 路径中查找找文件。
    
    **classpath*：**不仅包含 class 路径，还包括 jar 文件中（class 路径）进行查找。
    
    **注意：** 用 classpath*: 需要遍历所有的 classpath，所以加载速度是很慢的；因此，在规划的时候，应该尽可能规划好资源文件所在的路径，**尽量避免使用 classpath***。
    

#### 1.2.4 小结

本节主要讲解的是 properties 配置文件的加载，需要掌握的内容有:

*   **如何开启`context`命名空间**
    
    ![](<assets/1740015625802.png>)
    
*   **如何加载 properties 配置文件，context:property-placeholder 标签**
    
    ```
    <context:property-placeholder location="" system-properties-mode="NEVER"/>
    
    
    ```
    
*   如何在 applicationContext.**xml 引入 properties 配置文件中的值**
    
    ```
    ${key}
    
    
    ```
    

## 2，核心容器

前面已经完成 bean 与依赖注入的相关知识学习，接下来我们主要学习的是 IOC 容器中的**核心容器**。

这里所说的核心容器，大家可以把它简单的理解为**`ApplicationContext`**

**学习计划：** 

*   如何创建容器?
*   创建好容器后，如何从容器中获取 bean 对象?
*   容器类的层次结构是什么?
*   BeanFactory 是什么?

### 2.1 环境准备

先来准备下案例环境: 跟之前都一样，导入 Spring 依赖、创建 dao 接口和实现类，applicationContext.xml 里配置 dao 的 bean。

*   创建一个 Maven 项目
    
*   pom.xml 添加 Spring 的依赖
    
    ```
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.10.RELEASE</version>
        </dependency>
    </dependencies>
    ```
    
*   resources 下添加 applicationContext.xml
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="
                http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean/>
    </beans>
    ```
    
*   添加 BookDao 和 BookDaoImpl 类
    
    ```
    public interface BookDao {
        public void save();
    }
    public class BookDaoImpl implements BookDao {
        public void save() {
            System.out.println("book dao save ..." );
        }
    }
    ```
    
*   创建运行类 App
    
    ```
    public class App {
        public static void main(String[] args) {
            ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
            BookDao bookDao = (BookDao) ctx.getBean("bookDao");
            bookDao.save();
        }
    }
    ```
    

最终创建好的项目结构如下:

![](<assets/1740015628034.png>)

### 2.2 容器

#### 2.2.1 容器的两种创建方式（主要用过去方法）

**方式一（主要方式）：** 

入门案例中创建`ApplicationContext`的方式为:

```
ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");


```

这种方式翻译为: **类路径下的 XML 配置文件**

**方式二（了解）：** 

除了上面这种方式，Spring 还提供了**另外一种创建方式（耦合度较高, 不推荐使用）:**

```
ApplicationContext ctx = new FileSystemXmlApplicationContext("D:\\workspace\\spring\\spring_10_container\\src\\main\\resources\\applicationContext.xml");


```

这种方式翻译为: **文件系统下的 XML 配置文件**

这种方式是从项目路径下开始查找`applicationContext.xml`配置文件的，路径只需要右键 xml 配置文件拷贝路径再粘贴即可。

![](<assets/1740015629147.png>)

**说明:** 大家练习的时候，写自己的具体路径。

这种方式虽能实现，但是当项目的位置发生变化后, 代码也需要跟着改, **耦合度较高, 不推荐使用**。

#### 2.2.2 Bean 的三种获取方式

**方式一（主要方式）**，根据 id 获取，就是目前案例中获取的方式:

```
BookDao bookDao = (BookDao) ctx.getBean("bookDao");


```

这种方式存在的问题是每次获取的时候都需要进行类型转换，有没有更简单的方式呢?

**方式二（了解）：**

```
BookDao bookDao = ctx.getBean("bookDao"，BookDao.class);


```

优点：**解决类型强转问题**

缺点：参数又多加了一个，相对来说没有简化多少。

**方式三（看情况用）:**

```
BookDao bookDao = ctx.getBean(BookDao.class);


```

这种方式就**类似**我们之前所学习依赖注入中的**按类型注入**。必须要**确保 IOC 容器中该类型对应的 bean 对象只能有一个**。

#### 2.2.3 容器类 BeanFactory 继承实现关系

(1) 在 IDEA 中双击`shift`, 输入 BeanFactory

![](<assets/1740015629394.png>)

(2) 点击进入 BeanFactory 类，ctrl+h, 就能查看到如下结构的层次关系

![](<assets/1740015629672.png>)

从图中可以看出，容器类也是从无到有根据需要一层层叠加上来的，大家重点理解下这种设计思想。

继承关系：ClassPathXmlApplicationContext 实现 ApplicationContext，ApplicationContext 继承 BeanFactory

#### 2.2.4 BeanFactory 创建容器（了解）

这是最早期的创建容器方案，现在基本不用。

使用 BeanFactory 来创建 IOC 容器的具体实现方式为:

```
public class AppForBeanFactory {
    public static void main(String[] args) {
        Resource resources = new ClassPathResource("applicationContext.xml");
        BeanFactory bf = new XmlBeanFactory(resources);
        BookDao bookDao = bf.getBean(BookDao.class);
        bookDao.save();
    }
}
```

 **`BeanFactory`和`ApplicationContext`的区别**

为了更好的看出`BeanFactory`和`ApplicationContext`之间的区别，在 BookDaoImpl 添加如下构造函数:

```
public class BookDaoImpl implements BookDao {
    public BookDaoImpl() {
        System.out.println("constructor");
    }
    public void save() {
        System.out.println("book dao save ..." );
    }
}
```

如果不去获取 bean 对象，打印会发现：

*   **BeanFactory 是延迟加载**，只有**在获取 bean 对象的时候才会去创建**
    
*   **ApplicationContext** 是**立即加载**，容器加载的时候就会**创建 bean 对象**
    
*   ApplicationContext 要想成为延迟加载，只需要按照如下方式配置 lazy-init
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="
                http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean  lazy-init="true"/>
    </beans>
    ```
    

#### **2.2.5 小结**

这一节中所讲的知识点包括:

*   **容器创建的两种方式**
    
    *   ClassPathXmlApplicationContext[掌握]
    *   FileSystemXmlApplicationContext[知道即可]
*   **获取 Bean 的三种方式**
    
    *   getBean("名称"): 需要类型转换
    *   getBean("名称", 类型. class): 多了一个参数
    *   getBean(类型. class): 容器中不能有多个该类的 bean 对象
    
    上述三种方式，各有各的优缺点，用哪个都可以。
    
*   容器类层次结构
    
    *   只需要知晓容器的最上级的父接口为 BeanFactory 即可
*   BeanFactory
    
    *   使用 BeanFactory 创建的容器是延迟加载
    *   使用 ApplicationContext 创建的容器是立即加载
    *   具体 BeanFactory 如何创建只需要了解即可。

### 2.3 核心容器总结

#### 2.3.1 IOC 容器的类和接口

*   **BeanFactory 是 IOC 容器的顶层接口**，初始化 BeanFactory 对象时，加载的 bean **延迟加载**
*   **ApplicationContext** 接口是 Spring 容器的**核心接口**，初始化时 bean **立即加载**
*   ApplicationContext 接口提供基础的 bean 操作相关方法，通过其他接口扩展其功能
*   ApplicationContext 接口**常用初始化类**
    *   ****ClassPathXmlApplicationContext(常用)****
    *   FileSystemXmlApplicationContext

#### 2.3.2 bean 的属性

![](<assets/1740015629892.png>)

其实整个配置中最常用的就两个属性 **id** 和 **class**。

把 **scope、init-method、destroy-method** 框起来的原因是，后面注解在讲解的时候还会用到，所以大家对这三个属性关注下。

#### 2.3.3 两种依赖注入方法

![](<assets/1740015630070.png>)

## 3，IOC/DI 注解开发

Spring 的 IOC/DI 对应的配置开发就已经讲解完成，但是使用起来相对来说还是比较复杂的，复杂的地方在**配置文件**。

前面咱们聊 Spring 的时候说过，Spring 可以简化代码的开发，到现在并没有体会到。

所以 Spring 到底是如何简化代码开发的呢?

要想真正简化开发，就需要用到 Spring 的注解开发，Spring 对注解支持的版本历程:

*   2.0 版开始支持注解
*   2.5 版注解功能趋于完善
*   3.0 版支持纯注解开发

关于注解开发，我们会讲解两块内容**`注解开发定义bean`和`纯注解开发`。**

注解开发定义 bean 用的是 2.5 版提供的注解，纯注解开发用的是 3.0 版提供的注解。

### 3.-1 注解关键字汇总

先剧透一下，**配置类样子：**

```
@Configuration    //声明配置类
@ComponentScan("package1")    //组件扫描路径
@EnableAspectJAutoProxy        //开启aop
@EnableTransactionManagement    //开启业务管理
@PropertySource("classpath:jdbc.properties")        //properties配置文件路径
@Import({JdbcConfig.class,MybatisConfig.class})    //导入第三方配置文件
public class SpringConfig {
}
```

![](<assets/1740015630331.png>)

**IOC 相关注解** 

**@Configuration**

<table><thead><tr><th>名称</th><th>@Configuration</th></tr></thead><tbody><tr><td>类型</td><td>类注解</td></tr><tr><td>位置</td><td>类定义上方</td></tr><tr><td>作用</td><td>设置该类为 spring 配置类</td></tr><tr><td>属性</td><td>value（默认）：定义 bean 的 id</td></tr></tbody></table>

**@ComponentScan**

<table><thead><tr><th>名称</th><th>@ComponentScan</th></tr></thead><tbody><tr><td>类型</td><td>类注解</td></tr><tr><td>位置</td><td>类定义上方</td></tr><tr><td>作用</td><td>设置 spring 配置类扫描路径，用于加载使用注解格式定义的 bean</td></tr><tr><td>属性</td><td>value（默认）：扫描路径，此路径可以逐层向下扫描</td></tr></tbody></table>

**DI 相关注解**

**知识点 1：@Autowired**

<table><thead><tr><th>名称</th><th>@Autowired</th></tr></thead><tbody><tr><td>类型</td><td>属性注解 或 方法注解（了解） 或 方法形参注解（了解）</td></tr><tr><td>位置</td><td>属性定义上方 或 标准 set 方法上方 或 类 set 方法上方 或 方法形参前面</td></tr><tr><td>作用</td><td>为引用类型属性设置值</td></tr><tr><td>属性</td><td>required：true/false，定义该属性是否允许为 null</td></tr></tbody></table>

**知识点 2：@Qualifier**

<table><thead><tr><th>名称</th><th>@Qualifier</th></tr></thead><tbody><tr><td>类型</td><td>属性注解 或 方法注解（了解）</td></tr><tr><td>位置</td><td>属性定义上方 或 标准 set 方法上方 或 类 set 方法上方</td></tr><tr><td>作用</td><td>为引用类型属性指定注入的 beanId</td></tr><tr><td>属性</td><td>value（默认）：设置注入的 beanId</td></tr></tbody></table>

**知识点 3：@Value**

<table><thead><tr><th>名称</th><th>@Value</th></tr></thead><tbody><tr><td>类型</td><td>属性注解 或 方法注解（了解）</td></tr><tr><td>位置</td><td>属性定义上方 或 标准 set 方法上方 或 类 set 方法上方</td></tr><tr><td>作用</td><td>为 基本数据类型 或 字符串类型 属性设置值</td></tr><tr><td>属性</td><td>value（默认）：要注入的属性值</td></tr></tbody></table>

**知识点 4：@PropertySource**

<table><thead><tr><th>名称</th><th>@PropertySource</th></tr></thead><tbody><tr><td>类型</td><td>类注解</td></tr><tr><td>位置</td><td>类定义上方</td></tr><tr><td>作用</td><td>加载 properties 文件中的属性值</td></tr><tr><td>属性</td><td>value（默认）：设置加载的 properties 文件对应的文件名或文件名组成的数组</td></tr></tbody></table>

**第三方管理相关注解：** 

**知识点 1：@Bean**

<table><thead><tr><th>名称</th><th>@Bean</th></tr></thead><tbody><tr><td>类型</td><td>方法注解</td></tr><tr><td>位置</td><td>方法定义上方</td></tr><tr><td>作用</td><td>设置该方法的返回值作为 spring 管理的 bean</td></tr><tr><td>属性</td><td>value（默认）：定义 bean 的 id</td></tr></tbody></table>

**知识点 2：@Import**

<table><thead><tr><th>名称</th><th>@Import</th></tr></thead><tbody><tr><td>类型</td><td>类注解</td></tr><tr><td>位置</td><td>类定义上方</td></tr><tr><td>作用</td><td>导入配置类</td></tr><tr><td>属性</td><td>value（默认）：定义导入的配置类类名，<br>当配置类有多个时使用数组格式一次性导入多个配置类</td></tr></tbody></table>

**aop 相关注解：**

**知识点 1：@EnableAspectJAutoProxy**

<table><thead><tr><th>名称</th><th>@EnableAspectJAutoProxy</th></tr></thead><tbody><tr><td>类型</td><td>配置类注解</td></tr><tr><td>位置</td><td>配置类定义上方</td></tr><tr><td>作用</td><td>开启注解格式 AOP 功能</td></tr></tbody></table>

**知识点 2：@Aspect**

<table><thead><tr><th>名称</th><th>@Aspect</th></tr></thead><tbody><tr><td>类型</td><td>类注解</td></tr><tr><td>位置</td><td>切面类定义上方</td></tr><tr><td>作用</td><td>设置当前类为 AOP 切面类</td></tr></tbody></table>

**知识点 3：@Pointcut**

<table><thead><tr><th>名称</th><th>@Pointcut</th></tr></thead><tbody><tr><td>类型</td><td>方法注解</td></tr><tr><td>位置</td><td>切入点方法定义上方</td></tr><tr><td>作用</td><td>设置切入点方法</td></tr><tr><td>属性</td><td>value（默认）：切入点表达式</td></tr></tbody></table>

**aop 环绕通知相关注解：** 

**知识点 4：@Before**

<table><thead><tr><th>名称</th><th>@Before</th></tr></thead><tbody><tr><td>类型</td><td>方法注解</td></tr><tr><td>位置</td><td>通知方法定义上方</td></tr><tr><td>作用</td><td>设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法前运行</td></tr></tbody></table>

**知识点 5：@After**

<table><thead><tr><th>名称</th><th>@After</th></tr></thead><tbody><tr><td>类型</td><td>方法注解</td></tr><tr><td>位置</td><td>通知方法定义上方</td></tr><tr><td>作用</td><td>设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法后运行</td></tr></tbody></table>

**知识点 6：@AfterReturning**

<table><thead><tr><th>名称</th><th>@AfterReturning</th></tr></thead><tbody><tr><td>类型</td><td>方法注解</td></tr><tr><td>位置</td><td>通知方法定义上方</td></tr><tr><td>作用</td><td>设置当前通知方法与切入点之间绑定关系，当前通知方法在原始切入点方法正常执行完毕后执行</td></tr></tbody></table>

**知识点 7：@AfterThrowing**

<table><thead><tr><th>名称</th><th>@AfterThrowing</th></tr></thead><tbody><tr><td>类型</td><td>方法注解</td></tr><tr><td>位置</td><td>通知方法定义上方</td></tr><tr><td>作用</td><td>设置当前通知方法与切入点之间绑定关系，当前通知方法在原始切入点方法运行抛出异常后执行</td></tr></tbody></table>

**知识点 8：@Around**

<table><thead><tr><th>名称</th><th>@Around</th></tr></thead><tbody><tr><td>类型</td><td>方法注解</td></tr><tr><td>位置</td><td>通知方法定义上方</td></tr><tr><td>作用</td><td>设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法前后运行</td></tr></tbody></table>

**业务相关注解：**

**知识点 1：@EnableTransactionManagement**

<table><thead><tr><th>名称</th><th>@EnableTransactionManagement</th></tr></thead><tbody><tr><td>类型</td><td>配置类注解</td></tr><tr><td>位置</td><td>配置类定义上方</td></tr><tr><td>作用</td><td>设置当前 Spring 环境中开启注解式事务支持</td></tr></tbody></table>

**知识点 2：@Transactional**

<table><thead><tr><th>名称</th><th>@Transactional</th></tr></thead><tbody><tr><td>类型</td><td>接口注解 类注解 方法注解</td></tr><tr><td>位置</td><td>业务层接口上方 业务层实现类上方 业务方法上方</td></tr><tr><td>作用</td><td>为当前业务层方法添加事务（如果设置在类或接口上方则类或接口中所有方法均添加事务）</td></tr></tbody></table>

### 3.0 环境准备

在学习注解开发之前，先来准备下案例环境: 跟之前一样，导入 Spring 依赖、写 dao、service 的接口和实现类、创建 applicationContext.xml 配置文件、创建运行类测试。

*   创建一个 Maven 项目
    
*   pom.xml 添加 Spring 的依赖
    
    ```
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.10.RELEASE</version>
        </dependency>
    </dependencies>
    ```
    
*   resources 下添加 applicationContext.xml
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="
                http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean/>
    </beans>
    ```
    
*   添加 BookDao、BookDaoImpl、BookService、BookServiceImpl 类
    
    ```
    public interface BookDao {
        public void save();
    }
    public class BookDaoImpl implements BookDao {
        public void save() {
            System.out.println("book dao save ..." );
        }
    }
    public interface BookService {
        public void save();
    }
     
    public class BookServiceImpl implements BookService {
        public void save() {
            System.out.println("book service save ...");
        }
    }
    ```
    
*   创建运行类 App
    
    ```
    public class App {
        public static void main(String[] args) {
            ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
            BookDao bookDao = (BookDao) ctx.getBean("bookDao");
            bookDao.save();
        }
    }
    ```
    

最终创建好的项目结构如下:

![](<assets/1740015630765.png>)

### 3.1 注解开发案例（xml 里扫描组件）

#### 3.1.1 代码实现

**类注解 @Component：声明此类是一个 Bean，实现 bean 的注入。**

```
@Component("bean的id")    
//@Component    //仅一个实现类的话可以不加id，getBean通过字节码文件，按类型获取
 
```

**扩展：**

**三个衍生注解`@Controller`、`@Service`、`@Repository`**

通过查看源码会发现这三个注解**和 @Component 注解的作用是完全一样的**:

![](<assets/1740015630965.png>)

**样子不一样是为了区分三层：**

*   业务层使用 **@Service**
*   数据层使用 **@Repository**，译为仓库，数据库
*   表现层使用 **@Controller**，译为控制器
*   其他使用（包括工具类、下一章 spring 基础 3 讲到的 aop 层的**切面类 @Aspect**、ssm 整合的项目拦截器类）**@Component**

例如学 springmvc 时候，Spring 配置类的 **@ComponentScan 里可以添加 excludeFilters** 属性**排除掉注解** @Controller 的 bean，把他们交给 springmvc 配置类扫描和管理。

**步骤 1: 删除原 XML 配置里的 bean**

将配置文件中的`<bean>`标签删除掉

```
<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>


```

**步骤 2:Dao 上添加注解`@Component`**

在 BookDaoImpl 类上添加`@Component`注解

```
@Component    //只有一个实现类的话可以不加id，getBean时候通过bookDaoimpl.class和bookDao.class都是可以的
//@Component("bookDao")    //多个实现类的话必须起id
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ..." );
    }
}
```

**注意:@Component 注解一般添加在接口上，因为接口是无法创建对象的。接口里只有 dao 接口可以注解。**

XML 与注解配置的对应关系:

![](<assets/1740015631210.png>)

**步骤 3: 配置 Spring 的注解包扫描 @Component**

为了让 Spring 框架能够扫描到写在类上的注解，需要在配置文件上进行包扫描。

上面 xmlns 命名空间等配置**不用记**，**输入 context:component-scan 标签，beans 标签配置信息会自动导入**。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <context:component-scan base-package="com.itheima"/>
</beans>
```

**说明:**

**component-scan 组件扫描**

*   **component:** 组件, Spring 将管理的 **bean 视作自己的一个组件**
*   **scan**: 扫描

base-package 指定 Spring 框架扫描的包路径，它会**扫描**指定包**及其子包**中的所有类上的**注解**。

*   包路径越多 [如: com.itheima.dao.impl]，扫描的范围越小速度越快
*   包路径越少 [如: com.itheima], 扫描的范围越大速度越慢
*   一般扫描到项目的组织名称即 Maven 的 groupId 下 [如: com.itheima] 即可。

**步骤 4：通过 id 获取 bean，运行程序**

```
public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
        bookDao.save();
    }
}
```

运行`App`类查看打印结果

![](<assets/1740015631532.png>)

#### 3.1.2 @Component 内可省略 id

**注解方式 @Component 内可省略 id，默认值是首字母小写的类名**

**步骤 1:Service 上添加注解**

在 BookServiceImpl 类上也添加`@Component`交给 Spring 框架管理

```
@Component    //@Component(value="bookService")或@Component("bookService")
//默认注解id是首字母小写的接口名，即bookService
public class BookServiceImpl implements BookService {
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

**步骤 2: 字节码文件获取 bean，运行程序**

在 App 类中，从 IOC 容器中通过**字节码文件获取或默认 ID** 对应的 bean 对象，打印

**注意：**字节码文件都不加引号。

```
public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        BookService bookService = ctx.getBean(BookService.class);
        //BookService bookService = ctx.getBean("bookServiceImpl");
        System.out.println(bookService);
    }
}
```

打印观察结果，两个 bean 对象都已经打印到控制台

![](<assets/1740015631814.png>)

 **说明:**

*   BookServiceImpl 类没有起名称，所以在 App 中是按照类型来获取 bean 对象
    
*   **@Component 注解如果不起名称**，会有一个**默认值就是`当前类名首字母小写`**，所以也可以按照名称获取，如
    
    ```
    BookService bookService = (BookService)ctx.getBean("bookServiceImpl");
    System.out.println(bookService);
    ```

#### **3.1.3 知识点** @Component/@Controller/@Service/@Repository

<table><thead><tr><th>名称</th><th>@Component/@Controller/@Service/@Repository</th></tr></thead><tbody><tr><td>类型</td><td>类注解</td></tr><tr><td>位置</td><td>类定义上方</td></tr><tr><td>作用</td><td>设置该类为 spring 管理的 bean</td></tr><tr><td>属性</td><td>value（默认）：定义 bean 的 id</td></tr></tbody></table>

### 3.2 纯注解开发**`@Configuration和@ComponentScan`**

上面已经可以使用注解来配置 bean, 但是依然有用到配置文件，在配置文件中对包进行了扫描，Spring 在 3.0 版已经支持纯注解开发

*   Spring3.0 开启了**纯注解开发**模式，使用 **Java 类替代配置文件**，开启了 Spring 快速开发赛道

**具体如何实现?**

**删除 applicationContext.xml 配置文件，使用注解类替代，@Configuration 声明注解类，@Component() 扫描组件位置。**

#### 3.2.1 思路分析

实现思路为:

*   将配置文件 **applicationContext.xml 删除掉**，使用类来替换。

#### 3.2.2 代码实现

**步骤 1: 创建配置类**

创建 config 软件包，在其内创建一个配置类`SpringConfig`

```
public class SpringConfig {
}
```

**步骤 2: 标识该类为配置类`@Configuration`**

在配置类上添加**`@Configuration`**注解，将其标识为一个配置类, 替换`applicationContext.xml`

```
//@Configuration其实和@Component作用一样，可以互相替换，只是样子不同，为了更直观的看出它是配置类
@Configuration
public class SpringConfig {
}
```

**@Configuration 其实和 @Component 作用一样，可以互相替换，只是样子不同，为了更直观的看出它是配置类。**

**@Configuration 和 @Component 的区别：**

@Configuration 中所有带 @Bean 注解的方法都会被动态代理（cglib），因此调用该方法返回的都是同一个实例。

而 @Conponent 修饰的类不会被代理，每实例化一次就会创建一个新的对象。

**步骤 3: 用注解替换包扫描配置`@ComponentScan`**

在配置类上添加包扫描注解**`@ComponentScan`**替换`<context:component-scan base-package=""/>`

**如果扫描多个文件，有两种方法，推荐第二种。**

**方法一：用数组模式：**

![](<assets/1740015632026.png>)

这种配置相比直接扫描 com.itheima 不是多此一举，因为可能会存在某些 bean 不能乱扫描的情况。

**方法二：**

在整合 SpringMvc 时候，就必须设置 bean 的加载控制，排除加载表现层的 bean，这样注解形式的 @Controller 的 bean 就会被排除掉，交给 SpringMvcConfig 配置类的容器管理：

```
@Configuration
@ComponentScan(value="com.itheima",
    excludeFilters=@ComponentScan.Filter(
        type = FilterType.ANNOTATION,
        classes = Controller.class
    )
)
public class SpringConfig {
}
```

```
//有了@ComponentScan，@Configuration就可以省略
//@Configuration
@ComponentScan("com.itheima")
public class SpringConfig {
}
```

**步骤 4: 创建运行类并执行**

```
new ClassPathXmlApplicationContext("applicationContext.xml")

```

改成

```
new AnnotationConfigApplicationContext(SpringConfig.class);

```

**翻译：**

Annotation 译为注解，**AnnotationConfigApplicationContext** 译为**注解配置的应用上下文**

 ClassPathXmlApplicationContext 译为**类路径的 xml 应用上下文。**

**注意：**

一般字节码. class 文件都是不带引号的。

```
public class AppForAnnotation {
 
    public static void main(String[] args) {
        //注意字节码文件不加引号
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
        System.out.println(bookDao);
        BookService bookService = ctx.getBean(BookService.class);
        System.out.println(bookService);
    }
}
```

运行 AppForAnnotation, 可以看到两个对象依然被获取成功

![](<assets/1740015632251.png>)

**步骤回顾：** 

*   **Java 类替换 Spring 核心配置文件**
    
    ![](<assets/1740015632417.png>)
    
*   **@Configuration** 注解用于设定当前类为配置类
    
*   **@ComponentScan** 注解用于设定扫描路径，此注解只能添加一次，多个数据请用数组格式
    
    ```
    @ComponentScan({com.itheima.service","com.itheima.dao"})
    
    
    ```
    
*   读取 Spring 核心配置文件初始化容器对象切换为读取 Java 配置类初始化容器对象
    
    ```
    //加载配置文件初始化容器
    ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
    //加载配置类初始化容器
    ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
    ```
    

**3.2.3 知识点 @Configuration 和 @ComponentScan** 

**知识点 1：@Configuration**

<table><thead><tr><th>名称</th><th>@Configuration</th></tr></thead><tbody><tr><td>类型</td><td>类注解</td></tr><tr><td>位置</td><td>类定义上方</td></tr><tr><td>作用</td><td>设置该类为 spring 配置类</td></tr><tr><td>属性</td><td>value（默认）：定义 bean 的 id</td></tr></tbody></table>

**知识点 2：@ComponentScan**

<table><thead><tr><th>名称</th><th>@ComponentScan</th></tr></thead><tbody><tr><td>类型</td><td>类注解</td></tr><tr><td>位置</td><td>类定义上方</td></tr><tr><td>作用</td><td>设置 spring 配置类扫描路径，用于加载使用注解格式定义的 bean</td></tr><tr><td>属性</td><td>value（默认）：扫描路径，此路径可以逐层向下扫描</td></tr></tbody></table>

**小结:**

*   记住 @Component、@Controller、@Service、@Repository 这四个注解
*   applicationContext.xml 中**`<context:component-san/>`**的作用是指定**扫描包路径**，注解为 **@ComponentScan**
*   **@Configuration** 标识该类为配置类，使用类替换 applicationContext.xml 文件
*   ClassPathXmlApplicationContext 是加载 XML 配置文件
*   AnnotationConfigApplicationContext 是加载配置类

### 3.3 注解开发 bean 作用范围与生命周期管理

使用注解已经完成了 bean 的管理，接下来按照前面所学习的内容，将通过配置实现的内容都换成对应的注解实现，包含两部分内容:`bean作用范围`和`bean生命周期`。

#### 3.3.1 环境准备

老规矩，学习之前先来准备环境: **跟以前基本一样，xml 配置改成注解配置**

*   创建一个 Maven 项目
    
*   pom.xml 添加 Spring 的依赖
    
    ```
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.10.RELEASE</version>
        </dependency>
    </dependencies>
    ```
    
*   添加一个配置类`SpringConfig`
    
    ```
    @Configuration
    @ComponentScan("com.itheima")
    public class SpringConfig {
    }
    ```
    
*   添加 BookDao、BookDaoImpl 类
    
    ```
    public interface BookDao {
        public void save();
    }
    @Repository
    public class BookDaoImpl implements BookDao {
        public void save() {
            System.out.println("book dao save ..." );
        }
    }
    ```
    
*   创建运行类 App
    
    ```
    public class App {
        public static void main(String[] args) {
            AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
            BookDao bookDao1 = ctx.getBean(BookDao.class);
            BookDao bookDao2 = ctx.getBean(BookDao.class);
            System.out.println(bookDao1);
            System.out.println(bookDao2);
        }
    }
    ```
    

最终创建好的项目结构如下:

![](<assets/1740015632652.png>)

#### 3.3.2 Bean 的作用范围，**@Scope**

(1) 先运行 App 类, 在控制台打印两个一摸一样的地址。

说明两个 bean 属于同一个对象，**默认情况下 bean 是单例**

![](<assets/1740015632944.png>)

(2) 要想将 BookDaoImpl **变成非单例**，只需要在其类上**添加`@scope`注解原型 prototype**

```
@Repository
//@Scope设置bean的作用范围
@Scope("prototype")
public class BookDaoImpl implements BookDao {
 
    public void save() {
        System.out.println("book dao save ...");
    }
}
```

再次执行 App 类，打印结果:

![](<assets/1740015633146.png>)

**知识点 1：@Scope**

<table><thead><tr><th>名称</th><th>@Scope</th></tr></thead><tbody><tr><td>类型</td><td>类注解</td></tr><tr><td>位置</td><td>类定义上方</td></tr><tr><td>作用</td><td>设置该类创建对象的作用范围<br>可用于设置创建出的 bean 是否为单例对象</td></tr><tr><td>属性</td><td>value（默认）：定义 bean 作用范围，<br><strong>默认值 singleton（单例），可选值 prototype（原型模式，非单例）</strong></td></tr></tbody></table>

#### 3.3.3 Bean 的生命周期，**@PostConstruct 和** @PreDestroy

(1) 在 BookDaoImpl 中添加两个方法，`init`和`destroy`, 方法名可以任意

```
@Repository
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ...");
    }
    public void init() {
        System.out.println("init ...");
    }
    public void destroy() {
        System.out.println("destroy ...");
    }
}
```

(2) 如何对方法进行标识，哪个是初始化方法，哪个是销毁方法?

只需要在对应的**方法**上添加**`@PostConstruct`和`@PreDestroy`**注解即可。**`PostConstruct译为构造方法后，PreDestroy译为销毁前。`**

```
@Repository
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ...");
    }
    @PostConstruct //在构造方法之后执行，替换 init-method
    public void init() {
        System.out.println("init ...");
    }
    @PreDestroy //在销毁方法之前执行,替换 destroy-method
    public void destroy() {
        System.out.println("destroy ...");
    }
}
```

(3) 要想看到两个方法执行，需要注意的是`destroy`只有在容器关闭的时候，才会执行，所以需要修改 App 的类

```
public class App {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookDao bookDao1 = ctx.getBean(BookDao.class);
        BookDao bookDao2 = ctx.getBean(BookDao.class);
        System.out.println(bookDao1);
        System.out.println(bookDao2);
        ctx.close(); //关闭容器
    }
}
```

(4) 运行 App, 类查看打印结果，证明 init 和 destroy 方法都被执行了。

![](<assets/1740015633532.png>)

**注意:**@PostConstruct 和 @PreDestroy 注解如果找不到，需要导入下面的 jar 包

```
<dependency>
  <groupId>javax.annotation</groupId>
  <artifactId>javax.annotation-api</artifactId>
  <version>1.3.2</version>
</dependency>
```

找不到的原因是，从 **JDK9 以后 jdk 中的 javax.annotation 包被移除了**，这两个注解刚好就在这个包中。

#### 3.3.4 bean 标签属性对应的注解总结，知识点 **@PostConstruct 和** @PreDestroy

 **小结**

![](<assets/1740015633933.png>)

**知识点 1：@PostConstruct**

<table><thead><tr><th>名称</th><th>@PostConstruct</th></tr></thead><tbody><tr><td>类型</td><td>方法注解</td></tr><tr><td>位置</td><td>方法上</td></tr><tr><td>作用</td><td>设置该方法为初始化方法</td></tr><tr><td>属性</td><td>无</td></tr></tbody></table>

**知识点 2：@PreDestroy**

<table><thead><tr><th>名称</th><th>@PreDestroy</th></tr></thead><tbody><tr><td>类型</td><td>方法注解</td></tr><tr><td>位置</td><td>方法上</td></tr><tr><td>作用</td><td>设置该方法为销毁方法</td></tr><tr><td>属性</td><td>无</td></tr></tbody></table>

### 3.4 注解开发依赖注入**`@Autowired`**

Spring 为了使用注解简化开发，并没有提供`构造函数注入`、`setter注入`对应的注解，**只提供了自动装配的注解**实现。

#### 3.4.1 环境准备

在学习之前，把案例环境介绍下: 导入 spring 依赖、创建配置类、dao、service 接口和实现类、运行类。

*   创建一个 Maven 项目
    
*   pom.xml 添加 Spring 的依赖
    
    ```
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.10.RELEASE</version>
        </dependency>
    </dependencies>
    ```
    
*   添加一个配置类`SpringConfig`
    
    ```
    @Configuration
    @ComponentScan("com.itheima")
    public class SpringConfig {
    }
    ```
    
*   添加 BookDao、BookDaoImpl、BookService、BookServiceImpl 类
    
    ```
    public interface BookDao {
        public void save();
    }
    @Repository
    public class BookDaoImpl implements BookDao {
        public void save() {
            System.out.println("book dao save ..." );
        }
    }
    public interface BookService {
        public void save();
    }
    @Service
    public class BookServiceImpl implements BookService {
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
    
*   创建运行类 App
    
    ```
    public class App {
        public static void main(String[] args) {
            AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
            BookService bookService = ctx.getBean(BookService.class);
            bookService.save();
        }
    }
    ```
    

最终创建好的项目结构如下:

![](<assets/1740015634422.png>)

环境准备好后，运行后会发现有问题

![](<assets/1740015634597.png>)

**出现问题的原因是没有注入**。在 BookServiceImpl 类中添加了 BookDao 的属性，并提供了 setter 方法，但是目前是没有提供配置注入 BookDao 的，所以 **bookDao 对象为 Null**, 调用其 save 方法就会报`控指针异常`。

#### 3.4.2 按照类型注入（默认）

**`@Autowired`默认按照类型自动注入**

在 BookServiceImpl 类的 **bookDao 属性上添加`@Autowired`注解，删掉 setter 方法**，即可完成按照类型自动注入：

```
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;
//        setter方法可以删除
//      public void setBookDao(BookDao bookDao) {
//        this.bookDao = bookDao;
//    }
    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```

回顾 **xml 按类型自动配置注入**

```
<bean autowire="byType"/>


```

xml 自动注入的类必须有 setter 方法，优先级低于构造方法、setter 注入。

**而注解自动注入的类不用 setter 方法。**

**注意:**

*   @Autowired 可以写在属性上，也可也写在 setter 方法上，建议**`写在属性上并将setter方法删除掉`**
*   **为什么 setter 方法可以删除呢?**
    *   **自动装配基于反射设计创建对象**并通过暴力反射为私有属性进行设值
    *   普通反射只能获取 public 修饰的内容
    *   **暴力反射**除了获取 public 修饰的内容还**可以获取 private 修改的内容**
    *   所以此处无需提供 setter 方法

**BookDao 接口如果有多个未注解 id 的实现类直接运行会报错。**比如添加 BookDaoImpl2

```
@Repository
public class BookDaoImpl2 implements BookDao {
    public void save() {
        System.out.println("book dao save ...2");
    }
}
```

```
public class App {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookService bookService = ctx.getBean(BookService.class);
        bookService.save();
    }
}
```

这个时候再次运行 App，就会**报错**

![](<assets/1740015634804.png>)

此时，按照类型注入就无法区分到底注入哪个对象，**解决方案:`按照名称注入`**

**在类型不唯一时，自动注入将改为按照名称注入，此时名称必须和 service 里的引用名一样：** 

两个 Dao 类之间有一个 id 和 service 里创建的引用名一样，就可以注入成功：service 会调用 id 名和引用

```
@Repository("bookDao")
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ..." );
    }
}
@Repository("bookDao2")
public class BookDaoImpl2 implements BookDao {
    public void save() {
        System.out.println("book dao save ...2" );
    }
}
@Service
public class BookServiceImpl implements BookService {
 
    @Autowired
    private BookDao bookDao;
 
 
    @Override
    public void save() {
 
        System.out.println("service save...");
        bookDao.save();
    }
}
```

```
public class App {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookService bookService = ctx.getBean(BookService.class);
        bookService.save();
    }
}
```

![](<assets/1740015635074.png>)

**当只有一个 dao 实现类时，即使 id 和 service 里创建的引用不一样，也可以成功注入，此时是按类型注入。**

**思考:**

*   @Autowired 是按照类型注入的，给 BookDao 的两个实现起了名称，它还是有两个 bean 对象，为什么不报错?
    
*   **@Autowired 默认按照类型自动装配，如果 IOC 容器中同类的 Bean 找到多个，就按照变量名和 Bean 的名称匹配。**因为变量名叫`bookDao`而容器中也有一个`booDao`，所以可以成功注入。
    
*   分析下面这种情况是否能完成注入呢?
    
    ![](<assets/1740015635259.png>)
    
*   不行，因为按照类型会找到多个 bean 对象，此时会按照`bookDao`名称去找，因为 IOC 容器只有名称叫`bookDao1`和`bookDao2`, 所以找不到，会报`NoUniqueBeanDefinitionException`
    

#### 3.4.3 按照名称注入 @Qualifier

当根据类型在容器中找到多个 bean, 注入参数的属性名又和容器中 bean 的名称不一致，这个时候就需要使用到`@Qualifier`来指定注入哪个名称的 bean 对象。

```
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    @Qualifier("bookDao1")
    private BookDao bookDao;
 
    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```

**注意：**

*   如果不用 @Qualifier，按类型找到多个 bean 时，会注入 id 名与 service 里引用名一样的 dao。@Qualifier 可以实现指定注入 bean 的 id。
*   **@Qualifier 不能独立使用，必须和 @Autowired 一起使用**

@Qualifier 注解后的值就是需要注入的 bean 的名称。

#### 3.4.4 简单数据类型注入**`@Value`**

简单类型注入的是基本数据类型或者字符串类型，下面在`BookDaoImpl`类中添加一个`name`属性，用其进行简单类型注入

```
@Repository("bookDao")
public class BookDaoImpl implements BookDao {
    private String name;
    public void save() {
        System.out.println("book dao save ..." + name);
    }
}
```

数据类型换了，对应的注解也要跟着换，这次使用**`@Value`注解**，将值写入注解的参数中就行了

```
@Repository("bookDao")
public class BookDaoImpl implements BookDao {
    @Value("itheima")
    private String name;
    public void save() {
        System.out.println("book dao save ..." + name);
    }
}
```

注意**数据格式要匹配**，如将 "abc" 注入给 int 值，这样程序就会报错。

**思考：**

@Value **跟直接赋值似乎是一个效果**，还没有直接赋值简单，所以这个注解存在的意义是什么?

**答：**@Value 一般用于读取 properties 配置文件，可以动态更改简单数据类型的值，而直接赋值是写死的。

#### 3.4.5 注解读取 properties 配置文件`@PropertySource`

**只需要在配置类配置`@PropertySource，其他所有类`都可以`@Value("{xxx}")`从 properties 配置文件中读取内容进行使用。**

**步骤 1：resource 下准备 properties 文件**

jdbc.properties

```
name=itheima888


```

**步骤 2: 配置类注解`@PropertySource`加载 properties 配置文件**

在配置类上添加`@PropertySource`注解

```
@Configuration
@ComponentScan("com.itheima")
@PropertySource("jdbc.properties")
//@PropertySource("classpath:jdbc.properties")
public class SpringConfig {
}
```

**步骤 3：使用 @Value 读取配置文件中的内容**

```
@Repository("bookDao")
public class BookDaoImpl implements BookDao {
    @Value("${name}")
    private String name;
    public void save() {
        System.out.println("book dao save ..." + name);
    }
}
```

**步骤 4: 运行程序**

运行 App 类，查看运行结果，说明配置文件中的内容已经被加载到

![](<assets/1740015635463.png>)

**注意:**

*   如果读取的 properties 配置文件有多个，**可以像 @ComponentScan 以数组形式注解**
    
    ```
    @PropertySource({"jdbc.properties","xxx.properties"})
    
    
    ```
    
*   `@PropertySource`注解属性中**不支持使用通配符`*`**, 运行会报错。
    
    ```
    @PropertySource({"*.properties"})
    
    
    ```
    
*   `@PropertySource`注解属性中可以把**`classpath:`**加上, 代表从当前项目的根路径找文件
    
*   ```
    @PropertySource({"classpath:jdbc.properties"})
    
    
    ```
    
*    **回顾 xml** 读取 properties 配置文件，**支持使用通配符`*`**：

```
    <!--方式四：最推荐，根路径（当前工程、依赖的jar包）通配符配置-->
    <context:property-placeholder location="classpath*:*.properties" system-properties-mode="NEVER"/>
```

**问题：**

注解设置系统属性方法暂存疑，就尽量别在 properties 里直接出现 username，写成 jdbc.username 之类的样子。

下面是 xml 方法：

```
<context:property-placeholder location="jdbc.properties" system-properties-mode="NEVER"/>

``` 

#### 3.4.6 知识点 **@Autowired,@Qualifier,@Value,@PropertySource**

**知识点 1：@Autowired**

<table><thead><tr><th>名称</th><th>@Autowired</th></tr></thead><tbody><tr><td>类型</td><td>属性注解 或 方法注解（了解） 或 方法形参注解（了解）</td></tr><tr><td>位置</td><td>属性定义上方 或 标准 set 方法上方 或 类 set 方法上方 或 方法形参前面</td></tr><tr><td>作用</td><td>为引用类型属性设置值</td></tr><tr><td>属性</td><td>required：true/false，定义该属性是否允许为 null</td></tr></tbody></table>

**知识点 2：@Qualifier**

<table><thead><tr><th>名称</th><th>@Qualifier</th></tr></thead><tbody><tr><td>类型</td><td>属性注解 或 方法注解（了解）</td></tr><tr><td>位置</td><td>属性定义上方 或 标准 set 方法上方 或 类 set 方法上方</td></tr><tr><td>作用</td><td>为引用类型属性指定注入的 beanId</td></tr><tr><td>属性</td><td>value（默认）：设置注入的 beanId</td></tr></tbody></table>

**知识点 3：@Value**

<table><thead><tr><th>名称</th><th>@Value</th></tr></thead><tbody><tr><td>类型</td><td>属性注解 或 方法注解（了解）</td></tr><tr><td>位置</td><td>属性定义上方 或 标准 set 方法上方 或 类 set 方法上方</td></tr><tr><td>作用</td><td>为 基本数据类型 或 字符串类型 属性设置值</td></tr><tr><td>属性</td><td>value（默认）：要注入的属性值</td></tr></tbody></table>

**知识点 4：@PropertySource**

<table><thead><tr><th>名称</th><th>@PropertySource</th></tr></thead><tbody><tr><td>类型</td><td>类注解</td></tr><tr><td>位置</td><td>类定义上方</td></tr><tr><td>作用</td><td>加载 properties 文件中的属性值</td></tr><tr><td>属性</td><td>value（默认）：设置加载的 properties 文件对应的文件名或文件名组成的数组</td></tr></tbody></table>

###   
3.5 Bean 的默认名称

*   **使用 @Component 注册的 Bean**：默认名称是首字母小写后的类名。例如 TestServiceImpl 的默认 Bean 名称是 testServiceImpl
*   **使用 @Bean 注册的 Bean**：默认名称是注册方法名。

使用 SpringBoot 框架验证：

```
public interface TestService {
}
/**
 * @Author: vince
 * @CreateTime: 2024/10/23
 * @Description: 默认Bean名testServiceImpl1
 * @Version: 1.0
 */
@Component
 
public class TestServiceImpl1 implements TestService{
}
/**
 * @Author: vince
 * @CreateTime: 2024/10/23
 * @Description: 默认Bean名testServiceImpl2
 * @Version: 1.0
 */
@Component
 
public class TestServiceImpl2 implements TestService{
}
@Configuration
public class TestConfiguration {
    /**
     * 默认Bean名testServiceHah
     * @return {@link TestService }
     */
    @Bean
    public TestService testServiceHah() {
        return new TestServiceImpl3();
    }
}
/**
 * @Author: vince
 * @CreateTime: 2024/10/23
 * @Description: 默认Bean名testServiceHah
 * @Version: 1.0
 */
public class TestServiceImpl3 implements TestService{
}
```

查看所有 Bean：

```
 
@SpringBootTest
class DemoApplicationTests {
    @Autowired
    private ApplicationContext applicationContext;
    @Test
    void contextLoads() {
        String[] beanDefinitionNames = applicationContext.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
                System.out.println(beanDefinitionName);
        }
    }
 
}
```

![](<assets/1740015635650.png>)

## 4，IOC/DI 注解开发管理第三方 bean，Druid

前面定义 bean 的时候都是在自己开发的类上面写个注解就完成了，但如果是第三方的类，这些类都是在 jar 包中，我们没有办法在类上面添加注解，这个时候该怎么办?

遇到上述问题，我们就需要有一种更加灵活的方式来定义 bean, 这种方式不能在原始代码上面书写注解，一样能定义 bean, 这就用到了一个全新的注解 **@Bean**。

这个注解该如何使用呢?

咱们把之前使用配置方式管理的数据源使用注解再来一遍，通过这个案例来学习下 @Bean 的使用。

### 4.1 环境准备

学习 @Bean 注解之前先来准备环境: 导入 spring 依赖、创建配置类、dao 接口、实现类

*   创建一个 Maven 项目
    
*   pom.xml 添加 Spring 的依赖
    
    ```
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.10.RELEASE</version>
        </dependency>
    </dependencies>
    ```
    
*   添加一个配置类`SpringConfig`
    
    ```
    @Configuration
    public class SpringConfig {
    }
    ```
    
*   添加 BookDao、BookDaoImpl 类
    
    ```
    public interface BookDao {
        public void save();
    }
    @Repository
    public class BookDaoImpl implements BookDao {
        public void save() {
            System.out.println("book dao save ..." );
        }
    }
    ```
    
*   创建运行类 App
    
    ```
    public class App {
        public static void main(String[] args) {
            AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        }
    }
    ```
    

最终创建好的项目结构如下:

![](<assets/1740015635930.png>)

### 4.2 不使用外部配置类管理第三方 bean

在上述环境中完成对`Druid`数据源的管理，具体的实现步骤为:

**步骤 1: 导入 druid 的 jar 包**

```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.16</version>
</dependency>
```

**步骤 2: 在配置类中添加一个方法，设置数据库属性**

方法名设成返回的 bean 名，方法的返回值就是要创建的 Bean 对象类型

```
@Configuration
public class SpringConfig {
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://localhost:3306/spring_db");
        ds.setUsername("root");
        ds.setPassword("root");
        return ds;
    }
}
```

**步骤 3: 在方法上添加`@Bean`注解**

**@Bean 注解的作用：将方法的返回值制作为 Spring 管理的一个 bean 对象**

```
@Configuration
public class SpringConfig {
    @Bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://localhost:3306/spring_db");
        ds.setUsername("root");
        ds.setPassword("root");
        return ds;
    }
}
```

**注意: 不能使用`DataSource ds = new DruidDataSource()`**

因为 DataSource 接口中没有对应的 setter 方法来设置属性。不过能想到多态降低耦合是一个好习惯。

**步骤 4: 从 IOC 容器中获取对象并打印**

```
public class App {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        DataSource dataSource = ctx.getBean(DataSource.class);
        System.out.println(dataSource);
    }
}
```

至此使用 @Bean 来管理第三方 bean 的案例就已经完成。

如果有多个 bean 要被 Spring 管理，直接在配置类中多些几个方法，方法上添加 @Bean 注解即可。

### 4.3 创建并引用外部配置类管理第三方 bean

**如果把所有的第三方 bean 都配置到 Spring 的配置类`SpringConfig`中**，虽然可以，但是**不利于代码阅读和分类管理**。

所有我们将**第三方 bean 放到另外一个配置类**里，`SpringConfig`对这个配置类进行导入 @Import

对于数据源的 bean, 我们新建一个`JdbcConfig`配置类，并把数据源配置到该类下。

```
@Configuration
public class JdbcConfig {
    @Bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://localhost:3306/spring_db");
        ds.setUsername("root");
        ds.setPassword("root");
        return ds;
    }
}
```

现在的问题是，这个配置类如何能被 Spring 配置类加载到，并创建 DataSource 对象在 IOC 容器中?

针对这个问题，**有两个解决方案: 使用包扫描引入（不推荐）、使用`@Import精准`引入（推荐，直接能看出导入了哪些配置类，）。**

#### 4.3.1 方法一：使用包扫描引入，@ComponentScan

**步骤 1: 在 Spring 的配置类上添加包扫描**

```
@Configuration
@ComponentScan("com.itheima.config")
public class SpringConfig {
 
}
```

**步骤 2: 在 JdbcConfig 上添加配置注解**

JdbcConfig 类要放入到`com.itheima.config`包下，需要被 Spring 的配置类扫描到即可

```
@Configuration    //SpringConfig使用了@ComponentScan，这里必须加@Configuration注解
public class JdbcConfig {
    @Bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://localhost:3306/spring_db");
        ds.setUsername("root");
        ds.setPassword("root");
        return ds;
    }
}
```

**步骤 3: 运行程序**

依然能获取到 bean 对象并打印控制台。

这种方式虽然能够扫描到，但是不能很快的知晓都引入了哪些配置类，所有这种方式不推荐使用。

#### 4.3.2 方法二：使用`@Import精准`引入（不推荐）

方案一实现起来有点小复杂，Spring 早就想到了这一点，于是又给我们提供了第二种方案。

**这种方案可以不用加`@Configuration`注解，但是必须在 Spring 配置类上使用`@Import`注解手动引入需要加载的配置类。**但加上也没什么问题，建议加上，能更直观的看出它是配置类。

**步骤 1: 去除 JdbcConfig 类上的注解**

```
//@Configuration        //SpringConfig用了@import精准扫描，这里@Configuration可以不加
public class JdbcConfig {
    @Bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://localhost:3306/spring_db");
        ds.setUsername("root");
        ds.setPassword("root");
        return ds;
    }
}
```

**步骤 2: 在 Spring 配置类中引入字节码文件**

```
@Configuration
//@ComponentScan("com.itheima.config")
@Import({JdbcConfig.class})
public class SpringConfig {
 
}
```

**注意:**

*   扫描注解可以移除
    
*   @Import 参数需要的是一个数组，可以引入多个配置类。
    
*   @Import 注解在配置类中只能写一次，下面的方式是**不允许的**
    
    ```
    @Configuration
    //@ComponentScan("com.itheima.config")
    @Import(JdbcConfig.class)
    @Import(Xxx.class)
    public class SpringConfig {
     
    }
    ```
    

**步骤 3: 运行程序**

依然能获取到 bean 对象并打印控制台

#### 4.3.3 知识点 **@Bean 和 @Import**

**知识点 1：@Bean**

<table><thead><tr><th>名称</th><th>@Bean</th></tr></thead><tbody><tr><td>类型</td><td>方法注解</td></tr><tr><td>位置</td><td>方法定义上方</td></tr><tr><td>作用</td><td>设置该方法的返回值作为 spring 管理的 bean</td></tr><tr><td>属性</td><td>value（默认）：定义 bean 的 id</td></tr></tbody></table>

**知识点 2：@Import**

<table><thead><tr><th>名称</th><th>@Import</th></tr></thead><tbody><tr><td>类型</td><td>类注解</td></tr><tr><td>位置</td><td>类定义上方</td></tr><tr><td>作用</td><td>导入配置类</td></tr><tr><td>属性</td><td>value（默认）：定义导入的配置类类名，<br>当配置类有多个时使用数组格式一次性导入多个配置类</td></tr></tbody></table>

### 4.4 为外部配置类注入资源

在使用 @Bean 创建 bean 对象的时候，如果方法在创建的过程中需要其他资源该怎么办?

这些资源会有两大类，分别是**`简单数据类型` 和`引用数据类型`**。

#### 4.4.1 注入简单数据类型 @Value

**需求分析**

对于下面代码关于数据库的四要素不应该写死在代码中，应该是从 properties 配置文件中读取。

**待优化代码：**

```
public class JdbcConfig {
    @Bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://localhost:3306/spring_db");
        ds.setUsername("root");
        ds.setPassword("root");
        return ds;
    }
}
```

**代码实现**

**步骤 1: 类中提供四个属性**

```
public class JdbcConfig {
    private String driver;
    private String url;
    private String userName;
    private String password;
    @Bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://localhost:3306/spring_db");
        ds.setUsername("root");
        ds.setPassword("root");
        return ds;
    }
}
```

**步骤 2: 使用`@Value`注解引入值**

```
public class JdbcConfig {
    @Value("com.mysql.jdbc.Driver")
    private String driver;
    @Value("jdbc:mysql://localhost:3306/spring_db")
    private String url;
    @Value("root")
    private String userName;
    @Value("password")
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

**扩展**

现在的数据库连接四要素还是写在代码中，需要做的是将这些内容**提取到 jdbc.properties 配置文件**，大家思考下该如何实现?

1.resources 目录下添加 jdbc.properties

2. 配置文件中提供四个键值对分别是数据库的四要素

3. 使用 **@PropertySource 加载 jdbc.properties 配置文件**

4. 修改 **@Value 注解属性**的值，将其修改为**`${key}`**，key 就是键值对中的键的值

#### 4.4.2 注入引用数据类型

结论：以 bean 为形参传入方法中即可。

**需求分析**

**假设在构建 DataSource 对象的时候，需要用到 BookDao 对象**，该如何把 BookDao 对象注入进方法内让其使用呢?

```
public class JdbcConfig {
    @Bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://localhost:3306/spring_db");
        ds.setUsername("root");
        ds.setPassword("root");
        return ds;
    }
}
```

**步骤 1: 在 SpringConfig 中扫描 BookDao**

扫描的目的是让 Spring 能管理到 BookDao, 也就是说要让 IOC 容器中有一个 bookDao 对象

```
@Configuration
@ComponentScan("com.itheima.dao")
@Import({JdbcConfig.class})
public class SpringConfig {
}
```

**步骤 2: 在 JdbcConfig 类的方法上添加参数**

```
//@Configuration
public class JdbcConfig {
    @Bean
    public DataSource dataSource(BookDao bookDao){    //假如要用到bookDao，这样直接以形参的格式就可以自动注入
        System.out.println(bookDao);
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(userName);
        ds.setPassword(password);
        return ds;
    }
}
```

引用类型注入只需要为 bean 定义方法设置形参即可，不用注解 @AutoWired 之类的，**容器会根据类型自动装配对象**。

**步骤 3: 运行程序**

![](<assets/1740015636234.png>)

## 5，xml 配置和注解开发对比

前面我们已经完成了 XML 配置和注解的开发实现，至于两者之间的差异，咱们放在一块去对比回顾下:

![](<assets/1740015636570.png>)

 注解开发最常用：@Service,@ComponentScan,@Autowired,@Bean

## 6，Spring 整合

课程学习到这里，已经对 Spring 有一个简单的认识了，**Spring 有一个容器，叫做 IoC 容器，里面保存 bean**。在进行企业级开发的时候，其实除了将自己写的类让 Spring 管理之外，还有一部分重要的工作就是使用第三方的技术。

下面结合 IoC 和 DI，整合 2 个常用技术，进一步加深对 Spring 的使用理解。

### 6.1 Spring 整合 Mybatis

#### 6.1.1 环境准备（回顾 Mybatis）

在准备环境的过程中，我们也来回顾下 Mybatis 开发的相关内容:

**步骤 1: 准备数据库表**

Mybatis 是来操作数据库表，所以先创建一个数据库及表

```
create database spring_db character set utf8;
use spring_db;
create table tbl_account(
    id int primary key auto_increment,
    name varchar(35),
    money double
);
```

**步骤 2: 创建项目导入 jar 包**

项目的 pom.xml 添加依赖 spring、druid、Mybatis、mysql

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
</dependencies>
```

**步骤 3: 根据表创建模型类**

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
```

**步骤 5: 创建 Service 接口和实现类**

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
```

**步骤 6: 添加 jdbc.properties 文件**

resources 目录下添加，用于配置数据库连接四要素

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/spring_db?useSSL=false
jdbc.username=root
jdbc.password=root
```

useSSL: 关闭 MySQL 的 SSL 连接

**步骤 7: 添加 Mybatis 核心配置文件**

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--读取外部properties配置文件-->
    <properties resource="jdbc.properties"></properties>
    <!--别名扫描的包路径-->
    <typeAliases>
        <package name="com.itheima.domain"/>
    </typeAliases>
    <!--数据源-->
    <environments default="mysql">
        <environment id="mysql">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"></property>
                <property name="url" value="${jdbc.url}"></property>
                <property name="username" value="${jdbc.username}"></property>
                <property name="password" value="${jdbc.password}"></property>
            </dataSource>
        </environment>
    </environments>
    <!--映射文件扫描包路径-->
    <mappers>
        <package name="com.itheima.dao"></package>
    </mappers>
</configuration>
```

**步骤 8: 编写应用程序**

```
public class App {
    public static void main(String[] args) throws IOException {
        // 1. 创建SqlSessionFactoryBuilder对象
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        // 2. 加载SqlMapConfig.xml配置文件
        InputStream inputStream = Resources.getResourceAsStream("SqlMapConfig.xml.bak");
        // 3. 创建SqlSessionFactory对象
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
        // 4. 获取SqlSession
        SqlSession sqlSession = sqlSessionFactory.openSession();
        // 5. 执行SqlSession对象执行查询，获取结果User
        AccountDao accountDao = sqlSession.getMapper(AccountDao.class);
 
        Account ac = accountDao.findById(1);
        System.out.println(ac);
 
        // 6. 释放资源
        sqlSession.close();
    }
}
```

**步骤 9: 运行程序**

![](<assets/1740015636822.png>)

#### 6.1.2 整合思路分析

Mybatis 的基础环境我们已经准备好了，接下来就得分析下在上述的内容中，哪些对象可以交给 Spring 来管理?

*   Mybatis 程序核心对象分析
    
    ![](<assets/1740015636999.png>)
    
    从图中可以获取到，真正需要交给 Spring 管理的是 **SqlSessionFactory**
    
*   整合 Mybatis，就是将 Mybatis 用到的内容交给 Spring 管理，**分析下配置文件**
    
    ![](<assets/1740015637233.png>)
    

**回顾：**

**类型别名 typeAliases：**给指定包下所有类起别名，这样在 sql 映射文件中 resultType 里就可以直接写类名，不用写前面的包路径。

**mappers 映射器标签：**扫描包下的 mapper 类，让程序知道去哪里找 sql 映射文件。

**整合 spring 分析:**

*   第一行读取外部 properties 配置文件，Spring 有提供具体的解决方案**`@PropertySource`**, 需要交给 Spring
*   第二行起别名**包扫描**，为 SqlSessionFactory 服务的，**需要交给 Spring**
*   第三行主要用于做连接池，Spring 之前我们已经整合了 Druid 连接池，这块也需要交给 Spring
*   前面三行一起都是为了创建 SqlSession 对象用的，那么用 Spring 管理 SqlSession 对象吗? 回忆下 SqlSession 是由 SqlSessionFactory 创建出来的，所以**只需要将 SqlSessionFactory 交给 Spring 管理即可。**
*   第四行是 **Mapper 接口和映射文件** [如果使用注解就没有该映射文件]，这个是在获取到 SqlSession 以后执行具体操作的时候用，所以它和 SqlSessionFactory 创建的时机都不在同一个时间，可能需要**单独管理**。

#### 6.1.2 代码实现

**总结**

1.MybatisConfig 里两个类 **SqlSessionFactoryBean,MapperScannerConfigurer 替换 mybatis-config.xml**

2. 基于上面配置文件，service 直接通过 DI 调用 dao 方法。不需要引用工具类 sqlSessionFactoryUtil，不用 mapper。

**项目结构：**

![](<assets/1740015637625.png>)

 **整合的核心是两件事：**

**第一件事是:**Spring 要管理 MyBatis 中的 **SqlSessionFactory**，SqlSessionFactory 需要 typeAliases,environments

**第二件事是:**Spring 要管理 **Mapper 接口的扫描**

具体该如何实现，具体的步骤为:

**步骤 1: 项目中导入整合需要的 spring-jdbc、mybatis-spring 依赖**

**前面已导入 spring-context,mybatis 包，现在是另两个整合包，**spring 自带的整合 jdbc 的包，和第三方 Mybatis 整合 spring 的包。

```
<dependency>
    <!--Spring操作数据库需要该jar包-->
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.10.RELEASE</version>
</dependency>
<dependency>
    <!--
        Spring与Mybatis整合的jar包
        这个jar包mybatis在前面，是Mybatis提供的
    -->
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>1.3.0</version>
</dependency>
```

**步骤 2: 创建 Spring 的主配置类**

```
//配置类注解
@Configuration
//包扫描，根据定义的扫描路径,把符合扫描规则的类作为Bean装配到IOC容器中
@ComponentScan("com.itheima")
public class SpringConfig {
}
```

**步骤 3: 创建数据源的配置类**

在配置类中完成数据源的创建

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

**步骤 4: 主配置类中读 properties 并引入数据源配置类**

```
@Configuration
@ComponentScan("com.itheima")
@PropertySource("classpath:jdbc.properties")
@Import(JdbcConfig.class)
public class SpringConfig {
}
```

**步骤 5: 创建 Mybatis 配置类并配置 SqlSessionFactory**

```
public class MybatisConfig {
    //定义bean，SqlSessionFactoryBean，用于快速创建SqlSessionFactory对象
    @Bean
    //SqlSessionFactoryBean内部实现FactoryBean接口的getObject方法返回SqlSessionFactory，所以这里返回SqlSessionFactoryBean即可
    //为外部配置类注入引用数据类型DataSource，IOC里是有DataSource对象的，有扫描第三方配置类JdbcConfg
    public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource){
        //SqlSessionFactoryBean是FactoryBean的子类，用于封装sqlSessionFactory的环境信息，Mybatis核心配置中的typeAliases,environments
        //FactoryBean是一种创建bean的方法
        SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();
        //设置模型类的别名扫描
        ssfb.setTypeAliasesPackage("com.itheima.domain");
        //设置数据源，这里数据源对象是bean
        ssfb.setDataSource(dataSource);
        //应该还有一步设置事务处理，暂且不设置，前面导入的spring-jdbc包是可以事务处理的
        return ssfb;
    }
    //定义bean，返回映射扫描配置器对象MapperScannerConfigurer，设置映射在哪个包下。
    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
        MapperScannerConfigurer msc = new MapperScannerConfigurer();
        msc.setBasePackage("com.itheima.dao");
        return msc;
    }
}
```

**说明:**

*   **SqlSessionFactoryBean 封装** SqlSessionFactory 需要的**环境信息**
    
    ![](<assets/1740015637885.png>)
    
    *   **SqlSessionFactoryBean** 是 **FactoryBean 的一个子类**，该类将 **SqlSessionFactory 的创建**进行了封装，简化对象的创建，我们只需要将其需要的 **typeAliases,environments** 设置即可。
    *   因为 SqlSessionFactoryBean 内部实现的 getObject 方法返回值是 SqlSessionFactory，所以最终返回值是 SqlSessionFactory
        
        ![](<assets/1740015638128.png>)
        
    *   **回顾 FactoryBean<> 接口创建 bean 的方法：**
        
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
        
    *   **SqlSessionFactoryBean** 方法中有一个参数为 dataSource, 当前 Spring 容器中已经创建了 Druid 数据源，类型刚好是 DataSource 类型，此时在初 ` 始化 SqlSessionFactoryBean 这个对象的时候，发现需要使用 DataSource 对象，而容器中刚好有这么一个对象，就自动加载了 DruidDataSource 对象。
    
*   使用**映射扫描配置器 MapperScannerConfigurer** 加载 Dao 接口，创建代理对象保存到 IOC 容器中
    
    ![](<assets/1740015638288.png>)
    
    *   这个 MapperScannerConfigurer 对象也是 MyBatis 提供的专用于整合的 jar 包中的类，用来处理原始配置文件中的 mappers 相关配置，**加载数据层的 Mapper 接口类**
    *   MapperScannerConfigurer 有一个核心属性 **basePackage**，就是用来设置所扫描的包路径

**步骤 6: 主配置类中引入 Mybatis 配置类**

```
@Configuration
@ComponentScan("com.itheima")
@PropertySource("classpath:jdbc.properties")
@Import({JdbcConfig.class,MybatisConfig.class})
public class SpringConfig {
}
```

**步骤 7：配置 MybatisConfig 后 service 就可以直接调用 dao 的方法**

因为 SqlSessionFactory 和 MapperScannerConfigurer 已经在 bean 里了。

```
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
```

**步骤 8: 编写运行类**

在运行类中，从 IOC 容器中获取 Service 对象，调用方法获取结果

```
public class App2 {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
 
        AccountService accountService = ctx.getBean(AccountService.class);
 
        Account ac = accountService.findById(1);
        System.out.println(ac);
    }
}
```

**步骤 9: 运行程序**

![](<assets/1740015638527.png>)

支持 Spring 与 Mybatis 的整合就已经完成了，其中主要用到的两个类分别是:

*   **SqlSessionFactoryBean**
*   **MapperScannerConfigurer**

### 6.3 Spring 整合 Junit

**整合 Junit 与整合 Druid 和 MyBatis 差异比较大**，为什么呢？Junit 是一个搞**单元测试**用的工具，它不是我们程序的主体，也不会参加最终程序的运行，从作用上来说就和之前的东西不一样，它不是做功能的，看做是一个辅助工具就可以了。

#### 6.3.1 环境准备

直接使用 Spring 与 Mybatis 整合的环境。当然也可以重新创建一个，因为内容是一模一样。

**项目结构**:

![](<assets/1740015638799.png>)

#### 6.3.2 整合 Junit 步骤

在上述环境的基础上，我们来对 Junit 进行整合。

**步骤 1: 引入依赖 Junit 和 spring-test**

pom.xml

```
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
```

**步骤 2: 编写测试类**

在 test\java 下创建一个 AccountServiceTest

```
//设置类运行器
@RunWith(SpringJUnit4ClassRunner.class)
//设置Spring环境对应的配置类
@ContextConfiguration(classes = {SpringConfig.class}) //加载配置类
//@ContextConfiguration(locations={"classpath:applicationContext.xml"})//加载配置文件
public class AccountServiceTest {
    //自动装配注入要测试的bean
    @Autowired
    private AccountService accountService;
    @Test
    public void testFindById(){
        System.out.println(accountService.findById(1));
 
    }
    @Test
    public void testFindAll(){
        System.out.println(accountService.findAll());
    }
}
```

**注意:**

*   单元测试，**如果测试的是注解配置类**，则使用**`@ContextConfiguration(classes = 配置类.class)`**
*   单元测试，**如果测试的是配置文件**，则使用**`@ContextConfiguration(locations={配置文件名,...})`**
*   Junit 运行后是基于 Spring 环境运行的，所以 Spring 提供了一个**专用的类运行器**，这个务必要设置，这个类运行器就在 Spring 的测试专用包中提供的，导入的坐标就是这个东西`SpringJUnit4ClassRunner`
*   上面两个配置都是**固定格式**，当需要测试哪个 bean 时，使用自动装配加载对应的对象，下面的工作就和以前做 Junit 单元测试完全一样了

#### 知识点 1：@RunWith

<table><thead><tr><th>名称</th><th>@RunWith</th></tr></thead><tbody><tr><td>类型</td><td>测试类注解</td></tr><tr><td>位置</td><td>测试类定义上方</td></tr><tr><td>作用</td><td>设置 JUnit 运行器</td></tr><tr><td>属性</td><td>value（默认）：运行所使用的运行期</td></tr></tbody></table>

#### 知识点 2：@ContextConfiguration

<table><thead><tr><th>名称</th><th>@ContextConfiguration</th></tr></thead><tbody><tr><td>类型</td><td>测试类注解</td></tr><tr><td>位置</td><td>测试类定义上方</td></tr><tr><td>作用</td><td>设置 JUnit 加载的 Spring 核心配置</td></tr><tr><td>属性</td><td>classes：核心配置类，可以使用数组的格式设定加载多个配置类<br>locations: 配置文件，可以使用数组的格式设定加载多个配置文件名称</td></tr></tbody></table>