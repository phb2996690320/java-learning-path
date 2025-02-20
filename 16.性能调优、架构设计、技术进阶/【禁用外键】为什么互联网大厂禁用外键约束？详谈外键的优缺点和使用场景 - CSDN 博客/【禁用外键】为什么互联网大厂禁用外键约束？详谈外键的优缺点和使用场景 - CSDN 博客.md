---
url: https://blog.csdn.net/qq_40991313/article/details/131688530
title: 【禁用外键】为什么互联网大厂禁用外键约束？详谈外键的优缺点和使用场景 - CSDN 博客
date: 2025-02-20 13:58:01
tag: 
summary: 
---
**导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[一、外键介绍](#t0)

[1.1 概述](#t1) 

[1.2 练习](#t2)

[1.2.1 数据准备](#t3)

[1.2.2 验证有外键时，删除记录要维护外键](#t4)

[1.2.3 重新添加外键](#t5)

[1.2.4 查询所有外键](#t6)

[二、外键的优缺点](#t7) 

[2.1 外键缺点](#t8)

[2.2 外键优点](#t9)

[三、适用场景](#t10)

[3.1 哪些情况下不要用外键？为什么互联网大厂禁用外键约束？](#t11)

[3.2 哪些情况下可以用外键？](#t12)

## 一、外键介绍

### 1.1 概述 

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

### 1.2 练习

#### 1.2.1 数据准备

![](<assets/1740031081597.png>)

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

添加数据

```
SELECT 
  COLUMN_NAME, 
  CONSTRAINT_NAME, 
  REFERENCED_TABLE_NAME, 
  REFERENCED_COLUMN_NAME 
FROM 
  INFORMATION_SCHEMA.KEY_COLUMN_USAGE 
WHERE 
  TABLE_NAME = 'emp' 
  AND REFERENCED_TABLE_NAME IS NOT NULL;
```

![](<assets/1740031081694.png>)

![](<assets/1740031081911.png>)

#### **1.2.2 验证有外键时，删除记录要维护外键**

此时删除 `研发部` 这条数据，会发现无法删除。

```
DELETE FROM dept WHERE id=1
```

![](<assets/1740031082103.png>)

**1.2.3 验证没外键时，删除记录无需维护外键**  

删除外键

```
alter table emp drop FOREIGN key fk_emp_dept;
```

此时删除 `研发部` 这条数据，会发现删除成功：

```
DELETE FROM dept WHERE id=1
```

![](<assets/1740031083309.png>)

![](<assets/1740031083521.png>)

![](<assets/1740031083787.png>)

#### 1.2.3 重新添加外键

```
alter table emp add CONSTRAINT fk_emp_dept FOREIGN key(dep_id) REFERENCES dept(id);
```

#### 1.2.4 查询所有外键

```
SELECT 
  COLUMN_NAME, 
  CONSTRAINT_NAME, 
  REFERENCED_TABLE_NAME, 
  REFERENCED_COLUMN_NAME 
FROM 
  INFORMATION_SCHEMA.KEY_COLUMN_USAGE 
WHERE 
  TABLE_NAME = 'emp' 
  AND REFERENCED_TABLE_NAME IS NOT NULL;
```

![](<assets/1740031084183.png>)

## 二、外键的优缺点 

### 2.1 外键缺点

*   **改、删时要考虑外键：**每次做 DELETE 或者 UPDATE 都必须考虑外键约束，不方便。
*   **表级锁导致并发差：**并发问题外键约束会启用行级锁主表写入时会进入阻塞
*   **级联删除问题：**删除主表的一条记录，该记录外键关联的从表记录也会随之删除，导致数据不可控。例如删除 “订单表” 的一条订单，关联的 “订单详情表” 的一条记录也会随之删除。
*   **耦合高、迁移麻烦：**主表从表之间互相耦合，主表数据量过大要分表并迁移数据时，就必须先删除外键，不然你刚删完主表的一条记录，从表关联记录也级联删除了，导致数据丢失。

![](<assets/1740031084394.png>)

### 2.2 外键优点

*   **数据一致性：**数据库通过外键可以保证数据的完整性和一致性。因为在更新主表记录时，从表对应的记录也会随之更改，并且更改时会加表级锁，以牺牲并发性为代价，保证数据的一致性。例如删除 “订单表” 的一条订单，关联的 “订单详情表” 的一条记录也会随之删除。
*   **增加 ER 图的可读性：**通过外键，可以更直观的看出表与表之间的关系。

![](<assets/1740031084783.png>)

## 三、适用场景

### 3.1 哪些情况下不要用外键？为什么互联网大厂禁用外键约束？

综合对比外键的优缺点，可以得出结论：

并发量高、表数据量大的分布式项目适合使用外键。防止每次更新数据时都要加表级锁，维护外键的一致性。

因为互联网大厂的产品并发量高，并且一般是**分布式项目**，为了提高数据库的并发性能，一般都**禁用外键**，在业务层面实现数据一致性，例如删除 “订单表” 一条记录前，先关联查询 “订单详情表” 的对应记录，然后先后一起删除。

### 3.2 哪些情况下可以用外键？

综合对比外键的优缺点，可以得出结论：

并发量不高、表数据量不大、对数据一致性要求较高的单体式项目适合使用外键。