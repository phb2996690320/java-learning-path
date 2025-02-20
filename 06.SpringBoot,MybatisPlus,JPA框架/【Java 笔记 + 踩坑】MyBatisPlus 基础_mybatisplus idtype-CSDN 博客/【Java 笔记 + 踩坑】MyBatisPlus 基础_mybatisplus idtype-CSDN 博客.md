---
url: https://blog.csdn.net/qq_40991313/article/details/126470047
title: 【Java 笔记 + 踩坑】MyBatisPlus 基础_mybatisplus idtype-CSDN 博客
date: 2025-02-20 09:45:24
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1，MyBatisPlus 简介](#t0)

[1.1 回顾 SpringBoot 整合 Mybatis](#t1)

[1.2 快速入门，实体类注解总结](#t2)

[1.3 MybatisPlus 特性](#t3)

[2，标准数据层开发](#t4)

[2.1 标准 CRUD 使用](#t5)

[2.2 新增](#t6)

[2.3 删除](#t7)

[2.4 修改](#t8)

[2.5 根据 ID 查询](#t9)

[2.6 查询所有](#t10)

[2.7 Lombok 简化实体类开发](#t11)

[2.8 分页功能](#t12)

[2.9 业务层继承 IService、ServiceImpl](#t13)

[3，DQL 编程控制](#t14)

[3.1 条件查询](#t15)

[3.1.1 条件查询的 Wrapper 包装器类](#t16)

[3.1.1.5 基本比较操作](#t17)

[3.1.2 环境准备](#t18)

[3.1.2.5 排除控制台日志打印、banner、log](#t19)

[3.1.3 构建条件查询](#t20)

[3.1.4 多条件构建，链式编程](#t21)

[3.1.5 条件查询 null 值处理](#t22)

[3.2 查询投影](#t23)

[3.2.1 查询指定字段](#t24)

[3.2.2 聚合查询](#t25)

[3.2.3 分组查询](#t26)

[3.3 查询条件](#t27)

[3.3.1 等值查询](#t28)

[3.3.2 范围查询](#t29)

[3.3.3 模糊查询](#t30)

[3.3.4 排序查询](#t31)

[3.4 映射匹配兼容性，属性起别名、可见性、权限、表明和实体类名不匹配](#t32)

[知识点 @TableField@TableName](#t33)

[报错演示和解决](#t34)

[4，DML 编程控制](#t35)

[4.1 id 生成策略控制](#t36)

[4.1.0 知识点 @TableId](#t37)

[4.1.1 环境构建](#t38)

[4.1.2 五种 id 策略代码实现](#t39)

[4.1.3 ID 生成策略对比](#t40)

[4.1.4 配置方法设置 id 策略和实体类前缀 tbl_](#t41)

[4.2 批量操作](#t42)

[4.3 逻辑删除，@TableLogic](#t43)

[4.3.1 概述](#t44) 

[知识点 1：@TableLogic](#t45)

[4.4 乐观锁](#t46)

[4.4.1 乐观锁和悲观锁概念](#t47)

[4.4.2 实现思路](#t48)

[4.4.3 代码实现](#t49)

[5，快速开发](#t50)

[5.1 代码生成器原理分析](#t51)

[5.2 代码生成器实现](#t52)

[5.3 MP 中 Service 的 CRUD](#t53)

[5.3.0 快速入门 ，IService 和 ServiceImpl,>](#t54)

[5.3.1 保存，Save,SaveOrUpdate](#t55)

[5.3.2 移除 Remove](#t56)

[5.3.3 更改 Update](#t57)

[5.3.4 查询，Get,List,Page,Count,Chain](#t58)

[6，ActiveRecord 简介](#t59)

[6.1 概念](#t60)

[6.2 增删改查](#t61)

## 1，MyBatisPlus 简介

MyBatisPlus 主要是对 MyBatis 的简化，所有我们先体会下它简化在哪，然后再学习它是什么，以及它帮我们都做哪些事。

*   MybatisPlus(简称 MP) 是**基于 MyBatis 框架基础上开发的增强型工具**，旨在**简化开发**、提供效率。
    
*   开发方式
    
    *   基于 MyBatis 使用 MyBatisPlus
    *   基于 Spring 使用 MyBatisPlus
    *   **基于 SpringBoot 使用 MyBatisPlus**

SpringBoot 刚刚我们学习完成，它能快速构建 Spring 开发环境用以整合其他技术，使用起来是非常简单，对于 MP 的学习，我们也**基于 SpringBoot 来构建学习**。

### **1.1 回顾 SpringBoot 整合 Mybatis**

*   创建 SpringBoot 工程
    
    ![](<assets/1740015924340.png>)
    
*   勾选配置使用的技术，能够实现自动添加起步依赖包
    
    ![](<assets/1740015924610.png>)
    
*   设置 dataSource 相关属性 (JDBC 参数)
    
    ![](<assets/1740015924887.png>)
    
*   定义数据层接口映射配置
    
    ![](<assets/1740015925173.png>)
    

### **1.2 快速入门，实体类注解总结**

**关键步骤：导入 mybatis-plus-boot-starter 依赖，dao 继承 BaseMapper <实体类名>**

**步骤 1: 创建数据库及表**

**注意：**

id 类型不是 int，而是 **bigint(20)** ，因为 MybatisPlus 默认使用的雪花算法**自增 id 很长**，例如 1561681377489510402L

表明为 user，是为了和实体类名统一，如果表明 tb_user，实体类名 User，则实体类必须加 @TableName("tb_user")，或者配置文件里统一给实体类名加前缀 "tb_"

```
create database if not exists mybatisplus_db character set utf8;
use mybatisplus_db;
CREATE TABLE user (
    id bigint(20) primary key auto_increment,
    name varchar(32) not null,
    password  varchar(32) not null,
    age int(3) not null ,
    tel varchar(32) not null
);
insert into user values(1,'Tom','tom',3,'18866668888');
insert into user values(2,'Jerry','jerry',4,'16688886666');
insert into user values(3,'Jock','123456',41,'18812345678');
insert into user values(4,'传智播客','itcast',15,'4006184000');
```

**步骤 2: 创建 SpringBoot 工程** 

![](<assets/1740015925555.png>)

也可以创建 Maven 工程， 手动导入相关依赖

**步骤 3: 勾选配置使用技术，只使用 mysql**，不用添加 Mybatis，因为后面导入 MybatisPlus 依赖，会根据依赖传递自动导入 Mybatis、Mybatis 整合 spring 的依赖。

![](<assets/1740015925783.png>)

**说明:**

*   由于 MP 并未被收录到 idea 的系统内置配置，无法直接选择加入，需要手动在 pom.xml 中配置添加

**步骤 4:pom.xml 添加 MybatisPlus 和 druid 依赖**

```
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.1</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.16</version>
</dependency>
```

**说明:**

*   druid 数据源可以加也可以不加，SpringBoot 有内置的数据源，可以配置成使用 Druid 数据源
    
*   从 MP 的依赖关系可以看出，通过依赖传递已经**将 MyBatis 与 MyBatis 整合 Spring 的 jar 包导入，我们不需要额外在添加 MyBatis 的相关 jar 包**
    
    ![](<assets/1740015925994.png>)
    

**步骤 5: 添加 MP 的相关配置信息**

resources 默认生成的是 properties 配置文件，可以将其**替换成 yml 文件**，并在文件中配置**数据库连接的相关信息**:`application.yml`

```
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    #serverTimezone是用来设置时区，UTC是标准时区，和咱们的时间差8小时，所以可以将其修改为Asia/Shanghai
    url: jdbc:mysql://localhost:3306/mybatisplus_db?serverTimezone=UTC 
    username: root
    password: root
```

**注意：**

**如果需要导入 druid 专有属性，就必须换依赖和配置方法了：**

pom.xml

导入了 druid-spring-boot-starter 依赖，就**不用再导入 druid 依赖了**，它里面包含了 druid 依赖

```
 
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
        <version>1.2.6</version>
    </dependency>
```

**application.yml**

```
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
      username: root
      password: root
```

![](<assets/1740015926177.png>)

与 druid 相关的配置超过 200 条以上，这就告诉你，如果想做 druid 相关的配置，使用这种格式就可以了。 

**步骤 6: 根据数据库表创建实体类**

```
public class User {   
    private Long id;    //注意id类型是Long
    private String name;
    private String password;
    private Integer age;
    private String tel;
    //setter...getter...toString方法略，别忘了自己生成
}
```

**剧透一下 mp 在实体类的注解：**

```
//注解了lombok的@Data会自动生成getter,setter,toString方法
@Data
//一般数据库表名tbl_user，这里注解@TableName("tbl_user")，就可以对应上表名，毕竟mp的语句里没有指定表名，都是在数据库中搜索和首字母小的的实体类名对应的表的。
@TableName("user")
public class User {
    //设置主键自增策略为auto，mp默认自增策略是ASSIGN_ID，分布式、雪花算法。自增策略也可以在yml中全局配置。
    @TableId(type = IdType.AUTO)
    private Long id;
    private String name;
    //value属性起别名，select设置该字段是否参与查询，针对于一些密码等隐私数据不希望被查出来
    @TableField(value = "password",select = false)
    private String password;
    private Integer age;
 
    private String tel;
    //exist属性设置是否在数据库中存在该字段
    @TableField(exist = false)
    private String online;
    //乐观锁注解版本，需要搭配乐观拦截器
    @Version
    private Integer version;
    //逻辑删除，本质是更新，数据库内该字段默认是0，通过标记为1来判定删除。
    @TableLogic
    private Integer delete;
}
```

**步骤 7: 创建 Dao 接口，继承 基础 mapper：BaseMapper<User>**

```
@Mapper
public interface UserDao extends BaseMapper<User>{
}
```

继承之后 dao 类多出了很多方法：

![](<assets/1740015926452.png>)

**步骤 8: 编写引导类**

```
@SpringBootApplication
//@MapperScan("com.itheima.dao")
public class Mybatisplus01QuickstartApplication {
    public static void main(String[] args) {
        SpringApplication.run(Mybatisplus01QuickstartApplication.class, args);
    }
 
}
```

**说明: Dao 接口要想被容器扫描到**，有两种解决方案:

*   **方案一:** 在 Dao 接口上添加**`@Mapper`注解**，并且确保 Dao 处在引导类所在包或其子包中
    
    *   该方案的缺点是需要在**每一 Dao 接口中添加注解**
*   **方案二:** 在**引导类**上添加**`@MapperScan`**注解，其属性为所要扫描的 Dao 所在包
    
    *   该方案的好处是**只需要写一次**，则指定包下的所有 Dao 接口都能被扫描到，`@Mapper`就可以不写。

**步骤 9: 编写测试类**

```
@SpringBootTest
class MpDemoApplicationTests {
 
	@Autowired
	private UserDao userDao;
	@Test
	public void testGetAll() {
		List<User> userList = userDao.selectList(null);
		System.out.println(userList);
	}
}
```

**说明:**

userDao 注入的时候下面有红线提示的原因是什么?

*   UserDao 是一个接口，不能实例化对象
*   只有在服务器启动 IOC 容器初始化后，由框架创建 DAO 接口的代理对象来注入
*   现在服务器并未启动，所以代理对象也未创建，IDEA 查找不到对应的对象注入，所以提示报红
*   一旦服务启动，就能注入其代理对象，所以该错误提示不影响正常运行。

**查看运行结果:**

![](<assets/1740015926788.png>)

跟之前整合 MyBatis 相比，你会发现**我们不需要在 DAO 接口中编写方法和 SQL 语句了，只需要继承`BaseMapper`接口即可。**整体来说简化很多。

### 1.3 MybatisPlus 特性

MyBatisPlus（简称 MP）是基于 MyBatis 框架基础上开发的增强型工具，旨在**简化开发、提高效率**

通过刚才的案例，相信大家能够体会**简化开发和提高效率**这两个方面的优点。

**MyBatisPlus 的官网为:**

```
https://mp.baomidou.com/

```

官方文档中有一张很多小伙伴比较熟悉的图片:

![](<assets/1740015927078.png>)

从这张图中我们可以看出 MP 旨在成为 MyBatis 的最好搭档，而不是替换 MyBatis, 所以可以理解为 MP 是 MyBatis 的一套增强工具，它是在 MyBatis 的基础上进行开发的，我们虽然使用 MP 但是底层依然是 MyBatis 的东西，也就是说我们也可以在 MP 中写 MyBatis 的内容。

对于 MP 的学习，大家可以参考着官方文档来进行学习，里面都有详细的代码案例。

**MP 的特性:**

*   **无侵入：只做增强不做改变**，不会对现有工程产生影响
*   **强大的 CRUD 操作：**内置通用 Mapper，少量配置即可实现单表 CRUD 操作
*   支持 Lambda：编写查询条件无需担心字段写错
*   **支持主键五种自动生成策略**
*   **内置分页插件、代码生成器、全局拦截插件、sql 注入剥离器**（支持 sql 注入剥离，防止 SQL 注入攻击）
*   ……

**架构：**

![](<assets/1740015927384.png>)

mybatis-plus-boot-starter 是核心依赖，需要导入，是对 springboot 的整合。 

## 2，标准数据层开发

在这一节中我们重点学习的是数据层标准的 CRUD(增删改查) 的实现与分页功能。代码比较多，我们一个个来学习。

### 2.1 标准 CRUD 使用

对于标准的 CRUD 功能都有哪些以及 MP 都提供了哪些方法可以使用呢?

我们先来看张图:

![](<assets/1740015927651.png>)

对于这张图的方法，我们挨个来演示下:

首先说下，案例中的环境就是咱们入门案例的内容，第一个先来完成`新增`功能

### 2.2 新增

首先确保在上面 1.2 快速入门那里导入了 mybatis-plus-boot-starter 依赖，配置了 yml 数据源，dao 接口继承了 **BaseMapper<User>**。 

在进行新增之前，我们可以分析下新增的方法:

```
int insert (T t)


```

*   T: 泛型，新增用来保存新增数据
*   int: 返回值，新增成功后返回 1，没有新增成功返回的是 0

在测试类中进行新增操作:

```
@SpringBootTest
class Mybatisplus01QuickstartApplicationTests {
 
    @Autowired
    private UserDao userDao;
 
    @Test
    void testSave() {
        User user = new User();
        user.setName("黑马程序员");
        user.setPassword("itheima");
        user.setAge(12);
        user.setTel("4006184000");
        userDao.insert(user);
    }
}
```

执行测试后，数据库表中就会添加一条数据。

![](<assets/1740015927890.png>)

**修改 id 主键自增策略：** 

这里**主键 ID 是雪花算法 ASSIGN_ID 策略生成的，想改成自增在实体类 id 上注解**

```
@TableId(type = IdType.AUTO)

```

**或者配置类 yml 全局配置 id 策略 ASSIGN_ID：**

```
mybatis-plus:
  global-config:
    db-config:
    	id-type: assign_id
```

AUTO 策略：数据库默认自增策略  
NONE: 不设置 id 生成策略  
INPUT: 用户手工输入 id，如果 id 为 null 会报错  
ASSIGN_ID: 雪花算法生成 id(可兼容数值型与字符串型)，mp 默认 id 策略  
ASSIGN_UUID: 以 UUID 生成算法作为 id 生成策略  
其他的几个策略均已过时，都将被 ASSIGN_ID 和 ASSIGN_UUID 代替掉。

### 2.3 删除

在进行删除之前，我们可以分析下删除的方法:

```
// 根据 ID 删除
int deleteById(Serializable id);
// 根据多个ID 批量删除
int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);
// 根据 columnMap 批量删除
int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);
 
//不常用
// 根据 entity 条件，删除记录
//int delete(@Param(Constants.WRAPPER) Wrapper<T> wrapper);
```

*   **Serializable：**参数类型
    
    *   **思考:** 参数类型为什么是一个序列化类?
        
        ![](<assets/1740015928117.png>)
        
        从这张图可以看出，
        
        *   **String 和 Number 是 Serializable 的子类，**
        *   Number 又是 Float,Double,Integer 等类的父类，
        *   **能作为主键的数据类型都已经是 Serializable 的子类**，
        *   MP 使用 **Serializable** 作为参数类型，就**类似 java 中 Object 接收任何数据类型**一样。
*   **int:** 返回值类型，数据删除成功返回 1，未删除数据返回 0。
    

**根据 id 删除:**

```
 @SpringBootTest
class Mybatisplus01QuickstartApplicationTests {
 
    @Autowired
    private UserDao userDao;
 
    @Test
    void testDelete() {
        //Long类型后面都有L字母，不加会溢出、报错，即使值为1也要加，即1L
        userDao.deleteById(1401856123725713409L);
    }
}
 
```

 **根据 map 删除：**

```
Map<String, Object> columnMap = new HashMap<>();
columnMap.put("age",20);
columnMap.put("name","张三");
//将columnMap中的元素设置为删除的条件，多个之间为and关系
int result = userDao.deleteByMap(columnMap);
System.out.println("result = " + result);
```

### 2.4 修改

mybatisplus 的修改很细节， 传进只 set 了几属性 User 对象，对于 id 的几个属性就会更改，其他属性不用判断空字符串和 null，直接不修改。

**根据 id 修改:**

```
// 根据 ID 选择修改
boolean updateById(T t);
// 根据ID 批量更新
boolean updateBatchById(Collection<T> entityList);
// 根据 whereWrapper 条件，更新记录
boolean update(T updateEntity, Wrapper<T> whereWrapper);
```

*   **T:** 泛型，**需要修改的数据内容**，注意因为是根据 ID 进行修改，所以传入的对象中需要有 ID 属性值
*   **int:** 返回值，修改成功后返回 1，未修改数据返回 0

在测试类中进行新增操作:

```
@SpringBootTest
class Mybatisplus01QuickstartApplicationTests {
 
    @Autowired
    private UserDao userDao;
 
    @Test
    void testUpdate() {
        User user = new User();
        user.setId(1L);
        user.setName("Tom888");
        user.setPassword("tom888");
        userDao.updateById(user);
    }
}
```

**说明:** 修改的时候，只修改实体对象中有值的字段。

**根据条件更改：**

```
User user = new User();
user.setAge(22); //更新的字段
//更新的条件
QueryWrapper<User> wrapper = new QueryWrapper<>();
wrapper.eq("id", 6);
//执行更新操作
int result = this.userMapper.update(user, wrapper);
System.out.println("result = " + result);
```

### 2.5 根据 ID 查询

在进行根据 ID 查询之前，我们可以分析下根据 ID 查询的方法:

```
// 根据 ID 查询
T getById(Serializable id);
// 根据 Wrapper，查询一条记录。结果集，如果是多个会抛出异常，随机取一条加上限制条件 wrapper.last("LIMIT 1")
T getOne(Wrapper<T> queryWrapper);
// 根据 Wrapper，查询一条记录
T getOne(Wrapper<T> queryWrapper, boolean throwEx);
// 根据 Wrapper，查询一条记录
Map<String, Object> getMap(Wrapper<T> queryWrapper);
```

*   **Serializable：**参数类型, 主键 ID 的值
*   **T:** 根据 ID 查询只会返回一条数据

**查询操作:**

```
@SpringBootTest
class Mybatisplus01QuickstartApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetById() {
        User user = userDao.selectById(2L);
        System.out.println(user);
    }
}
```

### 2.6 查询所有

在进行查询所有之前，我们可以分析下查询所有的方法:

```
List<T> selectList(Wrapper<T> queryWrapper)


```

*   **Wrapper：**用来构建条件查询的条件，目前我们没有可直接传为 Null
*   **List:** 因为查询的是所有，所以返回的数据是一个集合

在测试类中进行新增操作:

```
@SpringBootTest
class Mybatisplus01QuickstartApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll() {
        List<User> userList = userDao.selectList(null);
        System.out.println(userList);
    }
}
```

我们所调用的方法都是来自于 DAO 接口继承的 BaseMapper 类中。里面的方法有很多，我们后面会慢慢去学习里面的内容。

### 2.7 Lombok 简化实体类开发

**概念**

*   Lombok，一个 Java 类库，提供了一组注解，简化 POJO 实体类开发。

**使用步骤**

**步骤 1: 添加 lombok 依赖**

```
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <!--<version>1.18.12</version>-->
</dependency>
```

**注意：**版本可以不用写，因为 **SpringBoot 中已经管理了 lombok 的版本**。

**步骤 2: 旧版本 IDEA 要安装 Lombok 的插件**

**新版本 IDEA 已经内置了该插件，如果删除 setter 和 getter 方法程序有报红，则需要安装插件**

![](<assets/1740015928407.png>)

如果在 IDEA 中找不到 lombok 插件，可以访问如下网站

```
https://plugins.jetbrains.com/plugin/6317-lombok/versions

```

根据自己 IDEA 的版本下载对应的 lombok 插件，下载成功后，在 IDEA 中采用离线安装的方式进行安装。

![](<assets/1740015928628.png>)

**步骤 3: 模型类上添加注解**

Lombok 常见的注解有:

*   @Setter: 为模型类的属性提供 setter 方法
*   @Getter: 为模型类的属性提供 getter 方法
*   @ToString: 为模型类的属性提供 toString 方法
*   @EqualsAndHashCode: 为模型类的属性提供 equals 和 hashcode 方法
*   **@Data: 是个组合注解，包含** Setter、Getter、ToString、EqualsAndHashCode
*   **@NoArgsConstructor: 提供一个无参构造函数**
*   **@AllArgsConstructor: 提供一个包含所有参数的构造函数**

Lombok 的注解还有很多，上面标红的三个是比较常用的，其他的大家后期用到了，再去补充学习。

```
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private Long id;
    private String name;
    private String password;
    private Integer age;
    private String tel;
}
```

**说明:**

Lombok 只是简化模型类的编写，我们之前的方法也能用，比如有人会问: 我如果只想要有 name 和 password 的构造函数，该如何编写?

```
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private Long id;
    private String name;
    private String password;
    private Integer age;
    private String tel;
 
    public User(String name, String password) {
        this.name = name;
        this.password = password;
    }
}
```

这种方式是被允许的。

### 2.8 分页功能

基础的增删改查就已经学习完了，刚才我们在分析基础开发的时候，有一个分页功能还没有实现，在 MP 中如何实现分页功能，就是咱们接下来要学习的内容。

**分页查询使用的方法是:**

```
IPage<T> selectPage(IPage<T> page, Wrapper<T> queryWrapper)


```

*   **IPage<T>:** 用来**构建分页查询条件**
*   **Wrapper：**译为包装器，封装器。用来构建**条件查询的条件**，目前我们没有可直接传为 Null

IPage 是一个接口，我们需要找到它的实现类来构建它，具体的实现类，可以进入到 IPage 类中按 ctrl+h, 会找到其有一个实现类为`Page`。

**步骤 1: 调用方法传入参数获取返回值**

```
@SpringBootTest
class Mybatisplus01QuickstartApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    //分页查询
    @Test
    void testSelectPage(){
        //1 创建IPage分页对象,设置分页参数,1为当前页码，3为每页显示的记录数
        IPage<User> page=new Page<>(1,3);
        //2 执行分页查询
        userDao.selectPage(page,null);
        //3 获取分页结果，不配置拦截器就只能获取到current和size的数据，pages,total都是0，records是全部数据
        System.out.println("当前页码值："+page.getCurrent());
        System.out.println("每页显示数："+page.getSize());
        System.out.println("一共多少页："+page.getPages());
        System.out.println("一共多少条数据："+page.getTotal());
        System.out.println("数据："+page.getRecords());
    }
}
```

**也可以这样，参数多时更简单快速。categoryId 为 0 时查询所有：**

```
    @Override
    public PageUtils queryPage(Map<String, Object> params, Long categoryId) {
        String key = (String) params.get("key");
        LambdaQueryWrapper<AttrGroupEntity> wrapper = new LambdaQueryWrapper<>();
        //先根据检索查
        if(StringUtils.isNotEmpty(key)){
//wrapper.eq(AttrGroupEntity::getAttrGroupId,key).or().like(AttrGroupEntity::getAttrGroupName,key);//也可以
            wrapper.and(
                    obj->obj.eq(AttrGroupEntity::getAttrGroupId,key).or().like(AttrGroupEntity::getAttrGroupName,key)
            );
        }
        if(categoryId!=0) {
            wrapper.eq(AttrGroupEntity::getCatelogId,categoryId);
        }
        IPage<AttrGroupEntity> page = this.page(
                new Query<AttrGroupEntity>().getPage(params),
                wrapper
        );
 
        return new PageUtils(page);
 
    }
```

**步骤 2: 在 config 包下创建分页拦截器类**

**分页拦截器类代码是通用的，可以直接复制粘贴。**

配置拦截器 MP，并将其配置成 Spring 管理的 bean 对象即可。需要用到 MybatisPlusInterceptor 对象的 addInnerInterceptor 方法。

```
//注解为配置类@Configuration，也可以在引导类@Import({MybatisPlusConfig.class})
@Configuration
public class MybatisPlusConfig {
    //被Spring容器管理    
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        //1 创建mp拦截器对象MybatisPlusInterceptor
        MybatisPlusInterceptor mpInterceptor=new MybatisPlusInterceptor();
        //2 添加内置拦截器，参数为分页内置拦截器对象PaginationInnerInterceptor
        mpInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return mpInterceptor;
    }
}
```

**说明:** 上面的代码记不住咋办呢?

这些内容在 MP 的官方文档中有详细的说明，我们可以查看官方文档类配置

![](<assets/1740015928994.png>)

**步骤 3: 运行测试程序**

![](<assets/1740015929200.png>)

**查看 MybatisPlus 标准日志：** 

如果想查看 MP 执行的 SQL 语句，可以修改 application.yml 配置文件，

```
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl #打印SQL日志到控制台
```

打开日志后，就可以在控制台打印出对应的 SQL 语句，**开启日志功能性能就会受到影响，调试完后记得关闭。**

![](<assets/1740015929552.png>)

### 2.9 业务层继承 IService、ServiceImpl

```
public interface EmployeeService extends IService<Employee> {
}
 
 
 
@Service
public class EmployeeServiceImpl extends ServiceImpl<EmployeeDao, Employee> implements EmployeeService {
}
```

**这里业务接口继承了 IService<T>，业务实现类继承了 ServiceImpl<M extends BaseMapper<T>, T>。**

**这样 service 就继承了 Mybatis-plus 的各业务层方法、变量。**

ServiceImpl<M extends BaseMapper<T>, T > 类各方法（未过期）的作用

getBaseMapper()  
getEntityClass()  
saveBatch()  
saveOrUpdate()  
saveOrUpdateBatch()  
updateBatchById()  
getOne()  
getMap()  
getObj()

  
ServiceImpl 类各属性的作用

log：打印日志  
baseMapper：实现了许多的 SQL 操作  
entityClass：实体类  
mapperClass：映射类

## 3，DQL 编程控制

增删改查四个操作中，**查询是非常重要的也是非常复杂的操作**，这块需要我们重点学习下.

### 3.1 条件查询

#### 3.1.1 条件查询的 Wrapper 包装器类

*   MyBatisPlus 将书写复杂的 SQL 查询条件进行了封装，使用编程的形式完成查询条件的组合。

这个我们在前面都有见过，比如**查询所有和分页查询的时候**，都有看到过一个**`Wrapper`类**，这个类就是用来**构建查询条件**的，如下图所示:

![](<assets/1740015929850.png>)

**包装器 Wrapper<T> 是接口**，实际开发中主要使用它的两个实现类：**QueryWrapper 和 LambdaQueryWrapper**

![](<assets/1740015930202.png>)

 两种方式各有优劣：

**QueryWrapper 存在属性名写错的危险，但是支持聚合、分组查询；**

**LambdaQueryWrapper 没有属性名写错的危险，但不支持聚合、分组查询；**

#### 3.1.1.5 基本比较操作

**eq：等于 =**

**alleq：全部 [eq](https://www.baomidou.com/pages/10c804/#eq "eq")(或个别 [isNull](https://www.baomidou.com/pages/10c804/#isnull "isNull"))**

例子：`allEq({id:1,name:"老王",age:null})`--->`id = 1 and name = '老王' and age is null` 

**ne：不等于 <>**

**gt：大于 >**

**ge：大于等于 >=**

**lt：小于 <**

**le：小于等于 <=**

**between：BETWEEN 值 1 AND 值 2**

**notBetween：NOT BETWEEN 值 1 AND 值 2**

**in：字段 IN (value.get(0), value.get(1), ...)** 

**notIn：字段 NOT IN (v0, v1, ...)**

#### 3.1.2 环境准备

在构建条件查询之前，我们先来准备下环境

*   创建一个 SpringBoot 项目，只用选择 mysql
    
*   pom.xml 中添加对应的依赖 mybatis-plus-boot-starter,druid,lombok
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>2.5.0</version>
        </parent>
        <groupId>com.itheima</groupId>
        <artifactId>mybatisplus_02_dql</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <properties>
            <java.version>1.8</java.version>
        </properties>
        <dependencies>
     
            <dependency>
                <groupId>com.baomidou</groupId>
                <artifactId>mybatis-plus-boot-starter</artifactId>
                <version>3.4.1</version>
            </dependency>
     
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter</artifactId>
            </dependency>
     
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>1.1.16</version>
            </dependency>
     
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <scope>runtime</scope>
            </dependency>
     
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-test</artifactId>
                <scope>test</scope>
            </dependency>
     
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
            </dependency>
     
        </dependencies>
     
        <build>
            <plugins>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                </plugin>
            </plugins>
        </build>
     
    </project>
     
    ```
    
*   **编写 UserDao 接口**
    
    ```
    @Mapper
    public interface UserDao extends BaseMapper<User> {
    }
    ```
    
*   编写模型类
    
    ```
    @Data
    public class User {
        private Long id;
        private String name;
        private String password;
        private Integer age;
        private String tel;
    }
    ```
    
*   编写引导类
    
    ```
    @SpringBootApplication
    public class Mybatisplus02DqlApplication {
     
        public static void main(String[] args) {
            SpringApplication.run(Mybatisplus02DqlApplication.class, args);
        }
     
    }
    ```
    
*   **编写配置文件**
    
    ```
    #dataSource
    spring:
      datasource:
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://localhost:3306/mybatisplus_db?serverTimezone=UTC
        username: root
        password: root
    #mp日志
    mybatis-plus:
      configuration:
        log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
    ```
    
*   编写测试类
    
    ```
    @SpringBootTest
    class Mybatisplus02DqlApplicationTests {
     
        @Autowired
        private UserDao userDao;
        
        @Test
        void testGetAll(){
            List<User> userList = userDao.selectList(null);
            System.out.println(userList);
        }
    }
    ```
    
    最终创建的项目结构为:
    
    ![](<assets/1740015930607.png>)
    

#### 3.1.2.5 排除控制台日志打印、banner、log

*   **配置取消初始化 spring 日志打印:**
    
    *   取消初始化 spring 日志打印，resources 目录下添加 logback.xml，名称固定，内容如下:
        
        ```
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>
        </configuration>
        ```
        
        **说明:**logback.xml 的配置内容，不是我们学习的重点，如果有兴趣可以自行百度查询。
        
    *   **取消 MybatisPlus 启动 banner 图标，并开启 StdOutImpl 日志**
        
        ![](<assets/1740015930822.png>)
        
        application.yml 添加如下内容:
        
        ```
        #mybatis-plus日志控制台输出
        mybatis-plus:
          configuration:
            log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
          global-config:
            banner: off  关闭mybatisplus启动图标
        ```
        
    *   **取消 SpringBoot 的 log 打印**
        
        ![](<assets/1740015931099.png>)
        
        application.yml 添加如下内容:
        
        ```
        spring:
          main:
            banner-mode: off  #关闭SpringBoot启动图标(banner)
        ```
        

解决控制台打印日志过多的相关操作可以不用去做，一般会被用来方便我们查看程序运行的结果。

#### 3.1.3 构建条件查询

在进行查询的时候，我们的**入口是在包装器 Wrapper 这个接口**上，因为它是一个接口，所以我们需要去找它对应的**实现类**，关于实现类也有很多，主要用 **QueryWrapper 和 LambdaQueryWrapper（主要用这个类）：**

![](<assets/1740015931469.png>)

*   **方法一（不建议）：查询包装器 QueryWrapper**

```
@SpringBootTest
class Mybatisplus02DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        //创建QueryWrapper对象
        QueryWrapper qw = new QueryWrapper();
        //lt代表小于，大于是gt
        qw.lt("age",18);
        List<User> userList = userDao.selectList(qw);
        System.out.println(userList);
    }
}
```

*   **lt:** 小于 (<) , 最终的 sql 语句为
    
    ```
    SELECT id,name,password,age,tel FROM user WHERE (age < ?)
    
    
    ```
    

第一种方式介绍完后，有个小问题就是在写条件的时候，容易出错，比如 age 写错，就会导致查询不成功

*   **方法二（不建议）：****QueryWrapper 的基础上使用 lambda**

这种方法可以**防止数据库属性名写错**。 

```
@SpringBootTest
class Mybatisplus02DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        //使用Lambda，QueryWrapper<User>必须加泛型
        QueryWrapper<User> qw = new QueryWrapper<User>();
        qw.lambda().lt(User::getAge, 10);//添加条件，使用Lambda不容易写错属性名
        List<User> userList = userDao.selectList(qw);
        System.out.println(userList);
    }
}
```

*   User::getAget, 为 lambda 表达式中的，类名:: 方法名，最终的 sql 语句为:

```
SELECT id,name,password,age,tel FROM user WHERE (age < ?)


```

**注意:** 构建 LambdaQueryWrapper 的时候泛型不能省。

此时我们再次编写条件的时候，就不会存在写错名称的情况，但是 qw 后面多了一层 lambda() 调用

*   **方法三（推荐）：****LambdaQueryWrapper**

```
@SpringBootTest
class Mybatisplus02DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        lqw.lt(User::getAge, 10);
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
}
```

这种方式就解决了上一种方式所存在的问题。

#### 3.1.4 多条件构建，链式编程

学完了三种构建查询对象的方式，每一种都有自己的特点，所以用哪一种都行，刚才都是一个条件，那如果有多个条件该如何构建呢?

**需求:** 查询数据库表中，年龄在 10 岁到 30 岁之间的用户信息

```
@SpringBootTest
class Mybatisplus02DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        lqw.lt(User::getAge, 30);
        lqw.gt(User::getAge, 10);
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
}
```

*   **gt：**大于 (>), 最终的 SQL 语句为
    
    ```
    SELECT id,name,password,age,tel FROM user WHERE (age < ? AND age > ?)
    
    
    ```
    
*   构建多条件的时候，可以支持**链式编程**
    
    ```
    LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
    lqw.lt(User::getAge, 30).gt(User::getAge, 10);
    List<User> userList = userDao.selectList(lqw);
    System.out.println(userList);
    ```
    

**多条件查询默认是 and，or 要用. or() ：**

**练习:** 查询数据库表中，年龄小于 10 **或**年龄大于 30 的数据

**答案：**

```
@SpringBootTest
class Mybatisplus02DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        lqw.lt(User::getAge, 10).or().gt(User::getAge, 30);
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
}
```

*   or() 就相当于我们 sql 语句中的`or`关键字, **不加默认是`and`**，最终的 sql 语句为:
    
    ```
    SELECT id,name,password,age,tel FROM user WHERE (age < ? OR age > ?)
    
    
    ```
    

#### 3.1.5 条件查询 null 值处理

**针对问题：区间条件查询，例如某宝筛选价格范围，用户只填最高价，最低价为 null 不算在查询条件里。** 

**解决办法：新建实体类继承原实体类，多出属性范围，使用 lqw.lt(null!=uq.getAge2(),User::getAge, uq.getAge2()); 进行判定。**

先来看一张图，

![](<assets/1740015931561.png>)

*   我们在做条件查询的时候，一般会有很多条件可以供用户进行选择查询。
*   这些条件用户可以选择使用也可以选择不使用，比如我要查询价格在 8000 以上的手机
*   在输入条件的时候，价格有一个区间范围，按照需求只需要在第一个价格输入框中输入 8000
*   后台在做价格查询的时候，一般会让 price > 值 1 and price < 值 2
*   因为前端没有输入值 2，所以如果不处理的话，就会出现 price>8000 and price < null 问题
*   这个时候查询的结果就会出问题，具体该如何解决?

![](<assets/1740015931783.png>)

**需求:** 查询数据库表中，根据输入年龄范围来查询符合条件的记录

用户在输入值的时候，

如果只输入第一个框，说明要查询大于该年龄的用户

如果只输入第二个框，说明要查询小于该年龄的用户

如果两个框都输入了，说明要查询年龄在两个范围之间的用户

**后台如果想接收前端的两个数据，该如何接收?**

我们可以使用两个简单数据类型，也可以使用一个模型类，但是 User 类中目前只有一个 age 属性, 如:

```
@Data
public class User {
    private Long id;
    private String name;
    private String password;
    private Integer age;
    private String tel;
}
```

**使用一个 age 属性，如何去接收页面上的两个值呢?** 这个时候我们有两个解决方案

**方案一（不建议）: 添加属性 age2**, 这种做法可以但是会影响到原模型类的属性内容

```
@Data
public class User {
    private Long id;
    private String name;
    private String password;
    private Integer age;
    private String tel;
    private Integer age2;
}
```

**方案二（推荐）: 在 domain.query 下新建一个模型类, 让其继承 User 类**，并在其中添加 age2 属性，UserQuery 在拥有 User 属性后同时添加了 age2 属性。

```
@Data
public class User {
    private Long id;
    private String name;
    private String password;
    private Integer age;
    private String tel;
}
 
@Data
public class UserQuery extends User {
    private Integer age2;
}
```

**区间条件查询代码实现：**

**方法一： （麻烦，不建议）**

```
@SpringBootTest
class Mybatisplus02DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        //模拟页面传递过来的查询数据
        UserQuery uq = new UserQuery();
        uq.setAge(10);
        uq.setAge2(30);
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        if(null != uq.getAge2()){
            lqw.lt(User::getAge, uq.getAge2());
        }
        if( null != uq.getAge()) {
            lqw.gt(User::getAge, uq.getAge());
        }
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
}
```

上面的写法可以完成条件为非空的判断。

**存在问题：**如果条件多的话，每个条件都需要判断，代码量就比较大，所以使用方法二：

**方法二（推荐）：MP 给我们提供了简化方式实现区间条件查询**：

```
@SpringBootTest
class Mybatisplus02DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        //模拟页面传递过来的查询数据
        UserQuery uq = new UserQuery();
        uq.setAge(10);
        uq.setAge2(30);
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        //第一个参数为判断条件
        lqw.lt(null!=uq.getAge2(),User::getAge, uq.getAge2());
        lqw.gt(null!=uq.getAge(),User::getAge, uq.getAge());
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
}
```

*   lt() 方法
    
    ![](<assets/1740015932032.png>)
    
    condition 为 boolean 类型，返回 true，则添加条件，返回 false 则不添加条件
    

### 3.2 查询投影

#### 3.2.1 查询指定字段

目前我们在查询数据的时候，什么都没有做默认就是查询表中所有字段的内容。

**查询投影：**不查询所有字段，**只查询出指定字段的数据**。

**查询指定字段 lqw.select()**

**方法一（推荐）：使用 Lambda**

```
@SpringBootTest
class Mybatisplus02DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        lqw.select(User::getId,User::getName,User::getAge);
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
}
```

*   select(...) 方法用来设置查询的字段列，可以设置多个，最终的 sql 语句为:
    
    ```
    SELECT id,name,age FROM user
    
    
    ```
    

**方法二（不建议）：不用 lambda**

```
@SpringBootTest
class Mybatisplus02DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        QueryWrapper<User> lqw = new QueryWrapper<User>();
        lqw.select("id","name","age","tel");
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
}
```

*   最终的 sql 语句为: SELECT id,name,age,tel FROM user

![](<assets/1740015932342.png>)

#### 3.2.2 聚合查询

**聚合查询不能用 Lambda，只能用 QueryWrapper。**

**方法 qw.select("count(*) as count"); 和 userDao.selectMaps(qw)**

**需求:** 聚合函数查询，完成 count、max、min、avg、sum 的使用

count: 总记录数

max: 最大值

min: 最小值

avg: 平均值

sum: 求和

```
@SpringBootTest
class Mybatisplus02DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        QueryWrapper<User> lqw = new QueryWrapper<User>();
        //lqw.select("count(*) as count");
        //SELECT count(*) as count FROM user
        //lqw.select("max(age) as maxAge");
        //SELECT max(age) as maxAge FROM user
        //lqw.select("min(age) as minAge");
        //SELECT min(age) as minAge FROM user
        //lqw.select("sum(age) as sumAge");
        //SELECT sum(age) as sumAge FROM user
        lqw.select("avg(age) as avgAge");
        //SELECT avg(age) as avgAge FROM user
        List<Map<String, Object>> userList = userDao.selectMaps(lqw);
        System.out.println(userList);
    }
}
```

为了在做结果封装的时候能够更简单，我们将上面的聚合函数都**起了个名称，方面后期来获取这些数据**

![](<assets/1740015932590.png>)

键是聚合函数起的别名，值是查询的数量。

#### 3.2.3 分组查询

需求: 分组查询，完成 group by 的查询使用

**分组查询一定是要配合聚合函数的：**

```
@SpringBootTest
class Mybatisplus02DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        QueryWrapper<User> lqw = new QueryWrapper<User>();
        lqw.select("count(*) as count,tel");
        lqw.groupBy("tel");
        List<Map<String, Object>> list = userDao.selectMaps(lqw);
        System.out.println(list);
    }
}
```

*   groupBy 为分组，最终的 sql 语句为
    
    ```
    SELECT count(*) as count,tel FROM user GROUP BY tel
    
    
    ```
    

**注意:**

*   聚合与分组查询，无法使用 lambda 表达式来完成
*   MP 只是对 MyBatis 的增强，**如果 MP 实现不了，我们可以直接在 DAO 接口中使用 MyBatis 的方式实现**

### 3.3 查询条件

前面我们只使用了 lt() 和 gt(), 除了这两个方法外，MP 还封装了很多条件对应的方法，这一节我们重点把 MP 提供的查询条件方法进行学习下。

MP 的查询条件有很多:

*   范围匹配（> 、 = 、between）
*   模糊匹配（like）
*   空判定（null）
*   包含性匹配（in）
*   分组（group）
*   排序（order）
*   ……

#### 3.3.1 等值查询

需求: 根据用户名和密码查询用户信息

```
@SpringBootTest
class Mybatisplus02DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        lqw.eq(User::getName, "Jerry").eq(User::getPassword, "jerry");
        User loginUser = userDao.selectOne(lqw);
        System.out.println(loginUser);
    }
}
```

*   **eq()： equal 的缩小，相当于 `=`**, 对应的 sql 语句为
    
    ```
    SELECT id,name,password,age,tel FROM user WHERE (name = ? AND password = ?)
    
    
    ```
    
*   **selectList：**查询结果为多个或者单个
    
*   **selectOne:** 查询结果为单个
    

#### 3.3.2 范围查询

需求: 对年龄进行范围查询，使用 lt()、le()、gt()、ge()、between() 进行范围查询

```
@SpringBootTest
class Mybatisplus02DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        lqw.between(User::getAge, 10, 30);
        //SELECT id,name,password,age,tel FROM user WHERE (age BETWEEN ? AND ?)
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
}
```

*   gt(): 大于 (>)
*   ge(): 大于等于 (>=)
*   lt(): 小于 (<)
*   lte(): 小于等于 (<=)
*   between():between ? and ?

#### 3.3.3 模糊查询

需求: 查询表中 name 属性的值以`J`开头的用户信息, 使用 like 进行模糊查询

```
@SpringBootTest
class Mybatisplus02DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        lqw.likeLeft(User::getName, "J");
        //SELECT id,name,password,age,tel FROM user WHERE (name LIKE ?)
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
}
```

*   **like(): 前后加百分号**, 如 %J%
*   **likeLeft(): 左边加百分号**, 如 %J
*   likeRight(): 后面加百分号, 如 J%

#### 3.3.4 排序查询

**需求:** 查询所有数据，然后按照 id 降序

```
@SpringBootTest
class Mybatisplus02DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        LambdaQueryWrapper<User> lwq = new LambdaQueryWrapper<>();
        /**
         * condition ：条件，返回boolean，
         		当condition为true，进行排序，如果为false，则不排序
         * isAsc:是否为升序，true为升序，false为降序
         * columns：需要操作的列
         */
        lwq.orderBy(true,false, User::getId);
 
        userDao.selectList(lw
    }
}
```

除了上面演示的这种实现方式，还有很多其他的排序方法可以被调用，如图:

![](<assets/1740015932800.png>)

*   **orderBy 排序**
    
    *   condition: 条件，true 则添加排序，false 则不添加排序
    *   isAsc: 是否为升序，true 升序，false 降序
    *   columns: 排序字段，可以有多个
*   **orderByAsc/Desc(单个 column):** 按照指定字段进行升序 / 降序
    
*   **orderByAsc/Desc(多个 column):** 按照多个字段进行升序 / 降序
    
*   **orderByAsc/Desc**
    
    *   condition: 条件，true 添加排序，false 不添加排序
    *   多个 columns：按照多个字段进行排序

除了上面介绍的这几种查询条件构建方法以外还会有很多其他的方法，比如 isNull,isNotNull,in,notIn 等等方法可供选择，具体参考官方文档的条件构造器来学习使用，具体的网址为:

`https://mp.baomidou.com/guide/wrapper.html#abstractwrapper`

### 3.4 映射匹配兼容性，属性起别名、可见性、权限、表明和实体类名不匹配

前面我们已经能从表中查询出数据，并将数据封装到模型类中，这整个过程涉及到一张表和一个模型类:

![](<assets/1740015933134.png>)

之所以数据能够成功的从表中获取并封装到模型对象中，原因是表的字段列名和模型类的属性名一样。

那么问题就来了:

**问题 1: 表字段与编码属性设计不同步**

**解决：`@TableField字段注解`里的 value 属性起别名**

当表的列名和模型类的属性名发生不一致，就会导致数据封装不到模型对象，这个时候就需要其中一方做出修改，那如果前提是两边都不能改又该如何解决?

MP 给我们提供了一个注解`@TableField`, 使用该注解可以实现模型类属性名和表的列名之间的映射关系

![](<assets/1740015933379.png>)

**问题 2: 编码中添加了数据库中未定义的属性**

**解决：`@TableField`注解里的 exist 属性设置其是否在数据库存在**

当模型类中多了一个数据库表不存在的字段，就会导致生成的 sql 语句中在 select 的时候查询了数据库不存在的字段，程序运行就会报错，错误信息为:

**Unknown column '多出来的字段名称' in 'field list'**

具体的解决方案用到的还是`@TableField`注解，它有一个属性叫`exist`，设置该字段是否在数据库表中存在，如果设置为 false 则不存在，生成 sql 语句查询的时候，就不会再查询该字段了。

![](<assets/1740015933616.png>)

**问题 3：采用默认查询开放了更多的字段查看权限**

**解决：`@TableField`注解里的 select 属性，设置属性是否参与查询**

查询表中所有的列的数据，就可能把一些敏感数据查询到返回给前端，这个时候我们就需要限制哪些字段默认不要进行查询。解决方案是`@TableField`注解的一个属性叫`select`，该属性设置默认是否需要查询该字段的值，true(默认值) 表示默认查询该字段，false 表示默认不查询该字段。

```
@Data
@TableName("user")
public class User {
 
    private Long id;
    private String name;
    @TableField(value = "password",select = false)
    private String password;
    private Integer age;
    private String tel;
    @TableField(exist = false)
    private String online;
    
 
}
```

**问题 4: 表名与编码开发设计不同步**

该问题主要是表的名称和模型类的名称不一致，导致查询失败，因为 mybatisplus 的语句中没有表名，不像 sql 能 select * form xxx，只能根据实体类名和表明匹配。这个时候通常会报如下错误信息:

**Table 'databaseName.tableNaem' doesn't exist**, 翻译过来就是数据库中的表不存在。

![](<assets/1740015933824.png>)

**解决方案一：**是使用 MP 提供的另外一个**注解`@TableName`来设置表与模型类之间的对应关系。**

![](<assets/1740015934031.png>)

解决方法二：配置统一给实体类名加前缀：

```
mybatis-plus:
  global-config:
    db-config:
    	table-prefix: tbl_
```

#### 知识点 @TableField@TableName

**知识点 1: @TableField**

<table><thead><tr><th>名称</th><th>@TableField</th></tr></thead><tbody><tr><td>类型</td><td><strong>属性注解</strong></td></tr><tr><td>位置</td><td>模型类属性定义上方</td></tr><tr><td>作用</td><td>设置当前属性对应的数据库表中的字段关系</td></tr><tr><td>相关属性</td><td>value(默认)：设置数据库表字段名称<br>exist: 设置属性在数据库表字段中是否存在，默认为 true，此属性不能与 value 合并使用<br>select: 设置属性是否参与查询，此属性与 select() 映射配置不冲突</td></tr></tbody></table>

**知识点 2：@TableName**

<table><thead><tr><th>名称</th><th>@TableName</th></tr></thead><tbody><tr><td>类型</td><td><strong>类注解</strong></td></tr><tr><td>位置</td><td>模型类定义上方</td></tr><tr><td>作用</td><td>设置当前类对应于数据库表关系</td></tr><tr><td>相关属性</td><td>value(默认)：设置数据库表名称</td></tr></tbody></table>

#### 报错演示和解决

**步骤 1: 修改数据库表 user 为 tbl_user**

直接查询会报错，原因是 MP 默认情况下会使用模型类的类名首字母小写当表名使用。

![](<assets/1740015934275.png>)

**步骤 2: 模型类添加 @TableName 注解**

```
@Data
@TableName("tbl_user")
public class User {
    private Long id;
    private String name;
    private String password;
    private Integer age;
    private String tel;
}
```

**步骤 3: 将字段 password 修改成 pwd**

直接查询会报错，原因是 MP 默认情况下会使用模型类的属性名当做表的列名使用

![](<assets/1740015934592.png>)

**步骤 4：使用 @TableField 映射关系**

```
@Data
@TableName("tbl_user")
public class User {
    private Long id;
    private String name;
    @TableField(value="pwd")
    private String password;
    private Integer age;
    private String tel;
}
```

**步骤 5: 添加一个数据库表不存在的字段**

```
@Data
@TableName("tbl_user")
public class User {
    private Long id;
    private String name;
    @TableField(value="pwd")
    private String password;
    private Integer age;
    private String tel;
    private Integer online;
}
```

直接查询会报错，原因是 MP 默认情况下会查询模型类的所有属性对应的数据库表的列，而 online 不存在

![](<assets/1740015934843.png>)

**步骤 6：使用 @TableField 排除字段**

```
@Data
@TableName("tbl_user")
public class User {
    private Long id;
    private String name;
    @TableField(value="pwd")
    private String password;
    private Integer age;
    private String tel;
    @TableField(exist=false)
    private Integer online;
}
```

**步骤 7: 查询时将 pwd 隐藏**

```
@Data
@TableName("tbl_user")
public class User {
    private Long id;
    private String name;
    @TableField(value="pwd",select=false)
    private String password;
    private Integer age;
    private String tel;
    @TableField(exist=false)
    private Integer online;
}
```

## 4，DML 编程控制

### 4.1 id 生成策略控制

前面我们在新增的时候留了一个问题，就是新增成功后，**主键 ID 是一个很长串的内容**，我们更想要的是按照数据库表字段进行自增长。

**ID 该如何选择:**

*   **不同的表应用不同的 id 生成策略**
    
    *   日志：自增（1,2,3,4，……）
    *   购物订单：特殊规则（FQ23948AK3843）
    *   外卖单：关联地区日期等信息（10 04 20200314 34 91）
    *   关系表：可省略 id
    *   ……

不同的业务采用的 ID 生成方式应该是不一样的，那么在 MP 中都提供了哪些主键生成策略，以及我们该如何进行选择?

在这里我们又需要用到 MP 的一个注解叫**`@TableId`**

#### 4.1.0 知识点 @TableId

<table><thead><tr><th>名称</th><th>@TableId</th></tr></thead><tbody><tr><td>类型</td><td><strong>属性注解</strong></td></tr><tr><td>位置</td><td>模型类中用于表示主键的属性定义上方</td></tr><tr><td>作用</td><td>设置当前类中主键属性的生成策略</td></tr><tr><td>相关属性</td><td><strong>value(默认)：设置数据库表主键名称<br>type: 设置主键属性的生成策略，值查照 IdType 的枚举值</strong></td></tr></tbody></table>

#### **4.1.1 环境构建**

在构建条件查询之前，我们先来准备下环境

*   创建一个 SpringBoot 项目
    
*   pom.xml 中添加对应的依赖
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>2.5.0</version>
            <relativePath/> <!-- lookup parent from repository -->
        </parent>
        <groupId>com.itheima</groupId>
        <artifactId>mybatisplus_03_dml</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <properties>
            <java.version>1.8</java.version>
        </properties>
        <dependencies>
     
            <dependency>
                <groupId>com.baomidou</groupId>
                <artifactId>mybatis-plus-boot-starter</artifactId>
                <version>3.4.1</version>
            </dependency>
     
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter</artifactId>
            </dependency>
     
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>1.1.16</version>
            </dependency>
     
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <scope>runtime</scope>
            </dependency>
     
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-test</artifactId>
                <scope>test</scope>
            </dependency>
     
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>1.18.12</version>
            </dependency>
     
        </dependencies>
     
        <build>
            <plugins>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                </plugin>
            </plugins>
        </build>
     
    </project>
     
    ```
    
*   编写 UserDao 接口
    
    ```
    @Mapper
    public interface UserDao extends BaseMapper<User> {
    }
    ```
    
*   编写模型类
    
    ```
    @Data
    @TableName("tbl_user")
    public class User {
        private Long id;
        private String name;
        @TableField(value="pwd",select=false)
        private String password;
        private Integer age;
        private String tel;
        @TableField(exist=false)
        private Integer online;
    }
    ```
    
*   编写引导类
    
    ```
    @SpringBootApplication
    public class Mybatisplus03DqlApplication {
     
        public static void main(String[] args) {
            SpringApplication.run(Mybatisplus03DqlApplication.class, args);
        }
     
    }
    ```
    
*   编写配置文件
    
    ```
     
    spring:
      datasource:
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://localhost:3306/mybatisplus_db?serverTimezone=UTC
        username: root
        password: root
     #mp日志
    mybatis-plus:
      configuration:
        log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
    ```
    
*   编写测试类
    
    ```
    @SpringBootTest
    class Mybatisplus02DqlApplicationTests {
     
        @Autowired
        private UserDao userDao;
        
        @Test
        void testGetAll(){
            List<User> userList = userDao.selectList(null);
            System.out.println(userList);
        }
    }
    ```
    
*   测试
    
    ```
    @SpringBootTest
    class Mybatisplus03DqlApplicationTests {
     
        @Autowired
        private UserDao userDao;
    	
        @Test
        void testSave(){
            User user = new User();
            user.setName("黑马程序员");
            user.setPassword("itheima");
            user.setAge(12);
            user.setTel("4006184000");
            userDao.insert(user);
        }
        @Test
        void testDelete(){
            userDao.deleteById(1401856123925713409L)
        }
        @Test
        void testUpdate(){
            User user = new User();
            user.setId(3L);
            user.setName("Jock666");
            user.setVersion(1);
            userDao.updateById(user);
        }
    }
    ```
    
*   最终创建的项目结构为:
    
    ![](<assets/1740015935093.png>)
    

#### 4.1.2 五种 id 策略代码实现

**概述：** 

*   **AUTO 策略：数据库默认自增策略**
*   **NONE: 不设置 id 生成策略**
*   **INPUT: 用户手工输入 id**，如果 id 为 null 会报错
*   **ASSIGN_ID: 雪花算法生成 id(可兼容数值型与字符串型)**，mp 默认 id 策略
*   **ASSIGN_UUID: 以 UUID 生成算法作为 id 生成策略**
*   **其他的几个策略均已过时，都将被 ASSIGN_ID 和 ASSIGN_UUID 代替掉。**

**注意：**

**除了 INPUT 策略，其他策略即使指定 id 为 null 会自动生成，不为 null 会用指定的 id。**

**拓展:**

**分布式 ID 是什么?**

*   当数据量足够大的时候，一台数据库服务器存储不下，这个时候就需要**多台数据库服务器进行存储**
*   比如订单表就有可能被存储在不同的服务器上
*   **如果用数据库表的自增主键，因为在两台服务器上所以会出现冲突**
*   这个时候就需要一个**全局唯一 ID, 这个 ID 就是分布式 ID。**

**代码实现：** 

**AUTO 策略：数据库默认自增策略**

**步骤 1: 给实体类 id 属性，设置生成策略为 AUTO**

```
@Data
@TableName("tbl_user")
public class User {
    @TableId(type = IdType.AUTO)
    private Long id;
    private String name;
    @TableField(value="pwd",select=false)
    private String password;
    private Integer age;
    private String tel;
    @TableField(exist=false)
    private Integer online;
}
```

**步骤 2: 删除测试数据并修改自增值**

*   删除测试数据
    
    ![](<assets/1740015935299.png>)
    
*   因为之前生成主键 ID 的值比较长，会把 MySQL 的自动增长的值变的很大，所以需要将其调整为目前最新的 id 值。
    

![](<assets/1740015935543.png>)

**步骤 3: 运行新增方法**

会发现，新增成功，并且主键 id 也是从 5 开始

![](<assets/1740015935817.png>)

经过这三步的演示，会发现`AUTO`的作用是**使用数据库 ID 自增**，在使用该策略的时候一定要确保对应的数据库表设置了 ID 主键自增，否则无效。

接下来，我们可以进入源码查看下 ID 的生成策略有哪些?

打开源码后，你会发现并没有看到中文注释，这就需要我们点击右上角的`Download Sources`, 会自动帮你把这个类的 java 文件下载下来，我们就能看到具体的注释内容。因为这个技术是国人制作的，所以他代码中的注释还是比较容易看懂的。

![](<assets/1740015936065.png>)

当把源码下载完后，就可以看到如下内容:

![](<assets/1740015936280.png>)

**INPUT 策略**

**步骤 1: 设置生成策略为 INPUT**

```
@Data
@TableName("tbl_user")
public class User {
    @TableId(type = IdType.INPUT)
    private Long id;
    private String name;
    @TableField(value="pwd",select=false)
    private String password;
    private Integer age;
    private String tel;
    @TableField(exist=false)
    private Integer online;
}
```

**注意:** 这种 ID 生成策略，需要将表的自增策略删除掉

![](<assets/1740015936526.png>)

**步骤 2: 添加数据手动设置 ID**

```
@SpringBootTest
class Mybatisplus03DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
	
    @Test
    void testSave(){
        User user = new User();
        //设置主键ID的值
        user.setId(666L);
        user.setName("黑马程序员");
        user.setPassword("itheima");
        user.setAge(12);
        user.setTel("4006184000");
        userDao.insert(user);
    }
}
```

**步骤 3: 运行新增方法**

如果没有设置主键 ID 的值，则会报错，错误提示就是主键 ID 没有给值:

![](<assets/1740015936764.png>)

如果设置了主键 ID, 则数据添加成功，如下:

![](<assets/1740015937011.png>)

**ASSIGN_ID 策略**

**步骤 1: 设置生成策略为 ASSIGN_ID**

```
@Data
@TableName("tbl_user")
public class User {
    @TableId(type = IdType.ASSIGN_ID)
    private Long id;
    private String name;
    @TableField(value="pwd",select=false)
    private String password;
    private Integer age;
    private String tel;
    @TableField(exist=false)
    private Integer online;
}
```

**步骤 2: 添加数据不设置 ID**

```
@SpringBootTest
class Mybatisplus03DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
	
    @Test
    void testSave(){
        User user = new User();
        user.setName("黑马程序员");
        user.setPassword("itheima");
        user.setAge(12);
        user.setTel("4006184000");
        userDao.insert(user);
    }
}
```

**注意:** 这种生成策略，不需要手动设置 ID，如果手动设置 ID，则会使用自己设置的值。

**步骤 3: 运行新增方法**

![](<assets/1740015937310.png>)

生成的 ID 就是一个 Long 类型的数据。

**ASSIGN_UUID 策略**

**步骤 1: 设置生成策略为 ASSIGN_UUID**

使用 uuid 需要注意的是，主键的类型不能是 Long，而应该改成 String 类型

```
@Data
@TableName("tbl_user")
public class User {
    @TableId(type = IdType.ASSIGN_UUID)
    private String id;
    private String name;
    @TableField(value="pwd",select=false)
    private String password;
    private Integer age;
    private String tel;
    @TableField(exist=false)
    private Integer online;
}
```

**步骤 2: 修改表的主键类型**

![](<assets/1740015937751.png>)

主键类型设置为 varchar，长度要大于 32，因为 UUID 生成的主键为 32 位，如果长度小的话就会导致插入失败。

**步骤 3: 添加数据不设置 ID**

```
@SpringBootTest
class Mybatisplus03DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
	
    @Test
    void testSave(){
        User user = new User();
        user.setName("黑马程序员");
        user.setPassword("itheima");
        user.setAge(12);
        user.setTel("4006184000");
        userDao.insert(user);
    }
}
```

**步骤 4: 运行新增方法**

![](<assets/1740015938258.png>)

接下来我们来聊一聊雪花算法:

雪花算法 (SnowFlake), 是 Twitter 官方给出的算法实现 是用 Scala 写的。其生成的结果是一个 64bit 大小整数，它的结构如下图:

![](<assets/1740015938576.png>)

*   1bit, 不用, 因为二进制中最高位是符号位，1 表示负数，**0 表示正数**。生成的 id 一般都是用整数，所以**最高位固定为 0**。
*   41bit- **时间戳**，用来记录时间戳，毫秒级
*   10bit- **工作机器 id**，用来记录工作机器 id, 其中高位 5bit 是数据中心 ID 其取值范围 0-31，低位 5bit 是工作节点 ID 其取值范围 0-31，两个组合起来最多可以容纳 1024 个节点
*   **序列号**占用 12bit，每个节点每毫秒 0 开始不断累加，最多可以累加到 4095，一共可以产生 4096 个 ID

#### 4.1.3 ID 生成策略对比

介绍了这些主键 ID 的生成策略，我们以后该用哪个呢?

*   **NONE: 不设置 id 生成策略**，MP 不自动生成，**约等于 INPUT**, 所以这两种方式都**需要用户手动设置**，但是手动设置第一个问题是容易出现相同的 ID 造成主键冲突，为了保证主键不冲突就需要做很多判定，实现起来比较复杂
*   **AUTO:** 数据库 ID 自增, 这种策略适合在数据库服务器只有 1 台的情况下使用, 不可作为分布式 ID 使用
*   **ASSIGN_UUID:** 可以在**分布式的情况下使用**，而且能够**保证唯一**，但是生成的主键是 **32 位的字符串，长度过长占用空间而且还不能排序，查询性能也慢**
*   **ASSIGN_ID:** 可以在**分布式**的情况下使用，生成的是 **Long 类型的数字**，可以**排序性能也高**，但是**生成的策略和服务器时间有关，如果修改了系统时间就有可能导致出现重复主键**
*   综上所述，每一种主键策略都有自己的优缺点，根据自己项目业务的实际情况来选择使用才是最明智的选择。

#### 4.1.4 配置方法设置 id 策略和实体类前缀 tbl_

**模型类主键策略设置**

对于主键 ID 的策略已经介绍完，但是如果要在项目中的每一个模型类上都需要使用相同的生成策略，如:

![](<assets/1740015938873.png>)

我们只需要在配置文件中添加如下内容，就能让所有的模型类都可以使用该主键 ID 策略

```
mybatis-plus:
  global-config:
    db-config:
    	id-type: assign_id
```

配置完成后，每个模型类的主键 ID 策略都将成为 assign_id.

**配置方法设置数据库表与模型类的映射关系**

**MP 会默认将模型类的类名名首字母小写作为表名使用**，假如数据库表的名称都以`tbl_`开头，那么我们就需要将所有的模型类上添加**`@TableName`**，如:

![](<assets/1740015939118.png>)

配置起来还是比较繁琐，简化方式为在配置文件中配置如下内容:

```
mybatis-plus:
  global-config:
    db-config:
    	table-prefix: tbl_
```

设置表的前缀内容，这样 MP 就会拿 `tbl_`加上模型类的首字母小写，就刚好组装成数据库的表名。

### 4.2 批量操作

先来看下问题:

![](<assets/1740015939360.png>)

**批量操作：**也就是前面有一个复选框，用户一次可以勾选多个也可以进行全选，然后删一次就可以将购物车清空，这个就需要用到`批量删除`的操作了。

**批量删除：** 

**批量删除的 API 方法**

```
int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);


```

翻译方法的字面意思为: 删除（根据 ID 批量删除）, 参数是一个集合，可以存放多个 id 值。

**代码演示：** 

需求: 根据传入的 id 集合将数据库表中的数据删除掉。

```
@SpringBootTest
class Mybatisplus03DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
	
    @Test
    void testDelete(){
        //删除指定多条数据
        List<Long> list = new ArrayList<>();
        list.add(1402551342481838081L);
        list.add(1402553134049501186L);
        list.add(1402553619611430913L);
        userDao.deleteBatchIds(list);
    }
}
```

执行成功后，数据库表中的数据就会按照指定的 id 进行删除。

**批量查询：** 

除了按照 id 集合进行批量删除，也可以按照 id 集合进行批量查询，还是先来看下 API

```
List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);


```

方法名称翻译为: 查询（根据 ID 批量查询），参数是一个集合，可以存放多个 id 值。

需求：根据传入的 ID 集合查询用户信息

```
@SpringBootTest
class Mybatisplus03DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
	
    @Test
    void testGetByIds(){
        //查询指定多条数据
        List<Long> list = new ArrayList<>();
        list.add(1L);
        list.add(3L);
        list.add(4L);
        userDao.selectBatchIds(list);
    }
}
```

查询结果就会按照指定传入的 id 值进行查询

![](<assets/1740015939593.png>)

### 4.3 逻辑删除，@TableLogic

#### 4.3.1 概述 

**逻辑删除底层其实是更新操作。** 

**分析问题:**

![](<assets/1740015939892.png>)

*   这是一个员工和其所签的合同表，关系是**一个员工可以签多个合同**，是一个**一 (员工) 对多(合同)** 的表，**逻辑删除就是删除一个员工，只是更改数据此员工的状态，不物理删除。**
    
*   员工 ID 为 1 的张业绩，总共签了三个合同，如果此时他离职了，我们需要将员工表中的数据进行删除，会执行 delete 操作
    
*   如果表在设计的时候有主外键关系，那么同时也得将合同表中的前三条数据也删除掉
    
    ![](<assets/1740015940119.png>)
    
*   后期要统计所签合同的总金额，就会发现对不上，原因是已经将员工 1 签的合同信息删除掉了
    
*   如果只删除员工不删除合同表数据，那么合同的员工编号对应的员工信息不存在，那么就会出现垃圾数据，就会出现无主合同，根本不知道有张业绩这个人的存在
    
*   所以经过分析，我们不应该将表中的数据删除掉，而是需要进行保留，但是又得把离职的人和在职的人进行区分，这样就解决了上述问题，如:
    
    ![](<assets/1740015940386.png>)
    
*   **区分的方式，就是在员工表中添加一列数据`deleted`，如果为 0 说明在职员工，如果离职则将其改完 1，（0 和 1 所代表的含义是可以自定义的）**
    

所以对于删除操作业务问题来说有:

*   **物理删除:** 业务数据从数据库中丢弃，执行的是 delete 操作
*   **逻辑删除**: **为数据设置是否可用状态字段**，**删除时设置状态字段为不可用状态，数据保留在数据库中，执行的是 update 操作**

**代码实现逻辑删除：**

**步骤 1: 修改数据库表添加`deleted`列**

字段名可以任意，内容也可以自定义，比如`0`代表正常，`1`代表删除，可以在添加列的同时设置其默认值为`0`正常。

![](<assets/1740015940741.png>)

**步骤 2: 实体类添加属性**

(1) 添加与数据库表的列对应的一个属性名，名称可以任意，如果和数据表列名对不上，可以使用 @TableField 进行关系映射，如果一致，则会自动对应。

**(2) 实体类设置逻辑删除成员：**

**方法一：单个实体类注解`@TableLogic`**

标识新增的字段为逻辑删除字段，使用**注解`@TableLogic，value属性是默认值，delval是删除后修改的值`**

```
@Data
//@TableName("tbl_user") 可以不写是因为配置了全局配置
public class User {
    @TableId(type = IdType.ASSIGN_UUID)
    private String id;
    private String name;
    @TableField(value="pwd",select=false)
    private String password;
    private Integer age;
    private String tel;
    @TableField(exist=false)
    private Integer online;
    @TableLogic(value="0",delval="1")
    //value属性是默认值，delval是删除后修改的值
    private Integer deleted;
}
```

 **方法二：yml 全局配置**

```
mybatis-plus:
  global-config:
    db-config:
       #逻辑删除字段名
      logic-delete-field: deleted
       #逻辑删除字面值：未删除为0
      logic-not-delete-value: 0
       #逻辑删除字面值：删除为1
      logic-delete-value: 1
```

**步骤 3: 运行删除方法**

```
@SpringBootTest
class Mybatisplus03DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
	
    @Test
    void testDelete(){
       userDao.deleteById(1L);
    }
}
```

![](<assets/1740015941040.png>)

从测试结果来看，逻辑删除最后走的是 update 操作，会将指定的字段修改成删除状态对应的值。

**思考**

**逻辑删除后查询操作：**

*   **执行查询操作默认给 sql 语句后面加 where deleted=0**
    
    ```
    @SpringBootTest
    class Mybatisplus03DqlApplicationTests {
     
        @Autowired
        private UserDao userDao;
    	
        @Test
        void testFind(){
           System.out.println(userDao.selectList(null));
        }
    }
    ```
    
    运行测试，会发现打印出来的 sql 语句中会多一个查询条件，如:
    
    ![](<assets/1740015941358.png>)
    
    可想而知，MP 的逻辑删除会将所有的查询都添加一个未被删除的条件，也就是已经被删除的数据是不应该被查询出来的。
    
*   **使用 Mybatis 依然能查出已经被逻辑删除的数据：**
    
    ```
    @Mapper
    public interface UserDao extends BaseMapper<User> {
        //查询所有数据包含已经被删除的数据
        @Select("select * from tbl_user")
        public List<User> selectAll();
    }
    ```
    

**逻辑删除的本质:** 

**逻辑删除的本质其实是修改操作。如果加了逻辑删除字段，查询数据时也会自动带上逻辑删除字段。**

执行的 SQL 语句为:

```
UPDATE tbl_user SET deleted=1 where id = ? AND deleted=0

```

执行数据结果为:

![](<assets/1740015941580.png>)

#### 知识点 1：@TableLogic

<table><thead><tr><th>名称</th><th>@TableLogic</th></tr></thead><tbody><tr><td>类型</td><td><strong>属性注解</strong></td></tr><tr><td>位置</td><td>模型类中用于表示删除字段的属性定义上方</td></tr><tr><td>作用</td><td>标识该字段为进行逻辑删除的字段</td></tr><tr><td>相关属性</td><td>value：逻辑未删除值<br>delval: 逻辑删除值</td></tr></tbody></table>

### 4.4 乐观锁

#### 4.4.1 乐观锁和悲观锁概念

![](<assets/1740015941864.png>)

![](<assets/1740015942063.png>)

**乐观锁通过版本号控制事务的并发。**

第一步，给数据加版本号

第二步，**保存数据时判断版本号是否取出时的版本，如果是则说明数据没有改动，直接保存并且版本号加 1，否则回退。**

**悲观锁：**就是锁定你想要使用的资源，其他请求想要使用这个资源就必须排队，等这个资源释放了才能继续。

在讲解乐观锁之前，我们还是先来分析下问题:

**乐观锁使用案例：**

业务并发现象带来的问题: **秒杀**

*   假如有 100 个商品或者票在出售，为了能保证每个商品或者票只能被一个人购买，如何保证不会出现超买或者重复卖
*   对于这一类问题，其实有很多的解决方案可以使用
*   第一个最先想到的就是锁，锁在一台服务器中是可以解决的，但是如果在多台服务器下锁就没有办法控制，比如 12306 有两台服务器在进行卖票，在两台服务器上都添加锁的话，那也有可能会导致在同一时刻有两个线程在进行卖票，还是会出现并发问题
*   我们接下来介绍的这种方式是针对于小型企业的解决方案，因为数据库本身的性能就是个瓶颈，如果对其并发量超过 2000 以上的就需要考虑其他的解决方案了。

简单来说，**乐观锁主要解决的问题是当要更新一条记录的时候，希望这条记录没有被别人更新。**  
 

#### 4.4.2 实现思路

乐观锁的实现方式:

*   数据库表中**添加 version 列**，比如默认值给 1
    
*   第一个线程要修改数据之前，取出记录时，获取当前数据库中的 version=1
    
*   第二个线程要修改数据之前，取出记录时，获取当前数据库中的 version=1
    
*   第一个线程执行更新时，set version = newVersion where version = oldVersion
    
    *   newVersion = version+1 [2]
    *   oldVersion = version [1]
*   第二个线程执行更新时，set version = newVersion where version = oldVersion
    
    *   newVersion = version+1 [2]
    *   oldVersion = version [1]
*   假如这两个线程都来更新数据，第一个和第二个线程都可能先执行
    
    *   **假如第一个线程先执行更新，乐观锁发现 version 依然为 1，就把 version 改为 2，**
    *   **第二个线程再更新**的时候，set version = 2 where version = 1, 此时**乐观锁发现数据库表 version 已经为 2，所以第二个线程会修改失败**
    *   假如第二个线程先执行更新，会把 version 改为 2，
    *   第一个线程再更新的时候，set version = 2 where version = 1, 此时数据库表的数据 version 已经为 2，所以第一个线程会修改失败
    *   **不管谁先执行都会确保只能有一个线程更新数据，这就是 MP 提供的乐观锁的实现原理分析。**

上面所说的步骤具体该如何实现呢?

#### 4.4.3 代码实现

分析完步骤后，具体的实现步骤如下:

**步骤 1: 数据库表添加列，默认值 1**

列名可以任意，比如使用`version`, 给列设置默认值为`1`

![](<assets/1740015942399.png>)

**步骤 2: 在模型类中添加对应的属性，@Version**

根据添加的字段列名，在模型类中添加对应的属性值 version，并注解 @Version

```
@Data
//@TableName("tbl_user") 可以不写是因为配置了全局配置
public class User {
    @TableId(type = IdType.ASSIGN_UUID)
    private String id;
    private String name;
    @TableField(value="pwd",select=false)
    private String password;
    private Integer age;
    private String tel;
    @TableField(exist=false)
    private Integer online;
    private Integer deleted;
    @Version
    private Integer version;
}
```

**步骤 3: 添加乐观锁的拦截器**

```
@Configuration
public class MpConfig {
    @Bean
    public MybatisPlusInterceptor mpInterceptor() {
        //1.定义Mp拦截器
        MybatisPlusInterceptor mpInterceptor = new MybatisPlusInterceptor();
        //2.添加乐观锁拦截器
        mpInterceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
         //分页拦截器 
        //mpInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());   
        return mpInterceptor;
    }
}
```

**步骤 4：先查询 version 再更新操作** 

**要想实现乐观锁，首先第一步应该是拿到表中的 version，然后拿 version 当条件在将 version 加 1 更新回到数据库表中**，所以我们在修改的时候，需要对其进行查询

```
@SpringBootTest
class Mybatisplus03DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
	
    @Test
    void testUpdate(){
        //1.先通过要修改的数据id将当前数据查询出来
        User user = userDao.selectById(3L);
        //2.将要修改的属性逐一设置进去
        user.setName("Jock888");
        userDao.updateById(user);
    }
}
```

![](<assets/1740015942730.png>)

**注意：必须先查询再修改，修改后系统自动给 version 属性加 1。手动加和不查不加都是步行的。**如果手动加 version，提交后系统还会加一次 1.

**模拟加锁情况：**

看看能不能实现多个人修改同一个数据的时候，只能有一个人修改成功。

```
@SpringBootTest
class Mybatisplus03DqlApplicationTests {
 
    @Autowired
    private UserDao userDao;
	
    @Test
    void testUpdate(){
       //1.先通过要修改的数据id将当前数据查询出来
        User user = userDao.selectById(3L);     //version=3
        User user2 = userDao.selectById(3L);    //version=3
        user2.setName("Jock aaa");
        userDao.updateById(user2);              //version=>4
        user.setName("Jock bbb");
        userDao.updateById(user);               //verion=3?条件还成立吗？
    }
}
```

运行程序，分析结果：

![](<assets/1740015942979.png>)

乐观锁就已经实现完成了，如果对于上面的这些步骤记不住咋办呢?

**参考官方文档来实现:**

`https://mp.baomidou.com/guide/interceptor-optimistic-locker.html#optimisticlockerinnerinterceptor`

![](<assets/1740015943263.png>)

## 5，快速开发

### 5.1 代码生成器原理分析

造句:

![](<assets/1740015943461.png>)

我们可以往空白内容进行填词造句，比如:

![](<assets/1740015943677.png>)

在比如:

![](<assets/1740015943920.png>)

观察我们之前写的代码，会发现其中也会有很多重复内容，比如:

![](<assets/1740015944297.png>)

那我们就想，如果我想**做一个 Book 模块的开发，是不是只需要将红色部分的内容全部更换成`Book`即可**，如：

![](<assets/1740015944565.png>)

所以我们会发现，做任何模块的开发，对于这段代码，基本上都是对红色部分的调整，所以我们把去掉红色内容的东西称之为**模板**，红色部分称之为**参数**，以后只需要传入不同的参数，就可以根据模板创建出不同模块的 dao 代码。

除了 Dao 可以抽取模块，其实我们常见的类都可以进行抽取，只要他们有公共部分即可。再来看下模型类的模板：

![](<assets/1740015944753.png>)

*   ① 可以根据数据库表的表名来填充
*   ② 可以根据用户的配置来生成 ID 生成策略
*   ③到⑨可以根据数据库表字段名称来填充

所以只要我们知道是对哪张表进行代码生成，这些内容我们都可以进行填充。

分析完后，我们会发现，要想完成代码自动生成，我们需要有以下内容:

*   模板: MyBatisPlus 提供，可以自己提供，但是麻烦，不建议
*   数据库相关配置: 读取数据库获取表和字段信息
*   开发者自定义配置: 手工配置，比如 ID 生成策略

### 5.2 代码生成器实现

**步骤 1: 创建一个 Maven 项目**

**步骤 2: 导入对应的 jar 包** mybatis-plus,druid,lombok, 代码生成器 mybatis-plus-generator 和模板引擎 velocity-engine-core 依赖。

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.1</version>
    </parent>
    <groupId>com.itheima</groupId>
    <artifactId>mybatisplus_04_generator</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <!--spring webmvc-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
 
        <!--mybatisplus-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.1</version>
        </dependency>
 
        <!--druid-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.16</version>
        </dependency>
 
        <!--mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
 
        <!--test-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
 
        <!--lombok-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.12</version>
        </dependency>
 
        <!--代码生成器-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.4.1</version>
        </dependency>
 
        <!--velocity模板引擎-->
        <dependency>
            <groupId>org.apache.velocity</groupId>
            <artifactId>velocity-engine-core</artifactId>
            <version>2.3</version>
        </dependency>
 
    </dependencies>
 
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
 
</project>
 
```

**步骤 3: 编写引导类**

```
@SpringBootApplication
public class Mybatisplus04GeneratorApplication {
 
    public static void main(String[] args) {
        SpringApplication.run(Mybatisplus04GeneratorApplication.class, args);
    }
 
}
```

**步骤 4: 在引导类包下，创建代码生成类 CodeGenerator** 

```
public class CodeGenerator {
    public static void main(String[] args) {
        //1.获取代码生成器的对象
        AutoGenerator autoGenerator = new AutoGenerator();
 
        //创建DataSourceConfig 对象设置数据库相关配置
        DataSourceConfig dataSource = new DataSourceConfig();
        dataSource.setDriverName("com.mysql.cj.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/mybatisplus_db?serverTimezone=UTC");
        dataSource.setUsername("root");
        dataSource.setPassword("root");
        autoGenerator.setDataSource(dataSource);
 
        //设置全局配置
        GlobalConfig globalConfig = new GlobalConfig();
        globalConfig.setOutputDir(System.getProperty("user.dir")+"/mybatisplus_04_generator/src/main/java");    //设置代码生成位置
        globalConfig.setOpen(false);    //设置生成完毕后是否打开生成代码所在的目录
        globalConfig.setAuthor("黑马程序员");    //设置作者
        globalConfig.setFileOverride(true);     //设置是否覆盖原始生成的文件
        globalConfig.setMapperName("%sDao");    //设置数据层接口名，%s为占位符，指代模块名称
        globalConfig.setIdType(IdType.ASSIGN_ID);   //设置Id生成策略
        autoGenerator.setGlobalConfig(globalConfig);
 
        //设置包名相关配置
        PackageConfig packageInfo = new PackageConfig();
        packageInfo.setParent("com.aaa");   //设置生成的包名，与代码所在位置不冲突，二者叠加组成完整路径
        packageInfo.setEntity("domain");    //设置实体类包名
        packageInfo.setMapper("dao");   //设置数据层包名
        autoGenerator.setPackageInfo(packageInfo);
 
        //策略设置
        StrategyConfig strategyConfig = new StrategyConfig();
        strategyConfig.setInclude("tbl_user");  //设置当前参与生成的表名，参数为可变参数
        strategyConfig.setTablePrefix("tbl_");  //设置数据库表的前缀名称，模块名 = 数据库表名 - 前缀名  例如： User = tbl_user - tbl_
        strategyConfig.setRestControllerStyle(true);    //设置是否启用Rest风格
        strategyConfig.setVersionFieldName("version");  //设置乐观锁字段名
        strategyConfig.setLogicDeleteFieldName("deleted");  //设置逻辑删除字段名
        strategyConfig.setEntityLombokModel(true);  //设置是否启用lombok
        autoGenerator.setStrategy(strategyConfig);
        //2.执行代码生成器
        autoGenerator.execute();
    }
}
```

对于代码生成器中的代码内容，我们可以直接从官方文档中获取代码进行修改，

```
https://mp.baomidou.com/guide/generator.html

```

**步骤 5: 运行生成器类的 main 方法**

运行成功后，会在当前项目中生成很多代码，代码包含`controller`,`service`，`mapper`和`entity`

![](<assets/1740015945045.png>)

至此代码生成器就已经完成工作，我们能快速根据数据库表来创建对应的类，简化我们的代码开发。

### 5.3 MP 中 Service 的 CRUD

#### 5.3.0 快速入门 ，IService<User> 和 ServiceImpl<UserDao, User>

**回顾**我们之前业务层代码的编写，编写接口和对应的实现类:

```
public interface UserService{
	
}
 
@Service
public class UserServiceImpl implements UserService{
 
}
```

接口和实现类有了以后，需要在接口和实现类中声明方法

```
public interface UserService{
	public List<User> findAll();
}
 
@Service
public class UserServiceImpl implements UserService{
    @Autowired
    private UserDao userDao;
    
	public List<User> findAll(){
        return userDao.selectList(null);
    }
}
```

MP 看到上面的代码以后就说这些方法也是比较固定和通用的，那我来帮你抽取下，所以 MP 提供了一个 Service 接口和实现类，分别是:**`IService`和`ServiceImpl`**, 后者是对前者的一个具体实现。

以后我们自己写的 Service 就可以进行如下修改:

```
public interface UserService extends IService<User>{
	
}
 
@Service
public class UserServiceImpl extends ServiceImpl<UserDao, User> implements UserService{
 
}
```

修改以后的好处是，MP 已经帮我们把业务层的一些基础的增删改查都已经实现了，可以直接进行使用。

编写测试类进行测试:

```
@SpringBootTest
class Mybatisplus04GeneratorApplicationTests {
 
    private IUserService userService;
 
    @Test
    void testFindAll() {
        List<User> list = userService.list();
        System.out.println(list);
    }
 
}
```

**注意:**mybatisplus_04_generator 项目中对于 MyBatis 的环境是没有进行配置，如果想要运行，需要提取将配置文件中的内容进行完善后在运行。

思考: 在 MP 封装的 Service 层都有哪些方法可以用?

查看官方文档:`https://mp.baomidou.com/guide/crud-interface.html`, 这些提供的方法大家可以参考官方文档进行学习使用，方法的名称可能有些变化，但是方法对应的参数和返回值基本类似。

#### 5.3.1 保存，Save,SaveOrUpdate

**Save**

```
// 插入一条记录（选择字段，策略插入）
boolean save(T entity);
// 插入（批量）
boolean saveBatch(Collection<T> entityList);
// 插入（批量）
boolean saveBatch(Collection<T> entityList, int batchSize);
```

参数说明

<table><thead><tr><th>类型</th><th>参数名</th><th>描述</th></tr></thead><tbody><tr><td>T</td><td>entity</td><td>实体对象</td></tr><tr><td>Collection&lt;T&gt;</td><td>entityList</td><td>实体对象集合</td></tr><tr><td>int</td><td>batchSize</td><td>插入批次数量</td></tr></tbody></table>

**SaveOrUpdate**

```
// TableId 注解存在更新记录，否插入一条记录
boolean saveOrUpdate(T entity);
// 根据updateWrapper尝试更新，否继续执行saveOrUpdate(T)方法
boolean saveOrUpdate(T entity, Wrapper<T> updateWrapper);
// 批量修改插入
boolean saveOrUpdateBatch(Collection<T> entityList);
// 批量修改插入
boolean saveOrUpdateBatch(Collection<T> entityList, int batchSize);
```

参数说明

<table><thead><tr><th>类型</th><th>参数名</th><th>描述</th></tr></thead><tbody><tr><td>T</td><td>entity</td><td>实体对象</td></tr><tr><td>Wrapper&lt;T&gt;</td><td>updateWrapper</td><td>实体对象封装操作类 UpdateWrapper</td></tr><tr><td>Collection&lt;T&gt;</td><td>entityList</td><td>实体对象集合</td></tr><tr><td>int</td><td>batchSize</td><td>插入批次数量</td></tr></tbody></table>

#### 5.3.2 移除 Remove

```
// 根据 entity 条件，删除记录
boolean remove(Wrapper<T> queryWrapper);
// 根据 ID 删除
boolean removeById(Serializable id);
// 根据 columnMap 条件，删除记录
boolean removeByMap(Map<String, Object> columnMap);
// 删除（根据ID 批量删除）
boolean removeByIds(Collection<? extends Serializable> idList);
```

参数说明

<table><thead><tr><th>类型</th><th>参数名</th><th>描述</th></tr></thead><tbody><tr><td>Wrapper&lt;T&gt;</td><td>queryWrapper</td><td>实体包装类 QueryWrapper</td></tr><tr><td>Serializable</td><td>id</td><td>主键 ID</td></tr><tr><td>Map&lt;String, Object&gt;</td><td>columnMap</td><td>表字段 map 对象</td></tr><tr><td>Collection&lt;? extends Serializable&gt;</td><td>idList</td><td>主键 ID 列表</td></tr></tbody></table>

#### 5.3.3 更改 Update

```
// 根据 UpdateWrapper 条件，更新记录 需要设置sqlset
boolean update(Wrapper<T> updateWrapper);
// 根据 whereWrapper 条件，更新记录
boolean update(T updateEntity, Wrapper<T> whereWrapper);
// 根据 ID 选择修改
boolean updateById(T entity);
// 根据ID 批量更新
boolean updateBatchById(Collection<T> entityList);
// 根据ID 批量更新
boolean updateBatchById(Collection<T> entityList, int batchSize);
```

参数说明

<table><thead><tr><th>类型</th><th>参数名</th><th>描述</th></tr></thead><tbody><tr><td>Wrapper&lt;T&gt;</td><td>updateWrapper</td><td>实体对象封装操作类 UpdateWrapper</td></tr><tr><td>T</td><td>entity</td><td>实体对象</td></tr><tr><td>Collection&lt;T&gt;</td><td>entityList</td><td>实体对象集合</td></tr><tr><td>int</td><td>batchSize</td><td>更新批次数量</td></tr></tbody></table>

#### 5.3.4 查询，Get,List,Page,Count,Chain

**Get** 

```
// 根据 ID 查询
T getById(Serializable id);
// 根据 Wrapper，查询一条记录。结果集，如果是多个会抛出异常，随机取一条加上限制条件 wrapper.last("LIMIT 1")
T getOne(Wrapper<T> queryWrapper);
// 根据 Wrapper，查询一条记录
T getOne(Wrapper<T> queryWrapper, boolean throwEx);
// 根据 Wrapper，查询一条记录
Map<String, Object> getMap(Wrapper<T> queryWrapper);
// 根据 Wrapper，查询一条记录
<V> V getObj(Wrapper<T> queryWrapper, Function<? super Object, V> mapper);
```

参数说明

<table><thead><tr><th>类型</th><th>参数名</th><th>描述</th></tr></thead><tbody><tr><td>Serializable</td><td>id</td><td>主键 ID</td></tr><tr><td>Wrapper&lt;T&gt;</td><td>queryWrapper</td><td>实体对象封装操作类 QueryWrapper</td></tr><tr><td>boolean</td><td>throwEx</td><td>有多个 result 是否抛出异常</td></tr><tr><td>T</td><td>entity</td><td>实体对象</td></tr><tr><td>Function&lt;? super Object, V&gt;</td><td>mapper</td><td>转换函数</td></tr></tbody></table>

**List**

```
// 查询所有
List<T> list();
// 查询列表
List<T> list(Wrapper<T> queryWrapper);
// 查询（根据ID 批量查询）
Collection<T> listByIds(Collection<? extends Serializable> idList);
// 查询（根据 columnMap 条件）
Collection<T> listByMap(Map<String, Object> columnMap);
// 查询所有列表
List<Map<String, Object>> listMaps();
// 查询列表
List<Map<String, Object>> listMaps(Wrapper<T> queryWrapper);
// 查询全部记录
List<Object> listObjs();
// 查询全部记录
<V> List<V> listObjs(Function<? super Object, V> mapper);
// 根据 Wrapper 条件，查询全部记录
List<Object> listObjs(Wrapper<T> queryWrapper);
// 根据 Wrapper 条件，查询全部记录
<V> List<V> listObjs(Wrapper<T> queryWrapper, Function<? super Object, V> mapper);
```

参数说明

<table><thead><tr><th>类型</th><th>参数名</th><th>描述</th></tr></thead><tbody><tr><td>Wrapper&lt;T&gt;</td><td>queryWrapper</td><td>实体对象封装操作类 QueryWrapper</td></tr><tr><td>Collection&lt;? extends Serializable&gt;</td><td>idList</td><td>主键 ID 列表</td></tr><tr><td>Map&lt;String, Object&gt;</td><td>columnMap</td><td>表字段 map 对象</td></tr><tr><td>Function&lt;? super Object, V&gt;</td><td>mapper</td><td>转换函数</td></tr></tbody></table>

**Page**

```
// 无条件分页查询
IPage<T> page(IPage<T> page);
// 条件分页查询
IPage<T> page(IPage<T> page, Wrapper<T> queryWrapper);
// 无条件分页查询
IPage<Map<String, Object>> pageMaps(IPage<T> page);
// 条件分页查询
IPage<Map<String, Object>> pageMaps(IPage<T> page, Wrapper<T> queryWrapper);
```

参数说明

<table><thead><tr><th>类型</th><th>参数名</th><th>描述</th></tr></thead><tbody><tr><td>IPage&lt;T&gt;</td><td>page</td><td>翻页对象</td></tr><tr><td>Wrapper&lt;T&gt;</td><td>queryWrapper</td><td>实体对象封装操作类 QueryWrapper</td></tr></tbody></table>

**Count**

```
// 查询总记录数
int count();
// 根据 Wrapper 条件，查询总记录数
int count(Wrapper<T> queryWrapper);
```

参数说明

<table><thead><tr><th>类型</th><th>参数名</th><th>描述</th></tr></thead><tbody><tr><td>Wrapper&lt;T&gt;</td><td>queryWrapper</td><td>实体对象封装操作类 QueryWrapper</td></tr></tbody></table>

**Chain**

```
// 链式查询 普通
QueryChainWrapper<T> query();
// 链式查询 lambda 式。注意：不支持 Kotlin
LambdaQueryChainWrapper<T> lambdaQuery();
 
// 示例：
query().eq("column", value).one();
lambdaQuery().eq(Entity::getId, value).list();
```

```
// 链式更改 普通
UpdateChainWrapper<T> update();
// 链式更改 lambda 式。注意：不支持 Kotlin
LambdaUpdateChainWrapper<T> lambdaUpdate();
 
// 示例：
update().eq("column", value).remove();
lambdaUpdate().eq(Entity::getId, value).update(entity);
```

## 6，**ActiveRecord** 简介

### 6.1 概念

**什么是 ActiveRecord？**  
**ActiveRecord 属于 ORM（对象关系映射）层**，由 Rails 最早提出，遵循标准的 **ORM 模型：表映射到记录，记录映射到对象，字段映射到对象属性。**配合遵循的命名和配置惯例，能够很大程度的快速实现模型的操作，而且简洁易懂。

  
**ActiveRecord 的主要思想是：**

*   **每一个数据库表对应创建一个类，类的每一个对象实例对应于数据库中表的一行记录；通常表的每个字段在类中都有相应的 Field；**
*   ActiveRecord 同时负责把自己**持久化**，在 ActiveRecord 中封装了对数据库的访问，即 CURD;；
*   ActiveRecord 是一种领域模型 (Domain Model)，封装了部分业务逻辑；

### 6.2 增删改查

在 MP 中，开启 AR 非常简单，只需要将**实体对象继承 Model <实体类>** 即可。

```
class User extends Model<User>{
    // fields...
}
```

**注意：**

*   **之后增删改查就可以不用自动注入 dao 接口，直接实体类对象就能调用 mp 的各种方法。**
*   **但 dao 还不能删除，因为 ar 底层还是 dao。**

 **查询：**

```
    @Test
    void test() {
        User user = new User();
        System.out.println(user.selectById(2L));    //或者给user设置id后user.selectById()
    }
```

```
User user = new User();
QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
userQueryWrapper.le("age","20");
List<User> users = user.selectList(userQueryWrapper);
for (User user1 : users) {
System.out.println(user1);
}
```

**新增：**

```
User user = new User();
user.setName("刘备");
user.setAge(30);
user.setPassword("123456");
user.setUserName("liubei");
user.setEmail("liubei@itcast.cn");
boolean insert = user.insert();
System.out.println(insert);
```

**删除：**

```
User user = new User();
user.setId(7L);
boolean delete = user.deleteById();
System.out.println(delete);
```

**修改：**

```
User user = new User();
user.setId(8L);
user.setAge(35);
boolean update = user.updateById();
System.out.println(update);
```