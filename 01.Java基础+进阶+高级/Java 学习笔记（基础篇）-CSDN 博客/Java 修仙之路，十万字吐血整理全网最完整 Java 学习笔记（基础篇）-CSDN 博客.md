---
url: https://blog.csdn.net/qq_40991313/article/details/134564921
title: Java 修仙之路，十万字吐血整理全网最完整 Java 学习笔记（基础篇）-CSDN 博客
date: 2025-02-20 09:07:07
tag: 
summary: 
---
**导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**推荐视频：**

[黑马程序员全套 Java 教程_哔哩哔哩](https://www.bilibili.com/video/BV18J411W7cE/?spm_id_from=333.337.search-card.all.click "黑马程序员全套Java教程_哔哩哔哩")

[尚硅谷 Java 入门视频教程_哔哩哔哩](https://www.bilibili.com/video/BV1Kb411W75N/?spm_id_from=333.337.search-card.all.click "尚硅谷Java入门视频教程_哔哩哔哩")

**推荐书籍：**

《Java 编程思想 （第 4 版）》 

《Java 核心技术 · 卷 I（原书第 12 版） : 开发基础》

**目录**

[零、引言](#t0)

[0.1 背景和写作目的](#t1)

[0.2 本期更新内容](#t2)

[一、环境准备](#t3)

[1.1 JDK8](#t4)

[1.1.1 下载](#t5)

[1.1.2 安装](#t6)

[1.1.3 配置环境变量](#t7)

[1.1.4 验证](#t8)

[1.1.5 知识加油站：JDK、JRE、JVM、Java 的区别](#t9)

[1.1.6 知识加油站：Java8 新特性](#t10)

[1.2 记事本体验 Hello World](#t11)

[二、Java 编译器：IDEA](#t12)

[2.1 下载安装配置](#t13)

[2.1.1 下载安装](#t14)

[2.1.2 配置](#t15)

[2.1.2.1 编码配置 UTF-8](#%E7%BC%96%E7%A0%81%E9%85%8D%E7%BD%AEUTF-8)

[2.1.2.2 配置 Maven 路径](#%E9%85%8D%E7%BD%AEMaven%E8%B7%AF%E5%BE%84)

 [2.1.2.3 （根据情况配置）调试时跳过反射以及 aop](#%C2%A02.1.2.3%20%EF%BC%88%E6%A0%B9%E6%8D%AE%E6%83%85%E5%86%B5%E9%85%8D%E7%BD%AE%EF%BC%89%E8%B0%83%E8%AF%95%E6%97%B6%E8%B7%B3%E8%BF%87%E5%8F%8D%E5%B0%84%E4%BB%A5%E5%8F%8Aaop)

[2.1.2.4 注释缩进调整](#2.1.2.4%20%E6%B3%A8%E9%87%8A%E7%BC%A9%E8%BF%9B%E8%B0%83%E6%95%B4)

[2.1.3 安装插件](#t16)

[2.1.3.1 基础插件](#%E5%9F%BA%E7%A1%80%E6%8F%92%E4%BB%B6)

[2.1.3.2 高级插件](#%E9%AB%98%E7%BA%A7%E6%8F%92%E4%BB%B6)

[2.1.3.3 工作插件](#2.1.3.3%20%E5%B7%A5%E4%BD%9C%E6%8F%92%E4%BB%B6)

[2.1.3.3 解决插件安装慢的问题](#2.1.3.3%20%E8%A7%A3%E5%86%B3%E6%8F%92%E4%BB%B6%E5%AE%89%E8%A3%85%E6%85%A2%E7%9A%84%E9%97%AE%E9%A2%98)

[2.1.5 编写 Hello World](#t17)

[2.1.6 常用快捷键](#t18)

[2.2 断点调试](#t19) 

[2.2.1 断点](#t20)

[2.2.2 调试](#t21) 

[2.2.3 知识加油站：高级断点调试](#t22)

[2.2.3 丢帧（退帧）](#2.3.3%20%E4%B8%A2%E5%B8%A7%EF%BC%88%E9%80%80%E5%B8%A7%EF%BC%89)

[2.2.4 强制返回](#2.3.4%20%E5%BC%BA%E5%88%B6%E8%BF%94%E5%9B%9E)

[2.2.5 stream 流调试](#2.3.5%20stream%E6%B5%81%E8%B0%83%E8%AF%95)

[2.2.6 评估表达式：ALT+F8](#2.3.6%20%E8%AF%84%E4%BC%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F)

[2.2.7 多线程调试](#2.3.7%20%E5%A4%9A%E7%BA%BF%E7%A8%8B%E8%B0%83%E8%AF%95%C2%A0) 

[三、Java 基本概念和语法](#t23) 

[3.1 基本数据类型](#t24)

[3.1.1 概述](#t25)

[3.1.2 知识加油站：基本数据类型和引用类型](#t26)

[3.1.3 知识加油站：基本数据类型的内存空间](#t27)

[3.2 数组](#t28)

[3.2.1 创建](#t29)

[3.2.1.1 数组的两种创建方式](#3.2.1.1%20%E6%95%B0%E7%BB%84%E7%9A%84%E4%B8%A4%E7%A7%8D%E5%88%9B%E5%BB%BA%E6%96%B9%E5%BC%8F%C2%A0) 

[3.2.1.2 知识加油站：数组的创建原理](#3.2.1.2%20%E7%9F%A5%E8%AF%86%E5%8A%A0%E6%B2%B9%E7%AB%99%EF%BC%9A%E6%95%B0%E7%BB%84%E7%9A%84%E5%88%9B%E5%BB%BA%E5%8E%9F%E7%90%86)

[3.2.2 初始化](#t30)

[3.2.2.1 静态初始化](#3.2.2.1%C2%A0%E9%9D%99%E6%80%81%E5%88%9D%E5%A7%8B%E5%8C%96)

[3.2.2.2 动态初始化](#3.2.2.2%C2%A0%E5%8A%A8%E6%80%81%E5%88%9D%E5%A7%8B%E5%8C%96)

[3.2.3 访问](#t31)

[3.2.3.1 基本访问方式](#3.2.3.1%20%E5%9F%BA%E6%9C%AC%E8%AE%BF%E9%97%AE%E6%96%B9%E5%BC%8F%C2%A0) 

[3.2.3.2 高级访问方式：迭代器、Stream 流、toString](#3.2.3.2%20%E9%AB%98%E7%BA%A7%E8%AE%BF%E9%97%AE%E6%96%B9%E5%BC%8F%EF%BC%9A%E8%BF%AD%E4%BB%A3%E5%99%A8%E3%80%81Stream%E6%B5%81%E3%80%81toString)

[3.2.4 操作](#t32) 

[3.2.4.1 数组和字符串的互相转换](#3.2.4.1%20%E6%95%B0%E7%BB%84%E5%92%8C%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%9A%84%E4%BA%92%E7%9B%B8%E8%BD%AC%E6%8D%A2)

[3.2.4.2 数组和集合的互相转换](#3.2.4.2%C2%A0%E6%95%B0%E7%BB%84%E5%92%8C%E9%9B%86%E5%90%88%E7%9A%84%E4%BA%92%E7%9B%B8%E8%BD%AC%E6%8D%A2)

[3.3 流程控制语句](#t33)

[3.3.1 概述](#t34)

[3.3.2 IF 分支语句](#t35)

[3.3.2.1 基本语法](#3.3.2.1%20%E5%9F%BA%E6%9C%AC%E8%AF%AD%E6%B3%95)

[3.3.2.2 练习](#3.3.2.2%C2%A0%E7%BB%83%E4%B9%A0)

[3.3.3 switch 分支语句](#t36)

[3.3.3.1 基本语法](#3.3.3.1%20%E5%9F%BA%E6%9C%AC%E8%AF%AD%E6%B3%95%C2%A0) 

[3.3.3.2 练习](#3.3.3.2%20%E7%BB%83%E4%B9%A0)

[3.4 修饰符](#t37)

[3.4.1 访问权限修饰符：public、protected、default、private](#t38)

[3.4.2 static](#t39)

[3.4.2.1 基本介绍](#3.4.2.1%20%E5%9F%BA%E6%9C%AC%E4%BB%8B%E7%BB%8D)

[3.4.2.2 知识加油站：类变量](#3.4.2.2%20%E7%9F%A5%E8%AF%86%E5%8A%A0%E6%B2%B9%E7%AB%99%EF%BC%9A%E7%B1%BB%E5%8F%98%E9%87%8F%E7%9A%84%E5%88%9B%E5%BB%BA%E5%8E%9F%E7%90%86)

[3.4.3 abstract](#t40)

[3.4.3.1 基本介绍](#3.4.3.1%20%E5%9F%BA%E6%9C%AC%E4%BB%8B%E7%BB%8D)

[3.4.3.2 知识加油站：接口和抽象类的区别](#3.4.3.2%20%E7%9F%A5%E8%AF%86%E5%8A%A0%E6%B2%B9%E7%AB%99%EF%BC%9A%E6%8E%A5%E5%8F%A3%E5%92%8C%E6%8A%BD%E8%B1%A1%E7%B1%BB%E7%9A%84%E5%8C%BA%E5%88%AB%C2%A0) 

[3.4.6 final](#t41)

[3.4 常用关键字](#t42)

[3.4.1 this](#t43)

[3.4.2 super](#t44)

[3.4.3 知识加油站：this 和 super 的区别](#t45)

[四、面向对象编程（OOP）](#t46)

[4.0 概述](#t47)

[4.1 类和对象](#t48)

[4.1.1 基本介绍](#t49)

[4.1.2 知识加油站：内部类](#t50)

[4.1.3 知识加油站：创建对象的几种方法](#t51) 

[4.2 方法](#t52)

[4.2.1 基本介绍](#t53)

[4.2.2 基本用法](#t54)

[4.2.2.1 定义](#4.2.2.1%20%E5%AE%9A%E4%B9%89)

[4.2.2.2 调用](#4.2.2.2%20%E8%B0%83%E7%94%A8)

[4.2.2.3 返回值](#4.2.2.3%20%E8%BF%94%E5%9B%9E%E5%80%BC)

[4.2.3 方法的重载](#t55)

[4.2.3.1 重载](#4.2.3.1%20%E9%87%8D%E8%BD%BD)

[3.2.3.2 重载和重写的区别](#3.2.3.2%20%E9%87%8D%E8%BD%BD%E5%92%8C%E9%87%8D%E5%86%99%E7%9A%84%E5%8C%BA%E5%88%AB)

[3.2.4 可变参数](#t56)

[3.2.5 构造方法](#t57)

[4.3 接口和抽象类](#t58)

[4.3.1 概述](#t59)

[4.3.2 接口](#t60)

[4.3.3 抽象类](#t61)

[4.3.4 知识加油站：抽象类和接口的区别](#t62)

[4.4 面对对象三特性](#t63)

[4.4.1 封装](#t64)

[4.4.2 继承](#t65)

[4.4.3 多态](#t66)

[五、常用类](#t67)

[5.1 String 类](#t68)

[5.1.1 基本介绍](#t69) 

[5.1.2 基本用法](#t70)

[5.1.2.1 创建](#5.1.2.1%20%E5%88%9B%E5%BB%BA)

[5.1.2.2 访问](#5.1.2.2%20%E8%AE%BF%E9%97%AE)

[5.1.2.3 连接](#5.1.2.3%C2%A0%E8%BF%9E%E6%8E%A5)

[5.1.2.4 子串](#5.1.2.7%C2%A0%E5%AD%90%E4%B8%B2)

[5.1.2.5 删除](#5.1.2.4%20%E5%88%A0%E9%99%A4)

[5.1.2.6 替换](#5.1.2.5%20%E6%9B%BF%E6%8D%A2)

[5.1.2.7 获取长度](#5.1.2.6%C2%A0%E8%8E%B7%E5%8F%96%E9%95%BF%E5%BA%A6)

[5.1.2.8 比较](#5.1.2.8%C2%A0%E6%AF%94%E8%BE%83)

[5.1.2.8 知识加油站：== 与 equals() 的区别](#5.1.2.8%20%E7%9F%A5%E8%AF%86%E5%8A%A0%E6%B2%B9%E7%AB%99%EF%BC%9A%3D%3D%E4%B8%8Eequals%28%29%E7%9A%84%E5%8C%BA%E5%88%AB)

[5.1.2.9 分割](#5.1.2.9%20%E5%88%86%E5%89%B2)

[5.1.2.10 转换大小写](#5.1.2.10%C2%A0%E8%BD%AC%E6%8D%A2%E5%A4%A7%E5%B0%8F%E5%86%99)

[5.1.2.11 字符串和数字的互相转换](#5.1.2.11%20%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%92%8C%E6%95%B0%E5%AD%97%E7%9A%84%E4%BA%92%E7%9B%B8%E8%BD%AC%E6%8D%A2)

[5.1.2.12 数组和字符串的互相转换](#5.1.2.12%C2%A0%E6%95%B0%E7%BB%84%E5%92%8C%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%9A%84%E4%BA%92%E7%9B%B8%E8%BD%AC%E6%8D%A2)

[5.1.3 知识加油站](#t71)

[5.1.3.1 字符串和字符数组的区别](#5.1.3.1%C2%A0%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%92%8C%E5%AD%97%E7%AC%A6%E6%95%B0%E7%BB%84%E7%9A%84%E5%8C%BA%E5%88%AB)

[5.1.3.2 字符串常量池](#5.1.3.2%20%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%B8%B8%E9%87%8F%E6%B1%A0)

[5.1.3.3 String 不可被继承、不可变的原因](#5.1.3.3%C2%A0String%E4%B8%8D%E5%8F%AF%E8%A2%AB%E7%BB%A7%E6%89%BF%E3%80%81%E4%B8%8D%E5%8F%AF%E5%8F%98%E7%9A%84%E5%8E%9F%E5%9B%A0)

[5.1.3.4 new String("abc") 创建的字符串对象数量](#5.1.3.4%20new%20String%28%22abc%22%29%E5%88%9B%E5%BB%BA%E7%9A%84%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%AF%B9%E8%B1%A1%E6%95%B0%E9%87%8F)

[5.2 StringBuilder 类](#t72)

[5.2.1 基本介绍](#t73)

[5.2.2 知识加油站：String、StringBuffer、Stringbuilder 的区别](#t74)

[5.3 Scanner 类](#t75)

[5.4 Object 类](#t76)

[5.4.1 基本介绍](#t77) 

[5.4.2 知识加油站：JVM 垃圾回收的可达性分析算法](#t78)

[5.4.3 知识加油站：hashCode() 和 equals() 的区别](#t79)

[5.5 System 类](#t80)

[5.6 Integer 类](#t81)

[5.6.1 基本介绍](#t82) 

[5.6.2 知识加油站：包装类的自动拆装箱与自动装箱](#t83)

[5.6.3 知识加油站：什么情况下用包装类？什么情况下用基本数据类型？](#t84)

[5.6.4 知识加油站：包装类和基本数据类型直接如何互相比较？（浮点数等号比较精度丢失问题）](#t85)

[5.6.5 知识加油站：Integer a1=127;Integer a2=127;a1==a2 原因](#t86)

[5.7 Arrays 类](#t87)

[5.6.1 排序](#t88)

[5.6.2 查找和判断](#t89)

[5.6.3 批量填充](#t90)

[5.6.4 转换字符串、链表](#t91)

[5.6.5 拷贝](#t92)

[5.8 Date 类](#t93)

[5.8.1 常用方法](#t94)

[5.8.2 代码示例](#t95)

[5.8.3 知识加油站：Date 类的线程安全问题](#t96)

[5.8.4 格式化日期：](#t97)

[5.8.4.1 SimpleDateformat 类：线程不安全](#5.8.4.1%20SimpleDateformat%E7%B1%BB%EF%BC%9A%E7%BA%BF%E7%A8%8B%E4%B8%8D%E5%AE%89%E5%85%A8)

[5.8.4.2 DateTimeFormatter 类：线程安全](#5.8.4.2%20DateTimeFormatter%E7%B1%BB%EF%BC%9A%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8)

[5.9 Math 类](#t98)

[5.10 Random](#t99)

[5.10.1 基本介绍](#t100)

[5.10.2 代码示例](#t101)

## 零、引言

### 0.1 背景和写作目的

**1. 提供一套完整的 Java 学习路线。**

本专栏旨在提供一套完整的 Java 学习路线，覆盖 [Java 基础知识](https://so.csdn.net/so/search?q=Java%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86&spm=1001.2101.3001.7020)、数据库、SSM/SpringBoot 等框架、Redis/MQ 等中间件、设计模式、架构设计、性能调优、源码解读、核心面试题等全面的知识点，并在未来不断更新和完善，帮助 Java 从业者在更短的时间内成长为高级开发。

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 黑马旅游 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码](https://blog.csdn.net/qq_40991313/article/details/126646289 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/黑马旅游/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码")

**2. 遵循开源精神，贡献自身力量。**

Java 语言拥有一个非常活跃的社区，在笔者的成长过程中，从很多文章中学习到有价值的知识，所以也希望可以能帮助到其他人。

**3. 温故知新，提高技术深度。**

人的记忆力有时候是不可靠的，所以在做事情的过程中要尽量有记录、有结果，这样随着自己的成长，技术深度的提升，可以不断地强化它们，并使自己可以有很多新的见解。

### 0.2 本期更新内容

本期更新了以下内容： 

1.  剔除冗余文字，内容更加精炼。
2.  新增知识加油站，达到深入浅出。
3.  优化大纲，调整章节顺序。
4.  图、文、代码块优化，提高可读性。

## 一、环境准备

### 1.1 JDK8

#### 1.1.1 下载

**下载 jdk8（JDK8 就是 JDK1.8）**

JDK8 或者 JDK1.8 是由于自从 JDK1.5/JDK5 命名方式改变后遗留的新旧命令方式问题。所以 JDK8 或者 JDK1.8 也是同一个东西。

下载地址：[Java Downloads | Oracle](https://www.oracle.com/java/technologies/downloads/#java8 "Java Downloads | Oracle")

选择 windows64 版本 exe 下载

![](<assets/1740013628316.png>)

需要登录才能下载：

![](<assets/1740013628517.png>)

登录后会自动下载：

![](<assets/1740013628618.png>)

#### 1.1.**2 安装**

下一步： 

![](<assets/1740013628718.png>)

设置安装路径：

![](<assets/1740013628810.png>)

点击下一步，将安装完成。 

#### 1.1.3 配置环境变量

此电脑右键 - 属性

![](<assets/1740013628900.png>)

高级系统设置： 

![](<assets/1740013628983.png>)

**环境变量：** 

![](<assets/1740013629072.png>)

**设置环境变量：**

```
JAVA_HOME

```

![](<assets/1740013629184.png>)

![](<assets/1740013629289.png>)

```
%JAVA_HOME%\bin

```

![](<assets/1740013629373.png>)

#### 1.1.4 验证

**验证是否安装成功：**

打开 Windows 的运行（可用 win+R 快捷键打开），输入 cmd

**验证 Java 版本：**

```
java -version

```

如果提示找不到命令，就尝试重新打开一个 cmd 窗口，如果还不行，就检查环境变量是不是配错了。 

**验证 Java 编译命令是否可用：**

```
javac

```

![](<assets/1740013629469.png>)

​​​​

#### 1.1.5 知识加油站：JDK、JRE、JVM、Java 的区别

简而言之，JVM 是 Java 虚拟机，JRE 是 Java 运行环境，JDK 是个 Java 开发的工具包，Java 是门编程语言。 

*   **JVM（Java Virtual Machine）：**是 Java 虚拟机，是 Java 程序运行的基础，它将 Java 程序编译后的字节码解释执行，并将其转换为机器码运行。
*   **JRE（Java Runtime Environment）：**是 Java 运行环境，包括了 JVM 以及 Java 程序运行所需的类库等。
*   **JDK：**Java 开发工具包，包括了 JRE 以及用于 Java 开发的工具，如编译器（javac）、调试器（jdb）、打包工具（jar）等。

#### 1.1.6 知识加油站：Java8 新特性

这里仅简单介绍，具体下面章节会详细介绍。

**Java8 新特性：**

*   **Lambda 表达式：**Lambda 表达式可以被视为一个对象，必须有上下文环境，作用是实现单方法的接口。该特性可以将功能视为方法参数, 或者将代码视为数据。上下文环境意思是能证明它是对象，例如让它处在方法或类的实参里，或者赋值给对象引用。
    *   **省略情况：**形参类型、返回类型可以省略，单参数能省略小括号，单语句能省略 return、分号和大括号（全省略或全不省略）
*   **方法引用：**引用已存在的 Lambda 表达式，达到相同的效果。引用已有 Java 类或对象（实例）的静态方法、实例方法、对象方法（System.out::println;）、构造器方法。可以与 Lambda 联合使用, 方法引用可以使语言的构造更紧凑简洁, 减少冗余代码。
*   **接口默认方法：**允许在接口中定义默认方法, 默认方法必须使用 default 修饰。默认方法是接口中有方法体的方法，用于向已有的接口添加新的功能，而无需破坏现有的实现。实现类可以直接调用默认方法，也可以重写默认方法。
*   **Stream API**：新添加的 Stream API（java.util.stream）支持对元素流进行函数式操作。Stream API 集成在 Collections API 中, 可以对集合进行批量操作（stream 流的生成、操作、收集），例如 filter() 过滤、distinct() 去重、map() 加工、sorted() 排序等操作。
*   **Date Time API 新增 LocalDate、LocalTime、DateTimeFormatter 等类：**加强对日期与时间的处理。LocalDate、LocalTime 可以获取本地时间。线程安全的 DateTimeFormatter 代替线程不安全的 SimpleDateFormat，用于将日期和字符串之间格式转换。
*   **HashMap 底层引入红黑树：**之前版本 HashMap 底层是 “数组 + 链表”，当头插法的 value 链表长度大于等于 8 时，链表会转为红黑树，红黑树查询性能稳定 O(logn)，是近似平衡二叉树，层数最高 2logn。
*   **ConcurrentHashMap 降低锁的粒度：**JDK1.8 之前采用分段锁，锁粒度是分段 segment，JDK1.8 采用 synchronized+CAS，锁粒度是槽（头节点）
*   **CompletableFuture：**是 Future 的实现类，JDK8 引入，用于异步编排。
*   **JVM 方法区的实现方式由永久代改为元空间：**元空间属于本地内存，由操作系统直接管理，不再受 JVM 管理。同时内存空间可以自动扩容，避免内存溢出。默认情况下元空间可以无限使用本地内存，也可以通过 - XX:MetaspaceSize 限制内存大小。

### 1.2 记事本体验 Hello World

1. 新增 txt 文件，输入内容如下，并将文件命名为 Test.java

```
public class Test {
    public Test() {
    }
 
    public static void main(String[] args) {
        System.out.println("hello~~~~~~~");
    }
}
```

![](<assets/1740013629555.png>)

2. 在当前目录下打开 cmd 命令行，编译、运行 Java 代码：

**编译：** 

```
#javac指令编译java代码为Test.class文件
javac Test.java
```

**运行：** 

```
#java指令运行Test.class文件。.class后缀可以省略
java Test
```

 可以看到，命令行会输出

![](<assets/1740013629679.png>)

​​

**扩展：**

**1. 如何打开命令行？**

方法一：在文件地址栏输入 cmd，回车，可以快速在当前目录打开命令行：

![](<assets/1740013629851.png>)

方法二：win+R 快捷键，输入 cmd，回车，然后用 cd 命令到达指定目录：

![](<assets/1740013630206.png>)

**2.javac 指令和 java 指令是什么？**

在 Java 中，`javac`和`java`是两个关键的命令行工具，用于编译和运行 Java 程序。

**javac:**

javac 是 Java 编译器的命令。它用于将 Java 源代码文件（以. java 为扩展名）编译成字节码文件（以. class 为扩展名）。

语法：

```
javac <options> <source files>

```

例如，要编译一个名为

```
MyProgram.java


```

的 Java 源文件，可以使用以下命令：

```
javac MyProgram.java


```

javac 的常见选项：

*   javac -d <directory>：指定生成的类文件存放的目录。
*   javac -cp 或 -classpath：指定查找用户类文件和注释处理程序的位置。
*   javac -source：指定要使用的源代码版本。
*   javac -target：生成的类文件的目标字节码版本。

**java:**

java 是 Java 虚拟机（JVM）的命令。它用于运行已经编译好的 Java 程序。

语法：

```
java <options> <class file>

```

例如，如果你有一个名为

```
MyProgram.class


```

的已编译 Java 程序，可以使用以下命令来运行它：

```
java MyProgram


```

java 的常见选项：

*   java -cp 或 -classpath：指定查找用户类文件的位置。
*   java -Xmx：设置 JVM 的最大堆内存大小。
*   java -Xms：设置 JVM 的初始堆内存大小。

## 二、Java 编译器：IDEA

### 2.1 下载安装配置

#### **2.1.1 下载安装**

[Other Versions - IntelliJ IDEA](https://www.jetbrains.com/idea/download/other.html "Other Versions - IntelliJ IDEA")

安装一直点击下一步即可，只有两点需要注意：

1. 设置安装路径：

![](<assets/1740013630407.png>)

2. 创建桌面快捷方式：

![](<assets/1740013630639.png>)

#### **2.1.2 配置**

**注意：所有设置都必须要关闭项目后，再进行设置，这样才是全局设置。**

**先关闭项目再点击设置**

##### **2.1.2.1 编码配置 UTF-8**

![](<assets/1740013630904.png>)

​​

##### **2.1.2.2** 配置 Maven 路径

这一步初学者可以直接略去，在后面学到 Maven 后再配置。

设置里搜索 Maven，然后配置主路径、仓库路径、配置路径

![](<assets/1740013631254.png>)

​​**Maven 配置依赖和插件后自动刷新：**

![](<assets/1740013631556.png>)

​​

#####  **2.1.2.3 （根据情况配置）调试时跳过**反射以及 aop

![](<assets/1740013631814.png>)

跳过反射

```
java.lang.reflect.* 

```

跳过 spring aop 

```
org.springframework.aop

```

##### 2.1.2.4 注释缩进调整

idea 默认注释会在第一列，需要调整成默认缩进：

![](<assets/1740013632192.png>)

#### **2.1.3 安装插件**

##### **2.1.3.1** 基础插件

IDEA 推荐安装以下插件，便捷开发，初学者可以仅安装第一个插件，后面学到再安装：

**1.Chinese(Simplified) Language Pack / 中文语言包**

用于汉化 IDEA，官方已经加汉化做的很好了，例如 checkout 移位签出，rebase 译为变基。当然习惯了英文可以继续用英文。

![](<assets/1740013632531.png>)

**2.Alibaba Java Coding Guidelines**

提供代码检查，对于不符合编程规范的代码，会进行提示：

```
/**
 * 没有注释作者
 */
public class Test {
    public static void main(String[] args) {
        int a=123;
        System.out.println("hello");//不要使用行尾注释
    }
}
```

![](<assets/1740013632777.png>)

##### **2.1.3.2** 高级插件

下面插件建议学到 Maven 和 Mybatis 后再安装： 

**1.Maven helper**

安装后在代码中右键，可以选择 Maven 模块，会出现打包，编译等选择，另外 Maven helper 可以解决依赖冲突问题。

![](<assets/1740013633028.png>)

**2.MybatisX**

使用 MybatisX 插件后，对应的 mapper.xml,mapper.java 出现小鸟图标，编写接口定义方法后，会自动在 xml 中生成 statement 语句，提高 Mybatis 框架的开发效率。

![](<assets/1740013633278.png>)

**3.RESTfulToolkit-fix** 

RestfulToolkit-fix 是一套 RESTful 服务开发辅助工具集，可以根据 URL 直接跳转到对应的 Controller 方法定义，在 Controller 的方法上添加了能复制请求 URL 和方法参数的功能。

![](<assets/1740013633547.png>)

安装后可以通过 ctrl+\ 快捷键，搜索 url，插件会帮你找到 url 对应的 Controller。 

**全局模糊匹配 URL：**

*   Windows 快捷键: Ctrl + \ 或者 Ctrl + Alt + N。
*   MacOS 快捷键: Command + \ 或者 Command + option + N。

![](<assets/1740013633819.png>)

**5.Private Notes**

用于在只读代码中添加注释，在阅读一些框架和中间件源码时使用比较方便。

在任何你想加注释的地方 按下 Alt + Enter 鼠标移出点击即可保存  
已有私人注释 按下 Alt + Enter 即可快速编辑  
Alt + p 可快速添加或者编辑私人注释  
Alt + o 展示私人注释的其它操作  
右键菜单私人注释查看操作

![](<assets/1740013634050.png>)

**6.easy javadoc**

首先安装插件后重启；

![](<assets/1740013634375.png>)

然后配置类注解：

```
/**
 * @Author: xxx
 * @CreateTime: $DATE$
 * @Description: TODO
 * @Version: 1.0
 */
```

![](<assets/1740013634568.png>)

修改快捷键：

![](<assets/1740013634868.png>)

在类上连按两次 “\”，就会自动生成注释：

![](<assets/1740013635294.png>)

##### **2.1.3.3 工作插件**

**7.LeetCode Editor**

相当于 idea 内置网页版的力扣，可以不打开浏览器，就可以刷题 

![](<assets/1740013635557.png>)

![](<assets/1740013635776.png>)

 **8.stoker**

![](<assets/1740013636053.png>)

![](<assets/1740013636319.png>)

##### 2.1.3.3 解决插件安装慢的问题

如果插件安装速度较慢，可以尝试以下方法进行解决。

**1. 访问 [DNS 查询](https://tool.chinaz.com/ "DNS查询")**

```
https://tool.chinaz.com/

```

**2. 检测**

在页面搜索框中输入 plugins.jetbrains.com，点击检测按钮

```
plugins.jetbrains.com

```

![](<assets/1740013636801.png>)

**3. 寻找最优 dns 地址**

下拉并复制第一个 ip 地址：例如 3.160.150.74  
 

**tip：**注意并不一定要用上面这个 ip 地址，要选择自己网络环境下最优的 ip 地址。选择原值是 TTL 值越大越好，解析时间越小越好

![](<assets/1740013637127.png>)

**4. 更改 hosts 文件**

打开 C:\Windows\System32\drivers\etc 目录下的 hosts 文件，在 hosts 文件中添加下述配置，然后保存退出即可。

```
3.160.150.74 plugins.jetbrains.com

```

![](<assets/1740013637372.png>)

**5. 刷新 DNS**

点击 Win+R，输入 cmd，点击 Enter 按键，然后在黑窗口中执行以下命令： 

```
ipconfig /flushdns

```

![](<assets/1740013637649.png>)

#### **2.1.5 编写** Hello World

**1. 创建空项目**

![](<assets/1740013638129.png>)

![](<assets/1740013638390.png>)

**2. 新建模块**

注意 SDK 开发工具包选 JDK8

![](<assets/1740013638637.png>)

![](<assets/1740013638902.png>)

![](<assets/1740013639201.png>)

 

**3. 新建包**

![](<assets/1740013639494.png>)

![](<assets/1740013639826.png>)

**4. 新建类**

![](<assets/1740013639971.png>)

![](<assets/1740013640291.png>)

然后 psvm 回车自动生成静态 main 方法，sout 回车自动生成 System.out.println() 输出方法。

![](<assets/1740013640447.png>)

```
package package1;
 
/**
 * @author xxx
 */
public class Test {
    public static void main(String[] args) {
        System.out.println("hello world~~~~");
    }
}
```

运行：

![](<assets/1740013640612.png>)

![](<assets/1740013640791.png>)

如果控制台中文乱码，就在设置里把文件编码页都改成 utf-8。

#### **2.1.6 常用快捷键**

**格式化：ctrl+alt+L**

main 方法 psvm 回车，输出 sout 回车，内容提示 ctrl+alt+space。alt+insert 生成构造、setget 方法。

**补全代码：Ctrl+Alt+v 或. var 或. for**

例如写 new Dog(); 按快捷键会自动补全声明 Dog dog=new Dog(); 或者写 new Dog().var 回车会自动补全成 Dog dog=new Dog(); 或者写 lists.for 回车会自动生成 for(List<String> list:lists){}

**万能快捷键：alt+enter**

获取警告、报错提示自动改正。

**查找：ctrl+f**

![](<assets/1740013641009.png>)

**替换：ctrl+r**

替换所有与选中内容相同的文本

![](<assets/1740013641220.png>)

**ctrl+b：跟进**

或者 ctrl + 鼠标左键、或者鼠标左键，跟进到该类的定义页面。如果选中的是标识符，可以查找页面内所有该标识符位置。

**alt + 左键：整列编辑**

**ctrl+d：复制光标行并粘贴**

**ctrl+x：删除光标行**

**Ctrl+h：查看该类继承关系：**

**ctrl+alt+m：抽取选中代码为方法**

![](<assets/1740013641429.png>)

​​

 **Ctrl+f12：查看类中所有方法。**

![](<assets/1740013641803.png>)

​​

**评估表达式：**ALT+F8

![](<assets/1740013642089.png>)

**查看本地历史记录：**

idea 是会自动保存各文件历史记录的，右键文件 - 查看本地历史记录即可：

![](<assets/1740013642364.png>)

​​

![](<assets/1740013642569.png>)

​​

### 2.2 断点调试 

#### 2.2.1 断点

*   调试按钮：
    
    ![](<assets/1740013642821.png>)
    
*   运行按钮：
    
    ![](<assets/1740013643023.png>)
    

**普通断点：**如果断点打在普通代码行上，点击 “debug” 会自动运行到该行暂停；

![](<assets/1740013643238.png>)

**方法断点：**如果断点打在方法上，点击 “debug” 会自动运行到该方法入口处暂停；

**接口断点：**如果断点打在接口方法，点击 “debug” 会自动运行到实现类对应方法入口处暂停；

**字段断点：**如果断点打在字段上，点击 “debug” 会自动运行到修改此字段的代码行处暂停（可以在断点处右键设置成访问时也暂停）；

**条件断点：**在断点处右键，可以输入布尔类型的条件，例如 i==0、user.getName()!=null&&"zhangsan".equals(user.getName())。

**示例：**当 i 为 10 时暂停代码运行：

![](<assets/1740013643432.png>)

debug 可以看到变量信息：

![](<assets/1740013643635.png>)

**异常断点：**在异常时暂停代码运行。在断点处右键 - 更多，设置断点为异常断点，程序将在发生异常时暂停。

![](<assets/1740013643883.png>)

#### 2.2.2 调试 

打断点后代码运行到这一步会停止：下面红色方框是最常用的两个调试按钮：

![](<assets/1740013644494.png>)

​​

蓝色代码行代表此时调试运行到这一行（这一行还没运行），能看到上一行的结果。

调试的五个核心图标分别是：

![](<assets/1740013644851.png>)

​​

1.  **“Step over”：步过。**往下运行一行代码并**跳过**当前方法的调用。
2.  **“Step into”：步入。**进入方法内部并继续逐行执行。适用于自定义方法或第三方方法，但无法进入 JDK 方法的内部。注意如果蓝色光标行不是方法，则此按钮相当于 step over。
3.  **“Force step into”：强制步入（不常用）。**强制进入方法的内部，即使常规的 "Step into" 操作无法进入方法内部时也可以使用。适用于**无法通过常规方式进入的方法**。例如 Symtem.out.println() 等 JDK 方法内部。
4.  **“Step out”：步出。**退出当前方法，即执行完当前方法后返回到调用该方法的位置。注意如果本方法是 main 方法，则将**直接步出这个 main 方法**。
5.  **“Resume Program”：运行到光标处。**继续运行程序直到下一个断点位置或程序结束。可用于暂停状态下的程序恢复运行。

#### 2.2.3 知识加油站：高级断点调试

##### 2.2.3 丢帧（退帧）

**退帧：**回退到此方法被调用之前。

当我们 Debug 从 A 方法进入 B 方法时，通过降帧（退帧）可以返回到调用 B 方法前，这样我们就可以再一次调用 B 方法。

通常用于当我们快执行完 B 方法后，发现某个重要流程被我们跳过了，想再看一下，则此时可以先回退到 A 方法，然后再次进入 B 方法。

![](<assets/1740013645103.png>)

​​

##### 2.2.4 强制返回

右键并点击 “force return” 后，此方法栈将直接终止，恢复到上个方法栈。

使用场景：需要结束当前断点，并且不希望接下来的程序操作数据库时，使用强制返回。

**注意：**注意如果点击红色方格

![](<assets/1740013645380.png>)

或重新调试

![](<assets/1740013645684.png>)

虽然也是终止，但它其实是运行完剩余代码后再终止。

*   强制返回：接下来的程序将不再继续执行。
*   终止：接下来的程序将走完，然后再终止程序。

![](<assets/1740013645917.png>)

​​

**示例：**在下面断点处强制返回，将不打印那行文字；终止则打印。

![](<assets/1740013646125.png>)

##### 2.2.5 stream 流调试

在调试中，断点到达 Stream 流，往往直接会跳过，而我们有时候是需要看 Stream 流的处理逻辑的，所以就需要对 Stream 流进行调试。

**调试方法：**在 stream 流处打断点，debug 后点击 “trace current stream chain”：

**示例：**Stream 流过滤出长度大于 5 单词：

```
public class Test {
    public static void main(String[] args) {
        // 创建一个列表
        List<String> words = Arrays.asList("apple", "banana", "orange", "grape", "watermelon");
 
        // 使用 Stream 进行过滤，只选择长度大于5的单词
        List<String> filteredWords = words.stream()
                .filter(word -> word.length() > 5)
                .collect(Collectors.toList());
 
        System.out.println("Filtered Words: " + filteredWords);
 
    }
}
```

点击 “跟踪当前流链” 即可看到过滤效果：

![](<assets/1740013646380.png>)

##### 2.2.6 评估表达式：ALT+F8

在 if 处打断点，debug 后点击 “evaluate expression”，可以在评估框下测试另一个分支：

![](<assets/1740013646619.png>)

​​

##### 2.2.7 多线程调试 

多线程环境，打断点并右键设置 Thread 或 All：

*   **Thread：**暂停进入断点的线程，不影响其他线程执行。所有线程依次 debug（即你如果给多个线程都加断点，第一个线程 debug 期间其他线程将保持断点暂停状态）
*   **ALL：**暂停全部线程。只 debug 第一个暂停线程（即你如果给多个线程都加断点，那么只第一个断点对应的线程会正常 debug，其他线程会并发运行）

![](<assets/1740013646714.png>)

​​

## 三、Java 基本概念和语法    

### 3.1 基本数据类型

#### 3.1.1 概述

Java 的八大基本数据类型分别是：

*   整型的 byte、short、int、long；
*   字符型的 char；
*   浮点型的 float、double；
*   布尔型的 boolean。 

**代码示例：**

```
public class Test {
    public static void main(String[] args) {
        // 1. 整型
        byte byteData = 120;
        short shortData = 30000;
        int intData = 2000000000;
        long longData = 9000000000000000000L;
 
        // 2. 字符型
        char charData = 'A';
 
        // 3. 浮点型
        float floatData = 3.14f;
        double doubleData = 123.456;
 
        // 4. 布尔型
        boolean booleanData = true;
 
        // 访问和输出变量值
        System.out.println("整型：");
        System.out.println("byteData: " + byteData);
        System.out.println("shortData: " + shortData);
        System.out.println("intData: " + intData);
        System.out.println("longData: " + longData);
 
        System.out.println("\n字符型：");
        System.out.println("charData: " + charData);
 
        System.out.println("\n浮点型：");
        System.out.println("floatData: " + floatData);
        System.out.println("doubleData: " + doubleData);
 
        System.out.println("\n布尔型：");
        System.out.println("booleanData: " + booleanData);
 
        // 操作变量
        intData++; // 增加 intData 的值
        doubleData *= 2; // 将 doubleData 的值乘以 2
 
        // 输出修改后的值
        System.out.println("\n操作后的整型和浮点型变量：");
        System.out.println("intData: " + intData);
        System.out.println("doubleData: " + doubleData);
    }
}
```

 **结果：**

![](<assets/1740013646989.png>)

#### 3.1.2 知识加油站：基本数据类型和引用类型

**基本数据类型** 

基本数据类型共有八大类，这八大数据类型又可分为四小类，分别是整数类型（byte/short/int/long）、浮点类型（float、double）、字符类型（char）和布尔类型（boolean）。

**引用类型**

引用类型包括数组引用、类引用、接口引用，还有一种特殊的 null 类型，所谓引用数据类型就是对一个对象的引用，对象包括实例和数组两种。

**区别：**

<table><thead><tr><th>特征</th><th>基本数据类型</th><th>引用类型</th></tr></thead><tbody><tr><td>存储方式</td><td>直接存储数据值</td><td>存储对象的引用，实际数据存储在堆内存中</td></tr><tr><td>默认值</td><td>有默认值，不可为 null</td><td>有默认值为 null</td></tr><tr><td>赋值方式</td><td>直接赋值</td><td>使用 new 关键字创建对象</td></tr><tr><td>内存分配</td><td>栈上分配</td><td>在堆上分配</td></tr><tr><td>大小</td><td>固定大小，与具体类型有关</td><td>大小不固定，由对象本身和其内容决定</td></tr><tr><td>效率</td><td>更高效，直接操作数据</td><td>相对较低，需要间接操作对象引用</td></tr><tr><td>比较</td><td>用 == 比较</td><td>通常使用 equals 方法比较</td></tr><tr><td>范围</td><td>有限，具体范围取决于数据类型</td><td>无限，取决于系统的内存大小</td></tr><tr><td>传递方式</td><td>值传递，传递的是实际的数据值</td><td>引用传递，传递的是对象的引用</td></tr><tr><td>示例</td><td><code onclick="mdcp.copyCode(event)">int num = 42;</code></td><td><code onclick="mdcp.copyCode(event)">String str = new String("Hello");</code></td></tr><tr><td>JVM 存储位置</td><td><p>方法参数和局部变量：存在本地方法栈的局部变量表；</p><p>final 常量、静态变量：存在类常量池</p><p></p></td><td>堆</td></tr></tbody></table>

#### **3.1.3 知识加油站：基本数据类型的内存空间**

对于基本数据类型，你需要了解每种类型所占据的内存空间，这是面试官喜欢追问的问题：

*    - byte：1 字节（8 位）, 数据范围是 `-2^7 ~ 2^7-1`。
*    - short：2 字节（16 位）, 数据范围是 `-2^15 ~ 2^15-1`。
*    - int：4 字节（32 位）, 数据范围是 `-2^31 ~ 2^31-1`。
*    - long：8 字节（64 位）, 数据范围是 `-2^63 ~ 2^63-1`。c 语言里 long 占 4 字节，long long 占 8 字节。
*    - float：4 字节（32 位）, 数据范围大约是 `-3.4*10^38 ~ 3.4*10^38`。
*    - double：8 字节（64 位）, 数据范围大约是 `-1.8*10^308 ~ 1.8*10^308`。
*    **- char：2 字节**（16 位）, 数据范围是 `\u0000 ~ \uffff`，unicode 编码英文和中文都占两个字节。c 语言使用 ASCII 编码 char 占 1 字节，不能存汉字。ASCII 编码是 Unicode 的一个子集，因此它们存在一些字符码值是相等的。
*    - boolean：Java 规范没有明确的规定, 不同的 JVM 有不同的实现机制。

### 3.2 数组

#### 3.2.1 创建

##### 3.2.1.1 数组的两种创建方式 

**一维数组：**

数组命名有两种方式。

*   **方式一（推荐，符合阿里规约）：**

```
// a和b都是一维数组 
int[] a,b;    
```

*   **方式二：**

```
//c和d都是一维数组，不推荐这种命名方法
int c[],d[];    
```

**二维数组：**

```
int[][] x;

```

**阿里规约：**

【强制】 类型与中括号紧挨相连来表示数组。

正例： 定义整形数组 int[] arrayDemo;

反例： 在 main 参数中，使用 String args[] 来定义

##### 3.2.1.2 知识加油站：数组的创建原理

JVM 内存模型：

[什么是 JVM 的内存模型？详细阐述 Java 中局部变量、常量、类名等信息在 JVM 中的存储位置 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/134742377?spm=1001.2014.3001.5501 "什么是JVM的内存模型？详细阐述Java中局部变量、常量、类名等信息在JVM中的存储位置-CSDN博客")

数组存放在 JVM 的堆中，是一片连续的存储空间，下标依次为 0,1,2,3,4...

**数组在 JVM 中的创建过程：** 

1.  main 方法进入方法栈执行
2.  创建数组，JVM 会在堆内存中开辟空间，存储数组
3.  数组在内存中会有自己的内存地址，以十六进制数表示
4.  数组中有 3 个元素，默认值为 0
5.  JVM 将数组的内存地址赋值给引用类型变量 array
6.  变量 array 保存的是数组内存中的地址，而不是一个具体数值，因此称为引用数据类型。

#### 3.2.2 初始化

Java 数组初始化有两种方式，分别是静态初始化、动态初始化。

*   **静态初始化：**不指定数组长度，由 JVM 自己识别
*   **动态初始化：**指定数组长度

##### 3.2.2.1 **静态初始化**

**静态初始化：**不指定数组长度，由 JVM 自己识别

格式：数据类型 []  数组名 = new 数据类型 {元素 1, 元素 2, 元素 3...}；

示例：

```
// 静态初始化：不指定长度，由系统猜
// 简写格式（推荐）
int[] c={1,2,3};
// 完整格式
int[] ff=new int[]{1,2,3};
```

##### 3.2.2.2 **动态初始化**

**动态初始化：**指定数组长度

格式：数据类型 [] 数组名 = new 数据类型 [数组的长度]; 

示例： 

```
// 动态初始化：指定长度
int[] ff=new int[3];
```

#### 3.2.3 访问

##### 3.2.3.1 基本访问方式 

**通过下标访问数组元素：**

数组存放在 JVM 的堆中，是一片连续的存储空间，下标依次为 0,1,2,3,4...

使用数组的索引（下标）来访问特定位置的元素。数组的索引从 0 开始，一直到数组长度减一。

```
int[] numbers = {1, 2, 3, 4, 5};
int element = numbers[2]; // 访问索引为 2 的元素，即数组中的第三个元素
System.out.println(element); // 输出：3
```

**for 遍历数组：**

使用循环结构（如 `for` 或 `foreach`）遍历数组，访问每个元素。

```
int[] numbers = {1, 2, 3, 4, 5};
 
// 使用 for 循环
for (int i = 0; i < numbers.length; i++) {
    System.out.println(numbers[i]);
}
 
// 使用 foreach 循环
for (int num : numbers) {
    System.out.println(num);
}
```

结果：

![](<assets/1740013647212.png>)

##### 3.2.3.2 高级访问方式：迭代器、Stream 流、toString

**遍历：迭代器**

对于集合类（如 `ArrayList`），可以使用迭代器来访问元素。

```
ArrayList<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.add(3);
 
Iterator<Integer> iterator = list.iterator();
while (iterator.hasNext()) {
    int element = iterator.next();
    System.out.println(element);
}
```

结果：

![](<assets/1740013647431.png>)

**遍历：Java 8 的 Stream**

使用 Java 8 引入的 Stream API 进行数组遍历和操作。

```
int[] numbers = {1, 2, 3, 4, 5};
 
// 使用 Stream.forEach
Arrays.stream(numbers).forEach(System.out::println);
```

**打印全部：Arrays 类的 toString 方法**

使用 `Arrays` 类的 `toString` 方法将整个数组转换为字符串。

```
int[] numbers = {1, 2, 3, 4, 5};
//输出：[1, 2, 3, 4, 5]
System.out.println(Arrays.toString(numbers));
//直接输出数组会输出地址：[I@5b444398
System.out.println(a);    
```

#### 3.2.4 操作 

数组本身的方法很少，只有 equals()、stream() 等简单的方法，一些对数组的高级操作，可以将数组转为 String 或集合，操作后再转回数组。 

##### **3.2.4.1 数组和字符串的互相转换**

**数组转字符串：** 

```
Arrays.toString(数组名)

```

示例： 

```
int[] numbers = {1, 2, 3, 4, 5};
//输出：[1, 2, 3, 4, 5]
System.out.println(Arrays.toString(numbers));
//直接输出数组会输出地址：[I@5b444398
System.out.println(a);    
```

**字符串转数组：**

*   方法名：`stringToIntArray(String str)、stringToCharArray(String str)等`
    

示例代码：

```
// 字符串转整型数组
int[] intArray = stringToIntArray("1 2 3 4 5");
// 字符串转字符数组 
char[] charArray = stringToCharArray("Hello");
```

##### **3.2.4.2** 数组和集合的互相转换

*   **数组转 List：**`List<T> arrayToList(T[] array)`
*   **List 转数组：**`T[] listToArray(List<T> list, Class<T> elementType)`
*   **数组转 Set：**`Set<T> arrayToSet(T[] array)`
*   **Set 转数组：**`T[] setToArray(Set<T> set, Class<T> elementType)`

**示例**：

```
 
        // 1. 数组转List
        String[] array = {"apple", "banana", "orange"};
        List<String> listFromArray = Arrays.asList(array);
        System.out.println("List from Array: " + listFromArray);
 
        // 2. List转数组
        List<String> fruitList = new ArrayList<>(List.of("apple", "banana", "orange"));
        String[] arrayFromList = fruitList.toArray(new String[0]);
        System.out.println("Array from List: " + Arrays.toString(arrayFromList));
 
        // 3. List转Set
        Set<String> setFromList = new HashSet<>(fruitList);
        System.out.println("Set from List: " + setFromList);
 
        // 4. Set转List
        Set<String> fruitSet = new HashSet<>(Set.of("apple", "banana", "orange"));
        List<String> listFromSet = new ArrayList<>(fruitSet);
        System.out.println("List from Set: " + listFromSet);
 
        // 5. Set转数组
        String[] arrayFromSet = fruitSet.toArray(new String[0]);
        System.out.println("Array from Set: " + Arrays.toString(arrayFromSet));
 
        // 6. 数组转Set
        Set<String> setFromArray = new HashSet<>(Arrays.asList(array));
        System.out.println("Set from Array: " + setFromArray);
    }
```

转为集合后，可以通过集合的 api 操作各元素。例如：

**判断数组是否包含某个元素：**

示例代码：

```
// 判断整型数组是否包含元素
int[] array = {1, 2, 3, 4, 5};
boolean isEle = Arrays.asList(array).contains("a");
System.out.println(isEle);
 
``` 

### 3.3 流程控制语句

#### 3.3.1 概述

流程控制语句分类：

*   顺序结构
*   分支结构（if、switch）
*   循环结构（for、while、do…while）

#### 3.3.2 IF 分支语句

##### 3.3.2.1 基本语法

**格式一：如果... 就...**

```
if (关系表达式) {
    语句体;
}
```

**执行流程：**

1.  首先计算关系表达式的值
2.  如果关系表达式的值为 true 就执行语句体
3.  如果关系表达式的值为 false 就不执行语句体
4.  继续执行后面的语句内容

**格式 2：如果... 就...，否则...**

```
if (关系表达式) {
    语句体1;
} else {
    语句体2;
}
```

**执行流程：**

1.  首先计算关系表达式的值
2.  如果关系表达式的值为 true 就执行语句体 1
3.  如果关系表达式的值为 false 就执行语句体 2
4.  继续执行后面的语句内容

**格式 3：如果... 就...，如果... 就...，否则...**

```
if (关系表达式1) {
    语句体1;
} else if (关系表达式2) {
    语句体2;
}
// ...
else {
    语句体n+1;
}
```

**执行流程：**

1.  首先计算关系表达式 1 的值
2.  如果值为 true 就执行语句体 1；如果值为 false 就计算关系表达式 2 的值
3.  如果值为 true 就执行语句体 2；如果值为 false 就计算关系表达式 3 的值 ...
4.  如果没有任何关系表达式值为 true 就执行语句体 n+1

##### 3.3.2.2 **练习**

如果年龄小于 20 岁，则打印年轻人，如果年龄大于 60 岁，则打印老年人，否则打印中年人。

```
public class AgeCategory {
    public static void main(String[] args) {
        // 声明年龄变量
        int age = 35;
 
        // 判断年龄范围并打印相应信息
        if (age < 20) {
            System.out.println("年轻人");
        } else if (age > 60) {
            System.out.println("老年人");
        } else {
            System.out.println("中年人");
        }
    }
}
```

#### 3.3.3 switch 分支语句

##### 3.3.3.1 基本语法 

**格式：**

```
switch (表达式) {
    case 值1:
        语句体1;
        break;
    case 值2:
        语句体2;
        break;
    // ...
    default:
        语句体n+1;
        break; //最后一个可以省略
}
```

**格式说明：**

*   表达式：取值为 byte、short、int、char，JDK5 以后可以是枚举，JDK7 以后可以是 String
*   case：后面跟的是要跟表达式比较的值
*   break：表示中断结束的意思，用来结束 switch 语句
*   default：表示所有情况都不匹配的时候，就执行该处内容，和 if 语句中的 else 相似

**执行流程：**

1.  首先计算表达式的值
2.  以此和 case 后面的值进行比较，如果有对应值，就会执行相应语句，在执行过程中，遇到 break 就会结束
3.  如果所有的 case 的值和表达式的值都不匹配，就会执行 default 里面的语句体，然后程序结束

**注意事项：**在 switch 语句中，如果 case 控制的语句后面不写 break，将会出现 "穿透" 现象。即

##### 3.3.3.2 练习

写一段 Java 代码，输入数字几，就输出星期几：

```
import java.util.Scanner;
 
public class DayOfWeek {
    public static void main(String[] args) {
        // 创建Scanner对象用于输入
        Scanner scanner = new Scanner(System.in);
 
        // 提示用户输入一个数字代表星期几
        System.out.print("请输入一个数字（1-7）代表星期几：");
        
        // 读取用户输入的数字
        int dayNumber = scanner.nextInt();
 
        // 使用Switch语句判断并输出对应的星期几
        switch (dayNumber) {
            case 1:
                System.out.println("星期日");
                break;
            case 2:
                System.out.println("星期一");
                break;
            case 3:
                System.out.println("星期二");
                break;
            case 4:
                System.out.println("星期三");
                break;
            case 5:
                System.out.println("星期四");
                break;
            case 6:
                System.out.println("星期五");
                break;
            case 7:
                System.out.println("星期六");
                break;
            default:
                System.out.println("输入无效，应为1-7之间的数字。");
        }
 
        // 关闭Scanner
        scanner.close();
    }
}
```

### 3.4 修饰符

#### 3.4.1 访问权限修饰符：**public、protected、default、****private**

*   **public** : 对所有类可见。使用对象：类、接口、变量、方法
    
*   **protected** : 对同包可见、对不同包子类可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**。
    
*   **default** : 同包可见。使用对象：类、接口、变量、方法。
    
*   **private** : 在同一类内可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**
    

<table><caption></caption><tbody><tr><th>修饰符</th><th>当前类</th><th>同一包内</th><th>子孙类 (同一包)</th><th>子孙类 (不同包)</th><th>其他包</th></tr><tr><td><code onclick="mdcp.copyCode(event)">public</code></td><td>Y</td><td>Y</td><td>Y</td><td>Y</td><td>Y</td></tr><tr><td><code onclick="mdcp.copyCode(event)">protected</code></td><td>Y</td><td>Y</td><td>Y</td><td>Y/N（<a href="https://www.runoob.com/java/java-modifier-types.html#protected-desc" rel="nofollow" title="说明" target="_blank">说明</a>）</td><td>N</td></tr><tr><td><code onclick="mdcp.copyCode(event)">default</code></td><td>Y</td><td>Y</td><td>Y</td><td>N</td><td>N</td></tr><tr><td><code onclick="mdcp.copyCode(event)">private</code></td><td>Y</td><td>N</td><td>N</td><td>N</td><td>N</td></tr></tbody></table>

**private**

 在同一类内可见，保护成员不被别的类使用。可以修饰变量、方法。 **注意：不能修饰类（外部类）**

private 变量不能被其他类直接访问，但可以通过 get 和 set 方法间接访问：

*   “get 变量名 ()” 方法：获取成员变量的值，方法用 public 修饰
*   “set 变量名 (参数)” 方法：设置成员变量的值，方法用 public 修饰

​​**示例：**

```
public class Phone {
    int price;//成员变量在堆内存
    private int age;
 
    public void setAge(int a) {
        age = a;
    }
//或者用this
//    public void setAge(int age) {
//        this.age = age;
//    }
 
    public int getAge() {
        return age;
    }
}
 
 
//使用
public class hello {
    public static void main(String args[]) {
        Phone p = new Phone();
        p.setAge(12);
        System.out.println(p.getAge());
 
    }
 
}
```

#### 3.4.2 static

##### 3.4.2.1 基本介绍

静态成员变量被所有对象共享，可用类名调用。局部变量不能被声明为 static 变量。

```
public class Phone {
    static int price;//成员变量在堆内存
}
 
 
//使用
public class hello {
    public static void main(String args[]) {
        Phone.price=4;
        //fun();会报错，静态方法只能访问静态方法或变量。
    }
    public fun(){};
}
```

静态方法只能访问静态变量和方法。非静态方法都可以访问。

静态方法中不能使用 this 关键字，因为在静态方法的局部变量表中并不存在 this 变量。

##### 3.4.2.2 **知识加油站：类变量**

**static 可以修饰什么？**

Java 类中包含了成员变量、方法、构造器、初始化块和内部类（包括接口、枚举）5 种成员。

static 关键字可以修饰成员变量、方法、初始化块和内部类，**不能修饰构造器**。

**static 访问规则：**

被 static 修饰的成员先于对象存在，所以又称为类变量。

类成员不能访问实例成员，即静态不能访问非静态，静态中也没有 this 关键字。this 是随着对象的创建存在的。

**类加载过程中类变量是怎样创建的？**

在类加载过程中的链接 - 准备阶段，JVM 会给类变量赋零值，初始化阶段为类变量赋初值，执行静态代码块。

**类加载过程：**加载、链接（验证、准备、解析）、初始化。这个过程是在类加载子系统完成的。

**加载：**生成类的 Class 对象。

1.  通过一个类的全限定名获取定义此类的二进制字节流（即编译时生成的类的 class 字节码文件）
2.  将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构。包括创建运行时常量池，将类常量池的部分符号引用放入运行时常量池。
3.  在内存中生成一个代表这个类的 java.lang.Class 对象，作为方法区这个类各种数据的访问入口。注意类的 class 对象是运行时生成的，类的 class 字节码文件是编译时生成的。

**链接：**将类的二进制数据合并到 JRE 中。该过程分为以下 3 个阶段：

*   **验证：**确保代码符合 JAVA 虚拟机规范和安全约束。包括文件格式验证、元数据验证、字节码验证、符号引用验证。
    *   **文件格式验证：**验证字节码文件是否符合规范。
        *   **魔数：**是否魔数 0xCAFEBABE 开头
        *   **版本号：**版本号是否在 JVM 兼容范围
        *   **常量类型：**类常量池里常量类型是否合法
        *   **索引值：**索引值是否指向不存在或不符合类型的常量。
    *   **元数据验证：**元数据是字节码里类的全名、方法信息、字段信息、继承关系等。
        *   **标识符：**验证类名接口名标识符有没有符合规范
        *   **接口实现方法：**有没有实现接口的所有方法
        *   **抽象类实现方法：**有没有实现抽象类的所有抽象方法
        *   **final 类：**是不是继承了 final 类。
    *   **指令验证：**主要校验类的方法体，通过数据流和控制流分析，保证方法在运行时不会危害虚拟机安全。
        *   **类型转换：**保证方法体中的类型转换是否有效。例如把某个类强转成没继承关系的类
        *   **跳转指令：**保证跳转指令不会跳转到方法体以外的字节码指令上；
        *   保证任意时刻操作数栈的数据类型与指令代码序列都能配合工作。
    *   **符号引用验证：**确保后面解析阶段能正常执行。
        *   **类全限定名地址：**验证类全限定名是否能找到对应的类字节码文件
        *   **引用地址：**引用指向地址是否存在实例
        *   **引用权限：**是否有权引用
*   **准备：**为类变量（即 static 变量）分配内存并赋零值。
*   **解析：**将方法区 - 运行时常量池内的符号引用（类的名字、成员名、标识符）转为直接引用（实际内存地址，不包含任何抽象信息，因此可以直接使用）。

**初始化：**类变量赋初值、执行静态语句块。

#### 3.4.3 abstract

##### 3.4.3.1 基本介绍

*   **一个类不能同时被 abstract 和 final 修饰**，抽象类的唯一目的是为了将来对该类进行**_扩充_**。
*   **抽象类可以包括抽象方法和非抽象方法，抽象方法只能存在于抽象类中。**
*   非抽象子类必须重写抽象父类中的**所有抽象方法**，抽象子类可以直接继承。

```
public abstract class Animal{
    abstract void m(); //抽象方法
abstract void n(); //抽象方法
}
 
class Dog extends Animal{    //非抽象子类
     //实现父类所有抽象方法
      void m(){
          .........
      }
      void n(){
          .........
      }
}
 
 
abstract class Cat extends Animal{}//抽象子类，不需要重写父抽象类的抽象方法
```

##### 3.4.3.2 知识加油站：接口和抽象类的区别 

**设计目的：** 

*   **接口**作为系统与外界交互的窗口, 体现了一种**规范**。它只能定义抽象方法及常量，而不允许存在初始化块、构造器、成员变量。
*   **抽象类**作为系统中多个子类的共同父类, 它体现的是一种**模板式设计**，它可以被当作系统实现过程中的中间产品，必须要有更进一步的完善。

**相同点：**

*   **实例化：**接口和抽象类都不能被实例化, 它们都位于继承树的顶端, 用于被其它类实现和继承
*   **抽象方法：**接口和抽象类都可以有抽象方法, 实现接口或继承抽象类的普通子类都必须实现这些抽象方法

**不同点：**

*   **普通方法：**接口里只能包含抽象方法和默认方法, 不能为普通方法提供方法实现；抽象类则可以包含普通方法。
*   **普通成员变量：**接口里只能定义静态常量（会自动加 static final，final 常量必须显示的指定初始值），不能定义普通成员变量；抽象类里既可以定义普通成员变量, 也可以定义静态常量
*   **构造器：**接口里不包含构造器；抽象类可以包含构造器, 但抽象类的构造器并不是用于创建对象, 而是让其子类调用这些构造器来完成属于抽象类的初始化操作
*   **初始化块：**接口里不能包含初始化块, 抽象类则可以包含初始化块（静态代码块和实例代码块）
*   **单继承多实现：**一个类最多只能有一个父类, 包括抽象类；但一个类可以直接实现多个接口, 通过实现多个接口可以弥补 Java 单继承的不足

#### 3.4.6 final

*   final 变量必须显式指定初始值，不能被重新赋值（非 final 成员变量都会自动有默认值）。
*   final 方法不能被子类重写。
*   final 类不能被继承。
*   final 引用不能变地址值，可以变地址内容。如 final M m=new M();m.age=12; 是正确，引用 m 始终指向对象 M 的内存地址。

### 3.4 常用关键字

#### 3.4.1 this

**①this 指代当前对象的引用**。通过 this 可以获取当前对象中的成员变量、方法。

常可以用于方法的形参与成员变量同名时进行区分，

```
public class Phone {
    private int age;
//如果这个方法是static的，则会报错。
//因为this指向当前对象的引用，不是指向当前类的引用。static方法是类方法，类方法不创建对象就可以通过类名调用。
    public void setAge(int age) {
        this.age = age;
    }
 
    public int getAge() {
        return age;
    }
}
```

**②this 指代构造方法的调用**

```
package package1;
 
public class Dog {
    private int age;
 
    public Dog() {
        System.out.println("无参构造方法。。");
    }
 
    public Dog(int age) {
        //先执行一次带参构造方法
        this();
        System.out.println("带参构造方法，年龄："+age);
        this.age = age;
    }
 
 
    public static void main(String[] args) {
        //带参构造方法创建对象
        Dog dog = new Dog(23);
    }
}
```

this 指代构造方法的调用时，有以下几点需要注意：

#### 3.4.2 super

**①指代仅包括父类特征的当前对象的引用。可以看做是父类的引用。**

通过 super 可以获取当前对象的父对象的成员变量、方法。

示例：

以 age 变量为例，父类的 age 变量是 3，子类的 age 变量是 4，子类方法的 age 局部变量是 5。

```
public class Animal {
    public String name;
    int age = 3;
 
    public void eat() {
        //吃东西方法的具体实现
        System.out.println("吃东西");
    }
} 
//子类 
public class Dog extends  Animal{ 
    int age=4;
    public void show(){
        int age=5;
//输出5。访问上一行的局部变量
         System.out.println(age);   
//4。通过this访问当前对象的成员变量age
        System.out.println(this.age);
//3。通过super访问当前对象的父对象的成员变量age
        System.out.println(super.age);
    }
}
```

**②指代父类构造方法的调用**

通过 super()，可以调用父类的各个构造方法。 

示例： 

```
public class Animal {
    public String name;
    int age = 3;
 
 
    public void eat() {
        //吃东西方法的具体实现
        System.out.println("吃东西");
    }
 
    public Animal(String name) {
        System.out.println("动物类带参构造方法"+name);
        this.name = name;
    }
 
    public void sleep() {
        //睡觉方法的具体实现
    }
}
public class Dog extends Animal {
 
    public Dog(int age,String name) {
//调用父类的带参构造方法
        super("小白");
        System.out.println("带参构造方法"+age+","+name);
    }
 
    public static void main(String[] args) {
        new Dog(23,"小黑");
    }
}
```

结果：

![](<assets/1740013647686.png>)

#### 3.4.3 知识加油站：this 和 super 的区别

**this 和 super 的区别：** 

*   this 是当前对象的引用，super 是当前对象的父对象的引用。
*   this() 是构造方法中调用本类其他的构造方法，super() 是当前对象构造方法中去调用自己父类的构造方法。
*   静态中没有 this 和 super 关键字。this 指向当前实例，super 指向父类实例，是随着对象的创建存在的。

**注意：**子类所有无参、带参构造方法第一行都会隐式或显式地加 super()。this() 和 super() 方法不能显式的共存，但可以隐式的共存，且都只能显式地出现在构造方法的第一行。

*   如果都不加，则系统会隐式加 super()；
*   如果加 super() 或 super(带参)，系统不会再隐式加 super()；
*   如果加 this()，则系统会隐式在 this() 前加 super()；

## 四、面向对象编程（OOP）

### 4.0 概述

面向对象（Object-Oriented，简称 OOP）是一种程序设计的范式，它基于对象的概念，**将数据和操作数据的行为封装在对象中**，以模拟现实世界的问题和解决方案。

**核心概念：**

*   **对象（Object）：** 对象是现实世界中的实体或概念，在程序中被抽象为具有状态（属性）和行为（方法）的实例。
*   **类（Class）：** 类是对象的模板，它定义了对象的属性和方法。类是对象的抽象，实际的对象是根据类的定义实例化而来的。
*   **封装（Encapsulation）：** 封装是将对象的属性和方法封装在一个单元内，对外部隐藏对象的具体实现细节。通过封装，对象的内部实现对外部是不可见的，只有公共接口对外部可见。
*   **继承（Inheritance）：** 继承允许一个类（子类）继承另一个类（父类）的属性和方法。子类可以继承父类的属性、重写父类的方法，从而减少代码量。
*   **多态（Polymorphism）：** 多态允许不同的对象对同一消息做出响应，提供了灵活性和可扩展性。多态的实现方式包括方法重载和方法重写。

### 4.1 类和对象

#### 4.1.1 基本介绍

*   **类：**现实中一种具有共同属性、行为的事物的抽象。它定义了一组属性（成员变量）和方法（成员方法），用于描述具有相似特征和行为的对象集合。
*   **对象：**对象是类的一个实例，是能够看得到摸的着的真实存在的实体。

**总结起来就是一句话：**类是对象的抽象，对象是类的具体实现。

**示例：** 

例如我声明一个 Person 类，有名字和年龄两个属性；

然后我就可以通过这个类创建两个对象，分别是 Alice 和 Bob，他们的年龄不一样。

```
public class Person {
    // 成员变量
    String name;
    int age;
 
    // 构造方法
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
 
    // 成员方法
    public void work() {
        System.out.println(name + " is working.");
    }
 
    public void study() {
        System.out.println(name + " is studying.");
    }
}
```

```
public class Test {
    public static void main(String[] args) {
        // 创建 Person 类的对象
        Person person1 = new Person("Alice", 25);
        Person person2 = new Person("Bob", 30);
 
        // 调用对象的方法
        person1.work();
        person2.study();
    }
}
```

**结果：**

```
Alice is working.
Bob is working.
```

#### 4.1.2 知识加油站：内部类

**内部类：**在一个类中定义类。

**分为：**

*   **成员内部类（成员位置）**
*   **局部内部类（成员方法内）**
*   **匿名内部类（方法内）**

**匿名内部类**：一个**继承**了其他类**或者实现**了其他接口的子类对象。

内部类可以直接访问外部类私有、公有成员。外部类要访问内部类成员要创建对象。

```
public class Outer {
    int num=10;
//定义成员内部类：在成员位置定义类
    public class Inner{        //一般private，用外部类访问内部类更安全
        public void show(){
            System.out.println("innershow"+num);    //内部类可直接访问外部类成员。
        }
    }
 
//外部类访问成员内部类
    public void fun(){        
        Inner in=new Inner();
        in.show();
    }
 
//局部内部类：在成员方法内定义类
    public void fun2(){
        class Inner2{        //成员内部类不能public和private
            void show(){
                System.out.println("成员内部类");
            }
        }
        Inner2 in=new Inner2();
        in.show();
    }
 
//匿名内部类：在成员方法内定义子类，实现接口或继承抽象类
    public void fun3(){
        Phone p=new Phone() {            //自己写的Phone接口
            @Override
            public void show() {
                System.out.println("实现接口的匿名内部类");
            }
        };            //注意是一个语句有分号
        p.show();
    }
}
 
 
//内部类创建对象（一般内部类私有，使用外部类访问内部类）
public class Test {
    public static void main(String[] args) {
    Outer.Inner i=new Outer().new Inner();
    i.show();
    }
}
```

#### 4.1.3 知识加油站：创建对象的几种方法 

**概述：** 

*   **new：**例如 Person person1 = new Person();
*   **反射：**先获取类的 Class 对象，通过 newInstance() 方法创建对象；
*   **对象的 clone() 方法：** 类实现 Cloneable 接口，重写 clone() 方法， 编写浅拷贝或者深拷贝的逻辑。然后调用对象的 clone() 方法就可以克隆出对象。
*   **反序列化：**反序列化对象时，JVM 会创建一个单独的对象。需要让类实现 Serializable 接口，通过 ObjectInputStream 类的 readObject() 方法从磁盘中反序列化对象。反序列化创建对象是深拷贝。

**详细：**

**使用 `new` 关键字：**

```
Person person1 = new Person();


```

这是最常见的创建对象的方式，使用 `new` 关键字直接调用类的构造方法。

**反射：**

通过反射机制，可以在运行时获取类的信息，动态创建对象。

```
Class<?> clazz = Person.class;
Person person2 = (Person) clazz.newInstance();
```

**对象的 `clone()` 方法：**

要实现克隆，类必须实现 `Cloneable` 接口，并重写 `clone()` 方法。这种方式可以实现对象的浅拷贝或深拷贝。

```
public class Person implements Cloneable {
    // 其他类定义
 
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
 
// 创建对象
Person person3 = (Person) person1.clone();
```

**反序列化：**

通过反序列化，可以将对象的状态从持久性存储中重新创建出来。需要让类实现 `Serializable` 接口，并使用 `ObjectInputStream` 类的 `readObject()` 方法从文件或网络中反序列化对象。

```
// 反序列化对象
try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("person.ser"))) {
    Person person4 = (Person) ois.readObject();
} catch (IOException | ClassNotFoundException e) {
    e.printStackTrace();
}
```

### 4.2 方法

#### 4.2.1 基本介绍

**方法（Method）：**一组执行特定任务的代码块。它可以在类中定义一次，然后在本方法、其他方法中被多次调用。

**作用：**提高代码的可读性和可维护性。

例如小明要吃多顿饭，每顿饭吃的食物不同，我们可以将 “吃饭” 这个行为抽象成方法，从而减少代码量：

```
public class Test {
    private static String name = "小明";
 
    /**
     * 吃饭方法
     *
     * @param food
     */
    public static void eat(String food) {
        System.out.println("=====");
        System.out.println(name + " is eating " + food);
        System.out.println("=====");
    }
 
    public static void main(String[] args) {
        eat("西瓜");
        eat("白菜");
        eat("可乐");
        eat("番茄");
 
    }
}
```

结果：

![](<assets/1740013647859.png>)

如果不用方法，将需要多写很多代码，可读性变差：

```
public class Test {
    private static String name = "小明";
 
    /**
     * 吃饭方法
     *
     * @param food
     */
    public static void eat(String food) {
        System.out.println("=====");
        System.out.println(name + " is eating " + food);
        System.out.println("=====");
    }
 
    public static void main(String[] args) {
        System.out.println("=====");
        System.out.println(name + " is eating 西瓜" );
        System.out.println("=====");
        System.out.println("=====");
        System.out.println(name + " is eating 白菜");
        System.out.println("=====");
        System.out.println("=====");
        System.out.println(name + " is eating 可乐");
        System.out.println("=====");
        System.out.println("=====");
        System.out.println(name + " is eating 番茄");
        System.out.println("=====");
 
    }
}
```

#### 4.2.2 基本用法

##### 4.2.2.1 定义

在 Java 中，方法的定义包括以下几个要素：

*   **修饰符：** 方法可以有访问修饰符，例如 `public`、`private`、`protected` 或默认（包内可见）。
*   **返回类型：** 方法可以返回一个值，指定返回值的数据类型，如果方法不返回任何值，可以使用 `void`。
*   **方法名：** 方法名是方法的标识符，用于在程序中调用方法。
*   **参数列表：** 方法可以接受零个或多个参数，参数用于向方法传递数据。
*   **方法体：** 方法体包含实际执行的代码块，实现方法的功能。

```
// 方法的定义
public int add(int num1, int num2) {
    int sum = num1 + num2;
    return sum;
}
```

##### 4.2.2.2 调用

在程序中，可以通过方法名和参数列表来调用方法。方法调用是程序执行的一个重要步骤，它使代码更具可重用性。

```
// 方法的调用
int result = add(5, 3);
System.out.println("Result: " + result);
```

##### 4.2.2.3 返回值

方法可以返回一个值，也可以是 `void`，表示不返回任何值。如果方法返回值，必须使用 `return` 语句返回对应类型的值。

```
// 方法的返回值
public int add(int num1, int num2) {
    int sum = num1 + num2;
    return sum;
}
```

#### 4.2.3 方法的重载

##### 4.2.3.1 重载

**重载（Overload）：**指**一个类中**可以有多个方法具有**相同的方法名**，但这些方法的**参数类型不同、个数不同、顺序不同**。

**注意：**方法返回值和[访问修饰符](https://so.csdn.net/so/search?q=%E8%AE%BF%E9%97%AE%E4%BF%AE%E9%A5%B0%E7%AC%A6&spm=1001.2101.3001.7020 "访问修饰符")可以不同。

示例： 

```
public class hello {
    public static void main(String args[]) {
        f();
 
    }
 
    public static void f() {
 
        System.out.println("3f");
    }
 
    public static void f(int a) {        //重载，注意返回值同，参数不同
 
        System.out.println(a);
    }
//    下面两个注释的方法就不是重载，会报错
//    public static int f(){    //返回值不同
//        System.out.println("hello");
//    }
//    void f(){    //修饰符不同
//        return 666;
//    }
}
```

**示例：求和的数学类：**

```
public class MathOperations {
 
    // 求和方法，接受两个整数参数
    public int sum(int num1, int num2) {
        return num1 + num2;
    }
 
    // 重载的求和方法，接受三个整数参数
    public int sum(int num1, int num2, int num3) {
        return num1 + num2 + num3;
    }
 
    // 重载的求和方法，接受两个双精度浮点数参数
    public double sum(double num1, double num2) {
        return num1 + num2;
    }
 
    public static void main(String[] args) {
        // 创建 MathOperations 对象
        MathOperations math = new MathOperations();
 
        // 使用不同的方法进行求和
        int result1 = math.sum(5, 10);
        int result2 = math.sum(3, 7, 12);
        double result3 = math.sum(2.5, 3.5);
 
        // 打印结果
        System.out.println("Sum of two integers: " + result1);
        System.out.println("Sum of three integers: " + result2);
        System.out.println("Sum of two doubles: " + result3);
    }
}
```

##### 3.2.3.2 重载和重写的区别

*   **重载：**方法名相同且参数列表

重载要求发生在同一个类中, 多个方法之间方法名相同且参数列表不同。

重载与返回类型和访问修饰符无关。方法名相同返回类型不同会直接报错。

*   **重写：**方法名、参数列表、返回类型与父类相同

重写发生在父类子类或接口实现类中, 若子类方法想要和父类方法构成重写关系, 则它的方法名、参数列表、返回类型必须与父类方法相同。

**重写时：**

*   返回值类可以是原返回值类的子类。例如工厂方法设计模式里，抽象工厂类的 createObject() 方法返回值是抽象产品类，具体工厂类的 createObject() 方法返回值是具体产品类
*   访问权限不能比其父类更为严格
*   抛出异常不能比父类更广泛

**思考：**构造方法能不能重写？

**答案：**构造方法不能重写。

#### 3.2.4 可变参数

Java 5 以后引入了可变参数（Varargs），允许方法接受可变数量的参数。可变参数在方法的参数列表中使用省略号 `...` 表示。

```
// 可变参数的定义
public void printNumbers(int... numbers) {
    for (int num : numbers) {
        System.out.print(num + " ");
    }
}
```

**注意：**

*   可变参数必须是方法的最后一个参数。例如 void (int a,int... b) 正确，而 void (int... a,int b) 会报错。
*   一个方法最多只能有一个可变参数。
*   可变参数在方法内部被当作数组处理。

**示例：求和的数学类：**

```
public class MathOperations {
    // 求和方法（整数数组）
    public static int sum(int[] numbers) {
        int result = 0;
        for (int num : numbers) {
            result += num;
        }
        return result;
    }
 
    // 求和方法（可变参数）
    public static int sum(int... numbers) {
        int result = 0;
        for (int num : numbers) {
            result += num;
        }
        return result;
    }
 
    // 求和方法（双精度浮点数数组）
    public static double sum(double[] numbers) {
        double result = 0;
        for (double num : numbers) {
            result += num;
        }
        return result;
    }
 
    // 求和方法（可变参数）
    public static double sum(double... numbers) {
        double result = 0;
        for (double num : numbers) {
            result += num;
        }
        return result;
    }
 
    public static void main(String[] args) {
        // 示例：整数求和
        int[] intArray = {1, 2, 3, 4, 5};
        int intSum = sum(intArray);
        System.out.println("Integer Sum: " + intSum);
 
        // 示例：可变参数整数求和
        int varArgsIntSum = sum(1, 2, 3, 4, 5);
        System.out.println("VarArgs Integer Sum: " + varArgsIntSum);
 
        // 示例：双精度浮点数求和
        double[] doubleArray = {1.1, 2.2, 3.3, 4.4, 5.5};
        double doubleSum = sum(doubleArray);
        System.out.println("Double Sum: " + doubleSum);
 
        // 示例：可变参数双精度浮点数求和
        double varArgsDoubleSum = sum(1.1, 2.2, 3.3, 4.4, 5.5);
        System.out.println("VarArgs Double Sum: " + varArgsDoubleSum);
    }
}
```

​​

#### 3.2.5 构造方法

构造方法是一种特殊的方法，与类同名，没有返回类型。

每次创建对象时，都会默认执行一次构造方法。

**特点：**

*   与类同名，没有返回类型；
*   构造方法在对象创建时执行，用于设置对象的初始状态。 
*   每个类都可以有一个或多个构造方法，但通常至少有一个默认构造方法（无参数）。
*   **默认构造方法：**果在类中没有明确定义任何构造方法，Java 会自动为该类提供一个默认的无参数构造方法。这个默认构造方法执行时不进行特定的初始化操作。
*   **重载：**和普通方法一样，构造方法也支持重载，即在同一个类中可以定义多个同名但参数列表不同的构造方法。

**简单的构造方法：** 

```
public class Phone {
 
    public Phone() {
//创建对象时会直接运行构造方法，输出hhh
        System.out.println("hhh");
    }
}
```

示例：

```
public class Car {
    private String brand;
    private String model;
    private int year;
 
    // 默认构造方法
    public Car() {
        System.out.println("Default constructor called.");
    }
 
    // 带参数的构造方法
    public Car(String brand, String model, int year) {
        this.brand = brand;
        this.model = model;
        this.year = year;
        System.out.println("Parameterized constructor called.");
    }
 
    // Getter 和 Setter 方法（省略）
 
    public static void main(String[] args) {
        // 使用默认构造方法创建对象
        Car defaultCar = new Car();
 
        // 使用带参数的构造方法创建对象
        Car customCar = new Car("Toyota", "Camry", 2022);
    }
}
```

### 4.3 接口和抽象类

#### **4.3.1 概述**

*   **接口：**对行为的抽象，如吃饭类、睡觉类。
*   **抽象类：**对事物的抽象，如动物类，小狗类。 

#### **4.3.2 接口**

**接口：**对行为的抽象，如吃饭类、睡觉类。 

**接口的特点：**

*   接口没有构造方法。
*   接口中的方法会被隐式的指定为 **public abstract 方法，不能定义静态方法**。
*    接口中的变量会被隐式的指定为 **public static final** 变量，不能定义私有成员。因为是 final 所以也要显式赋初值。
*   接口和接口多继承，接口和类多实现。
*   **接口的实现类：**除非实现接口的类是抽象类，否则该类要定义接口中的所有方法。

```
public interface Phone {
     public static final int price=4;    //修饰符可省略
     public abstract void show();        //修饰符可省略
}
 
 
public class PhoneImpl implements Phone{
    public void show(){        //必须是public，否则报错
        System.out.println("hello");
    }
}
```

#### **4.3.3 抽象类**

**抽象类：**对事物的抽象，如动物类，小狗类。 

抽象类包含抽象方法的类，它不能被实例化，通常用于作为其他类的基类。

**特点：**抽象类可以包含抽象方法、具体方法、字段和构造方法。

**使用场景：**

*   **建模共性行为：** 当多个类具有相同的行为，可以将这些行为提取到一个抽象类中，以便实现代码的重用。
*   **规范子类：** 抽象类可以用于规定子类应该实现的一组方法，强制子类提供这些方法的实现。

**示例：** 

```
public abstract class Animal {
    private String name;
 
    public Animal(String name) {
        this.name = name;
    }
 
    // 抽象方法
    public abstract void makeSound();
 
    // 具体方法
    public void eat() {
        System.out.println(name + " is eating");
    }
}
 
public class Dog extends Animal {
    public Dog(String name) {
        super(name);
    }
 
    // 实现抽象方法
    @Override
    public void makeSound() {
        System.out.println("Woof! Woof!");
    }
}
 
public class Cat extends Animal {
    public Cat(String name) {
        super(name);
    }
 
    // 实现抽象方法
    @Override
    public void makeSound() {
        System.out.println("Meow! Meow!");
    }
}
```

#### **4.3.4** 知识加油站：抽象类和接口的区别

**设计目的：** 

*   **接口**作为系统与外界交互的窗口, 体现了一种**规范**。它只能定义抽象方法及常量，而不允许存在初始化块、构造器、成员变量。
*   **抽象类**作为系统中多个子类的共同父类, 它体现的是一种**模板式设计**，它可以被当作系统实现过程中的中间产品，必须要有更进一步的完善。

**相同点：**

*   **实例化：**接口和抽象类都不能被实例化, 它们都位于继承树的顶端, 用于被其它类实现和继承
*   **抽象方法：**接口和抽象类都可以有抽象方法, 实现接口或继承抽象类的普通子类都必须实现这些抽象方法
*   **静态方法：**在 JDK8 引入接口静态方法后，接口和抽象类都可以有静态方法。

**不同点：**

*   **普通方法：**接口里只能包含抽象方法和默认方法, 不能为普通方法提供方法实现；抽象类则可以包含普通方法。
*   **静态方法：**在 JDK8 引入接口静态方法之前，抽象类可以有静态方法，接口不可以有静态方法。
*   **普通成员变量：**接口里只能定义静态常量（会自动加 static final，final 常量必须显示的指定初始值），不能定义普通成员变量；抽象类里既可以定义普通成员变量, 也可以定义静态常量
*   **构造器：**接口里不包含构造器；抽象类可以包含构造器, 但抽象类的构造器并不是用于创建对象, 而是让其子类调用这些构造器来完成属于抽象类的初始化操作
*   **初始化块：**接口里不能包含初始化块, 抽象类则可以包含初始化块（静态代码块和实例代码块）
*   **单继承多实现：**一个类最多只能有一个父类, 包括抽象类；但一个类可以直接实现多个接口, 通过实现多个接口可以弥补 Java 单继承的不足

### 4.4 面对对象三特性

面向对象的三大基本特征是：封装、继承、多态。 

#### 4.4.1 封装

**封装：**通过 private 修饰符，将类的某些信息隐藏在类的内部，不允许外部程序直接访问。

封装将对象的状态和行为包装在一个类中并对外界隐藏实现的细节，可以通过访问修饰符控制成员的访问权限，让外部程序通过该类提供的方法来实现对内部信息的操作和访问。

**示例：**将价格、年龄等信息封装到小狗类中，外部不能直接访问，只能通过 get 和 set 方法访问。

```
public class Dog{
    int price;//成员变量在堆内存
    private int age;
 
    public void setAge(int age) {
       this.age = age;
    }
 
    public int getAge() {
        return age;
    }
}
```

**优点：**安全性和复用性。

*   **安全性：**通过方法来控制成员变量的操作，提高了代码的安全性
*   **复用性：**把代码用方法进行封装，提高了代码的复用性​，降低了耦合性。

#### 4.4.2 继承

**继承：**继承是指一个类通过从另一个类继承其属性和方法。这使得子类具有其父类的行为和属性，同时可以扩展或修改这些行为和属性以满足特定的需求。 

*   **优点：**提高代码复用性，维护性，实现代码共享。
    
*   **缺点：**高耦合性，有侵入性，父类改变子类也会改变。
    
*   **特点：**子类拥有父类非 private 的属性、方法。可以拥有自己的属性和方法，即子类可以对父类进行扩展。可以用自己的方式实现父类的方法。
    
*   **注意：** Java 不支持多继承，但支持多重继承。
    

单继承是子类只能继承一个父类。相对于实现是一个子类可以实现多个接口。所以 java 是单继承多实现。

*   子类所有构造方法会默认先运行 super();
    

```
public class Animal { 
    public String name;   
    int age=3; 
    public Animal(String name, int age) { 
        //初始化属性值
    } 
    public void show() {  //吃东西方法的具体实现  } 
    public void sleep() { //睡觉方法的具体实现  } 
} 
//子类 
public class Penguin  extends  Animal{ 
    int age=4;
    public Penguin(){
        super();    //不写也会默认运行
}
@Override        //注解重写，检查重写是否正确。例如修饰符（子类重写方法的访问权限要≥父类）、函数名错误。
    public void show(){    //重写父类中show(),如果去掉public会报错。想再用super.show();
        int age=5;
         System.out.println(age);   //5,子类可以访问父类非私有成员变量
        System.out.println(this.age);//4
        System.out.println(super.age);//3
    }
}
```

使用 implements 关键字可以变相的使 java 具有多继承的特性

```
public interface A {
    public void eat();
    public void sleep();
}
 
public interface B {
    public void show();
}
 
public class C implements A,B {
}
```

#### 4.4.3 多态

**多态：**同一行为具有多个不同的表现形式或形态。例如同一个接口，使用不同的实例而执行不同操作。使程序更灵活、易于扩展。

多态最常用的是接口引用指向实现类对象，这样当之后程序需要更新时候，只需要修改 new 后面的实现类即可，左边就不用修改了，从而降低耦合。

简单一句话，多态是：**接口引用指向实现类对象。**

在 Spring 框架中，甚至可以通过配置或注解连实例化的 new 也不用了，从而更高层次的降低耦合。

**示例：**

```
List<String> a=new ArrayList<String>();

```

这样写的话，等号右边换成 _Vector 或 LinkedList_ 时，就可以很少修改代码量，降低耦合。

**向上转型：**父类引用指向子类对象。编译看左边（调用子类特有变量、方法会报错），运行看右边（优先运行子类重写后的方法）。

```
Animal a=new Cat();

```

**向下转型：** 子类引用指向父类对象。编译看右边，运行看右边。

```
    public static void main(String[] args) {
                
      Animal a = new Cat();  // 向上转型  
      a.eat();               // 调用的是 Cat 的 eat
      Cat c = (Cat)a;        // 向下转型  
      c.work();        // 调用的是 Cat 的 work
  } 
```

## 五、常用类

### 5.1 String 类

#### 5.1.1 基本介绍 

**什么是 String？**

String 是一个类，用于存储字符串，内部封装了一系列用于操作字符串的方法，底层是 final 修饰的 char 数组。

JDK9 开始，为了节省内存，进而减少垃圾回收次数，String 底层由 char 数组改成了 byte[]。

![](<assets/1740013648317.png>)

​

**Java 的 String 和 c++ 的 string 区别：**

*   java 中字符串一个汉字长度是 1；
*   c++ 中字符串，一个汉字长度是 2. 

#### 5.1.2 基本用法

##### 5.1.2.1 创建

**创建字符串的两种方式：** 

创建字符串有两种方式，一种是使用字符串直接量，另一种是使用 new + 构造器。采用 new 的方式会多创建出一个对象来，占用了更多的内存 ，所以**建议采用直接量的方式来创建字符串。**

*   **字符串直接量创建：**JVM 会使用常量池来管理这个字符串；
*   **new 创建：**JVM 会先使用常量池来管理字符串直接量（若已有此字符串则直接返回引用，若没有则实例化后再返回引用），再调用 String 类的构造器来创建一个新的 String 对象，新创建的 String 对象会被保存在堆内存中。字符串常量池源码用到了享元设计模式。

```
// String 直接创建
String s1 = "Runoob";       
// String 对象创建       
String s2 = new String("Runoob");   
 // 引用赋值的方法创建字符串
String s3 = s1;                   
```

##### 5.1.2.2 访问

通过索引访问字符：charAt()

```
String str = "Hello, World!";
char firstChar = str.charAt(0);
System.out.println("First Character: " + firstChar);
```

![](<assets/1740013648521.png>)

​

访问子串：substring()

```
        String str = "0123456789ABCDEFG";
// 提取索引 7 到 11 的子串
        String substring = str.substring(7, 12);
        System.out.println("Substring: " + substring);
```

![](<assets/1740013648843.png>)

​

**遍历字符串每个字母：**

*   使用 for 循环遍历字符串：

```
String str = "hello world";
for (int i = 0; i < str.length(); i++) {
    char c = str.charAt(i);
    System.out.println(c);
}
```

*   使用增强 for 循环遍历字符串：

```
String str = "hello world";
for (char c : str.toCharArray()) {
    System.out.println(c);
}
```

*   使用 while 循环遍历字符串：

```
String str = "hello world";
int i = 0;
while (i < str.length()) {
    char c = str.charAt(i);
    System.out.println(c);
    i++;
}
```

*   使用 Iterator 遍历字符串：

```
String str = "hello world";
Iterator<Character> iterator = str.chars().mapToObj(c -> (char) c).iterator();
while (iterator.hasNext()) {
    char c = iterator.next();
    System.out.println(c);
}
```

**结果：**  

![](<assets/1740013649119.png>)

##### 5.1.2.3 **连接**

字符串可以通过 “+” 号拼接，拼接过程中可以将数字型转为字符串。

示例： 

```
        String s1,s2;
//拼接字符串、数字、字符
        s1="我的名字是 ".concat("s1");
        s2="s2:";
        s2+="abc";
//字符串拼接数字，数字会转成字符串
        s2+=123;
        s2+='c';
        System.out.println(s1);
        System.out.println(s2);
```

结果： 

![](<assets/1740013649330.png>)

​

**字符串连接字符数组：** 

```
//连接字符串
    String s= "Hello";
    s= s+ ", World!";
 
//连接字符数组
        char[] greeting = {'H', 'e', 'l', 'l', 'o'};
        char[] suffix = {',', ' ', 'W', 'o', 'r', 'l', 'd', '!'};
        char[] result = new char[greeting.length + suffix.length];
        System.arraycopy(greeting, 0, result, 0, greeting.length);
        System.arraycopy(suffix, 0, result, greeting.length, suffix.length);
        System.out.println(result);
```

![](<assets/1740013649538.png>)

​

##### 5.1.2.4 **子串**

在 Java 中，可以使用 substring() 方法获取字符串的子串：

*   **substring(int beginIndex)：** 返回从指定索引开始到字符串末尾的子串。
    
    ```
    String originalString = "Hello, World!";
    String substring = originalString.substring(7);  // 从索引 7 开始到字符串末尾的子串
    System.out.println(substring);  // 输出结果为 "World!"
    ```
    
*   **substring(int beginIndex, int endIndex)：** 返回从指定索引开始到指定索引结束的子串（不包括 endIndex 处的字符）。
    
    ```
    String originalString = "Hello, World!";
    String substring = originalString.substring(7, 12);  // 从索引 7 开始到索引 12 结束的子串
    System.out.println(substring);  // 输出结果为 "World"
    ```
    

**注意：**

因为 String 是不可变的，对字符串的任何操作都会返回一个新的字符串。所以 substring 方法其实是创建了一个字符子串，而不是修改了原始字符串。

##### 5.1.2.5 删除

Java 中字符串没有直接根据索引删除的方法，所以删除字符串中指定索引的字母时，可以通过子串 substring() 删除。

**示例：**删除字符串中下标是 6 的字母

```
String str = "Hello, World!";
String newStr = str.substring(0, 5) + str.substring(7);
System.out.println("Modified String: " + newStr);
```

结果：

![](<assets/1740013649725.png>)

##### 5.1.2.6 替换

在 Java 中， 可以使用 replaceAll() 方法替换字符中的字符：

1.  **replaceAll(String regex, String replacement)：** 使用给定的替换字符串替换输入字符串中所有匹配正则表达式的部分。
    
    ```
    String originalString = "Hello, World!";
    String replacedString = originalString.replaceAll("o", "0");
    System.out.println(replacedString);  // 输出结果为 "Hell0, W0rld!"
    ```
    
    在上面的例子中，所有的字母 "o" 都被替换为数字 "0"。
    
2.  **replaceAll(String regex, Function<MatchResult, String> replacer)：** 使用给定的 Function 替换输入字符串中所有匹配正则表达式的部分。该方法允许更加灵活的替换逻辑，并且可以基于匹配的结果进行自定义的替换。
    
    ```
            String originalString = "Hello, World!";
            String replacedString = originalString.replaceAll("o", match -> match.group().toUpperCase());
            System.out.println(replacedString);  // 输出结果为 "HellO, WOrld!"
    ```
    
    在上面的例子中，将匹配到的字母 "o" 替换为大写的 "O"。
    

**练习：**将字符串中所有的 "Java" 都被替换为 "Python"： 

```
        String original = "Hello, Java! Java is fun. Java is powerful.";
 
        // 将所有的 "Java" 替换为 "Python"
        String modifiedString = original.replaceAll("Java", "Python");
 
        System.out.println("Original String: " + original);
        System.out.println("Modified String: " + modifiedString);
```

![](<assets/1740013649915.png>)

​

##### 5.1.2.7 **获取长度**

可以通过 length() 方法获取字符串的长度。 

**示例：**获取字符串的长度

```
public class Test {
    public static void main(String[] args) {
        String text = "Java Programming";
        // 返回字符串的长度
        int length = text.length();
        System.out.println(length);
 
    }
}
```

结果：

![](<assets/1740013650138.png>)

##### 5.1.2.8 **比较**

字符串内容的互相比较一般用 equals() 方法。

Java 中所有类都直接或间接继承了 Object 类，在 Object 类中，equals() 方法是 Java 中用于比较两个对象的引用是否相等（即内存地址是否相同）。

在 String 类中，equals() 方法被重写，用于比较两个字符串的内容是否相等。

**示例：**比较两个字符串内容是否相等

```
public class Test {
    public static void main(String[] args) {
        String str1 = "Hello";
        String str2 = "Hello";
        // 比较字符串内容是否相等
        boolean isEqual = str1.equals(str2);
        System.out.println(isEqual);
 
    }
}
```

结果：

![](<assets/1740013650389.png>)

**注意：**尽量不要用 == 进行比较，== 比较的是地址。

String 底层是常量池，使用享元设计模式，新创建的字符串会维护在常量池，下次再创建这个字符串，就直接从常量池取。

**验证：**

```
//比较地址
//只要new，就在堆内存开辟空间。直接赋值字符串在常量池里。
        //常量池里无“hello”对象，创建“hello”对象，str1指向常量池“hello”对象。
        String str1 = "hello";        
//先检查字符串常量池中有没有"hello"，如果字符串常量池中没有，则创建一个，然后 str1 指向字符串常量池中的对象，如果有，则直接将 str1 指向"hello"；
        //常量池里有“hello”对象，str2直接指向常量池“hello”对象。
        String str2 = "hello";
        //堆中new创建了一个对象。假如“hello”在常量池中不存在，Jvm还会常量池中创建这个对象“hello”。
        String str3 = new String("hello");   
        String str4 = new String("hello");
        //下面输出true，因为str1和str2指向的是常量池中的同一个内存地址
        System.out.println(str1 == str2);
        //下面输出false，str1常量池旧地址，str3是new出的新对象，指向一个全新的地址
        System.out.println(str1 == str3);
        //下面输出false，因为它们引用不同
        System.out.println(str4 == str3);
 
//比较内容
        //下面输出true，因为String类的equals方法重写过，比较的是字符串值
        System.out.println(str4.equals(str3));
```

结果：

![](<assets/1740013650632.png>)

​

##### 5.1.2.8 知识加油站：== 与 equals() 的区别

*   **==** 比较基本数据类型时，比较的是两个数值是否相等； 比较引用类型是，比较的是对象的内存地址是否相等。
*   **equals()** 没有重写时，Object 默认以 == 来实现，即比较两个对象的内存地址是否相等； 重写以后，按照重写的逻辑进行比较。

**示例：String 源码中重写的 equals() 方法：**

*   如果地址一样，则一定相等；
*   如果对比的元素不是 String 类型，则一定不相等；
*   遍历 String 底层的数组，逐个对比字符是否相等；

![](<assets/1740013650799.png>)

##### 5.1.2.9 分割

split() 方法是 Java 中 String 类的一个方法，用于将字符串分割成字符串数组，根据给定的正则表达式作为分隔符。该方法有两个重载的版本：

1.  **split(String regex)：** 使用给定的正则表达式作为分隔符，将字符串分割为字符串数组。
    
    ```
    String sentence = "Hello, World! How are you?";
    String[] words = sentence.split(" ");  // 使用空格作为分隔符
    for (String word : words) {
        System.out.println(word);
    }
    ```
    
    上述代码会输出每个单词：
    
    ```
    Hello,
    World!
    How
    are
    you?
    ```
    
2.  **split(String regex, int limit)：** 使用给定的正则表达式作为分隔符，将字符串分割为字符串数组，限制分割的次数。
    
    ```
    String sentence = "apple,orange,banana,grape";
    String[] fruits = sentence.split(",", 2);  // 使用逗号作为分隔符，限制分割次数为 2
    for (String fruit : fruits) {
        System.out.println(fruit);
    }
    ```
    
    上述代码会输出：
    
    ```
    apple
    orange,banana,grape
    ```
    

需要注意的是，split() 方法的参数是正则表达式，因此在传入正则表达式时，可能需要注意转义字符的使用。例如，如果要以点号 . 作为分隔符，由于点号在正则表达式中有特殊含义，需要使用双反斜杠 \ 进行转义。

```
String sentence = "one.two.three";
String[] parts = sentence.split("\\.");  // 使用点号作为分隔符，需要转义
for (String part : parts) {
    System.out.println(part);
}
```

上述代码会输出：

```
one
two
three
```

总的来说，split() 方法是在字符串处理中常用的方法，可以根据需要灵活地将字符串分割成多个部分。

```
        //根据指定字符串分割
        String s="1,2,3";
        String[] arr=s.split(",");
        for (String s1 : arr) {
            System.out.println(s1);
        }
```

结果：

![](<assets/1740013651050.png>)

​

##### 5.1.2.10 **转换大小写**

```
String text = "Java Programming";
String lowerCase = text.toLowerCase();
String upperCase = text.toUpperCase();
```

##### 5.1.2.11 字符串和数字的互相转换

**1. 字符串转整数：**

使用 `Integer.parseInt()` 方法：

```
String strNumber = "123";
int intValue = Integer.parseInt(strNumber);
System.out.println("Parsed Integer: " + intValue);
```

使用 `Integer.valueOf()` 方法：

```
String strNumber = "123";
Integer integerValue = Integer.valueOf(strNumber);
System.out.println("Integer Value: " + integerValue);
```

**2. 整数转字符串：**

使用 `String.valueOf()` 方法：

```
int intValue = 123;
String strNumber = String.valueOf(intValue);
System.out.println("String Value: " + strNumber);
```

使用 `Integer.toString()` 方法：

```
int intValue = 123;
String strNumber = Integer.toString(intValue);
System.out.println("String Value: " + strNumber);
```

##### 5.1.2.12 **数组和字符串的互相转换**

**数组转字符串：** 

```
Arrays.toString(数组名)

```

示例： 

```
int[] numbers = {1, 2, 3, 4, 5};
//输出：[1, 2, 3, 4, 5]
System.out.println(Arrays.toString(numbers));
//直接输出数组会输出地址：[I@5b444398
System.out.println(a);    
```

**字符串转数组：**

*   方法名：`stringToIntArray(String str)、stringToCharArray(String str)等`
    

示例代码：

```
// 字符串转整型数组
int[] intArray = stringToIntArray("1 2 3 4 5");
// 字符串转字符数组 
char[] charArray = stringToCharArray("Hello");
```

5.1.2.13 格式化数字为字符串

```
        double number = 123.456789;
        // 格式化浮点数为字符串，保留两位小数
        String formattedString = String.format("%.2f", number);
        System.out.println(formattedString);
```

结果： 

![](<assets/1740013651321.png>)

​

#### 5.1.3 知识加油站

##### 5.1.3.1 **字符串和字符数组的区别**

*   **可变性：**字符串是不可变的。一旦创建，字符串的内容就不能被修改。任何对字符串的修改底层都会创建一个新的字符串对象。而字符数组是可变的。你可以直接修改字符数组的元素。
*   **方法数量：**String 类提供了许多用于处理字符串的方法，如拼接、比较、截取、转换大小写等。而字符数组只有 toString()、equals() 等简单的数组通用方法。
*   **性质：**String 是类（底层是字节数组），字符数组是数组。
    *   **创建方式：**String str = "Hello";char[] charArray = {'H', 'e', 'l', 'l', 'o'};
    *   **连接：**字符串通过 “+” 连接，字符数组通过 System.arraycopy()连接。
*   **使用场景：**String 不可变，所以适用于不需要频繁修改字符串内容的情况。字符数组适用于需要频繁修改字符内容的情况

##### 5.1.3.2 字符串常量池

**常量池：**Java 虚拟机有一个常量池机制，它会直接把字符串常量放入常量池中，从而实现复用。

**Java 字符串存储原理：** 

1.  创建字符串常量时，JVM 会通过 equals() 检查字符串常量池中是否存在这个字符串；
2.  若字符串常量池中存在该字符串，则直接返回引用实例；
3.  若不存在，先实例化该字符串，并且将该字符串的引用放入字符串常量池中，以便于下次使用时，直接取用，达到缓存快速使用的效果。

##### 5.1.3.3 **String 不可被继承、不可变的原因**

**String 为什么不可被继承？**

因为 String 类底层的数组是由 final 修饰的，所以 String 类不可被继承。

**String 字符串为什么不可被变？**

因为 String 底层 char 类型的 value 数组是 private final 修饰的。

*   **final 修饰：**导致 value 不能指向新数组（但无法保证 value 这个引用变量指向的真实数组不可变）；
*   **private 修饰，且没对外暴露任何修改 value 的方法：**导致 value 这个引用变量指向的底层数组不可变；

**不可变的优点：**因为压根不会被改，所以线程安全、节省空间、效率高。 

##### 5.1.3.4 new String("abc") 创建的字符串对象数量

**答案：**一个或两个。

**原因：**

首先，new string 这边由于 new 关键字，所以这边肯定会在堆中**直接创建一个**字符串对象。

其次，**如果**字符串常量池中不存在 "abc"（通过 equals 比较）这个字符串的引用，则会在字符串常量池中**创建一个**字符串对象。如果已存在则不创建。注意这边说的在字符串常量池创建对象，最终对象还是在堆中创建，字符串常量池只放引用。

### 5.2 StringBuilder 类

#### 5.2.1 基本介绍

String 拼接字符串后原字符串还存在于内存中，浪费内存。

StringBuffer 类的对象能够被多次的修改，并且不产生新的未使用对象，所以涉及到字符串拼接，优先用 StringBuffer 。

```
//构造
        StringBuilder sb= new StringBuilder("abc");
 
//String转StringBuilder
        sb.append("d").append("e").append("f");
 
//StringBuilder转String
        String s=sb.toString();
 
//反转
        sb.reverse();
 
```

#### 5.2.2 知识加油站：String、StringBuffer、Stringbuilder 的区别

**String:** 不可变字符序列，效率低，但是复用率高、线程安全。

不可变是指 String 对象创建之后, 直到这个对象销毁为止, 对象中的字符序列都不能被改变。

复用率高是指 String 类型对象创建出来后归常量池管，可以随时从常量池调用同一个 String 对象。StringBuffer 和 StringBuider 在创建对象后一般要转化成 String 对象才调用。

**StringBuffer 和 StringBuilder**StringBuffer 和 Stringbuilder 都是字符序列可变的字符串，方法也一样，有共同的父类 AbstractStringBuilder。 

*   **StringBuffer:** 可变字符序列、效率较高 (增删)、线程安全
*   **StringBuilder:** 可变字符序列、效率最高、线程不安全

### 5.3 Scanner 类

Scanner 类用于从各种输入源（例如控制台）中获取基本数据类型和字符串，如 int、double、String 等。常用于从控制台、文件等读取数据。

**构造方法：**

*   **Scanner(InputStream source):** 构造一个新的 Scanner，生成的扫描器从指定的输入流读取数据。一般用 System.in，即标准输入流，用于读取用户在控制台输入的数据。
*   Scanner(File source): 构造一个新的 Scanner，生成的扫描器从指定的文件读取数据。
*   Scanner(String source): 构造一个新的 Scanner，生成的扫描器从指定的字符串读取数据。

**常用方法：**

*   **nextInt()、nextDouble()、next()：**获取输入的整数、浮点数、字符串（不包括空格）等。
*   **nextLine()：**获取一行输入（包括空格）。
*   **hasNextInt()、hasNextDouble()、hasNext()：** 判断下一个输入是否为整数、浮点数、字符串。
*   **useDelimiter(String pattern)：** 设置分隔符模式，用于指定不同类型数据之间的分隔符，默认为空白字符。
*   **close()：**关闭扫描器。

**示例：**

从控制台输入整数、浮点数、字符串，并在控制台打印。

```
import java.util.Scanner;
 
public class Test {
    public static void main(String[] args) {
        // 创建 Scanner 对象，关联 System.in（标准输入流）
        Scanner scanner = new Scanner(System.in);
 
        // 从控制台读取整数
        System.out.print("Enter an integer: ");
        int intValue = scanner.nextInt();
        System.out.println("You entered: " + intValue);
 
        // 从控制台读取浮点数
        System.out.print("Enter a double: ");
        double doubleValue = scanner.nextDouble();
        System.out.println("You entered: " + doubleValue);
 
        // 从控制台读取字符串
        System.out.print("Enter a string: ");
        String stringValue = scanner.next();
        System.out.println("You entered: " + stringValue);
 
        // 关闭 Scanner 对象
        scanner.close();
    }
}
```

结果：

![](<assets/1740013651605.png>)

### 5.4 Object 类

#### 5.4.1 基本介绍 

在 Java 中，Object 类是所有类的根类，所有其他类都直接或间接地继承自 Object 类。

Object 类定义了一些基本的方法，这些方法可以被所有对象继承和使用：

1.  **toString() 方法：** 返回对象的字符串表示。再不重写的情况下，toString() 返回的是对象的类名和散列码的十六进制表示。
    
    ```
    public class MyClass {
        private int value;
     
        public MyClass(int value) {
            this.value = value;
        }
    //自定义转字符串逻辑
        @Override
        public String toString() {
            return "MyClass{" +
                   "value=" + value +
                   '}';
        }
    }
     
    // 使用 toString() 方法
    MyClass obj = new MyClass(42);
    // toString将根据自己重新的逻辑返回字符串，而不是返回对象的类名和散列码的十六进制表示
    System.out.println(obj.toString());  // 输出结果为 "MyClass{value=42}"
    ```
    
2.  **equals(Object obj) 方法：** 比较对象是否相等。默认情况下，equals() 方法比较的是对象的引用（即内存地址），重写后可以自定义比较逻辑。
    
    ```
    public class Person {
        private String name;
        private int age;
     
        // ... 其他代码 ...
    //自定义比较逻辑
        @Override
        public boolean equals(Object obj) {
            if (this == obj) return true;
            if (obj == null || getClass() != obj.getClass()) return false;
            Person person = (Person) obj;
            return age == person.age && Objects.equals(name, person.name);
        }
    }
    ```
    
3.  **hashCode() 方法：** 返回对象的散列码。hashCode() 方法的默认实现返回对象的内存地址的散列码。通常情况下，如果 equals() 方法被覆盖，那么也应该同时覆盖 hashCode() 方法。
    
    ```
    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
    ```
    
4.  **getClass() 方法：** 返回对象的类。该方法基于反射，返回类的 Class 对象，该对象包含有关对象的类的信息。
    
    ```
    MyClass obj = new MyClass(42);
    Class<?> clazz = obj.getClass();
    System.out.println(clazz.getName());  // 输出结果为 "MyClass"
    ```
    
5.  **clone() 方法：** 创建并返回对象的拷贝。要实现 clone() 方法，类必须实现 Cloneable 接口。
    
    ```
    public class CloneableClass implements Cloneable {
        private int value;
     
        public CloneableClass(int value) {
            this.value = value;
        }
     
        @Override
        protected Object clone() throws CloneNotSupportedException {
            return super.clone();
        }
    }
    ```
    
6.  **finalize() 方法：** 在对象被垃圾收集器回收之前调用。通常不推荐使用 finalize() 方法，因为它的行为是不确定的，并且可能导致一些问题。
    
    ```
    @Override
    protected void finalize() throws Throwable {
        // 执行清理工作
        // ...
        super.finalize();
    }
    ```
    

#### 5.4.2 知识加油站：JVM 垃圾回收的可达性分析算法

JVM 垃圾回收的可达性分析算法有用到 Object 类的 finalize() 方法。

**可达性分析算法：**

以根对象集合 (GC Roots) 的每个跟对象为起始点，根据引用关系向下搜索，将所有与 GC Roots 直接或间接有引用关系的对象在对象头的 Mark Word 里标记为可达对象，即不需要回收的有引用关系对象。搜索过程所走过的路径称为“引用链” 。

**GC Roots：**即 GC 根节点集合，是**一组**必须活跃的引用。可作为 GC Roots 的对象：

*   **栈引用的对象：**Java 方法栈、本地方法栈中的参数引用、局部变量引用、临时变量引用等。临时变量是方法里的中间操作结果。
*   **方法区中常量、静态变量引用的对象；**
*   **所有被同步锁持有的对象；**
*   **所有线程对象；**
*   所有跨代引用对象；
*   JVM 内部的引用：如基本数据类型对应的 Class 对象，常驻的异常对象，以及应用程序类类加载器； 

**非可达对象****被回收需要两次标记：**

1.  **第一次标记后筛选非可达对象：**第一次被标记后，会进行一次筛选，筛选的条件是此对象是否有必要执行 finalize() 方法，也就是是否有机会自救。假如对象没有覆盖或者已被 JVM 调用过 finalize() 方法，也就是说不想自救或已自救过，那么此对象需要被回收；假如对象覆盖并没被 JVM 调用过 finalize() 方法，该对象将会被放置在一个名为 F-Queue 的队列之中，并在稍后由一条由虚拟机自动建立的、低调度优先级的 Finalizer 线程去执行它们的 finalize() 方法。
2.  **第二次标记 F-Queue** **里的未自救对象：**稍后，收集器将对 F-Queue 中的对象进行第二次小规模的标记。如果对象要在 finalize()中成功拯救自己——只要重新与引用链上的任何一个对象建立关联即可，譬如把自己（this）赋值给某个引用类型的类变量或者对象的成员变量，那在第二次标记时它将被移出 “即将回收” 的 F-Queue。如果对象这时候还没有逃脱，那基本上它就真的要被回收了。

**finalize() 方法：** 

finalize() 方法是对象逃脱死亡命运的最后一次机会，需要注意的是，任何一个对象的 finalize() 方法都只会被系统自动调用一次，如果对象面临下一次回收，它的 finalize() 方法不会被再次执行。

另外，finalize() 方法的运行代价高昂，不确定性大，无法保证各个对象的调用顺序，如今已被官方明确声明为不推荐使用的语法。

#### 5.4.3 知识加油站：hashCode() 和 equals() 的区别

**用途：**

*   hashCode() 方法的主要用途是获取哈希码；
*   equals() 主要用来比较两个对象是否相等。

**为什么重写 equals() 就要重写 hashcode()？** 

因为二者之间有两个约定，相等对象的哈希码也要相等。

所以 equals() 方法重写时, 通常也要将 hashCode() 进行重写, 使得这两个方法始终满足相关的约定。 例如 HashSet 排序机制底层就是通过计算哈希码进行排序的，如果只重写 equals() 将达不到根据哈希码排序的效果。

如果两个对象相等, 它们必须有相同的哈希码；但如果两个对象的哈希码相同, 他们却不一定相等。

**哈希碰撞：**由于哈希码是一个有限的整数，因此有可能会出现不同的对象计算出相同的哈希码的情况。 例如某类里有两个 int 型成员变量，重写后 equals() 比较两个变量是否相等，hashcode() 是两个变量的和，A 对象变量是 2 和 3，B 对象变量是 1 和 4，加起来都是 5，它们哈希码相等而不 equal。

### 5.5 System 类

System 类是 Java 标准库中的一个工具类，提供了与系统相关的一些操作。

它包含一些静态方法和字段，用于访问系统属性、标准输入输出、垃圾回收等。

**常用方法和字段：**

 PrintStream、PrintStream 等 IO 流详细看后文的 I/O 流。

1.  **out 字段：** 是 PrintStream 类的一个实例，用于标准输出。可以使用 System.out.println() 来向标准输出打印信息。
    
    ```
    System.out.println("Hello, World!");  // 将 "Hello, World!" 输出到标准输出
    
    
    ```
    
2.  **err 字段：** 是 PrintStream 类的一个实例，用于标准错误输出。可以使用 System.err.println() 来向标准错误输出打印错误信息。
    
    ```
    System.err.println("This is an error message.");  // 将错误信息输出到标准错误输出
    
    
    ```
    
3.  **in 字段：** 是 InputStream 类的一个实例，用于标准输入。可以使用 Scanner 或 BufferedReader 等类从标准输入读取用户输入的信息。
    
    ```
    Scanner scanner = new Scanner(System.in);
    System.out.print("Enter your name: ");
    String name = scanner.nextLine();
    ```
    
4.  **currentTimeMillis() 方法：** 返回当前时间与 1970 年 1 月 1 日午夜之间的毫秒数。通常用于测量代码的执行时间。
    
    ```
    long startTime = System.currentTimeMillis();
    // 执行一些操作
    long endTime = System.currentTimeMillis();
    long elapsedTime = endTime - startTime;
    ```
    
5.  **arraycopy(Object src, int srcPos, Object dest, int destPos, int length) 方法：** 用于复制数组。
    
    ```
    int[] sourceArray = {1, 2, 3, 4, 5};
    int[] destinationArray = new int[5];
     
    System.arraycopy(sourceArray, 0, destinationArray, 0, sourceArray.length);
     
    // 现在 destinationArray 中包含了 sourceArray 的内容
    ```
    
6.  **getProperty(String key) 方法：** 获取系统属性。可以用于获取一些系统相关的信息，比如操作系统类型、Java 版本等。
    
    ```
    String osName = System.getProperty("os.name");
    System.out.println("Operating System: " + osName);
    ```
    
7.  **exit(int status) 方法：** 终止当前运行的 Java 虚拟机。status 参数是一个整数，通常用于指示程序的退出状态。非零值通常表示发生了错误。
    
    ```
    System.exit(0);  // 正常退出程序
    System.exit(1);  // 异常退出程序
    ```
    
8.  **gc() 方法：** 强制调用垃圾回收器。虽然 Java 具有自动垃圾回收机制，但可以使用 System.gc() 来显式地触发垃圾回收。
    
    ```
    System.gc();  // 强制调用垃圾回收器
    
    
    ```
    

**练习：**获取 for 循环十万次的运行时长。

```
public class Test {
    public static void main(String[] args) {
        long start=System.currentTimeMillis();
        for(int i=0;i<100000;i++){}
        long end=System.currentTimeMillis();
        System.out.println("for循环十万次的运行时长:"+(end-start)+"毫秒");
    }
}
```

结果：

![](<assets/1740013651818.png>)

### 5.6 Integer 类

#### 5.6.1 基本介绍 

Integer 类是 Java 中用于表示整数的包装类，它提供了许多方法来对整数进行操作和转换。

相比于基本数据类型，包装类的优势：可以有更多的方法操作改数据。

```
//装箱：基本数据类型转为包装类
        Integer a=Integer.valueOf(32);
        Integer a2=32;
 
//拆箱：包装类转为基本数据类型
        int b=a.intValue();
 
//数字转String
String s=String.valueOf(12);
 
//String转数字
int c=Integer.parseInt(s);
 
//String转Integer转数字
int c=Integer.valueOf(s).intValue();
```

#### 5.6.2 知识加油站：包装类的自动拆装箱与自动装箱

**包装类：**包装类的主要作用是用于便于操作基本数据类型，将基本数据类型转换为对象，让基本数据类型拥有对象的特征，例如封装方法、泛型（基本数据类型不能作为泛型参数）、反射。 

自动装箱是指把一个基本类型的数据直接赋值给对应的包装类型；

自动拆箱是指把一个包装类型的对象直接赋值给对应的基本类型；

**向上转型：**布尔型外的基本数据类型在互相比较或运算时会向上转型：byte,short,char → int → long → float → double。原理是将低字节数的数据类型转换为高字节数的数据类型，可以保证数据精度不丢失。c 语言转型顺序：char→short→int→long→float→double 

#### 5.6.3 知识加油站：什么情况下用包装类？什么情况下用基本数据类型？

包装类适用场景： 

*   **实体类属性必须使用包装类：**《阿里规约》规定，所有的 POJO 类属性必须使用包装数据类型，而不是基本数据类型。因为数据库的查询结果可能是 null，因为自动拆箱，用基本数据类型接收有 NPE 风险（NullPointerException 空指针异常）。
*   **RPC 方法的返回值和参数必须使用包装类：**《阿里规约》规定，RPC 方法的返回值和参数必须使用包装数据类型。因为相比基本数据类型，包装类的 null 值能展示额外的信息。例如远程调用获取商品价格，如果用包装类，null 表示获取失败，0 表示价格是 0；而如果用基本数据类型，即使获取失败返回值也是 0，你就没法知道是价格 0 还是获取失败了。

基本数据类型适用场景： 

*   **局部变量尽量使用基本数据类型：**《阿里规约》建议，所有的局部变量使用基本数据类型。因为包装类对象是引用类型，JVM 中，基本数据类型存储在方法栈中，引用数据类型存储堆内存实际对象的地址值，如果局部变量定义为引用数据类型还得根据这个地址值去找值，性能差（每次要 new），而且耗费空间，毕竟它的作用域只是方法内。

#### 5.6.4 知识加油站：**包装类和基本数据类型直接如何互相比较？（浮点数等号比较精度丢失问题）**

**包装类和基本数据类型：**直接通过 == 比较。

```
        Integer integer = new Integer(3000);
        double b=3000;
//true，相等
        System.out.println(b==integer);
```

**整型：** 

*   **相同整型包装类必须通过 equals() 比较**。虽然两个通过自动装箱创建的、数值在缓存范围的相同整型包装类可以通过 == 比较（例如 Integer a=1,b=1，则 a==b），但《阿里规约》规定整型包装类必须通过 equals() 比较。包装类和各类型基本类型可以通过 == 比较。
*   **不同整型包装类必须转成相同类再通过 equals() 比较。**
*   **整型基本数据类型用 == 比较。**

**浮点型：**

*   浮点包装类先都转为 BigDecimal，再进行运算、比较。
    
*   浮点基本类型直接比较，要声明误差，两浮点数差值在此范围内，认为是相等的。 
    

**浮点数正确比较方法：**

**基本数据类型：** 

```
float a = 1.0f - 0.9f;
float b = 0.9f - 0.8f;
float diff = 1e-6f;
 
if (Math.abs(a - b) < diff) {
    System.out.println("a、b相等");
}
```

 **包装类：**

```
//为了保证高精度
//BigDecimal推荐入参为 String 的构造方法，或使用 BigDecimal 的 valueOf 方法
//valueOf方法内部其实执行了Double 的 toString，而 Double 的 toString 按 double 的实际能表达的精度对尾数进行了截断
        BigDecimal a = new BigDecimal("1.0");
        BigDecimal b = BigDecimal.valueOf(0.9);
        BigDecimal c = new BigDecimal("0.8");
 
        BigDecimal x = a.subtract(b);
        BigDecimal y = b.subtract(c);
 
        if (x.equals(y)) {
            System.out.println("a、b相等");
        }
```

#### 5.6.5 知识加油站：Integer a1=127;Integer a2=127;a1==a2 原因

**享元模式：** 

Integer 内部有享元模式设计，【-128,127】范围内的数字会被缓存，使用自动装箱方式赋值时，Java 默认通过 valueOf() 方法对 127 这个数字进行装箱操作，触发缓存机制，使 a1 和 a2 指向同一个内存地址。

*   Byte、Short、Integer、Long 缓存区间【-128,127】。
*   Character 包装类型缓存区间 [0,127]。
*   浮点型和布尔型没用享元模式，没有缓存区间。

```
Integer a = 127;  
Integer b = 127;  
Integer c = 128;  
Integer d = 128;  
System.out.println(a==b); //true  
System.out.println(c==d); //false  
```

### 5.7 Arrays 类

`Arrays` 类包含了一系列用于操作数组的静态方法。可以对数组排序、搜索、比较、转换、填充等。以下是 `Arrays` 类的一些常用方法：

#### 5.6.**1 排序**

*   **`sort(T[] a)`：** 对数组进行升序排序。
    
    ```
    int[] numbers = {5, 2, 8, 1, 6};
    Arrays.sort(numbers);
    ```
    
*   **`sort(T[] a, Comparator<? super T> c)`：** 使用指定的比较器对数组进行排序。
    
    ```
    String[] names = {"John", "Alice", "Bob", "Eve"};
    Arrays.sort(names, Comparator.reverseOrder());
    ```
    

#### 5.6.**2 查找和判断**

*   **`binarySearch(T[] a, T key)`：** 在已排序的数组中使用二分查找算法查找指定元素的索引。
    
    ```
    int[] numbers = {1, 2, 3, 4, 5, 6};
    int index = Arrays.binarySearch(numbers, 3);
    ```
    
*   **`equals(T[] a, T[] a2)`：** 比较两个数组是否相等。
    
    ```
    int[] arr1 = {1, 2, 3};
    int[] arr2 = {1, 2, 3};
    boolean isEqual = Arrays.equals(arr1, arr2);
    ```
    

#### 5.6.**3 批量填充**

*   **`fill(T[] a, T val)`：** 使用指定的值填充数组的所有元素。
    
    ```
    int[] numbers = new int[5];
    Arrays.fill(numbers, 42);
    ```
    

#### 5.6.**4 转换字符串、链表**

*   **`toString(T[] a)`：** 返回包含数组元素的字符串。
    
    ```
    int[] numbers = {1, 2, 3, 4, 5};
    String arrayString = Arrays.toString(numbers);
    ```
    
*   **`asList(T... a)`：** 将数组转换为固定大小的列表。
    
    ```
    String[] names = {"John", "Alice", "Bob"};
    List<String> nameList = Arrays.asList(names);
    ```
    

#### 5.6.**5 拷贝**

*   **`copyOf(T[] original, int newLength)`：** 复制指定长度的数组。
    
    ```
    int[] numbers = {1, 2, 3, 4, 5};
    int[] copiedNumbers = Arrays.copyOf(numbers, 3);
    ```
    

​​

### 5.8 Date 类

Date 类是 Java 中用于表示日期和时间的类，位于 java.util 包中。在 Java 8 及之前的版本中，Date 类是主要的日期时间处理类，但在 Java 8 引入了 java.time 包，推荐使用新的日期时间 API（java.time 包中的 LocalDate、LocalTime、LocalDateTime 等类）。尽管如此，我们仍然可以了解 Date 类的基本使用。

#### 5.8.1 常用方法

1.  **`getTime()`：** 返回自 1970 年 1 月 1 日 00:00:00 GMT 以来的毫秒数。
2.  **`toString()`：** 返回日期对象的字符串表示。
3.  after(Date when)： 判断某个日期是否在指定日期之后
4.  before(Date when)：判断某个日期是否在指定日期之前
5.  a.compareTo(b)：对两个日期进行比较 如果 a 时间在 b 之后，则返回 1 如果 a 时间在 b 之前，则返回 - 1 如果 a==b，则返回 0

#### 5.8.2 代码示例

```
import java.util.Date;
 
public class Test {
    public static void main(String[] args) {
        // 创建当前日期和时间的 Date 对象
        Date currentDate = new Date();
 
        // 输出当前日期和时间
        System.out.println("Current Date and Time: " + currentDate);
 
        // 获取毫秒表示的时间戳
        long timestamp = currentDate.getTime();
        System.out.println("Timestamp: " + timestamp);
 
        // 通过时间戳创建 Date 对象
        Date newDate = new Date(timestamp);
        System.out.println("New Date: " + newDate);
    }
}
```

输出结果：

![](<assets/1740013652071.png>)

#### 5.8.3 知识加油站：Date 类的线程安全问题

**Date 类的缺点：**

*   **线程不安全：**Date 类可变，在多线程环境中使用时需要额外的同步措施。**解决方案：**
    *   **使用局部变量：**局部变量不会被多个线程同时访问到。
    *   **加锁：**通过 synchronized 锁或者 lock 锁，保证线程同步。
    *   **ThreadLocal：**ThreadLocal 可以确保每个线程都可以得到单独的一个 SimpleDateFormat 的对象，那么自然也就不存在竞争问题了
    *   **DateTimeFormatter：**如果是 Java8 应用，可以使用 DateTimeFormatter 代替 SimpleDateFormat，这是一个线程安全的格式化工具类
*   **获取时间不方便：**Date 类的年份是从 1900 年开始的，月份从 0 开始。
*   **没时区：**Date 类并不提供国际化，没有时区支持，因此 Java 引入了 java.util.Calendar 和 java.util.TimeZone 类，但他们同样存在上述所有的问题。
*   **需要格式化：**需要搭配 SimpleDateformat 类格式化时间，而且 SimpleDateformat 类也是线程不安全的，如果使用线程安全的 DateTimeFormatter 类格式化时间，则需要 JDK 版本在 1.8 及以上。

#### 5.8.4 格式化日期：

##### 5.8.4.1 SimpleDateformat 类：线程不安全

SimpleDateformat 类用于格式化和解析日期。

```
    public static void main(String[] args) throws ParseException {    //throws只是把异常抛出，延迟处理。用trycatch处理比较好
        //Date转String
        Date d=new Date();
        SimpleDateFormat sdf=new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
        String str=sdf.format(d);
        System.out.println(str);
        //String转Date
        d=sdf.parse(str);
        System.out.println(d);
    }
```

**日期格式模式：**

`SimpleDateFormat` 使用一组模式字母来定义日期时间格式。以下是一些常见的模式字母：

*   **`y`：** 年份（如 "yy" 表示年份的后两位，"yyyy" 表示完整的年份）。
*   **`M`：** 月份（1-12，"MM" 表示两位数字，"MMM" 表示缩写，"MMMM" 表示全名）。
*   **`d`：** 日期（1-31，"dd" 表示两位数字）。
*   **`H`：** 小时（0-23，"HH" 表示两位数字）。
*   **`m`：** 分钟（0-59，"mm" 表示两位数字）。
*   **`s`：** 秒钟（0-59，"ss" 表示两位数字）。

##### 5.8.4.2 DateTimeFormatter 类：线程安全

DateTimeFormatter 类是 Java 8 引入的日期时间 API（java.time 包）的一部分，用于格式化和解析日期时间对象。

**特点：**

*   不可变且线程安全
*   支持新的日期时间类
*   适用于 Java 8 及以上版本。
*   异常处理：在解析字符串时，如果字符串的格式与指定的模式不匹配，会抛出 DateTimeParseException 异常，因此需要异常处理。

**常用方法：**

**1. 格式化日期时间：****`format(TemporalAccessor temporal)`：** 格式化指定的日期时间对象。

```
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
LocalDateTime dateTime = LocalDateTime.now();
String formattedDateTime = formatter.format(dateTime);
```

**2. 解析字符串为日期时间：****`parse(CharSequence text)`：** 解析输入的文本，返回一个解析后的日期时间对象。

```
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
String dateString = "2023-12-01 15:30:00";
LocalDateTime parsedDateTime = formatter.parse(dateString, LocalDateTime::from);
```

**3. 获取格式化 / 解析模式：****`toString()`：** 获取当前格式化 / 解析器的模式字符串。

```
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
String pattern = formatter.toString(); // 返回 "yyyy-MM-dd HH:mm:ss"
```

**代码示例：**将当前时间格式化为字符串

```
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
 
public class Test {
    public static void main(String[] args) {
        // 创建 DateTimeFormatter 对象，指定日期时间格式模式
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
 
        // 获取当前日期时间
        LocalDateTime currentDateTime = LocalDateTime.now();
 
        // 格式化日期时间为字符串
        String formattedDateTime = currentDateTime.format(formatter);
        System.out.println("Formatted Date and Time: " + formattedDateTime);
    }
}
```

### 5.9 Math 类

Math 类是数学工具类，提供了一系列用于执行基本数学运算的静态方法。

**基本使用：** 

```
public class Test {
    public static void main(String[] args) {
        // 基本数学运算
        double absoluteValue = Math.abs(-5.5);  // 返回绝对值，结果为 5.5
        double ceilValue = Math.ceil(4.3);      // 返回不小于参数的最小整数值，结果为 5.0
        double floorValue = Math.floor(4.9);    // 返回不大于参数的最大整数值，结果为 4.0
        double maxValue = Math.max(10.2, 5.8);   // 返回两个参数中的最大值，结果为 10.2
        double minValue = Math.min(3.5, 7.1);    // 返回两个参数中的最小值，结果为 3.5
        long roundValue = Math.round(3.8);       // 返回最接近参数的整数值，四舍五入，结果为 4
 
        // 指数运算
        double expValue = Math.exp(2.0);         // 返回 e 的指数值，结果为 7.3890560989306495
        double logValue = Math.log(10.0);        // 返回以 e 为底的对数值，结果为 2.302585092994046
        double powValue = Math.pow(2.0, 3.0);    // 返回 2 的 3 次方，结果为 8.0
 
        // 平方根和立方根
        double sqrtValue = Math.sqrt(25.0);      // 返回参数的平方根，结果为 5.0
        double cbrtValue = Math.cbrt(27.0);      // 返回参数的立方根，结果为 3.0
 
        // 三角函数
        double sinValue = Math.sin(Math.PI / 6);     // 返回参数的正弦值，结果为 0.5
        double cosValue = Math.cos(Math.PI / 3);     // 返回参数的余弦值，结果为 0.5
        double tanValue = Math.tan(Math.PI / 4);     // 返回参数的正切值，结果为 1.0
 
        // 角度和弧度转换
        double degreesValue = Math.toDegrees(Math.PI / 2);  // 将弧度转换为角度，结果为 90.0
        double radiansValue = Math.toRadians(180.0);        // 将角度转换为弧度，结果为 π
 
        // 随机数生成
        double randomValue = Math.random();  // 返回一个大于等于 0.0 且小于 1.0 的随机浮点数
 
        // 输出结果
        System.out.println("Absolute Value: " + absoluteValue);
        System.out.println("Ceil Value: " + ceilValue);
        System.out.println("Floor Value: " + floorValue);
        System.out.println("Max Value: " + maxValue);
        System.out.println("Min Value: " + minValue);
        System.out.println("Round Value: " + roundValue);
 
        System.out.println("Exp Value: " + expValue);
        System.out.println("Log Value: " + logValue);
        System.out.println("Pow Value: " + powValue);
 
        System.out.println("Sqrt Value: " + sqrtValue);
        System.out.println("Cbrt Value: " + cbrtValue);
 
        System.out.println("Sin Value: " + sinValue);
        System.out.println("Cos Value: " + cosValue);
        System.out.println("Tan Value: " + tanValue);
 
        System.out.println("Degrees Value: " + degreesValue);
        System.out.println("Radians Value: " + radiansValue);
 
        System.out.println("Random Value: " + randomValue);
    }
}
```

**结果：**

![](<assets/1740013652310.png>)

### 5.10 Random

#### 5.10.1 基本介绍

**java.util.Random 类：**主要用于生成伪随机数。

**伪随机：**Random 类产生的数字是伪随机的，在相同种子数（seed）下的相同次数产生的随机数是相同的。

**基本使用方法：**

创建 Random 对象：

```
Random random = new Random();


```

生成随机整数：

```
// 生成一个介于 0（包含）和指定范围（不包含）之间的整数
int randomNumber = random.nextInt(10); // 生成 0 到 9 之间的整数
```

生成随机浮点数：

```
// 生成一个介于 0（包含）和 1（不包含）之间的浮点数
double randomDouble = random.nextDouble();
```

生成随机布尔值：

```
boolean randomBoolean = random.nextBoolean();


```

生成随机字节数组：

```
byte[] randomBytes = new byte[10];
random.nextBytes(randomBytes); // 将随机字节填充到指定的字节数组中
```

设置种子值（可选）：

```
// 如果需要重复生成相同的随机数序列，可以设置相同的种子值
long seed = 1234;
Random seededRandom = new Random(seed);
```

#### 5.10.2 代码示例

**不指定种子：** 

```
        Random random = new Random();
 
        //生成 0 到 10（不包括10）之间的随机整数
		int num = random.nextInt(10);
        //生成 -1 到 10（不包括10）之间的随机整数
		int num2 = random.nextInt(10)-1;
		System.out.println("生成 0 到 10（不包括10）之间的随机整数："+num);
 
        // 生成随机浮点数
        double randomDouble = random.nextDouble();
        System.out.println("Random Double: " + randomDouble);
 
        // 生成随机布尔值
        boolean randomBoolean = random.nextBoolean();
        System.out.println("Random Boolean: " + randomBoolean);
 
        // 生成随机字节数组
        byte[] randomBytes = new byte[5];
        random.nextBytes(randomBytes);
        System.out.println("Random Bytes: " + Arrays.toString(randomBytes));
```

![](<assets/1740013652646.png>)

​

**指定种子：** 

相同种子数（seed）下的相同次数，产生的随机数是相同的。 

```
public class Test {
    public static void main(String[] args) {
        // 指定种子为42
        long seed = 42;
        Random randomWithSeed = new Random(seed);
 
        // 生成随机整数
        int randomNumber1 = randomWithSeed.nextInt(100);
        System.out.println("Random Number 1: " + randomNumber1);
 
        // 再次生成随机整数，因为种子相同，结果应该相同
        int randomNumber2 = randomWithSeed.nextInt(100);
        System.out.println("Random Number 2: " + randomNumber2);
 
        // 创建另一个 Random 对象，没有指定种子
        Random randomWithoutSeed = new Random();
 
        // 生成随机整数，因为没有指定种子，结果不受前面的影响
        int randomNumber3 = randomWithoutSeed.nextInt(100);
        System.out.println("Random Number 3: " + randomNumber3);
    }
}
```