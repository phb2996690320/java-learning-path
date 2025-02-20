---
url: https://blog.csdn.net/qq_40991313/article/details/134273900
title: 【阿里规约】阿里开发手册解读——代码格式篇_阿里巴巴代码规范手册 pdf-CSDN 博客
date: 2025-02-20 13:56:48
tag: 
summary: 
---
**导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**阿里规约 PDF：**

[阿里巴巴开发手册. pdf](https://wwmg.lanzouj.com/iBGjO1ctw3sb "阿里巴巴开发手册.pdf")

**目录**

[一、编码和换行符](#t0)

[二、空格规范](#t1)

[2.1 保留字](#t2)

[2.2 二目、三目运算符](#t3) 

[2.3 缩进](#t4)

[2.4 注释](#t5)

[2.5 强制转换](#t6)

[2.6 方法参数](#t7)

[三、行数、字符数](#t8)

[3.1 单行字符数](#t9)

[3.2 方法行数](#t10)

[四、括号](#t11)

[4.1 大括号换行规则](#t12)

[3.2 小括号规则](#t13)

## 一、编码和换行符

**规范：**代码的编码统一使用 UTF-8，换行符使用 Unix 格式，不要使用 Windows 格式。

**换行符：**

*   **Unix 格式**：在 Unix、Linux 以及类 Unix 操作系统中，使用换行符`\n`来表示新行，这是常用的行结束符。
    
*   **Windows 格式**：在 Windows 操作系统中，通常使用回车符和换行符`\r\n`（CR-LF，Carriage Return-Line Feed）来表示新行。
    

**IDEA 设置编码为 UTF-8：**

![](<assets/1740031008594.png>)

## 二、空格规范

### 2.1 保留字

**规范：**if/for/while/switch/do 等保留字与括号之间都必须加空格。 

**正例**（添加空格）：

```
if (condition) {
    // 代码逻辑
}
 
for (int i = 0; i < 10; i++) {
    // 循环体
}
 
while (condition) {
    // 循环体
}
 
switch (value) {
    case 1:
        // 情况1的代码
        break;
    default:
        // 默认情况的代码
}
```

**反例**（不添加空格）：

```
if(condition) {
    // 代码逻辑
}
 
for(int i = 0; i < 10; i++) {
    // 循环体
}
 
while(condition) {
    // 循环体
}
 
switch(value) {
    case 1:
        // 情况1的代码
        break;
    default:
        // 默认情况的代码
}
```

**Java 常见保留字：**

1.  **条件语句关键字**：如 if、else、switch、case、default。
2.  **循环控制关键字**：如 for、while、do、break、continue。
3.  **访问修饰符关键字**：如 public、private、protected。
4.  **数据类型关键字**：如 int、double、char、boolean。
5.  **类和对象关键字**：如 class、new、extends、implements。
6.  **异常处理关键字**：如 try、catch、throw、throws、finally。

### 2.2 二目、三目运算符 

**规范：**任何二目、三目运算符的左右两边需要加一个空格。

**正例**（添加空格）：

```
int result = a + b;
boolean condition = (x > y) ? true : false;
```

在正例中，二目运算符和三目运算符的左右两边都有一个空格，这是一种常见的代码风格。

**反例**（不添加空格）：

```
int result = a+b;
boolean condition=(x>y)?true:false;
```

**java 中常见的二目、三目运算符：**

*   **二目运算符：**+、-、*、/、%、=、==、>=、&&
*   **三目运算符：**?:

### 2.3 缩进

**规范：**采用 4 个空格缩进。可以使用 tab 快捷键直接四个空格，禁止直接使用 “tab” 字符 。

IDEA 设置 tab 为 4 个空格时，请勿勾选 Use tab character；而在 eclipse 中，必须勾选 insert spaces for tabs。

![](<assets/1740031008893.png>)

### 2.4 注释

**规范：**注释双斜线后紧跟一个空格。

```
// 正例： 这是示例注释，请注意在双斜线之后有一个空格
String param = new String();
//反例： 这是示例注释，请注意在双斜线之后没空格
String param = new String();
```

### 2.5 强制转换

**规范：**强制转换时，右括号后无空格。

```
// 正例：
long first = 1000000000000L;
int second = (int)first + 2;
// 反例：
long first = 1000000000000L;
int second = (int) first + 2;
```

### 2.6 方法参数

**规范：**方法参数逗号后加一个空格。

```
// 正例：下例中实参的 args1，后边必须要有一个空格。
method(args1, args2, args3);
 
// 反例：下例中实参的 args1，后边没空格。
method(args1,args2,args3);
method(args1 ,args2 ,args3);
```

## 三、行数、字符数

### 3.1 单行字符数

**规范：**单行字符数不超过 120 个，超过时需要换行。

**换行规则：**

*   第 2/3/4/.. 行相对第一行缩进 4 个空格。
*   运算符随下文一起换行。例如 a + b，应该换行成 a\n + b，而不是 a + \nb
*   点符号与下文一起换行。例如 dog.eat()，应该换行成 dog\n.eat()，而不是 dog.\neat()
*   方法的多个参数换行时，逗号不与下文一起换行。例如 sum (1,2,3)，应该换行成 sum (1,2,\n3)，而不是 sum (1,2\n,3)
*   在括号前不换行。例如 fun (a)，应该换行成 \ nfun (a)，而不是 fun \n(a)

```
// 正例：
StringBuilder sb = new StringBuilder(); 
// 超过 120 个字符的情况下，换行缩进 4 个空格，点号和方法名称一起换行
sb.append("Jack").append("Ma")... 
.append("alibaba")... 
.append("alibaba")... 
.append("alibaba");
 
// 反例：
StringBuilder sb = new StringBuilder(); 
// 超过 120 个字符的情况下，不要在括号前换行
sb.append("Jack").append("Ma")...append 
("alibaba"); 
// 参数很多的方法调用可能超过 120 个字符，不要在逗号前换行
method(args1, args2, args3, ... 
, argsX);
```

### 3.2 方法行数

**规范：**单个方法的总行数不超过 80 行。  

除注释之外的方法签名、左右大括号、方法内代码、空行、回车及任何不可见字符的总行数不超过 80 行。  
 

## 四、括号

### 4.1 大括号换行规则

**规范：**大括号内为空时无需换行。

```
// 正例
int number = 10;
// 大括号内为空，则不换行
if (number > 0) {}
 
// 反例
int number = 10;
if (number > 0) 
{
 
}else{
}
```

**规范：**大括号内非空时： 

*   **左大括号：**前不换行，后换行
*   **右大括号：**前换行，右换行（有 else 时不换行）

```
// 正例
int number = 10;
if (number > 0) {
    System.out.println("这个数字是正数");
} else if(number < -3){
    System.out.println("这个数字不是正数");
} else{}
System.out.println("...");
```

### 3.2 小括号规则

**规范：**小括号内侧不隔空格，外侧隔单空格。

```
// 正例：小括号内侧不隔空格，外侧隔单空格。
if (a == b)
// 反例：
// 小括号外侧没隔单空格。
if(a == b)
// 小括号内侧隔了空格
if ( a == b )
```