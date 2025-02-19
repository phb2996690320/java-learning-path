> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_43254100/article/details/131720004)

#### 文章目录

*   [存储引擎](#_1)
*   *   [MySQL 体系结构：连接层，Server 层，引擎层，存储层](#MySQLServer_3)
    *   [存储引擎](#_15)
    *   *   [存储引擎：InnoDB(MySQL 5.5 后默认的存储引擎)](#InnoDBMySQL_55_37)
        *   [存储引擎：MyISAM (MySQL 早期默认存储引擎)](#MyISAM_MySQL_68)
        *   [存储引擎：Memory](#Memory_85)
        *   [InnoDB, MyISAM, Memory 的区别，使用场景](#InnoDB_MyISAM_Memory_103)
*   [索引](#_132)
*   *   [索引结构：B+Tree, Hash, R-tree, Full-text](#BTree__Hash__Rtree__Fulltext_148)
    *   [索引分类：主键 / 唯一 / 常规 / 全文索引，聚集 / 二级索引](#_254)
    *   [索引语法：索引的创建，查看，删除](#_303)
    *   [SQL 性能分析：执行频率，慢查询日志，profile，explain](#SQLprofileexplain_322)
    *   [索引使用：单列索引，联合索引，前缀索引](#_425)
    *   [索引使用：验证索引效率，索引失效，SQL 提示，覆盖索引](#SQL_489)
    *   [索引设计原则](#_559)
*   [SQL 优化](#SQL_571)
*   [视图 / 存储过程 / 存储函数 / 触发器](#_726)
*   *   [视图](#_731)
    *   [存储过程](#_798)
    *   [存储函数](#_1132)
    *   [触发器 Triggers](#_Triggers_1165)
*   [锁](#_1196)
*   *   [全局锁](#_1202)
    *   [表级锁](#_1233)
    *   [行级锁](#_1286)
    *   [锁总结](#_1374)
*   [InnoDB 引擎](#InnoDB_1399)
*   *   [逻辑存储结构](#_1404)
    *   [架构——内存结构](#_1429)
    *   [架构——磁盘结构](#_1473)
    *   [事务原理](#_1545)
    *   [MVCC](#MVCC_1589)

存储引擎
----

### MySQL 体系结构：连接层，Server 层，引擎层，存储层

*   连接层：连接层负责处理客户端与 MySQL 服务器之间的连接和通信。它接收客户端的连接请求，并建立与客户端的网络连接。
*   Server 层：包括连接器、查询缓存、分析器、优化器、执行器等，涵盖 MySQL 的大多数核心服务功能，以及所有的内置函数（如日期、时间、数学和加密函数等），所有跨存储引擎的功能都在这一层实现，比如存储过程、触发器、视图等。
*   引擎层：负责数据的存储和检索。架构模式是插件式，服务器通过 API 和存储引擎进行通信。支持 InnoDB、MyISAM、Memory 等多个存储引擎。
*   存储层：MYSQL 的物理存储部分，负责将数据 (如: redolog、undolog、数据、索引、二进制日志、错误日志、查询 日志、慢查询日志等) 存储在磁盘上。

![](https://i-blog.csdnimg.cn/blog_migrate/272133a2156e72c3ca95113300b4b0d2.png#pic_center)  
（小林 coding 的 MySQL 体系结构：）  
![](https://i-blog.csdnimg.cn/blog_migrate/ccbe9034b38a54833e39f15fdb0e6d47.png#pic_center)

### 存储引擎

存储引擎是**存储数据、建立索引、更新 / 查询数据**等技术的实现方式 。存储引擎是**基于表**的，而不是 基于库的，所以存储引擎也可被称为表类型。可以在创建表的时指定选择的存储引擎，没有指定将自动选择默认的存储引擎。

*   存储引擎的查看和指定
    
    *   使用`show create table 表名;` 查看[建表语句](https://so.csdn.net/so/search?q=%E5%BB%BA%E8%A1%A8%E8%AF%AD%E5%8F%A5&spm=1001.2101.3001.7020)，可以看到当前表所使用的存储引擎
        
    *   在创建表时，指定存储引擎
        
        ```
        CREATE TABLE 表名(
        字段1 字段1类型 [ COMMENT 字段1注释 ] ,
        ......
        字段n 字段n类型 [COMMENT 字段n注释 ]
        ) ENGINE = INNODB [ COMMENT 表注释 ] ;
        
        ```
        
    *   查询当前数据库支持的存储引擎 `show engines;`
        

#### 存储引擎：InnoDB(MySQL 5.5 后默认的存储引擎)

**特点：**

*   DML 操作遵循 ACID 模型，支持事务；
*   行级锁，提高并发访问性能；
*   支持外键 FOREIGN KEY 约束，保证数据的完整性和正确性；

**文件：**

*   xxx.ibd 存储表结构（frm - 早期的 、sdi - 新版的）、数据和索引）
    
    xxx 是表名，innoDB 引擎每张表都会对应一个表空间文件  
    ![](https://i-blog.csdnimg.cn/blog_migrate/4335d27a4560d7df38dfa223ea1c4d68.png#pic_center)
    
*   `show variables like 'innodb_file_per_table';` 参数代表是否时一张表对应一个文件
    
    MySQL 8.0 版本以后默认打开  
    ![](https://i-blog.csdnimg.cn/blog_migrate/63533e57ae7405de79713cbaa4be4637.png#pic_center)
    

**逻辑存储结构：**

表空间 -> 段 -> 区 -> 页 -> 行

一区可以有 64 个连续的页  
![](https://i-blog.csdnimg.cn/blog_migrate/05ddba87dbb369b92cb32f072c593967.png#pic_center)

#### 存储引擎：MyISAM (MySQL 早期默认存储引擎)

**特点：**

*   不支持事务，不支持外键
*   支持表锁，不支持行锁
*   优点：更少的存储空间，支持全文索引，适用于读取频率较高、写入频率较低的应用场景

**文件：**

*   xxx.sdi： 存储**表结构信息**
*   xxx.MYD: 存储**数据**
*   xxx.MYI: 存储**索引**

![](https://i-blog.csdnimg.cn/blog_migrate/0162b1a79e7f2eb3bfe0b0ba4739b5bc.png#pic_center)

#### 存储引擎：Memory

Memory 引擎的表数据时存储在内存中，若受到硬件问题、或断电问题的影响，表中数据消失，一般用于临时缓存使用

**特点：**

*   内存存放，访问速度较快
*   hash 索引（默认）

**文件：**

*   xxx.sdi：存储**表结构信息**
*   数据， 都在内存中

#### InnoDB, MyISAM, Memory 的区别，使用场景

区别：

![](https://i-blog.csdnimg.cn/blog_migrate/0fb3844e83912d08779b1b8a1ab51f24.png#pic_center)

> **InnoDB 引擎与 MyISAM 引擎的区别 ?**
> 
> *   InnoDB 支持事务，而 MyISAM 不支持
> *   InnoDB 支持行锁和表锁，而 MyISAM 仅支持表锁, 不支持行锁
> *   InnoDB 支持外键, 而 MyISAM 不支持

适合的使用场景：

InnoDB：对事务的完整性有高要求，且在并发条件下要求数据的一致性，数据操作包含很多更删操作

MyISAM ： 以读操作和插入操作为主，有少量更新、删除操作，且对事务完整性、并发性要求不高

MEMORY：数据保存在内存中，访问速度快，用于临时表及缓存。缺陷就是 对表的大小有限制，太大的表无法缓存在内存中，且断电后数据消失 无法保障数据的安全性。

MyISAM 和 MEMORY 被 MongoDB 和 Redies 等 NoSQL 的 DBMS 所取代

章节总结：

![](https://i-blog.csdnimg.cn/blog_migrate/d4ecc325d4f1c84064afaf516383cb7c.png#pic_center)

索引
--

索引（index）是 MySQL 中高效获取数据的树结构（有序）

![](https://i-blog.csdnimg.cn/blog_migrate/40c81bd2db81dab60339a59238cbf784.png#pic_center)

索引优缺点：

*   优点：
    *   提高数据检索效率，降低数据库的 I/O 成本
    *   通过索引对数据进行排序，降低数据排序的成本，降低 CPU 消耗
*   缺点：
    *   索引占用了数据库的空间 （磁盘便宜
    *   索引提高了查询的效率，降低了更新表（Insert，update，delete）的速度，因为增删改表也需要同时维护索引。 （查询的次数远大于增删改操作的次数

### 索引结构：B+Tree, Hash, R-tree, Full-text

MySQL 的索引是在存储引擎层中实现的，不同的存储引擎有不同的索引结构：

*   **B+Tree 索引**：最常见的索引类型，大部分引擎都支持 B+ 树索引
*   **Hash 索引**：底层数据结构是用哈希表实现的, 只有精确匹配索引列的查询才有效，不支持范围查询
*   **R-tree(空间索引）**：是 MyISAM 引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少
*   **Full-text(全文 索引)**：是一种通过建立倒排索引, 快速匹配文档的方式。

![](https://i-blog.csdnimg.cn/blog_migrate/475af22e29ab502b621579c2b032f4be.png#pic_center)

**哈希：**

采用 hash 算法 将键 key 换算成新的 hash 值，映射到对应槽位上，存储在 hash 表中。若产生冲突，可以采用**拉链法**（相同的 key 往后延申成链表），**线性探测法**（逐个找哈希表中的空闲位置），二次探测法（以二次函数为步长找哈希表中空闲位置），**双重哈希法**（对 key 再计算一次 hash 值）等解决冲突

![](https://i-blog.csdnimg.cn/blog_migrate/f786884774e5f5ad037a1ed01bf36134.png#pic_center)

特点：

*   只能用于等值查询较 (=，in)，不支持范围查询（between，>，< ，…）
*   无法利用索引完成排序操作
*   查询效率高，通常 (不存在 hash 冲突的情况) 只需要一次检索就可以了，效率通常要高于 B+tree 索引

存储引擎支持：

在 MySQL 中，支持 hash 索引的是 Memory 存储引擎。 而 InnoDB 中具有自适应 hash 功能，hash 索引是 InnoDB 存储引擎根据 B+Tree 索引在指定条件下自动构建的

**二叉树：**

![](https://i-blog.csdnimg.cn/blog_migrate/7e04aaca3958d6ff560b1980bc489318.png#pic_center)  
如果主键顺序插入：![](https://i-blog.csdnimg.cn/blog_migrate/c5cdfd3920a3f5eadc1fc577cfadafe9.png#pic_center)

*   顺序插入时，会形成一个单项链表，查询性能大大降低
*   大数据量情况下，层级较深，检索速度慢

**红黑树：**

![](https://i-blog.csdnimg.cn/blog_migrate/7b57d601f7b51eaaf3db19eca283da91.png#pic_center)

*   解决二叉树的顺序插入后，树不平衡的问题
*   在大数据量的情况下，层级较深，检索速度较慢

**B-Tree 多路平衡查找树：**

以一颗最大度数为 5 的 b-tree 为例：

*   五个指针： 指针 1（key<20），指针 2（20<key<30），指针 3（30<key<62），
    
    指针 4（62<key<89），指针 5（key>89）
    
*   四个 key：20,30,62,89
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/0fe1eb0c82bf734a96194a4af1863d0f.png)
    
    树的度数指一个结点的子节点个数
    
    中间元素向上分裂
    
*   一个结点可以包含 2 个以上的子节点，解决了红黑树 层级较深的问题
    
*   B 树中，非叶子节点和叶子结点都会存放数据，一页中可以放的指针和数据太少，IO 次数变多
    

**B + 树：**

以一个最大度数为 4 的 b + 树为例：

度数为 4，key 有三个，指针有四个

![](https://i-blog.csdnimg.cn/blog_migrate/77fc42c24a060cf7006de3db4193cbc3.png)

*   索引部分：仅仅起到索引数据的作用，不存储数据
*   数据存储部分，在其叶子节点中要存储具体的数据

与 B-Tree 相比，区别：

*   所有的数据都会出现在叶子节点。
*   叶子节点形成一个单向链表。
*   非叶子节点仅仅起到索引数据作用，具体的数据都是在叶子节点存放的。

**MySQL 优化后的 B+ Tree：**

注意：橙色的是页 / 块，存储索引和数据

![](https://i-blog.csdnimg.cn/blog_migrate/4a6f5a698aa9090e68fdfb8bdf62563d.png)

*   在原 B+Tree 的基础上，变成了双向循环链表，形成了带有顺序指针的 B+Tree
    
    > **为什么 InnoDB 存储引擎选择使用 B+tree 索引结构?**
    > 
    > *   **相对于二叉树，层级更少，搜索效率高；**
    > *   **对于 B-tree，无论是叶子节点还是非叶子节点，都会保存数据，这样导致一页中存储 的键值减少，指针跟着减少，要同样保存大量数据，只能增加树的高度，导致性能降低；**
    > *   **相对 Hash 索引，B+tree 支持范围匹配及排序操作；**
    

### 索引分类：主键 / 唯一 / 常规 / 全文索引，聚集 / 二级索引

主键索引，唯一索引，常规索引，全文索引

聚集索引，二级索引

联合索引（创建索引时的字段顺序，注意：最左前缀法则：where 后面有最左列，且中间跳过的话 后面的列都失效），前缀索引（根据索引的选择性，选出前缀长度 n，create index column_idx on tablename(column(n)); ），单列索引

![](https://i-blog.csdnimg.cn/blog_migrate/129a6a12a505dd1348b191acc592a151.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/a5a626681232340af4bcdc9f0739e3c7.png)

*   如果存在主键，主键索引就是聚集索引
    
*   如果不存在主键，将使用第一个唯一（UNIQUE）索引作为聚集索引。 （在字段上加了唯一约束的时候，会自动加上该字段的唯一索引
    
*   如果表没有主键，或没有合适的唯一索引，则 InnoDB 会自动生成一个 rowid 作为隐藏的聚集索引
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/d21eff15352a444ab3762d35a77aee80.png)
    
    > **以下哪个 SQL 语句的执行效率会更高，为什么？（id 为主键，name 字段创建的有索引**
    > 
    > ​ ① `select * from user where id = 10;`
    > 
    > ​ ② `select * from user where name = 'Arm';`
    > 
    > 答：语句①只需要一次索引扫描，语句②需要先查找主键 再回表使用聚集索引获取一整行数据
    > 
    > ​ 因此语句①的执行效率会更高
    > 
    > **InnoDB 主键索引的 B+Tree 高度为多高？**
    > 
    > 答：假设一行数据大小 1k，一页大小为 16K 可以存储 16 行这样的数据，InnoDB 指针占用 6 个字节空间空间，假设 key 占用 8 个字节，
    > 
    > **树高度为 2**：
    > 
    > 非叶子结点页存储 key 的数量：n * 8 + (n + 1) * 6 = 16 * 1024, n = 1170，key 1170 个, 指针 1171 个
    > 
    > 一个指针指向一个叶子结点的页，一页能存储 16 行，所以可以存放 1171 * 16 = 18736
    > 
    > 树高度为 3：
    > 
    > `1171 * 1171 * 16 = 273,993,856` 可以存放百万级别的数据
    

### 索引语法：索引的创建，查看，删除

*   创建索引
    
    ```
    CREATE [UNIQUE | FULLTEXT] INDEX index_name ON table_name (index_col_name, ...) ;
    #index_name 索引名；index_col_name, ...多个字段 
    
    ```
    
    如果一个索引只关联一个字段，则该索引称为单列索引
    
    如果一个索引关联多个字段，则该索引称为 联合索引 / 组合索引
    
*   查看该表的所有索引 `SHOW INDEX FROM table_name ;`
    
*   删除该表的`indext_name`的索引 `DROP INDEX index_name ON table_name ;`
    

### SQL 性能分析：执行频率，慢查询日志，profile，explain

时间优化：

show global status like ‘Com_______’;

sudo tail -f /var/log/mysql/mysql-slow.log;

show profiles;

顺序优化：

explain select * from … 查询语句

分析结果的 Extra 信息，判断语句是否使用了索引

*   **SQL 执行频率：使用命令查看全局 / 当前会话的增删改查 次数 (7 个下划线**
    
    *   `show [session|global] status like 'Com_______';` 可以提供服务器增删改查的访问频次
        
        主要对于查询较多的数据库的数据库进行优化，若以增删改为主 考虑不对其进行索引优化
        
        session 当前会话的，golbal 全局的
        
*   **使用慢查询日志定位查询效率较低的 SQL 语句，从而对单个 SQL 语句进行优化**
    
    *   慢查询日志：记录了所有执行时间超过指定参数（long_query_time，单位：秒，默认 10 秒）的所有 SQL 语句的日志
        
        *   查看 MySQL 的慢查询日志是否开启，默认关闭：`show variables like 'slow_query_log';`
            
        *   配置文件`sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf`
            
        *   去掉 mysqld.cnf 中，`slow_query_log和long_query_time` 前面的注释符
            
        *   `show variables like 'slow_query_log';`重启 MySQL，采命令查看慢查询日志是否已开
            
    *   尝试使用慢查询日志：
        
        *   一个进程执行 `sudo tail -f /var/log/mysql/mysql-slow.log`查看日志文件的尾行
        *   一个进程进入 MySQL 执行 `SELECT BENCHMARK(1000000000, 1+1);`进行长时间的压测
*   **profile** 查看指令耗时。执行指令时，当查询时间超过设置时间后才会写慢查询入日志，但有些 SQL 语句任务简单 时间在超过的设置时间左右 是不合理的，如何发现这类 SQL 语句 **可以使用 profile 在做 SQL 优化时帮助我们了解时间耗费情况**
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/5360b4ec38bc268e6076f7f48ca62ade.png#pic_center)
    
    *   查看当前数据库是否支持 profiles `select @@have_profiling ;`
        
    *   查看当前数据库是否打开了 profiling `select @@profiling;`
        
    *   开启 profiling `SET profiling = 1;`
        
    *   使用指令查看当前会话指令的执行耗时
        
        *   `show profiles;` 查看每一条 SQL 的耗时基本情况
        *   `show profile for query query_id;` 查看指定 query_id 的 SQL 语句各个阶段的耗时情况
        *   `show profile cpu for query query_id;` 查看指定 query_id 的 SQL 语句 CPU 的使用情况
*   **explain 执行计划：其它的都是关于时间上的优化，explain 是关于执行顺序的优化**。
    
    EXPLAIN 或 DESC 命令获取 MySQL 如何执行 SELECT 语句的信息，包括在 SELECT 语句执行过程中表如何连接和连接的顺序。
    
    `explain / desc EXPLAIN SELECT 字段列表 FROM 表名 WHERE 条件 ;`
    
    直接在 select 语句之前加上关键字
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/e3f24b5e6034fcb78b818294c0011120.png)
    
    Explain 获取的信息：
    
    *   id：select 查询的序列号，表示查询中执行 select 子句或者是操作表的顺序
        
        *   id 相同，执行顺序从上到下
        *   d 不同，值越大，越先执行)。
    *   select_type：表示 SELECT 的类型，常见的取值有:
        
        *   SIMPLE，简单表，即不使用表连接 或者子查询
        *   PRIMARY，主查询，即外层的查询
        *   UNION，UNION 中的第二个或者后面的查询语句
        *   SUBQUERY，SELECT/WHERE 之后包含了子查询）
    *   **type：表示连接类型，性能由好到差的连接类型为：**
        
        **NULL、system、const、 eq_ref、ref、range、 index、all**
        
    *   **possible_key：显示可能应用在这张表上的索引，一个或多个**
        
    *   **key：实际使用的索引，如果为 NULL，则没有使用索引。**
        
    *   **key_len：表示索引字段最大可能长度，并非实际长度，不损失精确性的前提下越短越好**
        
    *   rows：MySQL 认为必须要执行查询的行数，innodb 引擎的表中是一个估计值， 并不总是准确
        
    *   filtered：表示返回结果的行数占需读取行数的百分比，值越大越好
        
    *   extra：额外字段
        

### 索引使用：单列索引，联合索引，前缀索引

*   联合索引：即一个索引包含了多个列
    
    *   最左前缀法则：联合索引的使用要遵循最左法则，最左前缀法则指的是查询从索引的最左列开始， 并且不跳过索引中的列。如果跳跃某一列，索引将会部分失效 (后面的字段索引失效)
        
        ```
        # 1. 为表中的字段：profession、age、status创建联合索引
        create index  pro_age_sta_idx on tb_user(profession,age,status);
        # ----------- 测试 联合索引是否失效,数字是explain的索引长度----------------
        # profession、age、status三个字段都使用到了索引  57
        explain select * from tb_user where profession = '软件工程' and age = 31 and status = '0'; 
        # profession、age 字段使用到了索引  49
        explain select * from tb_user where profession = '软件工程' and age = 31;
        # profession 字段使用到了索引 47
        explain select * from tb_user where profession = '软件工程';
        # 全表扫描，没有使用索引，不符合最左前缀法则 NULL
        explain select * from tb_user where age = 31 and status = '0';
        explain select * from tb_user where status = '0';
        # profession 字段使用到了索引，跳过了age，因此age后的status字段无法使用索引 47
        explain select * from tb_user where profession = '软件工程' and status = '0';
        # profession、age、status三个字段都使用到了索引，与查询时的位置顺序无关 57
        explain select * from tb_user where age = 31 and status = '0' and profession = '软件工程';
        
        ```
        
*   单列索引：即一个索引只包含单个列
    
    > 在业务场景中，如果存在多个查询条件，考虑针对于查询字段建立索引时
    > 
    > 建议建立联合索引， 而非单列索引。
    
*   前缀索引
    
    字段类型为字符串 (varchar,text)，若需要索引是很长的字符串，会使索引长度过长，浪费大量磁盘 IO 影响查询效率。因此仅对字符串的一部分前缀建立索引，节约索引空间，提高查询效率
    
    *   创建前缀索引：`create index idx_xxxx on table_name(column(n)) ;`
        
    *   前缀长度 n：根据索引选择性决定，选择性指不重复的索引值（基数）和 表记录总数的比值，索引选择性越高则查询效率越高。例如：唯一索引的选择性是 1，是最好的索引选择性，性能最好
        
        ```
        # 查询使用email整个字符串的索引选择比   1.0000
        select count(distinct email) / count(*) from tb_user;
        # 查询使用email 使用前缀5个字符串的索引选择比  0.9583
        select count(distinct substring(email,1,5)) / count(*) from tb_user ;
        # 查询使用email 使用前缀2个字符串的索引选择比   0.9167
        select count(distinct substring(email,1,2)) / count(*) from tb_user ;
        # 对字段email建立前缀索引，前缀长度为5  
        create index email_idx on tb_user(email(5));
        # 查看使用email前缀索引进行查询的执行结构
        explain select * from tb_user where email = 'xiaoyu666@qq.com'; 
        
        ```
        
        修改 n，查看索引选择性的值，在索引选择性和前缀长度 做权衡
        
    *   前缀索引的查询流程：
        
        *   查询字符串的前 N 个字符串去索引表中查找 获取到对应前缀的主键 ID，找到后回表去主键表中获取到整行数据（整个字符串），对比是否与查找字符串相等
        *   若在二级索引的表中 下一条数据的前缀 N 和当前查找的前缀 N 不等，则直接返回；
        *   若相等，则获取下一条数据的主键 ID，回表去主键表中获取整行数据，查看字符串是否和查询字符串相等；

### 索引使用：验证索引效率，索引失效，SQL 提示，覆盖索引

*   验证索引效率
    
    *   在未建立索引时，执行 SQL 语句 查看 SQL 耗时：
        
        `select * from tb_sku where sn = '100000003145001';`
        
    *   对字段`sn`建立索引后，重新进行查询，查看 SQL 耗时
        
        `create index sn_idx on tb_sku(sn);`
        
        `select * from tb_sku where sn = '100000003145001';`
        
        `show index from tb_sku;`
        
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/ae684cd8d76ff7bb86f57dd20efb3c82.png)
    
    使用`explain`解释和分析 查询语句
    
    `explain select * from tb_sku where sn = '100000003145001';`  
    ![](https://i-blog.csdnimg.cn/blog_migrate/86b2f13703fbf33f02fe0c029ea2d788.png)
    
*   索引失效情况：
    
    *   不遵循最左前缀法则 (联合索引)：没有有从索引的最左列开始，联合索引失效；中间跳过了索引的中间字段，则该字段后的联合索引都失效
        
    *   范围查询 (联合索引)：联合索引中，出现范围查询 (>,<)，范围查询右侧的列索引失效
        
        ```
        # profession、age使用到了索引， 49
        explain select * from tb_user where profession = '软件工程' and age > 30 and status = '0';
        # profession、age、status三个字段都使用到了索引  57
        explain select * from tb_user where profession = '软件工程' and age >= 30 and status = '0';
        
        ```
        
    *   索引列运算：在索引列上进行运算操作， 索引将失效
        
    *   字符串不加引号：字符串类型字段使用时，不加引号，索引将失效
        
    *   模糊查询：尾部模糊匹配，索引不会失效；头部模糊匹配，索引失效。
        
    *   or 连接条件：or 左右两侧的字段必须都有索引，若左有 右无 则左的索引失效
        
    *   数据分布影响：如果 MySQL 评估使用索引比全表更慢，则不使用索引
        
*   SQL 提示：是优化数据库的一个重要手段，简单说，是在 SQL 语句中加入一些人为提示达成优化操作
    
    ```
    # SQL提示 使用者根据自己的倾向 建议/忽略/强制使用 MySQL使用哪个索引进行查询
    # profession字段有两个索引：单列索引 profession_idx，联合索引 pro_age_sta_idx
    
    #use index: 建议MySQL使用哪一个索引完成此次查询（仅仅是建议，mysql内部还会再次进行评估）
    explain select * from tb_user use index(profession_idx) where profession = '软件工程';
    #ignore index: 忽略指定的索引
    explain select * from tb_user ignore index(pro_age_sta_idx) where profession = '软件工程';
    #force index: 强制使用索引
    explain select * from tb_user force index(profession_idx) where profession = '软件工程';
    
    ```
    
*   覆盖索引：指查询使用了索引，并且需要返回的列，在该索引中已经全部能够找到。尽量使用覆盖索引，减少 select *。 尽量返回使用索引就能得到的列，而不是需要回表
    

### 索引设计原则

*   针对于数据量较大，且查询比较频繁的表建立索引
*   针对于常作为查询条件（where）、排序（order by）、分组（group by）操作的字段建立索引
*   尽量选择区分度高的列作为索引，尽量建立唯一索引，区分度越高，使用索引的效率越高
*   如果是字符串类型的字段，字段的长度较长，可以针对于字段的特点，建立前缀索引
*   尽量使用联合索引，减少单列索引，查询时，联合索引很多时候可以覆盖索引，节省存储空间， 避免回表，提高查询效率。
*   要控制索引的数量，索引并不是多多益善，索引越多，维护索引结构的代价也就越大，会影响增删改的效率
*   如果索引列不能存储 NULL 值，请在创建表时使用 NOT NULL 约束它。当优化器知道每列是否包含 NULL 值时，它可以更好地确定哪个索引最有效地用于查询。

SQL 优化
------

分为：插入数据优化，主键优化，order by 优化，group by 优化，limit 优化，count 优化，update 优化

![](https://i-blog.csdnimg.cn/blog_migrate/5e3d809bad02c4c8dd4bc2e3ebc76097.png#pic_center)

*   插入数据
    
    *   批量插入数据，手动开启事务：若插入数据较多，不建议使用 insert 单条插入，MySQL 自动开启事务，每一次 insert 都会开启并且提交事务，影响效率。建议批量插入数据，若数据较多，则手动开启事务，多次批量提交数据后 提交事务。
        
    *   Load 数据文件：使用 MySQL 数据库提供的 load 指令插入大批量数据
        
        ```
        # 1. 本地数据库 创建数据库，创建表结构
        # 2. 将数据脚本文件上传至 本地/服务器 某路径下
        # 3. 客户端连接服务端时，加上参数
        mysql –-local-infile -u root -p
        # 4. 设置全局参数local_infile为1，开启从本地加载文件导入数据的开关
        set global local_infile = 1;
        # 执行load指令将准备好的数据，加载到表结构中 
        # '/root/sql1.log'数据脚本文件路径， ',' 一行数据的各个列分隔符, '\n' 行和行的分隔符
        load data local infile '/root/sql1.log' into table tb_user fields terminated by ',' lines terminated by '\n' ;
        
        ```
        
*   **主键优化**
    
    *   数据组织方式
        
        *   InnoDB 存储引擎中，表数据根据主键顺序组织存放的，以该存储方式存储的表为索引组织表
        *   行数据存储在聚集索引的叶子节点上
        *   表空间 -> 段 -> 区 -> 页
    *   **页分裂**：主键乱序插入下，如果页满 会导致页分裂
        
        每个页包含了 2-N 行数据 (如果一行数据过大，会行溢出；只有 1 个数据 形成链表)，根据主键排列
        
        *   主键顺序插入：
            
            ![](https://i-blog.csdnimg.cn/blog_migrate/cc3b11598e076e3059be26c6e0ec33a0.png)
            
        *   主键乱序插入：
            
            插入 50，但应该放在 1 页后 2 页前，但 1 页和 2 页都满，因此产生页分裂
            
            开启页 3，移动元素`23，47`，并插入新数据`50`至新页面
            
            再断开原 1 页 2 页的双向链表，并插入分裂出来的新页面，维护左右的双向链表关系
            
            ![](https://i-blog.csdnimg.cn/blog_migrate/b7e469a0edae5d35afdef7592553bac3.png)  
            ![](https://i-blog.csdnimg.cn/blog_migrate/d919569b42e55a3a8a7cea7e9b4b9f2b.png)
            
    *   **页合并**：删除操作并未 对记录进行物理删除，而是记录被逻辑标记为删除。且其空间变为允许被其他记录声明使用。当页中删除的记录达到 MERGE_THRESHOLD（默认为页的 50%），InnoDB 会开始寻找最靠近的页（前 或后）看看是否可以将两个页合并以优化空间使用。
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/71c3c6fb7b4ec19b8789668d4108a0d5.png)  
        ![](https://i-blog.csdnimg.cn/blog_migrate/bbc5dd1ea9b1a0640ed72c49e4c7f2bc.png)
        
    *   主键设计原则：
        
        *   满足业务需求的情况下，尽量降低主键的长度。
        *   插入数据时，尽量选择顺序插入，选择使用 AUTO_INCREMENT 自增主键。
        *   尽量不要使用 UUID 做主键或者是其他自然主键，如身份证号。
        *   业务操作时，避免对主键的修改。
*   **order by 优化**
    
    MySQL 的排序有两种方式（Explian 的 Extra 内容：
    
    *   **Using filesort** : 通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区 sort buffer 中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫 FileSort 排序
    *   **Using index** : 通过有序索引顺序扫描直接返回有序数据，不需要额外排序，操作效率高
    
    Using index 的性能高，Using filesort 的性能低，在优化排序操作时，尽量要优化为 Using index
    
    > order by 语句需要与索引的顺序完全匹配，创建联合索引时默认升序排列
    > 
    > `create index age_phodes_idx on tb_user(age asc, phone desc);`
    
    order by 优化原则：
    
    *   根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则
    *   尽量使用覆盖索引
    *   多字段排序, 一个升序一个降序，需要注意联合索引在创建时的规则（ASC/DESC）
    *   如果不可避免的出现 filesort，大数据量排序 可以适当增大排序缓冲区大小 sort_buffer_size(默认 256k)。
*   **group by 优化**
    
    在分组操作时，可以通过索引来提高效率。 分组操作时，索引的使用也满足最左前缀法则
    
    满足联合索引的最左前缀法则 Explain 语句中显示 Using idex，不满足则显示 Using temporary
    
*   **limit 优化**
    
    **在数据量比较大时，如果进行 limit 分页查询，在查询时，越往后，分页查询效率越低**。当在进行分页查询时，如果执行 limit 2000000,10 ，此时需要 MySQL 排序前 2000010 记录，并仅返回 2000000 - 2000010 的记录，其他记录丢弃，查询排序代价非常大
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/89dfff792f890dbb510f645cc49a6d22.png#pic_center)
    
    优化：分页查询时，通过创建 覆盖索引 能够提高性能，可以通过覆盖索引加子查询形式进行优化。
    
    ```
    # id 为主键索引 执行耗时11.46 sec， 低于直接limit查询的 19.39 sec
    select s.* from tb_sku s, (select id from tb_sku order by id limit 9000000,10) as a wehre s.id = a.id;
    
    ```
    
*   **count 优化**
    
    `select count(*) from tb_user ;` 若数据量很大，在执行 count 操作时，是非常耗时的
    
    *   MyISAM 引擎把一个表的总行数存在了磁盘上，因此执行 `count(*)` 时候会直接返回这个 数，效率很高； 但是如果是带条件的 count，MyISAM 也耗时
    *   InnoDB 引擎就麻烦了，执行 count(*) 的时候，需要把数据一行一行从引擎里面读取，然后累积计数
    
    优化：自己计数（借助于 redis 这样的数 据库进行，但对于带条件的 count 也耗时）
    
    count 的用法：
    
    *   count（*）：InnoDB 引擎并不会把全部字段取出来，而是专门做了优化，不取值，服务层直接按行进行累加。
        
    *   count（主键）：InnoDB 引擎遍历表，把每一行主键 id 值取出来返回给服务层。 服务层拿到主键后，直接按行进行累加 (主键不可能为 null)
        
    *   count（字段）：
        
        *   没有 not null 约束 : InnoDB 引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，服务层判断是否为 null，不为 null，计数累加。
        *   有 not null 约束：InnoDB 引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，直接按行进行累加。
    *   count（数字）：InnoDB 引擎遍历整张表，但不取值。服务层对于返回的每一行，放一个数字 “1” 进去，直接按行进行累加。
        
        > 按照效率排序：
        > 
        > `count(字段) < count(主键 id) < count(1) ≈ count(*)`
        > 
        > count(*) 和 count(1) 不取值，count(id) 取值但不用判断 null，count(字段) 取值还需要判断 null
        > 
        > 所以尽量使用 `count(*)`。
        
*   **update 优化**
    
    `update 表名 set 字段1 where 字段2 = 'xxx'`
    
    **当 `WHERE` 子句中的字段 2 没有索引时，数据库通常会使用表锁**。表锁会锁定整个表，以确保在更新过程中没有其他事务可以修改表中的数据。可能会导致并发性能下降，因为其他事务无法同时访问该表的其他行。
    
    ** 当 `WHERE` 子句中的字段有索引时，数据库通常会使用行锁。** 行锁仅锁定满足条件的行，而不是整个表。这样可以提高并发性能，因为其他事务可以同时访问不受锁限制的其他行。
    
    要根据索引字段进行更新，否则行级锁变成表级锁
    
    > InnoDB 的行锁是针对索引，不是针对记录加的锁，并且该索引不能失效，否则会从行锁 升级为表锁
    

视图 / 存储过程 / 存储函数 / 触发器
----------------------

![](https://i-blog.csdnimg.cn/blog_migrate/13bd2bd594275f0bd7ed102c8ae1ed2b.png#pic_center)

### 视图

视图（View）是一种虚拟存在的表。视图中的数据并不在数据库中实际存在，行和列数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的。

通俗的讲，视图只保存了查询的 SQL 逻辑，不保存查询结果。所以我们在创建视图的时候，主要的工作 就落在创建这条 SQL 查询语句上。

*   **视图的创建、查看、修改、删除**
    
    ```
    # 创建
    CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] AS SELECT语句 [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]
    # 查询
    SHOW CREATE VIEW 视图名称;      # 查看创建视图语句
    SELECT * FROM 视图名称 ...... ; # 查看视图数据
    # 修改 方式一
    CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] AS SELECT语句 [ WITH
    [ CASCADED | LOCAL ] CHECK OPTION ]
    # 修改 方式二
    ALTER VIEW 视图名称[(列名列表)] AS SELECT语句 [ WITH [ CASCADED |
    LOCAL ] CHECK OPTION ]
    # 删除
    DROP VIEW [IF EXISTS] 视图名称 [,视图名称] ...
    
    ```
    
*   **操作视图，以插入基表中 数据**
    
    **可以像操作表一样去操作视图，但视图并不存储数据，数据都存放在基表中**
    
    ```
    # 创建视图
    create or replace view stu_v_1 as select id,name,no from student where id <= 10;
    # 通过操作视图 以在基表中插入数据
    insert into stu_v_1 values('6,Tom'); # 基表插入数据成功
    # 插入数据成功，但视图中看不到，因为创建视图时指定了 id<= 10
    insert into stu_v_1 values(15,'Amy'); 
    #需要在创建视图的时候加上后面的选项，再次插入id=15数据，报错 阻止了不满足视图的数据插入
    create or replace view stu_v_1 as select id,name from student where id <= 10; with cascaded check option;
    
    ```
    
*   **视图的检查选项**
    
    使用子句`with check option`创建视图时，MYSQL 会通过视图检查正在更改的每行（插入，更新，删除等操作）是否符合视图的定义。MySQL 允许基于一个视图创建另一个视图，并检查依赖视图中的规则以保持一致性。为了确定检查的范围，提供了两个选项
    
    *   `WITH CASCADED CHECK OPTION` 是指在更新视图时，**会检查视图中所有相关的视图和基表的约束**。如果更新操作违反了任何一个相关表的约束条件，更新将被拒绝。
    *   `WITH LOCAL CHECK OPTION` 是指在更新视图时，**仅检查当前视图的约束条件**。如果更新操作违反了当前视图的约束条件，更新将被拒绝。不会检查其他相关表的约束条件。
*   **视图的更新**
    
    要使视图可更新，视图中的行与基础表中的行之间必须存在一对一的关系。
    
    如果视图包含以下任何一 项，则该视图不可更新：
    
    *   聚合函数或窗口函数（SUM()、 MIN()、 MAX()、 COUNT() 等）
    *   DISTINCT C. GROUP BY D. HAVING E. UNION 或者 UNION ALL
    
    举例：`create view v1 as select count(*) from student;` `insert int v1 values(10);`
    
    ​ 报错，提示当前视图不能插入数据，因为 count(*) 的结果 没有 和 student 一样是一一对应的关系
    
*   **视图的作用：**
    
    *   简单：把复杂查询条件定义为视图，简化用户操作
    *   安全：数据库可以通过视图 保证了某些数据的安全性，通过视图 用户只能查询和修改它们所能见到的数据
    *   数据独立：帮助用户屏蔽真实结构变化带来的影响

### 存储过程

*   **概念**：是一段预先编译并保存在数据库中的可重复使用的代码块。存储过程由一系列的 SQL 语句和控制流语句组成，可以接受参数，并且可以返回结果。存储过程通常用于执行复杂的数据库操作，实现业务逻辑和数据处理等任务。
    
*   **特点**：
    
    *   封装，复用。可以把某一业务 SQL 封装在存储过程中，使用时直接调用
    *   可以接收参数，也可以返回数据。存储过程中，可以传递参数，也可以接收返回值
    *   减少网络交互，效率提升。如果分步执行多条 SQL，则每执行一次都是一次网络传输。 而如果封装在存储过程中，则只需要一次网络交互即可。
*   **存储过程的 创建，调用，查看，删除**
    
    ```
    # ****************************创建****************************
    CREATE PROCEDURE 存储过程名称 ([ 参数列表 ])
    BEGIN
    -- SQL语句
    END ;
    # ****************************调用****************************
    CALL 名称 ([ 参数 ]);
    # ****************************查看****************************
    -- 查询指定数据库的存储过程及状态信息
    SELECT * FROM INFORMATION_SCHEMA.ROUTINES WHERE ROUTINE_SCHEMA = 'xxx'; 
    -- 查询某个存储过程的定义
    SHOW CREATE PROCEDURE 存储过程名称; 
    # ****************************删除****************************
    DROP PROCEDURE [ IF EXISTS ] 存储过程名称;
    # ****************************举例****************************
    delimiter $$  #设置MySQL语句结束符为$$
    create procedure p1()
    begin
        select count(*) from student;
    end$$
    delimiter ; #再设置为默认初始的 ;
    call p1;  # 调用p1
    drop procedure if exists p1; # 删除p1
    
    ```
    
*   **变量：在 MySQL 中变量分为三种类型: 系统变量、用户定义变量、局部变量。**
    
    *   **系统变量**：是 MySQL 服务器提供，非用户定义，属于服务器层面。分为全局变量、会话变量
        
        *   全局变量 (GLOBAL)：全局变量针对于所有的会话
        *   会话变量 (SESSION)：会话变量针对于单个会话，在另外一个会话窗口就不生效了
        
        > 注意：
        > 
        > 如果没有指定 SESSION/GLOBAL，默认是 SESSION，会话变量
        > 
        > mysql 服务重新启动之后，所设置的全局参数会失效，要想不失效，可以在 /etc/my.cnf 中配置。
        
    *   **用户定义变量**：用户根据需要自己定义的变量，不用提前声明，使用时直接 “@变量名” 就可以，其作用域为当前连接
        
    *   **局部变量**：是根据需要定义的在局部生效的变量，访问之前，需要 DECLARE 声明。可用作存储过程内的 局部变量和输入参数，局部变量的范围是在其内声明的 BEGIN … END 块
        
    
    ```
    # 查看系统变量 如果没有指定SESSION/GLOBAL，默认是SESSION，会话变量
    SHOW [ SESSION | GLOBAL ] VARIABLES ; -- 查看所有系统变量
    SHOW [ SESSION | GLOBAL ] VARIABLES LIKE 'xxx'; -- 可以通过LIKE模糊匹配方式查找变量
    SELECT @@[SESSION | GLOBAL] 系统变量名; -- 查看指定变量的值
    # 设置系统变量
    SET [ SESSION | GLOBAL ] 系统变量名 = 值 ;
    SET @@[SESSION | GLOBAL]系统变量名 = 值 ;
    
    # 赋值 用户定义变量  赋值时，可以使用 = ，也可以使用 := 
    SET @var_name = expr [, @var_name = expr] ... ;
    SET @var_name := expr [, @var_name := expr] ... ;
    # 使用 用户定义变量  用户定义的变量无需对其进行声明或初始化，只不过获取到的值为NULL
    SELECT @var_name ;
    
    # 声明 局部变量
    DECLARE 变量名 变量类型 [DEFAULT ... ] ;
    # 赋值 局部变量
    SET 变量名 = 值 ;
    SET 变量名 := 值 ;
    SELECT 字段名 INTO 变量名 FROM 表名 ... ;
    
    ```
    
*   if：if 条件判断的结构中，ELSE IF 结构可以有多个，也可以没有。 ELSE 结构可以有，也可以没有
    
    ```
    IF 条件1 THEN
    	..... # 条件1 成立执行该THEN后的语句
    ELSEIF 条件2 THEN -- 可选
    	..... # 条件2 成立执行该THEN后的语句
    ELSE -- 可选
    	..... # 以上条件都不成立 成立执行该THEN后的语句
    END IF;
    # ---------------------举例：根据定义参数score，判定当前分数对应等级--------------------
    drop procedure if exists p3;
    create procedure p3()
    begin
        declare score int default 58; #声明变量score为58，判断其分数等级
        declare grade varchar(10); #用于接收等级
        if score >= 85 then
            set grade := '优秀';
        elseif score >= 60 then
            set grade := '及格';
        else
            set grade := '不及格';
        end if;
        select grade;
    end;
    call p3; # 不及格
    
    ```
    
*   参数。参数的类型 主要分为以下三种：IN、OUT、INOUT。
    
    *   IN（默认）：该类参数作为输入，也就是需要调用时传入值
    *   OUT：该类参数作为输出，也就是该参数可以作为返回值
    *   INOUT：既可以作为输入参数，也可以作为输出参数
    
    ```
    CREATE PROCEDURE 存储过程名称 ([ IN/OUT/INOUT 参数名 参数类型 ])
    BEGIN
    	-- SQL语句
    END ;
    #---------------------举例：根据传入的200分制的分数，返回换算成百分制分数---------------------
    create procedure  p5(inout score double)
    begin
        set score := score * 0.5;
    end;
    set @myscore := 180;
    call p5(@myscore);
    select @myscore; # 90
    # ---------------------举例：根据传入参数score，判定并返回当前分数对应等级--------------------
    drop procedure if exists p4;
    create procedure p4(in score int, out grade varchar(10))
    begin
        if score >= 85 then
            set grade := '优秀';
        elseif score >= 60 then
            set grade := '及格';
        else
            set grade := '不及格';
        end if;
    end;
    call p4(66,@mygrade); # 调用时，直接传入两个参数，使用用户自定义变量接收grade的值
    select @mygrade;
    
    ```
    
*   case。case 结构及作用，与基础篇中的流程控制函数类似，两种语法格式：
    
    ```
    #语法1，含义： 当case_value的值为 when_value1时，执行statement_list1，当值为 when_value2时，执行statement_list2， 否则就执行 statement_list
    CASE case_value
    	WHEN when_value1 THEN statement_list1
    	[ WHEN when_value2 THEN statement_list2] ...
    	[ ELSE statement_list ]
    END CASE;
    #语法2，含义： 当条件search_condition1成立时，执行statement_list1，当条件search_condition2成立时，执行statement_list2， 否则就执行 statement_list
    CASE
    	WHEN search_condition1 THEN statement_list1
    	[WHEN search_condition2 THEN statement_list2] ...
    	[ELSE statement_list]
    END CASE;
    #---------------------举例：根据传入的月份，判定所属季度---------------------
    drop procedure if exists p6;
    create procedure p6(in month int)
    begin
        declare ans varchar(10);
        case
            when month in (1,2,3)
                then set ans := '第一季度';
            when month in (4,5,6)
                then set ans := '第二季度';
            when month in (7,8,9)
                then set ans := '第三季度';
            when month in (10,11,12)
                then set ans := '第四季度';
        end case;
        select concat('您输入的月份为：',month, ',所在季度是：',ans);
    end;
    
    call p6(10); # 第四季度
    
    ```
    
*   while。有条件的循环控制语句。满足条件后，再执行循环体中的 SQL 语句
    
    ```
    -- 先判定条件，如果条件为true，则执行逻辑，否则，不执行逻辑
    WHILE 条件 DO
    	SQL逻辑...
    END WHILE;
    #---------------------举例：计算从1累加到n的和，n为传入的参数值---------------------
    drop procedure if exists p7;
    create procedure p7(in n int)
    begin
        declare sum int default 0;
        while n > 0 do
            set sum := sum + n;
            set n := n - 1;
        end while;
        select concat('从1到n的和为：',sum);
    end;
    call p7(10); #55
    
    ```
    
*   repeat。是有条件的循环控制语句, 当满足 until 声明的条件的时候，则退出循环
    
    while 是不满足条件则退出循环，repeat 是满足条件则退出循环
    
    repeat 先执行一次，再判断是否满足循环条件
    
    ```
    -- 先执行一次逻辑，然后判定UNTIL条件是否满足，如果满足，则退出。如果不满足，则继续下一次循环
    REPEAT
    	SQL逻辑...
    	UNTIL 条件
    END REPEAT;
    #---------------------举例：计算从1累加到n的和，n为传入的参数值---------------------
    drop procedure if exists p8;
    create procedure p8(in n int)
    begin
        declare sum int default 0;
        repeat
            set sum := sum + n;
            set n := n - 1;
        until  n = 0
        end repeat;
        select concat('从1到n的和为：',sum);
    end;
    call p8(100); #5050
    
    ```
    
*   loop。实现简单的循环，如果不在 SQL 逻辑中增加退出循环的条件，可以用其来实现简单的死循环
    
    LOOP 可以配合以下两个语句使用：
    
    *   LEAVE ：配合循环使用，退出循环
    *   ITERATE：必须用在循环中，作用是跳过当前循环剩下的语句，直接进入下一次循环
    
    ```
    [begin_label:] LOOP
    	SQL逻辑...
    END LOOP [end_label];
    
    LEAVE label; -- 退出指定标记的循环体
    ITERATE label; -- 直接进入下一次循环
    #  begin_label，end_label，label 都是自定义的标记
    
    #---------------------举例：计算从1累加到n的偶数和，n为传入的参数值---------------------
    drop procedure if exists p10;
    create procedure p10(in n int)
    begin
        declare sum int default 0;
        getsum:loop #循环代码
            if n = 0 then
                leave getsum; # 退出循环 leave
            end if;
            if n%2 = 1 then
                set n := n - 1;
                ITERATE getsum;
            end if;
            set sum := sum + n;
            set n := n - 1;
        end loop getsum;
    
        select concat('从1到n的和为：',sum);
    end;
    call p10(10); #30
    
    ```
    
*   游标（CURSOR）。是用来存储查询结果集的数据类型 , 在存储过程和函数中可以使用游标对结果集进 行循环的处理。游标的使用包括游标的声明、OPEN、FETCH 和 CLOSE
    
    ```
    # 声明游标
    DECLARE 游标名称 CURSOR FOR 查询语句 ;
    # 打开游标
    OPEN 游标名称 ;
    # 获取游标记录
    FETCH 游标名称 INTO 变量 [, 变量 ] ;
    # 关闭游标
    CLOSE 游标名称 ;
    
    ```
    
*   条件处理程序（Handler）。可以用来定义在流程控制结构执行过程中遇到问题时相应的处理步骤
    
    ```
    DECLARE handler_action HANDLER FOR condition_value [, condition_value] ... statement ;
    
    handler_action：
    	CONTINUE: 继续执行当前程序
    	EXIT: 终止执行当前程序
    
    condition_value：
    	SQLSTATE sqlstate_value: 状态码，如 02000
    	SQLWARNING: 所有以01开头的SQLSTATE代码的简写
    	NOT FOUND: 所有以02开头的SQLSTATE代码的简写
    	SQLEXCEPTION: 所有没有被SQLWARNING 或 NOT FOUND捕获的SQLSTATE代码的简写
    	drop procedure if exists p12;
    
    
    #---------------------举例：根据传入的参数uage，查询用户表tb_user中所有用户年龄小于等于uage的name和profession，并将这两个字段插入到新表 (id,name,profession)中---------------------
    create procedure p12(in user_age int)
    begin
        # 有先后顺序：先声明普通变量，再声明游标
        declare namenew varchar(100);
        declare profnew varchar(100);
        # 1.声明游标 存储查询结果集
        declare getInfo cursor for
            select name,profession from tb_user where age <= user_age;
        # 定义 条件处理程序，当状态码为02000时触发条件处理程序 -> 退出操作，关闭游标
        declare exit handler for sqlstate '02000' close getInfo;
        # declare exit handler for not found close getInfo;
        # 2.创建新表的 表结构
        drop table if exists tb_user_new;
        create table if not exists tb_user_new(
            id int primary key auto_increment,
            name varchar(100),
            profession varchar(100)
        );
        # 3.开启游标
        open getInfo;
        # 4.获取游标查询的结果集，循环遍历
        # 4.1 遍历的时候把查询结果集的name和profession赋值给两个局部变量,声明两个变量
        while true do
            # 获取游标记录
            fetch getInfo into namenew,profnew;
            # 4.3 插入新表数据
            insert into tb_user_new values(null,namenew,profnew);
        end while;
        # 关闭游标
        close getInfo;
    end;
    
    call p12(30);
    
    ```
    

### 存储函数

概念：存储函数是**有返回值**的存储过程，存储函数的**参数只能是 IN 类型**的

```
CREATE FUNCTION 存储函数名称 ([ 参数列表 ])
RETURNS type [characteristic ...] # 指定返回值类型
BEGIN
	-- SQL语句
	RETURN ...; # 返回结果
END ;
#---------------------举例：计算从1累加到n的值，n为传入的参数值---------------------
create function fun1(n int)
RETURNS int deterministic
begin
    declare sum int default 0;
    while n > 0 do
        set sum := sum + n;
        set n := n - 1;
    end while;
    return sum;
end;
select fun1(100);

```

characteristic 说明：

*   DETERMINISTIC：相同的输入参数总是产生相同的结果
*   NO SQL ：不包含 SQL 语句
*   READS SQL DATA：包含读取数据的语句，但不包含写入数据的语句

### 触发器 Triggers

在 MySQL 中，触发器（Triggers）是一种数据库对象，它与表相关联，并在表上的特定事件发生时自动执行一系列动作。是在 insert/update/delete 之前 (BEFORE) 或之后(AFTER)，触发并执行触发器中定义的 SQL 语句集合。

使用别名 OLD 和 NEW 来引用触发器中发生变化的记录内容，这与其他的数据库是相似的。现在触发器还 只支持行级触发，不支持语句级触发。

old 用来引用原来的记录内容，new 用来引用新的记录内容

*   INSERT 型触发器：NEW 表示将要或者已经新增的数据
    
*   UPDATE 型触发器：OLD 表示修改之前的数据 , NEW 表示将要或已经修改后的数据
    
*   DELETE 型触发器： OLD 表示将要或者已经删除的数据
    

```
# 创建
CREATE TRIGGER trigger_name
BEFORE/AFTER INSERT/UPDATE/DELETE
ON tbl_name FOR EACH ROW -- 行级触发器
BEGIN
	trigger_stmt ;
END;
# 查看
SHOW TRIGGERS ;
# 删除
DROP TRIGGER [schema_name.]trigger_name ; 
-- 如果没有指定 schema_name，默认为当前数据库

```

锁
-

是计算机协调多个进程或线程并发访问某一资源的机制，用于保护并发访问数据的一致性和有效性

### 全局锁

对整个数据库实例加锁，加锁后整个实例处于只读状态，后续 DML 的写语句，DDL 语句，更新操作的事务提交都将被阻塞

使用场景：全库逻辑备份（将数据库中的数据备份成一个 SQL 文件保存在磁盘中

*   若执行备份过程中，还有数据的修改和插入，则备份的数据无法保证数据的一致性和完整性。
    
    例如表订单，表库存，表订单日志，逐个备份该表的过程（备份顺序：订单 -> 库存 -> 日志）中依然有下单的操作，即备份了订单 但库存已经减少等操作 会导致数据库的数据不一致
    
*   加锁后，DML 和 DDL 无法操作，DQL 查询数据　依然可以使用
    

```
flush tables with read lock ; # 加全局锁
sudo mysqldump -h 127.0.0.1 -uroot -p123456 -d db01 > db01.sql # 数据备份 shell界面中执行
unlock tables; # 再登录到MySQL，释放锁

```

> 数据库中加全局锁，是一个比较重的操作，存在以下问题：
> 
> *   如果在主库上备份，那么在备份期间都不能执行更新
> *   如果在从库上备份，那么在备份期间从库不能执行主库同步过来的二进制日志（binlog），会导致主从延迟
> 
> 在 InnoDB 引擎中，可以在备份时加上参数 --single-transaction 参数来完成不加锁的一致性数据备份
> 
> `sudo mysqldump -single-transaction -h 127.0.0.1 -uroot -p123456 -d db01 > db01.sql`

### 表级锁

*   表锁
    
    *   表共享读锁 (read lock)
    *   表独占写锁 (write lock)
    
    ```
    lock tables 表名... read/write  # 加锁
    unlock tables / 客户端断开连接  # 释放锁
    
    ```
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/47f6a242e9bc04e44aa0acb621a3efa8.png#pic_center)  
    ![](https://i-blog.csdnimg.cn/blog_migrate/d9acef3fb855eea607a925f7b941c85a.png#pic_center)
    
*   元数据锁（meta data lock, MDL）
    
    **MDL 加锁过程是系统自动控制，无需显式使用，在访问一张表的时候会自动加上**
    
    主要作用是维护表元数据的数据一致性，在表上有活动事务的时候，不可以对元数据进行写入操作。为了避免 DML 与 DDL 冲突（增删改 DDL：数据库 表 表字段 ，DML：表中数据），保证读写的正确性。
    
    元数据就是表结构，元数据锁就是维护表结构一致性的锁
    
    *   当对一张表进行增删改查的时候 DML 语句，加 MDL 读锁 (共享)
    *   当对表结构进行变 更操作的时候 DDL 语句，加 MDL 写锁 (排他)
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/1ba31b443405ed7fc37fc29f90fa80cc.png)
    
    > 查看元数据锁的加锁情况：
    > 
    > `select object_type, object_schema, object_name,lock_type, lock_duration from performance_schema.metadata_locks ;`
    
*   意向锁
    
    若两个线程操作一张表，线程 A 对表加了行锁 / 表锁，之后 线程 B 要对表加表锁，线程 B 需要先遍历表 查看表是否已经被加行锁 或者 表锁。
    
    为了减少类似以上，DML 执行时加的行锁与后来要加的表锁冲突的现象。InnoDB 引入了意向锁，使表锁不用再检查每行是否被加锁，使用意向锁减少了表锁的检查。
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/c53e87bb4dc2246fbbfbfcb1e3b6babe.png)![](https://i-blog.csdnimg.cn/blog_migrate/034085117234f80a6dcc3223ac7e89e4.png)
    
    *   意向共享锁 (IS)：由语句 select … lock in share mode 添加
        
        ​ 与表锁共享锁 (read)兼容，与表锁排他锁 (write) 互斥
        
    *   意向排他锁 (IX) 由 insert、update、delete、select…for update 添加 。
        
        ​ 与表锁共 享锁 (read) 及排他锁 (write) 都互斥，意向锁之间不会互斥
        
    
    通过 SQL 查看意向锁和行锁的加锁情况：`select object_schema, object_name, index_name, lock_type, lock_mode, lock_data from performance_schema.data_locks;`
    

### 行级锁

行级锁每次操作锁住对应的行数据。锁定粒度最小，发生锁冲突概率最低，并发度最高。

行级锁应用在 InnoDB 存储引擎中，InnoDB 的数据是基于索引组织的，行锁是**通过对索引上的索引项加锁**来实现的，而不是对记录加的锁。对于行级锁，主要分为以下三类：

*   行锁（Record Lock）：锁定**单个行记录**的锁，防止其他事务对此行进行 update 和 delete。在 RC、RR 隔离级别下都支持  
    ![](https://i-blog.csdnimg.cn/blog_migrate/53988e1de64ab16ccb95222b70cbfe24.png#pic_center)
    
    *   共享锁（S）：允许一个事务去读一行，阻止其他事务获得相同数据集的排它锁
        
    *   排他锁（X）：允许获取排他锁的事务更新数据，阻止其他事务获得相同数据集的共享锁和排他锁
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/b4914b1a4913b9b2541db707fa1a53f5.png)  
        ![](https://i-blog.csdnimg.cn/blog_migrate/b29f33934ef7afd9c9aeb6fbd3998cd7.png)
        
    
    > 注意：
    > 
    > *   针对唯一索引进行检索时，对已存在的记录进行等值匹配时，将会自动优化为行锁
    > *   InnoDB 的行锁是针对于索引加的锁，若不通过索引条件检索数据，那么 InnoDB 将对表中的所有记录加锁，此时就会升级为表锁
    
*   间隙锁（Gap Lock）：锁定索引记录**间隙**（不含该记录），确保索引记录间隙不变，防止其他事 务在这个间隙进行 insert，产生幻读。在 RR 隔离级别下都支持。
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/55f62f47af8a5721b38fc6bd91a225f7.png)
    
    举例：  
    ![](https://i-blog.csdnimg.cn/blog_migrate/99e20e7164d2b28fbfbc9ee9de4aa496.png#pic_center)
    
    ```
    # -----------------------------------------客户端1-----------------------------------------
    begin;
    update usr set age = 40 where id = 5; 
    # 在客户端1 update了一个不存在的id=5后，加上了间隙锁 (3,8)
    commit;
    # -----------------------------------------客户端2-----------------------------------------
    begin;
    INSERT INTO usr VALUES (7,'安拉', '17799998819', 'jd1h@126.com', '城市规划', 51,'2', '0', '2001-09-15 00:00:00');  # 阻塞，被间隙锁，直到客户端1 commit
    commit;
    
    ```
    
*   临键锁（Next-Key Lock）：行锁和间隙锁组合，同时锁住**数据**，并锁住数据前面的**间隙 Gap**。 在 RR 隔离级别下支持。
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/5fb2d077cd21c7e6196744960894f67e.png#pic_center)
    
    默认情况下，InnoDB 在 REPEATABLE READ 事务隔离级别运行，InnoDB 使用 next-key 锁进行搜 索和索引扫描，以防止幻读。
    
    举例：
    
    非唯一索引，为了其它事务向间隙中插入一条记录 出现幻读现象，因此会把（3，7）的间隙锁上，把 3 之前的这块间隙也锁住，3 这行数据也锁住
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/953a4e98bbfd510c15be84d4ef39fdcb.png#pic_center)
    
    > 注意：
    > 
    > *   索引上的等值查询 (唯一索引)，给不存在的记录加锁时, 优化为间隙锁 。
    >     
    > *   索引上的等值查询 (非唯一普通索引)，向右遍历时最后一个值不满足查询需求时，next-key lock 退化为间隙锁。
    >     
    > *   索引上的范围查询 (唯一索引)–会访问到不满足条件的第一个值为止
    >     
    >     举例：锁 19 这行的数据，邻键锁锁上（19，25）以及 25 这行记录，还有邻键锁锁上正无穷大 以及 (25,+∞) 的间隙
    >     
    >     ![](https://i-blog.csdnimg.cn/blog_migrate/fd54aa61a07ff4641c6a557f07132e01.png#pic_center)
    >     
    
    间隙锁锁间隙，不包含对应的数据记录，只锁定该数据记录之前的这部分间隙
    
    邻键锁即包含当前的数据记录，也会锁定该数据记录之前的这部分间隙
    
    间隙锁唯一目的是防止其他事务插入间隙。间隙锁可以共存，一个事务采用的间隙锁不会 阻止另一个事务在同一间隙上采用间隙锁
    

### 锁总结

按照锁的粒度进行分类：

*   全局锁：锁定数据库中的所有表（数据库备份
*   表级锁：每次操作锁住整张表
    *   读锁 / 写锁 `lock tables 表名... read/write; unlock tables;`
    *   元数据锁 自动加 DML 语句 数据的增删改查时 MDL 读锁，DDL 语句表结构变更 MDL 写锁
        *   MDL 读锁：
            *   shared_read：select, select … lock in share mode,
            *   shared_write：insert ,update, select … for update
        *   MDL 写锁：alter table
    *   意向共享锁 / 意向排它锁 自动加
        *   加意向共享锁：shared_read：select, select … lock in share mode,
        *   加意向排它锁：shared_write：insert ,update, select … for update
*   行级锁：每次操作锁住对应的行数据。只针对索引，如果不是索引 升级成表锁
    *   行锁：共享锁 / 排它锁
        *   排它：insert,update,delete, select … for update
        *   共享：select … lock in shared mode
        *   select 不加任何锁
    *   间隙锁和邻键锁：间隙锁锁住间隙，邻键锁锁数据和间隙。
*   乐观锁 / 悲观锁：
    *   乐观锁：查数据时不加锁，提交数据的时候检查是否有其它事务修改了同一条数据。没有修改则提交成功，否则通过 undo log 回滚或者重试。读多写少
    *   悲观锁：查数据加行级锁或者表级锁 share … lock in shared mode; 适合读少写多

InnoDB 引擎
---------

![](https://i-blog.csdnimg.cn/blog_migrate/9f7820cb837b9274064fe19a6b25855c.png#pic_center)

### 逻辑存储结构

![](https://i-blog.csdnimg.cn/blog_migrate/2ce98671ba17ff79dff67a11a2c4703f.png)

*   表空间（ibd 文件），一个 mysql 实例可以对应多个表空间，用于存储记录、索引等数据
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/ffc3d6067a979e78f5f857d95414b67c.png)
    
*   段，分为数据段、索引段、回滚段，InnoDB 是索引组织表，数据段是 B + 树的非叶子结点，段用来管理多个区
    
*   区，表空间的单元结构，每个区大小为 1M，默认情况下 InnoDB 存储引擎页大小为 16K，即一个区公共有 64 个连续的页
    
*   页，是 InnoDB 存储引擎磁盘管理的最小单元，每个页的大小默认为 16KB。为了保证页的连续性，每次 InnoDB 都会向磁盘申请 4-5 个区
    
*   行，InnoDB 存储引擎数据是按照行进行存放的
    
    *   记录中的每一列 col1，col2，col3
    *   两个隐藏列
        *   Trx id，最后一次操作该行的事务 ID
        *   Roll pointer，每次对改行记录进行改动时，会把旧版本写入 undo 日志中，该值是指针，通过它可以找到之前没有改动的旧版本

### 架构——内存结构

![](https://i-blog.csdnimg.cn/blog_migrate/073fe73b48c8e17de9b9817c59e2d207.png#pic_center)

内存结构

*   **Buffer Pool 缓冲池**
    
    *   **InnoDB 存储引擎基于磁盘文件存储，在物理硬盘和在内存中的速度相差很大，为了尽可能弥补这两者之间的 I/O 效率的差值，就需要把经常使用的数据加载到缓冲池中，避免每次访问都进行非常慢的且大部分都是随机的磁盘 I/O**。
        
        在 InnoDB 的缓冲池中不仅缓存了索引页和数据页，还包含了 undo 页、插入缓存、自适应哈希索引以及 InnoDB 的锁信息等等。
        
        缓冲池 Buffer Pool，是主内存中的一个区域，里面可以缓存磁盘上经常操作的真实数据，在执行增 删改查操作时，先操作缓冲池中的数据（若缓冲池没有数据，则从磁盘加载并缓存），然后再以一定频率刷新到磁盘，从而减少磁盘 IO，加快处理速度。
        
        缓冲池以 Page 页为单位，底层**采用链表数据结构管理 Page**。根据状态，将 Page 分为三种类型：
        
        *   free page：空闲 page，未被使用
        *   clean page：被使用 page，数据没有被修改过
        *   dirty page：脏页，被使用 page，数据被修改过，页中数据与磁盘的数据产生了不一致
*   **Change Buffer 更改缓冲区（针对于非唯一的二级索引页）**
    
    *   **在执行 DML（数据 增删改）语句时，如果这些数据 Page 没有在 Buffer Pool 中，不会直接操作磁盘，而会将数据变更存在更改缓冲区 Change Buffer 中，在未来数据被读取时，再将数据合并恢复到 Buffer Pool 中，再将合并后的数据以一定频率刷新到磁盘中**。
        
        意义：与聚集索引不同，二级索引通常是非唯一的，并且以相对随机的顺序插入二级索引。同样，删除和更新 可能会影响索引树中不相邻的二级索引页，如果每一次都操作磁盘，会造成大量的磁盘 IO。有了 ChangeBuffer 之后，我们可以在缓冲池中进行合并处理，减少磁盘 IO
        
*   **Adaptive Hash Index 自适应哈希索引**
    
    *   用于优化对 Buffer Pool 数据的查询。MySQL 的 innoDB 引擎中虽然没有直接支持 hash 索引，但是提供功能，即自适应 hash 索引。
        
        hash 索引对于等值匹配，一般性能高于 B + 树，因为 hash 索引一般只需要一次 IO 即可，而 B + 树可能需 要几次匹配，所以 hash 索引的效率要高，但 hash 索引又不适合做范围查询、模糊匹配等。
        
        因此 **InnoDB 存储引擎会监控对表上各索引页的查询，如果观察到在某条件下 hash 索引效率更高， 则建立 hash 索引，称为自适应 hash 索引。 自适应哈希索引无需人工干预，是系统根据情况自动完成**。
        
*   **Log Buffer 日志缓冲区**
    
    *   Log Buffer：日志缓冲区，**用来保存要写入到磁盘中的 log 日志数据（redo log 、undo log）， 默认大小为 16MB，日志缓冲区的日志会定期刷新到磁盘中**。如果需要更新、插入或删除许多行的事务，增加日志缓冲区的大小可以节省磁盘 I/O。
    *   参数：innodb_log_buffer_size：缓冲区大小
    *   参数：innodb_flush_log_at_trx_commit：日志刷新到磁盘时机，取值主要包含以下三个：
        *   1 日志在每次事务提交时写入并刷新到磁盘，默认值
        *   0: 每秒将日志写入并刷新到磁盘一次
        *   2: 日志在每次事务提交后写入，并每秒刷新到磁盘一次

### 架构——磁盘结构

![](https://i-blog.csdnimg.cn/blog_migrate/073fe73b48c8e17de9b9817c59e2d207.png#pic_center)  
磁盘结构

*   **System Tablespace 系统表空间**
    
    *   是更改缓冲区 Change Buffer 的存储区域。如果表在系统表空间而不是每个表文件或通用表空间中创建的，它也可能包含表和索引数据。(在 MySQL5.x 版本中还包含 InnoDB 数据字典、undolog 等)
*   **File-Per-Table Tablespaces**
    
    *   每张表的独立表空间，则每个表的文件表空间包含单个 InnoDB 表的数据和索引 ，并存储在文件系统上的单个数据文件中。 一个. ibd 对应一个表
*   **General Tablespaces 通用表空间**
    
    *   需要通过 CREATE TABLESPACE 语法创建通用表空间，在创建表时，可以指定该表空间
        
        ```
        # 创建表空间
        CREATE TABLESPACE ts_name ADD DATAFILE 'file_name' ENGINE = engine_name;
        # 创建表时指定表空间
        CREATE TABLE xxx ... TABLESPACE ts_name;
        #-----------举例-----------
        create tablespace ts_01 add datafile 'mydb01.ibd' engine = innodb;
        # 创建表空间成功后，后续创建表时 可以指定把表创建至该表空间里
        use db1;
        create table t1(
            id int primary key auto_increment, 
        	name varchar(10)) engine = innodb tablespace ts_01;
        
        ```
        
*   **Undo Tablespaces 撤销表空间**
    
    *   撤销表空间，MySQL 实例在初始化时会自动创建两个默认的 undo 表空间（初始大小 16M），用于存储 undo log 日志，undo_001, undo_002
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/3f38f6348fddd0f73297afbb4c946098.png)
        
*   **Temporary Tablespaces 临时会话表空间**
    
    *   InnoDB 使用会话临时表空间和全局临时表空间。存储用户创建的临时表等数据。
*   **Doublewrite Buffer Files 双写缓冲区**
    
    *   双写缓冲区，innoDB 引擎将数据页从 Buffer Pool 刷新到磁盘前，先将数据页写入双写缓冲区文件中，便于系统异常时恢复数据
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/ff8e06b80213ed143206f0cf852c1bb7.png)
        
*   **Redo Log 重做日志**
    
    *   用来实现事务的持久性。该日志文件由两部分组成：重做日志缓冲区（redo log buffer）（在内存结构的 Log Buffer 中）以及 重做日志文件（redo log），前者是在内存中，后者在磁盘中。
        
        当事务提交之后会把所有修改信息存到该日志中，用于在刷新脏页到磁盘 发生错误时，进行数据恢复使用。以循环方式写入重做日志文件，涉及两个文件
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/5a77100ad0cf729f5db58de34e45723b.png)
        

内存中的数据和磁盘的数据 是怎么 写入和读取的呢？后台线程

*   **Master Thread（核心后台线程）**：是 MySQL 的一个核心后台线程，负责管理和协调其他后台线程的工作，并将缓冲池中的数据异步刷新到磁盘中，保持数据的一致性；脏页的刷新、合并插入缓存、undo 页的回收
    
*   **IO Thread（读写线程）**：异步非阻塞 IO 极大地提高数据库的性能，这些线程负责处理 InnoDB 存储引擎的 IO 请求，包括读取和写入磁盘上的数据。
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/693c236d4c404a67a1db219f944ff744.png#pic_center)  
    ![](https://i-blog.csdnimg.cn/blog_migrate/a23e787736e333f1b85276efb5550b46.png#pic_center)
    
*   **Purge Thread（清理线程）**：Purge Thread 负责回收已完成提交事务的 undo log，将其释放以供后续事务使用。
    
*   **Page Cleaner Thread（页清理线程）**：该线程负责在 InnoDB 存储引擎中执行脏页的刷新操作，将脏页写回磁盘，以确保数据的持久性和一致性。协助 Master Thread。
    

### 事务原理

事务：一组操作的集合，要么全部成功，要么全部失败：

事务的四大特性：（ACID）原子性，一致性，隔离性，持久性

*   A：事务是不可分割的最小操作单元，要么全部成功，要么全部失败
    
*   C：事务完成时，必须使所有的数据都保持一致状态
    
*   I：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行
    
*   D：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/285dc30fca98ad27070bb8d4155e65cb.png)
    

事务的隔离级别：读未提交，读已提交，可重复读，串行化

*   **持久性：redo log。重做日志**
    
    *   记录事务提交时，对数据页的物理修改，实现事务的持久性
    *   包含两部分：重做日志缓冲（内存结构中的 Log Buffer）与 磁盘的重做日志文件，提交后会把所有修改信息保存到该文件中，用于当刷新脏页到磁盘发生错误时 进行数据恢复使用
    *   update/delete 执行：
        *   先看内存的 BufferPool 中有没有该页，如果没有该页则通过后台线程从磁盘中把页读到 Buffer Pool
        *   直接操作缓冲区中的数据，该页变成脏页
        *   首先把数据页的物理变化记录在内存中的 RodoLogBuffer，commit 事务提交的时候，redologBuffer 会直接把数据页变化刷新到磁盘当中，即 ib_logfile0，ib_logfile1 中
        *   在某个时机 该页以一定频率刷新到磁盘中进行持久化，若此时出错，则可以通过磁盘文件中的 redo_log 进行数据恢复
        *   若脏页顺利写入磁盘，则 redolog 文件就不再需要，因此每过一段时间就清理一次 redo log 日志，是循环性的，不是永久的
    
    > **日志文件都是追加的，是顺序磁盘 I/O，效率比数据在磁盘的随机存取快速的多**
    > 
    > WAL 先写日志 Write-Ahead Logging
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/a7c4b7c27e158a3ec2524d6a0515c98f.png)
    
*   **原子性：undo log。回滚日志**
    
    *   用于记录被修改前的信息，作用包括两个：提供回滚 和 MVCC（多版本并发控制）
        
        undo log 和 redo log 记录的物理日志不一样，它是逻辑日志。可以认为当 delete 一条记录时，undo log 会记录一条对应的 insert 记录，反之亦然，当 update 一条记录时，它记录一条对应相反的 update 记录（执行 update 之前 数据长的样子）。当执行 rollback 时，就可以从 undo log 中的逻辑记录读取到相应的内容并进行回滚
        
    *   Undo log 销毁：undo log 在事务执行时产生，事务提交时，并不会立即删除 undo log，因为这些日志可能还用于 MVCC
        
    *   Undo log 存储：undo log 采用段的方式进行管理和记录，存放在段中的 rollback segment **回滚段**中，内部包含 1024 个 undo log segment
        

### MVCC

基本概念：

*   **当前读：读取的是记录的最新版本，读取时还要保证其他并发事务不能修改当前记录，会对读取的记录进行加锁**。对于我们日常的操作，如：select … lock in share mode(共享锁)，select … for update、update、insert、delete(排他锁) 都是一种当前读。
    
    *   客户端 1 使用 select 语句，客户端 2 使用 update 语句进行更新，因为当前隔离级别是可重复读，因此客户端 1 无法看到客户端 2 事务对数据的更改
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/4796fbd57c9fb4d77cb071854422f00c.png#pic_center)
        
    *   当前读
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/63d3f4be3463316b3ae82f87038ef48c.png#pic_center)
        
*   快照读：简单的 select（不加锁）就是快照读，快照读 读取的是记录数据的可见版本，有可能是历史数据， 不加锁，是非阻塞读
    
    *   Read Committed 读已提交：每次 select，都生成一个快照读
    *   Repeatable Read 可重复高读：开启事务后第一个 select 语句才是快照读的地方。即第一次 select 查询产生快照读，后面的 select 查询直接使用前面的快照数据
    *   Serializable 串行化：快照读会退化为当前读，每次读取都需要加锁
*   MVCC： Multi-Version Concurrency Control，多版本并发控制
    
    *   维护一个数据的多个版本， 使得读写操作没有冲突，快照读 为 MySQL 实现 MVCC 提供了一个非阻塞读功能。MVCC 的具体实现 依赖于数据库记录中的三个隐式字段、undo log 日志、readView

MVCC 实现原理：

*   记录中的隐藏字段
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/6f4b8456ce0952985b7b1a43a3f95847.png)  
    ![](https://i-blog.csdnimg.cn/blog_migrate/8bb073b4ea9a70fb4ce47fb34e23603a.png)
    
*   undo log 日志 回滚日志
    
    *   在 insert、update、delete 的时候产生的便于数据回滚的日志。
        
        当 insert 的时候，产生的 undo log 日志只在回滚时需要，在事务提交后，可被立即删除
        
        update、delete 时，产生的 undo log 日志不仅在回滚时需要，在快照读时也需要，不会立即被删除
        
*   undo log 版本链
    
    最终不同事务或相同事务对同一条记录进行修改，会导致该记录的 undolog 生成一条 记录版本链表。
    
    链表的头部是最新的旧记录，链表尾部是最早的旧记录。
    
    那么每次查询的时候，返回哪一个版本的记录呢？ReadView 的作用
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/62240279074d9f04cb88eb9c3d46e299.png)
    
*   readView 读视图 决定查询读取 的记录
    
    ReadView（读视图）是 快照读 SQL 执行时 MVCC 提取数据的依据，记录并维护系统当前活跃的事务 （未提交的）id
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/886bebb605e14b9c1aec907c46bbdeae.png)  
    ![](https://i-blog.csdnimg.cn/blog_migrate/bca7db6bf7ac785b83bffbb00081f170.png)
    
    *   不同的隔离级别，生成 ReadView 的时机不同：
        
        *   READ COMMITTED ：在事务中每一次执行快照读时生成 ReadView
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/5648cb09cea5a34f5163e84c9163e013.png)  
        ![](https://i-blog.csdnimg.cn/blog_migrate/8d9b76a5d08d3e8b0cf55185946e8243.png)
        
        *   REPEATABLE READ：仅在事务中第一次执行快照读时生成 ReadView，后续复用该 ReadView
            
            在 RR 隔离级别下，只是在事务中第一次快照读时生成 ReadView，后续都是复用该 ReadView，那么既然 ReadView 都一样， ReadView 的版本链匹配规则也一样， 那么最终快照读返 回的结果也是一样的
            
            ![](https://i-blog.csdnimg.cn/blog_migrate/1e355a25e88972be40b057f3e37ede0b.png)
            

因此，MVCC 的实现原理就是通过 InnoDB 表的隐藏字段、UndoLog 版本链、ReadView 来实现的。 而 MVCC + 锁，则实现了事务的隔离性。 而一致性则是由 redolog 与 undolog 保证

![](https://i-blog.csdnimg.cn/blog_migrate/4444b572bc38baadab017041716ed98d.png#pic_center)