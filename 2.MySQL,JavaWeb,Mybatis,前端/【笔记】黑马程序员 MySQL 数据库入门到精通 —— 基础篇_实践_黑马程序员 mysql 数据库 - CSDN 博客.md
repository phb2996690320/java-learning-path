> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_43254100/article/details/131601689)

#### 文章目录

*   [SQL 语法](#SQL_1)
*   *   [SQL 语法：DDL 操作数据库，表的定义](#SQLDDL__3)
    *   *   [1. 操作数据库](#1_5)
        *   [2. 操作表](#2_22)
        *   [实践：设计一张员工信息表](#_38)
    *   [SQL 语法：DML 增删改 表中的数据](#SQLDML___99)
    *   [SQL 语法：DQL 查询表中的数据](#SQLDQL__152)
    *   *   [1. 基本查询（不带任何条件）](#1_189)
        *   [2. 条件查询（WHERE）](#2WHERE_215)
        *   [3. 聚合函数（count、max、min、avg、sum）](#3countmaxminavgsum_289)
        *   [4. 分组查询（group by）](#4group_by_325)
        *   [5. 排序查询（order by）](#5order_by_361)
        *   [6. 分页查询（limit）](#6limit_383)
        *   [DQL 案例](#DQL_402)
        *   [验证 DQL 的执行顺序](#DQL_436)
    *   [SQL 语法：DCL 管理数据库用户、控制数据库的访问权限](#SQLDCL__471)
    *   *   [管理用户](#_473)
        *   [管理权限](#_510)
*   [函数](#_542)
*   *   [函数：字符串函数 concat, lower, upper, lpad, rpad, trim, substring](#_concat_lower_upper_lpad_rpad_trim_substring_544)
    *   [函数：数值函数 ceil,floor,mod,rand,round](#_ceilfloormodrandround_600)
    *   [函数：日期函数 curdate, curtime, now, year, month, day, date_add, datediff](#_curdate_curtime_now_year_month_day_date_add_datediff_638)
    *   [函数：流程控制 if,ifnull,case..when..](#_ififnullcasewhen_701)
*   [约束](#_786)
*   *   [约束：非空约束，唯一约束，主键约束，默认约束，检查约束](#_788)
    *   [约束：外键约束](#_859)
*   [多表查询](#_945)
*   *   [多表查询：多表关系 1 对 1，1 对 N/N 对 1，N 对 N](#_111NN1NN_947)
    *   [多表查询： 普通多表查询](#__1058)
    *   [多表查询：内连接](#_1118)
    *   [多表查询：外连接](#_1135)
    *   [多表查询：自连接](#_1171)
    *   [多表查询：联合查询](#_1192)
    *   [多表查询：子查询——标量子查询](#_1229)
    *   [多表查询：子查询——列子查询](#_1265)
    *   [多表查询：子查询——行子查询](#_1333)
    *   [多表查询：子查询——表查询](#_1357)
    *   [多表查询——案例](#_1387)
*   [事务](#_1521)
*   *   [事务：事务操作](#_1523)
    *   [事务：事务的隔离级别](#_1612)
*   [总结：SQL，函数，约束，多表查询，事务](#SQL_1673)

SQL 语法
------

### SQL 语法：DDL 操作数据库，表的定义

#### 1. 操作数据库

```
#创建：
create database b1;

#查询：
show databases; 查询所有数据库 
select.database() 查询当前使用的数据库
show create database db1; 查看数据库b1的创建语句

# 删除：
Drop database db1; 删除数据库b1

```

![](https://i-blog.csdnimg.cn/blog_migrate/6f4e617a9727392310971640a866226d.png#pic_center)

#### 2. 操作表

创建表：

`create table tb1(id int, name varchar(50),age int, gender varchar(1));`

查看表的信息：

`desc tb1;` [查询表结构](https://so.csdn.net/so/search?q=%E6%9F%A5%E8%AF%A2%E8%A1%A8%E7%BB%93%E6%9E%84&spm=1001.2101.3001.7020)

`show create table tb1;` 查看系统中存放的表的创建信息

![](https://i-blog.csdnimg.cn/blog_migrate/6069689d001705de2df8e85b2a47b112.png#pic_center)

![](https://i-blog.csdnimg.cn/blog_migrate/1d6c90690c252eb59f740a3d471c567a.png#pic_center)

#### 实践：设计一张员工信息表

![](https://i-blog.csdnimg.cn/blog_migrate/bb4e923823c623454ea6af725f23ead1.png#pic_center)

```
create table emp_info(
    id int,
    worker_idx char(10),
    name varchar(10),
    gender varchar(1),
    age tinyint unsigned,
    idCard char(18),
    DateOfJoin date
);

```

*   创建表

![](https://i-blog.csdnimg.cn/blog_migrate/5fc48fd5ad0238f3ee1d234dc8456b65.png#pic_center)

*   修改表：添加字段
    
    `alter table emp_info add nickname varchar(20);`
    

![](https://i-blog.csdnimg.cn/blog_migrate/ce9d97b9ab1cac48e75a55c6ef2fa15e.png#pic_center)

*   修改表：修改字段名和字段类型
    
    `alter table emp_info change nickname username varchar(30);`
    

![](https://i-blog.csdnimg.cn/blog_migrate/f78fd47483da4c7330ea75596dd87c44.png#pic_center)

*   修改表：[删除字段](https://so.csdn.net/so/search?q=%E5%88%A0%E9%99%A4%E5%AD%97%E6%AE%B5&spm=1001.2101.3001.7020)
    
    `alter table emp_info drop username;`
    

![](https://i-blog.csdnimg.cn/blog_migrate/826b2060f6ef45b4704e7025cb9c078a.png#pic_center)

*   修改表：[修改表名](https://so.csdn.net/so/search?q=%E4%BF%AE%E6%94%B9%E8%A1%A8%E5%90%8D&spm=1001.2101.3001.7020)
    
    `alter table emp_info rename to employee;`
    

![](https://i-blog.csdnimg.cn/blog_migrate/c22c4440654fa2470ae7ad32496aedae.png#pic_center)

*   删除表：drop 或者 truncate
    
    `truncate table employee;`
    
    `drop table if exists to user;`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/da97925fc8b7d09d9689711a1a937e67.png)

### SQL 语法：DML 增删改 表中的数据

*   增加：
    
    ```
    #增加单挑数据 方式1：
    insert employee(id,worker_idx,name,gender,age,idCard,DateOfJoin) value(1,'1','张莉','女','23','111444555566663222','2022-01-01');
    
    # 增加单挑数据 方式2：
    insert employee value(2,'2','王无','女','20','566663222111444555','2020-01-01');
    
    # 增加批量数据 方式1：省略，注意要使用逗号分割
    # 增加批量数据 方式2：
    insert employee value
    (3,'3','小红','女','30','614432221156664555','2000-01-01'),
    (4,'4','小黄','女','31','455322211566661445','1999-01-01'),
    (5,'5','小蓝','男','35','566322211661444555','1980-01-01'),
    (6,'6','小紫','男','35','213221566661444555','1987-01-01');
    
    ```
    

![](https://i-blog.csdnimg.cn/blog_migrate/8f39668bbd536a19d15598760916d432.png#pic_center)

*   修改：
    
    ```
    update employee set name ='ITtest' where id = 1;
    update employee set name = '小朝',gender='男' where id = 1;
    update employee set DateOfJoin ='2008-01-01'; #修改表中该字段的所有值
    
    ```
    
    *   将 id=1 数据，name 修改为 ITtest  
        ![](https://i-blog.csdnimg.cn/blog_migrate/d4fb3b3492cf94022ec95ef1849afcf1.png#pic_center)
        
    *   修改 id=1 的数据，将 name 修改为小朝，gender 修改为男  
        ![](https://i-blog.csdnimg.cn/blog_migrate/6be50fb91b1d9ebc673cb8b127144afa.png#pic_center)
        
    *   将所有员工入职日志求改为 2008-01-01  
        ![](https://i-blog.csdnimg.cn/blog_migrate/32ae97b8a57bf8c55c692bdb8e1cd360.png#pic_center)
        
*   删除
    
    ```
    delete from employee where gender = '男';
    delete from employee; # 不加条件则选中所有删除
    
    ```
    
    *   删除 gender 为男的员工  
        ![](https://i-blog.csdnimg.cn/blog_migrate/3d0efa7ff39a4f0bfbf946da0bb880a2.png#pic_center)
        
    *   删除所有员工 不需要 where 条件  
        ![](https://i-blog.csdnimg.cn/blog_migrate/45be87a6557db013645e5389142af94e.png#pic_center)
        

### SQL 语法：DQL 查询表中的数据

创建表并且添加数据：

```
create table emp(
id int comment '编号',
workno varchar(10) comment '工号',
name varchar(10) comment '姓名',
gender char(1) comment '性别',
age tinyint unsigned comment '年龄',
idcard char(18) comment '身份证号',
workaddress varchar(50) comment '工作地址',
entrydate date comment '入职时间'
)comment '员工表';

INSERT INTO emp VALUES 
(1, '00001', '小红', '女', 20, '123456789012345678', '北京', '2000-01-01'),
(2, '00002', '张无忌', '男', 18, '123456789012345670', '北京', '2005-09-01'),
(3, '00003', '韦一笑', '男', 38, '123456789712345670', '上海', '2005-08-01'),
(4, '00004', '赵敏', '女', 18, '123456757123845670', '北京', '2009-12-01'),
(5, '00005', '小昭', '女', 16, '123456769012345678', '上海', '2007-07-01'),
(6, '00006', '杨逍', '男', 28, '12345678931234567X', '北京', '2006-01-01'),
(7, '00007', '范瑶', '男', 40, '123456789212345670', '北京', '2005-05-01'),
(8, '00008', '黛绮丝', '女', 38, '123456157123645670', '天津', '2015-05-01'),
(9, '00009', '小黄', '女', 45, '123156789012345678', '北京', '2010-04-01'),
(10, '00010', '陈友谅', '男', 53, '123456789012345670', '上海', '2011-01-01'),
(11, '00011', '张士诚', '男', 55, '123567897123465670', '江苏', '2015-05-01'), 
(12, '00012', '常遇春', '男', 32, '123446757152345670', '北京', '2004-02-01'),
(13, '00013', '张三丰', '男', 88, '123656789012345678', '江苏', '2020-11-01'),
(14, '00014', '灭绝', '女', 65, '123456719012345670', '西安', '2019-05-01'),
(15, '00015', '胡青牛', '男', 70, '12345674971234567X', '西安', '2018-04-01'),
(16, '00016', '周芷若', '女', 18, null, '北京', '2012-06-01');

```

![](https://i-blog.csdnimg.cn/blog_migrate/265f4d418bf623d3d3badefddcef59df.png#pic_center)

#### 1. 基本查询（不带任何条件）

1.  查询指定字段 name，workno，age 返回
    
    *   `select name,workno,age from emp;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/107cf6a2a80bdf7438b67f0671b298ed.png#pic_center)
2.  查询所有字段返回
    
    *   `select * from emp;`
3.  查询所有员工的工作地址，起别名
    
    *   `select workaddress as '工作地址' from emp;`
        
        `select workaddress '工作地址' from emp;` AS 可以省略
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/938ae5d4b476f9762f278389af371c11.png#pic_center)
4.  查询公司员工的上班地址，不要重复
    
    *   `select distinct workaddress '工作地址' from emp;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/2808ded7e8c2866b7371603bd10c9dc2.png#pic_center)

#### 2. 条件查询（WHERE）

1.  查询年龄等于 88 的员工
    
    *   `select * from emp where age = 88;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/3c214f61027cb02f4af65bf0b9153a4d.png#pic_center)
2.  查询年龄小于 20 的员工信息
    
    *   `select * from emp where age < 20;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/12da9169952220e3a597ca89ddb23681.png#pic_center)
3.  查询年龄小于等于 20 的员工信息
    
    *   `select * from emp where age <= 20;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/6dc10017142ce8eaa8ea7961a40a2fbc.png#pic_center)
4.  查询没有身份证号的员工信息
    
    *   `select * from emp where idcard is null;`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/49588aa47dc33ed29f3a845c90eef4a7.png#pic_center)
5.  查询有身份证号的员工信息
    
    *   `select * from emp where idcard is not null;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/df813a0438506af98f1d0e447cf7bf61.png#pic_center)
6.  查询年龄不等于 88 的员工信息
    
    *   `select * from emp where age != 88;`
        
        `select * from emp where age <> 88;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/3fe0a5d64a8a0225add08aeb3b01b6a4.png#pic_center)
7.  查询年龄在 15 岁 (包含) 到 20 岁(包含) 之间的员工信息
    
    *   `select * from emp where age between 15 and 20;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/8b2a79c9c72edef19259c9a72e978889.png#pic_center)
8.  查询性别为 女 且年龄小于 25 岁的员工信息
    
    *   `select * from emp where gender = '女' && age < 25;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/4e073dee8cff6f0cabd7a52c98ec93b3.png#pic_center)
9.  查询年龄等于 18 或 20 或 40 的员工信息
    
    *   `select * from emp where age = 18 || age = 20 || age = 40;`
        
        `select * from emp where age in(18,20,40);`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/ac027a6afaaff21357cde183edbf173f.png#pic_center)
10.  查询姓名为两个字的员工信息 _ %
    
    *   `select * from emp where name like '__';`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/b6ac9f5e725745c7bfe83a44b6e9d17d.png#pic_center)
11.  查询身份证号最后一位是 X 的员工信息
    
    *   `select * from emp where idcard like '%X';`
        
        `select * from emp where idcard like '_________________X';`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/79223992e50b88d5779c9f76e577756e.png#pic_center)

#### 3. 聚合函数（count、max、min、avg、sum）

`select 聚合函数(字段列表) from 表名;`

1.  统计该企业的员工数量
    
    *   `select count(*) from emp;`
        
        所有 Null 值不参与聚合运算，因此`select count(idcard) from emp;` 的计算结果是 15
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/0229f85760628fed5cacf591ec06f566.png#pic_center)
2.  统计该企业员工的平均年龄
    
    *   `select avg(age) from emp;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/9c7ea5cb3658d4a3d40ec9fffd93f670.png#pic_center)
3.  统计该企业员工的最大年龄
    
    *   `select max(age) from emp;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/a4ac9af1c1530c36f36f4243bc71dbeb.png#pic_center)
4.  统计该企业员工的最小年龄
    
    *   `select min(age) from emp;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/a2e37e72d2136a7f8b41de0dc89c0b7d.png#pic_center)
5.  统计西安地区员工的年龄之和
    
    *   `select sum(age) from emp where workaddress = '西安';`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/f93d2eb2443959ccaa05114bda5e4e31.png#pic_center)

#### 4. 分组查询（group by）

`select 字段列表 from 表名 [where 条件] group by 分组字段名 [having 分组后的过滤条件];`

1.  根据性别分组，统计男性员工和女性员工的数量
    
    *   `select gender,count(*) from emp group by gender;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/f8a9a80e3639720dc162390171e011cc.png#pic_center)
2.  根据性别分组，统计男性员工和女性员工的平均年龄
    
    *   `select gender,avg(age) from emp group by gender;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/8e4554dde5a83bbfe1a6e17e08c3d6ae.png#pic_center)
3.  查询年龄小于 45 的员工，并根据工作地址分组，获取员工数量大于等于 3 的工作地址
    
    *   `select workaddress,count(*) from emp where age < 45 group by workaddress;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/ba276cb77413190bc8b7d4fd1ba44d86.png#pic_center)
        
        给 count(*) 起别名参与到后面的 having 过滤中：
        
        `select workaddress,count(*) addr_count from emp where age < 45 group by workaddress having addr_count>= 3;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/4e3e8a0d3270ac032b7d4514144e627b.png#pic_center)
        
        分组之前的筛选 where
        
        分组之后的筛选 having 可以用聚合函数用作筛选条件
        
    *   执行顺序：where > 聚合函数 > having
        
    *   分组之后，查询的字段一般为聚合函数和分组字段
        

#### 5. 排序查询（order by）

`SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1 , 字段2 排序方式2 ;`

1.  根据年龄对公司的员工进行升序排序
    
    *   `select name,age from emp order by age;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/6ff4389e8ca32434980eafb4f4601d09.png#pic_center)
2.  根据入职时间, 对员工进行降序排序
    
    *   `select name,entrydate from emp order by entrydate DESC;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/06baab5d3645ec7e649767d9799af946.png#pic_center)
3.  根据年龄对公司的员工进行升序排序 , 年龄相同 , 再按照入职时间进行降序排序
    
    *   `SELECT name,age,entrydate FROM emp ORDER BY age ASC, entrydate DESC;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/a5f5b2938ad80b9d61f9380cc8bcad66.png#pic_center)

#### 6. 分页查询（limit）

`SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询记录数 ;`

1.  查询第 1 页员工数据, 每页展示 10 条记录
    
    *   `select * from emp limit 10;`
        
        `select * from emp limit 0,10;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/f5ef98572595a8b73652ac458185c993.png#pic_center)
2.  查询第 2 页员工数据, 每页展示 10 条记录 --------> 起始索引： (页码 - 1)* 页展示记录数
    
    *   `select * from emp limit 10,10;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/a9a173d1cca9fb5320c9304c8603e26f.png#pic_center)

#### DQL 案例

1.  查询年龄为 20,21,22,23 岁的女性员工信息
    
    *   `select * from emp where gender = '女' AND age between 20 and 23;`
        
        `select * from emp where gender = '女' AND age in(20,21,22,23);`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/0b44bfce6af05590cdc123e186a3b9aa.png#pic_center)
2.  查询性别为 男 ，并且年龄在 20-40 岁 (含) 以内的姓名为三个字的员工
    
    *   `select * from emp where gender='男' && (age between 20 and 40) && name like '___';`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/ac8558950ef33d3f5c465f167cccb6dc.png#pic_center)
3.  统计员工表中, 年龄小于 60 岁的 , 男性员工和女性员工的人数
    
    *   `select gender,count(*) from emp where age < 60 group by gender;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/8cee751fee80067920539d183029b0ee.png#pic_center)
4.  查询所有年龄小于等于 35 岁员工的姓名和年龄，并对查询结果按年龄升序排序，如果年龄相同按 入职时间降序排序。
    
    *   `select name,age,entrydate from emp where age <= 35 order by age ASC,entrydate DESC;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/a25434fc5d8cafe0f1e5cd3d73ea9eaa.png#pic_center)
5.  查询性别为男，且年龄在 20-40 岁 (含) 以内的前 5 个员工信息，对查询的结果按年龄升序排序， 年龄相同按入职时间升序排序。
    
    *   `select * from emp where gender ='男' && age between 20 and 40 order by age asc,entrydate asc limit 5;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/c58ac221794a0eae372d500fc3bab00d.png#pic_center)

#### 验证 DQL 的执行顺序

```
# 编写顺序
select 字段列表 from 表名列表 
group by 分组字段列表 having 分组后条件列表 
order by 排序字段列表 
limit 分页参数;
#执行顺序 from-> where -> group -> select -> order by, having -> limit

```

*   查询年龄大于 15 的员工的姓名、年龄，并根据年龄进行升序排序
    
    *   `select name,age from emp where age > 15 order by age ASC;`
        
        1.  先 from 后 where
            
            `select name,age from emp e where e.age > 15 order by age ASC;`
            
        2.  因为没有使用 order by 和 having 所以不再验证，验证先 where 后 select
            
            `select e.name,e.age from emp e where e.age > 15 order by age ASC;`
            
            `select e.name ename,e.age eage from emp e where eage > 15 order by age ASC;` 报错 where 语句中无法使用 select 中起的别名，因为 where 语句先执行
            
        3.  order by 在 select 的后面
            
            order by 使用 select 起的别名，因为先执行 select 再执行 order by
            
            `select e.name ename, e.age eage from emp e where e.age > 15 order by eage ASC;`
            
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/58c87bec71c8ba258625e0f0a22e4b51.png#pic_center)

### SQL 语法：DCL 管理数据库用户、控制数据库的访问权限

#### 管理用户

0.  使用 mysql，并查看当前的 user 表
    
    `use mysql;`
    
    `select Host,User from user;`
    

![](https://i-blog.csdnimg.cn/blog_migrate/b37d76ca55bad6a3ef597d5d126dd3ba.png#pic_center)

1.  创建用户 itcast, 只能够在当前主机 localhost 访问, 密码 123456
    
    *   `CREATE USER 'itcast'@'localhost' IDENTIFIED BY '123456';`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/d1ba3c91e038aa0b23f5e2dea4b5587d.png#pic_center)
        
        发现该用户 itcast 没有访问数据库的权限
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/618eb464c7f0b94856aa5395b4c62a5a.png#pic_center)![](https://i-blog.csdnimg.cn/blog_migrate/7839898a1268b1157e99cb2097f2a197.png#pic_center)
        
2.  创建用户 heima, 可以在任意主机访问该数据库, 密码 123456
    
    *   `CREATE USER 'heima'@'%' IDENTIFIED BY '123456';`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/1bb61fc7fb720b88a90bfc6b363021a1.png#pic_center)
3.  修改用户 heima 的访问密码为 1234
    
    *   `ALTER USER 'heima'@'%' IDENTIFIED WITH mysql_native_password BY '1234';`
4.  删除 itcast@localhost 用户
    
    *   `DROP USER 'itcast'@'localhost';`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/3ab77efb7d89371805fa7d48492cf39c.png#pic_center)

#### 管理权限

1.  查询权限 `SHOW GRANTS FOR '用户名'@'主机名' ;`
    
    *   `show grants for 'heima'@'%';`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/98f2e221b652d22f0842482e358b895b.png#pic_center)
        
        usage 指的是仅仅能够完成登录，没有其它权限
        
2.  授予权限 `GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';`
    
    *   `grant all on db1.emp to 'heima'@'%';`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/99165800ef5f2ec697f478f020475d90.png#pic_center)
        *   登录上 heima 用户，查看是否自己可以查看的表发生了变化
            
            ![](https://i-blog.csdnimg.cn/blog_migrate/c889e031f7307b585f40f6802110c6fb.png#pic_center)
3.  撤销权限 `REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';`
    
    *   `REVOKE all ON db1.emp FROM 'heima'@'%';`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/c9d13fa6cbc5f3ec46a1cf9a95a7266d.png#pic_center)
        *   登录上 heima 用户，查看自己是否还可以访问 db1.emp 表
            
            ![](https://i-blog.csdnimg.cn/blog_migrate/d40ad2aec2f9cee558573361dee7c2af.png#pic_center)

函数
--

### 函数：字符串函数 concat, lower, upper, lpad, rpad, trim, substring

*   `CONCAT(S1,S2,...Sn)` 字符串拼接
    
    *   `select concat('hello','world');`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/fcbf14e094254270445c54a6c98c79b5.png#pic_center)
*   `LOWER(str)` 将字符串 str 全部转为小写
    
    *   `select lower('HELLO WORLD');`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/8d8da30eb5af0e73f9108faf538921f1.png#pic_center)
*   `UPPER(str)` 将字符串 str 全部转为大写
    
    *   `select upper('hello world');`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/59a34e1575677692be485e731456809a.png#pic_center)
*   `LPAD(str,n,pad)` 左填充
    
    *   `select lpad('world',15,'hello');`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/1d153b9ee56dc4c9b6eec68f0220f606.png#pic_center)
*   `RPAD(str,n,pad)` 右填充
    
    *   `select rpad('hello',15,'world');`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/c5dd8280417115287c8bc9bd7b4652cd.png#pic_center)
*   `TRIM(str)` 去掉字符串头部和尾部的空格
    
    *   `select trim(' hello world ');`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/a51adbdd6344d03e94866fe50280a755.png#pic_center)
*   `SUBSTRING(str,start,len)` 返回子串
    
    *   `select substring('hello world',1,5);`
        
    *   注意：MYSQL 字符串中，索引是从 1 开始而不是从 0 开始
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/52f83fb302ce0aff9caff70d4fbeed63.png#pic_center)

**案例**

由于业务需求变更，企业员工的工号，统一为 3 位数，目前不足 5 位数的全部在前面补 0。比如： 1 号员 工的工号应该为 00001。

![](https://i-blog.csdnimg.cn/blog_migrate/f454e66be66af2d169144a1719744e02.png#pic_center)

*   `select lpad(workno,5,'0') from emp;`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/bac2e37f05bc4426813958719973cfd9.png#pic_center)

### 函数：数值函数 ceil,floor,mod,rand,round

*   `CEIL(x)` 向上取整
    
    *   `select ceil(3.2);`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/e504c70403c0ad16955ca07a99faf167.png#pic_center)
*   `FLOOR(x)` 向下取整
    
    *   `select floor(3.7);`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/daf9f1e7300cd7907c6bf1c16fdc774a.png#pic_center)
*   `MOD(x,y)` 返回 x/y 的模
    
    *   `select mod(20,6);`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/a6fcd2fed63c17f825db8c86f76e2e2e.png#pic_center)
*   `RAND()` 返回 0~1 内的随机数
    
    *   `select rand();`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/81d33acc840a85cbfb8bb54c8c5e506c.png#pic_center)
*   `ROUND(x,y)` 求参数 x 的四舍五入的值，保留 y 位小数
    
    *   `select round(rand(),3);`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/8d94d16d4bf3ffd6f02398a6489e9901.png#pic_center)

**案例**

通过数据库的函数，生成一个六位数的随机验证码

*   `select round(rand()*1000000,0);`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/5db3dc14e8d1cacb3449a69d5526b0c8.png#pic_center)

### 函数：日期函数 curdate, curtime, now, year, month, day, date_add, datediff

*   `CURDATE()` 返回当前日期 年月日
    
    *   `select curdate();`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/7c62c1ced8d025adc961d91975b42acf.png#pic_center)
*   `CURTIME()` 返回当前时间 时分秒
    
    *   `select curtime();`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/1bb6da62877612adbbd0343f13e1eb21.png#pic_center)
*   `NOW()` 返回当前日期和时间 年月日时分秒
    
    *   `select now();`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/1ac5a2dd5701e089102eebd88b2f8c0a.png#pic_center)
*   `YEAR(date)` 获取指定 date 的年份
    
    *   `select year(now());`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/41b9a36fd3e4875689d43aa0889ab61c.png#pic_center)
*   `MONTH(date)` 获取指定 date 的月份
    
    *   `select month(now());`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/71c473c085238f0aef6aa6c2a771725d.png#pic_center)
*   `DAY(date)` 获取指定 date 的日期
    
    *   `select date(now())`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/cc8b5bd169e55446877bd9fd2f150116.png#pic_center)
*   `DATE_ADD(date, INTERVAL expr type)` 返回一个日期 / 时间值加上时间间隔 expr 后的时间值
    
    *   `DATE_ADD(now(), interval 70 year);`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/3b7c2ad5ec1c3dbfff862e8014b76b5d.png#pic_center)
*   `DATEDIFF(date1,date2)` 返回起始时间 date1 和 结束时间 date2 之间的天数
    
    是第一个时间减去第二个时间
    
    *   `select datediff('2023-01-01',now());`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/bc306c29dc70526caa853b21ef363290.png#pic_center)

**案例**

查询所有员工的入职天数，并根据入职天数倒序排序。

`select name,datediff(now(),entrydate) as days from emp order by days desc;`

![](https://i-blog.csdnimg.cn/blog_migrate/4cba72072a57e67f0bdd222b8a1eb49f.png#pic_center)

### 函数：流程控制 if,ifnull,case…when…

*   `IF(value , t , f)` 如果 value 为 true，则返回 t，否则返回 f
    
    *   `select if(1>=1,'success!','faild');`
        
        `select if(1!=1,'success!','faild');`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/7a002ef3f8865194d54f4554a8ce945e.png#pic_center)
*   `IFNULL(value1 , value2)` 如果 value1 不为空，返回 value1，否则 返回 value2
    
    *   `select ifnull('notnull', 'null');` 返回 val1
        
        `select ifnull('', 'null');` 返回 val1，空串也是字符串 不是 null
        
        `select ifnull('null', 'null');` 返回 val2
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/117eb934a04c76fb04c6e3f9b99d197b.png#pic_center)
*   `CASE WHEN [ val1 ] THEN [res1] ... ELSE [ default ] END`
    
    如果 val1 为 true，返回 res1，… 否 则返回 default 默认值
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/73b2d51a38bd7dd084d511f85e65d9a0.png#pic_center)
*   `CASE [ expr ] WHEN [ val1 ] THEN [res1] ... ELSE [ default ] END`
    
    如果 expr 的值等于 val1，返回 res1，… 否则返回 default 默认值
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/22d5666038e2b38f6cbcfc194666477f.png#pic_center)

**案例**

1.  需求: 查询 emp 表的员工姓名和工作地址 (北京 / 上海 ----> 一线城市 , 其他 ----> 二线城市)
    
    ```
    select name,case workaddress when '北京' then '一线城市' when '上海' then '一线城市'
    else '二线城市' end from emp;
    
    select name, (case workaddress when workaddress in('北京','上海') then '一线城市' else '二线城市' end) as '工作地址' from emp;
    
    ```
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/03cc6b053c31e3be35ea8093721890bd.png#pic_center)
2.  统计各个班级的各个成员的成绩，展示规则如下：
    
    `>= 85` 展示优秀；`>= 60` 展示及格； 否则展示不及格
    
    创建表和添加数据的语句如下所示：
    
    ```
    create table score(
    id int comment 'ID',
    name varchar(20) comment '姓名',
    math int comment '数学',
    english int comment '英语',
    chinese int comment '语文'
    ) comment '学员成绩表';
    
    insert into score VALUES (1, 'Tom', 67, 88, 95), (2, 'Rose' , 23, 66, 90),(3, 'Jack', 56, 98, 76);
    
    ```
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/f4554b565c81f00bec8af4a4d50d819b.png#pic_center)
    *   ```
        select id,name,(case when math >= 85 then '优秀' when math >= 60 then '及格' else '不及格' end) as '数学成绩',(case when english >= 85 then '优秀' when english >= 60 then '及格' else '不及格' end) as '英语成绩',(case when chinese >= 85 then '优秀' when chinese >= 60 then '及格' else '不及格' end) as '语文成绩' from score;
        
        ```
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/113f41be924ce1ff6f94cd56975b94d1.png#pic_center)
        
        如果是一个具体的值用 :
        
        `CASE [ expr ] WHEN [ val1 ] THEN [res1] ... ELSE [ default ] END`
        
        但是现在是一个范围因此只能用:
        
        `CASE WHEN [ val1 ] THEN [res1] ... ELSE [ default ] END`
        

约束
--

### 约束：非空约束，唯一约束，主键约束，默认约束，检查约束

案例需求： 根据需求，完成表结构的创建。需求如下

![](https://i-blog.csdnimg.cn/blog_migrate/a487c3116152aefbb409cbfa3fd9c299.png#pic_center)

设计创表语句：

```
create table user(
   id int primary key auto_increment,
   name varchar(10) not null unique,
   age int check (age > 0 && age <= 120),
   status char(1) default '1',
   gender char(1)
);

```

![](https://i-blog.csdnimg.cn/blog_migrate/1cd6a527c275de30f232c6fb071f5de2.png#pic_center)

插入数据：

```
insert into user(name,age,status,gender) values('Rachel',23,'1','女'),('Tom',20,'0','男');

```

![](https://i-blog.csdnimg.cn/blog_migrate/b6616660bae602feb326367aaddd2746.png#pic_center)

*   验证主键约束 和 自增
    
    `insert into user(name,age,status,gender) values('David',21,'0','男');`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/ad56659cb4e859785ac172c828909da7.png#pic_center)
*   验证 name 唯一且非空
    
    *   验证非空，name 为空值则报错
        
        `insert into user(name,age,status,gender) values(null,21,'0','男');`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/4f807d83e128bc89136201e7b497453c.png#pic_center)
    *   验证唯一，name 为重复则报错
        
        `insert into user(name,age,status,gender) values('David',27,'0','男');`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/e3b0be2775c2a93a716d47e487a26f07.png#pic_center)
    *   插入正常的 name
        
        `insert into user(name,age,status,gender) values('Amy',30,'1','女');`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/beddb10bfcc716b3d499ead337d8a25a.png#pic_center)
        
        发现虽然删除失败，但主键已经申请
        
*   验证 age 的 check
    
    `insert into user(name,age,status,gender) values('Harry',-1,'1','男');`
    
    `insert into user(name,age,status,gender) values('Bob',121,'0','男');`  
    ![](https://i-blog.csdnimg.cn/blog_migrate/3a3779bed62eed5f9fefaf835543621f.png#pic_center)
    
*   验证 status 不传值是否默认为 1
    
    `insert into user(name,age,gender) values('Bob',50,'男');`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/0af5f7a13b9dbedc9281162fa2f5dfb4.png#pic_center)

### 约束：外键约束

![](https://i-blog.csdnimg.cn/blog_migrate/0633ebb8fe3d159da8e5c8f5be149736.png#pic_center)

创建表和插入数据：

```
create table dept(
id int auto_increment comment 'ID' primary key,
name varchar(50) not null comment '部门名称'
)comment '部门表';
INSERT INTO dept (id, name) VALUES (1, '研发部'), (2, '市场部'),(3, '财务部'), (4,
'销售部'), (5, '总经办');

create table emp(
id int auto_increment comment 'ID' primary key,
name varchar(50) not null comment '姓名',
age int comment '年龄',
job varchar(20) comment '职位',
salary int comment '薪资',
entrydate date comment '入职时间',
managerid int comment '直属领导ID',
dept_id int comment '部门ID'
)comment '员工表';

INSERT INTO emp (id, name, age, job,salary, entrydate, managerid, dept_id)
VALUES(1, '金庸', 66, '总裁',20000, '2000-01-01', null,5),(2, '张无忌', 20,
'项目经理',12500, '2005-12-05', 1,1),(3, '杨逍', 33, '开发', 8400,'2000-11-03', 2,1),(4, '韦一笑', 48, '开发',11000, '2002-02-05', 2,1),(5, '常遇春', 43, '开发',10500, '2004-09-07', 3,1),(6, '小昭', 19, 'HR',6600, '2004-10-12', 2,1);

```

![](https://i-blog.csdnimg.cn/blog_migrate/9ccad062c60c2990a83a31d9f195ac06.png#pic_center)![](https://i-blog.csdnimg.cn/blog_migrate/cda24ab444e008c1d26520114e5587b4.png#pic_center)

*   添加外键 创建表的时候添加，或者创建表后使用 alter 添加
    
    ```
    CREATE TABLE 表名(
    字段名 数据类型,
    ...
    [CONSTRAINT] [外键名称] FOREIGN KEY (外键字段名) REFERENCES 主表 (主表列名)
    );
    
    ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名)
    REFERENCES 主表 (主表列名) ;
    
    ```
    
    `alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references dept(id);`
    
    *   验证外键的作用：此刻将 dept 中 id=1 的数据删除，查看 emp 的表中是否有变化
        
        报错：无法删除，保证了一致性和完整性
        
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/88d4e2b64b3fbfd2b0d4a45bfa2b54fa.png#pic_center)
*   删除外键
    
    `alter table 表名 drop foreign key 外键名称;`
    
    `alter table emp drop foreign key fk_emp_dept_id;`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/17d529fab6f4b975abe7b0e39ddee3ac.png#pic_center)
    
    删除外键成功
    
*   外键的 删除 / 更新 行为
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/a8a70960166fef828cc873d6dd4a14ca.png#pic_center)
    
    前两个是默认行为，即删除会发生错误
    
    *   验证 cascade 模式
        
        `alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references dept(id) on update cascade on delete cascade;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/cd9a8adf0d48b26521d0620a82075190.png#pic_center)
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/d04b93c71dc432ae24999adba408fa90.png#pic_center)
    *   验证 set null 模式
        
        `alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references dept(id) on update SET NULL on delete SET NULL;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/38c208049c177e34ca65819b4fd9b210.png#pic_center)

多表查询
----

### 多表查询：多表关系 1 对 1，1 对 N/N 对 1，N 对 N

*   一对多 / 多对一
    
    *   例如：部门 1 和 员工 N，一个部门对应多个员工，一个员工对应一个部门
        
        实现：多的一方建立外键 指向一方的主键，作为子表
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/a61d8032ef1194ecf0711eb3e7010d49.png)
*   多对多
    
    *   例如：学生 N 和 课程 N，一个学生选多个课程，一个课程供多个学生选择
        
        实现：建立中间表 含有两个外键，分别关联学生表和课程表的主键
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/502c92a60bd055905caff307fb83087d.png)
*   一对一
    
    *   例如：用户于用户详情的关系，多用于单表拆分
        
        实现：实现：在任意一方加入外键，关联另一方的主键，并且设置为 唯一 unique  
        ![](https://img-blog.csdnimg.cn/6ca088aaaad646a78560b17364d8e919.png)
        

案例：多对多

*   首先，建表 添加数据
    
    ```
    create table student(
    id int auto_increment primary key comment '主键ID',
    name varchar(10) comment '姓名',
    no varchar(10) comment '学号'
    ) comment '学生表';
    insert into student values (null, '黛绮丝', '2000100101'),(null, '谢逊',
    '2000100102'),(null, '殷天正', '2000100103'),(null, '韦一笑', '2000100104');
    
    create table course(
    id int auto_increment primary key comment '主键ID',
    name varchar(10) comment '课程名称'
    ) comment '课程表';
    insert into course values (null, 'Java'), (null, 'PHP'), (null , 'MySQL') ,
    (null, 'Hadoop');
    
    ```
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/c05129ec6306a97f3a802ddb11380880.png#pic_center)
*   需要建立中间表，维护学生表 和 课程表之间的关系，即 建立新表作为中间表，新表包含学生表的主键和课程表的主键作为外键
    
    ```
    create table student_course(
    id int auto_increment comment '主键' primary key,
    studentid int not null comment '学生ID',
    courseid int not null comment '课程ID',
    constraint fk_courseid foreign key (courseid) references course (id),
    constraint fk_studentid foreign key (studentid) references student (id)
    )comment '学生课程中间表';
    insert into student_course values (null,1,1),(null,1,2),(null,1,3),(null,2,2),
    (null,2,3),(null,3,4);
    
    ```
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/c785358ed497a799587f16564edb398b.png#pic_center)

案例：1 对 1

tb_user_edu 表中设置了外键 userid， 且唯一

`userid int unique comment '用户ID'`

`constraint fk_userid foreign key (userid) references tb_user(id)`

```
create table tb_user(
id int auto_increment primary key comment '主键ID',
name varchar(10) comment '姓名',
age int comment '年龄',
gender char(1) comment '1: 男 , 2: 女',
phone char(11) comment '手机号'
) comment '用户基本信息表';

create table tb_user_edu(
id int auto_increment primary key comment '主键ID',
degree varchar(20) comment '学历',
major varchar(50) comment '专业',
primaryschool varchar(50) comment '小学',
middleschool varchar(50) comment '中学',
university varchar(50) comment '大学',
userid int unique comment '用户ID',
constraint fk_userid foreign key (userid) references tb_user(id)
) comment '用户教育信息表';

insert into tb_user(id, name, age, gender, phone) values
(null,'黄渤',45,'1','18800001111'),
(null,'冰冰',35,'2','18800002222'),
(null,'码云',55,'1','18800008888'),
(null,'李彦宏',50,'1','18800009999');

insert into tb_user_edu(id, degree, major, primaryschool, middleschool,
university, userid) values
(null,'本科','舞蹈','静安区第一小学','静安区第一中学','北京舞蹈学院',1),
(null,'硕士','表演','朝阳区第一小学','朝阳区第一中学','北京电影学院',2),
(null,'本科','英语','杭州市第一小学','杭州市第一中学','杭州师范大学',3),
(null,'本科','应用数学','阳泉第一小学','阳泉区第一中学','清华大学',4);

```

### 多表查询： 普通多表查询

创建表：

```
-- 创建dept表，并插入数据
create table dept(
id int auto_increment comment 'ID' primary key,
name varchar(50) not null comment '部门名称'
)comment '部门表';
INSERT INTO dept (id, name) VALUES (1, '研发部'), (2, '市场部'),(3, '财务部'), (4,
'销售部'), (5, '总经办'), (6, '人事部');


-- 创建emp表，并插入数据
create table emp(
id int auto_increment comment 'ID' primary key,
name varchar(50) not null comment '姓名',
age int comment '年龄',
job varchar(20) comment '职位',
salary int comment '薪资',
entrydate date comment '入职时间',
managerid int comment '直属领导ID',
dept_id int comment '部门ID'
)comment '员工表';
-- 添加外键
alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references
dept(id);
INSERT INTO emp (id, name, age, job,salary, entrydate, managerid, dept_id)
VALUES
(1, '金庸', 66, '总裁',20000, '2000-01-01', null,5),
(2, '张无忌', 20, '项目经理',12500, '2005-12-05', 1,1),
(3, '杨逍', 33, '开发', 8400,'2000-11-03', 2,1),
(4, '韦一笑', 48, '开发',11000, '2002-02-05', 2,1),
(5, '常遇春', 43, '开发',10500, '2004-09-07', 3,1),
(6, '小昭', 19, 'hr',6600, '2004-10-12', 2,1),
(7, '灭绝', 60, '财务总监',8500, '2002-09-12', 1,3),
(8, '周芷若', 19, '会计',48000, '2006-06-02', 7,3),
(9, '丁敏君', 23, '出纳',5250, '2009-05-13', 7,3),
(10, '赵敏', 20, '市场部总监',12500, '2004-10-12', 1,2),
(11, '鹿杖客', 56, '职员',3750, '2006-10-03', 10,2),
(12, '鹤笔翁', 19, '职员',3750, '2007-05-09', 10,2),
(13, '方东白', 19, '职员',5500, '2009-02-12', 10,2),
(14, '张三丰', 88, '销售总监',14000, '2004-10-12', 1,4),
(15, '俞莲舟', 38, '销售',4600, '2004-10-12', 14,4),
(16, '宋远桥', 40, '销售',4600, '2004-10-12', 14,4),
(17, '陈友谅', 42, null,2000, '2011-10-12', 1,null);

```

![](https://i-blog.csdnimg.cn/blog_migrate/a3918b6a603fc89c5f8a6ecb78983c26.png#pic_center)

*   `select * from emp,dept;`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/9a1896e172f7a4c6b2e023b8852d0058.png#pic_center) 共 17*6=102 条数据，最后结果是每个员工和六个部门的匹配，不是我们想要的现象
    
    修改：`select * from emp,dept where emp.dept_id = dept.id order by emp.id;` 不包括
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/0cd479566d6574c6122f9db59f57f16e.png#pic_center)

### 多表查询：内连接

*   隐式内连接 `SELECT 字段列表 FROM 表1 , 表2 WHERE 条件 ... ;`
*   显示内连接 `SELECT 字段列表 FROM 表1 [ INNER ] JOIN 表2 ON 连接条件 ... ;`

1.  查询每一个员工的姓名 , 及关联的部门的名称 (隐式内连接实现)
    
    *   `select emp.name,dept.name from emp,dept where emp.dept_id = dept.id;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/f299b77f5bdab865bd571aedc282a906.png#pic_center)
2.  查询每一个员工的姓名 , 及关联的部门的名称 (显式内连接实现)
    
    *   `select emp.name,dept.name from emp inner join dept on emp.dept_id=dept.id;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/ea48b16adc054d3046d29824657efb6f.png#pic_center)

### 多表查询：外连接

*   **左外连接：会返回表 1 的所有记录以及与表 1 关联的表 2 的匹配记录**
    
    `SELECT 字段列表 FROM 表1 LEFT [ OUTER ] JOIN 表2 ON 条件 ... ;`
    
*   **右外连接： RIGHT JOIN 会返回表 2 的所有记录以及与表 2 关联的表 1 的匹配记录**
    
    `SELECT 字段列表 FROM 表1 RIGHT [ OUTER ] JOIN 表2 ON 条件 ... ;`
    

1.  查询 emp 表的所有数据, 和对应的部门信息
    
    *   `select e.*,d.name from emp as e left outer join dept as d on e.dept_id = d.id;`
        
        可以查到 emp 的所有信息，包括 id=17 没有分部门的陈友谅
        
        左外连接，可以获得到左表 emp 的左右信息
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/aa2204bc01894d7d17dd58894e373494.png#pic_center)
2.  查询 dept 表的所有数据, 和对应的员工信息 (右外连接)
    
    *   右外连接：
        
        `select d.*, e.* from dept as d left join emp as e on e.dept_id = d.id;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/3cff11257ab1282d6c51c971675718bf.png#pic_center)
    *   左外连接
        
        `select d.*, e.* from dept as d left join emp as e on e.dept_id = d.id;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/ad50c32a016374191192686c99ba2065.png#pic_center)
        
        可以查到 dept 的所有数据 包括没有员工的人事部
        

### 多表查询：自连接

自连接 必须需要别名，否则全是一张表，是无法构成自连接的

`SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件 ... ;`

1.  查询员工 及其 所属领导的名字
    
    *   `select e_a.name,e_b.name from emp as e_a join emp as e_b on e_a.managerid=e_b.id;`
        
        `select a.name,b.name from emp a,emp b where a.managerid = b.id;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/86ecdb6718b974e803b6baf34c918657.png#pic_center)
2.  查询所有员工 emp 及其领导的名字 emp , 如果员工没有领导, 也需要查询出来 表结构: emp a , emp b
    
    *   `select a.name,b.name from emp as a left join emp as b on a.managerid = b.id;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/0b82d0f5ca806eb7657dc69f44234137.png#pic_center)

### 多表查询：联合查询

把多次查询结果合并，形成一个新的查询结果集

```
SELECT 字段列表 FROM 表A ...
UNION [ ALL ]
SELECT 字段列表 FROM 表B ....;

```

1.  将薪资低于 5000 的员工 , 和 年龄大于 50 岁的员工全部查询出来.
    
    *   `select * from emp where salary < 5000;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/c7d6ed7db8b03277a95b11df25e07fe8.png#pic_center)
        
        `select * from emp where age > 50;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/b663b50d1797ad18978ca691421ae390.png#pic_center)
    *   `select * from emp where salary < 5000 Union all select * from emp where age > 50;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/5e4dacd00598f13a9719f0f903d258d5.png)
        
        鹿杖客因为年薪低于 5000 且 年龄 大于 50，因此在 UNION 中出现了两次， 数据重复
        
        `select * from emp where salary < 5000 Union select * from emp where age > 50;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/7d0ab69e979842f094dbbf2c56612475.png#pic_center)
        
        union all 直接合并，而 union 会进行去重
        

### 多表查询：子查询——标量子查询

SQL 语句中嵌套 SELECT 语句，称为嵌套查询

`SELECT * FROM t1 WHERE column1 = ( SELECT column1 FROM t2 );`

子查询外部的语句可以是 INSERT / UPDATE / DELETE / SELECT 的任何一个

标量子查询：

1.  查询 “销售部” 的所有员工信息
    
    1.  销售部的 dept_id 是多少 `select id from dept where name = '销售部';`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/02f7ec1bd5e0c47096202f74445363c5.png#pic_center)
    2.  在 emp 表中找到 dept_id 是 4 的所有员工信息 `select * from emp where dept_id = (select id from dept where name = '销售部');`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/cec0d2b1d269ea326f51b1074c5f1066.png#pic_center)
2.  查询在 “方东白” 入职之后的员工信息
    
    1.  找到 "方东白" 的入职时间 `select entrydate from emp where name = '方东白';`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/5196fe7462ec5da49572c5703cab76bc.png#pic_center)
    2.  找到 datediff(大家的 entrydate，方东白的入职时间) > 0 的所有员工信息
        
        `select * from emp where entrydate > (select entrydate from emp where name = '方东白');`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/7b0919d0405c22262928ad8e2860056f.png#pic_center)

### 多表查询：子查询——列子查询

**列子查询 (子查询结果为一列)**

子查询返回的结果是一列（可以是多行），称为列子查询

常用的操作符：

*   IN 、在指定的集合范围之内，多选一
*   NOT IN 、 不在指定的集合范围之内
*   ANY 、子查询返回列表中，有任意一个满足即可
*   SOME 、 与 ANY 等同，使用 SOME 的地方都可以使用 ANY
*   ALL，子查询返回列表的所有值都必须满足

1.  查询 “销售部” 和 “市场部” 的所有员工信息
    
    1.  先查看销售部和市场部的部门 ID
        
        `select id from dept where name in('销售部','市场部');`
        
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/4a71221e128a8abd283ebda03b5694db.png#pic_center)
    2.  再在 emp 表中查询对应 dept_id 等于两个部门 ID 的员工信息
        
        `select * from emp where dept_id in (select id from dept where name in('销售部','市场部'));`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/453b1ba744455d122a4144eb2eb31336.png#pic_center)
2.  查询比 财务部 **所有人**工资都高的员工信息
    
    0.  先查找财务部的 id `select id from dept where name = '财务部';`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/13b5579820cd9b7671ed259b6ac25b1f.png#pic_center)
    1.  查找财务部 salary 字段的最大值
        
        `select max(salary) from emp where dept_id = (select id from dept where name = '财务部');`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/f9e28f376695262204460b249dc61d1a.png#pic_center)
    2.  再查询 emp 表中 salary 比该最大值大的员工信息
        
        `select * from emp where salary > (select max(salary) from emp where dept_id = (select id from dept where name = '财务部'));`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/0e3d993c5ad1ce50c833e908c88b1370.png#pic_center)
        
        `select * from emp where salary > all(select salary from emp where dept_id = (select id from dept where name = '财务部'));`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/cf658e8284ba310dab1a7a51567bfd66.png#pic_center)
3.  查询比研发部其中**任意一人**工资高的员工信息
    
    0.  研发部的部门 id `select id from dept where name = '研发部';`
        
    1.  找到研发部的薪水 `select salary from emp where dept_id = (select id from dept where name = '研发部');`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/31a85902ec48afa4e42ac681071e9f5b.png#pic_center)
    2.  找到 emp 中比任一研发部薪水高的 所有员工信息
        
        `select * from emp where salary > any(select salary from emp where dept_id = (select id from dept where name = '研发部'));`
        
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/b5a722b07057c3b5d7e25a523faab393.png#pic_center)

### 多表查询：子查询——行子查询

子查询返回的结果是一行（可以是多列），这种子查询称为行子查询。

常用的操作符：= 、<> 、IN 、NOT IN

1.  查询与 “张无忌” 的薪资及直属领导相同的员工信息 ;
    
    1.  “张无忌” 的薪资和直属领导 是多少
        
        `select salary,managerid from emp where name ='张无忌';`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/46ec7a813be4139cb3ef2ba8ed7e4022.png#pic_center)
    2.  emp 表中谁的薪资和直属领导和张无忌的相同
        
        `select * from emp where (salary,managerid)=(select salary,managerid from emp where name ='张无忌');`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/3b9f73f9e755e631d7524e301915f99b.png#pic_center)
        
        `(salary,managerid)=(select salary,managerid from emp where name ='张无忌');`
        
        因为直接返回的是一行，所以可以直接用来判断
        

### 多表查询：子查询——表查询

子查询返回的结果是多行多列，这种子查询称为表子查询。 常用的操作符：IN

1.  查询与 “鹿杖客” , “宋远桥” 的职位和薪资相同的员工信息
    
    1.  查找 “鹿杖客” , “宋远桥” 的职位和薪资
        
        `select job,salary from emp where name in('鹿杖客' , '宋远桥');`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/232417e066263c6fc088a3f7130dfc97.png#pic_center)
    2.  在 emp 中查找 职位 薪资与上面相同的员工信息
        
        `select * from emp where (job,salary) in(select job,salary from emp where name in('鹿杖客' , '宋远桥'));`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/4b56d96155518f6df2298be36df95536.png)
2.  查询入职日期是 “2006-01-01” 之后的员工信息 , 及其部门信息
    
    *   查询入职日期是 “2006-01-01” 之后的员工信息
        
        “2006-01-01” 之后的员工信息必须要有，部门信息可以为空，用左外或者右外
        
        `select e.*,d.* from (select * from emp where emp.entrydate > '2006-01-01') as e left join dept as d on e.dept_id = d.id;`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/f829d93f0dbc6d39c963a0453b8b3f38.png#pic_center)
        
        from 之后用到了子查询，它会把子查询的结果作为一张表，继续和另一张表进行联查
        

### 多表查询——案例

创建数据库 并且添加数据

```
create table salgrade(
grade int,
losal int,
hisal int
) comment '薪资等级表';
insert into salgrade values (1,0,3000);
insert into salgrade values (2,3001,5000);
insert into salgrade values (3,5001,8000);
insert into salgrade values (4,8001,10000);
insert into salgrade values (5,10001,15000);
insert into salgrade values (6,15001,20000);
insert into salgrade values (7,20001,25000);
insert into salgrade values (8,25001,30000);

```

![](https://i-blog.csdnimg.cn/blog_migrate/5bb493e6d8fd67e79764e6fc482f3ff6.png#pic_center)

![](https://i-blog.csdnimg.cn/blog_migrate/5efc457d35114a88b40f4c24f8b93b0b.png#pic_center)

1.  查询员工的姓名、年龄、职位、部门信息 （隐式内连接）
    
    `select e.name,e.age,e.job,d.name from emp as e,dept as d where e.dept_id=d.id;`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/825ebfe24b40e6fa3c2c8aafd7ac77c2.png#pic_center)
2.  查询年龄小于 30 岁的员工的姓名、年龄、职位、部门信息（显式内连接）
    
    `select e.name,e.age,e.job,d.name from emp as e join dept as d on e.dept_id=d.id where e.age < 30;`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/b6c61a3f0a767312f19aa43be5461af7.png#pic_center)
3.  查询拥有员工的部门 ID、部门名称
    
    `select distinct d.id,d.name from dept as d,emp as e where e.dept_id=d.id;`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/f1fba4e66624acc6eede1b1090dcc8df.png#pic_center)
4.  查询所有年龄大于 40 岁的员工, 及其归属的部门名称; 如果员工没有分配部门, 也需要展示出 来 (外连接)
    
    员工没有员工也需要显示——左外或者右外连接
    
    `select e.*,d.name from emp as e left join dept as d on e.dept_id = d.id where e.age > 40;`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/cb369cfb718ea9ab6952e1f6ae49f678.png#pic_center)
5.  查询所有员工的工资等级
    
    `select e.name,s.grade from emp as e left join salgrade as s on e.salary >= s.losal and e.salary <= s.hisal;`
    
    `select e.name,s.grade from emp as e left join salgrade as s on e.salary between s.losal and s.hisal;`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/312a8642138c75d279341f6233718f66.png#pic_center)
6.  查询 “研发部” 所有员工的信息及 工资等级
    
    `select e.* , s.grade from emp e , dept d , salgrade s where e.dept_id = d.id and ( e.salary between s.losal and s.hisal ) and d.name = '研发部';`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/f39ef02ade0a9c38b2a3dbf261eaa850.png#pic_center)
7.  查询 “研发部” 员工的平均工资
    
    `select avg(salary) from emp as e, dept as d where d.name='研发部' && e.dept_id = d.id;`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/c5430ffbff7840e3bbb47bec62441142.png#pic_center)
8.  查询工资比 “灭绝” 高的员工信息。
    
    `select * from emp where salary > (select salary from emp where name='灭绝');`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/629a4ce0c1b6a9489b37c65f6bb9c624.png#pic_center)
9.  查询比平均薪资高的员工信息
    
    `select * from emp where salary > (select avg(salary) from emp);`
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/50c737f71d91587054f4224bbdd502a2.png#pic_center)
10.  查询低于本部门平均工资的员工信息
    
    1.  查指定部门 id 的薪资
        
        `select avg(salary) from emp where dept_id = '1';`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/9c68fb893b73d26418861e33df77b2d0.png#pic_center)
    2.  `select * from emp e2 where e2.salary < (select avg(e1.salary) from emp as e1 where e1.dept_id=e2.dept_id);`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/1ac1e40f1fcf127168aedb3d5619946d.png#pic_center)
11.  查询所有的部门信息, 并统计部门的员工人数
    
    1.  查询一个指定部门的员工人数
        
        `select count(*) from emp where dept_id= '1';`
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/3877a613133668ba18e5eb8a2c3e722f.png#pic_center)
    2.  子查询 要求所有部门信息，计算部门人数
        
        `select d.id, d.name, ( select count(*) from emp e where e.dept_id = d.id ) '人数' from dept d;`
        
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/b8b950c9c1aa8835b2ebc5b97b4b9166.png#pic_center)
12.  查询所有学生的选课情况, 展示出学生名称, 学号, 课程名称 `student` `student_course` `course`
    

![](https://i-blog.csdnimg.cn/blog_migrate/34f8e4c7c0ac39831e4c9a1bcb17b475.png#pic_center)![](https://i-blog.csdnimg.cn/blog_migrate/7352847a4645ae422b1747fc0cde0ff5.png#pic_center)![](https://i-blog.csdnimg.cn/blog_migrate/91c64fe4db0b08940df62f85f6721b72.png#pic_center)

选课表 和 学生表的融合：

`select sc.id,s.name,s.no,sc.courseid from student_course as sc,student as s where s.id=sc.studentid;`

![](https://i-blog.csdnimg.cn/blog_migrate/3194dcabc797159ff09ab3c9bc75287a.png#pic_center)

上面的表和 course 表的融合

`select a.id,a.name,a.no,c.name from course as c, (select sc.id,s.name,s.no,sc.courseid from student_course as sc,student as s where s.id=sc.studentid) as a where a.courseid=c.id;`

![](https://i-blog.csdnimg.cn/blog_migrate/ffeac930209ab6ba616eb5792df32be4.png#pic_center)

老师写法：

`select s.name,s.no,c.name from student as s, student_course as sc, course as c where sc.studentid=s.id && sc.courseid=c.id;`

![](https://i-blog.csdnimg.cn/blog_migrate/c545849b7b4d15b5432c03d8007ae15b.png#pic_center)

事务
--

### 事务：事务操作

添加表和数据

```
drop table if exists account;
create table account(
id int primary key AUTO_INCREMENT comment 'ID',
name varchar(10) comment '姓名',
money double(10,2) comment '余额'
) comment '账户表';

insert into account(name, money) VALUES ('张三',2000), ('李四',2000);

```

![](https://i-blog.csdnimg.cn/blog_migrate/ada0493409c5cc0ecb1c525267573d7a.png#pic_center)

银行转账操作，张三给李四转账 1000 元，存在以下三个步骤：

1.  查询 张三 账户余额
2.  张三 账户 -1000
3.  李四 账户 +1000

*   模拟正常流程的情况：
    
    ```
    select money from account where name='张三';                # 1. 查询 张三 账户余额 
    update account set money = money - 1000 where name = '张三';# 2. 张三 账户 -1000 
    update account set money = money + 1000 where name ='李四'; # 3. 李四 账户 +1000  
    
    ```
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/a634c503f2c1693234beda15ea91a10d.png#pic_center)
*   将数据都恢复成 2000 后，再模拟非正常情况：
    
    ```
    select money from account where name='张三';                # 1. 查询 张三 账户余额 
    update account set money = money - 1000 where name = '张三';# 2. 张三 账户 -1000 
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
    update account set money = money + 1000 where name ='李四'; # 3. 李四 账户 +1000  
    
    ```
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/8f5339a1f5020419c97bb93d9789873c.png#pic_center)
*   将数据都恢复成 2000 后，使用事务：
    
    *   开启手动管理事务
        
        ```
        SELECT @@autocommit ;  # 设置为手动提交
        SET @@autocommit = 0 ;
        
        ```
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/3fb0b0dc49374d2d6cf3e7bffa02ab52.png#pic_center)
    *   模拟转账成功过程
        
        ```
        select money from account where name='张三'; 
        update account set money = money - 1000 where name = '张三';
        update account set money = money + 1000 where name ='李四'; 
        
        ```
        
        *   没有 commit 提交的结果：
            
            ![](https://i-blog.csdnimg.cn/blog_migrate/5050971eea70d8a092239b03107b1ce5.png#pic_center)
        *   进行 commit 提交的结果：
            
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/daaf6433832ef5b701222aff2f0e4c6c.png#pic_center)
    *   模拟转账中间出现错误过程：
        
        ```
        select money from account where name='张三';               
        update account set money = money - 1000 where name = '张三';
        aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa;
        update account set money = money + 1000 where name ='李四';
        rollback; #回滚
        
        ```
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/1ae97e31c8ac15b2df4dd47b3a4879bc.png#pic_center)
        
        > 注意：在 MySQL 中，如果在事务执行过程中出现错误，通常会直接进行回滚（rollback），而不会执行提交（commit）操作。
        > 
        > 事务是一组数据库操作的逻辑单元，通常用于确保一系列操作要么全部成功执行，要么全部回滚，保持数据库的一致性
        

### 事务：事务的隔离级别

*   查看事务隔离级别
    
    ```
    SELECT @@TRANSACTION_ISOLATION;
    
    ```
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/d4be9cef4b779375fa1bcb8ad3b4ab3d.png)
*   设置事务隔离级别
    
    ```
    SET [SESSION | GLOBAL ] TRANSACTION ISOLATION LEVEL { READ UNCOMMITTED |
    READ COMMITTED | REPEATABLE READ | SERIALIZABLE }
    # 会话级别：SESSION 仅当前对话窗口有效，GLOBAL 对所有对话窗口有效
    # 后面指定的四种级别
    SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED; # 读未提交
    SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;    # 读已提交
    SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;   # 可重复读
    SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE ;     # 串行化
    
    ```
    
    *   **读未提交 的隔离状态：**
        
        *   脏读：
            
            ![](https://i-blog.csdnimg.cn/blog_migrate/6dff3675f8b8ffb18c54f6da111570ce.png#pic_center)
    *   **读已提交的隔离状态：**
        
        *   脏读 已解决
            
            ![](https://i-blog.csdnimg.cn/blog_migrate/43103007e2bb5021236773c9f263dced.png#pic_center)
        *   不可重复度 未解决
            
            ![](https://i-blog.csdnimg.cn/blog_migrate/dbfb5214d1d98481c6f1c36c8c0f0a8a.png#pic_center)
    *   **可重复读 的隔离状态**
        
        *   不可重复读 已解决
            
            ![](https://i-blog.csdnimg.cn/blog_migrate/3ff7bf44b1e3245b1b5a150b50a55bdd.png#pic_center)
        *   幻读 未解决
            
        
        ![](https://i-blog.csdnimg.cn/blog_migrate/d47ef7472746dc5abe28261eb07f751c.png#pic_center)
    *   串行化的隔离级别
        
        *   幻读问题 解决了
            
            ![](https://i-blog.csdnimg.cn/blog_migrate/9f0bc41bdf49a94a56f31d92b554697a.png#pic_center) ![](https://i-blog.csdnimg.cn/blog_migrate/ee33571e7b1ceea722dcc5ca01e7cd19.png#pic_center)
            
            事务 A 在未提交的时候，事务 B 会被阻塞，知道事务 A 提交完毕
            

总结：SQL，函数，约束，多表查询，事务
--------------------

SQL 语句：

*   DDL：![](https://i-blog.csdnimg.cn/blog_migrate/76259429d6ac012bed0a65c40e86d76f.png)
    
*   DML：![](https://i-blog.csdnimg.cn/blog_migrate/5284a0ca1e5bdbdb95ae977bb8671f54.png)
    
*   DQL：![](https://i-blog.csdnimg.cn/blog_migrate/a72aa80fe6801c69a1e8d078517133ff.png)
    
*   DCL：![](https://i-blog.csdnimg.cn/blog_migrate/e91b7412e79fbdc16d8bcd7c9f568c7d.png)
    

函数：![](https://i-blog.csdnimg.cn/blog_migrate/11ce6b582592cf052daf41c27c2e7537.png)

约束：![](https://i-blog.csdnimg.cn/blog_migrate/0ec57c0435c4f4e155d3cbe184232735.png)

![](https://i-blog.csdnimg.cn/blog_migrate/0f96504f7daf603a9a42cf92ad72a6ca.png)

多表查询：![](https://i-blog.csdnimg.cn/blog_migrate/bb8726f042c4a93bf5a2fede2c7c010e.png)

事务：![](https://i-blog.csdnimg.cn/blog_migrate/1906d8a897ef2e933902c4ec14fa0e7c.png#pic_center)