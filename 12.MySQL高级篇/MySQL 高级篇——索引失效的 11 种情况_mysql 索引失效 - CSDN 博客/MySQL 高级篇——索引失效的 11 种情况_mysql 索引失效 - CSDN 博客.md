---
url: https://blog.csdn.net/qq_40991313/article/details/130779528
title: MySQL 高级篇——索引失效的 11 种情况_mysql 索引失效 - CSDN 博客
date: 2025-02-20 13:50:20
tag: 
summary: 
---
**导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**最新索引失效的文章：** 

[【MySQL 调优】如何进行 MySQL 调优？从参数、数据建模、索引、SQL 语句等方向，三万字详细解读 MySQL 的性能优化方案（2024 版）-CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/139088747 "​​​​​​​【MySQL调优】如何进行MySQL调优？从参数、数据建模、索引、SQL语句等方向，三万字详细解读MySQL的性能优化方案（2024版）-CSDN博客")

**目录**

[1. 索引优化思路](#t0)

[2. 索引失效的 11 种情况](#t1)

[2.0. 数据准备](#t2)

[2.1 要尽量满足全值匹配](#t3)

[2.2 要满足最佳左前缀法则](#t4)

[2.3 主键插入顺序尽量自增](#t5)

[2.4 计算、函数导致索引失效](#t6)

[2.5 类型转换导致索引失效](#t7)

[2.6 范围条件右边的列索引失效](#t8)

[2.7 没覆盖索引时，“不等于” 导致索引失效](#t9)

[2.8 没覆盖索引时，is not null、not like 导致索引失效](#t10)

[2.9 没覆盖索引时，左模糊查询导致索引失效](#t11)

[2.10 “OR” 前后存在非索引列，导致索引失效](#t12)

[2.11 不同字符集导致索引失败，建议 utf8mb4](#t13)

[2.12 总结](#t14)

[3. 自测练习](#t15)

[3.1 练习题和答案解析](#t16)

[3.2 验证](#t17)

[3.2.1 准备数据](#t18) 

[3.2.2 测试 1：命中覆盖索引](#t19)

[3.2.3 测试 2：命中索引，需要回表](#t20)

[3.3 创建索引的一些建议](#t21)

## 1. 索引优化思路

都有哪些维度可以进行数据库调优？简言之：

*   索引失效、没有充分利用到索引——**建立索引**
    
*   关联查询太多 JOIN（设计缺陷或不得已的需求）——**SQL 优化**
    
*   服务器调优及各个参数设置（关闭慢查询日志、缓冲、线程数等）——**调整 my.cnf**
    
*   数据过多——**分库分表**
    
*   **定期清理垃圾：**对于不再使用的表、数据、日志、缓存等，应该及时清理，避免占用过多的 MySQL 资源，从而提高 MySQL 的性能。
    
*   **使用合适的存储引擎：**MyISAM 比较适合读取频繁，写入较少的场景（因为表级锁、B + 树叶存地址），而 InnoDB 比较适合并发写入的场景（因为行级锁、B + 树叶存记录）。
    

关于数据库调优的知识非常分散。不同的 DBMS，不同的公司，不同的职位，不同的项目遇到的问题都不尽相同。这里我们分为三个章节进行细致讲解。

虽然 SQL 查询优化的技术有很多，但是大方向上完全可以分成**物理查询优化**和**逻辑查询优化**两大块。

*   **物理查询优化**：**索引**和**表连接方式**等技术来进行优化，这里重点需要掌握索引的使用。
    
*   **逻辑查询优化**：通过 SQL **等价变换**提升查询效率，直白一点就是说，换一种查询写法效率可能更高。
    

## 2. 索引失效的 11 种情况

**用不用索引，最终都是优化器说了算：**

优化器是基于什么的优化器? 基于 **cost 开销 (CostBaseOptimizer)**，它不是基于规则 (Rule-Basedoptimizer)，也不是基于语义。怎么样开销小就怎么来。另外，SQL 语句是否使用索引，跟数据库版本、数据量、数据选择度都有关系。

### 2.0. 数据准备

**学员表**插 **50 万** 条，**班级表** 插 **1 万** 条。

**步骤 0：建库** 

```
CREATE DATABASE test;
USE test;
```

**步骤 1：建表**

```
CREATE TABLE `class` (
    `id` INT(11) NOT NULL AUTO_INCREMENT,
    `className` VARCHAR(30) DEFAULT NULL,
    `address` VARCHAR(40) DEFAULT NULL,
    `monitor` INT NULL ,
    PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
 
CREATE TABLE `student` (
    `id` INT(11) NOT NULL AUTO_INCREMENT,
    `stuno` INT NOT NULL ,
    `name` VARCHAR(20) DEFAULT NULL,
    `age` INT(3) DEFAULT NULL,
    `classId` INT(11) DEFAULT NULL,
    PRIMARY KEY (`id`)
    #CONSTRAINT `fk_class_id` FOREIGN KEY (`classId`) REFERENCES `t_class` (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

**步骤 2：设置参数**

*   命令开启：允许创建函数设置：

```
DELIMITER //
 
CREATE FUNCTION rand_string(n INT) RETURNS VARCHAR(255)
DETERMINISTIC
NO SQL
BEGIN
    DECLARE chars_str VARCHAR(100) DEFAULT 'abcdefghijklmnopqrstuvwxyzABCDEFJHIJKLMNOPQRSTUVWXYZ';
    DECLARE return_str VARCHAR(255) DEFAULT '';
    DECLARE i INT DEFAULT 0;
    WHILE i < n DO
        SET return_str = CONCAT(return_str,SUBSTRING(chars_str,FLOOR(1+RAND()*52),1));
        SET i = i + 1;
    END WHILE;
    RETURN return_str;
END //
 
DELIMITER ;
```

**步骤 3：创建函数**

保证每条数据都不同。

```
DELIMITER //
 
CREATE FUNCTION rand_num (from_num INT ,to_num INT) RETURNS INT(11)
DETERMINISTIC
NO SQL
BEGIN
    DECLARE i INT DEFAULT 0;
    SET i = FLOOR(from_num +RAND()*(to_num - from_num+1)) ;
    RETURN i;
END //
 
DELIMITER ;
```

随机产生班级编号

```
#创建往stu表中插入数据的存储过程
DELIMITER //
CREATE PROCEDURE insert_stu( START INT , max_num INT )
BEGIN
DECLARE i INT DEFAULT 0;
SET autocommit = 0; #设置手动提交事务
REPEAT #循环
SET i = i + 1; #赋值
INSERT INTO student (stuno, name ,age ,classId ) VALUES
((START+i),rand_string(6),rand_num(1,50),rand_num(1,1000));
UNTIL i = max_num
END REPEAT;
COMMIT; #提交事务
END //
DELIMITER ;
#假如要删除
#drop PROCEDURE insert_stu;
```

**步骤 4：创建存储过程**

```
#执行存储过程，往class表添加随机数据
DELIMITER //
CREATE PROCEDURE `insert_class`( max_num INT )
BEGIN
DECLARE i INT DEFAULT 0;
SET autocommit = 0;
REPEAT
SET i = i + 1;
INSERT INTO class ( classname,address,monitor ) VALUES
(rand_string(8),rand_string(10),rand_num(1,100000));
UNTIL i = max_num
END REPEAT;
COMMIT;
END //
DELIMITER ;
#假如要删除
#drop PROCEDURE insert_class;
```

创建往 class 表中插入数据的存储过程

```
#执行存储过程，往class表添加1万条数据
CALL insert_class(10000);
```

**步骤 5：调用存储过程**

class

```
#执行存储过程，往stu表添加50万条数据
CALL insert_stu(100000,500000);
```

stu

```
DELIMITER //
CREATE PROCEDURE `proc_drop_index`(dbname VARCHAR(200),tablename VARCHAR(200))
BEGIN
        DECLARE done INT DEFAULT 0;
        DECLARE ct INT DEFAULT 0;
        DECLARE _index VARCHAR(200) DEFAULT '';
        DECLARE _cur CURSOR FOR SELECT index_name FROM
information_schema.STATISTICS WHERE table_schema=dbname AND table_name=tablename AND
seq_in_index=1 AND index_name <>'PRIMARY' ;
#每个游标必须使用不同的declare continue handler for not found set done=1来控制游标的结束
        DECLARE CONTINUE HANDLER FOR NOT FOUND set done=2 ;
#若没有数据返回,程序继续,并将变量done设为2
        OPEN _cur;
        FETCH _cur INTO _index;
        WHILE _index<>'' DO
            SET @str = CONCAT("drop index " , _index , " on " , tablename );
            PREPARE sql_str FROM @str ;
            EXECUTE sql_str;
            DEALLOCATE PREPARE sql_str;
            SET _index='';
            FETCH _cur INTO _index;
        END WHILE;
    CLOSE _cur;
END //
DELIMITER ;
```

**步骤 6：删除某表上的索引**

创建存储过程

```
CREATE TABLE person_info(
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    birthday DATE NOT NULL,
    phone_number CHAR(11) NOT NULL,
    country varchar(100) NOT NULL,
    PRIMARY KEY (id),
    KEY idx_name_birthday_phone_number (name(10), birthday, phone_number)
);
```

执行存储过程

```
CREATE INDEX idx_name ON student(NAME);
#1.函数导致索引失效，没有走索引
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE LEFT(student.name,3) = 'abc';
#索引优化成like，走索引
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.name LIKE 'abc%';
 
#2.计算导致索引失效
CREATE INDEX idx_sno ON student(stuno);
EXPLAIN SELECT SQL_NO_CACHE id, stuno, NAME FROM student WHERE stuno+1 = 900001;
```

### 2.1 要尽量满足全值匹配

查询 age and classId and name 时，(age,classId,name)索引比 (age,classId) 快。

查询顺序不重要，例如查询 age and classId and name 和查询 classId and name and age 是一样的。

**分析查询计划：**

```
CREATE INDEX idx_name ON student(NAME);
 
#1.手动类型转换，通过调用函数，导致索引失效
EXPLAIN SELECT id, stuno, name FROM student WHERE name=CAST(123 as CHAR);
 
#2.自动类型转换导致索引失效。name字段类型是varchar，你赋值成数字它会默认转成字符串导致索引失败
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE name=123;
# 索引优化成目标字符串，走索引
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE name='123';
```

发现没有走索引：

![](<assets/1740030620986.png>)

![](<assets/1740030621195.png>)

**建立 age 索引**

```
CREATE INDEX idx_age_classid_name ON student(age,classId,name);
 
#都不使用范围查询。key_len为73
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.age=30 AND student.classId=4 AND student.name = 'abcd';
 
#联合索引第一个参数age使用范围查询，全部索引失效。ken_len为0，全表扫描。
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.age>30 AND student.classId=4 AND student.name = 'abcd';
 
 
#联合索引第二个参数classId使用范围查询，name索引失效。key_len为10，即只用age和classId。
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.age=30 AND student.classId>3 AND student.name = 'abcd';
 
#联合索引第三个参数name使用范围查询，索引都生效。key_len为73
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.age=30 AND student.classId=4 AND student.name > 'abcd';
```

再次分析查询计划，发现走索引：

![](<assets/1740030621321.png>)

![](<assets/1740030621515.png>)

**建立 (age,classId) 索引**

```
CREATE INDEX idx_age_name ON student(age, NAME);
#查所有字段，并且使用“不等于”，索引失效
EXPLAIN SELECT * FROM student WHERE age <> 20;
```

再次分析查询计划，发现走索引：

![](<assets/1740030621647.png>)

![](<assets/1740030621792.png>)

**建立 (age,classId,name) 索引**

```
CREATE INDEX idx_age_name ON student(age, NAME);
#查的两个字段正好被联合索引“idx_age_name ”覆盖了，索引成功
EXPLAIN SELECT age,name FROM student WHERE age <> 20;
```

再次分析查询计划，发现走索引：

![](<assets/1740030621971.png>)

### 2.2 要满足最佳左前缀法则

在 MySQL 建立联合索引时会遵守最佳左前缀原则，即最左优先，在检索数据时从联合索引的最左边开始匹配。

例如索引 (a,b,c)，只有查询(a),(a,b),(a,b,c) 会走索引，而 (b),(b,c),(c) 都不会走索引。

举例 1：

```
#没覆盖索引的情况下，左模糊查询导致索引失效
CREATE INDEX idx_age_name ON student(age, NAME);
EXPLAIN SELECT * FROM student WHERE NAME LIKE '%abc';
```

举例 2：

```
#有覆盖索引的情况下，左模糊查询索引生效
CREATE INDEX idx_age_name ON student(age, NAME);
EXPLAIN SELECT id,age,NAME FROM student WHERE NAME LIKE '%abc';
```

举例 3：索引 **idx_age_classid_name** 还能否正常使用？

```
# 未使用到索引
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age = 10 OR classid = 100;
```

如果索引了多列，要遵守最左前缀法则。指的是查询从索引的最左前列开始并且不跳过索引中的列。

```
#使用到索引
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age = 10 OR name = 'Abel';
```

![](<assets/1740030622171.png>)

虽然可以正常使用，但是只有部分被使用到了。

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

![](<assets/1740030622512.png>)

完全没有使用上索引。

结论：MySQL 可以为多个字段创建索引，一个索引可以包含 16 个字段。对于多列索引，**过滤条件要使用索引必须按照索引建立时的顺序，依次满足，一旦跳过某个字段，索引后面的字段都无法被使用**。如果查询条件中没有用这些字段中第一个字段时，多列（或联合）索引不会被使用。

### 2.3 主键插入顺序尽量自增

对于一个使用 InnoDB 存储引擎的表来说，在我们没有显式的创建索引时，表中的数据实际上都是存储在 聚簇索引的叶子节点的。而记录又是存储在数据页中的，数据页和记录又是按照记录 主键值从小到大 的顺序进行排序所以如果我们 插入 的记录的 主键值是依次增大 的话，那我们每插满一个数据页就换到下一个数据页继续插，而如果我们插入的 主键值忽大忽小的话，就比较麻烦了，假设某个数据页存储的记录已经满了，它存储的主键值在 1~100 之间:

![](<assets/1740030622691.png>)

如果此时再插入一条主键值为 9 的记录，那它插入的位置就如下图：

![](<assets/1740030622915.png>)

可这个数据页已经满了，再插进来咋办呢？我们需要把当前 **页面分裂** 成两个页面，把本页中的一些记录移动到新创建的这个页中。页面分裂和记录移位意味着什么？意味着： **性能损耗** ！所以如果我们想尽量避免这样无谓的性能损耗，最好让插入的记录的 **主键值依次递增** ，这样就不会发生这样的性能损耗了。 所以我们建议：让主键具有 **AUTO_INCREMENT** ，让存储引擎自己为表生成主键，而不是我们手动插入 ， 比如： **person_info** 表：

```
-- 创建复合索引
CREATE INDEX idx_a_b_c ON test(a, b, c);
 
-- 创建单列索引
CREATE INDEX idx_b ON test(b);
CREATE INDEX idx_c ON test(c);
 
-- 注意此时再创建idx_a_b、idx_a就没意义了，即使创建后也用不到，浪费资源。
-- 理由：当多个索引（联合或非联合）第一个字段都是a字段时，那么只要查询条件中有a，则会走最早创建的那个索引。
-- CREATE INDEX idx_a_b_c ON test(a, b, c);
-- CREATE INDEX idx_b ON test(a);
```

我们自定义的主键列 **id** 拥有 **AUTO_INCREMENT** 属性，在插入记录时存储引擎会自动为我们填入自增的主键值。这样的主键占用空间小，顺序写入，减少页分裂。

### 2.4 计算、函数导致索引失效

下面 sql，第一条走索引，第二条没有走索引：

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

测试前先删除其他索引 

### 2.5 类型转换导致索引失效

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

测试前先删除其他索引 

### 2.6 范围条件右边的列索引失效

例如（a,b,c）联合索引，查询条件 a,b,c，如果 b 使用了范围查询，那么 b 右边的 c 索引失效。这里右边看的联合索引的键右边。

解决办法：新建联合索引（a,c,b）或（c,a,b），把需要范围查询的字段放在最后

范围包括：(<) (<=) (>) (>=) 和 between。

注意：模糊查询 “like” 不是范围查询。

**案例：** 

```
CREATE INDEX idx_age_classid_name ON student(age,classId,name);
 
#都不使用范围查询。key_len为73
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.age=30 AND student.classId=4 AND student.name = 'abcd';
 
#联合索引第一个参数age使用范围查询，全部索引失效。ken_len为0，全表扫描。
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.age>30 AND student.classId=4 AND student.name = 'abcd';
 
 
#联合索引第二个参数classId使用范围查询，name索引失效。key_len为10，即只用age和classId。
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.age=30 AND student.classId>3 AND student.name = 'abcd';
 
#联合索引第三个参数name使用范围查询，索引都生效。key_len为73
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.age=30 AND student.classId=4 AND student.name > 'abcd';
```

应用开发中范围查询，例如：金额查询，日期查询往往都是范围查询。创建的联合索引中，务必把范围涉及到的字段写在最后，然后将范围查询条件放置 where 语句最后。

### 2.7 没覆盖索引时，“不等于” 导致索引失效

因为 “不等于” 不能精准匹配，全表扫描二级索引树再回表效率不如直接全表扫描聚簇索引树。但使用覆盖索引时，联合索引数据量小，加载到内存所需空间比聚簇索引树小，且不需要回表，索引效率优于全表扫描聚簇索引树。**覆盖索引：**一个索引包含了满足查询结果的数据就叫做覆盖索引，不需要回表等操作。 

**不等于符号：**!= 或者 <>，这两个符号意义相同。

没覆盖索引的情况下，使用 “不等于” 导致索引失效。因为如果使用索引，则需要依次遍历非聚簇索引 B + 树里所有叶节点，时间复杂度 O(n)，找到记录后还要回表，加在一起效率不如全表扫描，所以查询优化器就选择全表扫描了。

**案例：** 

 没覆盖索引的情况下，使用 “不等于” 导致索引失效。因为如果使用索引，则需要依次遍历非聚簇索引 B + 树里所有叶节点，时间复杂度 O(n)，找到记录后还要回表，加在一起效率不如全表扫描，所以查询优化器就选择全表扫描了。

```
CREATE INDEX idx_age_name ON student(age, NAME);
#查所有字段，并且使用“不等于”，索引失效
EXPLAIN SELECT * FROM student WHERE age <> 20;
```

![](<assets/1740030623085.png>)

覆盖索引，查的两个字段被联合索引给覆盖了，性能更高。虽然还是需要依次遍历非聚簇索引 B + 树里所有叶节点，时间复杂度 O(n)，但是不需要回表了，整体效率比不用索引更高，查询优化器就又使用索引了。

```
CREATE INDEX idx_age_name ON student(age, NAME);
#查的两个字段正好被联合索引“idx_age_name ”覆盖了，索引成功
EXPLAIN SELECT age,name FROM student WHERE age <> 20;
```

![](<assets/1740030623248.png>)

**案例 2：**

*   为 name 字段创建索引

```
CREATE INDEX idx_name ON student(NAME);
```

*   查看索引是否失效

```
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.name <> 'abc';
```

![](<assets/1740030623650.png>)

或者

```
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.name != 'abc';
```

![](<assets/1740030623790.png>)

场景举例：用户提出需求，将财务数据，产品利润金额不等于 0 的都统计出来。

### 2.8 没覆盖索引时，is not null、not like 导致索引失效

因为 is not null、not like 不能精准匹配，全表扫描二级索引树再回表效率不如直接全表扫描聚簇索引树。但使用覆盖索引时，联合索引数据量小，加载到内存所需空间比聚簇索引树小，且不需要回表，索引效率优于全表扫描聚簇索引树。**覆盖索引：**一个索引包含了满足查询结果的数据就叫做覆盖索引，不需要回表等操作。 

is null 可以使用索引，is not null 无法使用索引 

**原因：**

在进行索引扫描时，MySQL 会优先利用索引中已经存在的值进行查询，在查询时直接跳过为 NULL 的那些行。但是，如果使用了 IS NOT NULL 条件，那么 MySQL 无法在索引中找到 NULL 值，也就是说，MySQL 无法像前面那样跳过那些为 NULL 的行，只能扫描整张表来找到符合条件的行，因此无法使用索引。

**解决方案：**

*   **设计数据库的时候就将字段设置为 NOT NULL 约束**
*   将 INT 类型的字段，默认值设置为 0。
*   将字符类型的默认值设置为空字符串 ('')。

扩展：同理，在查询中使用 **not like** 也无法使用索引，导致全表扫描。 

*   IS NULL: 可以触发索引

```
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age IS NULL;
```

*   IS NOT NULL: 无法触发索引。

```
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age IS NOT NULL;
```

![](<assets/1740030623966.png>)

### 2.9 没覆盖索引时，左模糊查询导致索引失效

因为字符串开头都不能精准匹配，全表扫描二级索引树再回表效率不如直接全表扫描聚簇索引树。但使用覆盖索引时，联合索引数据量小，加载到内存所需空间比聚簇索引树小，且不需要回表，索引效率优于全表扫描聚簇索引树。

**覆盖索引：**一个索引包含了满足查询结果的数据就叫做覆盖索引，不需要回表等操作。

没覆盖索引的情况下，like 以通配符 “%” 或“_”开头，导致索引失效。在使用 LIKE 关键字进行查询的查询语句中，如果匹配字符串的第一个字符为'%'，索引就不会起作用。只有'%'不在第一个位置，索引才会起作用。

**原因：**没覆盖索引的情况下，字符串开头都不知道是什么，走索引再回表效率不如直接全表扫描

Alibaba《Java 开发手册》：**【强制】页面搜索严禁左模糊或者全模糊**，如果需要请走搜索引擎来解决。

 **没覆盖索引的情况下，左模糊查询导致索引失效**

```
#没覆盖索引的情况下，左模糊查询导致索引失效
CREATE INDEX idx_age_name ON student(age, NAME);
EXPLAIN SELECT * FROM student WHERE NAME LIKE '%abc';
```

![](<assets/1740030624090.png>)

**覆盖索引情况下，左模糊查询索引生效**

主要原因也是因为走非聚簇索引 B + 树遍历叶节点，不回表，效率会比全表扫描时高，查询优化器选择效率高的方案。

```
#有覆盖索引的情况下，左模糊查询索引生效
CREATE INDEX idx_age_name ON student(age, NAME);
EXPLAIN SELECT id,age,NAME FROM student WHERE NAME LIKE '%abc';
```

![](<assets/1740030624187.png>)

*   使用到索引

```
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE name LIKE 'ab%';
```

![](<assets/1740030624287.png>)

*   未使用到索引

```
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE name LIKE '%ab%';
```

![](<assets/1740030624414.png>)

### 2.10 “OR” 前后存在非索引列，导致索引失效

MySQL 里，即使 or 左边条件满足，右边条件依然要进行判断。 

在 WHERE 子句中，如果在 OR 前的条件列进行了索引，而在 OR 后的条件列没有进行索引，那么索引会失效。

**index_merge：**OR 前后的两个条件中的列都是索引时，查询中才使用索引，索引类型是 “index_merge”，将两个索引字段分别扫描，然后合并。

因为 OR 的含义就是两个只要满足一个即可，因此**只有一个条件列进行了索引是没有意义的**，只要有条件列没有进行索引，就会进行**全表扫描**，因此所以的条件列也会失效。

查询语句使用 OR 关键字的情况：

```
# 未使用到索引
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age = 10 OR classid = 100;
```

![](<assets/1740030624706.png>)

因为 classId 字段上没有索引，所以上述查询语句没有使用索引。

```
#使用到索引
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age = 10 OR name = 'Abel';
```

![](<assets/1740030624898.png>)

因为 age 字段和 name 字段上都有索引，所以查询中使用了索引。你能看到这里使用到了 index_merge，简单来说 index_merge 就是对 age 和 name 分别进行了扫描，然后将这两个结果集进行了合并。这样做的好处就是避免了全表扫描。

### 2.11 不同字符集导致索引失败，建议 utf8mb4

不同的字符集进行比较前需要进行**转换**会造成索引失效。

数据库和表的字符集统一使用 utf8mb4。统一使用 utf8mb4(5.5.3 版本以上支持) 兼容性更好，统一字符集可以**避免由于字符集转换产生的乱码**。

### 2.12 总结

**尽量全值匹配：**查询 age and classId and name 时，(age,classId,name)索引比 (age,classId) 快。

**考虑最左前缀：**联合索引把频繁查询的列放左。索引（a,b,c），只能查 (a,b,c),(a,b),(a)。

**主键尽量自增：**如果主键不自增，需要查找目标位置再插入，并且如果目标位置所在数据页满了就必须得分页，造成性能损耗。

**计算、函数导致索引失效：**计算例如 where num+1=2，函数例如 abs(num) 取绝对值

**类型转换导致索引失效：**例如 name=123，而不是 name='123'。又例如使用了不同字符集。

**范围条件右边的列索引失效：**例如（a,b,c）联合索引，查询条件 a,b,c，如果 b 使用了范围查询，那么 b 右边的 c 索引失效。建议把需要范围查询的字段放在最后。范围包括：(<) (<=) (>) (>=) 和 between。

**没覆盖索引时，“不等于” 导致索引失效：**因为 “不等于” 不能精准匹配，全表扫描二级索引树再回表效率不如直接全表扫描聚簇索引树。但使用覆盖索引时，联合索引数据量小，加载到内存所需空间比聚簇索引树小，且不需要回表，索引效率优于全表扫描聚簇索引树。**覆盖索引：**一个索引包含了满足查询结果的数据就叫做覆盖索引，不需要回表等操作。

**没覆盖索引时，左模糊查询导致索引失效：**例如 LIKE '%abc'。因为字符串开头都不能精准匹配。跟上面一个道理。

**没覆盖索引时，is not null、not like 无法使用索引：**因为不能精准匹配。跟上面一个道理。

**“OR” 前后存在非索引列，导致索引失效：**MySQL 里，即使 or 左边条件满足，右边条件依然要进行判断。

**不同字符集导致索引失败：**建议 utf8mb4，不同的字符集进行比较前需要进行 转换 会造成索引失效。

## 3. 自测练习

### **3.1 练习题和答案解析**

看不懂看参考：

[MySQL 高级篇——性能分析工具_mysql 分析工具 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/130355955 "MySQL高级篇——性能分析工具_mysql分析工具-CSDN博客")

**假设：**表 a,b,c,d 四个字段，创建索引 (a),(b),(c),(a,b),(a,c)(a,b,c)

```
explain select * from test + 下面条件
```

<table><thead><tr><th>Where 语句</th><th>索引是否被使用</th><th>使用到哪些列</th></tr></thead><tbody><tr><td>where a = 3</td><td><p>Y。命中非唯一索引。</p><p>如果是 select a，则命中覆盖索引</p></td><td>a, b, c</td></tr><tr><td>where a = 3 and b =5</td><td>Y。命中非唯一索引。<p>如果是 select a,b，则命中覆盖索引</p></td><td>a, b, c</td></tr><tr><td>where a = 3 and b = 5 and c =4</td><td>Y。命中非唯一索引。<p>如果是 select a,b,c，则命中覆盖索引</p></td><td>a, b, c</td></tr><tr><td>where b=3</td><td>N。不符合最佳左前缀原则</td><td></td></tr><tr><td>where b=3 and c= 4</td><td>Y。没完全命中索引，即 using where</td><td>b</td></tr><tr><td>where d = 4</td><td>N。没命中索引，不符合最佳左前缀原则</td><td></td></tr><tr><td>where c = 3 and d =5</td><td>Y。没完全命中索引</td><td>c</td></tr><tr><td>where a = 3 and b&gt; 4 and c= 5</td><td><p>Y。范围查询 + 索引下推</p><p>（idx_abc 树上 a 精准, b 范围过滤后条件判断 c，</p><p>然后回表查 a,b,c,d）</p></td><td>a, b, c</td></tr><tr><td>where a is null and b is not null</td><td><p>Y。索引下推。如果是 select a,b,c,，</p><p>则 Using where; Using index，没完全命中索引，</p><p>is not null 导致索引失效</p></td><td>a, b, c</td></tr><tr><td>where a &lt;&gt;3</td><td><p>N。全表扫描。</p><p>没覆盖索引时，不等于导致索引失效</p></td><td></td></tr><tr><td>where abs(a)=3</td><td>N。全表扫描。函数导致索引失效</td><td></td></tr><tr><td><p><strong>b 改为 varchar 类型后：</strong></p><p>where a =3 and b like 'kk%' and c=4</p></td><td>Y。范围查询 + 索引下推</td><td>a, b, c</td></tr><tr><td><p>where a = 3 and b like 'kk' and c = 4</p></td><td><p>Y。索引下推。</p><p>注意 like 后没有通配符所以不是范围查询</p></td><td>a, b, c</td></tr><tr><td>where a = 3 and b like '%kk%' and c= 4</td><td>Y。索引下推。如果是 select a,b,c,，<p>则 Using where; Using index，没完全命中索引，</p><p>左模糊查询导致索引失效。</p></td><td>a, b, c</td></tr><tr><td>where a = 3 and b like 'k%kk%'</td><td>Y。索引下推。</td><td>a, b, c</td></tr></tbody></table>

### **3.2 验证**

#### 3.2.1 准备数据 

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
-- 创建复合索引
CREATE INDEX idx_a_b_c ON test(a, b, c);
 
-- 创建单列索引
CREATE INDEX idx_b ON test(b);
CREATE INDEX idx_c ON test(c);
 
-- 注意此时再创建idx_a_b、idx_a就没意义了，即使创建后也用不到，浪费资源。
-- 理由：当多个索引（联合或非联合）第一个字段都是a字段时，那么只要查询条件中有a，则会走最早创建的那个索引。
-- CREATE INDEX idx_a_b_c ON test(a, b, c);
-- CREATE INDEX idx_b ON test(a);
```

查看所有索引：

```
SHOW INDEX FROM test;
```

删除索引：

```
drop index idx_a on test;
```

#### 3.2.2 **测试 1：命中**覆盖索引

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

![](<assets/1740030625091.png>)

**解读：**

*   **ref：**命中了非唯一索引；
*   **using index：**使用了覆盖索引，即不需要回表；
*   **key 是 idx_a_b_c：**命中了 idx_a_b_c 这个索引

#### 3.2.3 测试 2：命中索引，需要回表

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

![](<assets/1740030625185.png>)

**解读：**

*   **ref：**命中了非唯一索引；
*   **key 是 idx_a_b_c：**命中了 idx_a_b_c 这个索引

**详细解读参考：**

[MySQL 高级篇——性能分析工具_mysql 分析工具 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/130355955 "MySQL高级篇——性能分析工具_mysql分析工具-CSDN博客")

### **3.3 创建索引的一些建议**

**一般性建议**

*   对于单列索引，尽量选择针对当前 query 过滤性更好的索引
*   在选择组合索引的时候，当前 query 中过滤性最好的字段在索引字段顺序中，位置越靠前越好。
*   在选择组合索引的时候，尽量选择能够当前 query 中 where 子句中更多的索引。
*   在选择组合索引的时候，如果某个字段可能出现范围查询时，尽量把这个字段放在索引次序的最后面。

**总之，书写 SQL 语句时，尽量避免造成索引失效的情况**