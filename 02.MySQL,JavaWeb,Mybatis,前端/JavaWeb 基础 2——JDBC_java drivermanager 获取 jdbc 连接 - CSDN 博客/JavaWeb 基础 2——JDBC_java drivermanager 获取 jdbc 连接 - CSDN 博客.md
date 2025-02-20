---
url: https://blog.csdn.net/qq_40991313/article/details/125706218
title: JavaWeb 基础 2——JDBC_java drivermanager 获取 jdbc 连接 - CSDN 博客
date: 2025-02-20 09:37:24
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[一、JDBC](#t0)

[1.1 JDBC 概述](#t1)

[1.1.1 JDBC 概念](#t2)

[1.1.2 JDBC 本质](#t3)

[1.1.3 JDBC 好处](#t4)

[1.2 JDBC 步骤](#t5)

[1.2.1 标准代码](#t6)

[1.2.2 编写代码步骤](#t7)

[1.3 获取数据库连接的多种方式](#t8)

[二、JDBC 所有 API](#t9)

[2.1 驱动管理类 DriverManager](#t10)

[2.2 数据库连接对象 Connection](#t11)

[2.2.1 获取执行对象](#t12)

[2.2.2 事务管理](#t13)

[2.3 Statement](#t14)

[2.3.1 结果集对象 ResultSet](#t15)

[2.3.2 SQL 注入问题](#t16)

[2.4 PreparedStatement](#t17)

[2.4.1 概述](#t18) 

[2.4.2 PreparedStatement 原理](#t19)

[2.5 数据库连接池 DataSource](#t20)

[2.5.1 数据库连接池简介](#t21)

[2.5.2 数据库连接池实现](#t22)

[2.5.3 Driud 下载](#t23)

[2.5.4 Driud 使用](#t24)

[2.5.5 Druid 配置文件详解](#t25)

[三、JDBC 练习](#t26)

[3.1 需求：完成商品品牌数据的增删改查操作](#t27)

[3.2 案例实现](#t28)

## 一、JDBC

### 1.1 JDBC 概述

#### 1.1.1 JDBC 概念

**了解即可**，后面开发都用 Mybatis 框架。MyBatis 是一个持久层 ORM 框架, 底层是对 JDBC 的封装。

JDBC 是 Java 提供的一个**操作数据库**的 API; 

**全称：**(Java DataBase Connectivity) **Java 数据库连接**

**为什么要用 jdbc？**

我们开发的**同一套 Java 代码无法操作不同的关系型数据库**，因为每一个关系型数据库的底层实现细节都不一样。为了解决这个问题，JDBC 中定义了**所有操作关系型数据库的规则（接口）**。

众所周知接口是无法直接使用的，我们需要使用接口的实现类，而这套**实现类（称之为：驱动）**就由各自的数据库厂商给出。

#### 1.1.2 JDBC 本质

*   官方（sun 公司）定义的一套**操作所有关系型数据库的规则，即接口**
    
*   各个数据库厂商去实现这套接口，提供**数据库驱动 jar 包**
    
*   我们可以使用这套接口（JDBC）编程，真正执行的代码是**驱动 jar 包中的实现类**
    

#### 1.1.3 JDBC 好处

*   各数据库厂商使用相同的接口，Java 代码不需要针对不同数据库分别开发
    
*   **可随时替换底层数据库**，访问数据库的 Java 代码基本不变
    

以后编写操作数据库的代码只需要面向 JDBC（接口），操作哪儿个关系型数据库就需要导入该数据库的驱动包，如需要操作 MySQL 数据库，就需要再项目中导入 MySQL 数据库的驱动包。如下图就是 MySQL 驱动包：

![](<assets/1740015444641.png>)

### 1.2 JDBC 步骤

 通过 Java 操作数据库的流程：

![](<assets/1740015444829.png>)

第一步：编写 Java 代码

第二步：Java 代码将 SQL 发送到 MySQL 服务端

第三步：MySQL 服务端接收到 SQL 语句并执行该 SQL 语句

第四步：将 SQL 语句执行的结果返回给 Java 代码

#### 1.2.1 标准代码

 jdbc 整个技术点**了解即可**，后面更多用 Mybatis 框架简化了 jdbc 开发，Mybatis 整合 spring 后代码还有改变。

**剧透 Mybatis：**下面是 Mybatis 框架操作数据库代码，下一章详细讲，**现在看看就行**

[JavaWeb 基础 3——Maven&MyBatis_vincewm 的博客 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/125818307?spm=1001.2014.3001.5501 "JavaWeb基础3——Maven&MyBatis_vincewm的博客-CSDN博客")

```
 
public class Demo {
 
    public static void main(String[] args) throws IOException {
        //1. 加载mybatis的核心配置文件，获取 SqlSessionFactory
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
 
        //2. 获取SqlSession对象，用它来执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //3. 执行sql
        //参数是一个字符串，该字符串必须是映射配置文件的namespace.id
//        List<User> users = sqlSession.selectList("test.selectAll");
        //3.1获取UserMapper接口的代理对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        //3.2调用sql方法
        List<User> users = userMapper.selectAll();
        System.out.println(users);
        //4. 释放资源
        sqlSession.close();
    }
}
```

**简单版：**

```
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;
 
/**
 * 简单版
 */
public class Demo {
 
    public static void main(String[] args) throws Exception {
        //1. 注册驱动
        
        //Class.forName("com.mysql.jdbc.Driver");//通过反射获取Driver实现类对象，从而加载驱动，注册驱动语句DriverManager.registerDriver(driver)在Driver的静态代码块里做过了。
        //此句仅在mysql可省略，其他数据库不能省略
        //2. 获取连接
        String url = "jdbc:mysql://127.0.0.1:3306/db1?useSSL=false";    //?useSSL=false禁用安全连接方式，解决警告提示
        String username = "root";
        String passWord = "1234";
        Connection conn = DriverManager.getConnection(url, username, passWord);
        //3. 定义sql
        String sql = "update student set age = 80 where name='xiaohua'";
        //4. 获取执行sql的对象 Statement
        Statement stmt = conn.createStatement();
        //5. 执行sql
        int count = stmt.executeUpdate(sql);//受影响的行数
        //6. 处理结果
        System.out.println(count);
        //7. 释放资源
        stmt.close();
        conn.close();
    }
}
```

**优化版，**Properties 和 PreparedStatement：

```
import java.io.FileReader;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.Statement;
import java.util.Properties;
 
/**
 * 优化版
 */
public class Demo {
 
    public static void main(String[] args) throws Exception {
        Properties prop=new Properties();
        //System.out.println(System.getProperty("user.dir")); //获取当前路径
        FileReader fileReader = new FileReader("module2/src/jdbc.properties");
        prop.load(fileReader);
 
        //1. 注册驱动
//        Class.forName(prop.getProperty("driverClass"));
        //2. 获取连接
        String url = prop.getProperty("url");
        String username = prop.getProperty("username");
        String password = prop.getProperty("password");
        Connection conn = DriverManager.getConnection(url, username, password);
        //3. 定义sql
        //4. 获取执行sql的对象 Statement
        PreparedStatement pstmt = conn.prepareStatement("update student set age = ? where name=?"); //PreparedStatement防止sql注入
        pstmt.setInt(1,30);pstmt.setString(2,"xiaoming");
        //5. 执行sql
        int count = pstmt.executeUpdate();//受影响的行数
        //6. 处理结果
        System.out.println(count);
        //7. 释放资源
        pstmt.close();
        conn.close();
    }
}
```

```
#jdbc.properties
username=root
password=1234
url=jdbc:mysql://127.0.0.1:3306/db1
driverClass=com.mysql.jdbc.Driver
```

#### 1.2.2 编写代码步骤

**0. 导入驱动 jar 包**。

![](<assets/1740015445162.png>)

**为什么要导包？**

因为 jdbc 本质是关系型数据库接口，需要导入数据库的实现类的包才能连接数据库。

**方法**：将 mysql 的驱动包放在模块下的 lib 目录（随意命名）下，右键 add as library，也就是将该 jar 包添加为库文件，选择模块有效，name 为空就行。

**地址**：[MySQL :: Download Connector/J](https://dev.mysql.com/downloads/connector/j/ "MySQL :: Download Connector/J")，选择 **Platform Independent。**

在添加为库文件的时候，有如下三个选项

Global Library ： 全局有效

Project Library : 项目有效

Module Library ： 模块有效

**1. 加载并注册驱动**

```
Class.forName("com.mysql.jdbc.Driver");    //把mysql的驱动类Driver的对象加载进内存并初始化

```

**驱动类 Driver 的方法：**

*   *   <table border="0" cellpadding="3" cellspacing="0"><tbody><tr><td><code onclick="mdcp.copyCode(event)">static <a href="../../java/lang/Class.html" rel="nofollow" target="_blank">类</a>&lt;?&gt;</code></td><td><code onclick="mdcp.copyCode(event)"><a href="../../java/lang/Class.html#forName-java.lang.String-" rel="nofollow" target="_blank">forName</a>(<a href="../../java/lang/String.html" rel="nofollow" target="_blank">String</a>&nbsp;className)</code><p>返回与给定字符串名称的类或接口相关联的 <code onclick="mdcp.copyCode(event)">类</code>对象。</p></td></tr></tbody></table>
        

JDBC 中定义了所有操作关系型数据库的规则（接口），**驱动类就是这些接口的实现类。** 

 **Class.forName 方法的作用**：初始化驱动类。

**MySQL 中此行代码可以省略：** 

而我们给定的 MySQL 的 Driver 类中，它在**静态代码块**中通过 JDBC 的 DriverManager 注册了一下驱动。我们也可以直接使用 JDBC 的驱动管理器 DriverManager 注册 mysql 驱动，从而代替使用 Class.forName。

```
DriverManager.registerDriver(new Driver());

```

**2. 获取连接**（获取 java 与 mysql 服务端之间的连接）

```
Connection conn = DriverManager.getConnection(url, username, password);

```

驱动管理器类 DriverManager 的方法：

*   *   <table border="0" cellpadding="3" cellspacing="0"><tbody><tr><td><code onclick="mdcp.copyCode(event)">static <a href="../../java/sql/Connection.html" rel="nofollow" target="_blank">Connection</a></code></td><td><code onclick="mdcp.copyCode(event)"><a href="../../java/sql/DriverManager.html#getConnection-java.lang.String-java.lang.String-java.lang.String-" rel="nofollow" target="_blank">getConnection</a>(<a href="../../java/lang/String.html" rel="nofollow" target="_blank">String</a>&nbsp;url, <a href="../../java/lang/String.html" rel="nofollow" target="_blank">String</a>&nbsp;user, <a href="../../java/lang/String.html" rel="nofollow" target="_blank">String</a>&nbsp;password)</code><p>尝试建立与给定数据库 URL 的连接。</p></td></tr></tbody></table>
        

Java 代码需要发送 SQL 给 MySQL 服务端，就需要先建立连接

**3. 定义 SQL 语句**

```
String sql =  “update…” ;

```

**4. 获取执行 SQL 对象**

执行 SQL 语句需要 SQL 执行对象，而这个执行对象就是 Statement 对象

```
Statement stmt = conn.createStatement();

```

**5. 执行 SQL**（把 SQL 语句发送到 mysql 服务器，mysql 服务器执行 SQL 语句）

```
stmt.executeUpdate(sql);  

```

6.java 处理返回结果

7. 释放资源

### 1.3 获取数据库连接的多种方式

*   *   <table border="0" cellpadding="3" cellspacing="0"><tbody><tr><td><code onclick="mdcp.copyCode(event)"><a href="../../java/sql/Connection.html" rel="nofollow" target="_blank">Connection</a></code></td><td><code onclick="mdcp.copyCode(event)"><a href="../../java/sql/Driver.html#connect-java.lang.String-java.util.Properties-" rel="nofollow" target="_blank">connect</a>(<a href="../../java/lang/String.html" rel="nofollow" target="_blank">String</a>&nbsp;url, <a href="../../java/util/Properties.html" rel="nofollow" target="_blank">Properties</a>&nbsp;info)</code><p>尝试使数据库连接到给定的 URL。</p></td></tr></tbody></table>
        

**方法一：**通过 Driver 对象获取： 

```
        Driver driver=new com.mysql.jdbc.Driver();  //多态语句，driver是Driver接口对象，com.mysql.jdbc.Driver()是mysql的实现类
        String url="jdbc:mysql://127.0.0.1:3306/db1";   //jdbc是主协议，mysql是子协议，主协议:子协议://ip地址:端口/数据库名
        Properties info=new Properties();
        info.setProperty("user","root");
        info.setProperty("password","1234");
        Connection conn=driver.connect(url,info);
```

**方法二：**通过反射获取： 

```
        Class c=Class.forName("com.mysql.jdbc.Driver");    //通过反射，获取Driver实现类对象c
        Driver driver=(Driver) c.newInstance();            //调用类对象c的构造方法，实现构造方法对象driver的创建
        String url="jdbc:mysql://127.0.0.1:3306/db1";   //jdbc是主协议，mysql是子协议，主协议:子协议://ip地址:端口/数据库名
        Properties info=new Properties();
        info.setProperty("user","root");
        info.setProperty("password","1234");
        Connection conn=driver.connect(url,info);
```

**方法三：**DriverManager 替代 Driver

DriverManager 方法：

*   *   <table border="0" cellpadding="3" cellspacing="0"><tbody><tr><td><code onclick="mdcp.copyCode(event)">static void</code></td><td><code onclick="mdcp.copyCode(event)"><a href="../../java/sql/DriverManager.html#registerDriver-java.sql.Driver-" rel="nofollow" target="_blank">registerDriver</a>(<a href="../../java/sql/Driver.html" rel="nofollow" target="_blank">Driver</a>&nbsp;driver)</code><p>注册与给定的驱动程序 <code onclick="mdcp.copyCode(event)">DriverManager</code> 。</p></td></tr><tr><td><code onclick="mdcp.copyCode(event)">static <a href="../../java/sql/Connection.html" rel="nofollow" target="_blank">Connection</a></code></td><td><code onclick="mdcp.copyCode(event)"><a href="../../java/sql/DriverManager.html#getConnection-java.lang.String-java.lang.String-java.lang.String-" rel="nofollow" target="_blank">getConnection</a>(<a href="../../java/lang/String.html" rel="nofollow" target="_blank">String</a>&nbsp;url, <a href="../../java/lang/String.html" rel="nofollow" target="_blank">String</a>&nbsp;user, <a href="../../java/lang/String.html" rel="nofollow" target="_blank">String</a>&nbsp;password)</code><p>尝试建立与给定数据库 URL 的连接。</p></td></tr></tbody></table>
        

![](<assets/1740015445270.png>)

 **方法四：**可以只是加载驱动，注册驱动在 mysql 的 Driver 实现类静态代码块里做过了，故可以省略相关语句。

```
        //1. 注册驱动
//        Class.forName("com.mysql.jdbc.Driver");
        //2. 获取连接
        String url = "jdbc:mysql://127.0.0.1:3306/db1";
        String username = "root";
        String passWord = "1234";
        Connection conn = DriverManager.getConnection(url, username, passWord);
```

![](<assets/1740015445672.png>)

 **方法五：**

src 下新建 jdbc.properties:

![](<assets/1740015445894.png>)

![](<assets/1740015446200.png>)

## 二、JDBC 所有 API

### 2.1 驱动管理类 DriverManager

**作用**：注册驱动和获取数据库连接

**注册驱动方法：**

![](<assets/1740015446454.png>)

 而在实际连接数据库的时候并不需要 registerDriver，因为 mysql 的 **Driver 实现类**里有**静态代码块**已经 registerDriver 过了：

![](<assets/1740015446677.png>)

 因此我们只需要加载 `Driver` 类进内存，该静态代码块就会执行：

```
// 将类Driver加载到内存，在内存会产生一个和类Driver对应的Class实例
Class class = Class.forName("com.mysql.jdbc.Driver");
```

**提示：**mysql 加载 Driver 类方法可以省略

*   MySQL 5 之后的驱动包，可以省略注册驱动的步骤
    
*   自动加载 jar 包中 META-INF/services/java.sql.Driver 文件中的驱动类
    

DriverManager 类，**获取数据库连接**方法：

![](<assets/1740015446940.png>)

**url** ： 连接路径

**语法：**主协议 jdbc: 子协议 mysql://ip 地址 (域名): 端口号 / 数据库名称? 参数键值对 1 & 参数键值对 2…

**示例：**jdbc:mysql://127.0.0.1:3306/db1

jdbc:mysql://localhost:3306/db1

**细节**：

*   **url 简写依据：**如果连接的是本机 mysql 服务器，并且 mysql 服务默认端口是 3306，则 url 可以**简写**为：jdbc:mysql:/// 数据库名称? 参数键值对，示例：jdbc:mysql:///db1?useSSL=false
    
*   配置 **useSSL=false** 参数，禁用安全连接方式，解决警告提示，示例：jdbc:mysql://127.0.0.1:3306/db1?useSSL=false
    

### 2.2 数据库连接对象 Connection

```
        //4. 获取执行sql的对象 Statement
 
        Statement stmt = conn.createStatement();
        //Statement对象：用于执行静态SQL语句并返回其生成的结果的对象。 
```

**Connection（数据库连接对象）作用：**

*   获取执行 SQL 的对象
    
*   管理事务
    

#### 2.2.1 获取执行对象

*   **普通执行 SQL 对象**
    
    ```
    Statement createStatement()   
     
    ```
    
    标准代码中就是通过该方法获取的执行对象。
    
*   **预编译 SQL 的执行 SQL 对象：防止 SQL 注入**
    
    ```
    PreparedStatement  prepareStatement(sql)
    
    ```
    
    通过这种方式获取的 `PreparedStatement` SQL 语句执行对象是我们一会重点要进行讲解的，它可以防止 SQL 注入。
    
*   执行存储过程的对象
    
    ```
    CallableStatement prepareCall(sql)
    
    ```
    
    通过这种方式获取的 `CallableStatement` 执行对象是用来执行存储过程的，而存储过程在 MySQL 中不常用，所以这个我们将不进行讲解。
    

#### 2.2.2 事务管理

![](<assets/1740015447176.png>)

先回顾一下 MySQL 事务管理的操作：

*   开启事务 ： BEGIN; 或者 START TRANSACTION;
    
*   提交事务 ： COMMIT;
    
*   回滚事务 ： ROLLBACK;
    

**MySQL 默认是自动提交事务**

也可以通过下面语句查询默认提交方式：

```
SELECT @@autocommit;

```

查询到的结果是 1 则表示自动提交，结果是 0 表示手动提交。当然也可以通过下面语句修改提交方式

```
set @@autocommit = 0;

``` 

接下来学习 JDBC 事务管理的方法。

**Connection 几口中定义了 3 个对应的方法：**

**开启事务**

![](<assets/1740015447458.png>)

参与 autoCommit 表示是否自动提交事务，true 表示自动提交事务，false 表示手动提交事务。而开启事务需要将该参数设为为 false。

**提交事务**

![](<assets/1740015447744.png>)

**回滚事务**

![](<assets/1740015447960.png>)

**具体代码实现如下：**

```
/**
 * JDBC API 详解：Connection
 */
public class JDBCDemo3_Connection {
 
    public static void main(String[] args) throws Exception {
        //1. 注册驱动
        //Class.forName("com.mysql.jdbc.Driver");
        //2. 获取连接：如果连接的是本机mysql并且端口是默认的 3306 可以简化书写
        String url = "jdbc:mysql:///db1?useSSL=false";
        String username = "root";
        String passWord = "1234";
        Connection conn = DriverManager.getConnection(url, username, passWord );
        //3. 定义sql
        String sql1 = "update account set money = 3000 where id = 1";
        String sql2 = "update account set money = 3000 where id = 2";
        //4. 获取执行sql的对象 Statement
        Statement stmt = conn.createStatement();
 
        try {
            // ============开启事务==========
            conn.setAutoCommit(false);
            //5. 执行sql
            int count1 = stmt.executeUpdate(sql1);//受影响的行数
            //6. 处理结果
            System.out.println(count1);
            int i = 3/0;
            //5. 执行sql
            int count2 = stmt.executeUpdate(sql2);//受影响的行数
            //6. 处理结果
            System.out.println(count2);
 
            // ============提交事务==========
            //程序运行到此处，说明没有出现任何问题，则需求提交事务
            conn.commit();
        } catch (Exception e) {
            // ============回滚事务==========
            //程序在出现异常时会执行到这个地方，此时就需要回滚事务
            conn.rollback();
            e.printStackTrace();
        }
 
        //7. 释放资源
        stmt.close();
        conn.close();
    }
}
```

### 2.3 Statement

 Statement 对象的**作用**就是用来**执行 SQL 语句**。

```
        String sql = "update student set age = 222 where name='xiaohua'";
        //4. 获取执行sql的对象 Statement
        Statement stmt = conn.createStatement();
        //5. 执行sql
        int count = stmt.executeUpdate(sql);//受影响的行数
```

**注意：**

*   以后开发很少使用 java 代码操作 DDL 语句
    

**执行 DDL、DML 语句**

![](<assets/1740015448171.png>)

**DDL：**show,create,use,select,drop,alter

**DML：**insert,update,delete

 **执行 DQL 语句**

![](<assets/1740015448459.png>)

 **DQL:**select,from,where,group by,having,order by,limit

#### 2.3.1 结果集对象 ResultSet

**作用：**封装了 SQL 查询语句的结果，执行了 DQL 语句后就会返回该对象。

**回顾 Statement 执行 DQL 语句的方法，返回值就是 ResultSet 对象：**

```
ResultSet  executeQuery(sql)：执行DQL 语句，返回 ResultSet 对象

```

**方法：**

boolean next()

*   将光标从当前位置向下移动一行，并判断当前行是否为有效行
    

方法返回值说明：

*   true ： 有效行，当前行有数据
    
*   false ： 无效行，当前行没有数据
    

xxx getXxx(参数)：获取数据

*   xxx : 数据类型；如： int getInt(参数) ；String getString(参数)
    
*   参数
    
    *   int 类型的参数：列的编号，从 1 开始
        
    *   String 类型的参数： 列的名称
        

示例：

```
import java.io.FileReader;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.Properties;
 
/**
 * JDBC快速入门
 */
public class Demo {
 
    public static void main(String[] args) throws Exception {
        Properties prop=new Properties();
        FileReader fileReader = new FileReader("module2/src/jdbc.properties");
        prop.load(fileReader);
 
        //1. 注册驱动
        Class.forName(prop.getProperty("driverClass"));
        //2. 获取连接
        String url = prop.getProperty("url");
        String username = prop.getProperty("username");
        String password = prop.getProperty("password");
        Connection conn = DriverManager.getConnection(url, username, password);
        //3. 定义sql
        String sql = "select * from student";
        //4. 获取执行sql的对象 Statement
        Statement stmt = conn.createStatement();
        //5. 执行sql
        ResultSet resultSet = stmt.executeQuery(sql);
        while(resultSet.next()){
            System.out.println(resultSet.getInt("id")+" "+resultSet.getString("name"));
        }
 
        //7. 释放资源
        stmt.close();
        conn.close();
        resultSet.close();
    }
}
```

#### 2.3.2 SQL 注入问题

SQL 注入是通过操作输入来**修改事先定义好的 SQL 语句**，用以达到执行代码**对服务器进行攻击**的方法。

**SQL 注入问题模拟：**

![](<assets/1740015448659.png>)

这样只需要输入框写入'1' = '1 就可以令拼接后 sql 语句条件判断为 true，从而获取到所有用户信息。

**详细解释：**上面代码是将用户名和密码拼接到 sql 语句中，拼接后的 sql 语句如下

![](<assets/1740015448887.png>)

从上面语句可以看出条件

![](<assets/1740015449097.png>)

不管是否满足， `or` 后面的 `'1' = '1'` 都是始终满足的，最终条件是成立的，就可以非法登陆成功。

**PreparedStatement 对象可以预防 sql 注入。**

### 2.4 PreparedStatement

#### 2.4.1 概述 

**作用**：预编译 SQL 语句并执行，**预防 SQL 注入问题。**

```
        PreparedStatement pstmt = conn.prepareStatement("update student set age = ? where name=?"); //PreparedStatement防止sql注入
        pstmt.setInt(1,30);pstmt.setString(2,"xiaoming");
```

通过这种方式，字符串内容如果有敏感字符，则会转义成符号，从而防止 sql 注入问题。

*   **获取 PreparedStatement 对象**
    
    ```
    // SQL语句中的参数值，使用？占位符替代
    String sql = "select * from user where username = ? and password = ?";
    // 通过Connection对象获取，并传入对应的sql语句
    PreparedStatement pstmt = conn.prepareStatement(sql);
    ```
    
*   **设置参数值**
    
    上面的 sql 语句中参数使用 ? 进行占位，在之前之前肯定要设置这些 ? 的值。
    
    **PreparedStatement 对象：**setXxx(参数 1，参数 2)：给 ? 赋值
    
    *   Xxx：数据类型 ； 如 setInt (1，234) 为设置第一个问号值为 234
        
    *   参数：
        
        *   参数 1： ？的位置编号，从 1 开始
            
        *   参数 2： ？的值
            
    
*   **执行 SQL 语句**
    
    executeUpdate(); 执行 DDL 语句和 DML 语句
    
    executeQuery(); 执行 DQL 语句
    
    **注意：**
    
    *   调用这两个方法时不需要传递 SQL 语句，因为获取 SQL 语句执行对象时已经对 SQL 语句进行预编译了。
        
    

#### 2.4.2 PreparedStatement 原理

**PreparedStatement 好处：**

*   预编译 SQL，性能更高
    
*   防止 SQL 注入：**将敏感字符进行转义**
    

**PreparedStatement 开启预编译功能：**

在代码中编写 url 时需要加上以下参数。不开启预编译的情况下，PreparedStatement 对象只是解决了 SQL 注入漏洞。

```
useServerPrepStmts=true

```

**为什么预编译性能更高？**

MySQL 服务器检查 SQL 和编译 SQL 花费的时间比执行 SQL 的时间还要长。如果我们只是重新设置参数，那么**检查 SQL 语句和编译 SQL 语句将不需要重复执行**。这样就提高了性能。

**PrepareStatement 预编译过程：**

*   在获取 PreparedStatement 对象时，将 sql 语句发送给 mysql 服务器进行检查，编译（这些步骤很耗时）
    
*   执行时就不用再进行这些步骤了，速度更快
    
*   **如果 sql 模板一样，则只需要进行一次检查、编译**
    

![](<assets/1740015449329.png>)

### 2.5 数据库连接池 **DataSource**

#### 2.5.1 数据库连接池简介

*   数据库连接池是个**容器**，负责分配、管理数据库连接 (Connection)。
    
*   它允许应用程序**重复使用一个现有的数据库连接**，而不是再重新建立一个；
    
*   释放空闲时间超过 “最大空闲时间” 的数据库连接，从而避免因为没有释放数据库连接而引起的数据库连接遗漏
    

**好处**

*   资源重用
    
*   提升系统响应速度
    
*   避免数据库连接遗漏
    

**之前**我们代码中使用连接是没有使用都创建一个 Connection 对象，使用完毕就会将其销毁。这样**重复创建销毁**的过程是特别耗费计算机的性能的及消耗时间的。

而数据库使用了数据库连接池后，就能达到 **Connection 对象的复用**，如下图

![](<assets/1740015449688.png>)

  

**连接池是在一开始就创建好了一些连接（Connection）对象存储起来。**用户需要连接数据库时，不需要自己创建连接，而只需要**从连接池中获取一个连接进行使用**，使用完毕后再将连接对象归还给连接池；这样就可以起到资源重用，也节省了频繁创建连接销毁连接所花费的时间，从而提升了系统响应的速度。

#### 2.5.2 数据库连接池实现

**标准接口：DataSource**

官方 (SUN) 提供的**数据库连接池标准接口**，由第三方组织实现此接口。该接口提供了获取连接的功能：

```
Connection getConnection()

```

那么以后就不需要通过 `DriverManager` 对象获取 `Connection` 对象，而是通过**连接池**（**DataSource**）**获取 `Connection` 对象**。

**常见的数据库连接池**

我们现在使用更多的是 Druid，它的性能比其他两个会好一些。

*   DBCP
    
*   C3P0
    
*   Druid
    
*   **Druid（德鲁伊）**
    
    *   Druid 连接池是阿里巴巴开源的数据库连接池项目
        
    *   功能强大，性能优秀，是 Java 语言最好的数据库连接池之一
        

#### 2.5.3 Driud 下载

下载地址：

[Central Repository: com/alibaba/druid](https://repo1.maven.org/maven2/com/alibaba/druid/ "Central Repository: com/alibaba/druid")

选择版本：

![](<assets/1740015450076.png>)

选择 jar 包：

![](<assets/1740015450334.png>)

#### 2.5.4 Driud 使用

*   导入 jar 包 
    
*   定义配置文件
    
*   加载配置文件
    
*   获取数据库连接池对象
    
*   获取连接
    

**导入 jar 包**

跟 mysql 的 jar 包类似，复制粘贴到模块目录下 lib 文件夹下，右键 add as library，level 选择 module library

**定义配置文件：**

在模块下新建 druid.properties，编写配置文件，前四句跟 jdbc.properties 一样。示例：

```
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql:///db1?useSSL=false&useServerPrepStmts=true
username=root
password=1234
 
 
# 初始化连接数量
initialSize=5
# 最大连接数
maxActive=10
# 最大等待时间
maxWait=3000
```

**加载配置文件：**

Properties 的 load 方法。

获取当前路径：

```
System.out.println(System.getProperty("user.dir"));

```

**获取数据库连接池对象：**

使用德鲁伊连接池工厂类 DruidDataSourceFactory 的创建连接池方法 createDataSource 获取连接池对象。

```
DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);

```

 **获取数据库连接 Connection：**

```
        Connection connection = dataSource.getConnection();
        System.out.println(connection); //获取到了连接后就可以继续做其他操作了
```

**代码示例：**

```
/**
 * Druid数据库连接池演示
 */
public class DruidDemo {
 
    public static void main(String[] args) throws Exception {
        //1.导入jar包
        //2.定义配置文件
        //3. 加载配置文件
        Properties prop = new Properties();
        //System.out.println(System.getProperty("user.dir")); //获取当前路径
        prop.load(new FileInputStream("jdbc-demo/src/druid.properties"));
        //4. 获取连接池对象
        DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);
 
        //5. 获取数据库连接 Connection
        Connection connection = dataSource.getConnection();
        //System.out.println(connection); //获取到了连接后就可以继续做其他操作了
    }
}
```

**剧透一下** spring 框架的 Jdbc.config 配置文件使用 druid：

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

**对比不使用连接池**

```
        //1. 注册驱动
        
        //Class.forName("com.mysql.jdbc.Driver");//通过反射获取Driver实现类对象，从而加载驱动，注册驱动语句DriverManager.registerDriver(driver)在Driver的静态代码块里做过了。
        //此句仅在mysql可省略，其他数据库不能省略
        //2. 获取连接
        String url = "jdbc:mysql://127.0.0.1:3306/db1?useSSL=false";
        String username = "root";
        String passWord = "1234";
        Connection conn = DriverManager.getConnection(url, username, passWord);
```

#### 2.5.5 Druid 配置文件详解

<table align="left" border="2"><tbody><tr><td>&nbsp;&nbsp;配置</td><td>缺省值</td><td>说明</td></tr><tr><td>name</td><td></td><td>配置这个属性的意义在于，如果存在多个数据源，监控的时候&nbsp;<br>可以通过名字来区分开来。如果没有配置，将会生成一个名字，&nbsp;<br>格式是："DataSource-" + System.identityHashCode(this)</td></tr><tr><td>jdbcUrl</td><td></td><td>连接数据库的 url，不同数据库不一样。例如：&nbsp;<br>MYSQL :&nbsp; &nbsp; jdbc:mysql://10.20.153.104:3306/druid2&nbsp;&nbsp;<br>ORACLE :&nbsp; &nbsp;jdbc:oracle:thin:@10.20.149.85:1521:ocnauto</td></tr><tr><td>username</td><td></td><td>连接数据库的用户名</td></tr><tr><td>password</td><td></td><td>连接数据库的密码。如果你不希望密码直接写在配置文件中，&nbsp;<br>可以使用 ConfigFilter。详细看这里：&nbsp;<br><a href="https://github.com/alibaba/druid/wiki/%E4%BD%BF%E7%94%A8ConfigFilter" title="使用ConfigFilter · alibaba/druid Wiki · GitHub" target="_blank">使用 ConfigFilter · alibaba/druid Wiki · GitHub</a></td></tr><tr><td>driverClassName</td><td>根据 url 自动识别</td><td>这一项可配可不配，如果不配置 druid 会根据 url 自动识别 dbType，然后选择相应的 driverClassName</td></tr><tr><td>initialSize</td><td>0</td><td>初始化时建立物理连接的个数。初始化发生在显示调用 init 方法，或者第一次 getConnection 时</td></tr><tr><td>maxActive</td><td>8</td><td>最大连接池数量</td></tr><tr><td>maxIdle</td><td>8</td><td>已经不再使用，配置了也没效果</td></tr><tr><td>minIdle</td><td></td><td>最小连接池数量</td></tr><tr><td>maxWait</td><td></td><td>获取连接时最大等待时间，单位毫秒。配置了 maxWait 之后，&nbsp;<br>缺省启用公平锁，并发效率会有所下降，&nbsp;<br>如果需要可以通过配置 useUnfairLock 属性为 true 使用非公平锁。</td></tr><tr><td>poolPreparedStatements</td><td>false</td><td>是否缓存 preparedStatement，也就是 PSCache。&nbsp;<br>PSCache 对支持游标的数据库性能提升巨大，比如说 oracle。&nbsp;<br>在 mysql5.5 以下的版本中没有 PSCache 功能，建议关闭掉。<br>作者在 5.5 版本中使用 PSCache，通过监控界面发现 PSCache 有缓存命中率记录，&nbsp;<br>该应该是支持 PSCache。</td></tr><tr><td>maxOpenPreparedStatements</td><td>-1</td><td>要启用 PSCache，必须配置大于 0，当大于 0 时，&nbsp;<br>poolPreparedStatements 自动触发修改为 true。&nbsp;<br>在 Druid 中，不会存在 Oracle 下 PSCache 占用内存过多的问题，&nbsp;<br>可以把这个数值配置大一些，比如说 100</td></tr><tr><td>validationQuery</td><td></td><td>用来检测连接是否有效的 sql，要求是一个查询语句。&nbsp;<br>如果 validationQuery 为 null，testOnBorrow、testOnReturn、&nbsp;<br>testWhileIdle 都不会其作用。</td></tr><tr><td>testOnBorrow</td><td>true</td><td>申请连接时执行 validationQuery 检测连接是否有效，做了这个配置会降低性能。</td></tr><tr><td>testOnReturn</td><td>false</td><td>归还连接时执行 validationQuery 检测连接是否有效，做了这个配置会降低性能</td></tr><tr><td>testWhileIdle</td><td>false</td><td>建议配置为 true，不影响性能，并且保证安全性。&nbsp;<br>申请连接的时候检测，如果空闲时间大于&nbsp;<br>timeBetweenEvictionRunsMillis，&nbsp;<br>执行 validationQuery 检测连接是否有效。</td></tr><tr><td>timeBetweenEvictionRunsMillis</td><td></td><td>有两个含义：&nbsp;<br>1) Destroy 线程会检测连接的间隔时间&nbsp;<br>2) testWhileIdle 的判断依据，详细看 testWhileIdle 属性的说明</td></tr><tr><td>numTestsPerEvictionRun</td><td></td><td>不再使用，一个 DruidDataSource 只支持一个 EvictionRun</td></tr><tr><td>minEvictableIdleTimeMillis</td><td></td><td></td></tr><tr><td>connectionInitSqls</td><td></td><td>物理连接初始化的时候执行的 sql</td></tr><tr><td>exceptionSorter</td><td>根据 dbType 自动识别</td><td>当数据库抛出一些不可恢复的异常时，抛弃连接</td></tr><tr><td>filters</td><td></td><td>属性类型是字符串，通过别名的方式配置扩展插件，&nbsp;<br>常用的插件有：&nbsp;<br>监控统计用的 filter:stat&nbsp;&nbsp;<br>日志用的 filter:log4j&nbsp;<br>防御 sql 注入的 filter:wall</td></tr><tr><td>proxyFilters</td><td></td><td>类型是 List&lt;com.alibaba.druid.filter.Filter&gt;，&nbsp;<br>如果同时配置了 filters 和 proxyFilters，&nbsp;<br>是组合关系，并非替换关系</td></tr></tbody></table>

## 三、JDBC 练习

#### 3.1 需求：完成商品品牌数据的增删改查操作

*   查询：查询所有数据
    
*   添加：添加品牌
    
*   修改：根据 id 修改
    
*   删除：根据 id 删除
    

#### 3.2 案例实现

环境准备

*   数据库表 `tb_brand`
    
    ```
    -- 删除tb_brand表
    drop table if exists tb_brand;
    -- 创建tb_brand表
    create table tb_brand (
        -- id 主键
        id int primary key auto_increment,
        -- 品牌名称
        brand_name varchar(20),
        -- 企业名称
        company_name varchar(20),
        -- 排序字段
        ordered int,
        -- 描述信息
        description varchar(100),
        -- 状态：0：禁用  1：启用
        status int
    );
    -- 添加数据
    insert into tb_brand (brand_name, company_name, ordered, description, status)
    values ('三只松鼠', '三只松鼠股份有限公司', 5, '好吃不上火', 0),
           ('华为', '华为技术有限公司', 100, '华为致力于把数字世界带入每个人、每个家庭、每个组织，构建万物互联的智能世界', 1),
           ('小米', '小米科技有限公司', 50, 'are you ok', 1);
    ```
    
*   在 pojo 包下实体类 Brand
    
    ```
    /**
     * 品牌
     * alt + 鼠标左键：整列编辑
     * 在实体类中，基本数据类型建议使用其对应的包装类型
     */
    public class Brand {
        // id 主键
        private Integer id;
        // 品牌名称
        private String brandName;
        // 企业名称
        private String companyName;
        // 排序字段
        private Integer ordered;
        // 描述信息
        private String description;
        // 状态：0：禁用  1：启用
        private Integer status;
    ​
        public Integer getId() {
            return id;
        }
    ​
        public void setId(Integer id) {
            this.id = id;
        }
    ​
        public String getBrandName() {
            return brandName;
        }
    ​
        public void setBrandName(String brandName) {
            this.brandName = brandName;
        }
    ​
        public String getCompanyName() {
            return companyName;
        }
    ​
        public void setCompanyName(String companyName) {
            this.companyName = companyName;
        }
    ​
        public Integer getOrdered() {
            return ordered;
        }
    ​
        public void setOrdered(Integer ordered) {
            this.ordered = ordered;
        }
    ​
        public String getDescription() {
            return description;
        }
    ​
        public void setDescription(String description) {
            this.description = description;
        }
    ​
        public Integer getStatus() {
            return status;
        }
    ​
        public void setStatus(Integer status) {
            this.status = status;
        }
    ​
        @Override
        public String toString() {
            return "Brand{" +
                    "id=" + id +
                    ", brandName='" + brandName + '\'' +
                    ", companyName='" + companyName + '\'' +
                    ", ordered=" + ordered +
                    ", description='" + description + '\'' +
                    ", status=" + status +
                    '}';
        }
    }
    ```
    

**查询所有**

```
 /**
   * 查询所有
   * 1. SQL：select * from tb_brand;
   * 2. 参数：不需要
   * 3. 结果：List<Brand>
   */
​
@Test
public void testSelectAll() throws Exception {
    //1. 获取Connection
    //3. 加载配置文件
    Properties prop = new Properties();
    prop.load(new FileInputStream("jdbc-demo/src/druid.properties"));
    //4. 获取连接池对象
    DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);
​
    //5. 获取数据库连接 Connection
    Connection conn = dataSource.getConnection();
    //2. 定义SQL
    String sql = "select * from tb_brand;";
    //3. 获取pstmt对象
    PreparedStatement pstmt = conn.prepareStatement(sql);
    //4. 设置参数
    //5. 执行SQL
    ResultSet rs = pstmt.executeQuery();
    //6. 处理结果 List<Brand> 封装Brand对象，装载List集合
    Brand brand = null;
    List<Brand> brands = new ArrayList<>();
    while (rs.next()){
        //获取数据
        int id = rs.getInt("id");
        String brandName = rs.getString("brand_name");
        String companyName = rs.getString("company_name");
        int ordered = rs.getInt("ordered");
        String description = rs.getString("description");
        int status = rs.getInt("status");
        //封装Brand对象
        brand = new Brand();
        brand.setId(id);
        brand.setBrandName(brandName);
        brand.setCompanyName(companyName);
        brand.setOrdered(ordered);
        brand.setDescription(description);
        brand.setStatus(status);
​
        //装载集合
        brands.add(brand);
    }
    System.out.println(brands);
    //7. 释放资源
    rs.close();
    pstmt.close();
    conn.close();
}
```

添加数据

```
/**
  * 添加
  * 1. SQL：insert into tb_brand(brand_name, company_name, ordered, description, status) values(?,?,?,?,?);
  * 2. 参数：需要，除了id之外的所有参数信息
  * 3. 结果：boolean
  */
@Test
public void testAdd() throws Exception {
    // 接收页面提交的参数
    String brandName = "香飘飘";
    String companyName = "香飘飘";
    int ordered = 1;
    String description = "绕地球一圈";
    int status = 1;
​
    //1. 获取Connection
    //3. 加载配置文件
    Properties prop = new Properties();
    prop.load(new FileInputStream("jdbc-demo/src/druid.properties"));
    //4. 获取连接池对象
    DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);
    //5. 获取数据库连接 Connection
    Connection conn = dataSource.getConnection();
    //2. 定义SQL
    String sql = "insert into tb_brand(brand_name, company_name, ordered, description, status) values(?,?,?,?,?);";
    //3. 获取pstmt对象
    PreparedStatement pstmt = conn.prepareStatement(sql);
    //4. 设置参数
    pstmt.setString(1,brandName);
    pstmt.setString(2,companyName);
    pstmt.setInt(3,ordered);
    pstmt.setString(4,description);
    pstmt.setInt(5,status);
​
    //5. 执行SQL
    int count = pstmt.executeUpdate(); // 影响的行数
    //6. 处理结果
    System.out.println(count > 0);
​
    //7. 释放资源
    pstmt.close();
    conn.close();
}
```

**修改数据**

```
/**
  * 修改
  * 1. SQL：
​
     update tb_brand
         set brand_name  = ?,
         company_name= ?,
         ordered     = ?,
         description = ?,
         status      = ?
     where id = ?
​
   * 2. 参数：需要，所有数据
   * 3. 结果：boolean
   */
​
@Test
public void testUpdate() throws Exception {
    // 接收页面提交的参数
    String brandName = "香飘飘";
    String companyName = "香飘飘";
    int ordered = 1000;
    String description = "绕地球三圈";
    int status = 1;
    int id = 4;
​
    //1. 获取Connection
    //3. 加载配置文件
    Properties prop = new Properties();
    prop.load(new FileInputStream("jdbc-demo/src/druid.properties"));
    //4. 获取连接池对象
    DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);
    //5. 获取数据库连接 Connection
    Connection conn = dataSource.getConnection();
    //2. 定义SQL
    String sql = " update tb_brand\n" +
        "         set brand_name  = ?,\n" +
        "         company_name= ?,\n" +
        "         ordered     = ?,\n" +
        "         description = ?,\n" +
        "         status      = ?\n" +
        "     where id = ?";
​
    //3. 获取pstmt对象
    PreparedStatement pstmt = conn.prepareStatement(sql);
​
    //4. 设置参数
    pstmt.setString(1,brandName);
    pstmt.setString(2,companyName);
    pstmt.setInt(3,ordered);
    pstmt.setString(4,description);
    pstmt.setInt(5,status);
    pstmt.setInt(6,id);
​
    //5. 执行SQL
    int count = pstmt.executeUpdate(); // 影响的行数
    //6. 处理结果
    System.out.println(count > 0);
​
    //7. 释放资源
    pstmt.close();
    conn.close();
}
```

**删除数据**

```
/**
  * 删除
  * 1. SQL：
            delete from tb_brand where id = ?
  * 2. 参数：需要，id
  * 3. 结果：boolean
  */
@Test
public void testDeleteById() throws Exception {
    // 接收页面提交的参数
    int id = 4;
    //1. 获取Connection
    //3. 加载配置文件
    Properties prop = new Properties();
    prop.load(new FileInputStream("jdbc-demo/src/druid.properties"));
    //4. 获取连接池对象
    DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);
    //5. 获取数据库连接 Connection
    Connection conn = dataSource.getConnection();
    //2. 定义SQL
    String sql = " delete from tb_brand where id = ?";
    //3. 获取pstmt对象
    PreparedStatement pstmt = conn.prepareStatement(sql);
    //4. 设置参数
    pstmt.setInt(1,id);
    //5. 执行SQL
    int count = pstmt.executeUpdate(); // 影响的行数
    //6. 处理结果
    System.out.println(count > 0);
​
    //7. 释放资源
    pstmt.close();
    conn.close();
}
```