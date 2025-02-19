> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_43254100/article/details/131514645)

#### 文章目录

*   [MYSQL 概述](#MYSQL_8)
*   [SQL](#SQL_24)
*   *   [SQL 通用语法](#SQL_26)
    *   [SQL 分类](#SQL_35)
    *   [① DDL：增删改查 数据库、表、表中字段](#_DDL__49)
    *   *   [数据库操作：增删改查 表](#__51)
        *   [表操作：增删改查 表字段](#__80)
    *   [② DML：增删改 表中的数据](#_DML__182)
    *   *   [添加数据 insert](#_insert_184)
        *   [删除数据 delete](#_delete_218)
        *   [修改数据 update](#_update_240)
    *   [③ DQL 查询表中数据 select,from,where,order by, group by,having, limit](#_DQL__selectfromwhereorder_by_group_byhaving_limit_253)
    *   [④ DCL 管理数据库用户、控制数据库的访问权限](#_DCL__351)
*   [函数](#_385)
*   [约束](#_451)
*   [多表查询 (JOIN, 子查询)](#_JOIN_510)
*   *   [多表关系 1 对 1,1 对 N/N 对 1, N 对 N](#_111NN1_NN_512)
    *   [多表查询概述](#_537)
*   [事务](#_660)
*   *   [事务操作](#_678)
    *   [事务四大特性](#_725)
    *   [并发事务问题](#_738)
    *   [事务隔离级别](#_757)

基础篇：MYSQL 概述，SQL，函数，约束，多表查询，事务

进阶：存储引擎，索引，[SQL 优化](https://so.csdn.net/so/search?q=SQL%E4%BC%98%E5%8C%96&spm=1001.2101.3001.7020)，视图 / 存储过程 / 触发器，锁，InnoDB 核心，MYSQL 管理

运维：日志，主从复制，分库分表，读写分离

MYSQL 概述
--------

数据库：DataBase(DB)，是存储数据的仓库

数据库管理系统：DataBase Management System ，是操纵和管理数据库的大型软件

SQL：Structed Query Language，结构化查询语言，是操作关系型数据库的编程语言

`mysql [-h 127.0.0.1] [-P 3306] -u root -p`

-h 指定连接的主机地址；-P 指定连接端口号；-u 指定用户名 -p 指定用户名密码

关系型数据库 (RDBMS)：基于表存储数据，Oracle，MYSQL，SQL Server，PostgreSQL，SQLite，

![](https://i-blog.csdnimg.cn/blog_migrate/2f4f8e77ba13b22c292ed3e22bc4369f.png#pic_center)

SQL
---

### SQL 通用语法

1.  SQL 语句可以单行或者多行书写，以**分号结尾**
2.  SQL 语句可以使用空格 / 缩进增强语句可读性
3.  **MYSQL 数据库的 SQL 语句不区分大小写**，关键字建议使用大写
4.  注释：
    1.  单行注释 `-- 注释内容` 或者 `# 注释内容 (MYSQL特有`
    2.  多行注释 `/* */`

### SQL 分类

*   DDL(Data Definition Language) 数据定义语言：
    *   **操作数据库和表**，定义数据库对象：数据库，表，列等。关键字：create, drop,alter 等
        
    *   DML(Data Manipulation Language) 数据操作语言：
        
        *   增删改表中数据，对数据库中**表的数据进行增删改**。关键字：insert, delete, update 等
    *   DQL(Data Query Language) 数据查询语言：
        
        *   查询表中数据，用来**查询数据库中表的记录 (数据)**。关键字：select, where 等
    *   DCL(Data Control Language) 数据控制语言：
        
        *   **管理用户，授权**，定义数据库访问权限和安全级别及创建用户。关键字：GRANT， REVOKE 等

### ① DDL：增删改查 数据库、表、表中字段

#### 数据库操作：增删改查 表

**1.C(Create): 创建**

*   创建数据库：`create database 数据库名称;`
*   创建数据库，判断不存在，再创建：`create database if not exists 数据库名称;`
*   创建数据库，并指定字符集 `create database 数据库名称 character set 字符集名;`
*   练习： 创建 db4 数据库，判断是否存在，并制定字符集为 gbk
    *   `create database if not exists db4 character set gbk;`

**2.R(Retrieve)：查询**

*   查询所有数据库的名称: `show databases;`
*   查询某个数据库的字符集 / 创建语句 `show create database 数据库名称;`

**3.U(Update): 修改**

*   修改数据库的字符集 `alter database 数据库名称 character set 字符集名称;`

**4.D(Delete): 删除**

*   删除数据库 `drop database 数据库名称;`
*   判断数据库存在，存在再删除 `drop database if exists 数据库名称;`

**5. 使用数据库**

*   查询当前正在使用的数据库名称 `select database();`
*   使用数据库 `use 数据库名称;`

#### 表操作：增删改查 表字段

**1.C(Create): 创建**

```
create table 表名(
	列名1 数据类型1,
	列名2 数据类型2,
	....
	列名n 数据类型n  // 注意：最后一列，不需要加逗号（,）
);
// 举例：
create table student(
	id int,
	name varchar(32),
	age int ,
	score double(4,1),
	birthday date,
	insert_time timestamp
);

```

*   字段类型：
    
    *   数值类型
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/b2dfb482103469e63a83e7ff2d9e6f89.png#pic_center)
        
        举例：
        
        *   int：整数类型 `age TINYINT UNSIGNED,` 0-255 有符号
            
        *   double：小数类型 `score double(4,2)` double 整个长度为 4，允许出现 2 位小数
            
        *   TinyInt(1 字节), SmallInt(2 字节)，MediumInt(3 字节),Int/Integer(4 字节)
            
            Float (4 字节) Double(8 字节) Decimal 精度
            
            可以把以上数据类型设置为**有符号**或者**无符号**范围
            
    *   字符串类型
        
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/ddf32c69e834bb9c79df2167191a712b.png#pic_center)
    
    一般不用 Blob，而是把 Blob 直接存储成文件，而不是放在数据库中
    
    常用的就是 Char 和 VarChar，Char(最大存储长度)，超出最长存储长度报错
    
    *   Char 是定长的，当前字符串长度大小 < 最大存储长度，未占用的空间补零
        
    *   VarChar 是不定长的，在最大存储长度以内，只存储当前字符串大小
        
    *   因此长度一样的情况下，VarChar 比 Char 性能差，因为需额外计算存储空间大小
        
    
    举例：
    
    *   用户昵称`username varchar(50)` 建议使用`varchar` 因为昵称字符串大小变化太大
    *   性别 `gender char(1)` 建议用`char`因为长度只用一位即可
    
    *   日期和时间戳 类型
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/5d6ab7732157a18518805ce8e145d5f4.png#pic_center)
        
        1.  date：日期，年月日，yyyy-MM-dd
        2.  datetime：日期，年月日时分秒 yyyy-MM-dd HH:mm:ss
        3.  timestamp：时间戳，年月日时分秒 yyyy-MM-dd HH:mm:ss
            *   如果不给这个字段赋值，或赋值为 null，则默认使用当前的系统时间，自动赋值
        
        举例：保存朋友的生日 `birthday date`
        

复制表：`create table 表名 like 被复制的表名;`

**2.R(Retrieve)：查询**

*   查询某个数据库中所有的表名称 `show tables;`
*   查询表结构 `desc 表名;`

**3.U(Update): 修改**

*   添加字段 `alter table 表名 add 列名 数据类型(长度) 约束;`
    
    例如：`alter table emp_info add nickname varchar(50);`
    
*   修改数据类型 `alter table 表名 modify 列名 新数据类型;`
    
    修改字段名和字段类型 `alter table 表名 change 列名 新列别 新数据类型;`
    
    例如：`alter emp_info change nickname username varchar(30);`
    
*   删除字段 `alter table 表名 drop 字段名;`
    
*   修改表名 `alter table 表名 rename to 新的表名;`
    
*   修改表的字符集 `alter table 表名 character set 字符集名称;`
    

**4.D(Delete): 删除**

*   `drop table 表名;` 或 `drop table if exists 表名 ;`

### ② DML：增删改 表中的数据

#### 添加数据 insert

*   指定字段添加数据 `insert into 表名(列名1,列名2,...列名n) values(值1,值2,...值n);`
    
*   全部字段添加数据 `insert into 表名 values(值1,值2, ... 值n);`
    
*   批量添加数据
    
    `isnert into 表名(字段1,字段2,...) values(值1,值2..),(值1,值2,..),(值1,值2,..);`
    
    `insert into 表名 values(值1,值2..),(值1,值2,..),(值1,值2,..);`
    

注意：

*   插入数据时，字段名顺序 与 值 顺序 要一一对应
*   除了数字类型，其他类型需要使用引号 (单双都可以) 引起来
*   插入数据大小，应该在数据类型范围内
*   如果表名后，不定义列名，则默认给所有列添加值

```
#增加单挑数据 方式1：
insert employee(id,worker_idx,name,gender,age,idCard,DateOfJoin) value(1,'1','张莉','女','23','111444555566663222','2022-01-01');

# 增加单挑数据 方式2：
insert employee value(2,'2','王无','女','20','566663222111444555','2020-01-01');

# 增加批量数据  方式1：省略;方式2：  注意要使用逗号分割
insert employee value
(3,'3','小红','女','30','614432221156664555','2000-01-01'),
(4,'4','小黄','女','31','455322211566661445','1999-01-01'),
(5,'5','小蓝','男','35','566322211661444555','1980-01-01'),
(6,'6','小紫','男','35','213221566661444555','1987-01-01');

```

#### 删除数据 delete

*   语法： `delete from 表名 [where 条件]`
    
*   注意：
    
    *   DELETE 语句的条件可以有，也可以没有，如果不加条件，则删除表中所有数据
        
    *   DELETE 语句不能删除某个字段的值
        
    *   如果要删除所有记录
        
        *   delete from 表名; – 不推荐使用。有多少条记录就会执行多少次删除操作
            
        *   TRUNCATE TABLE 表名; – 推荐使用，效率更高 先删除表，然后再创建一张一样的表
            

```
 delete from employee where gender = '男';
 delete from employee; #没有where条件，删除全部数据

```

#### 修改数据 update

*   语法： `update 表名 set 字段1 = 值1, 字段2 = 值2,... [where 条件];`
*   注意： 修改语句的条件可以有，也可以没有，如果不加任何条件，则会将表中所有记录全部修改。

```
update employee set name='ITtest' where id = 1;
update employee set name='小朝',gender='男' where id = 1;
update employee set DateOfJoin='2008-01-01'; # 没有where条件，修改所有数据

```

### ③ DQL 查询表中数据 select,from,where,order by, group by,having, limit

```
# 编写顺序
select 字段列表 from 表名列表 
group by 分组字段列表 having 分组后条件列表 
order by 排序字段列表 
limit 分页参数;
#执行顺序 from-> where -> group -> select -> order by, having -> limit

```

语法：`select * from 表名;`

*   **基本查询（不带任何条件）**
    
    *   查询多个字段 `select 字段1,字段2,字段3.. from 表名;`
        
        查询所有字段 `select * from 表名;`
        
    *   设置别名 `select 字段1 as 别名1, 字段2 as 别名2... from 表名;`
        
    *   对查询结果进行去重 `select distinct from 表名;`
        
*   **条件查询（WHERE）**
    
    *   `select 字段列表 from where 条件列表`
        
        *   条件列表——比较运算符
            
            `>,>=,<,<=,=,!=,`
            
            `Between..And(某个范围内 含最大值和最小值),`
            
            `In(..)在in之后的列表中的值，多选一`
            
            `Like 模糊匹配 占位符(_匹配单个字符, %匹配任意多个字符)`
            
            `is null` 是否是 NULL
            
        *   条件列表——逻辑运算符 且，或，非
            
            `and 或 &&` ；`or 或 ||` ；`not 或 !`
            
*   **聚合函数（count、max、min、avg、sum）**
    
    将一列数据作为一个整体，进行纵向计算，用于某一列
    
    `select 聚合函数(字段列表) from 表名;`
    
    *   count：计算个数（一般选择非空的列：主键；`count(*)`查询所有的数据个数）
        
    *   max：计算最大值；min：计算最小值；sum：计算和；avg：计算平均值
        
        *   注意：所有 Null 值不参与聚合运算
*   **分组查询（group by）**
    
    `select 字段列表 from 表名 [where 条件] group by 分组字段名 [having 分组后的过滤条件];`
    
    *   where 和 having 区别：
        
        *   执行时机不同：
            
            where 是分组之前进行过滤，不满足 where 条件，则不参与分组；
            
            而 having 是分组 之后对结果进行过滤。
            
        *   判断条件不同：where 不能对聚合函数进行判断，而 having 可以。
            
    *   注意：
        
        *   执行顺序：where > 聚合函数 > having
        *   分组之后，having 查询的字段一般为聚合函数和分组字段，查询其它字段无意义
*   **排序查询（order by）**
    
    `SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1 , 字段2 排序方式2 ;`
    
    *   ASC 升序（默认值），DESC 降序
        
    *   如果是多字段排序，当地一个字段值相同时，才会根据第二个字段进行排序
        
*   **分页查询（limit）**
    
    `SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询记录数 ;`
    
    分页操作在业务系统开发时，也是非常常见的一个功能，我们在网站中看到的各种各样的分页条，后台 都需要借助于数据库的分页操作。
    
    *   注意:
        *   起始索引从 0 开始，起始索引 =（查询页码 - 1）* 每页显示记录数
        *   分页查询是数据库的方言，不同的数据库有不同的实现，MySQL 中是 LIMIT
        *   如果查询的是第一页数据，起始索引可以省略，直接简写为 limit 10

### ④ DCL 管理数据库用户、控制数据库的访问权限

*   用户管理
    
    *   使用用户 `use mysql;`
    *   查询用户 `select * from user;`
    *   创建用户 `CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';`
    *   修改用户密码 `ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码' ;`
    *   删除用户 `DROP USER '用户名'@'主机名' ;`
    *   注意：
        *   在 MySQL 中需要通过用户名 @主机名的方式，来唯一标识一个用户
        *   主机名可以使用 % 通配
        *   开发人员操作较少，主要是 DBA（ Database Administrator 数据库管理员）使用
*   权限控制
    

![](https://i-blog.csdnimg.cn/blog_migrate/574d0bf8f8f5da06e856dc502ef3b4e7.png#pic_center)

*   查询权限 `SHOW GRANTS FOR '用户名'@'主机名' ;`
    
*   授予权限 `GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';`
    
*   撤销权限 `REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';`
    
*   注意：
    
    *   多个权限之间，使用逗号分隔
        
    *   授权时， 数据库名和表名可以使用 * 进行通配，代表所有
        
        例如`GRANT 权限列表 ON 数据库名.* TO '用户名'@'主机名';` 给用户分配数据库名的所有表
        
        例如`GRANT 权限列表 ON *.* TO '用户名'@'主机名';` 给该用户分配所有数据库的所有表
        

函数
--

函数：一段可以直接被另一段程序调用的程序或代码，调用内置函数实现业务需求

MySQL 中的函数主要分为以下四类： 字符串函数、数值函数、日期函数、流程函数。

`select 函数(参数)`

*   **字符串函数**
    
    *   `CONCAT(S1,S2,...Sn)` 字符串拼接，将 S1，S2，… Sn 拼接成一个字符串
    *   `LOWER(str)` 将字符串 str 全部转为小写
    *   `UPPER(str)` 将字符串 str 全部转为大写
    *   `LPAD(str,n,pad)` 左填充，用字符串 pad 对 str 的左边进行填充，达到 n 个字符串长度
    *   `RPAD(str,n,pad)` 右填充，用字符串 pad 对 str 的右边进行填充，达到 n 个字符 串长度
    *   `TRIM(str)` 去掉字符串头部和尾部的空格
    *   `SUBSTRING(str,start,len)` 返回从字符串 str 从 start 位置起的 len 个长度的字符串
*   **数值函数**
    
    *   `CEIL(x)` 向上取整
    *   `FLOOR(x)` 向下取整
    *   `MOD(x,y)` 返回 x/y 的模
    *   `RAND()` 返回 0~1 内的随机数
    *   `ROUND(x,y)` 求参数 x 的四舍五入的值，保留 y 位小数
*   **日期函数**
    
    *   `CURDATE()` 返回当前日期
    *   `CURTIME()` 返回当前时间
    *   `NOW()` 返回当前日期和时间
    *   `YEAR(date)` 获取指定 date 的年份
    *   `MONTH(date)` 获取指定 date 的月份
    *   `DAY(date)` 获取指定 date 的日期
    *   `DATE_ADD(date, INTERVAL expr type)` 返回一个日期 / 时间值加上时间间隔 expr 后的时间值
    *   `DATEDIFF(date1,date2)` 返回起始时间 date1 和 结束时间 date2 之间的天数
*   **流程控制函数** 可以在 SQL 语句中实现条件筛选，从而提高语句效率
    
    *   `IF(value , t , f)`
        
        如果 value 为 true，则返回 t，否则返回 f
        
    *   `IFNULL(value1 , value2)`
        
        如果 value1 不为空，返回 value1，否则 返回 value2
        
    *   `CASE WHEN [ val1 ] THEN [res1] ... ELSE [ default ] END`
        
        如果 val1 为 true，返回 res1，… 否 则返回 default 默认值
        
        `CASE [ expr ] WHEN [ val1 ] THEN [res1] ... ELSE [ default ] END`
        
        如果 expr 的值等于 val1，返回 res1，… 否则返回 default 默认值
        
        *   如果是具体的值 用第二条
        *   如果是一个范围 用第一条
        
        ```
        select id,name,(case when math >= 85 then '优秀' when math >= 60 then '及格' else '不及格' end) as '数学成绩' from score;  #不用case expr
        
        select name,case workaddress when '北京' then '一线城市' when '上海' then '一线城市'else '二线城市' end from emp; #使用case exptr
        
        ```
        

约束
--

约束是**作用于表中字段上**的规则，在**创建表 / 修改表的时候添加约束**，用于限制在表中的数据，保证了数据库中的正确、有效性和完整性。

*   **NOT NULL，非空约束**，限制该字段的数据不能为 null
    
*   **UNIQUE，唯一约束**，保证该字段的所有数据都是唯一、不重复的
    
*   **PRIMARY KEY，主键约束**，主键是一行数据的唯一标识，要求非空且唯一
    
    *   `自增关键字：auto_increment`
*   **DEFAULT，默认约束**，保存数据时，如果未指定该字段的值，则采用默认值
    
*   **CHECK，检查约束** (8.0.16 版本 之后)，保证字段值满足某一个条件
    
    ```
    create table user(
        id int primary key auto_increment,  # 主键约束 自增
        name varchar(10) not null unique,   # 非空  唯一
        age int check (age > 0 && age <= 120),  # 检查约束
        status char(1) default '1',  # 默认约束
        gender char(1) # 无约束
    );
    
    ```
    
*   **FOREIGN KEY，外键约束**，用来让两张表的数据之间建立连接，保证数据的一致性和完整性
    
    *   添加、删除 外键
    *   外键的删除 / 更新 行为
        *   NO ACTION。当在父表中删除 / 更新对应记录时，首先检查该记录是否有对应外键，如果有则不 允许删除 / 更新。 (与 RESTRICT 一致) **默认行为**
        *   RESTRICT。当在父表中删除 / 更新对应记录时，首先检查该记录是否有对应外键，如果有则不 允许删除 / 更新。 (与 NO ACTION 一致) **默认行为**
        *   CASCADE。当在父表中删除 / 更新对应记录时，首先检查该记录是否有对应外键，如果有，则 也删除 / 更新外键在子表中的记录。
        *   SET NULL。当在父表中删除对应记录时，首先检查该记录是否有对应外键，如果有则设置子表 中该外键值为 null（这就要求该外键允许取 null）。
        *   SET DEFAULT。父表有变更时，子表将外键列设置成一个默认的值 (Innodb 不支持)
    
    ```
    # 外键约束： 添加外键
    CREATE TABLE 表名(
    字段名 数据类型,
    ...
    [CONSTRAINT] [外键名称] FOREIGN KEY (外键字段名) REFERENCES 主表 (主表列名)
    );
    
    ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名)
    REFERENCES 主表 (主表列名) ;
    
    # 外键约束：删除外键
    alter table 表名 drop foreign key 外键名称;
    
    # 创建外键的同时 并指定外键的 删除/更新 行为
    ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段) REFERENCES
    主表名 (主表字段名) ON UPDATE CASCADE ON DELETE CASCADE;
    ALTER TABLE emp  ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段) REFERENCES 主表名 (主表字段名) ON UPDATE SET NULLON UPDATE SET NULL;
    
    ```
    

多表查询 (JOIN, 子查询)
----------------

### 多表关系 1 对 1,1 对 N/N 对 1, N 对 N

*   一对多 / 多对一
    
    *   例如：部门 1 和 员工 N，一个部门对应多个员工，一个员工对应一个部门
        
        实现：多的一方建立外键 指向一方的主键，作为子表
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/880a8cfa5377dc63a4892074bcc7a95f.png#pic_center)
        
*   多对多
    
    *   例如：学生 N 和 课程 N，一个学生选多个课程，一个课程供多个学生选择
        
        实现：建立中间表 含有两个外键，分别关联学生表和课程表的主键  
        ![](https://i-blog.csdnimg.cn/blog_migrate/545fd5a523b30e948843a31891659e21.png#pic_center)
        
*   一对一
    
    *   例如：用户于用户详情的关系，多用于单表拆分
        
        实现：在任意一方加入外键，关联另一方的主键，并且设置为 唯一 unique
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/3b528ff84a75cc8036dee22723be4eef.png#pic_center)
        

### 多表查询概述

对多张表进行查询

**普通查询**：

直接`select * from A,B;` 会发生笛卡尔积，集合 A 和 集合 B 组合的所有情况（在多表查询时，需要消除无效的笛卡尔积，解决`select * from A,B where A.aid = b.id;` 通过外键去重

![](https://i-blog.csdnimg.cn/blog_migrate/199288a8a27348c42420621edffa5967.png#pic_center)

连接查询：

*   **内连接：两张表之间的交集数据**

![](https://i-blog.csdnimg.cn/blog_migrate/6b7d229d9ab37b68d42270388b3d2c2e.png#pic_center)

*   隐式内连接 `SELECT 字段列表 FROM 表1 , 表2 WHERE 条件 ... ;`
    
*   显示内连接 `SELECT 字段列表 FROM 表1 [ INNER ] JOIN 表2 ON 连接条件 ... ;`
    
*   **外连接：**
    
    *   **左外连接：会返回表 1 的所有记录以及与表 1 关联的表 2 的匹配记录。**
        
        `SELECT 字段列表 FROM 表1 LEFT [ OUTER ] JOIN 表2 ON 条件 ... ;`
        
    *   **右外连接： RIGHT JOIN 会返回表 2 的所有记录以及与表 2 关联的表 1 的匹配记录**
        
        `SELECT 字段列表 FROM 表1 RIGHT [ OUTER ] JOIN 表2 ON 条件 ... ;`
        
*   **自连接：当前表与自身的连接查询，自连接必须使用表别名**
    
    `SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件 ... ;`
    
    对于自连接查询，可以是内连接查询，也可以是 (左 / 右) 外连接查询。
    
*   **联合查询**：union, union all
    
    对于 Union 查询，就是把多次查询结果合并，形成一个新的查询结果集
    
    ```
    SELECT 字段列表 FROM 表A ...
    UNION [ ALL ]
    SELECT 字段列表 FROM 表B ....;
    
    ```
    
    注意：
    
    *   字段列表和字段类型 上下两个表需要相同
    *   union all 直接合并两个结果集 会导致重复，而只用 union 则会进行去重

**子查询**： 分步骤操作即可

SQL 语句中嵌套 SELECT 语句，称为嵌套查询

`SELECT * FROM t1 WHERE column1 = ( SELECT column1 FROM t2 );`

子查询外部的语句可以是 INSERT / UPDATE / DELETE / SELECT 的任何一个

*   根据子查询结果不同，分为：
    
    *   **标量子查询（子查询结果为单个值）**
        
        *   子查询返回的结果是单个值（数字、字符串、日期等），称为标量子查询。
        *   常用的操作符：= <>> >= < <=
    *   **列子查询 (子查询结果为一列)**
        
        常用的操作符：
        
        *   IN 、在指定的集合范围之内，多选一
        *   NOT IN 、 不在指定的集合范围之内
        *   ANY 、子查询返回列表中，有任意一个满足即可
        *   SOME 、 与 ANY 等同，使用 SOME 的地方都可以使用 ANY
        *   ALL，子查询返回列表的所有值都必须满足
    *   **行子查询 (子查询结果为一行)**
        
        *   常用的操作符：= 、<> 、IN 、NOT IN
    *   **表子查询 (子查询结果为多行多列)**
        
        *   子查询返回的结果是多行多列，这种子查询称为表子查询。
        *   常用的操作符：IN
*   根据子查询位置，分为
    
    *   WHERE 之后
    *   FROM 之后
    *   SELECT 之后

```
# 内连接  查询每一个员工的姓名 , 及关联的部门的名称 (隐式内连接)(显示内连接)
select emp.name,dept.name from emp,dept where emp.dept_id = dept.id;
select emp.name,dept.name from emp inner join dept on emp.dept_id=dept.id;

# 外连接  查询dept表的所有数据, 和对应的员工信息
select d.*, e.* from dept as d left join emp as e on e.dept_id = d.id; #右外
select d.*, e.* from dept as d left join emp as e on e.dept_id = d.id; #左外

# 自连接  查询所有员工 emp 及其领导的名字 emp , 如果员工没有领导, 也需要查询出来
select a.name,b.name from emp as a left join emp as b on a.managerid = b.id;

# 联合查询 union all不去重，union去重  查询薪资低于5000的员工和年龄大于50岁的员工信息
select * from emp where salary < 5000 Union select * from emp where age > 50;

# 标量子查询（子查询结果为单个值） 查询 "销售部" 的所有员工信息
select * from emp where dept_id = (select id from dept where name = '销售部');

# 列子查询(子查询结果为一列)  查询 "销售部" 和 "市场部" 的所有员工信息
select * from emp where dept_id in (select id from dept where name in('销售部','市场部'));

# 行子查询(子查询结果为一行) 查询与 "张无忌" 的薪资及直属领导相同的员工信息 
select * from emp where (salary,managerid)=(select salary,managerid from emp where name ='张无忌');

# 表子查询(子查询结果为多行多列) 查询入职日期是 "2006-01-01" 之后的员工信息 , 及其部门信息
select e.*,d.* from (select * from emp where emp.entrydate > '2006-01-01') as e left join dept as d on e.dept_id = d.id;

```

事务
--

**事务** 是一组操作的集合，它是一个**不可分割的工作单位**，事务会把所有的操作作为**一个整体**一起向系 统提交或撤销操作请求，即**这些操作要么同时成功，要么同时失败**。

例如银行转账操作，张三给李四转账 1000 元，存在以下三个步骤：

1.  查询 张三 账户余额
2.  张三 账户 -1000
3.  李四 账户 +1000

如果第二步骤到第三步骤出现了异常情况报错，则出现数据不一致问题。因此需要事务来完成，业务逻辑执行之前开启事务，执行 完毕后提交事务。如果执行过程中报错，则回滚事务，把数据恢复到事务开始之前的状态。

![](https://i-blog.csdnimg.cn/blog_migrate/d691b379a056c2685632862ad5ed5a94.png#pic_center)

> 默认 MySQL 的事务是自动提交的，即执行完一条 DML 语句后，MySQL 会立即隐式的提交事务。

### 事务操作

将自动提交模式（autocommit mode）设置为手动提交模式：

*   查看 / 设置事务提交方式 修改事务的自动提交行为，从默认提交改为了手动提交，则此时的 DML 语句都不会提交，需要手动执行 commit 提交
    
    ```
    SELECT @@autocommit ;
    SET @@autocommit = 0 ;
    
    ```
    
*   提交事务
    
    ```
    COMMIT;
    
    ```
    
*   回滚事务
    
    ```
    ROLLBACK;
    
    ```
    

显式地开始一个事务：

*   开启手动提交事务
    
    ```
    START TRANSACTION; 或 BEGIN ;
    
    ```
    
*   提交事务
    
    ```
    COMMIT;
    
    ```
    
*   回滚事务
    
    ```
    ROLLBACK;
    
    ```
    

### 事务四大特性

事务的四大特性，简称 ACID：

*   原子性（**A**tomicity）：
    *   事务是不可分割的最小操作单元，**要么全部成功，要么全部失败**。
*   一致性（**C**onsistency）：
    *   事务完成时，必须使所有的数据都保持一致状态。
*   隔离性（**I**solation）：
    *   数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。
*   持久性（**D**urability）：
    *   事务一旦提交或回滚，它对数据库中的数据的改变就是永久的，（磁盘中的数据。

### 并发事务问题

多个并发事务在执行过程中出现脏读，不可重复读，幻读等问题

*   脏读：一个事务读到另外一个事务还没有提交的数据
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/7bf7d47d9bee343cb2fc1f0d699f5fd9.png#pic_center)
    
*   不可重复读：一个事务先后读取同一条记录，但两次读取的数据不同，称之为不可重复读
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/421cccf2b4c623a14a055c73da6d7986.png#pic_center)
    
*   幻读：一个事务按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据 已经存在，好像出现了 “幻影”。
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/940f1ad88cb30d293100fc30fa95d695.png#pic_center)
    

### 事务隔离级别

为了解决并发事务所引发的问题，在数据库中引入了事务隔离级别。主要有以下几种：

读未提交 < 读已提交 < 可重复读 < 串行化

事务隔离级别越高，数据越安全，但是性能越低。

![](https://i-blog.csdnimg.cn/blog_migrate/af9c63981801d1352d26e61debc502e9.png#pic_center)

*   查看事务隔离级别
    
    ```
    SELECT @@TRANSACTION_ISOLATION;
    
    ```
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/6b06cafde9259b681dc787901c271045.png#pic_center)
    
*   设置事务隔离级别
    
    ```
    SET [ SESSION | GLOBAL ] TRANSACTION ISOLATION LEVEL { READ UNCOMMITTED |
    READ COMMITTED | REPEATABLE READ | SERIALIZABLE }
    # 会话级别：SESSION 仅当前对话窗口有效，GLOBAL 对所有对话窗口有效
    # 后面指定的四种级别
    
    ```