---
url: https://blog.csdn.net/qq_40991313/article/details/125676205
title: JavaWeb 基础 1——MySQL_javaweb 模糊查询 mysql-CSDN 博客
date: 2025-02-20 09:37:20
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[一、概念](#t0)

[二、MySQL 下载安装配置](#t1)

[三、关系型数据库](#t2)

[四、SQL 语句](#t3)

[4.1 概述](#t4)

[4.2 DDL 数据定义语言](#t5)

[4.2.0 mysql 自带数据库](#t6)

[4.2.1 数据库的增删查、使用](#t7)

[4.2.2 DDL 查询表](#t8)

[4.2.3 DDL 创建表](#t9)

[4.2.4 三类数据类型，数值、日期、字符串](#t10)

[4.2.5 DDL 删除表](#t11)

[4.2.6 DDL 修改表](#t12)

[4.3 Navicat 下载安装使用](#t13)

[4.4 数据操作语言 DML](#t14)

[4.4.1 DML 添加数据](#t15)

[4.4.2 修改数据](#t16)

[4.4.3 删除数据](#t17)

[4.5 数据查询语言 DQL](#t18)

[4.5.1 查询的完整语法](#t19)

[4.5.2 创建练习查询的表](#t20)

[4.5.3 基础查询](#t21)

[4.5.4 条件查询（包括模糊查询）](#t22)

[4.5.5 排序查询](#t23)

[4.5.6 聚合函数](#t24)

[4.5.7 带条件的聚合函数：count(name='abcd' or null)](#4.5.7%20%E5%B8%A6%E6%9D%A1%E4%BB%B6%E7%9A%84%E8%81%9A%E5%90%88%E5%87%BD%E6%95%B0%EF%BC%9Acount%28name%3D'%20rel=)

[4.5.7 分组查询](#t25)

[4.5.8 分页查询](#t26)

[五、约束](#t27)

[5.1 概念](#t28)

[5.2 常用约束](#t29)

[5.2.1 介绍](#t30)

[5.2.2 常用命令](#t31)

[5.3 增删约束](#t32)

[5.4 外键约束](#t33)

[5.4.1 概述](#t34) 

[5.4.2 练习](#t35)

[六、数据库设计](#t36)

[6.1 概念](#t37)

[6.2 表关系](#t38)

[6.2.1 一对多](#t39)

[6.2.2 多对多](#t40)

[6.2.3 一对一](#t41)

[七、多表查询](#t42)

[7.1 创建练习的表](#t43)

[7.2 连接查询](#t44)

[7.2.1 概念](#t45) 

[7.2.2 内连接查询](#t46)

[7.2.3 自连接](#t47)

[7.2.4 递归查询](#t48)

[7.2.5 外连接查询](#t49)

[7.3 子查询](#t50)

[7.4 多表查询练习题：](#t51)

[八、事务](#t52)

[8.1 概念](#t53)

[8.2 语法](#t54)

[8.3 事务的四大特征](#t55)

[九、函数](#t56)

[9.1 数值型函数](#t57) 

[9.2 字符串函数](#t58)

[9.3 日期和时间函数](#t59)

[9.4 聚合函数](#t60)

[9.5 流程控制函数](#t61)

[十、SQL 练习题：高频 SQL 50 题（基础版）](#t62)

## 一、概念

![](<assets/1740015440838.png>)

**常用关系型数据库管理系统：** 

![](<assets/1740015441144.png>)

## 二、MySQL 下载安装配置

**第一步：去官网下载安装**

[MySQL :: Download MySQL Community Server (Archived Versions)](https://downloads.mysql.com/archives/community/ "MySQL :: Download MySQL Community Server (Archived Versions)")  

![](<assets/1740015441583.png>)

**第二步：配置**

先解压，然后在 mysql 下创建一个 **my.ini** 文件，更改 my.ini 文件里面的前两行安装目录，第二行加上 \ data。注意 my.ini 文件不能多一个符号或者少一个符号，第二行第三行改成自己的 MySQL 路径

![](<assets/1740015441871.png>)

```
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
# basedir=D:\\mysql\\mysql-8.0.26-winx64
basedir=D:\\ajavautils\\mysql-5.7.41-winx64 # 切记此处要么双斜杠\\，要么单斜杠/，单斜杠\会出错
# 设置mysql数据库的数据的存放目录，MySQL 8+ 可以不需要以下配置，系统自己生成即可，否则有可能报错
datadir=D:\\ajavautils\\mysql-5.7.41-winx64\\data # 此处同上
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=10
# 服务端使用的字符集默认为UTF8
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
default_authentication_plugin=mysql_native_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8
```

在 path（**环境变量**里面）加上 mysql 的 bin 路径（D:\ajavautils\mysql-5.7.41-winx64\bin）  
(填写自己的 mysql 安装路径)

![](<assets/1740015442092.png>)

  

![](<assets/1740015442432.png>)

**第三步：初始化**

进入命令指示符（在 bin 目录下运行 cmd）

![](<assets/1740015442676.png>)

  
输入下面命令初始化数据库，并设置默认 root 为空，初始化完成后，在 mysql 根目录中会自动生成 data 文件

```
# 设置数据目录和创建系统数据库和表。
# initialize-insecure指示 MySQL 初始化数据目录，但不会设置 root 用户的密码。这意味着初始化后的数据库实例将没有 root 密码，用户可以直接连接到 MySQL 服务器并设置密码。
mysqld --initialize-insecure --user=mysql
```

  
再输入 mysqld -install, 为 windows 安装 mysql 服务，默认服务名为 mysql

```
mysqld -install

```

  
出现 service successfully installed. 表示配置完成  

![](<assets/1740015442894.png>)

**第四步：启动数据库**

```
net start mysql

```

**解决发生系统错误 2。系统找不到指定的文件**

如果报错 “发生系统错误 2。系统找不到指定的文件。”，则以下方法解决：

![](<assets/1740015443175.png>)

1.  Win+R 输入 regedit 打开注册表
2.  HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MySQL 在该路径下找到 MySQL 映像文件 ImagePath。
3.  查看 ImagePath 里面本地安装 MySQL 的路径是否有误，如果有误修改相应的路径即可。

![](<assets/1740015443354.png>)

**第五步：初始化密码**

输入 mysql -u root -p 进行登录 。

**命令解读：**

*   mysql 是安装目录 bin 下的 mysql.exe 与服务 mysql 间进行通信
*   -u 后跟账户名 root
*   -p 后先不设置密码

```
mysql -u root -p

```

不用输入密码直接回车 

![](<assets/1740015443590.png>)

  
出现 mysql > 配置完成  
**修改密码：**

```
alter user user() identified by "你要设置的密码，例如1234";

```

![](<assets/1740015443834.png>)

  
mysql 退出 mysql>quit;

```
exit;

```

**其他命令（慎用）：**

 **关闭数据库**

```
net stop mysql

```

![](<assets/1740015444091.png>)

**卸载**

cmd 先停止服务

```
net stop mysql

```

再卸载服务

```
mysqld -remove mysql

```

最后删除目录和环境变量

## 三、关系型数据库

![](<assets/1740015444317.png>)

例如下面关系模型的二维表： 

![](<assets/1740015444614.png>)

数据库在 mysql 的 data 目录下。 

## 四、SQL 语句

### 4.1 概述

![](<assets/1740015444882.png>)

![](<assets/1740015445219.png>)

 注意单行注释 -- 后有空格。

![](<assets/1740015445440.png>)

![](<assets/1740015445665.png>)

### 4.2 DDL 数据定义语言

#### 4.2.0 mysql 自带数据库

![](<assets/1740015445983.png>)

**information_schema 是信息数据库。**

其中保存着关于 MySQL 服务器所维护的所有其他数据库的信息。如数据库名，数据库的表，表栏的数据类型与访问权 限等。在 INFORMATION_SCHEMA 中，有数个只读表。它们实际上是视图，而不是基本表，因此，你将无法看到与之相关的任何文件。

**mysql 核心数据库**，存储 MySQL 数据库里最核心的信息，例如权限、安全。

**sys：系统数据库。**

**performance_schema** 主要用于收集数据库服务器性能参数（研究性能调优要用到）

#### 4.2.1 数据库的增删查、使用

![](<assets/1740015446226.png>)

#### 4.2.2 DDL 查询表

**查询当前数据库下所有表名称**

```
SHOW TABLES;

```

**查询表结构**

```
DESC 表名称;        #desc是describe缩写，译为描述

```

#### 4.2.3 DDL 创建表

```
CREATE TABLE 表名 (
	字段名1  数据类型1,
	字段名2  数据类型2,
	…
	字段名n  数据类型n
);
```

 注意：字段名是列名。

最后一行末尾，不能加逗号

```
create table tb_user (
	id int,
    username varchar(20),    #sql语句中字符串是char和varchar类型
    password varchar(32)
);
```

![](<assets/1740015446504.png>)

#### 4.2.4 三类数据类型，数值、日期、字符串

*   **数值**
    
    ```
    tinyint : 小整数型，占一个字节
    int ： 大整数类型，占四个字节
        eg ： age int
    double ： 浮点类型
        使用格式： 字段名 double(总长度,小数点后保留的位数)
        eg ： score double(5,2)   
    ```
    
*   **日期**
    
    ```
    date ： 日期值要带引号。只包含年月日
        eg ：birthday date 
    ​​​​​​​time :  时间值或持续时间
    year :  年分值
    datetime ： 混合日期和时间值。包含年月日时分秒
    ```
    
*   **字符串。要带引号**
    
    ```
    char ： 定长字符串。
        优点：存储性能高
        缺点：浪费空间
        eg ： name char(10)  如果存储的数据字符个数不足10个，也会占10个的空间，汉字占1个字符
    varchar ： 变长字符串。
        优点：节约空间
        缺点：存储性能底
        eg ： name varchar(10) 如果存储的数据字符个数不足10个，那就数据字符个数是几就占几个的空间    
    ```
    

#### 4.2.5 DDL 删除表

*   **删除表**
    

```
DROP TABLE 表名;

```

*   **删除表时判断表是否存在**
    

```
DROP TABLE IF EXISTS 表名;

```

#### 4.2.6 DDL 修改表

关键字 rename，add，modify，change，drop

*   **修改表名**
    

```
ALTER TABLE 表名 RENAME TO 新的表名;
​
-- 将表名student修改为stu
alter table student rename to stu;
```

*   **添加一列**
    

```
ALTER TABLE 表名 ADD 列名 数据类型;
​
-- 给stu表添加一列address，该字段类型是varchar(50)
alter table stu add address varchar(50);
```

*   **修改数据类型**
    

```
ALTER TABLE 表名 MODIFY 列名 新数据类型;
​
-- 将stu表中的address字段的类型改为 char(50)
alter table stu modify address char(50);
```

*   **修改列名和数据类型**
    

```
ALTER TABLE 表名 CHANGE 列名 新列名 新数据类型;
​
-- 将stu表中的address字段名改为 addr，类型改为varchar(50)
alter table stu change address addr varchar(50);
```

*   **删除列**
    

```
ALTER TABLE 表名 DROP 列名;
​
-- 将stu表中的addr字段 删除
alter table stu drop addr;
```

*   **删除外键约束**
    

```
ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;

```

### 4.3 Navicat 下载安装使用

下载地址：

```
https://wwo.lanzouj.com/b00jddp8za


```

密码: fx4h

注意注册时要管理员运行和断网，版本必须是我这个版本的 16，最新的版本只能购买。

主机那里填 localhost 或 127.0.0.1，端口 3306。

可以通过 “美化 sql” 格式化代码：

![](<assets/1740015446701.png>)

![](<assets/1740015446905.png>)

![](<assets/1740015447117.png>)

### **4.4 数据操作语言​​​​​​​DML**

#### 4.4.1 DML 添加数据

*   **给指定列添加数据**
    

```
INSERT INTO 表名(列名1,列名2,…) VALUES(值1,值2,…);

```

*   **给全部列添加数据**
    

```
INSERT INTO 表名 VALUES(值1,值2,…);

```

*   **批量添加数据**
    

```
INSERT INTO 表名(列名1,列名2,…) VALUES(值1,值2,…),(值1,值2,…),(值1,值2,…)…;
INSERT INTO 表名 VALUES(值1,值2,…),(值1,值2,…),(值1,值2,…)…;
```

注意添加字符串时候要加引号

```
-- 给指定列添加数据
INSERT INTO stu (id, NAME) VALUES (1, '张三');
-- 给所有列添加数据，列名的列表可以省略的
INSERT INTO stu (id,NAME,sex,birthday,score,email,tel,STATUS) VALUES (2,'李四','男','1999-11-11',88.88,'lisi@itcast.cn','13888888888',1);
 
INSERT INTO stu VALUES (2,'李四','男','1999-11-11',88.88,'lisi@itcast.cn','13888888888',1);
 
-- 批量添加数据
INSERT INTO stu VALUES 
	(2,'李四','男','1999-11-11',88.88,'lisi@itcast.cn','13888888888',1),
	(2,'李四','男','1999-11-11',88.88,'lisi@itcast.cn','13888888888',1),
	(2,'李四','男','1999-11-11',88.88,'lisi@itcast.cn','13888888888',1);
```

#### 4.4.2 修改数据

​​​​​​​​​​​​​​

```
UPDATE 表名 SET 列名1=值1,列名2=值2,… [WHERE 条件] ;

```

**注意：**

1.  修改语句中如果不加条件，则将所有记录都修改！
    
2.  像上面的语句中的中括号，表示在写 sql 语句中可以省略这部分
    

```
update stu set sex = '女' where name = '张三';

```

#### 4.4.3 删除数据

*   **删除数据**
    

```
DELETE FROM 表名 [WHERE 条件] ;

```

*   **练习**
    

```
-- 删除张三记录
delete from stu where name = '张三';
​
-- 删除stu表中所有的数据
delete from stu;
```

**注意：**

1.  和上面一样，删除语句中如果不加条件，所有记录都将被删除，慎重！
    
2.  中括号，表示在写 sql 语句中可以省略的部分
    

### 4.5 数据查询语言 DQL

#### 4.5.1 **查询的完整语法**

查询最重要，最常用。

```
SELECT 
    字段列表
FROM 
    表名列表 
WHERE 
    条件列表
GROUP BY
    分组字段
HAVING
    分组后条件
ORDER BY
    排序字段
LIMIT
    分页限定
​​​​​​​
```

#### 4.5.2 创建练习查询的表

```
-- 删除stu表
drop table if exists stu;
 
 
-- 创建stu表
CREATE TABLE stu (
 id int, -- 编号
 name varchar(20), -- 姓名
 age int, -- 年龄
 sex varchar(5), -- 性别
 address varchar(100), -- 地址
 math double(5,2), -- 数学成绩
 english double(5,2), -- 英语成绩
 hire_date date -- 入学时间
);
 
-- 添加数据
INSERT INTO stu(id,NAME,age,sex,address,math,english,hire_date) 
VALUES 
(1,'马运',55,'男','杭州',66,78,'1995-09-01'),
(2,'马花疼',45,'女','深圳',98,87,'1998-09-01'),
(3,'马斯克',55,'男','香港',56,77,'1999-09-02'),
(4,'柳白',20,'女','湖南',76,65,'1997-09-05'),
(5,'柳青',20,'男','湖南',86,NULL,'1998-09-01'),
(6,'刘德花',57,'男','香港',99,99,'1998-09-01'),
(7,'张学右',22,'女','香港',99,99,'1998-09-01'),
(8,'德玛西亚',18,'男','南京',56,65,'1994-09-02');
```

#### 4.5.3 基础查询

*   **示例**

```
SELECT DISTINCT name AS '名字',age AS '年龄' FROM stu;

```

![](<assets/1740015447413.png>)

*   **查询多个字段**
    

```
SELECT 字段列表 FROM 表名;
SELECT * FROM 表名; -- 查询所有数据
```

*   **查询字段并去除重复记录**
    

```
SELECT DISTINCT 字段列表 FROM 表名;

```

​​​​​​​

![](<assets/1740015447644.png>)

​​​​​​​去重后

![](<assets/1740015447868.png>)

*   **起别名**
    

```
AS: AS 也可以省略

```

#### 4.5.4 条件查询（包括模糊查询）

```
SELECT 字段列表 FROM 表名 WHERE 条件列表;

```

举例： 

```
SELECT DISTINCT name AS '名字',age AS '年龄' FROM stu WHERE age>20 && age<=40;

```

![](<assets/1740015448101.png>)

  **模糊查询：**

```
SELECT  * FROM stu WHERE name LIKE '_斯%';

```

![](<assets/1740015448416.png>)

**模糊查询替换符：** 

下划线是必须一个字符，百分号替换 0 - 多个字符

*   **条件**
    

![](<assets/1740015448651.png>)

**注意：**

1.   null 不能和等号运算，要 IS NULL 或 IS NOT NULL，而不是 = null
2.  SQL 语句没有 ==，相等是 =，没有赋值的概念。

#### 4.5.5 排序查询

```
SELECT 字段列表 FROM 表名 ORDER BY 排序字段名1 [排序方式1],排序字段名2 [排序方式2] …;

```

*   查询学生信息，按照数学成绩降序排列，如果数学成绩一样，再按照英语成绩升序排列
    
    ```
    select * from stu order by math desc , english asc ;
    
    
    ```
    
    ![](<assets/1740015448954.png>)
    

上述语句中的排序方式有两种，分别是：

*   **ASC ：** 升序排列 **（默认值）**_ascending_  /əˈsendɪŋ/ 
    
*   **DESC ：** 降序排列，descending /dɪˈsendɪŋ/ 
    

**注意：**如果有多个排序条件，当前边的条件值一样时，才会根据第二条件进行排序

#### 4.5.6 聚合函数

```
SELECT 聚合函数名(列名) FROM 表;

```

示例：

```
select count(id) from stu;    #统计id字段非null的记录数量
select count(*) from stu;# 统计“存在非null字段”的记录数量，* 表示所有字段数据，只要某行有一个非空数据，就会被统计在内
```

![](<assets/1740015449189.png>)

![](<assets/1740015449583.png>)

![](<assets/1740015449808.png>)

![](<assets/1740015450003.png>)

 **聚合函数：**

<table><thead><tr><th>函数名</th><th>功能</th></tr></thead><tbody><tr><td>count(列名)</td><td>统计数量（选用不为 null 的列）</td></tr><tr><td>max(列名)</td><td>最大值</td></tr><tr><td>min(列名)</td><td>最小值</td></tr><tr><td>sum(列名)</td><td>求和</td></tr><tr><td>avg(列名)</td><td>平均值</td></tr></tbody></table>

**注意：null 值不参与所有聚合函数运算**

#### 4.5.7 带条件的聚合函数：count(name='abcd' or null)

统计所有名字为‘abcd’的学生：

```
SELECT count(name='abcd' or null) FROM student;#使用count带条件统计数量必须or null
SELECT count(date  between '2019-01-01' and '2019-03-31' or null) #使用count带条件统计数量必须or null
SELECT count(distinct name) FROM student;#注意distinct不能or null
 
#使用sum带条件统计数量不用or null。尽量别用sum，因为sum主要用来取和，如果这个name字段是数字型，则会是取和。
SELECT sum(name='abcd') FROM student;
```

第一个和第四个结果都是 1：

![](<assets/1740015450250.png>)

**注意：**

*   使用 count 带条件统计数量必须 or null，否则是统计总数量（条件是 distinct 除外）
*   使用 sum 带条件统计数量不用 or null

示例：使用 count 带条件统计数量如果不加 or null，就会统计这个字段总数量

```
SELECT count(name='abcd') FROM student;

```

![](<assets/1740015450428.png>) 

#### 4.5.7 分组查询

常常和聚合函数一起用。 

```
SELECT 字段列表 FROM 表名 [WHERE 分组前条件限定] GROUP BY 分组字段名 [HAVING 分组后条件过滤];

```

**注意：**分组之后，查询的字段为聚合函数和分组字段，查询其他字段无任何意义

**练习：**

查询男同学和女同学各自的数学平均分：

```
#根据性别分组，每组统计平均值
select sex, avg(math) from stu group by sex;
```

![](<assets/1740015450627.png>)

**注意：**在分组的情况下，查询字段为聚合函数时，这个聚合函数统计的将是**每组的信息**

查询男同学和女同学各自的数学平均分，以及各自人数；要求：分数低于 70 分的不参与分组，分组之后人数大于 2 个。

```
#根据性别分组，每组统计数学平均值、人数；分组前过滤math > 70，分组后过滤只展示人数>2的分组
select sex, avg(math),count(*) from stu where math > 70 group by sex having count(*)>2;
```

![](<assets/1740015450799.png>)

因为分组后男性人数没满足 having 条件，所以男性分组没展示。

**where 和 having 区别：**

*   执行时机不一样：where 是分组之前进行限定，不满足 where 条件，则不参与分组，而 having 是分组之后对结果进行过滤。
    
*   可判断的条件不一样：where 不能对聚合函数进行判断，having 可以。执行顺序 where > 聚合函数 > having，不可能判断后面执行的条件。
    

#### 4.5.8 分页查询

```
SELECT 字段列表 FROM 表名 LIMIT  起始索引 , 查询条目数;

```

**练习：**

起始索引 = (当前页码 - 1) * 每页显示的条数

*   从 0 开始查询，查询 3 条数据
    
    ```
    select * from stu limit 0 , 3;
    
    ```
    
*   每页显示 3 条数据，查询第 1 页数据
    
    ```
    select * from stu limit 0 , 3;
    
    ```
    

## 五、约束

### 5.1 概念

*   约束是作用于表中**列上的规则**，**用于限制加入表的数据**
    
    例如：我们可以给 id 列加约束，让其值不能重复，不能为 null 值。
    
*   添加约束可以在添加数据的时候就限制不正确的数据。例如把年龄是 3000，数学成绩是 - 5 分这样无效的数据限制掉，继而保障数据的完整性。
    

### **5.2 常用约束**

#### 5.2.1 介绍

<table><thead><tr><th>约束名</th><th>约束关键字</th><th>说明</th></tr></thead><tbody><tr><td>主键</td><td>primary key</td><td>唯一，非空</td></tr><tr><td>唯一</td><td>unique</td><td>不能重复，最多只有一个非空记录</td></tr><tr><td>默认</td><td>default</td><td>没有输入值，使用默认值</td></tr><tr><td>非空</td><td>not null</td><td>必须输入</td></tr><tr><td></td><td></td><td></td></tr><tr><td>外键</td><td>foreign key … references</td><td>外键在从表 主表：1 方 从表：多方</td></tr><tr><td>自增</td><td>auto_increment</td><td>从 1 开始自增，只有唯一和主键约束能用</td></tr><tr><td>检查 (mysql 不支持)&nbsp;&nbsp;</td><td>check</td><td>保证列中的值满足某一条件。</td></tr></tbody></table>

#### 5.2.2 常用命令

查询表中所有约束

```
SELECT 
    TABLE_NAME,
    COLUMN_NAME,
    CONSTRAINT_NAME,
    REFERENCED_TABLE_NAME,
    REFERENCED_COLUMN_NAME
FROM
    INFORMATION_SCHEMA.KEY_COLUMN_USAGE
WHERE
    TABLE_SCHEMA = '表名'
    AND TABLE_NAME = '表名';
```

删除约束（DROP CONSTRAINT）:

```
ALTER TABLE table_name
DROP PRIMARY KEY; 
-- 或 DROP FOREIGN KEY, DROP UNIQUE, 等等
```

### **5.3 增删约束**

![](<assets/1740015451040.png>)

 **示例：**

![](<assets/1740015451286.png>)

### 5.4 外键约束

#### 5.4.1 概述 

```
-- 创建表时添加外键约束
CREATE TABLE 表名(
   列名 数据类型,
   …
   [CONSTRAINT] [外键取名名称] FOREIGN KEY(外键列名) REFERENCES 主表(主表列名) 
); 
 
-- 创建表时添加外键约束，constraint译作限制，束缚；references译作关联，参考，提及
create table 表名(
   列名 数据类型,
   …
   [constraint] [外键取名名称] foreign key(外键列名) references 主表(主表列名) 
); 
```

```
-- 建完表后添加外键约束
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称);
 
-- 建完表后添加外键约束
alter table 表名 add constraint 外键名称 foreign key (外键字段名称) references 主表名称(主表列名称);
```

*   删除外键约束
    

```
ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;

```

#### 5.4.2 练习

![](<assets/1740015451797.png>)

```
-- 删除表
DROP TABLE IF EXISTS emp;
DROP TABLE IF EXISTS dept;
 
-- 部门表
CREATE TABLE dept(
	id int primary key auto_increment,
	dep_name varchar(20),
	addr varchar(20)
);
-- 员工表 
CREATE TABLE emp(
	id int primary key auto_increment,
	name varchar(20),
	age int,
	dep_id int,
 
	-- 添加外键 dep_id,关联 dept 表的id主键
	CONSTRAINT fk_emp_dept FOREIGN KEY(dep_id) REFERENCES dept(id)	
);
```

添加数据

```
-- 添加 2 个部门
insert into dept(dep_name,addr) values
('研发部','广州'),('销售部', '深圳');
​
-- 添加员工,dep_id 表示员工所在的部门
INSERT INTO emp (name, age, dep_id) VALUES 
('张三', 20, 1),
('李四', 20, 1),
('王五', 20, 1),
('赵六', 20, 2),
('孙七', 22, 2),
('周八', 18, 2);
```

此时删除 `研发部` 这条数据，会发现无法删除。

删除外键

```
alter table emp drop FOREIGN key fk_emp_dept;

```

重新添加外键

```
alter table emp add CONSTRAINT fk_emp_dept FOREIGN key(dep_id) REFERENCES dept(id);

```

![](<assets/1740015452114.png>)

右键逆向到表模型，可以查看关系:

![](<assets/1740015452360.png>)

## 六、数据库设计

### 6.1 概念

*   软件的研发步骤
    
    ![](<assets/1740015452581.png>)
    

*   **数据库设计概念**
    
    *   设计方向：有哪些表？表里有哪些字段？表和表之间有什么关系？
        
    *   **数据库设计就是根据业务系统的具体需求，结合我们所选用的 DBMS，为这个业务系统构造出最优的数据存储模型。**
        
    *   建立数据库中的**表结构**以及**表与表之间的关联关系**的过程。
        
*   **数据库设计的步骤**
    
    *   需求分析（数据是什么? 数据具有哪些属性? 数据与属性的特点是什么）
        
    *   逻辑分析（通过 ER 图对数据库进行逻辑建模，不需要考虑我们所选用的数据库管理系统）
        
        如下图就是 ER(Entity/Relation) 图：
        
        ![](<assets/1740015452815.png>)
        
        ​​​​​​​
        
*   *   物理设计（根据数据库自身的特点把逻辑设计转换为物理设计）
        
    *   维护设计（1. 对新的需求进行建表；2. 表优化）
        

### 6.2 表关系

#### 6.2.1 一对多

如：部门 和 员工

一个部门对应多个员工，一个员工对应一个部门。

**实现方式**

**在多的一方建立外键，指向一的一方的主键**

![](<assets/1740015453103.png>)

 建表语句：

```
-- 删除表
DROP TABLE IF EXISTS tb_emp;
DROP TABLE IF EXISTS tb_dept;
 
-- 部门表
CREATE TABLE tb_dept(
	id int primary key auto_increment,
	dep_name varchar(20),
	addr varchar(20)
);
-- 员工表 
CREATE TABLE tb_emp(
	id int primary key auto_increment,
	name varchar(20),
	age int,
	dep_id int,
 
	-- 添加外键 dep_id,关联 dept 表的id主键
	CONSTRAINT fk_emp_dept FOREIGN KEY(dep_id) REFERENCES tb_dept(id)	
);
```

#### 6.2.2 多对多

如：商品 和 订单

一个商品对应多个订单，一个订单包含多个商品。

**实现方式：**

建立第三张**中间表**，中间表至少包含**两个外键**，分别**关联两方主键**。

**案例：**

![](<assets/1740015453356.png>)

建表语句：

```
-- 删除表
DROP TABLE IF EXISTS tb_order_goods;
DROP TABLE IF EXISTS tb_order;
DROP TABLE IF EXISTS tb_goods;
 
-- 订单表
CREATE TABLE tb_order(
	id int primary key auto_increment,
	payment double(10,2),
	payment_type TINYINT,
	status TINYINT
);
 
-- 商品表
CREATE TABLE tb_goods(
	id int primary key auto_increment,
	title varchar(100),
	price double(10,2)
);
 
-- 订单商品中间表
CREATE TABLE tb_order_goods(
	id int primary key auto_increment,
	order_id int,
	goods_id int,
	count int
);
 
-- 建完表后，添加外键
alter table tb_order_goods add CONSTRAINT fk_order_id FOREIGN key(order_id) REFERENCES tb_order(id);
alter table tb_order_goods add CONSTRAINT fk_goods_id FOREIGN key(goods_id) REFERENCES tb_goods(id);
```

 表结构模型：

![](<assets/1740015453618.png>)

#### 6.2.3 一对一

如：用户 和 用户详情

一对一关系多用于表拆分，将一个实体中经常使用的字段放一张表，不经常使用的字段放另一张表，用于提升查询性能。

**实现方式：**

在任意一方加入外键，关联另一方主键，并且设置外键为唯一 (UNIQUE)

**案例：**

![](<assets/1740015453791.png>)

 而在真正使用过程中发现 id、photo、nickname、age、gender 字段比较常用，此时就可以将这张表查分成两张表：

![](<assets/1740015454007.png>)

**建表语句如下：**

```
create table tb_user_desc (
    id int primary key auto_increment,
    city varchar(20),
    edu varchar(10),
    income int,
    status char(2),
    des varchar(100)
);
​
create table tb_user (
    id int primary key auto_increment,
    photo varchar(100),
    nickname varchar(50),
    age int,
    gender char(1),
    desc_id int unique,
    -- 添加外键
    CONSTRAINT fk_user_desc FOREIGN KEY(desc_id) REFERENCES tb_user_desc(id)    
);
```

**查看表结构模型图：**

![](<assets/1740015454284.png>)

## 七、多表查询

### 7.1 创建练习的表

```
DROP TABLE IF EXISTS emp;
DROP TABLE IF EXISTS dept;
 
 
# 创建部门表
	CREATE TABLE dept(
        did INT PRIMARY KEY AUTO_INCREMENT,
        dname VARCHAR(20)
    );
 
	# 创建员工表
	CREATE TABLE emp (
        id INT PRIMARY KEY AUTO_INCREMENT,
        NAME VARCHAR(10),
        gender CHAR(1), -- 性别
        salary DOUBLE, -- 工资
        join_date DATE, -- 入职日期
        dep_id INT,
        FOREIGN KEY (dep_id) REFERENCES dept(did) -- 外键，关联部门表(部门表的主键)
    );
	-- 添加部门数据
	INSERT INTO dept (dNAME) VALUES ('研发部'),('市场部'),('财务部'),('销售部');
	-- 添加员工数据
	INSERT INTO emp(NAME,gender,salary,join_date,dep_id) VALUES
	('孙悟空','男',7200,'2013-02-24',1),
	('猪八戒','男',3600,'2010-12-02',2),
	('唐僧','男',9000,'2008-08-08',2),
	('白骨精','女',5000,'2015-10-07',3),
	('蜘蛛精','女',4500,'2011-03-14',1),
	('小白龙','男',2500,'2011-02-14',null);	
```

员工表：

![](<assets/1740015454530.png>)

部门表：

![](<assets/1740015454724.png>)

### 7.2 连接查询

#### 7.2.1 概念 

![](<assets/1740015454977.png>)

*   内连接查询 ：相当于查询 AB 交集数据
    
*   外连接查询
    
    *   左外连接查询 ：相当于查询 A 表所有数据和交集部门数据
        
    *   右外连接查询 ： 相当于查询 B 表所有数据和交集部分数据
        

**关联查询结果行数：**假设 a 表 x 行，b 表 y 行；

*   **a 左连接 b：**x 行~ x*y 行
*   **a 右连接 b：**y 行~ y*x 行
*   **内连接：**0 行~ min(x,y) 行

#### 7.2.2 内连接查询

相当于查询 AB 交集数据。

**语句：**

```
-- 隐式内连接。没有JOIN关键字，条件使用WHERE指定。书写简单，多表时效率低
SELECT 字段列表 FROM 表1,表2… WHERE 条件;
 
-- 显示内连接。使用INNER JOIN ... ON语句, 可以省略INNER。书写复杂，多表时效率高
SELECT 字段列表 FROM 表1 [INNER] JOIN 表2 ON 条件;
```

隐式连接好理解好书写，语法简单，担心的点较少。

但是**显式连接**可以减少字段的扫描，有**更快的执行速度**。这种速度优势在 3 张或更多表连接时比较明显 

 **示例：**

```
#隐式内连接
SELECT
	emp. NAME,
	emp.gender,
	dept.dname
FROM
	emp,
	dept
WHERE
	emp.dep_id = dept.did;
```

```
#显式内连接
select * from emp inner join dept on emp.dep_id = dept.did;
```

![](<assets/1740015455164.png>)

员工表：

![](<assets/1740015455506.png>)

部门表：

![](<assets/1740015455600.png>)

#### 7.2.3 自连接

自连接是一种特殊的[内连接](https://baike.baidu.com/item/%E5%86%85%E8%BF%9E%E6%8E%A5?fromModule=lemma_inlink "内连接")，它是指相互连接的表在物理上为同一张表，但可以在**逻辑上**分为两张表。

**注意：自连接查询的列名必须是 “表名.*”，而不是直接写 “*”**

案例：

要求检索出学号为 **20210** 的学生的同班同学的信息

```
SELECT stu.*        #一定注意是stu.*，不是*
 
FROM stu JOIN stu AS stu1 ON stu.grade= stu1.grade
 
WHERE stu1.id='20210'
```

#### 7.2.4 递归查询

with 语法：

```
   WITH [RECURSIVE]
        cte_name [(col_name [, col_name] ...)] AS (subquery)
        [, cte_name [(col_name [, col_name] ...)] AS (subquery)] ...
```

recurslve 译为递归。

**with：**在 mysql 中被称为**公共表达式**, 可以作为一个**临时表**然后在其他结构中调用. 如果是自身调用那么就是后面讲的递归.

cte_name : 公共表达式的名称, 可以理解为表名, 用来表示 as 后面跟着的子查询

col_name : 公共表达式包含的列名, 可以写也可以不写

**例子：使用 MySQL 临时表遍历 1~5**

```
with RECURSIVE t1  AS    #这里t1函数名，也是临时表的表名
(
  SELECT 1 as n        #n是列的别名，1是初始记录
  UNION ALL        #把递归结果（2,3,4,5）合并到t1表中
  SELECT n + 1 FROM t1 WHERE n < 5    #n+1是参数，t1是函数名，n<5是遍历终止条件
)
SELECT * FROM t1;        #正常查询t1这个临时表，相当于调用这个函数。
 
```

![](<assets/1740015455704.png>)

​​

**说明：**

t1 相当于一个表名

select 1 相当于这个表的初始值，这里使用 UNION ALL 不断将每次递归得到的数据加入到表中。

n<5 为递归执行的条件，当 n>=5 时结束递归调用。

**案例，递归查询课程多级分类：**

```
with recursive t1 as (        #t1是函数名、临时表名
select * from  course_category where  id= '1'   #初始记录，也就是根节点
union all         #把递归结果合并到t1表中
 select t2.* from course_category as t2 inner join t1 on t1.id = t2.parentid    #递归，用分类表t和临时表t1内连接查询
)
 
select *  from t1 order by t1.id, t1.orderby    #查t1表，相当于调用这个函数。
```

![](<assets/1740015456031.png>)

​

**mysql 递归特点，对比 Java 递归的优势**

**mysql 递归次数限制：**

mysql 为了避免无限递归默认递归次数为 1000，可以通过设置 cte_max_recursion_depth 参数增加递归深度，还可以通过 max_execution_time 限制执行时间，超过此时间也会终止递归操作。

**对比 Java 递归的优势：**

mysql 递归相当于在存储过程中执行若干次 sql 语句，java 程序仅与数据库建立一次链接执行递归操作。相比之下，Java 递归性能就很差，每次递归都会建立一次数据库连接。

#### 7.2.5 外连接查询

**语句：**

```
-- 左外连接
SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件;
 
-- 右外连接
SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件;
```

一般都用左外连接，因为右外连接可用左外连接实现，可读性更好。 

**示例：**

查询 emp 表所有数据和对应的部门信息（左外连接）

```
select * from emp left join dept on emp.dep_id = dept.did;

```

![](<assets/1740015456297.png>)

​

查询 dept 表所有数据和对应的员工信息（右外连接）

```
select * from emp right join dept on emp.dep_id = dept.did;

```

![](<assets/1740015456705.png>)

​

### 7.3 子查询

查询中嵌套查询，称嵌套查询为子查询。

注意：子语句没有分号。

**子查询根据查询结果不同，作用不同，可分为：**

*   **子查询语句结果是单行单列**，子查询语句作为条件值，使用 = != > < 等进行条件判断

![](<assets/1740015456940.png>)

​

 示例：

查询比猪八戒薪水高的员工：

```
SELECT * FROM emp WHERE salary >(SELECT salary FROM emp WHERE name='猪八戒');

```

*   **子查询语句结果是多行单列**，子查询语句作为条件值，使用 in 等关键字进行条件判断

![](<assets/1740015457161.png>)

​

 示例：

查询 '财务部' 和 '市场部' 所有的员工信息：

```
SELECT * FROM emp WHERE dep_id in (SELECT did FROM dept WHERE dname IN ('财务部','市场部'));

```

*   **子查询语句结果是多行多列**，子查询语句作为虚拟表

![](<assets/1740015457646.png>)

​

示例：

 查询入职日期是 '2011-11-11' 之后的员工信息和部门信息：

```
select * from (select * from emp where join_date > '2011-11-11' ) AS t1, dept where t1.dep_id = dept.did;

```

### 7.4 多表查询练习题：

```
DROP TABLE IF EXISTS emp;
DROP TABLE IF EXISTS dept;
DROP TABLE IF EXISTS job;
DROP TABLE IF EXISTS salarygrade;
 
-- 部门表
CREATE TABLE dept (
  did INT PRIMARY KEY PRIMARY KEY, -- 部门id
  dname VARCHAR(50), -- 部门名称
  loc VARCHAR(50) -- 部门所在地
);
 
-- 职务表，职务名称，职务描述
CREATE TABLE job (
  id INT PRIMARY KEY,
  jname VARCHAR(20),
  description VARCHAR(50)
);
 
-- 员工表
CREATE TABLE emp (
  id INT PRIMARY KEY, -- 员工id
  ename VARCHAR(50), -- 员工姓名
  job_id INT, -- 职务id
  mgr INT , -- 上级领导
  joindate DATE, -- 入职日期
  salary DECIMAL(7,2), -- 工资
  bonus DECIMAL(7,2), -- 奖金
  dept_id INT, -- 所在部门编号
  CONSTRAINT emp_jobid_ref_job_id_fk FOREIGN KEY(job_id) REFERENCES job(id),
  CONSTRAINT emp_deptid_ref_dept_id_fk FOREIGN KEY(dept_id) REFERENCES dept(did)
);
-- 工资等级表
CREATE TABLE salarygrade (
  grade INT PRIMARY KEY,   -- 级别
  losalary INT,  -- 最低工资
  hisalary INT -- 最高工资
);
				
-- 添加4个部门
INSERT INTO dept(did,dname,loc) VALUES 
(10,'教研部','北京'),
(20,'学工部','上海'),
(30,'销售部','广州'),
(40,'财务部','深圳');
 
-- 添加4个职务
INSERT INTO job (id, jname, description) VALUES
(1, '董事长', '管理整个公司，接单'),
(2, '经理', '管理部门员工'),
(3, '销售员', '向客人推销产品'),
(4, '文员', '使用办公软件');
 
 
-- 添加员工
INSERT INTO emp(id,ename,job_id,mgr,joindate,salary,bonus,dept_id) VALUES 
(1001,'孙悟空',4,1004,'2000-12-17','8000.00',NULL,20),
(1002,'卢俊义',3,1006,'2001-02-20','16000.00','3000.00',30),
(1003,'林冲',3,1006,'2001-02-22','12500.00','5000.00',30),
(1004,'唐僧',2,1009,'2001-04-02','29750.00',NULL,20),
(1005,'李逵',4,1006,'2001-09-28','12500.00','14000.00',30),
(1006,'宋江',2,1009,'2001-05-01','28500.00',NULL,30),
(1007,'刘备',2,1009,'2001-09-01','24500.00',NULL,10),
(1008,'猪八戒',4,1004,'2007-04-19','30000.00',NULL,20),
(1009,'罗贯中',1,NULL,'2001-11-17','50000.00',NULL,10),
(1010,'吴用',3,1006,'2001-09-08','15000.00','0.00',30),
(1011,'沙僧',4,1004,'2007-05-23','11000.00',NULL,20),
(1012,'李逵',4,1006,'2001-12-03','9500.00',NULL,30),
(1013,'小白龙',4,1004,'2001-12-03','30000.00',NULL,20),
(1014,'关羽',4,1007,'2002-01-23','13000.00',NULL,10);
 
 
-- 添加5个工资等级
INSERT INTO salarygrade(grade,losalary,hisalary) VALUES 
(1,7000,12000),
(2,12010,14000),
(3,14010,20000),
(4,20010,30000),
(5,30010,99990);
```

需求

1.  查询所有员工信息。查询员工编号，员工姓名，工资，职务名称，职务描述
    
    ```
    /*
        分析：
            1. 员工编号，员工姓名，工资 信息在emp 员工表中
            2. 职务名称，职务描述 信息在 job 职务表中
            3. job 职务表 和 emp 员工表 是 一对多的关系 emp.job_id = job.id
    */
    -- 方式一 ：隐式内连接
    SELECT
        emp.id,
        emp.ename,
        emp.salary,
        job.jname,
        job.description
    FROM
        emp,
        job
    WHERE
        emp.job_id = job.id;
    ​
    -- 方式二 ：显式内连接
    SELECT
        emp.id,
        emp.ename,
        emp.salary,
        job.jname,
        job.description
    FROM
        emp
    INNER JOIN job ON emp.job_id = job.id;
    ```
    
2.  查询员工编号，员工姓名，工资，职务名称，职务描述，部门名称，部门位置
    
    ```
    /*
        分析：
            1. 员工编号，员工姓名，工资 信息在emp 员工表中
            2. 职务名称，职务描述 信息在 job 职务表中
            3. job 职务表 和 emp 员工表 是 一对多的关系 emp.job_id = job.id
    ​
            4. 部门名称，部门位置 来自于 部门表 dept
            5. dept 和 emp 一对多关系 dept.id = emp.dept_id
    */
    ​
    -- 方式一 ：隐式内连接
    SELECT
        emp.id,
        emp.ename,
        emp.salary,
        job.jname,
        job.description,
        dept.dname,
        dept.loc
    FROM
        emp,
        job,
        dept
    WHERE
        emp.job_id = job.id
        and dept.id = emp.dept_id
    ;
    ​
    -- 方式二 ：显式内连接
    SELECT
        emp.id,
        emp.ename,
        emp.salary,
        job.jname,
        job.description,
        dept.dname,
        dept.loc
    FROM
        emp
    INNER JOIN job ON emp.job_id = job.id
    INNER JOIN dept ON dept.id = emp.dept_id
    ```
    
3.  查询员工姓名，工资，工资等级
    
    ```
    /*
        分析：
            1. 员工姓名，工资 信息在emp 员工表中
            2. 工资等级 信息在 salarygrade 工资等级表中
            3. emp.salary >= salarygrade.losalary  and emp.salary <= salarygrade.hisalary
    */
    SELECT
        emp.ename,
        emp.salary,
        t2.*
    FROM
        emp,
        salarygrade t2
    WHERE
        emp.salary >= t2.losalary
    AND emp.salary <= t2.hisalary
    ```
    
4.  查询员工姓名，工资，职务名称，职务描述，部门名称，部门位置，工资等级
    
    ```
    /*
        分析：
            1. 员工编号，员工姓名，工资 信息在emp 员工表中
            2. 职务名称，职务描述 信息在 job 职务表中
            3. job 职务表 和 emp 员工表 是 一对多的关系 emp.job_id = job.id
    ​
            4. 部门名称，部门位置 来自于 部门表 dept
            5. dept 和 emp 一对多关系 dept.id = emp.dept_id
            6. 工资等级 信息在 salarygrade 工资等级表中
            7. emp.salary >= salarygrade.losalary  and emp.salary <= salarygrade.hisalary
    */
    SELECT
        emp.id,
        emp.ename,
        emp.salary,
        job.jname,
        job.description,
        dept.dname,
        dept.loc,
        t2.grade
    FROM
        emp
    INNER JOIN job ON emp.job_id = job.id
    INNER JOIN dept ON dept.id = emp.dept_id
    INNER JOIN salarygrade t2 ON emp.salary BETWEEN t2.losalary and t2.hisalary;
    ```
    

## 八、事务

### 8.1 概念

数据库的事务（Transaction）是一种机制、一个操作序列，包含了**一组数据库操作命令**。

事务把所有的命令作为一个整体一起向系统提交或撤销操作请求，即这一组数据库命令**要么同时成功，要么同时失败**。

事务是一个不可分割的工作逻辑单元。

**示例：**

![](<assets/1740015457937.png>)

​

 在转账前开启事务，如果出现了异常回滚事务，三步正常执行就提交事务，这样就可以完美解决问题。

### 8.2 语法

*   开启事务
    
    ```
    START TRANSACTION;        --transaction译为事务，业务，交易
    或者  
    BEGIN;
    ```
    
*   提交事务
    
    ```
    commit;
    
    ```
    

**示例：**

```
-- 开启事务
BEGIN;
-- 转账操作
-- 1. 查询李四账户金额是否大于500
 
-- 2. 李四账户 -500
UPDATE account set money = money - 500 where name = '李四';
 
出现异常了...  -- 此处不是注释，在整体执行时会出问题，后面的sql则不执行
-- 3. 张三账户 +500
UPDATE account set money = money + 500 where name = '张三';
 
-- 提交事务
COMMIT;
 
-- 回滚事务
ROLLBACK;
```

上面 sql 中的执行成功进选择执行提交事务，而出现问题则执行回滚事务的语句。以后我们肯定不可能这样操作，而是在 java 中进行操作，在 java 中可以抓取异常，没出现异常提交事务，出现异常回滚事务。  

### 8.3 事务的四大特征

*   原子性（Atomicity）: 事务是不可分割的最小操作单位，要么同时成功，要么同时失败
    
*   一致性（Consistency） : 事务完成时，必须使所有的数据都保持一致状态
    
*   隔离性（Isolation） : 多个事务之间，操作的可见性
    
*   持久性（Durability） : 事务一旦提交或回滚，它对数据库中的数据的改变就是永久的
    

**说明**：

mysql 中事务是自动提交的。

也就是说我们不添加事务执行 sql 语句，语句执行完毕会自动的提交事务。

可以通过下面语句查询默认提交方式：

```
SELECT @@autocommit;

```

查询到的结果是 1 则表示自动提交，结果是 0 表示手动提交。当然也可以通过下面语句修改提交方式

```
set @@autocommit = 0;

``` 

## 九、函数

### 9.1 数值型函数 

<table><caption></caption><tbody><tr><th>函数名称</th><th>作&nbsp;用</th></tr><tr><td><a href="http://c.biancheng.net/mysql/abc.html" rel="nofollow" title="ABS" target="_blank">ABS</a></td><td>求绝对值</td></tr><tr><td><a href="http://c.biancheng.net/mysql/sqrt.html" rel="nofollow" title="SQRT" target="_blank">SQRT</a></td><td>求二次方根</td></tr><tr><td><a href="http://c.biancheng.net/mysql/mod.html" rel="nofollow" title="MOD" target="_blank">MOD</a></td><td>求余数</td></tr><tr><td><a href="http://c.biancheng.net/mysql/ceil_celing.html" rel="nofollow" title="CEIL 和&nbsp;CEILING" target="_blank">CEIL 和&nbsp;CEILING</a></td><td>两个函数功能相同，都是返回不小于参数的最小整数，即向上取整</td></tr><tr><td><a href="http://c.biancheng.net/mysql/floor.html" rel="nofollow" title="FLOOR" target="_blank">FLOOR</a></td><td>向下取整，返回值转化为一个 BIGINT</td></tr><tr><td><a href="http://c.biancheng.net/mysql/rand.html" rel="nofollow" title="RAND" target="_blank">RAND</a></td><td>生成一个 0~1 之间的随机数，传入整数参数是，用来产生重复序列</td></tr><tr><td><a href="http://c.biancheng.net/mysql/round.html" rel="nofollow" title="ROUND" target="_blank">ROUND</a></td><td>对所传参数进行四舍五入。例如 round(3.1415926,3) 是四舍五入保留三位小数</td></tr><tr><td><a href="http://c.biancheng.net/mysql/sign.html" rel="nofollow" title="SIGN" target="_blank">SIGN</a></td><td>返回参数的符号</td></tr><tr><td><a href="http://c.biancheng.net/mysql/pow_power.html" rel="nofollow" title="POW 和&nbsp;POWER" target="_blank">POW 和&nbsp;POWER</a></td><td>两个函数的功能相同，都是所传参数的次方的结果值</td></tr><tr><td><a href="http://c.biancheng.net/mysql/sin.html" rel="nofollow" title="SIN" target="_blank">SIN</a></td><td>求正弦值</td></tr><tr><td><a href="http://c.biancheng.net/mysql/asin.html" rel="nofollow" title="ASIN" target="_blank">ASIN</a></td><td>求反正弦值，与函数 SIN 互为反函数</td></tr><tr><td><a href="http://c.biancheng.net/mysql/cos.html" rel="nofollow" title="COS" target="_blank">COS</a></td><td>求余弦值</td></tr><tr><td><a href="http://c.biancheng.net/mysql/acos.html" rel="nofollow" title="ACOS" target="_blank">ACOS</a></td><td>求反余弦值，与函数 COS 互为反函数</td></tr><tr><td><a href="http://c.biancheng.net/mysql/tan.html" rel="nofollow" title="TAN" target="_blank">TAN</a></td><td>求正切值</td></tr><tr><td><a href="http://c.biancheng.net/mysql/atan.html" rel="nofollow" title="ATAN" target="_blank">ATAN</a></td><td>求反正切值，与函数 TAN 互为反函数</td></tr><tr><td><a href="http://c.biancheng.net/mysql/cot.html" rel="nofollow" title="COT" target="_blank">COT</a></td><td>求余切值</td></tr></tbody></table>

### 9.2 字符串函数

<table><caption></caption><tbody><tr><th>函数名称</th><th>作&nbsp;用</th></tr><tr><td><a href="http://c.biancheng.net/mysql/length.html" rel="nofollow" title="LENGTH" target="_blank">LENGTH</a></td><td>计算字符串长度函数，返回字符串的字节长度</td></tr><tr><td><a href="http://c.biancheng.net/mysql/concat.html" rel="nofollow" title="CONCAT" target="_blank">CONCAT</a></td><td>合并字符串函数，返回结果为连接参数产生的字符串，参数可以使一个或多个</td></tr><tr><td><a href="http://c.biancheng.net/mysql/insert.html" rel="nofollow" title="INSERT" target="_blank">INSERT</a></td><td>替换字符串函数</td></tr><tr><td><a href="http://c.biancheng.net/mysql/lower.html" rel="nofollow" title="LOWER" target="_blank">LOWER</a></td><td>将字符串中的字母转换为小写</td></tr><tr><td><a href="http://c.biancheng.net/mysql/upper.html" rel="nofollow" title="UPPER" target="_blank">UPPER</a></td><td>将字符串中的字母转换为大写</td></tr><tr><td><a href="http://c.biancheng.net/mysql/left.html" rel="nofollow" title="LEFT" target="_blank">LEFT</a></td><td>从左侧字截取符串，返回字符串左边的若干个字符</td></tr><tr><td><a href="http://c.biancheng.net/mysql/right.html" rel="nofollow" title="RIGHT" target="_blank">RIGHT</a></td><td>从右侧字截取符串，返回字符串右边的若干个字符</td></tr><tr><td><a href="http://c.biancheng.net/mysql/trim.html" rel="nofollow" title="TRIM" target="_blank">TRIM</a></td><td>删除字符串左右两侧的空格</td></tr><tr><td><a href="http://c.biancheng.net/mysql/replace.html" rel="nofollow" title="REPLACE" target="_blank">REPLACE</a></td><td>字符串替换函数，返回替换后的新字符串</td></tr><tr><td><a href="http://c.biancheng.net/mysql/substring.html" rel="nofollow" title="SUBSTRING" target="_blank">SUBSTRING</a></td><td>截取字符串，返回从指定位置开始的指定长度的字符换</td></tr><tr><td><a href="http://c.biancheng.net/mysql/reverse.html" rel="nofollow" title="REVERSE" target="_blank">REVERSE</a></td><td>字符串反转（逆序）函数，返回与原始字符串顺序相反的字符串</td></tr></tbody></table>

### 9.3 日期和时间函数

<table><caption></caption><tbody><tr><th>函数名称</th><th>作 用</th></tr><tr><td><a href="http://c.biancheng.net/mysql/curdate_current_date.html" rel="nofollow" title="CURDATE&nbsp;和 CURRENT_DATE" target="_blank">CURDATE&nbsp;和 CURRENT_DATE</a></td><td>两个函数作用相同，返回当前系统的日期值</td></tr><tr><td><a href="http://c.biancheng.net/mysql/curtime_current_time.html" rel="nofollow" title="CURTIME 和 CURRENT_TIME" target="_blank">CURTIME 和 CURRENT_TIME</a></td><td>两个函数作用相同，返回当前系统的时间值</td></tr><tr><td><a href="http://c.biancheng.net/mysql/now_sysdate.html" rel="nofollow" title="NOW&nbsp;和&nbsp; SYSDATE" target="_blank">NOW&nbsp;和&nbsp; SYSDATE</a></td><td>两个函数作用相同，返回当前系统的日期和时间值</td></tr><tr><td><a href="http://c.biancheng.net/mysql/unix_timestamp.html" rel="nofollow" title="UNIX_TIMESTAMP" target="_blank">UNIX_TIMESTAMP</a></td><td>获取 UNIX 时间戳函数，返回一个以 UNIX 时间戳为基础的无符号整数</td></tr><tr><td><a href="http://c.biancheng.net/mysql/from_unixtime.html" rel="nofollow" title="FROM_UNIXTIME" target="_blank">FROM_UNIXTIME</a></td><td>将 UNIX 时间戳转换为时间格式，与 UNIX_TIMESTAMP 互为反函数</td></tr><tr><td><a href="http://c.biancheng.net/mysql/month.html" rel="nofollow" title="MONTH" target="_blank">MONTH</a></td><td>获取指定日期中的月份</td></tr><tr><td><a href="http://c.biancheng.net/mysql/monthname.html" rel="nofollow" title="MONTHNAME" target="_blank">MONTHNAME</a></td><td>获取指定日期中的月份英文名称</td></tr><tr><td><a href="http://c.biancheng.net/mysql/dayname.html" rel="nofollow" title="DAYNAME" target="_blank">DAYNAME</a></td><td>获取指定曰期对应的星期几的英文名称</td></tr><tr><td><a href="http://c.biancheng.net/mysql/dayofweek.html" rel="nofollow" title="DAYOFWEEK" target="_blank">DAYOFWEEK</a></td><td>获取指定日期对应的一周的索引位置值</td></tr><tr><td><a href="http://c.biancheng.net/mysql/week.html" rel="nofollow" title="WEEK" target="_blank">WEEK</a></td><td>获取指定日期是一年中的第几周，返回值的范围是否为 0〜52 或 1〜53</td></tr><tr><td><a href="http://c.biancheng.net/mysql/dayofyear.html" rel="nofollow" title="DAYOFYEAR" target="_blank">DAYOFYEAR</a></td><td>获取指定曰期是一年中的第几天，返回值范围是 1~366</td></tr><tr><td><a href="http://c.biancheng.net/mysql/dayofmonth.html" rel="nofollow" title="DAYOFMONTH" target="_blank">DAYOFMONTH</a></td><td>获取指定日期是一个月中是第几天，返回值范围是 1~31</td></tr><tr><td><a href="http://c.biancheng.net/mysql/year.html" rel="nofollow" title="YEAR" target="_blank">YEAR</a></td><td>获取年份，返回值范围是 1970〜2069</td></tr><tr><td><a href="http://c.biancheng.net/mysql/time_to_sec.html" rel="nofollow" title="TIME_TO_SEC" target="_blank">TIME_TO_SEC</a></td><td>将时间参数转换为秒数</td></tr><tr><td><a href="http://c.biancheng.net/mysql/sec_to_time.html" rel="nofollow" title="SEC_TO_TIME" target="_blank">SEC_TO_TIME</a></td><td>将秒数转换为时间，与 TIME_TO_SEC 互为反函数</td></tr><tr><td><a href="http://c.biancheng.net/mysql/date_add_adddate.html" rel="nofollow" title="DATE_ADD 和 ADDDATE" target="_blank">DATE_ADD 和 ADDDATE</a></td><td>两个函数功能相同，都是向日期添加指定的时间间隔</td></tr><tr><td><a href="http://c.biancheng.net/mysql/date_sub_subdate.html" rel="nofollow" title="DATE_SUB 和 SUBDATE" target="_blank">DATE_SUB 和 SUBDATE</a></td><td>两个函数功能相同，都是向日期减去指定的时间间隔</td></tr><tr><td><a href="http://c.biancheng.net/mysql/addtime.html" rel="nofollow" title="ADDTIME" target="_blank">ADDTIME</a></td><td>时间加法运算，在原始时间上添加指定的时间</td></tr><tr><td><a href="http://c.biancheng.net/mysql/subtime.html" rel="nofollow" title="SUBTIME" target="_blank">SUBTIME</a></td><td>时间减法运算，在原始时间上减去指定的时间</td></tr><tr><td><a href="http://c.biancheng.net/mysql/datediff.html" rel="nofollow" title="DATEDIFF" target="_blank">DATEDIFF</a></td><td>获取两个日期之间间隔，返回参数 1 减去参数 2 的值</td></tr><tr><td><a href="http://c.biancheng.net/mysql/date_format.html" rel="nofollow" title="DATE_FORMAT" target="_blank">DATE_FORMAT</a></td><td>格式化指定的日期，根据参数返回指定格式的值</td></tr><tr><td><a href="http://c.biancheng.net/mysql/weekday.html" rel="nofollow" title="WEEKDAY" target="_blank">WEEKDAY</a></td><td>获取指定日期在一周内的对应的工作日索引</td></tr></tbody></table>

### 9.4 聚合函数

<table><caption></caption><tbody><tr><th>函数名称</th><th>作用</th></tr><tr><td><a href="http://c.biancheng.net/mysql/max.html" rel="nofollow" title="MAX" target="_blank">MAX</a></td><td>查询指定列的最大值</td></tr><tr><td><a href="http://c.biancheng.net/mysql/min.html" rel="nofollow" title="MIN" target="_blank">MIN</a></td><td>查询指定列的最小值</td></tr><tr><td><a href="http://c.biancheng.net/mysql/count.html" rel="nofollow" title="COUNT" target="_blank">COUNT</a></td><td>统计查询结果的行数</td></tr><tr><td><a href="http://c.biancheng.net/mysql/sum.html" rel="nofollow" title="SUM" target="_blank">SUM</a></td><td>求和，返回指定列的总和</td></tr><tr><td><a href="http://c.biancheng.net/mysql/avg.html" rel="nofollow" title="AVG" target="_blank">AVG</a></td><td>求平均值，返回指定列数据的平均值</td></tr></tbody></table>

### 9.5 流程控制函数

<table><caption></caption><tbody><tr><th>函数名称</th><th>作用</th></tr><tr><td><a href="http://c.biancheng.net/mysql/if.html" rel="nofollow" title="IF(expr1,expr2,expr3)" target="_blank">IF(expr1,expr2,expr3)</a></td><td>判断，流程控制。expr1 的值为 TRUE，则返回值为 expr2 。否则返回 expr3</td></tr><tr><td><a href="http://c.biancheng.net/mysql/ifnull.html" rel="nofollow" title="IFNULL" target="_blank">IFNULL</a>（expr1,expr2）</td><td>判断是否为空。例如 select ifnull(age,0) from stu where id=0; 如果这个学生年龄是 null 则返回 0</td></tr><tr><td><a href="http://c.biancheng.net/mysql/case.html" rel="nofollow" title="CASE" target="_blank">CASE</a></td><td>搜索语句</td></tr></tbody></table>

## 十、SQL 练习题：高频 SQL 50 题（基础版）

初学者可以每天做一道，或者每周做一道，提高自己写 SQL 的能力。

[高频 SQL 50 题（基础版） - 学习计划 - 力扣（LeetCode）全球极客挚爱的技术成长平台](https://leetcode.cn/studyplan/sql-free-50/ "高频 SQL 50 题（基础版） - 学习计划 - 力扣（LeetCode）全球极客挚爱的技术成长平台")