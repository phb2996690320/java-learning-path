---
url: https://blog.csdn.net/qq_40991313/article/details/126709822?spm=1001.2014.3001.5501
title: MySQL 主从复制实现读写分离_mysql 也支持主从复制来实现读写分离 - CSDN 博客
date: 2025-02-20 09:49:17
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[读写分离](#t0)

[1.1 多台数据库](#t1)

[1.2.MySQL 主从复制（二进制日志）](#t2)

[1.2.1 介绍](#t3)

[1.2.2 配置主从数据库](#t4)

[1.2.3 测试](#t5)

[1.3 读写分离案例](#t6)

[1.3.1 背景](#t7)

[1.3.2 Sharding-JDBC 实现读写分离](#t8)

[1.3.3 入门案例](#t9)

[1.4. 项目实现读写分离](#t10)

[1.4.1 主从复制](#t11)

[1.4.2 代码实现](#t12)

## 读写分离

### 1.1 多台数据库

**单台数据库**

读和写都在单台数据库，访问压力过大，数据库服务器磁盘损坏则数据丢失，单点故障。 

![](<assets/1740016157059.png>)

**多台数据库减小服务器压力**

写操作在主库完成，主库数据同步到从库，读操作在从库完成。

![](<assets/1740016157404.png>)

### 1.2.MySQL 主从复制（二进制日志）

#### 1.2.1 介绍

**MySQL 主从复制是一个异步的复制过程，底层是基于 Mysql 数据库自带的二进制日志功能。**

就是一台或多台 MysQL 数据库 (slave，即**从库**) **从**另一台 NySQL 数据库（master，即**主库**）**进行日志的复制**然后再**解析日志并应用到自身**，最终实现**从库的数据和主库的数据保持一致**。MySQL 主从复制是 NySQL 数据库**自带功能**，无需借助第三方工具。

**MySQL 复制过程:**

1.  master 将 “改变” 记录到二进制日志（binary log)
2.  slave 将 master 的 binary log 拷贝到它的中继日志（relay log）
3.  slave 重做中继日志中的事件，将改变应用到自己的数据库中

![](<assets/1740016157650.png>)

**主库 1 个，从库可以有多个**

#### 1.2.2 配置主从数据库

**前置条件：**提前准备两台服务器（linux），使用 finalshell 连接后，分别安装 mysql 并启动服务成功。 

1.2.2**.1 配置主库 maser（centos1）**

**建议主从库都用 centos，跟自己 Windows 环境的数据库区分开**

**第一步：修改 MySQL 数据库的配置文件**， /etc/my.cfg

```
vim /ect/my.cnf

```

修改 

```
[mysqld]
log-bin=mysql-bin #[必须]启用二进制日志
server-id=100 #[必须] 服务器唯一id
```

**第二步：重启 MySQL**

 如果是 linux：

```
systemctl restart mysqld


```

**第三步：登录 MySQL 数据库，执行以下 SQL**

```
mysql -uroot -p密码

```

```
GRANT REPLICATION SLAVE ON *.* to 'xiaoming'@'%' identified by 'Root@123456';


```

**注意：**

*   上面 SQL 的作用是创建一个用户 xiaoming, 密码为 Root@123456, 并且给 xiaoming 用户授予 REPLICATION SLAVE 权限。密码和用户名可以自己设置。
*   REPLICATION SLAVE（replication 译为主从复制，努力）权限常用于建立复制时所需要用到的用户权限，也就是 slave 必须被 master 授权具有该权限的用户，才能通过该用户复制。

**第四步：查看 master 状态，执行以下 sql，记录结果中的 FIle 和 Position 的值**

```
show master status;


```

![](<assets/1740016157900.png>)

执行完上面 SQL 语句，不要再执行任何操作

1.2.2**.2 配置从库 Slave（centos 数据库 2）**

可以新建一个 centos，安装数据库作为从库，也可以使用当前 Windows，主要是主从库不能是同一台服务器。

**第一步，修改 MySQL 数据库的配置文件**， /etc/my.cnf

```
vim /ect/my.cnf

```

 修改：

```
[mysqld]
#从数据库的唯一id
server-id=101
```

如果是 Windows，修改 mysql 安装目录的 my.ini

**第二步：重新启动 mysql 服务**

```
systemctl restart mysqld


```

 如果是 Windows

```
net stop mysql
net start mysql
```

**第三步：登录 mysl，设置主库 ip、用户名密码、日志文件名、密码**

```
mysql -uroot -p123456


```

**执行以下 SQL 语句**

先将下面信息修改成自己的主数据库信息： 

```
change master to
master_host= '192.168.112.100',master_user='xiaoming',master_password='Root@123456', master_log_file= 'mysql-bin.000002',master_log_pos=441;
start slave;
 
```

**注意：**

*   如果提示已经有 slave 运行就先停止：stop slave; 然后再绑定主机
*   **日志名、位置一定要根据自己查主库状态：**

```
show master status;

```

![](<assets/1740016158107.png>) 

**第四步：查询从数据库的状态**

```
show slave status;


```

复制粘贴到记事本，更直观： 

![](<assets/1740016158374.png>)

#### 1.2.3 测试

主库新增数据，从库也自动跟着新增数据。 

![](<assets/1740016158769.png>)

### 1.3 读写分离案例

#### 1.3.1 背景

面对日益增加的系统访问量, 数据库的吞吐量面临着巨大瓶颈。对于同一时刻有大量并发读操作和较少写操作类型的应用系统来说，将数据库拆分为主库和从库，

*   **主库负责处理事务性的增删改操作**
*   **从库负责处理查询操作**

能够有效的避免由数据更新导致的行锁，使得整个系统的查询性能得到极大的改善。

![](<assets/1740016159019.png>)

#### 1.3.2 Sharding-JDBC 实现读写分离

Sharding-JDBC 定位为轻量级 Java 框架，在 Java 的 JDBC 层提供的额外服务。它**使用客户端直连数据库，以 jar 包形式提供服务，Maven 中导入坐标即可使用**，可理解为增强版的 JDBC 驱动，**完全兼容 JDBC 和各种 ORM 框架**。 使用 Sharding-JDBC 可以在程序中轻松的实现数据库读写分离。

*   适用于任何基于 JDBC 的 ORM 框架，如: JPA, Hibernate, Mybatis, Spring JDBC Template 或直接使用 JDBC。
*   支持任何第三方的数据库连接池，如: DBCP, C3PO, BoneCP, Druid, HikariCP 等。
*   支持任意实现 JDBC 规范的数据库。目前支持 MySQL, Oracle, SQLServer, PostgreSQL 以及 任何遵循 SQL92 标准的数据库。

#### 1.3.3 入门案例

**1.3.3.1 导入 Sharding-JDBC 的 maven 坐标**

```
<dependency>
    <groupId>org.apache.shardingsphere</groupId>
    <artifactId> sharding-jdbc-spring-boot-starter</artifactId>
    <version>4.0. 0-RC1</version>
</dependency>
```

**1.3.3.2 在配置文件中配置读写分离规则**

```
server:
  port: 8080
mybatis-plus:
  configuration:
    #在映射实体或者属性时，将数据库中表名和字段名中的下划线去掉，按照驼峰命名法映射
    map-underscore-to-camel-case: true
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  global-config:
    db-config:
      id-type: ASSIGN_ID
 
spring:
  shardingsphere:
    datasource:
      names:
        master,slave
      ## 主数据源
      master:
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://192.168.112.100:3306/rw?characterEncoding=utf-8
        username: root
        password: 123456
      ## 从数据源
      slave:
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://192.168.138.101:3306/rw?characterEncoding=utf-8
        username: root
        password: 123456
    masterslave:
      ## 读写分离配置
      load-balance-algorithm-type: round_robin
      ## 最终的数据源名称
      name: dataSource
      ## 主库数据源名称
      master-data-source-name: master
      ## 从库数据源名称列表，多个逗号分隔
      slave-data-source-names: slave
    props:
      sql:
        show: true #开启SQL显示，默认false
  main:
    allow-bean-definition-overriding: true
```

**1.3.3.3 在配置文件中配置允许 bean 定义覆盖配置项**

```
  main:
    allow-bean-definition-overriding: true
```

### 1.4. 项目实现读写分离

#### 1.4.1 主从复制

利用前面配置好的主从关系，在主库中创建一个数据库，并导入提供的数据库表，更新主从数据库即可

![](<assets/1740016159258.png>)

![](<assets/1740016159615.png>)

#### 1.4.2 代码实现

**1.4.2.1 导入 maven 依赖**

```
<dependency>
    <groupId>org.apache.shardingsphere</groupId>
    <artifactId> sharding-jdbc-spring-boot-starter</artifactId>
    <version>4.0.0-RC1</version>
</dependency>
```

**1.4.2.2 在配置文件中配置读写分离规则**

```
server:
  port: 8080
spring:
  application:
    name: reggie_take_out
  shardingsphere:
    datasource:
#自定义数据源名字，名字随便取，注意是下面masterslave配置主从数据源名字
      names:
        master,slave
      ## 主数据源
      master:
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://192.168.112.100:3306/rjdb?characterEncoding=utf-8
        username: root
        password: 123456
      ## 从数据源
      slave:
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://127.0.0.1:3306/rjdb?characterEncoding=utf-8
        username: root
        password: 123456
#配置主从信息
    masterslave:
      ## 从库的负载均衡算法类型，round_robin意思是几个从库轮流查询
      load-balance-algorithm-type: round_robin
      ## 最终的数据源名称
      name: dataSource
      ## 主库数据源名称
      master-data-source-name: master
      ## 从库数据源名称列表，多个逗号分隔
      slave-data-source-names: slave
    props:
      sql:
        show: true #开启SQL显示，默认false
#允许bean定义覆盖配置项
  main:
    allow-bean-definition-overriding: true
 
  redis:
    host: 192.168.112.100
    port: 6379
    password: 123456
    database: 0
  cache:
    redis:
      time-to-live: 1800000  #设置缓存数据的过期时间
mybatis-plus:
  configuration:
    #在映射实体或者属性时，将数据库中表名和字段名中的下划线去掉，按照驼峰命名法映射
    map-underscore-to-camel-case: true
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  global-config:
    db-config:
      id-type: ASSIGN_ID
 
## 文件上传后保存路径
rj:
  path: F:\JavaCode\RuiJiProject\src\main\java\com\jq\uploaddata\
```

**1.4.2.3 允许 bean 定义覆盖配置项**

**不配置时报错：**

![](<assets/1740016160006.png>)

配置：

```
#允许bean定义覆盖配置项
spring:
  main:
    allow-bean-definition-overriding: true
```