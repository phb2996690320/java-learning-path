---
url: https://blog.csdn.net/qq_40991313/article/details/126898388?spm=1001.2014.3001.5501
title: SpringCloud 基础 6——分布式事务，Seata_springcloud seata 连接问题 - CSDN 博客
date: 2025-02-20 09:59:31
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1. 分布式事务问题](#t0)

[1.1. 本地事务](#t1)

[1.1.1.MySQL 事务的 ACID 原则](#t2) 

[1.1.2.MySQL 事务的隔离级别](#t3) 

[1.1.3. 事务的传播行为](#t4)

[1.1.4. 本地事务代理对象、事务传播行为不生效问题](#t5)

[1.2. 分布式事务](#t6)

[1.3. 演示分布式事务问题](#t7)

[2. 理论基础](#t8)

[2.1.CAP 定理](#t9)

[2.1.1. 一致性 C：数据同步](#t10)

[2.1.2. 可用性 A：节点正常访问](#t11)

[2.1.3. 分区容错 P：分区后也要对外提供服务](#t12)

[2.1.4. 矛盾](#t13)

[2.2.BASE 理论](#t14)

[2.3. 解决分布式事务的思路，最终一致和强一致](#t15)

[3. 初识 Seata](#t16)

[3.1.Seata 的架构，TC,TM,RM](#t17)

[3.2. 部署 Seata 的 TC（事务协调者）服务，tc-server](#t18)

[1. 下载](#t19)

[2. 解压](#t20)

[3.registry.conf 修改 Seata 的注册中心、配置中心](#t21)

[4. 在 nacos 添加配置](#t22)

[5. 创建数据库表，分支事务表和全局事务表](#t23)

[6. 启动 TC 服务](#t24)

[3.3. 微服务集成 Seata](#t25)

[3.3.1. 引入依赖](#t26)

[3.3.2. 配置 TC 地址](#t27)

[3.3.3. 其它服务](#t28)

[4. 详解 Seata 的四种分布式事务方案](#t29)

[4.1.XA 模式，两阶段都加锁](#t30)

[4.1.1. 两阶段提交](#t31)

[4.1.2.Seata 的 XA 模型](#t32)

[4.1.3. 优缺点](#t33)

[4.1.4. 实现 XA 模式](#t34)

[4.2.AT 模式：一阶段直接提交，二阶段统一回滚](#t35)

[4.2.1.Seata 的 AT 模型](#t36)

[4.2.2. 流程梳理](#t37)

[4.2.3.AT 与 XA 的区别](#t38)

[4.2.4. 脏写问题，全局锁](#t39)

[4.2.5. 优缺点](#t40)

[4.2.6. 实现 AT 模式（完整流程）](#t41)

[4.2.7.@Transactional 和 @GlobalTransactional 对比](#t42)

[4.3.TCC 模式：基于资源预留](#t43)

[4.3.1. 流程分析](#t44)

[4.3.2.Seata 的 TCC 模型](#t45)

[4.3.3. 优缺点](#t46)

[4.3.4. 事务悬挂和空回滚](#t47)

[4.3.5. 下单代码实现 TCC 模式，@TwoPhaseBusinessAction，@BusinessActionContextParameter，](#t48)

[4.4.SAGA 模式：基于一系列本地事务](#t49)

[4.4.1. 原理](#t50)

[4.4.2. 优缺点](#t51)

[4.5. 四种模式对比，XA,AT,TCC,SAGA](#t52)

[5. 其他分布式事务方案（不使用 Seata）](#t53)

[5.1. 柔性事务方法实现最终一致性](#t54)

[5.1.1. 柔性事务 - 最大努力通知型方案](#t55)

[5.1.2. 柔性事务 - 可靠消息 + 最终一致性方案（异步确保型）](#t56)

[5.2.xxl-job 和消息组件实现最终一致性](#t57)

[6. 高可用](#t58)

[6.1. 高可用架构模型](#t59)

[6.2. 实现高可用，异地容灾](#t60)

[6.2.1. 模拟异地容灾的 TC 集群](#t61)

[6.2.2. 将事务组映射配置到 nacos](#t62)

[3. 微服务读取 nacos 配置](#t63)

## 1. 分布式事务问题

### 1.1. 本地事务

#### 1.1.1.MySQL 事务的 ACID 原则 

只要是事务，就必须要**满足四个原则：**

ACID 是指事务四个特性：原子性（Atomicity）、一致性（Consistency）、隔离性（Isolation）和持久性（Durability）。

**原子性：**事务的所有操作，要么全部成功，要么全部失败。 

**一致性：**事务前后，数据库的约束没有被破坏，保持前后一致。

**隔离性：**操作同一资源的并发事务之间相互隔离，不会互相干扰。

**持久性：**事务的结果最终一定会持久化到数据库，宕机等故障也无法影响。

![](<assets/1740016782382.png>)

#### 1.1.2.MySQL **事务的隔离级别** 

事务隔离级别是指操作同一资源的并发事务之间的隔离度。隔离级别越高，事务之间的相互干扰就越小，安全性就越高。

**读问题：**

*   **脏读：**读到了脏数据。当前事务读到另一个未提交事务刚改的数据。只有读未提交会脏读。
*   **不可重复读：**前后重复读的数据不一样。前后两次读同数据，这期间数据被其他事务改了，导致前后读取的数据不同。
*   **幻读：**前后读的数据是一样，但多了几行或少了几行，像幻觉一样。事务前后读的数据集合不同，导致出现 “幻像” 行。仅串行化能解决幻读问题。

**事务隔离级别**

*   **读未提交：**事务能读到所有未提交事务的数据。压根不加锁、没隔离，性能最高。
*   **读提交：**事务能读到已提交事务的数据。底层由 MVVC 实现 。解决脏读问题。
*   **可重复读（默认）：**前后读的数据相同。底层由 MVVC 实现 。解决脏读、不可重复读问题。
*   **串行化：**事务一拿到锁阻塞其他事务，直到释放锁。读时共享锁，写时排它锁。阻塞导致性能最差。解决脏读、不可重复读、幻读问题。

MVVC：多版本并发控制。

共享锁：在共享锁下，多个线程可以同时读取数据，但只有一个线程能够修改数据。当一个线程在修改数据时，必须获得独占锁，以便其他线程不能访问数据。

排它锁：在排它锁下，只有一个线程可以修改数据，其他线程不允许访问数据。

SQL 标准定义了四种隔离级别，这四种隔离级别分别是：

- 读未提交（READ UNCOMMITTED）

- 读提交 （READ COMMITTED）；

- 可重复读 （REPEATABLE READ）；

- 串行化 （SERIALIZABLE）。

4 种隔离级别 MySQL 都支持，并且 **InnoDB** 存储引擎**默认**的支持隔离级别是**可重复读** REPEATABLE READ，但是与标准 SQL 不同的是，InnoDB 存储引擎在 REPEATABLE READ 事务隔离级别下，使用 Next-Key Lock 的锁算法，因此避免了幻读的产生。所以，InnoDB 存储引擎在默认的事务隔离级别下已经能完全保证事务的隔离性要求，即达到 SQL 标准的 SERIALIZABLE 隔离级别； 

下面是四种隔离级别在解决脏读、不可重复读、幻读问题方面的情况：

<table><thead><tr><th>隔离级别</th><th>脏读</th><th>不可重复读</th><th>幻读</th></tr></thead><tbody><tr><td>读未提交</td><td>存在</td><td>存在</td><td>存在</td></tr><tr><td>读已提交</td><td>不存在</td><td>存在</td><td>存在</td></tr><tr><td>可重复读</td><td>不存在</td><td>不存在</td><td>存在</td></tr><tr><td>串行化</td><td>不存在</td><td>不存在</td><td>不存在</td></tr></tbody></table>

脏读（Dirty Read）：指一个事务读取了另一个未提交的事务所写入的数据，如果隔离级别越高，则越不容易出现脏读问题。

不可重复读（Non-Repeatable Read）：指一个事务在读取同一数据时，由于另外一个事务的修改或删除，导致两次读取的数据不同。如果隔离级别越高，则越不容易出现不可重复读问题。

幻读（Phantom Read）：指一个事务多次执行同一个查询，但每次返回的数据集合都不同，导致出现 “幻像” 行。如果隔离级别越高，则越不容易出现幻读问题。

#### 1.1.3. 事务的传播行为

**PROPAGATION_REQUIRED（默认）：** 如果当前没有事务， 就创建一个新事务， 如果当前存在事务，就加入该事务， 该设置是**最常用**的设置。

![](<assets/1740016782558.png>)

  
PROPAGATION_SUPPORTS： 支持当前事务， 如果当前存在事务， 就加入该事务， 如果当前不存在事务， 就以非事务执行。  
PROPAGATION_MANDATORY： 支持当前事务， 如果当前存在事务， 就加入该事务， 如果当前不存在事务， 就抛出异常。  
**PROPAGATION_REQUIRES_NEW：** 创建新事务， 无论当前存不存在事务， 都创建新事务。  
PROPAGATION_NOT_SUPPORTED： 以非事务方式执行操作， 如果当前存在事务， 就把当前事务挂起。  
PROPAGATION_NEVER： 以非事务方式执行， 如果当前存在事务， 则抛出异常。  
PROPAGATION_NESTED： 如果当前存在事务， 则在嵌套事务内执行。 如果当前没有事务，则执行与 PROPAGATION_REQUIRED 类似的操作。

#### 1.1.4. 本地事务代理对象、**事务传播行为不生效问题**

**理论上，**下面代码，执行 a 方法，因为事务失败，b 会回滚、c 不会回滚；a,b 共用一个事务，a 的超时时间会覆盖 b 的超时时间。

**但是**，因为方法 a、b、c 都在同一个 service 里面，**事务传播行为不生效**，他们之间只是一个简单的方法调用，共享一个事务  
**原理：**事务是用代理对象来控制的，内部调用 b(),c(), 就相当于直接调用没有经过事务【**绕过了代理对象】**

```
//1、如果方法a、b、c都在同一个service里面，事务传播行为不生效，共享一个事务
//	原理：事务是用代理对象来控制的，内部调用b(),c(),就相当于直接调用没有经过事务【绕过了代理对象】
//	解决：不能使用this.b()；也不能注入自己【要使用代理对象来调用事务方法】
		
		
@Transactional(timeout=30)
public void a() {
	b();// a事务传播给了b事务，并且b事务的设置失效
	c();// c单独创建一个新事务
}
 
@Transactional(propagation = Propagation.REQUIRED, timeout=2)
public void b() {
 
}
 
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void c() {
 
}
```

**解决：要使用代理对象来调用事务方法。**不能使用 this.b()；也不能注入自己

 1. 引入 aop 的 starter 依赖，里面有 aspectj 依赖

```
<!-- 引入aop，解决本地事务失效问题 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
        </dependency
```

2、开启动态代理【默认使用 jdk 动态代理，需要有接口】 

```
OrderServiceImpl orderService = (OrderServiceImpl)AopContext.currentProxy(); 
orderService.b(); 
orderService.c(); 
```

3、获取动态代理对象

```
registry {
  ## tc服务的注册中心类，这里选择nacos，也可以是eureka、zookeeper等
  type = "nacos"    #注册中心类型是nacos
 
                    
  nacos {
    ## seata tc 服务注册到 nacos的服务名称，可以自定义
    application = "seata-tc-server"
    serverAddr = "127.0.0.1:8848"
    group = "DEFAULT_GROUP"
    namespace = ""
    cluster = "SH"
    username = "nacos"
    pass改成自己的密码word = "nacos"
  }
}
 
config {
  ## 读取tc服务端的配置文件的方式，这里是从nacos配置中心读取，这样如果tc是集群，可以共享配置
  type = "nacos"    #也可以设置成file类型配置，会从本目录下找"file.conf"读取配置
  ## 配置nacos地址等信息
  nacos {
    serverAddr = "127.0.0.1:8848"
    namespace = ""
    group = "SEATA_GROUP"
    username = "nacos"
    pass改成自己的密码word = "nacos"
    dataId = "seataServer.properties"
  }
}
```

### 1.2. 分布式事务

 _分布式事务_是指事务的参与者、支持事务的服务器、资源服务器以及事务管理器分别位于不同的分布式系统的**不同节点**之上。

**分布式事务的场景：**

**跨 JVM 进程：**微服务架构下，远程调用

![](<assets/1740016782750.png>)

**跨数据库：**单服务操作多个数据库：

注意这里说的是多个数据库，而不是同一个数据库下的多个表

![](<assets/1740016783098.png>)

**多服务单数据库:**

![](<assets/1740016783599.png>)

**分布式事务**，就是指不是在单个服务或单个数据库架构下，产生的事务，例如：

*   跨数据源的分布式事务
*   跨服务的分布式事务
*   综合情况

在数据库水平拆分、服务垂直拆分之后，一个业务操作通常要跨多个数据库、服务才能完成。例如电商行业中比较常见的下单付款案例，包括下面几个行为：

*   创建新订单
*   扣减商品库存
*   从用户账户余额扣除金额

完成上面的操作**需要访问三个不同的微服务和三个不同的数据库。**

![](<assets/1740016783944.png>)

订单的创建、库存的扣减、账户扣款在每一个服务和数据库内是一个本地事务，可以保证 ACID 原则。

但是当我们把三件事情看做一个 "业务"，要满足保证 “业务” 的原子性，**要么所有操作全部成功，要么全部失败**，不允许出现部分成功部分失败的现象，这就是**分布式系统下的事务**了。

此时 ACID 难以满足，这是分布式事务要解决的问题

### 1.3. 演示分布式事务问题

我们通过一个案例来演示分布式事务的问题：

1）**创建数据库，名为 seata_demo，然后导入课前资料提供的 SQL 文件：**

![](<assets/1740016784218.png>)

**订单表 order_tbl**

![](<assets/1740016784450.png>)

**账户表 account_tbl** 

![](<assets/1740016784742.png>)

**库存表 storage_tbl：**

![](<assets/1740016785016.png>)

2）**导入课前资料提供的微服务：**

![](<assets/1740016785263.png>)

微服务结构如下：

![](<assets/1740016785444.png>)

**记得改一下 yml 中数据库密码** 

其中：

**seata-demo：**父工程，负责管理项目依赖

*   **account-service：**账户服务，负责管理用户的资金账户。提供扣减余额的接口
*   **storage-service：**库存服务，负责管理商品库存。提供扣减库存的接口
*   **order-service：**订单服务，负责管理订单。创建订单时，需要调用 account-service 和 storage-service

**3）启动 nacos、所有微服务**

```
#store事务日志存在哪里，这里设成数据库。也可以设成file类型
# 数据存储方式，db代表数据库
store.mode=db
store.db.datasource=druid
store.db.dbType=mysql
store.db.driverClassName=com.mysql.jdbc.Driver
store.db.url=jdbc:mysql://127.0.0.1:3306/seata?useUnicode=true&rewriteBatchedStatements=true
store.db.user=root
store.db.password=123
store.db.minConn=5
store.db.maxConn=30
store.db.globalTable=global_table    #全局事务的表
store.db.branchTable=branch_table    #分支事务的表
store.db.queryLimit=100
store.db.lockTable=lock_table        #锁的表
store.db.maxWait=5000
## 事务、日志等配置
server.recovery.committingRetryPeriod=1000
server.recovery.asynCommittingRetryPeriod=1000
server.recovery.rollbackingRetryPeriod=1000
server.recovery.timeoutRetryPeriod=1000
server.maxCommitRetryTimeout=-1
server.maxRollbackRetryTimeout=-1
server.rollbackRetryTimeoutUnlockEnable=false
server.undo.logSaveDays=7
server.undo.logDeletePeriod=86400000
 
## 客户端与服务端传输方式
transport.serialization=seata
transport.compressor=none
## 关闭metrics功能，提高性能
metrics.enabled=false
metrics.registryType=compact
metrics.exporterList=prometheus
metrics.exporterPrometheusPort=9898
```

**4）测试下单功能，发出 Post 请求：**

请求如下：

**坑点：**

*   ```
    transport {
      # tcp udt unix-domain-socket
      type = "TCP"
      #NIO NATIVE
      server = "NIO"
      #enable heartbeat
      heartbeat = true
      #thread factory for netty
      thread-factory {
        boss-thread-prefix = "NettyBoss"
        worker-thread-prefix = "NettyServerNIOWorker"
        server-executor-thread-prefix = "NettyServerBizHandler"
        share-boss-worker = false
        client-selector-thread-prefix = "NettyClientSelector"
        client-selector-thread-size = 1
        client-worker-thread-prefix = "NettyClientWorkerThread"
        # netty boss thread size,will not be used for UDT
        boss-thread-size = 1
        #auto default pin or 8
        worker-thread-size = 8
      }
      shutdown {
        # when destroy server, wait seconds
        wait = 3
      }
      serialization = "seata"
      compressor = "none"
    }
    service {
      #vgroup->rgroup
      vgroup_mapping.gulimall-order-fescar-service-group = "default"
      #only support single node
      default.grouplist = "127.0.0.1:8091"
      #degrade current not support
      enableDegrade = false
      #disable
      disable = false
      #unit ms,s,m,h,d represents milliseconds, seconds, minutes, hours, days, default permanent
      max.commit.retry.timeout = "-1"
      max.rollback.retry.timeout = "-1"
    }
     
    client {
      async.commit.buffer.limit = 10000
      lock {
        retry.internal = 10
        retry.times = 30
      }
      report.retry.count = 5
    }
     
    ## transaction log store
    store {
      ## store mode: file、db
      mode = "file"
     
      ## file store
      file {
        dir = "sessionStore"
     
        # branch session size , if exceeded first try compress lockkey, still exceeded throws exceptions
        max-branch-session-size = 16384
        # globe session size , if exceeded throws exceptions
        max-global-session-size = 512
        # file buffer size , if exceeded allocate new buffer
        file-write-buffer-cache-size = 16384
        # when recover batch read size
        session.reload.read_size = 100
        # async, sync
        flush-disk-mode = async
      }
     
      ## database store
      db {
        ## the implement of javax.sql.DataSource, such as DruidDataSource(druid)/BasicDataSource(dbcp) etc.
        datasource = "dbcp"
        ## mysql/oracle/h2/oceanbase etc.
        db-type = "mysql"
        url = "jdbc:mysql://127.0.0.1:3306/seata"
        user = "mysql"
        passw改成自己密码ord = "mysql"
        min-conn = 1
        max-conn = 3
        global.table = "global_table"
        branch.table = "branch_table"
        lock-table = "lock_table"
        query-limit = 100
      }
    }
    lock {
      ## the lock store mode: local、remote
      mode = "remote"
     
      local {
        ## store locks in user's database
      }
     
      remote {
        ## store locks in the seata's server
      }
    }
    recovery {
      committing-retry-delay = 30
      asyn-committing-retry-delay = 30
      rollbacking-retry-delay = 30
      timeout-retry-delay = 30
    }
     
    transaction {
      undo.data.validation = true
      undo.log.serialization = "jackson"
    }
     
    ## metrics settings
    metrics {
      enabled = false
      registry-type = "compact"
      # multi exporters use comma divided
      exporter-list = "prometheus"
      exporter-prometheus-port = 9898
    }
    ```
    
*   报错几次后，每次检查数据库，账户金额别小于 200，库存数量别少于 2. 因为这里微服务还没有实现分布式事务问题，下单、扣钱、减库存，哪一环节出错其他环节的数据都不会回滚。

 **测试正确操作：**

```
SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;
 
-- ----------------------------
-- 分支事务表
-- ----------------------------
DROP TABLE IF EXISTS `branch_table`;
CREATE TABLE `branch_table`  (
  `branch_id` bigint(20) NOT NULL,
  `xid` varchar(128) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  `transaction_id` bigint(20) NULL DEFAULT NULL,
  `resource_group_id` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `resource_id` varchar(256) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `branch_type` varchar(8) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `status` tinyint(4) NULL DEFAULT NULL,
  `client_id` varchar(64) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `application_data` varchar(2000) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `gmt_create` datetime(6) NULL DEFAULT NULL,
  `gmt_modified` datetime(6) NULL DEFAULT NULL,
  PRIMARY KEY (`branch_id`) USING BTREE,
  INDEX `idx_xid`(`xid`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Compact;
 
-- ----------------------------
-- 全局事务表
-- ----------------------------
DROP TABLE IF EXISTS `global_table`;
CREATE TABLE `global_table`  (
  `xid` varchar(128) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  `transaction_id` bigint(20) NULL DEFAULT NULL,
  `status` tinyint(4) NOT NULL,
  `application_id` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `transaction_service_group` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `transaction_name` varchar(128) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `timeout` int(11) NULL DEFAULT NULL,
  `begin_time` bigint(20) NULL DEFAULT NULL,
  `application_data` varchar(2000) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `gmt_create` datetime NULL DEFAULT NULL,
  `gmt_modified` datetime NULL DEFAULT NULL,
  PRIMARY KEY (`xid`) USING BTREE,
  INDEX `idx_gmt_modified_status`(`gmt_modified`, `status`) USING BTREE,
  INDEX `idx_transaction_id`(`transaction_id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Compact;
 
SET FOREIGN_KEY_CHECKS = 1;
```

发现订单新增成功、钱减少了 200，数量少了 2。

![](<assets/1740016785687.png>)

**测试报错操作、分布式事务不会滚问题**（减库存后库存是负数，报错、回滚）： 

```
<!--seata-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
    <exclusions>
        <!--版本较低，1.3.0，因此排除--> 
        <exclusion>
            <artifactId>seata-spring-boot-starter</artifactId>
            <groupId>io.seata</groupId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>io.seata</groupId>
    <artifactId>seata-spring-boot-starter</artifactId>
    <!--seata starter 采用1.4.2版本-->
    <version>${seata.version}</version>
</dependency>
```

如图：

![](<assets/1740016785877.png>)

 测试发现，当库存不足时，如果余额已经扣减，**并不会回滚，出现了分布式事务问题。**

![](<assets/1740016786081.png>)

## 2. 理论基础

解决分布式事务问题，需要一些分布式系统的基础知识作为理论指导。

### 2.1.CAP 定理

1998 年，加州大学的计算机科学家 Eric Brewer 提出，分布式系统有三个指标，**三个指标不可能同时做到**。

*   **Consistency（一致性）**
*   **Availability（可用性）**
*   **Partition tolerance （分区容错性）**

![](<assets/1740016786351.png>)

它们的第一个字母分别是 C、A、P。

Eric Brewer 说，这**三个指标不可能同时做到，最多只能同时满足两个。这个结论就叫做 CAP 定理。**

#### 2.1.1. 一致性 C：数据同步

**Consistency（一致性）：**用户访问分布式系统中的任意节点，得到的数据必须一致。

比如现在包含两个节点，其中的初始数据是一致的：

![](<assets/1740016786605.png>)

当我们修改其中一个节点的数据时，两者的数据产生了差异：

![](<assets/1740016786820.png>)

要想保住一致性，就必须实现 node01 到 node02 的数据 同步：

![](<assets/1740016787062.png>)

#### 2.1.2. 可用性 A：节点正常访问

**Availability （可用性）：**用户访问集群中的任意**健康节点**，**必须能得到响应**，**而不是超时或拒绝**。

如图，有三个节点的集群，访问任何一个都可以及时得到响应：

![](<assets/1740016787354.png>)

当有部分节点因为**网络故障或其它原因无法访问时，代表节点不可用：**

![](<assets/1740016787631.png>)

#### 2.1.3. 分区容错 P：分区后也要对外提供服务

**Partition（分区）**：因为**网络故障或其它原因**导致分布式系统中的**部分节点**与其它节点**失去连接**，**形成独立分区。**

![](<assets/1740016787955.png>)

**Tolerance（容错）**：在集群出现分区时，整个系统也要持续对外提供服务

#### 2.1.4. 矛盾

在分布式系统中，系统间的**网络不能 100% 保证健康**，一定会有故障的时候，而服务有必须对外保证服务。因此 **Partition Tolerance 不可避免。**

当节点接收到新的数据变更时，就会出现问题了：

![](<assets/1740016788248.png>)

如果此时要保证**一致性**，就必须等待网络恢复，完成数据同步后，整个集群才对外提供服务，服务处于阻塞状态，不可用。

如果此时要保证**可用性**，就不能等待网络恢复，那 node01、node02 与 node03 之间就会出现数据不一致。

也就是说，**在 P 一定会出现的情况下，A 和 C 之间只能实现一个。**要么让 node1、node2 停掉，使三个节点保持同步，要么让 node1、node2 组成新分区，对外提供服务。

### 2.2.BASE 理论

BASE 理论是对 CAP 的**一种解决思路**，包含三个思想：

*   **Basically Available** **（基本可用）**：分布式系统在出现**故障**时，允许损失部分可用性，即**保证核心可用。**
*   **Soft State（软状态）：**在一定时间内，允许出现中间状态，比如**临时的不一致状态**。
*   **Eventually Consistent（最终一致性）**：虽然无法保证强一致性，但是在**软状态结束后**，**最终达到数据一致。**

### 2.3. 解决分布式事务的思路，最终一致和强一致

分布式事务最大的问题是各个子事务的一致性问题，因此可以借鉴 CAP 定理和 BASE 理论，有两种解决思路：

*   **AP 模式（最终一致）：**各子事务分别执行和提交，允许出现结果不一致，然后采用弥补措施恢复数据即可，实现**最终一致**。
    
*   **CP 模式（强一致）：**各个子事务执行后互相等待，**同时提交，同时回滚**，达成**强一致**。但事务**等待过程**中，处于**弱可用状态**。
    

但不管是哪一种模式，都需要在子系统事务之间互相通讯，协调事务状态，也就是需要一个**事务协调者 (TC)**：

![](<assets/1740016788531.png>)

这里的子系统事务，称为**分支事务**；有关联的各个分支事务在一起称为**全局事务**。

## 3. 初识 Seata

Seata 是 2019 年 1 月份蚂蚁金服和阿里巴巴共同开源的分布式事务解决方案。致力于提供高性能和简单易用的分布式事务服务，为用户打造一站式的分布式解决方案。

**官网地址：**http://seata.io/，其中的文档、播客中提供了大量的使用说明、源码分析。

![](<assets/1740016788734.png>)

### 3.1.Seata 的架构，TC,**TM,RM**

Seata 事务管理中有三个重要的角色：

*   **TC (Transaction Coordinator) -** **事务协调者：**维护全局和分支事务的状态，**协调全局事务提交或回滚**。
    
*   **TM (Transaction Manager) -** **事务管理器：定义**全局事务的**范围**、**开始**全局事务、**提交或回滚**全局事务。
    
*   **RM (Resource Manager) -** **资源管理器：**管理分支事务处理的资源，与 TC 交谈以注册分支事务和报告分支事务的状态，并驱动分支事务提交或回滚。
    

**整体的架构如图：**

![](<assets/1740016788947.png>)

Seata 基于上述架构提供了四种不同的**分布式事务解决方案：**

*   **XA 模式：**强一致性分阶段事务模式，牺牲了一定的可用性，无业务侵入
*   **TCC 模式：**最终一致的分阶段事务模式，有业务侵入
*   **AT 模式（默认）：**最终一致的分阶段事务模式，无业务侵入，也是 Seata 的默认模式
*   **SAGA 模式：**长事务模式，有业务侵入

无论哪种方案，都离不开 TC，也就是事务的协调者。

### 3.2. 部署 Seata 的 TC（事务协调者）服务，tc-server

#### 1. 下载

首先我们要下载 seata-server 包，地址在 [http](http://seata.io/zh-cn/blog/download.html "http")[😕/seata.io/zh-cn/blog/download](http://seata.io/zh-cn/blog/download.html "😕/seata.io/zh-cn/blog/download")[.](http://seata.io/zh-cn/blog/download.html ".")[html](http://seata.io/zh-cn/blog/download.html "html")

当然，课前资料也准备好了：

![](<assets/1740016789331.png>)

#### 2. 解压

在非中文目录解压缩这个 zip 包，其目录结构如下：

![](<assets/1740016789607.png>)

#### 3.registry.conf 修改 Seata 的注册中心、配置中心

修改 conf 目录下的 registry.conf 文件：

![](<assets/1740016789893.png>)

内容如下：

```
seata:
 registry: # TC服务注册中心的配置，微服务根据这些信息去注册中心获取tc服务地址
 type: nacos # 注册中心类型 nacos
 nacos:
 server-addr: 127.0.0.1:8848 # nacos地址
 namespace: "" # namespace，默认为空
 group: DEFAULT_GROUP # 分组，默认是DEFAULT_GROUP
 application: seata-tc-server # seata服务名称
 username: nacos
 password: nacos
 tx-service-group: seata-demo # 事务组名称
 service:
 vgroup-mapping: # 事务组与cluster的映射关系
 seata-demo: SH
```

#### 4. 在 nacos 添加配置

特别注意，为了让 tc 服务的集群可以共享配置，我们选择了 nacos 作为统一配置中心。因此服务端配置文件 seataServer.properties 文件需要在 nacos 中配好。

格式如下：

![](<assets/1740016790138.png>)

配置内容如下（要修改数据库信息）：

```
seata:
 data-source-proxy-mode: XA    #开启数据源代理的XA模式。代理数据源，拦截业务sql操作
```

其中的数据库地址、用户名、密码都需要修改成你自己的数据库信息。

也可以在文件中配置，需要 registry.conf 设置配置方式为 file：

![](<assets/1740016790350.png>)

 file.conf

```
{
    "id": 1, "money": 100
}
```

#### 5. 创建数据库表，分支事务表和全局事务表

特别注意：tc 服务在管理分布式事务时，需要记录事务相关数据到数据库中，你需要提前创建好这些表。

新建一个名为 seata 的数据库，运行课前资料提供的 sql 文件：

![](<assets/1740016790589.png>)

这些表主要记录全局事务、分支事务、全局锁信息：

```
<!--seata-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
</dependency>
```

#### 6. 启动 TC 服务

进入 bin 目录，运行其中的 seata-server.bat 即可：

![](<assets/1740016790802.png>)

启动成功后，seata-server 应该已经注册到 nacos 注册中心了。

打开浏览器，访问 nacos 地址：http://localhost:8848，然后进入服务列表页面，可以看到 seata-tc-server 的信息：

![](<assets/1740016790984.png>)

### 3.3. 微服务集成 **Seata**

我们以 order-service 为例来演示。

#### 3.3.1. 引入依赖

首先，在 order-service 中引入依赖：

```
-- 注意此处0.7.0+ 增加字段 context
CREATE TABLE `undo_log` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `branch_id` bigint(20) NOT NULL,
  `xid` varchar(100) NOT NULL,
  `context` varchar(128) NOT NULL,
  `rollback_info` longblob NOT NULL,
  `log_status` int(11) NOT NULL,
  `log_created` datetime NOT NULL,
  `log_modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `ux_undo_log` (`xid`,`branch_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

#### 3.3.2. 配置 TC 地址

在 order-service 中的 application.yml 中，配置 TC 服务信息，通过注册中心 nacos，结合服务名称获取 TC 地址：

```
seata:
 data-source-proxy-mode: AT # 默认就是AT
```

微服务如何根据这些配置寻找 TC 的地址呢？

我们知道注册到 Nacos 中的微服务，确定一个具体实例需要四个信息：

*   namespace：命名空间
*   group：分组
*   application：服务名
*   cluster：集群名

以上四个信息，在刚才的 yaml 文件中都能找到：

![](<assets/1740016791160.png>)

namespace 为空，就是默认的 public

结合起来，TC 服务的信息就是：public@DEFAULT_GROUP@seata-tc-server@SH，这样就能确定 TC 服务集群了。然后就可以去 Nacos 拉取对应的实例信息了。

#### 3.3.3. 其它服务

其它两个微服务也都参考 order-service 的步骤来做，完全一样。

## 4. 详解 Seata 的四种分布式事务方案

下面我们就一起学习下 Seata 中的四种不同的事务模式。

### 4.1.XA 模式，两阶段都加锁

XA 规范 是 X/Open 组织定义的分布式事务处理（DTP，Distributed Transaction Processing）标准，XA 规范 描述了全局的 TM 与局部的 RM 之间的接口，**几乎所有主流的数据库都对 XA 规范 提供了支持。**

#### 4.1.1. 两阶段提交

XA 是规范，目前主流数据库都实现了这种规范，实现的原理都是基于两阶段提交。

**正常情况：**

![](<assets/1740016791531.png>)

**异常情况：**

![](<assets/1740016791808.png>)

**一阶段：**

*   **事务协调者通知每个事务参与者执行本地事务**
*   本地事务执行完成后**报告事务执行状态给事务协调者**，此时事务不提交，继续持有数据库锁

**二阶段：**

*   **事务协调者基于一阶段的报告来判断下一步操作**
    *   如果一阶段都成功，则通知所有事务参与者，提交事务
    *   如果一阶段**任意一个参与者失败**，则**通知所有事务参与者回滚事务**

#### 4.1.2.Seata 的 XA 模型

Seata 对原始的 XA 模式做了简单的封装和改造，以适应自己的事务模型，基本架构如图：

![](<assets/1740016792181.png>)

**资源管理者 RM 一阶段的工作：**

​ ① 注册分支事务到 TC

​ ② 执行分支业务 sql 但不提交

​ ③ 报告执行状态到 TC

**业务协调者 TC 二阶段的工作：**

*   TC 检测各分支事务执行状态
    
    a. 如果都成功，通知所有 RM 提交事务
    
    b. 如果有失败，**通知所有 RM 回滚事务**
    

**RM 二阶段的工作：**

*   接收 TC 指令，提交或回滚事务

#### 4.1.3. 优缺点

**XA 模式的优点是什么？**

*   事务的**强一致性**，满足 ACID 原则。
*   **常用数据库都支持**，实现简单，并且**没有代码侵入**

**XA 模式的缺点是什么？**

*   因为**一阶段需要锁定数据库资源**，等待二阶段结束才释放，**性能较差**
*   **依赖关系型数据库**实现事务

#### 4.1.4. 实现 XA 模式

Seata 的 starter 已经完成了 XA 模式的自动装配，实现非常简单，步骤如下：

**1）修改 application.yml 文件（每个参与事务的微服务），开启 XA 模式：**

```
@GlobalTransactional
@Transactional  // 本地事务，在分布式系统，只能控制住自己的回滚，控制不了其他服务的回滚。
@Override
public SubmitOrderResponseVo submitOrder(OrderSubmitVo vo) {
  //.....
}
```

**2）给发起全局事务的入口方法添加 @GlobalTransactional 注解:**

本例中是 OrderServiceImpl 中的 create 方法，把 @Transactional 改成 **@GlobalTransactional**

![](<assets/1740016792473.png>)

**3）重启服务并测试**

重启 order-service，再次测试，发现无论怎样出错（例如操作完成后钱或库存是负数），**三个微服务都能成功回滚。**

### 4.2.AT 模式：一阶段直接提交，二阶段统一回滚

AT 模式同样是分阶段提交的事务模型，不过**弥补了 XA 模型中资源锁定周期过长的缺陷**。

#### 4.2.1.Seata 的 AT 模型

基本流程图：

![](<assets/1740016792782.png>)

**阶段一 RM 的工作：**

*   注册分支事务
*   记录 undo-log（数据快照）
*   执行业务 sql 并提交
*   报告事务状态

**阶段二提交时 RM 的工作：**

*   **删除 undo-log 即可**

**阶段二回滚时 RM 的工作：**

*   **根据 undo-log 恢复数据到更新前**

#### 4.2.2. 流程梳理

我们用一个真实的业务来梳理下 AT 模式的原理。

比如，现在又一个数据库表，记录用户余额：

<table><thead><tr><th><strong>id</strong></th><th><strong>money</strong></th></tr></thead><tbody><tr><td>1</td><td>100</td></tr></tbody></table>

其中一个分支业务要执行的 SQL 为：

```
@Configuration
public class MySeataConfig {
 
    @Autowired
    DataSourceProperties dataSourceProperties;
 
 
    @Bean
    public DataSource dataSource(DataSourceProperties dataSourceProperties) {
 
        HikariDataSource dataSource = dataSourceProperties.initializeDataSourceBuilder().type(HikariDataSource.class).build();
        if (StringUtils.hasText(dataSourceProperties.getName())) {
            dataSource.setPoolName(dataSourceProperties.getName());
        }
 
        return new DataSourceProxy(dataSource);
    }
 
}
```

AT 模式下，当前分支事务执行流程如下：

**一阶段：**

1）TM 发起并注册全局事务到 TC

2）TM 调用分支事务

3）分支事务准备执行业务 SQL

4）RM 拦截业务 SQL，根据 where 条件查询原始数据，形成**快照。**

```
@Configuration
public class DataSourceProxyConfig {
 
    @Bean
    @ConfigurationProperties(prefix = "spring.datasource")
    public DruidDataSource druidDataSource() {
        return new DruidDataSource();
    }
 
    @Primary
    @Bean
    public DataSourceProxy dataSource(DruidDataSource druidDataSource) {
        return new DataSourceProxy(druidDataSource);
    }
 
}
```

5）RM 执行业务 SQL，提交本地事务，释放数据库锁。此时 `money = 90`

6）RM 报告本地事务状态给 TC

**二阶段：**

1）TM 通知 TC 事务结束

2）TC 检查分支事务状态

​ a）如果都成功，则立即删除快照

​ b）如果有分支事务失败，需要回滚。读取快照数据（`{"id": 1, "money": 100}`），将快照恢复到数据库。此时数据库再次恢复为 100

**流程图：**

![](<assets/1740016792977.png>)

#### 4.2.3.AT 与 XA 的区别

简述 AT 模式与 XA 模式最大的区别是什么？

*   XA 模式一阶段不提交事务，锁定资源；**AT 模式一阶段直接提交，不锁定资源。**
*   **XA 模式依赖数据库机制实现回滚**；**AT 模式利用数据快照实现数据回滚。**
*   **XA** 模式**强一致**；**AT** 模式**最终一致**

#### 4.2.4. 脏写问题，全局锁

**快照恢复数据时数据库被更新。**

在多线程并发访问 AT 模式的分布式事务时，有**可能出现脏写问题，归根结底是业务之间的隔离问题**，导致 a 业务根据快照恢复数据时，另一个 a 业务已提交业务成功，导致恢复的数据是未更新时的样子，如图：

![](<assets/1740016793213.png>)

**解决思路就是引入了全局锁**的概念。在释放 DB 锁之前，先拿到全局锁。避免同一时刻有另外一个事务来操作当前数据。

![](<assets/1740016793507.png>)

#### 4.2.5. 优缺点

**AT 模式的优点：**

*   **一阶段**完成**直接提交事务**，释放数据库资源，**性能比较好**
*   利用**全局锁**实现**读写隔离**
*   没有代码侵入，框架自动完成回滚和提交

**AT 模式的缺点：**

*   两阶段之间属于**软状态**，属于**最终一致**
*   框架的快照功能会影响**性能**，但**比 XA 模式要好很多**

#### 4.2.6. 实现 AT 模式（完整流程）

AT 模式中的快照生成、回滚等动作都是由框架自动完成，没有任何代码侵入，因此实现非常简单。

只不过，AT 模式需要一个表来记录全局锁、另一张表来记录数据快照 undo_log。

**0）环境准备**

安装启动事务协调器 seata-server，registry.conf 设置注册中心（nacos）和配置中心（file）、各服务导入依赖

```
CREATE TABLE `account_freeze_tbl` (
  `xid` varchar(128) NOT NULL,
  `user_id` varchar(255) DEFAULT NULL COMMENT '用户id',
  `freeze_money` int(11) unsigned DEFAULT '0' COMMENT '冻结金额',
  `state` int(1) DEFAULT NULL COMMENT '事务状态，0:try，1:confirm，2:cancel',
  PRIMARY KEY (`xid`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8 ROW_FORMAT=COMPACT;
```

**1）导入数据库表，记录全局锁**

每个需要用的 seata 的数据库导入 undo_log 表，undo_log 表是回滚日志表：

```
CREATE TABLE `account_freeze_tbl` (
  `xid` varchar(128) NOT NULL,
  `user_id` varchar(255) DEFAULT NULL COMMENT '用户id',
  `freeze_money` int(11) unsigned DEFAULT '0' COMMENT '冻结金额',
  `state` int(1) DEFAULT NULL COMMENT '事务状态，0:try，1:confirm，2:cancel',
  PRIMARY KEY (`xid`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8 ROW_FORMAT=COMPACT;
```

**2）修改 application.yml 文件，将事务模式修改为 AT 模式即可：**

```
package cn.itcast.account.service;
 
import io.seata.rm.tcc.api.BusinessActionContext;
import io.seata.rm.tcc.api.BusinessActionContextParameter;
import io.seata.rm.tcc.api.LocalTCC;
import io.seata.rm.tcc.api.TwoPhaseBusinessAction;
 
//声明为ttc
@LocalTCC
public interface AccountTCCService {
 
    //在try方法上@TwoPhaseBusinessAction声明try、confirm、cancel的方法名
    @TwoPhaseBusinessAction(name = "deduct", commitMethod = "confirm", rollbackMethod = "cancel")
    //参数注解@BusinessActionContextParameter将参数放在上下文对象，在本接口所有方法都能拿到这个参数
    void deduct(@BusinessActionContextParameter(paramName = "userId") String userId,
                @BusinessActionContextParameter(paramName = "money")int money);
 
    //confirm方法名要跟@TwoPhaseBusinessAction声明的一致，BusinessActionContext上下文对象获取try方法注解@BusinessActionContextParameter的参数
    boolean confirm(BusinessActionContext ctx);
 
    boolean cancel(BusinessActionContext ctx);
}
```

**3）给发起全局事务的入口方法添加 @GlobalTransactional 注解:**

给分布式大事务注解 @GlobalTransactional; 每一个远程的小事务用 @Transactional

```
package cn.itcast.account.service.impl;
 
import cn.itcast.account.entity.AccountFreeze;
import cn.itcast.account.mapper.AccountFreezeMapper;
import cn.itcast.account.mapper.AccountMapper;
import cn.itcast.account.service.AccountTCCService;
import io.seata.core.context.RootContext;
import io.seata.rm.tcc.api.BusinessActionContext;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
 
@Service
@Slf4j
public class AccountTCCServiceImpl implements AccountTCCService {
 
    @Autowired
    private AccountMapper accountMapper;
    @Autowired
    private AccountFreezeMapper freezeMapper;
 
    @Override
    //业务注解
    @Transactional
    public void deduct(String userId, int money) {
        // 0.获取事务id，RootContext是seata提供的工具类，用来获取id。
        String xid = RootContext.getXID();
        
        //判断事务悬挂，如果有冻结记录则是悬挂，直接return。对于已经空回滚的业务，之前被阻塞的try操作恢复，继续执行try，就永远不可能confirm或cancel ，事务一直处于中间状态，这就是业务悬挂。
        if(accountMapper.selectById(xid)==null) return;
        // 1.扣减可用余额，这里不用余额判断，因为数据库余额字段类型是非负，扣成负数就业务失败
        accountMapper.deduct(userId, money);
        // 2.记录冻结金额，事务状态
        AccountFreeze freeze = new AccountFreeze();
        freeze.setUserId(userId);
        freeze.setFreezeMoney(money);
        //自定义常量AccountFreeze.State.TRY值就是0
        freeze.setState(AccountFreeze.State.TRY);
        freeze.setXid(xid);
        freezeMapper.insert(freeze);
    }
 
    @Override
    public boolean confirm(BusinessActionContext ctx) {
        // 1.获取事务id，也可以用RootContext获取xid
        String xid = ctx.getXid();
        // 2.根据id删除冻结记录，confirm方法不用幂等处理，因为它是直接删除，删多少次都是删
        int count = freezeMapper.deleteById(xid);
        return count == 1;
    }
 
    @Override
    public boolean cancel(BusinessActionContext ctx) {
        // 0.查询冻结记录
        String xid = ctx.getXid();
        AccountFreeze freeze = freezeMapper.selectById(xid);
        
        //判断空回滚。当某分支事务的try阶段阻塞时，可能导致全局事务超时而触发二阶段的cancel操作。在未执行try操作时先执行了cancel操作，这时cancel不能做回滚，就是空回滚。
        if(freeze==null){freeze =new freeze;设置userId,state,xid;freezeMapper.insert(freeze);return true}
 
        //幂等判断，如果已经处理过一次cancel时不需要再cancel了（可能源于超时被认为失败其实没失败，再cancel了一次）。confirm方法不用幂等处理，因为它是直接删除，删多少次都是删
        if(freeze.getState()==AccountFreeze.State.CANCEL){return true;}
        // 1.恢复可用余额
        accountMapper.refund(freeze.getUserId(), freeze.getFreezeMoney());
        // 2.将冻结金额清零，状态改为CANCEL
        freeze.setFreezeMoney(0);
        freeze.setState(AccountFreeze.State.CANCEL);
        int count = freezeMapper.updateById(freeze);
        return count == 1;
    }
}
```

**4） 配置类注入 seata 代理数据源**

Seata 通过代理数据源实现分支事务，如果没有注入，会导致无法回滚问题。

如果使用的是 Hikari 数据源配置类： 

```
registry {
  ## tc服务的注册中心类，这里选择nacos，也可以是eureka、zookeeper等
  type = "nacos"
 
  nacos {
    ## seata tc 服务注册到 nacos的服务名称，可以自定义
    application = "seata-tc-server"
    serverAddr = "127.0.0.1:8848"
    group = "DEFAULT_GROUP"
    namespace = ""
    cluster = "HZ"
    username = "nacos"
    pass改成自己的密码word = "nacos"
  }
}
 
config {
  ## 读取tc服务端的配置文件的方式，这里是从nacos配置中心读取，这样如果tc是集群，可以共享配置
  type = "nacos"
  ## 配置nacos地址等信息
  nacos {
    serverAddr = "127.0.0.1:8848"
    namespace = ""
    group = "SEATA_GROUP"
    username = "nacos"
    pass改成自己的密码word = "nacos"
    dataId = "seataServer.properties"
  }
}
```

Druid 数据源

```
## 事务组映射关系
service.vgroupMapping.seata-demo=SH
 
service.enableDegrade=false
service.disableGlobalTransaction=false
## 与TC服务的通信配置
transport.type=TCP
transport.server=NIO
transport.heartbeat=true
transport.enableClientBatchSendRequest=false
transport.threadFactory.bossThreadPrefix=NettyBoss
transport.threadFactory.workerThreadPrefix=NettyServerNIOWorker
transport.threadFactory.serverExecutorThreadPrefix=NettyServerBizHandler
transport.threadFactory.shareBossWorker=false
transport.threadFactory.clientSelectorThreadPrefix=NettyClientSelector
transport.threadFactory.clientSelectorThreadSize=1
transport.threadFactory.clientWorkerThreadPrefix=NettyClientWorkerThread
transport.threadFactory.bossThreadSize=1
transport.threadFactory.workerThreadSize=default
transport.shutdown.wait=3
## RM配置
client.rm.asyncCommitBufferLimit=10000
client.rm.lock.retryInterval=10
client.rm.lock.retryTimes=30
client.rm.lock.retryPolicyBranchRollbackOnConflict=true
client.rm.reportRetryCount=5
client.rm.tableMetaCheckEnable=false
client.rm.tableMetaCheckerInterval=60000
client.rm.sqlParserType=druid
client.rm.reportSuccessEnable=false
client.rm.sagaBranchRegisterEnable=false
## TM配置
client.tm.commitRetryCount=5
client.tm.rollbackRetryCount=5
client.tm.defaultGlobalTransactionTimeout=60000
client.tm.degradeCheck=false
client.tm.degradeCheckAllowTimes=10
client.tm.degradeCheckPeriod=2000
 
## undo日志配置
client.undo.dataValidation=true
client.undo.logSerialization=jackson
client.undo.onlyCareUpdateColumns=true
client.undo.logTable=undo_log
client.undo.compress.enable=true
client.undo.compress.type=zip
client.undo.compress.threshold=64k
client.log.exceptionRate=100
```

**5）将** [file.conf](https://github.com/seata/seata-samples/blob/master/springcloud-jpa-seata/account-service/src/main/resources/file.conf "file.conf") **和** [registry.conf](https://github.com/seata/seata-samples/blob/master/springcloud-jpa-seata/account-service/src/main/resources/registry.conf "registry.conf") **两个配置文件移动到项目 resources 下：**

前面事务协调器 seata-server 的配置文件 registry.conf 设置了配置中心方式为 file。

地址：

[https://github.com/seata/seata-samples/tree/master/springcloud-jpa-seata/account-service/src/main/resources](https://github.com/seata/seata-samples/tree/master/springcloud-jpa-seata/account-service/src/main/resources "https://github.com/seata/seata-samples/tree/master/springcloud-jpa-seata/account-service/src/main/resources")

file.conf

registry.conf 

**注意：**file.conf 的 service.vgroup_mapping 配置必须和`spring.application.name`一致

在 `org.springframework.cloud:spring-cloud-starter-alibaba-seata` 的`org.springframework.cloud.alibaba.seata.GlobalTransactionAutoConfiguration` 类中，默认会使用 `${spring.application.name}-fescar-service-group`作为服务名注册到 Seata Server 上，如果和`file.conf` 中的配置不一致，会提示 `no available server to connect`错误

也可以通过配置 `spring.cloud.alibaba.seata.tx-service-group`修改后缀，但是必须和`file.conf`中的配置保持一致

![](<assets/1740016793866.png>)

或者 yml 配置 service.vgroup_mapping 的服务名：

![](<assets/1740016794084.png>)

**7）启动并测试**

#### 4.2.7.@Transactional 和 @GlobalTransactional 对比

**结论：**给分布式大事务注解 @GlobalTransactional; 每一个远程的小事务用 @Transactional

        ①、只用 @Transactional

                开启本地事务，对本地事务进行支持。如果用了 @Transactional 则保证 a、b、c 操作都在同一个本地事务中执行，并且更新时会加行锁，如果本地事务不统一提交，其他 SQL 不能再更新此条数据。如果不加 @Transactional 则默认没有事务 a、b、c 操作分别执行，不会加行锁，其他 SQL 都可以随便更新。

            ②、只用 @GlobalTransactional

                开启全局事务，保证分布式事务。如果只用 @GlobalTransactional，那就保证分布式事务，各个分支事务（本地事务）的统一，但是不能保证各个分支事务（本地事务）操作的统一。各个本地操作在无事务的状态下执行操作，不会加锁，别的操作可以随意修改。

            ③、@GlobalTransactional 和 @Transactional 一起用

                @Transactional 保证本地事务一致性，@GlobalTransactional 保证全局事务的一致性。  
 

### 4.3.TCC 模式：基于资源预留

XA 和 AT 模式都要加锁，性能低。 而 TCC 模式**不需要加锁**，因为每次业务会预留资源，例如冻结应付金额方法。**不加锁性能高。**

**TCC 模式**与 AT 模式非常相似，**每阶段都是独立事务**，不同的是 TCC 通过**人工编码来实现数据恢复。**

需要**实现三个方法：**

*   **Try：阶段 1，资源的检测和预留（例如冻结应付金额）**；
    
*   **Confirm：提交，完成资源操作业务**；要求 Try 成功 Confirm 一定要能成功。
    
*   **Cancel**：**回滚，预留资源释放**，可以理解为 try 的反向操作。
    

#### 4.3.1. 流程分析

**举例，**一个扣减用户余额的业务。假设账户 A 原来余额是 100，需要余额扣减 30 元。

*   **阶段一（ Try ）**：**检查余额是否充足**，如果充足则**冻结金额从 0 变 30，可用余额从 100 到 70**

初识余额：

![](<assets/1740016794287.png>)

余额充足，可以冻结：

![](<assets/1740016794489.png>)

此时，总金额 = 冻结金额 + 可用金额，数量依然是 100 不变。事务直接提交无需等待其它事务。

*   **阶段二（Confirm)**：假如要**提交**（Confirm），则**冻结金额扣减 30**

确认可以提交，不过之前可用金额已经扣减过了，这里只要清除冻结金额就好了：

![](<assets/1740016794696.png>)

此时，总金额 = 冻结金额 + 可用金额 = 0 + 70 = 70 元

*   **阶段二 (Canncel)**：如果要**回滚**（Cancel），则**冻结金额扣减 30，可用余额增加 30**

需要回滚，那么就要释放冻结金额，恢复可用金额：

![](<assets/1740016794957.png>)

#### 4.3.2.Seata 的 TCC 模型

Seata 中的 TCC 模型依然延续之前的事务架构，如图：

![](<assets/1740016795168.png>)

#### 4.3.3. 优缺点

**TCC 模式的每个阶段是做什么的？**

*   Try：资源检查和预留
*   Confirm：业务执行和提交
*   Cancel：预留资源的释放

**TCC 的优点是什么？**

*   **一阶段完成直接提交事务，释放数据库资源**，性能好
*   相比 AT 模型，无需生成快照，**无需使用全局锁，性能最强**
*   不依赖数据库事务，而是依赖补偿操作，**可以用于非事务型数据库**

**TCC 的缺点是什么？**

*   有**代码侵入**，需要**人为编写** try、Confirm 和 Cancel 接口，太麻烦
*   软状态，事务是最终一致
*   **做好幂等处理：**需要考虑 Confirm 和 Cancel 的失败情况，做好幂等处理（例如 cancel 超时，系统以为 cancel 失败就又调用了一次 cancel，而其实第一次 cancel 是成功的。多次 cancel 结果是一样的，就是幂等了。）

**代码侵入：**

**当你的代码引入了一个组件, 导致其它代码或者设计, 要做相应的更改以适应新组件**. 这样的情况我们就认为这个新组件具有**侵入性**

同时, 这里又涉及到一个设计方面的概念, 就是**耦合性**的问题.

我们代码设计的思路是 " **高内聚, 低耦合** ", 为了实现这个思路, 就必须降低代码的侵入性.

#### 4.3.4. 事务悬挂和空回滚

**1）空回滚**

某分支事务因为阻塞导致 try 阶段没有执行，超时后 TC 通知全局事务回滚，而该分支因为 try 压根就没有执行，所以无需回滚。

当执行 cancel 操作时，应当判断 try 是否已经执行，如果尚未执行，则应该空回滚。

如图：

![](<assets/1740016795517.png>)

**2）业务悬挂**

对于已经空回滚的业务，之前被阻塞的 try 操作恢复，继续执行 try 冻结资源，然而二阶段早已执行完毕，此分支就永远停留在预留资源而不能提交或回滚的状态。

**解决办法：数据库记录事务状态。**Redis 或数据库记录事务状态，每次 try 前先从 Redis 或数据库判断是否已经 cancel 过，每次 cancel 后修改事务状态为 “已 cancel”。

**事务表：** 

```
seata:
 config:
 type: nacos
 nacos:
 server-addr: 127.0.0.1:8848
 username: nacos
 password: nacos
 group: SEATA_GROUP
 data-id: client.properties
```

这个表一方面冻结金额、一方面记录状态。

**判断空回滚：**cancel 时根据事务表的 id 查到状态 state 为 null，就空回滚；

**防止业务悬挂：**try 时查到状态 state 为 2 即回滚，就阻止业务悬挂。

#### 4.3.5. 下单代码实现 TCC 模式，@TwoPhaseBusinessAction，@BusinessActionContextParameter，

解决空回滚和业务悬挂问题，必须要记录当前事务状态，是在 try、还是 cancel？

**1）思路分析**

这里我们定义一张表：

```
CREATE TABLE `account_freeze_tbl` (
  `xid` varchar(128) NOT NULL,
  `user_id` varchar(255) DEFAULT NULL COMMENT '用户id',
  `freeze_money` int(11) unsigned DEFAULT '0' COMMENT '冻结金额',
  `state` int(1) DEFAULT NULL COMMENT '事务状态，0:try，1:confirm，2:cancel',
  PRIMARY KEY (`xid`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8 ROW_FORMAT=COMPACT;
```

**其中：**

*   **xid：**是全局事务 id
*   **freeze_money：**用来记录用户冻结金额
*   **state：**用来记录事务状态

**业务实现方法：**

*   Try 业务：
    *   记录冻结金额和事务状态到 account_freeze 表
    *   扣减 account 表可用金额
*   Confirm 业务
    *   根据 xid 删除 account_freeze 表的冻结记录
*   Cancel 业务
    *   修改 account_freeze 表，冻结金额为 0，state 为 2
    *   修改 account 表，恢复可用金额
*   **如何判断是否空回滚？**
    *   cancel 业务中，根据 xid 查询 account_freeze，如果为 null 则说明 try 还没做，需要空回滚
*   **如何避免业务悬挂？**
    *   try 业务中，根据 xid 查询 account_freeze ，如果已经存在则证明 Cancel 已经执行，拒绝执行 try 业务

接下来，我们改造 account-service，利用 TCC 实现余额扣减功能。

**2）声明 TCC 接口**

TCC 的 Try、Confirm、Cancel 方法都需要在接口中**基于注解来声明**，

我们在 account-service 项目中的`cn.itcast.account.service`包中新建一个接口，声明 TCC 三个接口：

```
package cn.itcast.account.service;
 
import io.seata.rm.tcc.api.BusinessActionContext;
import io.seata.rm.tcc.api.BusinessActionContextParameter;
import io.seata.rm.tcc.api.LocalTCC;
import io.seata.rm.tcc.api.TwoPhaseBusinessAction;
 
//声明为ttc
@LocalTCC
public interface AccountTCCService {
 
    //在try方法上@TwoPhaseBusinessAction声明try、confirm、cancel的方法名
    @TwoPhaseBusinessAction(name = "deduct", commitMethod = "confirm", rollbackMethod = "cancel")
    //参数注解@BusinessActionContextParameter将参数放在上下文对象，在本接口所有方法都能拿到这个参数
    void deduct(@BusinessActionContextParameter(paramName = "userId") String userId,
                @BusinessActionContextParameter(paramName = "money")int money);
 
    //confirm方法名要跟@TwoPhaseBusinessAction声明的一致，BusinessActionContext上下文对象获取try方法注解@BusinessActionContextParameter的参数
    boolean confirm(BusinessActionContext ctx);
 
    boolean cancel(BusinessActionContext ctx);
}
```

**3）编写实现类**

在 account-service 服务中的`cn.itcast.account.service.impl`包下新建一个类，实现 TCC 业务：

```
package cn.itcast.account.service.impl;
 
import cn.itcast.account.entity.AccountFreeze;
import cn.itcast.account.mapper.AccountFreezeMapper;
import cn.itcast.account.mapper.AccountMapper;
import cn.itcast.account.service.AccountTCCService;
import io.seata.core.context.RootContext;
import io.seata.rm.tcc.api.BusinessActionContext;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
 
@Service
@Slf4j
public class AccountTCCServiceImpl implements AccountTCCService {
 
    @Autowired
    private AccountMapper accountMapper;
    @Autowired
    private AccountFreezeMapper freezeMapper;
 
    @Override
    //业务注解
    @Transactional
    public void deduct(String userId, int money) {
        // 0.获取事务id，RootContext是seata提供的工具类，用来获取id。
        String xid = RootContext.getXID();
        
        //判断事务悬挂，如果有冻结记录则是悬挂，直接return。对于已经空回滚的业务，之前被阻塞的try操作恢复，继续执行try，就永远不可能confirm或cancel ，事务一直处于中间状态，这就是业务悬挂。
        if(accountMapper.selectById(xid)==null) return;
        // 1.扣减可用余额，这里不用余额判断，因为数据库余额字段类型是非负，扣成负数就业务失败
        accountMapper.deduct(userId, money);
        // 2.记录冻结金额，事务状态
        AccountFreeze freeze = new AccountFreeze();
        freeze.setUserId(userId);
        freeze.setFreezeMoney(money);
        //自定义常量AccountFreeze.State.TRY值就是0
        freeze.setState(AccountFreeze.State.TRY);
        freeze.setXid(xid);
        freezeMapper.insert(freeze);
    }
 
    @Override
    public boolean confirm(BusinessActionContext ctx) {
        // 1.获取事务id，也可以用RootContext获取xid
        String xid = ctx.getXid();
        // 2.根据id删除冻结记录，confirm方法不用幂等处理，因为它是直接删除，删多少次都是删
        int count = freezeMapper.deleteById(xid);
        return count == 1;
    }
 
    @Override
    public boolean cancel(BusinessActionContext ctx) {
        // 0.查询冻结记录
        String xid = ctx.getXid();
        AccountFreeze freeze = freezeMapper.selectById(xid);
        
        //判断空回滚。当某分支事务的try阶段阻塞时，可能导致全局事务超时而触发二阶段的cancel操作。在未执行try操作时先执行了cancel操作，这时cancel不能做回滚，就是空回滚。
        if(freeze==null){freeze =new freeze;设置userId,state,xid;freezeMapper.insert(freeze);return true}
 
        //幂等判断，如果已经处理过一次cancel时不需要再cancel了（可能源于超时被认为失败其实没失败，再cancel了一次）。confirm方法不用幂等处理，因为它是直接删除，删多少次都是删
        if(freeze.getState()==AccountFreeze.State.CANCEL){return true;}
        // 1.恢复可用余额
        accountMapper.refund(freeze.getUserId(), freeze.getFreezeMoney());
        // 2.将冻结金额清零，状态改为CANCEL
        freeze.setFreezeMoney(0);
        freeze.setState(AccountFreeze.State.CANCEL);
        int count = freezeMapper.updateById(freeze);
        return count == 1;
    }
}
```

**测试：**

先修改金额为不够和库存数量为充足、再次发请求：

```
http://localhost:8082/order?userId=user202103032042012&commodityCode=100202003032041&count=1&money=2000000
```

发现报错、回滚。 

### 4.4.SAGA 模式：基于一系列本地事务

Saga 模式是 Seata 即将开源的**长事务解决方案**，将由蚂蚁金服主要贡献。

其理论基础是 Hector & Kenneth 在 1987 年发表的论文 [Sagas](https://microservices.io/patterns/data/saga.html "Sagas")。

Seata 官网对于 Saga 的指南：https://seata.io/zh-cn/docs/user/saga.html

#### 4.4.1. 原理

Saga 模式使用**一系列本地事务**来提供事务管理。 本地事务是由 Saga 参与者执行的原子工作。 每个本地事务都会更新数据库，并发布消息或事件来**触发** Saga 中的下一个本地事务。 如果本地事务失败，则 Saga 将执行一系列补偿事务，以撤消上一个本地事务所做的更改。 

在 Saga 模式下，分布式事务内有多个参与者，每一个参与者都是一个冲正补偿服务，需要用户根据业务场景实现其正向操作和逆向回滚操作。

分布式事务执行过程中，依次执行各参与者的正向操作，如果所有正向操作均执行成功，那么分布式事务提交。如果任何一个正向操作执行失败，那么分布式事务会去退回去执行前面各参与者的逆向回滚操作，回滚已提交的参与者，使分布式事务回到初始状态。

没有全局锁，也没有冻结资源，所以**存在脏写问题。**

**脏写：快照恢复数据时数据库被更新。**

在多线程并发访问 AT 模式的分布式事务时，有**可能出现脏写问题，归根结底是业务之间的隔离问题**，导致 a 业务根据快照恢复数据时，另一个 a 业务已提交业务成功，导致恢复的数据是未更新时的样子

![](<assets/1740016795803.png>)

Saga 也分为两个阶段：

*   **一阶段：直接提交本地事务**
*   **二阶段：成功则什么都不做；失败则通过编写补偿业务来回滚**

#### 4.4.2. 优缺点

**优点：**

*   事务参与者可以**基于事件驱动**实现**异步调用，吞吐高，适合长事务**
*   一阶段直接提交事务，无锁，性能好
*   不用编写 TCC 中的三个阶段，实现简单

**缺点：**

*   软状态持续时间不确定，时效性差
*   没有锁，没有事务隔离，**会有脏写**

### 4.5. 四种模式对比，XA,AT,TCC,SAGA

我们从以下几个方面来对比四种实现：

*   一致性：能否保证事务的一致性？强一致还是最终一致？
*   隔离性：事务之间的隔离性如何？
*   代码侵入：是否需要对业务代码改造？
*   性能：有无性能损耗？
*   场景：常见的业务场景

如图：

![](<assets/1740016796043.png>)

**XA：**一阶段**锁定**数据库资源，业务协调者 TC 通知各业务执行并获取执行状态，二阶段如果存在业务失败，则通知所有业务回滚。

**AT：**一阶段资源管理者 RM **记录 undo-log（数据快照）**，二阶段如果存在业务失败，根据快照回滚。全局锁是在释放 DB 锁之前，先拿到全局锁。避免同一时刻有另外一个事务来操作当前数据。

**TCC：**一阶段 try 方法**资源**的检测和**预留，**二阶段如果存在业务失败，根据 **Cancel 方法**编写的内容**释放预留资源**

**SAGA：**一阶段直接提交本地事务，二阶段失败则通过**编写补偿业务**来回滚

## 5. 其他分布式事务方案（不使用 Seata）

### 5.1. 柔性事务方法实现最终一致性

#### 5.1.1. 柔性事务 - 最大努力通知型方案

按规律进行通知，不保证数据一定能通知成功，但会提供可查询操作接口进行**核对**。

这种方案主要用在与第三方系统通讯时，比如：调用微信或支付宝支付后的支付结果通知。这种方案也是结合 MQ 进行实现，例如： 通过 MQ 发送 http 请求，设置最大通知次数。达到通知次数后即不再通知。  
案例：银行通知、商户通知等 (各大交易业务平台间的商户通知：多次通知、查询校对、对账文件)，支付宝的支付成功异步回调。

#### 5.1.2. 柔性事务 - 可靠消息 + 最终一致性方案（异步确保型）

业务处理服务在业务事务提交之前，向实时消息服务**请求发送消息**，实时消息服务只记录消息数据，而不是真正的发送。

业务处理服务在**业务事务提交之后**，向实时消息服务确认发送。只有在得到确认发送指令后，实时消息服务才会**真正发送**。

**案例：**

在商品下单业务的最后要锁定库存，我们设置在锁定库存后发 RabbitMQ 延迟队列消息，通知锁定库存成功，两分钟后消费消息，根据库存信息查询检查订单是否存在，若不存在代表下订单失败，此时要回滚，也就是解锁库存。

### 5.2.**xxl-job 和消息组件实现最终一致性**

在主要业务的数据库里创建消息表和消息记录表； 

在执行主要业务成功后插入一条消息表数据，xxl-job 定时扫描任务列表并发处理任务。因为主要业务的表和消息表在一个数据库里，所以是本地事务，不是分布式事务。

![](<assets/1740016796378.png>)

**举例：** 

例如课程发布业务， 在课程信息入库后，插入一条消息表数据，课程 id 为业务信息 1，阶段 1 是存 redis、阶段 2 是 ES 添加索引，阶段 3 是静态化页面到文件系统：

**强一致：**

如果要满足 CP 就表示课程发布操作后向数据库、redis、elasticsearch、MinIO 写四份数据，只要有一份写失败其它的全部回滚。

**最终一致：**

课程发布操作后，先更新数据库中的课程发布状态，更新后向 redis、elasticsearch、MinIO 写课程信息，只要在一定时间内最终向 redis、elasticsearch、MinIO 写数据成功即可。  
 

## 6. 高可用

Seata 的 TC 服务作为分布式事务核心，一定要保证集群的高可用性。

### 6.1. 高可用架构模型

搭建 TC 服务集群非常简单，启动多个 TC 服务，注册到 nacos 即可。

但集群并不能确保 100% 安全，万一集群所在机房故障怎么办？所以如果要求较高，一般都会做异地多机房容灾。

比如一个 TC 集群在上海，另一个 TC 集群在杭州：

![](<assets/1740016796636.png>)

微服务基于事务组（tx-service-group) 与 TC 集群的映射关系，来查找当前应该使用哪个 TC 集群。当 SH 集群故障时，只需要将 vgroup-mapping 中的映射关系改成 HZ。则所有微服务就会切换到 HZ 的 TC 集群了。

### 6.2. 实现高可用，异地容灾

#### 6.2.1. 模拟异地容灾的 TC 集群

计划启动两台 seata 的 tc 服务节点：

<table><thead><tr><th>节点名称</th><th>ip 地址</th><th>端口号</th><th>集群名称</th></tr></thead><tbody><tr><td>seata</td><td>127.0.0.1</td><td>8091</td><td>SH</td></tr><tr><td>seata2</td><td>127.0.0.1</td><td>8092</td><td>HZ</td></tr></tbody></table>

之前我们已经启动了一台 seata 服务，端口是 8091，集群名为 SH。

现在，将 seata 目录复制一份，起名为 seata2

修改 seata2/conf/registry.conf 内容如下：

```
registry {
  ## tc服务的注册中心类，这里选择nacos，也可以是eureka、zookeeper等
  type = "nacos"
 
  nacos {
    ## seata tc 服务注册到 nacos的服务名称，可以自定义
    application = "seata-tc-server"
    serverAddr = "127.0.0.1:8848"
    group = "DEFAULT_GROUP"
    namespace = ""
    cluster = "HZ"
    username = "nacos"
    pass改成自己的密码word = "nacos"
  }
}
 
config {
  ## 读取tc服务端的配置文件的方式，这里是从nacos配置中心读取，这样如果tc是集群，可以共享配置
  type = "nacos"
  ## 配置nacos地址等信息
  nacos {
    serverAddr = "127.0.0.1:8848"
    namespace = ""
    group = "SEATA_GROUP"
    username = "nacos"
    pass改成自己的密码word = "nacos"
    dataId = "seataServer.properties"
  }
}
```

进入 seata2/bin 目录，然后运行命令：

```
seata-server.bat -p 8092
```

打开 nacos 控制台，查看服务列表：

![](<assets/1740016797062.png>)

点进详情查看：

![](<assets/1740016797267.png>)

#### 6.2.2. 将事务组映射配置到 nacos

接下来，我们需要将 tx-service-group 与 cluster 的映射关系都配置到 nacos 配置中心。

新建一个配置：

![](<assets/1740016797477.png>)

配置的内容如下：

```
## 事务组映射关系
service.vgroupMapping.seata-demo=SH
 
service.enableDegrade=false
service.disableGlobalTransaction=false
## 与TC服务的通信配置
transport.type=TCP
transport.server=NIO
transport.heartbeat=true
transport.enableClientBatchSendRequest=false
transport.threadFactory.bossThreadPrefix=NettyBoss
transport.threadFactory.workerThreadPrefix=NettyServerNIOWorker
transport.threadFactory.serverExecutorThreadPrefix=NettyServerBizHandler
transport.threadFactory.shareBossWorker=false
transport.threadFactory.clientSelectorThreadPrefix=NettyClientSelector
transport.threadFactory.clientSelectorThreadSize=1
transport.threadFactory.clientWorkerThreadPrefix=NettyClientWorkerThread
transport.threadFactory.bossThreadSize=1
transport.threadFactory.workerThreadSize=default
transport.shutdown.wait=3
## RM配置
client.rm.asyncCommitBufferLimit=10000
client.rm.lock.retryInterval=10
client.rm.lock.retryTimes=30
client.rm.lock.retryPolicyBranchRollbackOnConflict=true
client.rm.reportRetryCount=5
client.rm.tableMetaCheckEnable=false
client.rm.tableMetaCheckerInterval=60000
client.rm.sqlParserType=druid
client.rm.reportSuccessEnable=false
client.rm.sagaBranchRegisterEnable=false
## TM配置
client.tm.commitRetryCount=5
client.tm.rollbackRetryCount=5
client.tm.defaultGlobalTransactionTimeout=60000
client.tm.degradeCheck=false
client.tm.degradeCheckAllowTimes=10
client.tm.degradeCheckPeriod=2000
 
## undo日志配置
client.undo.dataValidation=true
client.undo.logSerialization=jackson
client.undo.onlyCareUpdateColumns=true
client.undo.logTable=undo_log
client.undo.compress.enable=true
client.undo.compress.type=zip
client.undo.compress.threshold=64k
client.log.exceptionRate=100
```

#### 3. 微服务读取 nacos 配置

接下来，需要修改每一个微服务的 application.yml 文件，让微服务读取 nacos 中的 client.properties 文件：

```
seata:
 config:
 type: nacos
 nacos:
 server-addr: 127.0.0.1:8848
 username: nacos
 password: nacos
 group: SEATA_GROUP
 data-id: client.properties
```

重启微服务，现在微服务到底是连接 tc 的 SH 集群，还是 tc 的 HZ 集群，都统一由 nacos 的 client.properties 来决定了。