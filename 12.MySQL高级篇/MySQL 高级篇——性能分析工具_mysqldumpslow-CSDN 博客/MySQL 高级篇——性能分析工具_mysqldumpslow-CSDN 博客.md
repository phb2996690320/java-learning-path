---
url: https://blog.csdn.net/qq_40991313/article/details/130355955
title: MySQL 高级篇——性能分析工具_mysqldumpslow-CSDN 博客
date: 2025-02-20 13:50:18
tag: 
summary: 
---
 **导航：** 

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1. 数据库服务器的优化步骤](#t0)

[2. 查看系统性能参数](#t1)

[2.1 SHOW STATUS LIKE '参数'](#2.1%20SHOW%20STATUS%20LIKE%20'%20rel=)

[2.2 查看 SQL 的查询成本](#t2)

[3. 定位执行慢的 SQL：慢查询日志](#t3)

[3.0 介绍](#t4) 

[3.1 开启慢查询日志参数](#t5)

[3.2 查看慢查询次数](#t6)

[3.5 慢查询日志分析工具：mysqldumpslow](#t7)

[3.6 关闭慢查询日志](#t8)

[3.7 删除慢查询日志](#t9)

[4. 定位慢查询语句、查看 SQL 执行成本：show profile](#t10)

[5. 执行计划表：EXPLAIN](#t11)

[5.1 简介](#t12)

[5.2 基本语法](#t13)

[5.3 执行计划表介绍](#t14) 

[5.3.1 执行计划各个列的作用（概述）](#t15)

[5.3.2 详细介绍](#t16)

[5.3.2.1 select_type](#5.3.2.1%20select_type)

[5.3.2.2 key](#5.3.2.2%20key)

[5.3.2.3 type](#5.3.2.3%20type)

[5.3.2.4 Extra](#5.3.2.4%20Extra)

[5.4 EXPLAIN 四种输出格式](#t17)

[5.5 SHOW WARNINGS 的使用](#t18)

[6. 分析优化器执行计划：trace](#t19)

[7. MySQL 监控分析视图 - sys schema](#t20)

[7.1 简介](#t21)

[7.2 使用场景](#t22)

## 1. 数据库服务器的优化步骤

在数据库调优中，我们的目标就是**响应时间更快，吞吐量更大**。利用宏观的**监控工具**和微观的**日志分析**可以帮我们快速找到调优的思路和方式。 

**调优流程：**

1.  **SHOW STATUS** 观察服务器状态，是否存在周期性波动；如果存在的话就**缓存优化**；
2.  如果还存在不规则延迟或卡顿的话，就**开启慢查询、explan 分析查询语句**；
3.  如果发现 sql 等待时间长，就**调优服务器参数**；如果发现 sql 执行时间长，就**索引优化、表优化；**
4.  如果还存在不规则延迟或卡顿的话，就**观察** sql 查询是否到瓶颈了；是的话就**读写分离、分库分表。**

**三种分析工具（SQL 调优三步骤）：**慢查询、EXPLAN、SHOW PROFLING 

![](<assets/1740030618723.png>)

![](<assets/1740030619082.png>)

整个流程划分成了观察（Show status） 和行动（Action） 两个部分。字母 S 的部分代表观察（会使用相应的分析工具），字母 A 代表的部分是行动（对应分析可以采取的行动）。

![](<assets/1740030619370.png>)

## 2. 查看系统性能参数

### 2.1 **SHOW STATUS** LIKE '参数'

在 MySQL 中，可以使用 **SHOW STATUS** 语句查询一些 MySQL **数据库服务器**的**性能参数**、执行频率。

SHOW STATUS 语句语法如下：

```
CREATE TABLE `student_info` (
`id` INT(11) NOT NULL AUTO_INCREMENT,
`student_id` INT NOT NULL ,
`name` VARCHAR(20) DEFAULT NULL,
`course_id` INT NOT NULL ,
`class_id` INT(11) DEFAULT NULL,
`create_time` DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

示例，查看数据库连接次数和运行时长： 

![](<assets/1740030619648.png>)

 中括号代表可省略。

一些常用的性能参数如下：

**•　Connections：**连接 MySQL 服务器的次数。

**•　Uptime：**MySQL 服务器的上线时间。重启服务器后会重置。

•　**Slow_queries：慢查询的次数**。查询时长超过指定时间，次数越少越好。

•　Innodb_rows_read：Select 查询返回的行数

•　Innodb_rows_inserted：执行 INSERT 操作插入的行数

•　Innodb_rows_updated：执行 UPDATE 操作更新的行数

•　Innodb_rows_deleted：执行 DELETE 操作删除的行数

•　Com_select：查询操作的次数。

•　Com_insert：插入操作的次数。对于批量插入的 INSERT 操作，只累加一次。

•　Com_update：更新操作的次数。

•　Com_delete：删除操作的次数。

•　**last_query_cost：查询优化器上一个查询的成本**，最近一次删除用到数据页数量。

### 2.2 查看 SQL 的查询成本

```
SELECT student_id, class_id, NAME, create_time FROM student_info
WHERE id = 900001;
```

SQL 查询是一个动态的过程，从页加载的角度来看:

**1. 缓冲池查询效率优于从磁盘查**

如果页就在数据库**缓冲池**中，那么**效率是最高**的，否则还需要从内存或者磁盘中进行读取，当然针对单个页的读取来说，如果页存在于内存中，会比在磁盘中读取效率高很多.

MySQL 的缓冲池被分为多个不同的缓存池，其中包括：

*   查询缓存：用来缓存查询结果。
*   InnoDB 缓存池：用来缓存热点表和索引数据页。
*   MyISAM 缓存池：用来缓存表数据块。

当缓冲池中已经存储了较多的数据时，MySQL 会使用一种叫做缓冲池替换算法的方法，将部分缓存数据替换出去，以腾出空间为新的数据做缓存。

**MySQL 的缓冲池使用的是 LRU（最近最少使用）算法**，它会优先缓存最近使用的数据。当缓冲池的空间不足时，MySQL 会将最不常用的数据从缓冲池中替换出去，以腾出空间缓存新的数据。

**2. 批量顺序查询平均下来每页查询更高**

如果我们从磁盘中对单一页进行随机读，那么效率是很低的 (差不多 10ms)，而采用**顺序读取**的方式，**批量对页进行读取**，**平均一页的读取效率就会提升**很多，甚至要快于单个页面在内存中的随机读取。

所以说，遇到 IO 并不用担心，方法找对了，效率还是很高的。我们首先要考虑数据存放的位置，如果是**经常使用的数据就要尽量放到缓冲池**中，其次我们可以充分利用磁盘的吞吐能力，一次性**批量读取数据**，这样单个页的读取效率也就得到了提升。 

**测试缓冲池缓存已使用的表和索引到内存中，效率高：**查询 900001 和 900001~9000100 查询成本差很多，查询速度差不多

学生信息表为例：

```
+-----------------+----------+
| Variable_name | Value |
+-----------------+----------+
| Last_query_cost | 1.000000 |
+-----------------+----------+
```

如果我们想要查询 **id=900001** 的记录，然后看下查询成本，我们可以直接在聚簇索引上进行查找：

```
SELECT student_id, class_id, NAME, create_time FROM student_info
WHERE id BETWEEN 900001 AND 900100;
```

运行结果（1 条记录，运行时间为 0.042s ）

![](<assets/1740030619845.png>)

 然后再看下**查询优化器的成本**，实际上我们**只需要检索一个页**即可：

```
mysql> SHOW STATUS LIKE 'last_query_cost';
+-----------------+-----------+
| Variable_name | Value |
+-----------------+-----------+
| Last_query_cost | 21.134453 |
+-----------------+-----------+
```

```
[mysqld]
slow_query_log=ON # 开启慢查询日志的开关
slow_query_log_file=/var/lib/mysql/atguigu-slow.log #慢查询日志的目录和文件名信息
long_query_time=3 #设置慢查询的闽值为3秒，超出此设定值的SQL即被记录到慢查询日志
log_output=FILE
```

如果我们想要查询 id 在 **900001** 到 **9000100** 之间的学生记录呢？

```
#测试发现：设置global的方式对当前session的long_query_time失效。对新连接的客户端有效。所以可以一并
执行下述语句
set global long_query_time = 1;    #设置全局慢查询阈值1s
show global variables like '%long_query_time%';    #全局1s
set long_query_time=1;
show variables like '%long_query_time%';    #当前会话10s
```

运行结果（100 条记录，运行时间为 0.046s ）：

然后**再看下查询优化器的成本**，这时我们大概需要进行 20 个页的查询。

```
#得到返回记录集最多的10个SQL
mysqldumpslow -s r -t 10 /var/lib/mysql/atguigu-slow.log
 
#得到访问次数最多的10个SQL
mysqldumpslow -s c -t 10 /var/lib/mysql/atguigu-slow.log
 
#得到按照时间排序的前10条里面含有左连接的查询语句
mysqldumpslow -s t -t 10 -g "left join" /var/lib/mysql/atguigu-slow.log
 
#另外建议在使用这些命令时结合 | 和more 使用 ，否则有可能出现爆屏情况
mysqldumpslow -s r -t 10 /var/lib/mysql/atguigu-slow.log | more
```

你能看到页的数量是刚才的 20 倍，但是**查询的效率并没有明显的变化**，实际上这两个 SQL 查询的时间基本上一样，就是因为采用了**顺序读取**的方式将页面一次性加载到**缓冲池**中，然后再进行查找。虽然**页数量**（last_query_cost）**增加了不少**，但是通过缓冲池的机制，并**没有增加多少查询时间。**

**为什么第二次是直接从缓冲池查？**

因为 mysql 缓存淘汰策略 lru 最近最少使用，会优先缓存最近查询数据，优先淘汰最近最少使用数据。

使用场景：它对于比较开销是非常有用的，特别是我们有好几种查询方式可选的时候。

## 3. 定位执行慢的 SQL：慢查询日志

### 3.0 介绍 

MySQL 的慢查询日志，用来**记录**在 MySQL 中**响应时间超过阀值的语句**，具体指运行时间超过 **long-query_time 值**的 SQL，则会被记录到慢查询日志中。 long_query_time 的默认值为 10，意思是运行 10 秒以上 (不含 10 秒) 的语句，认为是超出了我们的最大忍耐时间值。

它的主要作用是，帮助我们发现那些执行时间特别长的 SOL 查询，并且有针对性地进行优化，从而提高系统的整体效率。当我们的数据库服务器发生阻塞、运行变慢的时候，检查一下慢查询日志，找到那些慢查询，对解决问题很有帮助。比如一条 sq 执行超过 5 秒钟，我们就算慢 SQL，希望能收集超过 5 秒的 sql，结合 explain 进行全面分析

默认情况下，MySQL 数据库 没有开启慢查询日志 ，需要我们手动来设置这个参数。**如果不是调优需要的话，一般不建议启动该参数**，因为开启慢查询日志会或多或少带来一定的**性能影响**  
慢查询日志支持将日志记录写入文件 

### 3.1 开启慢查询日志参数

**0. 慢查询是否开启** 

```
[mysqld]
slow_query_log=OFF
```

![](<assets/1740030620098.png>)

慢查询日志位置：

```
[mysqld]
#slow_query_log =OFF
```

![](<assets/1740030620419.png>)

 慢查询阈值：

**1. 开启慢查询日志 slow_query_log**

```
SHOW VARIABLES LIKE '%slow%'; #查询慢查询日志所在目录
SHOW VARIABLES LIKE '%long_query_time%'; #查询超时时长
```

然后我们再来查看下慢查询日志是否开启，以及慢查询日志文件的位置：

![](<assets/1740030620619.png>)

你能看到这时慢查询分析已经开启，同时文件保存在 /var/lib/mysql/atguigu02-slow.log 文件

中。

**2. 修改慢查询阈值 long_query_time**

查看慢查询的时间阈值：

```
SHOW VARIABLES LIKE '%slow%';    #发现关闭成功
#慢查询阈值
SHOW VARIABLES LIKE '%long_query_time%';   #10s。前面改的时候没有加global，所以重启服务器后阈值恢复10s。
```

查看全局慢查询的时间阈值：

```
EXPLAIN SELECT select_options
#一般指定在查询时不使用缓存
EXPLAIN SELECT SQL_NO_CACHE select_options
#或者
DESCRIBE SELECT select_options
```

![](<assets/1740030620861.png>) 

**临时修改慢查询的时间阈值：** 

当前会话：

```
mysql> EXPLAIN FORMAT=tree SELECT * FROM s1 INNER JOIN s2 ON s1.key1 = s2.key2 WHERE
s1.common_field = 'a'\G
*************************** 1. row ***************************
EXPLAIN: -> Nested loop inner join (cost=1360.08 rows=990)
-> Filter: ((s1.common_field = 'a') and (s1.key1 is not null)) (cost=1013.75
rows=990)
-> Table scan on s1 (cost=1013.75 rows=9895)
-> Single-row index lookup on s2 using idx_key2 (key2=s1.key1), with index
condition: (cast(s1.key1 as double) = cast(s2.key2 as double)) (cost=0.25 rows=1)
1 row in set, 1 warning (0.00 sec)
```

全局：

```
mysql> EXPLAIN FORMAT=tree SELECT * FROM s1 INNER JOIN s2 ON s1.key1 = s2.key2 WHERE
s1.common_field = 'a'\G
*************************** 1. row ***************************
EXPLAIN: -> Nested loop inner join (cost=1360.08 rows=990)
-> Filter: ((s1.common_field = 'a') and (s1.key1 is not null)) (cost=1013.75
rows=990)
-> Table scan on s1 (cost=1013.75 rows=9895)
-> Single-row index lookup on s2 using idx_key2 (key2=s1.key1), with index
condition: (cast(s1.key1 as double) = cast(s2.key2 as double)) (cost=0.25 rows=1)
1 row in set, 1 warning (0.00 sec)
```

对于 “global” 选项，是全局级别的配置参数。它可以在 MySQL 服务器启动时或 MySQL 安装时在 MySQL 配置文件中设置，或者通过 SET GLOBAL 命令在运行时更改。全局级别的配置参数对所有的 MySQL 连接都有效。

永久修改（重启数据库后依然有效，**不建议永久修改**，仅在优化时候打开，慢查询拖性能） ：

修改 my.cnf

```
-- 删除test表（如果存在）
DROP TABLE IF EXISTS test;
 
-- 创建test表
CREATE TABLE test (
    id INT PRIMARY KEY AUTO_INCREMENT,
    a INT,
    b INT,
    c INT,
    d INT
);
 
-- 插入20条数据
INSERT INTO test (a, b, c, d) VALUES
(1, 10, 100, 1000),
(2, 20, 200, 2000),
(3, 30, 300, 3000),
(4, 40, 400, 4000),
(5, 50, 500, 5000),
(6, 60, 600, 6000),
(7, 70, 700, 7000),
(8, 80, 800, 8000),
(9, 90, 900, 9000),
(10, 100, 1000, 10000),
(11, 110, 1100, 11000),
(12, 120, 1200, 12000),
(13, 130, 1300, 13000),
(14, 140, 1400, 14000),
(15, 150, 1500, 15000),
(16, 160, 1600, 16000),
(17, 170, 1700, 17000),
(18, 180, 1800, 18000),
(19, 190, 1900, 19000),
(20, 200, 2000, 20000);
```

注意： 

```
-- 创建单列索引
CREATE INDEX idx_a ON test(a);
CREATE INDEX idx_b ON test(b);
CREATE INDEX idx_c ON test(c);
 
-- 创建复合索引
CREATE INDEX idx_a_b ON test(a, b);
CREATE INDEX idx_a_c ON test(a, c);
CREATE INDEX idx_a_b_c ON test(a, b, c);
```

![](<assets/1740030621097.png>)

### 3.2 查看慢查询次数

查询当前系统中有多少条慢查询记录

```
EXPLAIN SELECT
	a,b,c
FROM
	test 
WHERE
	a = 3 
	AND b = 5 
	AND c =4
```

### 3.5 慢查询日志分析工具：mysqldumpslow

`mysqldumpslow` 是一个用于分析 MySQL 慢查询日志的命令行工具。通过解析慢查询日志，可以了解到数据库的性能问题，从而进行优化。

**查看 mysqldumpslow 的帮助信息**

```
EXPLAIN SELECT
	*
FROM
	test 
WHERE
	a = 3 
	AND b = 5 
	AND c =4
```

mysqldumpslow 命令的具体参数如下：

*   -a: 不将数字抽象成 N，字符串抽象成 S
    
*   **-s: 是表示按照何种方式排序：**
    
    *   c: 访问次数
    *   l: 锁定时间
    *   r: 返回记录
    *   **t: 查询时间**
    *   al: 平均锁定时间
    *   ar: 平均返回记录数
    *   **at: 平均查询时间 （默认方式）**
    *   ac: 平均查询次数
*   **-t: 即为返回前面多少条的数据；**
    
*   -g: 后边搭配一个正则匹配模式，大小写不敏感的；
    

**案例：**

举例：按照查询时间排序，查看前五条 慢查询 SQL 语句，这样写即可：

```
INSERT INTO my_table (name, age) VALUES ('John Doe', 150);
SHOW WARNINGS;
```

![](<assets/1740030621368.png>)

举例，假设想查看最慢的 10 个查询，可以使用如下命令：

```
SET @trace_feature = 'qa';
SET @max_execution_time=50000;
SET @trace_level = '+ddl,+engine';
SET @trace_feature_check = 1;
SET @trace_unique_check = 1;
SET @trace_protocol = 1;
SET @trace_max_protocol = 6;
```

这条命令表示按照时间排序，显示最慢的 10 个查询，/var/log/mysql/mysql-slow.log 是 MySQL 慢查询日志的路径。

另外，如果需要筛选特定的查询，可以使用 `-g` 参数，比如：

```
SET global general_log = on;
SET global performance_schema = on;
```

这条命令表示按照时间排序，显示最慢的 10 个查询，其中关键字为 "SELECT * FROM user"。

总的来说，`mysqldumpslow` 命令提供了一种简单快捷的方式帮助开发人员、DBA 等分析 MySQL 慢查询问题，优化数据库的性能。

 **其他常用案例：**

```
SELECT *
FROM my_table
WHERE my_column = 'some_value';
SHOW SESSION STATUS LIKE 'Last_Query_Plan';
```

### 3.6 关闭慢查询日志

MySQL 服务器停止慢查询日志功能有两种方法：

**方式 1：永久性方式**

```
SET global general_log = off;
SET global performance_schema = off;
```

**mysql 默认关闭慢查询日志**，或者，把 slow_query_log 一项注释掉 或 删除

```
#1. 查询冗余索引
select * from sys.schema_redundant_indexes;
#2. 查询未使用过的索引
select * from sys.schema_unused_indexes;
#3. 查询索引的使用情况
select index_name,rows_selected,rows_inserted,rows_updated,rows_deleted
from sys.schema_index_statistics where table_schema='dbname';
```

重启 MySQL 服务，执行如下语句查询慢日志功能。

```
# 1. 查询表的访问量
select table_schema,table_name,sum(io_read_requests+io_write_requests) as io from
sys.schema_table_statistics group by table_schema,table_name order by io desc;
# 2. 查询占用bufferpool较多的表
select object_schema,object_name,allocated,data
from sys.innodb_buffer_stats_by_table order by allocated limit 10;
# 3. 查看表的全表扫描情况
select * from sys.statements_with_full_table_scans where db='dbname';
```

**方式 2：临时性方式**

使用 SET 语句来设置。

停止 MySQL 慢查询日志功能，具体 SQL 语句如下。

```
#1. 监控SQL执行的频率
select db,exec_count,query from sys.statement_analysis
order by exec_count desc;
#2. 监控使用了排序的SQL
select db,exec_count,first_seen,last_seen,query
from sys.statements_with_sorting limit 1;
#3. 监控使用了临时表或者磁盘临时表的SQL
select db,exec_count,tmp_tables,tmp_disk_tables,query
from sys.statement_analysis where tmp_tables>0 or tmp_disk_tables >0
order by (tmp_tables+tmp_disk_tables) desc;
```

golbal 全局有效。

**重启 MySQL 服务**，使用 SHOW 语句查询慢查询日志功能信息，发现慢查询日志已经关闭成功。

```
#1. 查看消耗磁盘IO的文件
select file,avg_read,avg_write,avg_read+avg_write as avg_io
from sys.io_global_by_file_by_bytes order by avg_read limit 10;
```

![](<assets/1740030621615.png>)

### 3.7 删除慢查询日志

**手动删除** 

使用 SHOW 语句显示慢查询日志信息，具体 SQL 语句如下。

```
#1. 行锁阻塞情况
select * from sys.innodb_lock_waits;
```

![](<assets/1740030621892.png>)

从执行结果可以看出，慢查询日志的目录默认为 MySQL 的数据目录，在该目录下 手动删除慢查询日志文件 即可。

**自动删除** 

使用命令 mysqladmin flush-logs 来重新生成查询日志文件，执行完毕会在数据目录下重新生成慢查询日志文件。

**重新生成慢查询日志文件（直接删除旧的）**

```
mysqladmin -uroot -p flush-logs slow
```

**提示**

慢查询日志都是使用 mysqladmin flush-logs 命令来**删除重建**的。使用时一定要注意，一旦执行了这个命令，慢查询日志都只存在新的日志文件中，如果需要旧的查询日志，就必须事先备份。

## 4. 定位慢查询语句、查看 SQL 执行成本：show profile

show profile 是 MySQL 提供的可以用来分析**当前会话中** **SQL 都做了什么、执行的资源消耗工具的情况**，可用于 sql 调优的测量。默认情况下处于关闭状态，并保存最近 15 次的运行结果。

`SHOW PROFILE` 是一个用于查看会话执行的查询的性能分析信息的 MySQL 命令。它可以帮助开发人员和 DBA **分析查询语句执行时的瓶颈**，并找出哪些部分需要优化。 

**查看配置是否开启 profile：** 

```
mysql > show variables like 'profiling';
```

*   **SHOW VARIABLES** 显示了 MySQL 服务器的当前配置变量，包括全局配置变量和会话配置变量，以及它们的值。SHOW VARIABLES 用于查看 MySQL 配置系统参数的详细信息并进行系统参数的修改。
*   SHOW STATUS 显示服务器的性能参数，包括连接、线程、查询等方面的状态信息，以及它们的值。 

![](<assets/1740030622076.png>)

**开启 show profile:**

```
mysql > set profiling = 'ON';
```

![](<assets/1740030622442.png>)

 然后执行相关的查询语句:

```
select * from employees
```

**show profiles; 查询当前会话所有查询语句持续时间**

```
mysql > show profiles;
```

![](<assets/1740030622757.png>)

 **show profile; 查询当前会话最近 sql 语句的执行成本：**

你能看到当前会话一共有 2 个查询。如果我们想要查看最近一次查询的开销，可以使用：

```
mysql > show profile;
```

![](<assets/1740030623032.png>)

**show profile cpu for 2; 查询指定 QueryID 的 cpu 信息：**

可以查看指定的 QueryID 的开销，比如 show profile for query 2 。在 SHOW PROFILE 中可以查看不同部分的开销，比如 cpu、block.io 等:

```
mysql> show profile cpu,block io for query 2
```

![](<assets/1740030623474.png>)

 **show profile 的常用查询参数：**

① ALL：显示所有的开销信息。

② BLOCK IO：显示块 IO 开销。

③ CONTEXT SWITCHES：上下文切换开销。

④ CPU：显示 CPU 开销信息。

⑤ IPC：显示发送和接收开销信息。

⑥ MEMORY：显示内存开销信 息。

⑦ PAGE FAULTS：显示页面错误开销信息。

⑧ SOURCE：显示和 Source_function，Source_file， Source_line 相关的开销信息。

⑨ SWAPS：显示交换次数开销信息。

**日常开发需注意：**

① `converting HEAP to MyISAM`: 查询结果太大，内存不够，数据往磁盘上搬了。

② `Creating tmp table`：创建临时表。先拷贝数据到临时表，用完后再删除临时表。

③ `Copying to tmp table on disk`：把内存中临时表复制到磁盘上，警惕！

④ `locked`。

如果在 show profile 诊断结果中出现了以上 4 条结果中的任何一条，则 sql 语句需要优化。

**注意：**

不过 **SHOW PROFILE 命令将被弃用**，我们可以从 **information_schema 中的 profiling 数据表**进行查看。

## 5. 执行计划表：EXPLAIN

### 5.1 简介

MySQL 的 EXPLAIN 是一种**分析 SQL 语句查询性能的工具**。当我们在 MySQL 中执行 SELECT 语句时，EXPLAIN 可以帮助我们**查看 MySQL 如何执行这个查询，即执行计划**，包括**使用哪些索引、选择哪些表、以及如何读取数据**等信息。通过分析 EXPLAIN 的输出结果，我们可以更好地**优化查询语句**，提高查询效率。

EXPLAIN 的使用方式非常简单，只需要在执行 **SELECT 语句**时在**前面加上 EXPLAIN 关键字**即可，例如：

```
EXPLAIN SELECT * FROM my_table WHERE my_column = 'my_value';
```

执行以上命令后，MySQL 会返回一张查询执行计划表，其中包含了 MySQL 执行这个查询的详细信息。我们可以通过分析查询执行计划表来**了解查询的性能瓶颈**，以及如何优化查询语句，从而提高查询性能。  

**注意：** 

*   EXPLAIN 不考虑各种 Cache
    
*   EXPLAIN 不能显示 MySQL 在执行查询时所作的优化工作
    
*   EXPLAIN 不会告诉你关于触发器、存储过程的信息或用户自定义函数对查询的影响情况
    
*   部分统计信息是估算的，并非精确值
    

**上一步用 show profile 定位了查询慢的 SQL 之后**，我们就可以使用 **EXPLAIN** 或 DESCRIBE 工具做针对性的**分析查询语句**。DESCRIBE 语句的使用方法与 EXPLAIN 语句是一样的，并且分析结果也是一样的。

**MySQL 中有专门负责优化 SELECT 语句的优化器模块**，主要功能: 通过计算分析系统中收集到的统计信息，为客户端请求的 Query 提供**它认为最优的 执行计划** (他认为最优的数据检索方式，但不见得是 DBA（数据库管理员）认为是最优的，这部分最耗费时间)。

这个**执行计划展示了接下来具体执行查询的方式**，比如多表连接的顺序是什么，对于每个表采用什么访问方法来具体执行查询等等。MySOL 为我们提供了 EXPLAIN 语句来帮助我们查看某个查询语句的具体执行计划，大家看懂 EXPLAIN 语句的各个输出项，可以有针对性的提升我们查询语句的性能。 

**1. 能做什么？**

*   表的读取顺序
*   数据读取操作的操作类型
*   哪些索引可以使用
*   哪些索引被实际使用
*   表之间的引用
*   每张表有多少行被优化器查询

**2. 官网介绍**

[MySQL :: MySQL 5.7 Reference Manual :: 8.8.2 EXPLAIN Output Format](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html "MySQL :: MySQL 5.7 Reference Manual :: 8.8.2 EXPLAIN Output Format")

[MySQL :: MySQL 8.0 Reference Manual :: 8.8.2 EXPLAIN Output Format](https://dev.mysql.com/doc/refman/8.0/en/explain-output.html "MySQL :: MySQL 8.0 Reference Manual :: 8.8.2 EXPLAIN Output Format")

**3. 版本情况**

*   **MySQL 5.6.3 以前只能 EXPLAIN SELECT** ；**MYSQL 5.6.3 以后**就可以 EXPLAIN SELECT，**UPDATE， DELETE**
*   在 5.7 以前的版本中，想要显示 partitions 需要使用 explain partitions 命令；想要显示 filtered 需要使用 explain extended 命令。在 5.7 版本后，默认 explain 直接显示 partitions 和 filtered 中的信息。

### 5.2 基本语法

EXPLAIN 或 DESCRIBE 语句的语法形式如下：

```
EXPLAIN SELECT select_options
#一般指定在查询时不使用缓存
EXPLAIN SELECT SQL_NO_CACHE select_options
#或者
DESCRIBE SELECT select_options
```

如果我们想看看某个查询的执行计划的话，可以在具体的查询语句前边加一个 EXPLAIN ，就像这样：

```
EXPLAIN SELECT SQL_NO_CACHE * FROM course_base;
```

![](<assets/1740030623713.png>)

输出的上述信息就是所谓的 执行计划。在这个执行计划的辅助下，我们需要知道应该怎样改进自己的查询语句以使查询执行起来更高效。其实除了以 SELECT 开头的查询语句，其余的 DELETE、INSERT、REPLACE 以及 UPDATE 语句等都可以加上 EXPLAIN，用来查看这些语句的执行计划，只是**平时我们对 SELECT 语句更感兴趣** 

### 5.3 执行计划表介绍 

#### **5.3.1 执行计划各个列的作用（概述）**

<table border="1" cellpadding="1" cellspacing="1"><tbody><tr><td>id</td><td>每个 SELECT 子句或者 join 操作都会被分配一个唯一的编号，编号越小优先级越高，id 相同的语句可以被认为是一组。id 为 NULL 表示独立的子查询，子查询优先级都比主查询高。</td></tr><tr><td>select_type</td><td>查询的类型。主查询 (primary)、普通查询 (simple)、联合查询、子查询 (subquery)、derived(from 表临时子查询)、union(union 后查询)、union result()</td></tr><tr><td>table</td><td>表名。显示当前这行的数据是哪个表的。</td></tr><tr><td>partitions</td><td>匹配的分区信息。如果表未分区则为 NULL。</td></tr><tr><td><strong>type</strong></td><td><strong>访问类型，根据索引、全表扫描等方法来执行查询的优化策略。</strong>all（全表扫描），ref（命中非唯一索引），index(没命中索引，扫描索引树再回表)、const（命中主键 / 唯一索引）、range(范围索引查询)、index_merge(使用多个索引)、&nbsp;system(一行记录时, 快速查询)。</td></tr><tr><td>possible_keys</td><td><p>可能用到的索引。列出 MySQL 能够使用哪些索引来查询。</p><p>如果该列只有一个 possible_keys，通常意味着这个查询是高效的。</p><p>如果这个列有多个 possible_keys，并且 MySQL 只使用了其中一个，则需要考虑是否需要在该列上增加一个联合索引。</p></td></tr><tr><td><strong>key</strong></td><td><strong>实际上使用的索引。</strong>如果没有明确的指定 KEY，MySQL 会根据查询条件自动选择最优的索引。</td></tr><tr><td><strong>key_len</strong></td><td>实际使用到索引的字节数长度。越短表示越快，一般表示索引字段越小越好。</td></tr><tr><td>ref</td><td>当使用索引列等值查询时，与索引列进行等值匹配的对象信息。常量等值查询 const, 表达式 / 函数使用到时 func, 关联查询显示关联字段名</td></tr><tr><td><strong>rows</strong></td><td>预估的需要读取的记录条数。数值越小越好，表示结果集越小，查询越高效。</td></tr><tr><td>filtered</td><td>某个表经过搜索条件过滤后剩余记录条数的百分比。这个值越小越好，说明可通过索引直接返回数据。</td></tr><tr><td><strong>Extra</strong></td><td>额外信息。看有没有走索引，还是全表扫描了。一般搭配 type 字段看。Using index(使用到覆盖索引)、Using where(未完全命中索引)、Using temporary(临时表存储结果集. 排序 / 分组会使用)、Using filesort(排序操作未用索引)、Using join buffer(连接条件未用索引)、Impossible where(where 约束语句可能有问题导致没有结果集)</td></tr></tbody></table>

#### **5.3.2 详细介绍**

##### **5.3.2.1 select_type**

**select_type：**查询的类型，有以下几种取值：

*   SIMPLE：不使用子查询或 UNION，不包含 UNION ALL 的简单 SELECT 查询。
*   PRIMARY：最外层的 SELECT 查询。
*   DERIVED：以 FROM 子句中的子查询方式出现的 SELECT 语句。
*   UNION：UNION 中的第二个或之后的 SELECT 查询。
*   UNION RESULT：从 UNION 的结果集中获取数据的 SELECT 查询。
*   SUBQUERY：不在 FROM 子句中出现的子查询，通常在 SELECT 语句中使用。
*   DEPENDENT SUBQUERY：子查询依赖外层查询的结果集。

##### **5.3.2.2 key**

**key：**实际上使用的索引。在 MySQL 中创建索引时使用的是 INDEX 关键字，但在 EXPLAIN 执行计划表中，显示的是 KEY，这是因为 **MySQL 允许在创建索引时指定统计信息**，例如最小值、最大值等，这些**统计信息**在索引中被视为**索引键**（Index key），所以在执行计划表中，显示为 KEY。

##### **5.3.2.3 type**

**type：**访问类型，根据索引、全表扫描等方法来执行查询的优化策略。当 type 列的取值不是 Const 时，我们需要重点关注有关索引、缓存的性能调优，对 SQL 语句进行优化，适当修复可能的数据设计问题。

*   **system：一行记录时, 快速查询。**只有一行数据即将被查询。这是最快的查询类型，通常出现在系统表的查询中。
*   **const：命中主键或唯一索引。**使用主键或唯一索引查找单个行时使用，此时查询只能返回一行数据。这是一种非常快的查询类型。
*   eq_ref：连接使用唯一索引查找符合查询条件的数据时使用，每个连接类型都需要使用唯一索引进行访问，比 ref 执行速度更快。
*   **ref：命中非唯一索引。**使用非唯一索引查找数据时使用，查询结果比 eq_ref 大，但仍很快。
*   **range：范围索引查询。**使用索引范围查找数据时使用，可能会查找一定范围内的数据，如使用 BETWEEN 或 > 或 > < 等操作时的查询。
*   **index：没命中索引，扫描非聚簇索引树再回表。**
    *   直接在某个索引树上做条件判断，并且不需要回表。全表扫描没有好的索引适用时使用，相比于全表扫描速度更快。
    *   index 是另外一种形式的全表扫描，扫描已有索引树然后回表取数据。和 all 相比，他要回表随机取数据，因此 index 不可能会比 all 快（取同一个表数据），官方手册说它的效率说的比 all 好，唯一可能的原因在于，按照索引扫描全表的数据是有序的。这样一来，结果不同，也就没法比效率的问题了。
    *   比如：select t3.key1 from t3 where t3.key2 =6 ; 当我们创建了联合索引 idx_key1_key2(key1,key2) 时，判断条件 key2=6 时，其虽然不满足索引的最左前缀原则，但是我们可以遍历 idx_key1_key2 这颗索引树，找到 key2=6 的记录即可。由于查询结果需要的 key1 在这个联合索引上，也不需要回表，此时就可以使用 index。
*   **all：全表扫描。**扫描整个表以获得需要的数据，速度最慢，必须尽量避免使用。
*   unique_subquery：在对查询结果进行过滤或使用 IN 操作时，优化器会选择使用此类型的查询，使用了 In 操作符的子查询依赖于外层查询的唯一索引。
*   index_subquery：使用了 In 操作符但子查询使用的普通索引，而不是唯一索引。
*   range_check：在使用索引来检查外键参照时使用。
*   index_merge：使用多个索引。

##### **5.3.2.4 Extra**

*   **using index：**使用了覆盖索引，即不需要回表。查询的几个列正好都在这个聚簇索引树上。覆盖索引参考：[MySQL 高级篇——覆盖索引、前缀索引、索引下推、SQL 优化、主键设计_mysql 前缀索引 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/130804019 "MySQL高级篇——覆盖索引、前缀索引、索引下推、SQL优化、主键设计_mysql前缀索引-CSDN博客")
*   **Using where：**通过 where 过滤。没完全命中索引，需要回表。例如 index(a)，查的是 where a=2 and b=3，查 b=3 时就要回表过滤。
*   **Using index condition：**使用了索引下推。
*   Using temporary：临时表存储结果集. 排序 / 分组会使用
*   Using filesort：排序操作未用索引
*   Using join buffer：连接条件未用索引
*   Impossible where：where 约束语句可能有问题导致没有结果集

### 5.4 EXPLAIN 四种输出格式

这里谈谈 EXPLAIN 的输出格式。EXPLAIN 可以输出四种格式： `传统格式` ，`JSON格式` ， `TREE格式` 以及 `可视化输出` 。用户可以根据需要选择适用于自己的格式。

1. 传统格式

传统格式简单明了，输出是一个表格形式，概要说明查询计划。

```
mysql> EXPLAIN SELECT s1.key1, s2.key1 FROM s1 LEFT JOIN s2 ON s1.key1 = s2.key1 WHERE s2.common_field IS NOT NULL;
```

2. JSON 格式

第 1 种格式中介绍的`EXPLAIN`语句输出中缺少了一个衡量执行好坏的重要属性 —— `成本`。而 JSON 格式是四种格式里面输出`信息最详尽`的格式，里面包含了执行的成本信息。

*   JSON 格式：在 EXPLAIN 单词和真正的查询语句中间加上 FORMAT=JSON 。

```
EXPLAIN FORMAT=JSON SELECT ....
```

*   id：查询所对应的唯一标识符（id）
*   select_type：查询类型（type）
*   table：正在访问的表（table_name）
*   partitions：正在访问的分区（partition_name）
*   type：使用的访问方法（access_type）
*   possible_keys：可能使用的索引（possible_keys）
*   key：实际使用的索引（key）
*   key_len：索引长度（key_length）
*   ref：与索引匹配的列或常数（ref）
*   rows：估计的检索行数（rows）
*   filtered：使用 WHERE 筛选后，剩余行数的百分比（filtered）
*   Extra：其他信息（extra）

3. TREE 格式

TREE 格式是 8.0.16 版本之后引入的新格式，主要根据查询的 `各个部分之间的关系` 和 `各部分的执行顺序` 来描述如何查询。

```
mysql> EXPLAIN FORMAT=tree SELECT * FROM s1 INNER JOIN s2 ON s1.key1 = s2.key2 WHERE
s1.common_field = 'a'\G
*************************** 1. row ***************************
EXPLAIN: -> Nested loop inner join (cost=1360.08 rows=990)
-> Filter: ((s1.common_field = 'a') and (s1.key1 is not null)) (cost=1013.75
rows=990)
-> Table scan on s1 (cost=1013.75 rows=9895)
-> Single-row index lookup on s2 using idx_key2 (key2=s1.key1), with index
condition: (cast(s1.key1 as double) = cast(s2.key2 as double)) (cost=0.25 rows=1)
1 row in set, 1 warning (0.00 sec)
```

4. 可视化输出

可视化输出，可以通过 MySQL Workbench 可视化查看 MySQL 的执行计划。通过点击 Workbench 的放大镜图标，即可生成可视化的查询计划。

3. TREE 格式

TREE 格式是 8.0.16 版本之后引入的新格式，主要根据查询的 `各个部分之间的关系` 和 `各部分的执行顺序` 来描述如何查询。

```
mysql> EXPLAIN FORMAT=tree SELECT * FROM s1 INNER JOIN s2 ON s1.key1 = s2.key2 WHERE
s1.common_field = 'a'\G
*************************** 1. row ***************************
EXPLAIN: -> Nested loop inner join (cost=1360.08 rows=990)
-> Filter: ((s1.common_field = 'a') and (s1.key1 is not null)) (cost=1013.75
rows=990)
-> Table scan on s1 (cost=1013.75 rows=9895)
-> Single-row index lookup on s2 using idx_key2 (key2=s1.key1), with index
condition: (cast(s1.key1 as double) = cast(s2.key2 as double)) (cost=0.25 rows=1)
1 row in set, 1 warning (0.00 sec)
```

4. 可视化输出

可视化输出，可以通过 MySQL Workbench 可视化查看 MySQL 的执行计划。通过点击 Workbench 的放大镜图标，即可生成可视化的查询计划。

![](<assets/1740030623950.png>)

上图按从左到右的连接顺序显示表。红色框表示 ` 全表扫描 ` ，而绿色框表示使用 ` 索引查找 ` 。对于每个表， 显示使用的索引。还要注意的是，每个表格的框上方是每个表访问所发现的行数的估计值以及访问该表的成本。 

### 5.6 自测练习

#### 5.6**.1 练习题和答案解析**

看不懂看参考：

[MySQL 高级篇——性能分析工具_mysql 分析工具 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/130355955 "MySQL高级篇——性能分析工具_mysql分析工具-CSDN博客")

**假设：**表 a,b,c,d 四个字段，创建索引 (a),(b),(c),(a,b),(a,c)(a,b,c)

```
explain select * from test + 下面条件
```

<table><thead><tr><th>Where 语句</th><th>索引是否被使用</th><th>使用到哪些列</th></tr></thead><tbody><tr><td>where a = 3</td><td><p>Y。命中非唯一索引。</p><p>如果是 select a，则命中覆盖索引</p></td><td>a, b, c</td></tr><tr><td>where a = 3 and b =5</td><td>Y。命中非唯一索引。<p>如果是 select a,b，则命中覆盖索引</p></td><td>a, b, c</td></tr><tr><td>where a = 3 and b = 5 and c =4</td><td>Y。命中非唯一索引。<p>如果是 select a,b,c，则命中覆盖索引</p></td><td>a, b, c</td></tr><tr><td>where b=3</td><td>N。不符合最佳左前缀原则</td><td></td></tr><tr><td>where b=3 and c= 4</td><td>Y。没完全命中索引，即 using where</td><td>b</td></tr><tr><td>where d = 4</td><td>N。没命中索引，不符合最佳左前缀原则</td><td></td></tr><tr><td>where c = 3 and d =5</td><td>Y。没完全命中索引</td><td>c</td></tr><tr><td>where a = 3 and b&gt; 4 and c= 5</td><td><p>Y。范围查询 + 索引下推</p><p>（idx_abc 树上 a 精准, b 范围过滤后条件判断 c，</p><p>然后回表查 a,b,c,d）</p></td><td>a, b, c</td></tr><tr><td>where a is null and b is not null</td><td><p>Y。索引下推。如果是 select a,b,c,，</p><p>则 Using where; Using index，没完全命中索引，</p><p>is not null 导致索引失效</p></td><td>a, b, c</td></tr><tr><td>where a &lt;&gt;3</td><td><p>N。全表扫描。</p><p>没覆盖索引时，不等于导致索引失效</p></td><td></td></tr><tr><td>where abs(a)=3</td><td>N。全表扫描。函数导致索引失效</td><td></td></tr><tr><td><p><strong>b 改为 varchar 类型后：</strong></p><p>where a =3 and b like 'kk%' and c=4</p></td><td>Y。范围查询 + 索引下推</td><td>a, b, c</td></tr><tr><td><p>where a = 3 and b like 'kk' and c = 4</p></td><td><p>Y。索引下推。</p><p>注意 like 后没有通配符所以不是范围查询</p></td><td>a, b, c</td></tr><tr><td>where a = 3 and b like '%kk%' and c= 4</td><td>Y。索引下推。如果是 select a,b,c,，<p>则 Using where; Using index，没完全命中索引，</p><p>左模糊查询导致索引失效。</p></td><td>a, b, c</td></tr><tr><td>where a = 3 and b like 'k%kk%'</td><td>Y。索引下推。</td><td>a, b, c</td></tr></tbody></table>

#### 5.6**.2 验证**

##### 5.6.2.1 准备数据 

准备表和数据： 

```
-- 删除test表（如果存在）
DROP TABLE IF EXISTS test;
 
-- 创建test表
CREATE TABLE test (
    id INT PRIMARY KEY AUTO_INCREMENT,
    a INT,
    b INT,
    c INT,
    d INT
);
 
-- 插入20条数据
INSERT INTO test (a, b, c, d) VALUES
(1, 10, 100, 1000),
(2, 20, 200, 2000),
(3, 30, 300, 3000),
(4, 40, 400, 4000),
(5, 50, 500, 5000),
(6, 60, 600, 6000),
(7, 70, 700, 7000),
(8, 80, 800, 8000),
(9, 90, 900, 9000),
(10, 100, 1000, 10000),
(11, 110, 1100, 11000),
(12, 120, 1200, 12000),
(13, 130, 1300, 13000),
(14, 140, 1400, 14000),
(15, 150, 1500, 15000),
(16, 160, 1600, 16000),
(17, 170, 1700, 17000),
(18, 180, 1800, 18000),
(19, 190, 1900, 19000),
(20, 200, 2000, 20000);
```

创建索引：

```
-- 创建单列索引
CREATE INDEX idx_a ON test(a);
CREATE INDEX idx_b ON test(b);
CREATE INDEX idx_c ON test(c);
 
-- 创建复合索引
CREATE INDEX idx_a_b ON test(a, b);
CREATE INDEX idx_a_c ON test(a, c);
CREATE INDEX idx_a_b_c ON test(a, b, c);
```

查看所有索引：

```
SHOW INDEX FROM test;
```

##### 5.6.2.2 **测试 1：命中**覆盖索引

```
EXPLAIN SELECT
	a,b,c
FROM
	test 
WHERE
	a = 3 
	AND b = 5 
	AND c =4
```

![](<assets/1740030624251.png>)

**解读：**

*   **ref：**命中了非唯一索引；
*   **using index：**使用了覆盖索引，即不需要回表；
*   **key 是 idx_a_b_c：**命中了 idx_a_b_c 这个索引

##### 5.6.2.3 测试 2：命中索引，需要回表

```
EXPLAIN SELECT
	*
FROM
	test 
WHERE
	a = 3 
	AND b = 5 
	AND c =4
```

![](<assets/1740030624438.png>)

**解读：**

*   **ref：**命中了非唯一索引；
*   **key 是 idx_a_b_c：**命中了 idx_a_b_c 这个索引

### 5.5 SHOW WARNINGS 的使用

在 MySQL 中，SHOW WARNINGS 是一个可以查看最近一次执行的语句中产生的警告信息的命令。当 MySQL 执行语句时，如果发现一些不符合预期的情况，会产生一些警告信息。这些警告信息可以包括非致命性错误，例如某些类型的数据不能隐式转换或某些数据截断等。

当我们执行 SHOW WARNINGS 命令时，MySQL 会返回警告信息的详细列表，包括：

*   Warning：该警告的类型
*   Level：该警告的级别，通常为 Note、Warning 或 Error
*   Code：警告的返回代码
*   Message：警告信息的内容

可以使用 SELECT 的方式来查看最近一次操作的警告信息：

```
SHOW WARNINGS;
```

也可以配合使用 INSERT、UPDATE、DELETE、ALTER TABLE 等命令，检查某个具体操作产生的警告信息：

```
INSERT INTO my_table (name, age) VALUES ('John Doe', 150);
SHOW WARNINGS;
```

在开发和调试的过程中，SHOW WARNINGS 对于定位和解决某些问题非常有用，例如数据截断、类型转换等问题。

## 6. 分析优化器执行计划：trace

在 MySQL 中，可以使用 trace 命令来进行优化器执行计划的跟踪和分析。trace 命令可以显示 MySQL 优化器在生成执行计划时所采取的决策，包括哪些表被处理，以及使用哪些索引、算法等。

使用 trace 命令需要先启用 general_log 和 performance_schema 两个系统变量，其次需要使用 SET 语句来设置一些参数，例如 trace-unique-check、trace-max-protocol、trace-protocol、trace-feature、trace-feature-check 等。设置完成后，可以通过 SET global trace_format='json'语句来选择输出结果的格式。

下面是对使用 trace 命令的一个简单示例：

首先，设置参数：

```
SET @trace_feature = 'qa';
SET @max_execution_time=50000;
SET @trace_level = '+ddl,+engine';
SET @trace_feature_check = 1;
SET @trace_unique_check = 1;
SET @trace_protocol = 1;
SET @trace_max_protocol = 6;
```

然后，启用 general_log 和 performance_schema：

```
SET global general_log = on;
SET global performance_schema = on;
```

接着，执行查询并查看结果：

```
SELECT *
FROM my_table
WHERE my_column = 'some_value';
SHOW SESSION STATUS LIKE 'Last_Query_Plan';
```

最后，关闭 general_log 和 performance_schema：

```
SET global general_log = off;
SET global performance_schema = off;
```

在 trace 输出中，我们可以看到优化器在执行计划中使用的索引、执行算法、行数估计等细节信息。通过分析 trace 结果，我们可以找到一些性能问题的根源，并进行相应的调整和优化。但要注意，trace 命令可能会带来额外的性能消耗和 IO 开销，不应该在生产环境中长期启用。

## 7. MySQL 监控分析视图 - sys schema

### 7.1 简介

MySQL 在 8.0 版本引入了 sys schema，该模式包含用于监视和分析 MySQL 服务器性能的视图和函数。sys schema 提供了一组易于使用的视图和函数，可以帮助我们更好地理解和分析 MySQL 数据库的行为和性能。

以下是 sys schema 中一些常用的监控分析视图：

*   sys.statements_with_sorting: 显示哪些语句使用了排序操作，包括使用哪些排序操作、每个语句排序的次数以及排序操作的资源消耗。
*   sys.statements_with_runtimes_in_95th_percentile: 显示执行时间最长的语句。
*   sys.io_global_by_file_by_bytes: 显示每个文件的磁盘 IO 字节数，可以用来检测 IO 瓶颈。
*   sys.memory_by_host_by_current_bytes: 显示每个客户端的当前内存使用情况，可以用于检测内存泄漏或内存占用高的情况。
*   sys.waits_global_by_latency: 显示哪些等待操作最耗费时间，可以帮助我们找到性能问题的瓶颈所在。
*   sys.processlist: 显示当前正在运行的线程和进程的信息，包括执行的语句、查询 ID、用户、主机、线程 ID 和状态等信息。

总的来说，sys schema 中包含的视图和函数为我们提供了更深入的 MySQL 性能分析和监控功能，可以帮助我们更好地理解 MySQL 数据库的行为和性能瓶颈。 

1.  **主机相关**：以 host_summary 开头，主要汇总了 IO 延迟的信息。
2.  **Innodb 相关**：以 innodb 开头，汇总了 innodb buffer 信息和事务等待 innodb 锁的信息。
3.  **I/o 相关**：以 io 开头，汇总了等待 I/O、I/O 使用量情况。
4.  **内存使用情况**：以 memory 开头，从主机、线程、事件等角度展示内存的使用情况
5.  **连接与会话信息**：processlist 和 session 相关视图，总结了会话相关信息。
6.  **表相关**：以 schema_table 开头的视图，展示了表的统计信息。
7.  **索引信息**：统计了索引的使用情况，包含冗余索引和未使用的索引情况。
8.  **语句相关**：以 statement 开头，包含执行全表扫描、使用临时表、排序等的语句信息。
9.  **用户相关**：以 user 开头的视图，统计了用户使用的文件 I/O、执行语句统计信息。
10.  **等待事件相关信息**：以 wait 开头，展示等待事件的延迟情况。

### 7.2 使用场景

索引情况

```
#1. 查询冗余索引
select * from sys.schema_redundant_indexes;
#2. 查询未使用过的索引
select * from sys.schema_unused_indexes;
#3. 查询索引的使用情况
select index_name,rows_selected,rows_inserted,rows_updated,rows_deleted
from sys.schema_index_statistics where table_schema='dbname';
```

表相关

```
# 1. 查询表的访问量
select table_schema,table_name,sum(io_read_requests+io_write_requests) as io from
sys.schema_table_statistics group by table_schema,table_name order by io desc;
# 2. 查询占用bufferpool较多的表
select object_schema,object_name,allocated,data
from sys.innodb_buffer_stats_by_table order by allocated limit 10;
# 3. 查看表的全表扫描情况
select * from sys.statements_with_full_table_scans where db='dbname';
```

语句相关

```
#1. 监控SQL执行的频率
select db,exec_count,query from sys.statement_analysis
order by exec_count desc;
#2. 监控使用了排序的SQL
select db,exec_count,first_seen,last_seen,query
from sys.statements_with_sorting limit 1;
#3. 监控使用了临时表或者磁盘临时表的SQL
select db,exec_count,tmp_tables,tmp_disk_tables,query
from sys.statement_analysis where tmp_tables>0 or tmp_disk_tables >0
order by (tmp_tables+tmp_disk_tables) desc;
```

IO 相关

```
#1. 查看消耗磁盘IO的文件
select file,avg_read,avg_write,avg_read+avg_write as avg_io
from sys.io_global_by_file_by_bytes order by avg_read limit 10;
```

Innodb 相关

```
#1. 行锁阻塞情况
select * from sys.innodb_lock_waits;
```