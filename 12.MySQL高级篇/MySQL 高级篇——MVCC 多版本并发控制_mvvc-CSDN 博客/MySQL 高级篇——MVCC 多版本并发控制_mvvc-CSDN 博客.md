---
url: https://blog.csdn.net/qq_40991313/article/details/130462818
title: MySQL 高级篇——MVCC 多版本并发控制_mvvc-CSDN 博客
date: 2025-02-20 13:50:23
tag: 
summary: 
---
 **导航：** 

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1. 什么是 MVCC](#t0)

[2. 快照读与当前读](#t1)

[2.1 快照读](#t2)

[2.2 当前读](#t3)

[3. MVCC 三剑客](#t4)

[3.1 回顾隔离级别](#t5)

[3.2 隐藏字段、Undo Log 版本链](#t6)

[3.3 ReadView](#t7)

[3.3.1 ReadView（读视图）简约版](#t8)

[3.3.2 设计思路](#t9)

[3.3.3 ReadView 的规则](#t10)

[3.3.4 MVCC 整体操作流程](#t11)

[4. 举例说明 MVCC 流程](#t12)

[4.1 读提交的 MVCC 流程](#t13)

[4.2 可重复读的 MVCC 流程](#t14) 

[4.3 InnoDB 解决幻读问题](#t15)

[5. 总结](#t16)

## 1. 什么是 MVCC

多版本并发控制，通过管理记录的多个版本，实现了数据库事务并发时的一致性读，即当前事务读取正在被其他事务更新的行时，能读到该记录被更新之前的版本。解决了读写冲突。 

MVCC （Multiversion Concurrency Control），**多版本并发控制**。顾名思义，MVCC 是通过数据行的多个版本管理来实现数据库的 并发控制。这项技术使得在 InnoDB 的事务隔离级别下执行**一致性读**操作有了保证。换言之，就是为了**查询一些正在被另一个事务更新的行，并且可以看到它们被更新之前的值**，这样在做查询的时候就不用等待另一个事务释放锁。

**MVCC 没有正式的标准**，在不同的 DBMS（数据库管理系统）中 MVCC 的实现方式可能是不同的，也不是普遍使用的（大家可以参考相关的 DBMS 文档）。**这里讲解 InnoDB 中 MVCC 的实现机制**（MySQL 其他的存储引擎并不支持它）。

## 2. 快照读与当前读

快照读：MVCC 在 MySQLInnoDB 中的实现主要是为了提高数据库并发性能，用更好的方式去处理读 - 写冲突，做到即使有**读写冲突时**，也能做到**不加锁**，**非阻塞并发读**，而这个读指的就是**快照读**, 而非当前读。MVCC 本质是采用**乐观锁思想**的一种方式。

当前读实际上是一种加锁的操作，是悲观锁的实现。

### 2.1 快照读

快照读又叫一致性读，读取的是快照数据。**不加锁的简单的 SELECT** 都属于快照读，即**不加锁的非阻塞读**；比如这样：

```
SELECT * FROM student LOCK IN SHARE MODE; # 共享锁
SELECT * FROM student FOR UPDATE; # 排他锁
INSERT INTO student values ... # 排他锁
DELETE FROM student WHERE ... # 排他锁
UPDATE student SET ... # 排他锁
```

之所以出现快照读的情况，是基于提高并发性能的考虑，快照读的实现是基于 MVCC，它在很多情况下， 避免了加锁操作，降低了开销。

既然是基于多版本，那么快照读可能**读到的并不一定是数据的最新版本**，而有可能是之前的历史版本。

快照读的前提是隔离级别不是串行级别，串行级别下的快照读会退化成当前读。

### 2.2 当前读

当前读读取的是记录的**最新版本**（**最新数据，而不是历史版本的数据**），读取时还要保证其他并发事务 不能修改当前记录，会**对读取的记录进行加锁**。加锁的 SELECT，或者对数据进行增删改都会进行当前 读。比如：

```
# Transaction 10
BEGIN;
UPDATE student SET name="李四" WHERE id=1;
UPDATE student SET name="王五" WHERE id=1;
# Transaction 20
BEGIN;
# 更新了一些别的表的记录
...
```

## 3. MVCC 三剑客

### 3.1 回顾隔离级别

事务隔离级别是指操作同一资源的并发事务之间的隔离度。隔离级别越高，事务之间的相互干扰就越小，安全性就越高。

**读问题：**

*   **脏读：**读到了脏数据。当前事务读到另一个未提交事务刚改的数据。只有读未提交会脏读。
*   **不可重复读：**前后重复读的数据不一样。前后两次读同数据，这期间数据被其他事务改了，导致前后读取的数据不同。
*   **幻读：**前后读的数据是一样，但多了几行或少了几行，像幻觉一样。事务前后读的数据集合不同，导致出现 “幻像” 行。仅串行化能解决幻读问题。

**事务隔离级别**

*   **读未提交：**事务能读到所有未提交事务的数据。压根不加锁、没隔离，性能最高。
*   **读提交：**事务能读到已提交事务的数据。底层由 MVVC 实现，每次快照读都生成读视图 。解决脏读问题。
*   **可重复读（默认）：**前后读的数据相同。底层由 MVVC 实现，仅第一次快照读生成读视图。解决脏读、不可重复读问题。MySQL InnoDB 引擎的可重复读解决了幻读问题。快照读由 MVCC 解决，当前读通过 next-key lock 解决。
*   **串行化：**事务一拿到锁阻塞其他事务，直到释放锁。读时共享锁，写时排它锁。阻塞导致性能最差。解决脏读、不可重复读、幻读问题。 

**MVVC：**多版本并发控制。MVCC 三剑客：隐藏字段、Undo Log、Read View。

**共享锁（读锁）：**在共享锁下，多个线程可以同时读取数据，但只有一个线程能够修改数据。当一个线程在修改数据时，必须获得独占锁，以便其他线程不能访问数据。

**排它锁（写锁）：**在排它锁下，只有一个线程可以修改数据，其他线程不允许访问数据。

**MVCC** 可以不采用锁机制，而是通过乐观锁的方式来解决不可重复读和幻读问题! 它可以在大多数情况下替代行级锁，降低系统的开销

![](<assets/1740030625057.png>)

### 3.2 隐藏字段、Undo Log 版本链

**隐藏字段** 

对于使用 InnoDB 存储引擎的表来说，它的**聚簇索引记录中**都包含两个必要的**隐藏列**。

*   **trx_id（事务 id） ：**每次一个事务对某条聚簇索引记录进行**改动时**，都会把该事务的 **事务 id** 赋值给 trx_id 隐藏列。
*   roll_pointer（回滚指针） ：指向 undo 日志里该记录版本链的最近节点。每次对某条聚簇索引记录进行**改动时**，都会把**旧的版本**写入到 **undo 日志** 中，然后这个隐藏列**就相当于一个指针**，可以通过它来**找到该记录修改前的信息**。

**Undo Log（回滚日志）**

当一个事务对数据库执行写操作的时候，MySQL 会将要修改的数据的**旧版本数据先写入到 Undo Log** 中，然后再将**新版本数据写入到数据库中**。在事务提交之前，Undo Log 中的数据可以**用于回滚**；在事务提交后，Undo Log 中的旧版本数据也可以被用来提供读取操作的历史版本数据。

**Undo Log（回滚日志）的版本链**

在 MySQL 的实现中，Undo Log 版本链用于**维护数据的历史版本**，以及**回滚和读取历史版本**时用到的数据。 

Undo Log 的版本链就是用于维护这些历史版本数据的一个**链表**。

在 Undo Log 版本链中，**每个版本对应一个版本号或时间戳**，**新版本数据会被添加到版本链的头部。**同时，Undo Log 版本链也可以通过一个类似于链表的指针结构来维护多个事务间的不同版本数据。

对于读取操作，如果需要读取历史版本的数据，MySQL 就可以通过访问 Undo Log 版本链来获取指定版本下的数据。而对于回滚操作，MySQL 可以简单地恢复 Undo Log 中对应版本的数据，从

**案例：** 

假设插入该记录的事务 id 为 8，那么此刻该条记录的示意图如下所示： 

![](<assets/1740030625302.png>)

![](<assets/1740030625730.png>)

insert undo 只在事务回滚时起作用，当**事务提交后，该类型的 undo 日志就没用了**，它占用的 Undo Log Segment 也会被系统回收（也就是该 undo 日志占用的 Undo 页面链表要么被重用，要么被释放）。

假设之后两个事务 id 分别为 `10` 、 `20` 的事务对这条记录进行`UPDATE` 操作，操作流程如下：

![](<assets/1740030625959.png>)

能不能在两个事务中交叉更新同一条记录呢?

不能! 这不就是一个事务修改了另一个未提交事务修改过的数据，脏写。

**lnnoDB 使用锁来保证不会有脏写**情况的发生，也就是在第一个事务更新了某条记录后，就会给这条记录加锁，另一个事务再次更新时就需要等待第一个事务提交了，把锁释放之后才可以继续更新。 

每次对记录进行改动，都会记录一条 undo 日志，**每条 undo 日志也都有一个 roll_pointer 属性** （ INSERT 操作对应的 undo 日志没有该属性，因为该记录并没有更早的版本），可以将这些 **undo 日志 都连起来**，串成一个链表：

![](<assets/1740030626213.png>)

对该记录每次更新后，都会将旧值放到一条 undo 日志 中，就算是该记录的一个旧版本，随着更新次数 的增多，所有的版本都会被 roll_pointer 属性连接成一个链表，我们把这个链表称之为 版本链 ，**版本链的头节点就是当前记录最新的值**。

每个版本中还包含生成该版本时对应的事务 id。

### 3.3 ReadView

MVCC 的实现依赖于：隐藏字段、Undo Log、Read View。

#### 3.3.1 ReadView（读视图）简约版

Read View 是事务进行**快照读操作的时候生产的读视图**，在该事务执行快照读的那一刻，会生成一个数据系统当前的快照，**记录并维护系统当前活跃事务的 id**，**事务的 id 值是递增的**。

其实 Read View 的**最大作用**是用来做**可见性判断**的，也就是说当某个事务在执行快照读的时候，对该记录创建个 Read View 的视图，**把这个读视图当作条件去判断当前事务能够看到哪个版本的数据**，有可能读取到的是最新的数据也有可能读取的是当前行记录的 undolog 中某个版本的数据

首先要知道 Read View 中的全局属性:

*   **creator_trx_id ：**创建这个读视图的**事务的 ID**。增删改事务才有资格分配 id，读事务 id 为 0.
*   **trx_ids：**表示在生成 ReadView 时当前系统中活跃的读写事务的**事务 id 列表 。**
*   **up_limit_id：**记录活跃事务列表中**最小的事务 ID**。最小即最早。
*   **low_limit_id：**表示生成 ReadView 时系统中应该分配给**下一个事务的 id 值**。将是列表里最大 id。

**Read View 的规则，即可见性算法：**

通过读视图，可以判断当前查询中，记录的某个版本是否可见。

判断方法是比较各版本的 trx_id 和读视图里的活跃事务 id，如果某版本 trx_id 小于读视图的最小事务 id，则代表那个版本是生成读视图之前的已提交版本，当前查询就可以访问那个版本。

**MVCC 流程：**

查询，生成读视图，用读视图的活跃事务 id 依次对比各版本的事务 id，找到符合规则的数据。

**应用：** 事务隔离级别里的读提交和可重复读底层是由 MVCC 实现的。并且 MySQL InnoDB 引擎的可重复读由 MVCC 解决了幻读问题。

**读提交的 MVCC 原理：**事务每次读到的都是最新已提交的数据。**每次读取数据前都生成一个 ReadView。**快照读生成 Read View，不断对比版本链各版本的 trx_id，直到发现某版本 trx_id 比 Read View 的活跃事务列表里最小 trx_id 还小，该版本则是快照读前最新已提交的数据。

**可重复读的 MVCC 原理：**只在第一次查询时生成 ReadView，之后查询用第一次快照读时生成的 ReadView。

在 MVCC 机制中，多个事务对同一个行记录进行更新会产生多个历史快照，这些**历史快照保存在 Undo Log 里**。如果一个事务想要查询这个行记录，需要读取哪个版本的行记录呢? 这时就需要用到 Readview 了，它帮我们解决了行的可见性问题。

**ReadView** 就是事务在使用 MVCC 机制进行**快照读操作时产生的读视图**。当事务启动时，会生成数据库系统当前的个快照，lnnoDB 为每个事务构造了一个数组，用来记录并维护系统当前 **活跃事务**的 ID(“活跃” 指的就是，**启动了但还没提交**)。 

#### 3.3.2 设计思路

使用 READ UNCOMMITTED 隔离级别的事务，由于可以读到未提交事务修改过的记录，所以直接读取记录的最新版本就好了。

使用 SERIALIZABLE 隔离级别的事务，InnoDB 规定使用加锁的方式来访问记录。

使用 READ COMMITTED 和 REPEATABLE READ 隔离级别的事务，都必须保证读到 已经提交了的 事务修改过的记录。假如另一个事务已经修改了记录但是尚未提交，是不能直接读取最新版本的记录的，核心问题就是需要判断一下版本链中的哪个版本是当前事务可见的，这是 ReadView 要解决的主要问题。

这个 ReadView 中主要包含 4 个比较重要的内容，分别如下：

1.  creator_trx_id ，创建这个 Read View 的事务 ID。
    
    说明：只有在对表中的记录做改动时（执行 INSERT、DELETE、UPDATE 这些语句时）才会为 事务分配事务 id，否则在一个只读事务中的事务 id 值都默认为 0。
    
2.  trx_ids ，表示在生成 ReadView 时当前系统中活跃的读写事务的 事务 id 列表 。
    
3.  up_limit_id ，活跃的事务中最小的事务 ID。
    
4.  low_limit_id ，表示生成 ReadView 时系统中应该分配给下一个事务的 id 值。low_limit_id 是系 统最大的事务 id 值，这里要注意是系统中的事务 id，需要区别于正在活跃的事务 ID。
    

注意：low_limit_id 并不是 trx_ids 中的最大值，事务 id 是递增分配的。比如，现在有 id 为 1， 2，3 这三个事务，之后 id 为 3 的事务提交了。那么一个新的读事务在生成 ReadView 时， trx_ids 就包括 1 和 2，up_limit_id 的值就是 1，low_limit_id 的值就是 4。

*   **creator_trx_id ：**创建这个读视图的**事务的 ID**。增删改事务才有资格分配 id，读事务 id 为 0.
*   **trx_ids：**表示在生成 ReadView 时当前系统中活跃的读写事务的**事务 id 列表 。**
*   **up_limit_id：**记录活跃事务列表中**最小的事务 ID**。最小即最早。
*   **low_limit_id：**表示生成 ReadView 时系统中应该分配给**下一个事务的 id 值**。将是列表里最大 id。

![](<assets/1740030626491.png>)

#### 3.3.3 ReadView 的规则

有了这个 ReadView，这样**在当前事务访问某条记录时**，只需要按照下边的步骤**判断记录的某个版本是否可见。**

*   如果被访问版本的 trx_id 属性值与 ReadView 中的 creator_trx_id 值相同，意味着当前事务在访问它自己修改过的记录，所以该版本可以被当前事务访问。
*   如果被访问版本的 trx_id 属性值小于 ReadView 中的 up_limit_id 值，表明生成该版本的事务在当前事务生成 ReadView 前已经提交，所以该版本可以被当前事务访问。
*   如果被访问版本的 trx_id 属性值大于或等于 ReadView 中的 low_limit_id 值，表明生成该版本的事务在当前事务生成 ReadView 后才开启，所以该版本不可以被当前事务访问。
*   如果被访问版本的 trx_id 属性值在 ReadView 的 up_limit_id 和 low_limit_id 之间，那就需要判断一下 trx_id 属性值是不是在 trx_ids 列表中。
    *   如果在，说明创建 ReadView 时生成该版本的事务还是活跃的，该版本不可以被访问。
    *   如果不在，说明创建 ReadView 时生成该版本的事务已经被提交，该版本可以被访问。

#### 3.3.4 MVCC 整体操作流程

了解了这些概念之后，我们来看下**当查询一条记录的时候**，系统如何通过 **MVCC** 找到它：

1.  首先获取事务自己的版本号，也就是事务 ID；
2.  获取 ReadView；
3.  查询得到的数据，然后与 ReadView 中的事务版本号进行比较；
4.  如果不符合 ReadView 规则，就需要从 Undo Log 中获取历史快照；
5.  最后返回符合规则的数据。

如果某个版本的数据对当前事务不可见的话，那就顺着版本链找到下一个版本的数据，继续按照上边的步骤判断可见性，依此类推，直到版本链中的最后一个版本。如果最后一个版本也不可见的话，那么就意味着该条记录对该事务完全不可见，查询结果就不包含该记录。  
InnoDB 中，MVCC 是通过 Undo Log + Read View 进行数据读取，Undo Log 保存了历史快照，而 Read View 规则帮我们判断当前版本的数据是否可见。  

在隔离级别为读已提交（Read Committed）时，一个事务中的每一次 SELECT 查询都会重新获取一次 Read View。

如表所示：

![](<assets/1740030626768.png>)

注意，此时同样的查询语句都会重新获取一次 Read View，这时如果 Read View 不同，就可能产生不可重复读或者幻读的情况。

当隔离级别为可重复读的时候，就避免了不可重复读，这是因为一个事务只在第一次 SELECT 的时候会获取一次 Read View，而后面所有的 SELECT 都会复用这个 Read View，如下表所示：

![](<assets/1740030627006.png>)

## 4. 举例说明 MVCC 流程

![](<assets/1740030627232.png>)

### 4.1 **读提交的 MVCC 流程**

**读提交：**事务每次读到的都是最新已提交的数据。底层由 MVVC 实现 ，每次读取数据前都生成一个 ReadView。只解决脏读问题。

 **读提交的 MVCC 原理：**每次读都生成 Read View，不断对比版本链各版本的 trx_id，直到发现某版本 trx_id 比 Read View 的活跃事务列表里最小 trx_id 还小，该版本则是**读前最新已提交的数据**。

现在有两个 **事务 id** 分别为 **10 、 20** 的事务在执行:

```
# 使用READ COMMITTED隔离级别的事务
BEGIN;
 
# SELECT1：Transaction 10、20未提交
SELECT * FROM student WHERE id = 1; # 得到的列name的值为'张三'，对应事务id为8
```

说明：事务执行过程中，只有在第一次真正修改记录时（比如使用 INSERT、DELETE、UPDATE 语句），才会被分配一个单独的事务 id，这个事务 id 是递增的。所以我们才在事务 2 中更新一些别的表的记录，目的是让它分配事务 id。

此刻，表 student 中 id 为 1 的记录得到的版本链表如下所示：

![](<assets/1740030627482.png>)

假设现在有一个使用 **读提交** READ COMMITTED 隔离级别的事务开始执行：

```
# Transaction 10
BEGIN;
UPDATE student SET name="李四" WHERE id=1;
UPDATE student SET name="王五" WHERE id=1;
COMMIT;
```

这个 SELECT1 的执行过程如下:

1.  在执行 **SELECT 语句（快照读）**时会先**生成一个 ReadView**。ReadView 的 trx_ids 列表的内容就是 **[10，20]**。up_limit_id 为 10，low_limit_id 为 21，creator_trx_id 为 0。
2.  从版本链中挑选可见的记录，从图中看出，**最新版本**的列 name 的内容是**王五**，该版本的 **trx_id 值为 10**，**在 trx_ids 列表内**，所以**不符合可见性要求**，根据 roll_pointer 跳到下一个版本。
3.  下一个版本的列 name 的内容是，李四，该版本的 trx_id 值也为 10，也在 trx_ids 列表内，所以也不符合要求，继续跳到下一个版本。
4.  **下一个版本**的列 name 的内容是，**张三**，该版本的 **trx_id 值为 8**，**小于** ReadView 中的**最小活跃事务** up_limit_id 值 10，说明该版本是**生成 ReadView 之前的最近版本**，所以这个版本是符合要求的，最后返回给用户的版本就是这条列 name 为，张三的记录。

之后，我们把 `事务id` 为 `10` 的事务提交一下：

```
# Transaction 20
BEGIN;
# 更新了一些别的表的记录
...
UPDATE student SET name="钱七" WHERE id=1;
UPDATE student SET name="宋八" WHERE id=1;
```

然后再到 `事务id` 为 `20` 的事务中更新一下表 `student` 中 `id` 为 `1` 的记录：

```
# 使用READ COMMITTED隔离级别的事务
BEGIN;
 
# SELECT1：Transaction 10、20均未提交
SELECT * FROM student WHERE id = 1; # 得到的列name的值为'张三'
 
# SELECT2：Transaction 10提交，Transaction 20未提交
SELECT * FROM student WHERE id = 1; # 得到的列name的值为'王五'
```

此刻，表 student 中 `id` 为 `1` 的记录的版本链就长这样：

![](<assets/1740030627841.png>)

然后再到刚才使用 `READ COMMITTED` 隔离级别的事务中继续查找这个 id 为 1 的记录，如下：

```
# Transaction 10
BEGIN;
UPDATE student SET name="李四" WHERE id=1;
UPDATE student SET name="王五" WHERE id=1;
# Transaction 20
BEGIN;
# 更新了一些别的表的记录
...
```

这个 SELECT2 的执行过程如下

1.   在执行 SELECT 语句时会又会单独生成一个 **ReadView**，该 ReadView 的 trx_ids 列表的内容 **[20]**，就是 up_limit_id 为 20，low_limit_id 为 21， creator_trx_id 为 0。
2.  从版本链中挑选可见的记录，从图中看出，最新版本的列 name 的内容是，宋八，该版本的 trx_id 值为 20，在 trx_ids 列表内，所以不符合可见性要求，根据 roll_pointer 跳到下一个版本。
3.  下一个版本的列 name 的内容是钱七’，该版本的 trx_id 值为 28，也在 trx_ids 列表内，所以也不符合要求，继续跳到下一个版本。
4.  **下一个版本**的列 name 的内容是**'王五’**，该版本的 trx_id 值为 10，小于 ReadView 中的 up_limit_id 值 28，所以这个版本是符合要求的，最后返回给用户的版本就是这条列 name 为，王五’的记录。 

### 4.2 可重复读的 MVCC 流程 

**只在第一次查询时生成 ReadView**，之后查询用第一次快照读时生成的 ReadView。

比如，系统里有两个 `事务id` 分别为 `10` 、 `20` 的事务在执行：

```
# 使用REPEATABLE READ隔离级别的事务
BEGIN;
 
# SELECT1：Transaction 10、20未提交
SELECT * FROM student WHERE id = 1; # 得到的列name的值为'张三'
```

此刻，表 student 中 id 为 1 的记录得到的版本链表如下所示：

![](<assets/1740030628152.png>)

假设现在有一个使用 `REPEATABLE READ` 隔离级别的事务开始执行：

```
# Transaction 10
BEGIN;
 
UPDATE student SET name="李四" WHERE id=1;
UPDATE student SET name="王五" WHERE id=1;
 
COMMIT;
```

这个 SELECT1 的执行过程如下:

1.  在执行 SELECT 语句时会先生成一个 Readview，ReadView 的 trx_ids 列表的内容就是 [10，20].up_limit_id 为 10，low_limit_id 为 21，creator_trx_id 为 0。
2.  然后从版本链中挑选可见的记录，从图中看出，最新版本的列 name 的内容是，王五’，该版本的 trx_id 值为 10，在 trx_ids 列表内，所以不符合可见性要求，根据 rol1_pointer 跳到下一个版本.
3.  下一个版本的列 name 的内容是，李四’，该版本的 trx_id 值也为 10，也在 trx_ids 列表内，所以也不符合要求，继续跳到下一个版本。
4.  下一个版本的列 name 的内容是'张三'，该版本的 trx_id 值为 8，小于 Readview 中的 up_limit_id 值 10，所以这个版本是符合要求的，最后返回给用户的版本就是这条列 name 为，张三’的记录。

之后，我们把 事务 id 为 10 的事务提交一下，就像这样：

```
# Transaction 20
BEGIN;
# 更新了一些别的表的记录
...
UPDATE student SET name="钱七" WHERE id=1;
UPDATE student SET name="宋八" WHERE id=1;
```

然后再到 事务 id 为 20 的事务中更新一下表 student 中 id 为 1 的记录：

```
# 使用REPEATABLE READ隔离级别的事务
BEGIN;
# SELECT1：Transaction 10、20均未提交
SELECT * FROM student WHERE id = 1; # 得到的列name的值为'张三'
# SELECT2：Transaction 10提交，Transaction 20未提交
SELECT * FROM student WHERE id = 1; # 得到的列name的值仍为'张三'
```

此刻，表 student 中 `id` 为 `1` 的记录的版本链长这样：

![](<assets/1740030628457.png>)

然后再到刚才使用 `REPEATABLE READ` 隔离级别的事务中继续查找这个 `id` 为 `1` 的记录，如下：

```
insert into student(id,name) values(2,'李四');
insert into student(id,name) values(3,'王五');
```

SELECT2 的执行过程如下:

1.  因为当前事务的隔离级别为 REPEATABLE READ，而之前在执行 SELECT1 时已经生成过 ReadView 了，所以**此时直接复用之前的 ReadView**，之前的 ReadView 的 trx_ids 列表的内容就是 [10，20]，up_limit_id 为 1， low_limit_id 为 21， creator_trx_id 为 0
2.  然后从版本链中挑选可见的记录，从图中可以看出，最新版本的列 name 的内容是宋八’，该版本的 trx_id 值为 29，在 trx_ds 列表内，所以不符合可见性要求，根据 ro11 pointer 跳到下一个版本
3.  下一个版本的列 name 的内容是'钱七’，该版本的 trx_id 值为 20，也在 trx_ids 列表内，所以也不符合要求，继续跳到下一个版本。
4.  下一个版本的列 name 的内容是，王五’，该版本的 trx_id 值为 10，而 trx_ids 列表中是包含值为 10 的事务 id 的，所以该版本也不符合要求，同理下一个列 name 的内容是，李四的版本也不符合要求。继续跳到下个版本
5.  下一个版本的列 name 的内容是'张三’，该版本的 trx_id 值为 80，小于 ReadView 中的 up_limit_id 值 18，所以这个版本是符合要求的，最后返回给用户的版本就是这条列 c 为，张三’的记录。

**这次 SELECT 查询得到的结果是重复的**，记录的列 c 值都是张三，这就是**可重复读的含义**。如果我们之后再把事务 id 为 20 的记录提交了，然后再到刚才使用 REPEATABLE READ 隔离级别的事务中继续查找这个 id 为 1 的记录，得到的结果还是张三，具体执行过程大家可以自己分析一下。

### 4.3 InnoDB 解决幻读问题

接下来说明 InnoDB 是如何解决幻读的。

假设现在表 student 中只有一条数据，数据内容中，主键 id=1，隐藏的 trx_id=10，它的 undo log 如下图所示。

![](<assets/1740030628670.png>)

假设现在有事务 A 和事务 B 并发执行，**事务 A 的事务 id 为 20 ， 事务 B 的事务 id 为 30** 。

**步骤 1：**事务 A 开始第一次查询数据，查询的 SQL 语句如下。

```
select * from student where id >= 1;
```

在开始查询之前，MySQL 会为事务 A 产生一个 ReadView，此时 ReadView 的内容如下： trx_ids= [20,30] ， up_limit_id=20 ， low_limit_id=31 ， creator_trx_id=20 。

由于此时表 student 中只有一条数据，且符合 where id>=1 条件，因此会查询出来。然后根据 ReadView 机制，发现该行数据的 trx_id=10，小于事务 A 的 ReadView 里 up_limit_id，这表示这条数据是事务 A 开启之前，其他事务就已经提交了的数据，因此事务 A 可以读取到。

结论：**事务 A 的第一次查询，能读取到一条数据，id=1。**

**步骤 2：**接着事务 B(trx_id=30)，往表 student 中新插入两条数据，并提交事务。

```
insert into student(id,name) values(2,'李四');
insert into student(id,name) values(3,'王五');
```

此时表 student 中就有三条数据了，对应的 undo 如下图所示：

![](<assets/1740030628857.png>)

**步骤 3：**接着**事务 A 开启第二次查询**，根据可重复读隔离级别的规则，此时事务 A 并不会再重新生成 ReadView。此时表 student 中的 3 条数据都满足 where id>=1 的条件，因此会先查出来。然后根据 ReadView 机制，判断每条数据是不是都可以被事务 A 看到。

1）首先 id=1 的这条数据，前面已经说过了，可以被事务 A 看到。

2）然后是 id=2 的数据，它的 trx_id=30，此时事务 A 发现，这个值处于 up_limit_id 和 low_limit_id 之 间，因此还需要再判断 30 是否处于 trx_ids 数组内。由于事务 A 的 trx_ids=[20,30]，因此在数组内，这表 示 id=2 的这条数据是与事务 A 在同一时刻启动的其他事务提交的，所以这条数据不能让事务 A 看到。

3）同理，id=3 的这条数据，trx_id 也为 30，因此也不能被事务 A 看见。

![](<assets/1740030629219.png>)

结论：最终**事务 A 的第二次查询**，只能查询出 id=1 的这条数据。这和事务 A 的**第一次查询的结果是一样 的**，因此**没有出现幻读现**象，所以说在 **MySQL 的可重复读隔离级别下，不存在幻读问题。**

## 5. 总结

这里介绍了 MVCC 在 READ COMMITTD 、 REPEATABLE READ 这两种隔离级别的事务在执行快照读操作时 访问记录的版本链的过程。这样使不同事务的 读 - 写 、 写 - 读 操作并发执行，从而提升系统性能。

核心点在于 ReadView 的原理， READ COMMITTD 、 REPEATABLE READ 这两个隔离级别的一个很大不同 就是生成 ReadView 的时机不同：

*   READ COMMITTD 在每一次进行普通 SELECT 操作前都会生成一个 ReadView
*   REPEATABLE READ 只在第一次进行普通 SELECT 操作前生成一个 ReadView，之后的查询操作都重复 使用这个 ReadView 就好了。

说明: 我们之前说执行 DELETE 语句或者更新主键的 UPDATE 语句并不会立即把对应的记录完全从页面中删除而是执行一个所谓的 delete mark 操作，相当于只是对记录打上了一个删除标志位，这主要就是为 MVCC 服务的。

通过 MVCC 我们可以解决：

*   读写之间阻塞的问题。通过 MVCC 可以让读写互相不阻塞，即读不阻塞写，写不阻塞读，这样就可以提升事务并发处理能力。
*   降低了死锁的概率。这是因为 MVCC 采用了乐观锁的方式，读取数据时并不需要加锁，对于写操作，也只锁定必要的行。
*   解决快照读的问题。当我们查询数据库在某个时间点的快照时，只能看到这个时间点之前事务提交更新的结 3 果，而不能看到这个时间点之后事务提交的更新结果.