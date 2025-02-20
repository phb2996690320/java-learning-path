---
url: https://blog.csdn.net/qq_40991313/article/details/126453284
title: 【Java 笔记 + 踩坑】SpringBoot——基础_java war 应用调用 springboot-CSDN 博客
date: 2025-02-20 09:45:28
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1，SpringBoot 简介](#t0)

[1.0 SpringMvc 回顾](#t1) 

[1.1 SpringBoot 快速入门](#t2)

[1.1.1 开发步骤](#t3)

[1.1.2 SpringBoot 对比 Spring](#t4)

[1.1.3 官网构建工程](#t5)

[1.1.4 SpringBoot 工程打包启动](#t6)

[1.2 SpringBoot 概述](#t7)

[1.2.1 起步依赖](#t8)

[1.2.2 程序启动](#t9)

[1.2.3 切换 web 服务器，由 Tomcat 切换成 Jetty](#t10)

[2，配置文件](#t11)

[2.1 配置文件格式](#t12)

[2.1.1 环境准备](#t13)

[2.1.2 不同配置文件配置端口](#t14)

[2.1.3 三种配置文件的优先级](#t15)

[2.2 yaml 配置文件介绍](#t16)

[2.2.1 概述](#t17)

[2.2.2 语法规则](#t18)

[2.2.3 常用配置、清理控制台](#t19)

[2.3 yaml 配置文件数据读取](#t20)

[2.3.1 环境准备](#t21)

[2.3.2 三种读取 yaml 配置数据的方式](#t22)

[2.4 多环境配置](#t23)

[2.4.1 yaml 文件](#t24)

[2.4.2 properties 文件](#t25)

[2.4.3 命令行启动参数设置，修改端口、环境](#t26)

[2.4.4 maven 和 springboot 多环境开发兼容](#t27)

[2.5 配置文件分类](#t28)

[2.5.1 概述](#t29)

[2.5.1 代码演示](#t30)

[3，SpringBoot 整合 junit](#t31)

[3.0 回顾 Spring 整合 junit](#t32)

[3.1 环境准备](#t33)

[3.2 编写测试类,@SpringBootTest](#t34)

[4，SpringBoot 整合 mybatis](#t35)

[4.1 回顾 Spring 整合 Mybatis](#t36)

[4.2 SpringBoot 整合 mybatis](#t37)

[4.2.1 创建模块](#t38)

[4.2.2 定义实体类](#t39)

[4.2.3 定义 dao 接口](#t40)

[4.2.4 定义测试类](#t41)

[4.2.5 编写配置](#t42)

[4.2.6 代码测试，根据 id 查书籍](#t43)

[4.2.7 配置内置数据源、Druid 数据源](#t44)

[4.3 案例，整合 ssm 的书籍增删改查项目](#t45)

[4.3.1 创建工程](#t46)

[4.3.2 代码拷贝、dao 注解 @Mapper、WebMvcConfigurer 添加资源处理器、拦截器](#t47)

[4.3.3 yml 配置端口、数据源](#t48)

[4.3.4 静态资源](#t49)

[4.4 SpringBoot 整合 MyBatis-Plus 回顾](#t50)

[5、SSMP 整合综合案例，Book 增删改查分页](#t51)

[0. 模块创建](#t52)

[1. 实体类开发，lombok](#t53)

[2. 数据层开发——基础 CRUD](#t54)

[3. 数据层开发——分页功能制作](#t55)

[4. 数据层开发——条件查询功能制作](#t56)

[5. 业务层开发](#t57)

[业务层快速开发（慎用）](#t58)

[6. 表现层开发](#t59)

[7. 表现层消息一致性处理](#t60)

[8. 前后端联通性测试](#t61)

[9. 页面基础功能开发](#t62)

[F-1. 列表功能（非分页版）](#t63)

[F-2. 添加功能](#t64)

[F-3. 删除功能](#t65)

[F-4. 修改功能](#t66)

[10. 业务消息一致性处理](#t67)

[11. 页面功能开发](#t68)

[F-5. 分页功能](#t69)

[F-6. 删除功能维护](#t70)

[F-7. 条件查询功能](#t71)

## 1，SpringBoot 简介

### 1.0 SpringMvc 回顾 

`SpringBoot` 是由 `Pivotal` 团队提供的全新框架，其**设计目的是用来简化 `Spring` 应用的初始搭建以及开发过程。**

使用了 `Spring` 框架后已经简化了我们的开发。而 `SpringBoot` 又是对 `Spring` 开发进行简化的，可想而知 `SpringBoot` 使用的简单及广泛性。

**回顾 `SpringMVC` ：**

*   **1. 创建工程，并在 `pom.xml` 配置文件中配置所依赖的坐标，还可以导入 jackson，JSON 类型转换**

![](<assets/1740015928608.png>)

*   **2. 编写 `web3.0` 的配置类**
    
    作为 `web` 程序，`web3.0` 的配置类不能缺少，而这个配置类还是比较麻烦的，代码如下
    

![](<assets/1740015928840.png>)

*   **3. 编写 `SpringMVC` 的配置类**

![](<assets/1740015929118.png>)

做到这只是将工程的架子搭起来。要想被外界访问，最起码还需要提供一个 `Controller` 类，在该类中提供一个方法。

*   **4. 编写 `Controller` 类**

![](<assets/1740015929328.png>)

从上面的 `SpringMVC` 程序开发可以看到，前三步都是在搭建环境，而且这三步基本都是固定的。`SpringBoot` 就是对这三步进行简化了。接下来我们通过一个入门案例来体现 `SpingBoot` 简化 `Spring` 开发。

### 1.1 SpringBoot 快速入门

#### 1.1.1 开发步骤

`SpringBoot` 开发起来特别简单，分为如下几步：

1.  创建新模块，**选择 Spring 初始化**，并配置模块相关基础信息
2.  选择当前模块需要使用的技术集
3.  开发控制器类
4.  运行自动生成的 Application 类

知道了 `SpringBoot` 的开发步骤后，接下来我们进行具体的操作

**创建新模块**

*   点击 `+` 选择 `New Module` 创建新模块

![](<assets/1740015929687.png>)

*   **选择 `Spring Initializr`** ，用来创建 `SpringBoot` 工程
    
    以前我们选择的是 `Maven` ，今天选择 `Spring Initializr` 来快速构建 `SpringBoot` 工程。而在 `Module SDK` 这一项选择我们安装的 `JDK` 版本。
    

![](<assets/1740015929950.png>)

如果只能选择 JDK17，则修改服务器 url 为阿里云的服务：

```
https://start.aliyun.com

```

 **如果报错：** 

```
'https://start.spring.io' 的初始化失败
请检查 URL、网络和代理设置。
 
错误消息:
Cannot download 'https://start.spring.io': Connection reset
 
```

则需要设置代理：

![](<assets/1740015930309.png>)

```
https://plugins.jetbrains.com/ 

``` 

*   **对 `SpringBoot` 工程进行相关的设置**
    
    我们使用这种方式构建的 **`SpringBoot` 工程其实也是 `Maven` 工程**，而该方式只是一种**快速构建**的方式而已。**注意**选择的包是 jar，不是 war。java 版本和 jdk 版本一致，包名跟组名写一样方便阅读。
    
    ![](<assets/1740015930541.png>)
    
    **注意：打包方式这里需要设置为 `Jar`**
    

![](<assets/1740015930842.png>)

*   选中 **`Web`**，然后勾选 **`Spring Web`**
    
    由于我们需要开发一个 `web` 程序，使用到了 `SpringMVC` 技术，所以按照下图红框进行勾选
    

![](<assets/1740015931090.png>)

![](<assets/1740015931402.png>)

*   下图界面不需要任何修改，直接点击 `Finish` 完成 `SpringBoot` 工程的构建

![](<assets/1740015931640.png>)

经过以上步骤后就创建了如下结构的模块，它会帮我们自动生成一个 `Application` 类，而该类一会再启动服务器时会用到。

*   删除选中的文件、目录：

![](<assets/1740015931836.png>)

![](<assets/1740015932039.png>)

**报错解决：**

如果 maven 模块的 pom 图标显示不正常，就右键 pom.xml，添加成 Maven 项目：

![](<assets/1740015932229.png>)

![](<assets/1740015932444.png>)

**找不到插件'org.springframework.boot:spring-boot-maven-plugin:'：**

手动导入版本，版本号与 springboot 一致：

```
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<version>2.7.3</version>
			</plugin>
```

**注意：**

1.  在创建好的工程中**不需要创建 SpringConfig 和 SpringMvcConfig 配置类**
    
2.  创建好的项目会**自动生成其他的一些文件**，而这些文件目前对我们来说没有任何作用，所以可以将这些文件**删除**。
    
    可以删除的目录和文件如下：
    
    *   `.mvn`
    *   `.gitignore`
    *   `HELP.md`
    *   `mvnw`
    *   `mvnw.cmd`

**所有文件介绍：**

*   \1. .gitignore：分布式版本控制系统 git 的配置文件，意思为忽略提交
*   在 .gitingore 文件中，遵循相应的语法，即在每一行指定一个忽略规则。 如：.log、/target/、.idea
*   \2. mvnw：全名是 maven wrapper 的文件
*   它的作用是在 maven-wrapper.properties 文件中记录你要使用的 maven 版本，当用户执行 mvnw clean 命令时，发现当前用户的 maven 版本和期望的版本不一致，那么就下载期望的版本，然后用期望的版本来执行 mvn 命令，比如 mvn clean 命令。
*   \3. mvn 文件夹：存放 mvnw 相关文件
*   存放着 maven-wrapper.properties 和相关 jar 包以及名为 MavenWrapperDownloader 的 java 文件
*   \4. mvn.cmd：执行 mvnw 命令的 cmd 入口
*   _注：mvnw 文件适用于 Linux（bash），mv_ 斜体样式 * nw.cmd 适用于 Windows 环境。
*   \5. .iml 文件：intellij idea 的工程配置文件
*   里面包含当前 project 的一些配置信息，如模块开发的相关信息，比如 java 组件，maven 组件，插件组件等，还可能会存储一些模块路径信息，依赖信息以及一些别的信息。
*   \6. .idea 文件夹：存放项目的配置信息
*   包括数据源，类库，项目字符编码，历史记录，版本控制信息等。
*   \7. pom.xml：项目对象模型（核心重要）
*   pom.xml 主要描述了项目的 maven 坐标，依赖关系，开发者需要遵循的规则，缺陷管理系统，组织和 licenses，以及其他所有的项目相关因素，是项目级别的配置文件。

**创建 `Controller`**

在 `com.itheima.controller` 包下创建 `BookController` ，代码如下：

```
@RestController
@RequestMapping("/books")
public class BookController {
 
    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println("id ==> "+id);
        return "hello , spring boot!";
    }
}
```

**启动服务器**

运行 `SpringBoot` 工程不需要使用本地的 `Tomcat` 和 插件，只需要**右键运行项目 `com.itheima` 包下的 `Application` 类**，我们就可以在控制台看出如下信息

![](<assets/1740015932722.png>)

启动后修改配置：

![](<assets/1740015933011.png>)

![](<assets/1740015933255.png>)

**如果报错：找不到插件 org.springframework.boot spring-boot-*_**_[maven](https://so.csdn.net/so/search?q=maven&spm=1001.2101.3001.7020 "maven")_***_-plugin**

进入 C:\Users \ 用户名. m2\repository

然后在这个文件夹下搜索 spring-boot-[maven](https://so.csdn.net/so/search?q=maven&spm=1001.2101.3001.7020 "maven")-plugin，打开这个文件夹，里面都是版本号

![](<assets/1740015933435.png>)

选择加入 pom.xml 即可：

![](<assets/1740015933657.png>)

**进行测试**

使用 `Postman` 工具来测试我们的程序

![](<assets/1740015933843.png>)

**`SpringBoot` 是如何让开发变简单的呢？**

我们代码之所以能简化，就是**因为指定的父工程和 `Spring Web` 依赖实现的**。

要研究这个问题，我们需要看看 `Application` 类和 `pom.xml` 都书写了什么。先看看 `Applicaion` 类，该类内容如下：

```
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

这个类中的东西很简单，就在类上添加了一个 **`@SpringBootApplication` 注解**，而在主方法中就一行代码。我们在**启动服务器时就是执行的该类中的主方法**。

再看看 **`pom.xml` 配置文件**中的内容，可以发现除了打包插件之外，引入的父工程、两个依赖前缀都是 spring-boot-starter

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <!--指定了一个父工程，父工程中的东西在该工程中可以继承过来使用-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.0</version>
    </parent>
    <groupId>com.itheima</groupId>
    <artifactId>springboot_01_quickstart</artifactId>
    <version>0.0.1-SNAPSHOT</version>
 
    <!--JDK 的版本-->
    <properties>
        <java.version>8</java.version>
    </properties>
    
    <dependencies>
        <!--该依赖就是我们在创建 SpringBoot 工程勾选的那个 Spring Web 产生的-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
		<!--这个是单元测试的依赖，我们现在没有进行单元测试，所以这个依赖现在可以没有-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
 
    <build>
        <plugins>
            <!--这个插件是在打包时需要的，而这里暂时还没有用到-->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

#### 1.1.2 `SpringBoot` 对比`Spring`

做完 `SpringBoot` 的入门案例后，接下来对比一下 `Spring` 程序和 `SpringBoot` 程序。如下图

![](<assets/1740015934120.png>)

*   **坐标**
    
    `Spring` 程序中的坐标需要自己编写，而且坐标非常多
    
    `SpringBoot` 程序中的坐标是我们在创建工程时进行勾选自动生成的
    
*   **web3.0 配置类**
    
    `Spring` 程序需要自己编写这个配置类。这个配置类大家之前编写过，肯定感觉很复杂
    
    `SpringBoot` 程序不需要我们自己书写
    
*   **配置类**
    
    `Spring/SpringMVC` 程序的配置类需要自己书写。而 `SpringBoot` 程序则不需要书写。
    

**注意：基于 Idea 的 `Spring Initializr` 快速构建 `SpringBoot` 工程时需要联网。**

#### 1.1.3 官网构建工程

在入门案例中**之所以能快速构建 `SpringBoot` 工程，是因为 `Idea` 使用了官网提供了快速构建 `SpringBoot` 工程的组件**实现的。那如何在官网进行工程构建呢？通过如下步骤构建

**进入 SpringBoot 官网**

官网地址如下：

```
https://spring.io/projects/spring-boot


```

进入到 `SpringBoot` 官网后拖到最下方就可以看到如下内容

![](<assets/1740015934375.png>)

然后点击 `Spring Initializr` 超链接就会跳转到如下页面

![](<assets/1740015934584.png>)

这个页面内容是不是感觉很眼熟的，这和我们使用 `Idea` 快速构建 `SpringBoot` 工程的界面基本相同。在上面页面输入对应的信息

**选择依赖**

选择 `Spring Web` 可以点击上图右上角的 `ADD DEPENDENCIES... CTRL + B` 按钮，就会出现如下界面

![](<assets/1740015934853.png>)

**生成工程**

以上步骤完成后就可以生成 `SpringBoot` 工程了。在页面的最下方点击 `GENERATE CTRL + 回车` 按钮生成工程并下载到本地，如下图所示

![](<assets/1740015935138.png>)

打开下载好的压缩包可以看到工程结构和使用 `Idea` 生成的一模一样，如下图

![](<assets/1740015935391.png>)

而打开 `pom.xml` 文件，里面也包含了父工程和 `Spring Web` 的依赖。

通过上面官网的操作，我们知道 `Idea` 中快速构建 `SpringBoot` 工程其实就是使用的官网的快速构建组件，那以后即使没有 `Idea` 也可以使用官网的方式构建 `SpringBoot` 工程。

#### 1.1.4 SpringBoot 工程打包启动

**问题导入**

![](<assets/1740015935723.png>)

以后我们和前端开发人员协同开发，而**前端开发人员需要测试前端程序就需要后端开启服务器**，这就受制于后端开发人员。**为了摆脱这个受制**，前端开发人员尝试着在自己电脑上安装 `Tomcat` 和 `Idea` ，在自己电脑上启动后端程序，这显然不现实。

我们**后端可以将 `SpringBoot` 工程打成 `jar` 包**，该 `jar` 包**运行不依赖于 `Tomcat` 和 `Idea` 这些工具**也可以正常运行，只是这个 `jar` 包在运行过程中**连接和我们自己程序相同的 `Mysql` 数据库**即可。这样就可以解决这个问题，如下图

![](<assets/1740015935947.png>)

**打包**

**打包 package 前先 clean 是好习惯。**

先终止当前运行的项目。由于我们在构建 `SpringBoot` 工程时已经在 `pom.xml` 中配置了 spring-boot-maven-plugin 插件

```
//spring-boot-maven-plugin插件打包时需要的
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
```

所以我们只需要使用 `Maven` 的 **`package` 指令打包**就会在 **`target` 目录**下**生成对应的 `Jar` 包**。

![](<assets/1740015936229.png>)

**注意：该插件必须配置，不然打好的 `jar` 包也是有问题的。**

**启动**

![](<assets/1740015936448.png>)

进入 `jar` 包所在位置，地址栏输入 cmd 回车，在 `命令提示符` 中输入如下命令（将 jar 包名字改为自己的）：

```
java -jar springboot_01_quickstart-0.0.1-SNAPSHOT.jar


```

**启动 jar 包并覆盖设置端口、 切换环境：**

```
java –jar springboot.jar –-server.port=88 –-spring.profiles.active=test
 
 
``` 

执行上述命令就可以看到 `SpringBoot` 运行的日志信息

![](<assets/1740015936700.png>)

然后 postman 发请求，可以看到命令行里输出后端信息，postman 展示响应信息。

非 springboot 的 maven 是不能用 java -jar 命令的。

### 1.2 SpringBoot 概述

`SpringBoot` 是由 Pivotal 团队提供的全新框架，其设计目的是用来**简化** Spring 应用的**初始搭建**以及**开发过程**。

大家已经感受了 `SpringBoot` 程序，回过头看看 `SpringBoot` 主要作用是什么，就是简化 `Spring` 的搭建过程和开发过程。

**原始 `Spring` 环境搭建和开发存在以下问题：**

*   **配置繁琐**
*   **依赖设置繁琐**

**`SpringBoot` 程序优点**恰巧就是针对 `Spring` 的缺点

*   **自动配置。**这个是用来解决 `Spring` 程序配置繁琐的问题
*   **起步依赖（简化以来配置）。**这个是用来解决 `Spring` 程序依赖设置繁琐的问题
*   **辅助功能**（内置服务器,...）。我们在**启动 `SpringBoot` 程序**时既没有使用本地的 `tomcat` 也没有使用 `tomcat` 插件，而是使用 **`SpringBoot` 内置的服务器**。

接下来我们来说一下 `SpringBoot` 的起步依赖

#### 1.2.1 起步依赖

我们使用 `Spring Initializr` 方式创建的 `Maven` 工程的的 `pom.xml` 配置文件中自动生成了很多**包含 `starter` 的依赖**，如下图

![](<assets/1740015936973.png>)

这些依赖就是**启动依赖**，接下来我们探究一下他是如何实现的。

**1.2.1.1 探索父工程**

**爷工程 spring-boot-dependencies.pom 里配置了各依赖插件的特定版本，因此我们只需要使用对应版本的 springboot，在 pom.xml 里配置不含版本的插件或依赖，就可以继承对应插件或依赖。**

**爷工程 pom.xml 查看：**

从上面的文件中可以看到指定了一个父工程，我们 ctrl+b 进入到父工程，**发现父工程中又指定了一个父工程**，如下图所示

![](<assets/1740015937244.png>)

**再进入到该父工程中**，在该工程中我们可以看到配置内容结构如下图所示

![](<assets/1740015937510.png>)

上图中的 **`properties` 标签**中**定义了各个技术软件依赖的版本**，避免了我们在使用不同软件技术时考虑版本的兼容问题。

![](<assets/1740015937773.png>)

在 `properties` 中我们找 `servlet` 和 `mysql` 的版本如下图

![](<assets/1740015938086.png>)

**爷工程 pom.xml 里依赖管理标签：**

**`dependencyManagement` 标签**是进行依赖版本锁定，但是**并没有导入对应的依赖**。

如果我们工程需要哪个依赖**只需要引入依赖的 `groupid` 和 `artifactId` 不需要定义 `version`。**

![](<assets/1740015938320.png>)

**爷工程 pom.xml 里插件管理：**

而 `build` 标签中也对插件的版本进行了锁定，如下图

![](<assets/1740015938485.png>)

看完了父工程中 `pom.xml` 的配置后不难理解**我们工程的的依赖不配置 `version的原因`**。

**1.2.1.2 探索依赖**

在我们创建的工程中的 `pom.xml` 中配置了 spring-boot-starter-web 如下依赖

![](<assets/1740015938730.png>)

进入到该依赖，查看 `pom.xml` 的依赖会发现它引入了如下的依赖

![](<assets/1740015938913.png>)

里面**引入了 `spring-web` 和 `spring-webmvc` 的依赖**，这就是为什么我们的工程中没有依赖这两个包还能正常使用 `springMVC` 中的注解的原因。

而**依赖 `spring-boot-starter-tomcat`** ，从名字基本能确认**内部依赖了 `tomcat`**，**所以我们的工程才能正常启动。**

**结论：以后需要使用技术，只需要引入该技术对应的起步依赖即可**

同样方法，spring-boot-starter-test 依赖里有 spring-test 等依赖。

**小结：**

**starter**

*   只要依赖名字中有 starter，就一定是起步以来。starter 是`SpringBoot` 中常见项目名称，**定义了当前项目使用的所有项目坐标**，以达到减少依赖配置的目的。

**parent**

*   所有 `SpringBoot` 项目要继承的项目，定义了若干个坐标版本号（依赖管理，而非依赖），以达到减少依赖冲突的目的
*   `spring-boot-starter-parent`（2.5.0）与 `spring-boot-starter-parent`（2.4.6）共计 57 处坐标版本不同。**springboot 版本也不是越新越好，实际开发中要看公司项目的`spring-boot-starter-parent`版本，然后使用对应的 springboot 版本。**

**实际开发**

*   **使用任意坐标时，仅书写 GAV 中的 G 和 A，V 由 SpringBoot 提供**
    
    G：groupid
    
    A：artifactId
    
    V：version
    
*   如发生坐标错误，再指定 version（要小心版本冲突）
    

#### 1.2.2 程序启动

创建的每一个 `SpringBoot` 程序时都包含一个类似于下面的类，我们将这个类称作**引导类、主启动类**

```
@SpringBootApplication
public class Springboot01QuickstartApplication {
    
    public static void main(String[] args) {
        SpringApplication.run(Springboot01QuickstartApplication.class, args);
    }
}
```

**注意：**

*   `SpringBoot` 在创建项目时，采用 **jar 的打包方式**
    
*   `SpringBoot` 的**引导类是项目的入口**，**运行 `main` 方法就可以启动项目**
    
    因为我们在 `pom.xml` 中配置了 **`spring-boot-starter-web` 依赖**，而该依赖通过前面的学习知道**它依赖 `tomcat`** ，所以运行 `main` 方法就可以使用 `tomcat` 启动咱们的工程。
    

#### 1.2.3 切换 web 服务器，由 Tomcat 切换成 Jetty

**`使用spring-boot-starter-web` 依赖**实现内置 Tomcat 服务器，是 springboot 带来的辅助功能。

现在我们启动工程使用的是 `tomcat` 服务器，那能不能**不使用 `tomcat` 而使用 `jetty` 服务器**。

`jetty` 在我们 `maven` 高级时讲 `maven` 私服使用的服务器，比 Tomcat 更轻量级，可扩展性强，谷歌应用引擎已全面切换为 Jetty，但目前大型应用一般用的还是 Tomcat。

而要**切换 `web` 服务器就需要**使用 **`exclusion` 标签将默认的 `tomcat` 服务器给排除掉。**

**排除 Tomcat 依赖**

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <groupId>org.springframework.boot</groupId>
        </exclusion>
    </exclusions>
</dependency>
```

**引入 `jetty` 的起步依赖：**

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jetty</artifactId>
</dependency>
```

接下来再次运行引导类，在日志信息中就可以看到使用的是 `jetty` 服务器

![](<assets/1740015939173.png>)

**小结：**

通过切换服务器，我们不难发现在使用 `SpringBoot` 换技术时只需要导入该技术的起步依赖即可。

## 2，配置文件

### 2.1 配置文件格式

我们现在启动服务器**默认的端口号是 `8080`**，访问路径可以书写为

```
http://localhost:8080/books/1


```

在线上环境我们还是希望将**端口号改为 `80`**，这样在访问的时候就可以不写端口号了，如下

```
http://localhost/books/1


```

**`SpringBoot` 提供了多种属性配置方式**

*   `application.properties，常用，在resources包下`
    
    ```
    server.port=80
    
    
    ```
    
*   **`application.yml（以后用这种）`**
    
    ```
    server:
    	port: 81
    ```
    
*   `application.yaml`
    
    ```
    server:
    	port: 82
    ```
    

**注意：`SpringBoot` 程序的配置文件名必须是 `application` ，只是后缀名不同而已。**

#### 2.1.1 环境准备

创建一个新工程 `springboot_02_base_config` 用来演示不同的配置文件，工程环境和入门案例一模一样，结构如下：

![](<assets/1740015939421.png>)

在该工程中的 `com.itheima.controller` 包下创建一个名为 `BookController` 的控制器。内容如下：

```
@RestController
@RequestMapping("/books")
public class BookController {
 
    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println("id ==> "+id);
        return "hello , spring boot!";
    }
}
```

#### 2.1.2 不同配置文件配置端口

*   **方法一（默认，不建议）：application.properties 配置文件**

现在需要进行配置，配合文件必须放在 `resources` 目录下，而该目录下有一个名为 `application.properties` 的配置文件，我们就可以在该配置文件中修改端口号，在该配置文件中书写 `port` ，`Idea` 就会提示，如下

![](<assets/1740015939708.png>)

`application.properties` 配置文件内容如下：

```
#端口号
server.port = 8888 
 
#项目名，如果不设定，默认是 /
server.servlet.context-path : /demo 
```

启动服务，会在控制台打印出日志信息，从日志信息中可以看到绑定的端口号已经修改了

![](<assets/1740015940024.png>)

*   **方法二（以后使用）：application.yml 配置文件**

删除 `application.properties` 配置文件中的内容。在 `resources` 下创建一个名为 `application.yml` 的配置文件，在该文件中书写端口号的配置项，格式如下：

```
server:
  #端口号
  port: 8888
  #项目名，如果不设定，默认是 /
  servlet:
    context-path: /demo
```

**注意： 在`:`后，数据前一定要加空格。**

而在 `yml` 配置文件中也是有提示功能的，我们也可以在该文件中书写 `port` ，然后 `idea` 就会提示并书写成上面的格式

![](<assets/1740015940350.png>)

启动服务，可以在控制台看到绑定的端口号是 `81`

![](<assets/1740015940572.png>)

*   **方法三（不建议）：application.yaml 配置文件**

删除 `application.yml` 配置文件和 `application.properties` 配置文件内容，然后在 `resources` 下创建名为 `application.yaml` 的配置文件，**配置内容和后缀名为 `yml` 的配置文件中的内容相同，只是使用了不同的后缀名而已**

`application.yaml` 配置文件内容如下：

```
server:
	port: 83
```

启动服务，在控制台可以看到绑定的端口号

![](<assets/1740015940768.png>)

**注意：在配置文件中输入 port，如果没有提示，可以使用以下方式解决**

*   点击 `File` 选中 `Project Structure`

![](<assets/1740015940968.png>)

*   弹出如下窗口，按图中标记红框进行选择

![](<assets/1740015941267.png>)

*   通过上述操作，会弹出如下窗口

![](<assets/1740015941671.png>)

*   点击上图的 `+` 号，弹出选择该模块的配置文件

![](<assets/1740015941928.png>)

*   通过上述几步后，就可以看到如下界面。`properties` 类型的配合文件有一个，`ymal` 类型的配置文件有两个

![](<assets/1740015942141.png>)

#### 2.1.3 三种配置文件的优先级

**结论：优先级 properties>yml>yaml**

在三种配合文件中分别配置不同的端口号，启动服务查看绑定的端口号。用这种方式就可以看到哪个配置文件的优先级更高一些

`application.properties` 文件内容如下：

```
server.port=80


```

`application.yml` 文件内容如下：

```
server:
	port: 81
```

`application.yaml` 文件内容如下：

```
server:
	port: 82
```

启动服务，在控制台可以看到使用的端口号是 `80`。说明 `application.properties` 的优先级最高

注释掉 `application.properties` 配置文件内容。再次启动服务，在控制台可以看到使用的端口号是 `81`，说明 `application.yml` 配置文件为第二优先级。

从上述的验证结果可以确定三种配置文件的优先级是：

**`application.properties` > `application.yml` > `application.yaml`**

**注意：**

*   `SpringBoot` 核心配置文件名为 `application`
    
*   `SpringBoot` 内置属性过多，且所有属性集中在一起修改，在使用时，通过提示键 + 关键字修改属性
    
    例如要设置日志的级别时，可以在配置文件中书写 `logging`，就会提示出来。配置内容如下
    
    ```
    logging:
      level:
        root: info
    ```
    

### 2.2 yaml 配置文件介绍

#### 2.2.1 概述

`properties` 类型的配合文件之前我们学习过，接下来我们重点学习 `yaml` 类型的配置文件。

**yaml 格式扩展名：.yml（主流）或. yaml。**

**YAML（YAML Ain't Markup Language），一种数据序列化格式。**这种格式的配置文件在近些年已经占有主导地位，那么这种配置文件和前期使用的配置文件是有一些优势的，我们先看之前使用的配置文件。

最开始我们使用的是 `xml` ，格式如下：

```
<enterprise>
    <name>itcast</name>
    <age>16</age>
    <tel>4006184000</tel>
</enterprise>
```

而 `properties` 类型的配置文件如下

```
enterprise.name=itcast
enterprise.age=16
enterprise.tel=4006184000
```

`yaml` 类型的配置文件内容如下

```
enterprise:
	name: itcast
	age: 16
	tel: 4006184000
```

**优点：**

*   **容易阅读**
    
    `yaml` 类型的配置文件比 `xml` 类型的配置文件更容易阅读，结构更加清晰
    
*   容易与脚本语言交互
    
*   以数据为核心，**重数据轻格式**，如下图数据清爽
    
    `yaml` 更注重数据，而 `xml` 更注重格式
    

![](<assets/1740015942411.png>)

**YAML 文件扩展名：**

*   `.yml` (主流)
*   `.yaml`

上面两种后缀名都可以，以后使用更多的还是 `yml` 的。

#### 2.2.2 语法规则

*   **大小写敏感**
    
*   属性层级关系使用多行描述，每行结尾使用冒号结束（这里冒号说的是**标签名后的冒号**）
    
*   **使用缩进表示层级关系**，**同层级左侧对齐**，**只允许使用空格（不允许使用 Tab 键）**
    
    **空格的个数并不重要，只要保证同层级的左侧对齐即可。**
    
*   **属性值前面添加空格**（属性名与属性值之间使用冒号加空格作为分隔）
    
*   #表示注释
    

**核心规则：数据前面要加空格与冒号隔开**

**数组数据**在数据书写位置的下方使用**减号作为数据开始符号**，每行书写一个数据，减号与数据间**空格分隔**，例如

```
enterprise:
  name: itcast
  age: 16
  tel: 4006184000
  subject:
    - Java
    - 前端
    - 大数据
```

**连接符 --- 作用：**单文件中可以通过`---`实现多文件的效果。

```
#开发
spring:
  profiles: dev #给开发环境起的名字
server:
  port: 80
---
#生产
spring:
  profiles: pro #给生产环境起的名字
server:
  port: 81
---
#测试
spring:
  profiles: test #给测试环境起的名字
server:
  port: 82
---
```

#### 2.2.3 常用配置、清理控制台

properties 下新建 logback.xml：

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
</configuration>
```

```
server:
  port: 80
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    url: jdbc:mysql://localhost/ssm_db?serverTimezone=UTC
    username: root
    password: 填密码
    driver-class-name: com.mysql.cj.jdbc.Driver
  main:
    banner-mode: off
mybatis-plus:
  global-config:
    db-config:
      table-prefix: tbl_
#关闭图标
    banner: false
#打开日志
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

### 2.3 yaml 配置文件数据读取

#### 2.3.1 环境准备

新创建一个名为 `springboot_03_read_data` 的 `SpringBoot` 工程，目录结构如下

![](<assets/1740015942685.png>)

在 `com.itheima.controller` 包写创建名为 **`BookController` 的控制器**，内容如下

```
@RestController
@RequestMapping("/books")
public class BookController {
 
    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println("id ==> "+id);
        return "hello , spring boot!";
    }
}
```

在 `com.itheima.domain` 包下创建一个名为 `Enterprise` 的实体类等会用来封装数据，内容如下

```
public class Enterprise {
    private String name;
    private int age;
    private String tel;
    private String[] subject;
    
    //自己加上setter and getter，别忘了，别忘了，别忘了
    
    //toString
}
```

在 `resources` 下创建一个名为 `application.yml` 的配置文件，里面配置了不同的数据，内容如下

```
lesson: SpringBoot
 
server:
  port: 80
 
enterprise:
  name: itcast
  age: 16
  tel: 4006184000
  subject:
    - Java
    - 前端
    - 大数据
```

#### 2.3.2 三种读取 yaml 配置数据的方式

**2.3.2.1 使用 @Value 注解读取 yaml（适合少量数据）**

使用 `@Value("表达式")` 注解可以从配合文件中读取数据，注解中用于读取属性名引用方式跟 Spring 里一样，是：`${一级属性名.二级属性名……}`

这种方法和 Spring 读取配置文件很像，不同点是 Spring 要读取配置文件，要先在 SpringConfig 配置类扫描配置文件，例如 @PropertySource("classpath:jdbc.properties")

我们可以在 `BookController` 中使用 `@Value` 注解读取配合文件数据，如下

```
@RestController
@RequestMapping("/books")
public class BookController {
    
    @Value("${lesson}")
    private String lesson;
    @Value("${server.port}")
    private Integer port;
    @Value("${enterprise.subject[0]}")
    private String subject_00;
 
    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println(lesson);
        System.out.println(port);
        System.out.println(subject_00);
        return "hello , spring boot!";
    }
}
```

**2.3.2.2 自动注入 Environment 对象读取 yaml（少用，了解）**

上面方式读取到的数据特别零散，`SpringBoot` 还可以使用 **`@Autowired` 注解注入 `Environment` 对象的方式读取数据**。这种方式 `SpringBoot` 会将配置文件中所有的数据封装到 `Environment` 对象中，如果需要使用哪个数据只需要通过调用 `Environment` 对象的 `getProperty(String name)` 方法获取。具体代码如下：

```
@RestController
@RequestMapping("/books")
public class BookController {
    
    @Autowired
    private Environment env;
    
    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println(env.getProperty("lesson"));
        System.out.println(env.getProperty("enterprise.name"));
        System.out.println(env.getProperty("enterprise.subject[0]"));
        return "hello , spring boot!";
    }
}
```

**注意：这种方式，框架内容大量数据，而在开发中我们很少使用。**

**2.3.2.3 自定义实体类 bean 读取 yaml（最常用），@ConfigurationProperties**

`SpringBoot` 还提供了**将配置文件中的数据封装到我们自定义的实体类对象中**的方式。具体操作如下：

*   将**实体类 `bean` 的创建交给 `Spring` 管理**。
    
    在类上添加 `@Component` 注解
    
*   使用 **`@ConfigurationProperties` 注解**表示加载配置文件
    
    在该注解中也可以使用 `prefix` 属性指定只加载指定前缀的数据
    
*   在 `BookController` 中进行注入
    

为什么这种方法最常用：

例如配置 Mybatis 时候，会定义一个实体类 bean，加载 yaml 中的数据库信息。

**具体代码如下：**

`Enterprise` 实体类内容如下：实体类要有 getter,setter,toString 好习惯，防止报错。

```
@Component
//ConfigurationProperties译为配置属性，prefix前缀必须设为"enterprise"，不加冒号，不能少字母。配置prefix后可以读取到yaml中前缀为enterprise标签的所有属性。
//虽然prefix叫前缀，但它的意思是通过标签名看成前缀。
@ConfigurationProperties(prefix = "enterprise")
public class Enterprise {
    private String name;
    private int age;
    private String tel;
    private String[] subject;
 
    public String getName() {
        return name;
    }
 
    public void setName(String name) {
        this.name = name;
    }
 
    public int getAge() {
        return age;
    }
 
    public void setAge(int age) {
        this.age = age;
    }
 
    public String getTel() {
        return tel;
    }
 
    public void setTel(String tel) {
        this.tel = tel;
    }
 
    public String[] getSubject() {
        return subject;
    }
 
    public void setSubject(String[] subject) {
        this.subject = subject;
    }
 
    @Override
    public String toString() {
        return "Enterprise{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", tel='" + tel + '\'' +
                ", subject=" + Arrays.toString(subject) +
                '}';
    }
}
```

`BookController` 内容如下：

```
@RestController
@RequestMapping("/books")
public class BookController {
    
    @Autowired
    private Enterprise enterprise;
 
    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println(enterprise.getName());
        System.out.println(enterprise.getAge());
        System.out.println(enterprise.getSubject());
        System.out.println(enterprise.getTel());
        System.out.println(enterprise.getSubject()[0]);
        return "hello , spring boot!";
    }
}
```

**注意：**

**警告：**使用第三种方式，在实体类上**有如下警告提示**

![](<assets/1740015942899.png>)

**解决：**这个警告提示解决是在 `pom.xml` 中添加如下依赖即可

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

**为了加强管理第三方配置类，这种方式还可以优化：**

 **步骤①**：**在引导类上开启 @EnableConfigurationProperties 注解，并标注要使用 @ConfigurationProperties 注解绑定属性的类**

```
@SpringBootApplication
@EnableConfigurationProperties(ServerConfig.class)
public class Springboot13ConfigurationApplication {
}
```

**步骤②**：在对应的类上直接使用 @ConfigurationProperties 进行属性绑定

```
@Data
@ConfigurationProperties(prefix = "servers")
//这里不用加@component了
public class ServerConfig {
    private String ipAddress;
    private int port;
    private long timeout;
}
```

**注意：**

*   **开启了 @EnableConfigurationProperties 注解后**，**绑定属性的 ServerConfig 类就不能声明 @Component 注解，否则会被 spring 检测到两个 bean**。

**当使用 @EnableConfigurationProperties 注解时，spring 会默认将其标注的类定义为 bean，因此不能再次声明 @Component 注解了。**

### 2.4 多环境配置

以后在工作中，对于开发环境、测试环境、生产环境的配置肯定都不相同，比如我们开发阶段会在自己的电脑上安装 `mysql` ，连接自己电脑上的 `mysql` 即可，但是项目开发完毕后要上线就需要该配置，将环境的配置改为线上环境的。

![](<assets/1740015943072.png>)

来回的修改配置会很麻烦，而 **`SpringBoot` 给开发者提供了多环境的快捷配置，需要切换环境时只需要改一个配置即可。**不同类型的配置文件多环境开发的配置都不相同，接下来对不同类型的配置文件进行说明

#### 2.4.1 yaml 文件

在 `application.yml` 中使用 `---` 来分割不同的配置，内容如下

**连接符 --- 作用：**单文件中可以通过`---`实现多文件的效果。

**配置多个环境并设置启动环境：**

```
#设置启用的环境
spring:
  profiles:
    active: dev  #表示使用的是开发环境的配置
 
---
#开发
spring:
  profiles: dev
server:
  port: 80
---
#生产
spring:
  profiles: pro
server:
  port: 81
---
#测试
spring:
  profiles: test
server:
  port: 82
---
```

上面配置中 `spring.profiles` 是用来给不同的配置起名字的。

**注意：**

在上面配置中给不同配置起名字的 **`spring.profiles` 配置项已经过时**。最新用来起名字的配置项是

```
#开发
spring:
  config:
    activate:
      on-profile: dev
```

#### 2.4.2 properties 文件

`properties` 类型的配置文件配置多环境需要**另外定义不同的配置文件**

*   `application-dev.properties` 是开发环境的配置文件。我们在该文件中配置端口号为 `80`
    
    ```
    server.port=80
    
    
    ```
    
*   `application-test.properties` 是测试环境的配置文件。我们在该文件中配置端口号为 `81`
    
    ```
    server.port=81
    
    
    ```
    
*   `application-pro.properties` 是生产环境的配置文件。我们在该文件中配置端口号为 `82`
    
    ```
    server.port=82
    
    
    ```
    

`SpringBoot` 只会默认加载名为 `application.properties` 的配置文件，所以需要在 **`application.properties` 配置文件中设置启用哪个配置文件**，配置如下:

```
spring.profiles.active=pro


```

#### 2.4.3 命令行启动参数设置，修改端口、环境

使用 `SpringBoot` 开发的程序以后都是打成 `jar` 包，通过 `java -jar xxx.jar` 的方式启动服务的。

**命令行 jar 包切换环境：**

注意： **命令行启动参数都是临时更改，不影响源代码。**

我们知道 `jar` 包其实就是一个压缩包，可以解压缩，然后修改配置，最后再打成 jar 包就可以了。这种方式显然有点麻烦。

而 `SpringBoot` 提供了在运行 `jar` 时设置开启指定的环境的方式，如下

```
java –jar xxx.jar –-spring.profiles.active=test


```

**命令行 jar 包修改端口：**

那么这种方式能不能临时修改端口号呢？也是可以的，可以通过如下方式

```
java –jar xxx.jar –-server.port=88


```

**命令行 jar 包同时修改环境和端口：**

当然也可以同时设置多个配置，比如即指定启用哪个环境配置，又临时指定端口，如下

```
java –jar springboot.jar –-server.port=88 –-spring.profiles.active=test


```

大家进行测试后就会发现命令行设置的端口号优先级高（也就是使用的是命令行设置的端口号），**配置的优先级其实 `SpringBoot` 官网已经进行了说明** :

```
https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config


```

进入上面网站后会看到如下页面

![](<assets/1740015943374.png>)

如果使用了多种方式配合同一个配置项，优先级高的生效。

#### **2.4.4 maven 和 springboot 多环境开发兼容**

Maven 和 springboot 都有 profile 环境配置，在实际开发中，应该 maven 为主，springboot 为辅。springboot 读取 maven 多环境配置。

**pom.xml**

解析 resources 资源的插件 maven-resources-plugin

![](<assets/1740015943626.png>)

配置多环境

![](<assets/1740015943816.png>)

**application.yml**

![](<assets/1740015944160.png>)

### 2.5 配置文件分类

#### 2.5.1 概述

![](<assets/1740015944343.png>)

有这样的场景，我们开发完毕后需要测试人员进行测试。

**由于测试环境和开发环境的很多配置都不相同，所以测试人员在运行我们的工程时需要临时修改很多配置**，如下

```
java –jar springboot.jar –-spring.profiles.active=test --server.port=85 --server.servlet.context-path=/heima --server.tomcat.connection-timeout=-1 …… …… …… …… ……


```

针对这种情况，`SpringBoot` 定义了配置文件不同的放置的位置；而放在不同位置的优先级时不同的。

`SpringBoot` 中 4 级配置文件放置位置：

*   1 级：classpath：application.yml
*   2 级：classpath：config/application.yml
*   3 级：file ：application.yml
*   4 级：file ：config/application.yml

**说明：**

*   **级别越高优先级越高，file:config 优先级最高**
*   **一二级 classpath 是为开发用的，三四级 file 是为打包后设置通用属性用的。**
*   resources 下 config 目录里放配置文件
*   classpath: 是类路径，在 resources 下，优先级高。
*   file: 是在包目录下，这个目录下创建 config 文件夹里放配置文件优先级最高。

#### 2.5.1 代码演示

在这里我们只演示不同级别配置文件放置位置的优先级。

**2.5.1.1 环境准备**

创建一个名为 `springboot_06_config_file` 的 `SpringBoot` 工程，目录结构如下

![](<assets/1740015944566.png>)

在 `resources` 下创建一个名为 `config` 的目录，在该目录中创建 `application.yml` 配置文件，而在该配置文件中将端口号设置为 `81`，内容如下

```
server:
  port: 81
```

而在 `resources` 下创建的 `application.yml` 配置文件中并将端口号设置为 `80`，内容如下

```
server:
  port: 80
```

**2.5.1.2 验证 1 级和 2 级的优先级**

运行启动引导类，可以在控制台看到如下日志信息

![](<assets/1740015944763.png>)

通过这个结果可以得出**类路径下的 `config` 下的配置文件优先于类路径下的配置文件。**

**2.5.1.3 验证 2 级和 4 级的优先级**

**结论：file： `config` 下的配置文件优先于类路径下的配置文件。**

要验证 4 级，按照以下步骤完成

*   将工程打成 `jar` 包
    
    点击工程的 `package` 来打 `jar` 包
    
    ![](<assets/1740015944944.png>)
    
*   在硬盘上找到 `jar` 包所在位置
    
    ![](<assets/1740015945205.png>)
    
*   在 `jar` 包所在位置创建 `config` 文件夹，在该文件夹下创建 `application.yml` 配置文件，而在该配合文件中将端口号设置为 `82`
    
*   在命令行使用以下命令运行程序
    
    ```
    java -jar springboot_06_config_file-0.0.1-SNAPSHOT.jar
    
    
    ```
    
    运行后日志信息如下
    
    ![](<assets/1740015945450.png>)
    
    通过这个结果可以得出 **file： `config` 下的配置文件优先于类路径下的配置文件。**
    

**注意：**

SpringBoot 2.5.0 版本存在一个 bug，我们在使用这个版本时，需要在 `jar` 所在位置的 `config` 目录下创建一个任意名称的文件夹

## 3，SpringBoot 整合 junit

### **3.0 回顾 `Spring` 整合 `junit`**

**回顾 `Spring` 整合 `junit`**

```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class UserServiceTest {
    
    @Autowired
    private BookService bookService;
    
    @Test
    public void testSave(){
        bookService.save();
    }
}
```

使用 `@RunWith` 注解指定运行器，使用 `@ContextConfiguration` 注解来指定配置类或者配置文件。

而 **`SpringBoot` 整合 `junit` 特别简单**，分为以下三步完成

*   在**测试类上添加 `SpringBootTest` 注解**
*   使用 `@Autowired` 注入要测试的资源
*   定义测试方法进行测试

在 springboot 中，@RunWith 不用特别写，springboot 内部自己加上了。@ContextConfiguration 加载 SpringConfig 也不用特别写了，因为 springboot 的引导类起到了配置类的作用，这个类会把同位置下所有包扫描一遍，所以 @Component 的类才能加载成 bean，测试类也就不用写 @ContextConfiguration 加载 SpringConfig 了。

### 3.1 环境准备

创建一个名为 `springboot_07_test` 的 `SpringBoot` 工程，工程目录结构如下

![](<assets/1740015945780.png>)

在 `com.itheima.service` 下创建 **`BookService` 接口**，内容如下

```
public interface BookService {
    public void save();
}
```

在 `com.itheima.service.impl` 包写创建一个 `BookServiceImpl` 类，使其实现 `BookService` 接口，内容如下

```
@Service
public class BookServiceImpl implements BookService {
    @Override
    public void save() {
        System.out.println("book service is running ...");
    }
}
```

### 3.2 编写测试类,@SpringBootTest

实际上，整合很简单，只有一步，包都不用再导， 直接给引导类同包下的测试类注解 @SpringBootTest

在 **`test/java` 下**创建 `com.itheima` 包，在该包下创建测试类，将 `BookService` 注入到该测试类中

```
@SpringBootTest
class Springboot07TestApplicationTests {
 
    @Autowired
    private BookService bookService;
 
    @Test
    public void save() {
        bookService.save();
    }
}
```

**注意：**这里的**引导类所在包必须是测试类所在包及其子包。**

例如：

*   引导类所在包是 `com.itheima`
*   测试类所在包是 `com.itheima`

**如果不满足**这个要求的话，就需要在使用 `@SpringBootTest` 注解时，使用 `classes` 属性**指定引导类的字节码对象**。如 **`@SpringBootTest(classes = Springboot07TestApplication.class)`**

## 4，SpringBoot 整合 mybatis

### 4.1 回顾 Spring 整合 Mybatis

`Spring` 整合 `Mybatis` 需要定义很多配置类

*   `SpringConfig` 配置类
    
    *   导入 `JdbcConfig` 配置类
        
    *   导入 `MybatisConfig` 配置类
        
        ```
        @Configuration
        @ComponentScan("com.itheima")
        @PropertySource("classpath:jdbc.properties")
        @Import({JdbcConfig.class,MyBatisConfig.class})
        public class SpringConfig {
        }
        ```
        
*   `JdbcConfig` 配置类
    
    *   定义数据源（加载 properties 配置项：driver、url、username、password）
        
        ```
        public class JdbcConfig {
            @Value("${jdbc.driver}")
            private String driver;
            @Value("${jdbc.url}")
            private String url;
            @Value("${jdbc.username}")
            private String userName;
            @Value("${jdbc.password}")
            private String password;
         
            @Bean
            public DataSource getDataSource(){
                DruidDataSource ds = new DruidDataSource();
                ds.setDriverClassName(driver);
                ds.setUrl(url);
                ds.setUsername(userName);
                ds.setPassword(password);
                return ds;
            }
        }
        ```
        
*   `MybatisConfig` 配置类
    
    *   定义 `SqlSessionFactoryBean`
        
    *   定义映射配置
        
        ```
        public class MybatisConfig {
            @Bean
            public SqlSessionFactoryBean sqlSessionFactoryBean(DataSource dataSource){
                SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
                factoryBean.setDataSource(dataSource);
                factoryBean.setTypeAliasesPackage("package1.pojo");
                return factoryBean;
            }
            @Bean
            public MapperScannerConfigurer mapperScannerConfigurer(){
                MapperScannerConfigurer msc=new MapperScannerConfigurer();
                msc.setBasePackage("package1.dao");
                return msc;
            }
        }
        ```
        

### 4.2 SpringBoot 整合 mybatis

**两个关键步骤：@Mapper 和 yml 配置数据源**

*   在 dao 接口 **@Mapper 或引导类 @MapperScan**，类同于 ssm 中 MybatisConfig 的 mapper 扫描包。
*   导入 druid 依赖、在 **application.yml 中**配置数据源 **dataSource**。

#### **4.2.1 创建模块**

*   创建新模块，选择 `Spring Initializr`，并配置模块相关基础信息

![](<assets/1740015945985.png>)

*   选择当前模块需要使用的技术集（MyBatis、MySQL）。如果需要写 controller 层则再添加 Web 里的 Spring Web，或者在 pom.xml 导入依赖 spring-boot-starter-web
    
    ![](<assets/1740015946223.png>)
    

#### **4.2.2 定义实体类**

在 `com.itheima.domain` 包下定义实体类 `Book`，内容如下

```
public class Book {
    private Integer id;
    private String name;
    private String type;
    private String description;
    
    //自己生成setter and  getter，别忘了，别忘了，别忘了，别忘了，别忘了，别忘了，
    
    //toString
}
```

#### **4.2.3 定义 dao 接口**

在 `com.itheima.dao` 包下定义 `BookDao` 接口，内容如下

或者可以在**引导类扫描 @MapperScan**

```
//@Repository    //BookDao是接口无实现类，不定义成bean也能在Service自动注入
@Mapper
public interface BookDao {
    @Select("select * from tbl_book where id = #{id}")
    public Book getById(Integer id);
}
```

#### **4.2.4 定义测试类**

在 `test/java` 下定义包 `com.itheima` ，在该包下测试类，内容如下

```
@SpringBootTest
class Springboot08MybatisApplicationTests {
 
	@Autowired
	private BookDao bookDao;
 
	@Test
	void testGetById() {
		Book book = bookDao.getById(1);
		System.out.println(book);
	}
}
```

#### **4.2.5 编写配置**

我们代码中并没有指定连接哪儿个数据库，用户名是什么，密码是什么。所以这部分需要在 `SpringBoot` 的配置文件中进行配合。

 **`application.yml`** 

```
spring:
  datasource:
    #mysql6以后必须driver-class-name中间加cj，url设置时区，否则会报错
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    username: root
    password: root
```

**com.mysql.jdbc.Driver 与 com.mysql.cj.jdbc.Driver 的区别：**

*   JDBC 连接 Mysql5 需用 com.mysql.jdbc.Driver
*   JDBC 连接 Mysql6 需用 com.mysql.cj.jdbc.Driver，同时 **url 需要指定时区 serverTimezone。**
*   设定时区时，serverTimezone=UTC 比中国时间早 8 个小时，若在中国，可设置 serverTimezone=Asia/Shanghai

#### **4.2.6 代码测试，根据 id 查书籍**

运行测试方法，我们会看到如下错误信息

![](<assets/1740015946505.png>)

错误信息显示在 `Spring` 容器中没有 `BookDao` 类型的 `bean`。为什么会出现这种情况呢？

原因是 **`Mybatis` 会扫描接口并创建接口的代码对象交给 `Spring` 管理**，但是现在并没有告诉 `Mybatis` 哪个是 `dao` 接口。而我们要解决这个问题需要**在`BookDao` 接口上使用 `@Mapper`**。

**`BookDao` 接口加上 @Mapper 注解**或者引导类加 @MapperScan：

```
//@Mapper替代MybatisConfig里的MapperScannerConfigurer方法。
//即Mybatis核心配置文件的<mappers>标签里的扫描mapper包
@Mapper    
public interface BookDao {
    @Select("select * from tbl_book where id = #{id}")
    public Book getById(Integer id);
}
```

**注意：**

`SpringBoot` 版本低于 2.4.3(不含)，Mysql 驱动版本大于 8.0 时，需要在 url 连接串中配置时区 `jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC`，或在 MySQL 数据库端配置时区解决此问题

#### **4.2.7 配置内置数据源、Druid 数据源**

application.yml

```
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db
    username: root
    password: root
```

此时运行引导类，项目就已经能跑起来了，不过实际开发中更多的是使用 druid 数据源。

**Druid 数据源**

现在我们并没有指定数据源，`SpringBoot` 有默认的数据源，我们也可以指定使用 `Druid` 数据源，按照以下步骤实现

*   **导入 `Druid` 依赖**
    
    ```
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.1.16</version>
    </dependency>
    ```
    
*   **在 `application.yml` 配置文件配置**
    
    可以通过 `spring.datasource.type` 来配置使用什么数据源。配置文件内容可以改进为
    
    ```
    spring:
      datasource:
    #druid建议driver设成com.mysql.cj.jdbc.Driver
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://localhost:3306/ssm_db
        username: root
        password: root
        type: com.alibaba.druid.pool.DruidDataSource
    ```
    

如果 **spring-boot-starter-parent 版本是 2.4.3 以前**，在 url 后面加上? serverTimezone=UTC，否则会报错：

```
spring:
  datasource:
#druid建议driver设成com.mysql.cj.jdbc.Driver
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    username: root
    password: root
    type: com.alibaba.druid.pool.DruidDataSource
```

### 4.3 案例，整合 ssm 的书籍增删改查项目

`SpringBoot` 到这就已经学习完毕，接下来我们将学习 `SSM` 时做的三大框架整合的案例用 `SpringBoot` 来实现一下。我们完成这个案例基本是将之前做的拷贝过来，修改成 `SpringBoot` 的即可，主要从以下几部分完成

1.  pom.xml
    
    配置起步依赖，必要的资源坐标 (druid)
    
2.  application.yml
    
    设置数据源、端口等
    
3.  配置类
    
    全部删除
    
4.  dao
    
    设置 @Mapper
    
5.  测试类
    
6.  页面
    
    放置在 resources 目录下的 static 目录中
    

#### 4.3.1 创建工程

创建 `SpringBoot` 工程，在创建工程时需要**勾选 `web`、`mysql`、`mybatis`**，工程目录结构如下

![](<assets/1740015946772.png>)

由于我们工程中使用到了 `Druid` ，所以需要导入 `Druid` 的坐标

```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.16</version>
</dependency>
```

#### 4.3.2 代码拷贝、dao 注解 @Mapper、WebMvcConfigurer 添加资源处理器、拦截器

将上一节整合 ssm 的项目， `springmvc_11_page` 工程中的 `java` 代码及测试代码连同包拷贝到 `springboot_09_ssm` 工程，按照下图进行拷贝

![](<assets/1740015946961.png>)

**需要修改的内容如下：**

*   `Springmvc_11_page` 中 `config` 包下的是配置类，而 **`SpringBoot` 工程不需要这些配置类**，所以这些可以直接删除
    
*   **`dao` 包下的接口**上在拷贝到 `springboot_09-ssm` 工程中需要在接口中添加 **`@Mapper` 注解**
    
*   `BookServiceTest` **测试类**需要加 @SpringBootTest 注解，改成 `SpringBoot` 整合 `junit` 的
    
    ```
    @SpringBootTest
    public class BookServiceTest {
     
        @Autowired
        private BookService bookService;
     
        @Test
        public void testGetById(){
            Book book = bookService.getById(2);
            System.out.println(book);
        }
     
        @Test
        public void testGetAll(){
            List<Book> all = bookService.getAll();
            System.out.println(all);
        }
    }
    ```
    

另外还需要修改配置文件。

controller 包下拦截器 interceptor 失效，因为以前需要 SpringMvcConfig 实现 WebMvcConfigurer 接口；

因为没有了 ServletConfig，springmvc 也就不会拦截资源，也就不用放行静态资源了

**SpringMvcConfig 回顾:**

```
@Configuration
//package1.config主要为了扫描项目拦截器类、ServletContainersInitConfig类
@ComponentScan({"package1.controller","package1.config"})
@EnableWebMvc
public class SpringMvcConfig implements WebMvcConfigurer {
    @Autowired
    private ProjectInterception projectInterception;
    //添加拦截器，配置本地资源映射路径，在访问A（虚拟的）的时候，需要到B（实际的）的位置去访问。
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(projectInterception).addPathPatterns("/books","/books/*");
        //如果只拦截/books，发送http://localhost/books/100后会发现拦截器没有被执行
        //registry.addInterceptor(projectInterceptor).addPathPatterns("/books");
    }
    //添加资源处理器
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
        registry.addResourceHandler("/js/**").addResourceLocations("/js/");
        registry.addResourceHandler("/css/**").addResourceLocations("/css/");
        registry.addResourceHandler("/plugins/**").addResourceLocations("/plugins/");
 
    }
}
```

#### 4.3.3 yml 配置端口、数据源

在 `application.yml` 配置文件中需要配置如下内容

*   服务的端口号
*   连接数据库的信息
*   数据源

```
server:
  port: 80
 
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?servierTimezone=UTC
    username: root
    password: root
```

#### 4.3.4 静态资源

在 `SpringBoot` 程序中是没有 `webapp` 目录的，那么在 `SpringBoot` 程序中静态资源需要放在什么位置呢？

**静态资源需要放在 `resources` 下的 `static` 下**，如下图所示

![](<assets/1740015947375.png>)

另外配置一个 index.html，实现直接 [http://localhost/](http://localhost/ "http://localhost/") 即可访问 books 列表：

![](<assets/1740015947607.png>)

然后**运行引导类**，先输入 [http://localhost/books](http://localhost/books "http://localhost/books") 测试后端，再用 postman 测试所有功能，完成。

![](<assets/1740015947945.png>)

### 4.4 SpringBoot 整合 MyBatis-Plus 回顾

**具体整合看下面文章的入门案例：**

[MyBatisPlus 基础_vincewm 的博客 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/126470047?spm=1001.2014.3001.5501 "MyBatisPlus基础_vincewm的博客-CSDN博客")

**步骤①**：导入对应的 starter

```
<!--mybatis和mysql依赖勾选自动生成-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3</version>
</dependency>
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
        <version>1.2.6</version>
    </dependency>
</dependencies>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
```

关于这个坐标，此处要说明一点，之前我们看的 starter 都是 spring-boot-starter-？？？，也就是说都是下面的格式

```
Spring-boot-start-***


```

而 MyBatis 与 MyBatisPlus 这两个坐标的名字书写比较特殊，是第三方技术名称在前，boot 和 starter 在后。

**springboot 依赖命名规范：**

<table><thead><tr><th>starter 所属</th><th>命名规则</th><th>示例</th></tr></thead><tbody><tr><td>官方提供</td><td>spring-boot-starter - 技术名称</td><td>spring-boot-starter-web spring-boot-starter-test</td></tr><tr><td>第三方提供</td><td>第三方技术名称 - spring-boot-starter</td><td>mybatis-spring-boot-starter druid-spring-boot-starter</td></tr><tr><td>第三方提供</td><td>第三方技术名称 - boot-starter（第三方技术名称过长，简化命名）</td><td>mybatis-plus-boot-starter</td></tr></tbody></table>

**温馨提示**

创建项目时勾选里是没有 mybatisplus 的，因为 SpringBoot 官网还未收录此坐标。

**步骤②**：**配置数据源相关信息**

```
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db
    username: root
    password: root
```

**步骤 3：映射接口（Dao）**

```
@Mapper
public interface BookDao extends BaseMapper<Book> {
}
```

核心在于 Dao 接口继承了一个 BaseMapper 的接口，这个接口中帮助开发者预定了若干个常用的 API 接口，简化了通用 API 接口的开发工作。

![](<assets/1740015948273.png>)

下面就可以写一个测试类进行测试了，此处省略。

**温馨提示**

目前数据库的表名定义规则是 tbl_模块名称，为了能和实体类相对应，需要实体类注解 @TableName("表名")，或者配置 application.yml 文件，添加如下配置即可，设置所有表名的通用前缀名。

```
//注解了lombok的@Data会自动生成getter,setter,toString方法
@Data
//一般数据库表名tbl_user，这里注解@TableName("tbl_user")，就可以对应上表名，毕竟mp的语句里没有指定表名，都是在数据库中搜索和首字母小的的实体类名对应的表的。
@TableName("user")
public class User {
    //设置主键自增策略为auto，mp默认自增策略是ASSIGN_ID，分布式、雪花算法。自增策略也可以在yml中全局配置。
    @TableId(type = IdType.AUTO)
    private Long id;
    private String name;
    //value属性起别名，select设置该字段是否参与查询，针对于一些密码等隐私数据不希望被查出来
    @TableField(value = "password",select = false)
    private String password;
    private Integer age;
 
    private String tel;
    //exist属性设置是否在数据库中存在该字段
    @TableField(exist = false)
    private String online;
    //乐观锁注解版本，需要搭配乐观拦截器
    @Version
    private Integer version;
    //逻辑删除，本质是更新，数据库内该字段默认是0，通过标记为1来判定删除。
    @TableLogic
    private Integer delete;
}
```

**或者配置 yml**

```
mybatis-plus:
  global-config:
    db-config:
      table-prefix: tbl_		#设置所有表的通用前缀名称为tbl_
```

## 5、SSMP 整合综合案例，Book 增删改查分页

**主页面**

![](<assets/1740015948539.png>)

**添加**

![](<assets/1740015948800.png>)

**删除**

![](<assets/1740015949158.png>)

**修改**

![](<assets/1740015949362.png>)

**分页**

![](<assets/1740015949571.png>)

**条件查询**

![](<assets/1740015949878.png>)

**整体案例中需要采用的技术如下：**

1.  **实体类开发**————使用 **Lombok** 快速制作实体类
    
2.  **Dao 开发**————**整合 MyBatisPlus**，制作数据层测试
    
3.  **Service 开发**————基于 MyBatisPlus 进行增量开发，制作业务层测试类
    
4.  **Controller 开发**————**基于 Restful 开发，前后端开发协议制作**，使用 PostMan 测试接口功能
    
5.  **页面开发**————**基于 VUE+ElementUI 制作，前后端联调**，页面数据处理，页面消息处理
    
    *   列表
    *   新增
    *   修改
    *   删除
    *   分页
    *   查询
6.  **项目异常处理————异常拦截器、业务和系统异常类**
    
7.  **按条件查询**————页面功能调整、Controller 修正功能、Service 修正功能
    

### 0. 模块创建

对于这个案例如果按照企业开发的形式进行应该制作后台微服务，前后端分离的开发。

![](<assets/1740015950111.png>)

这个对初学的小伙伴要求太高了，咱们简化一下。后台做单体服务器，前端不使用前后端分离的制作了。

![](<assets/1740015950368.png>)

一个服务器即充当后台服务调用，又负责前端页面展示，降低学习的门槛。

**模块构建步骤：**

**pom.xml**

```
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

**application.yml**

```
server:
  port: 80
```

### 1. 实体类开发，lombok

本案例对应的模块表结构如下：

```
-- ----------------------------
-- Table structure for tbl_book
-- ----------------------------
DROP TABLE IF EXISTS `tbl_book`;
CREATE TABLE `tbl_book`  (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `type` varchar(20) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `name` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `description` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 51 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
 
-- ----------------------------
-- Records of tbl_book
-- ----------------------------
INSERT INTO `tbl_book` VALUES (1, '计算机理论', 'Spring实战 第5版', 'Spring入门经典教程，深入理解Spring原理技术内幕');
INSERT INTO `tbl_book` VALUES (2, '计算机理论', 'Spring 5核心原理与30个类手写实战', '十年沉淀之作，手写Spring精华思想');
INSERT INTO `tbl_book` VALUES (3, '计算机理论', 'Spring 5 设计模式', '深入Spring源码剖析Spring源码中蕴含的10大设计模式');
INSERT INTO `tbl_book` VALUES (4, '计算机理论', 'Spring MVC+MyBatis开发从入门到项目实战', '全方位解析面向Web应用的轻量级框架，带你成为Spring MVC开发高手');
INSERT INTO `tbl_book` VALUES (5, '计算机理论', '轻量级Java Web企业应用实战', '源码级剖析Spring框架，适合已掌握Java基础的读者');
INSERT INTO `tbl_book` VALUES (6, '计算机理论', 'Java核心技术 卷I 基础知识（原书第11版）', 'Core Java 第11版，Jolt大奖获奖作品，针对Java SE9、10、11全面更新');
INSERT INTO `tbl_book` VALUES (7, '计算机理论', '深入理解Java虚拟机', '5个维度全面剖析JVM，大厂面试知识点全覆盖');
INSERT INTO `tbl_book` VALUES (8, '计算机理论', 'Java编程思想（第4版）', 'Java学习必读经典,殿堂级著作！赢得了全球程序员的广泛赞誉');
INSERT INTO `tbl_book` VALUES (9, '计算机理论', '零基础学Java（全彩版）', '零基础自学编程的入门图书，由浅入深，详解Java语言的编程思想和核心技术');
INSERT INTO `tbl_book` VALUES (10, '市场营销', '直播就该这么做：主播高效沟通实战指南', '成长为网红的秘密都在书中');
INSERT INTO `tbl_book` VALUES (11, '市场营销', '直播销讲实战一本通', '和秋叶一起学系列网络营销书籍');
INSERT INTO `tbl_book` VALUES (12, '市场营销', '直播带货：淘宝、天猫直播从新手到高手', '一本教你如何玩转直播的书，10堂课轻松实现带货月入3W+');
```

根据上述表结构，制作对应的实体类

**实体类**

```
public class Book {
    private Integer id;
    private String type;
    private String name;
    private String description;
}
```

实体类的开发可以自动通过工具手工生成 get/set 方法，然后覆盖 toString() 方法，方便调试，等等。不过这一套操作书写很繁琐，有对应的工具可以帮助我们简化开发，介绍一个小工具，lombok。

Lombok，一个 Java 类库，提供了一组注解，简化 POJO 实体类开发，SpringBoot 目前默认集成了 lombok 技术，并提供了对应的版本控制，所以只需要提供对应的坐标即可，在 pom.xml 中添加 lombok 的坐标。

```
<dependencies>
    <!--lombok-->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </dependency>
</dependencies>
```

使用 lombok 可以通过一个注解 @Data 完成一个实体类对应的 getter，setter，toString，equals，hashCode 等操作的快速添加

```
import lombok.Data;
@Data
public class Book {
    private Integer id;
    private String type;
    private String name;
    private String description;
}
```

到这里实体类就做好了，是不是比不使用 lombok 简化好多，这种工具在 Java 开发中还有 N 多，后面遇到了能用的实用开发技术时，在不增加各位小伙伴大量的学习时间的情况下，尽量多给大家介绍一些。

**总结**

1.  实体类制作
    
2.  使用 lombok 简化开发
    
    *   导入 lombok 无需指定版本，由 SpringBoot 提供版本
    *   @Data 注解

### 2. 数据层开发——基础 CRUD

数据层开发本次使用 MyBatisPlus 技术，数据源使用前面学习的 Druid，学都学了都用上。

**步骤①**：导入 MyBatisPlus 与 Druid 对应的 starter，当然 mysql 的驱动不能少

```
<dependencies>
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.4.3</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
        <version>1.2.6</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

 spring-boot-starter-web 和 spring-boot-starter-test 在创建 springboot 项目时已经勾选创建了：

**步骤②**：配置 druid 数据库连接相关的数据源配置

```
server:
  port: 80
 
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
      username: root
      password: root
```

**步骤③**：使用 MyBatisPlus 的标准通用接口 BaseMapper 加速开发，别忘了 @Mapper 和泛型的指定

```
@Mapper
public interface BookDao extends BaseMapper<Book> {
}
```

**步骤④**：制作测试类测试结果，这个测试类制作是个好习惯，不过在企业开发中往往都为加速开发跳过此步，且行且珍惜吧

```
package com.itheima.dao;
 
import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.itheima.domain.Book;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
 
@SpringBootTest
public class BookDaoTestCase {
 
    @Autowired
    private BookDao bookDao;
 
    @Test
    void testGetById(){
        System.out.println(bookDao.selectById(1));
    }
 
    @Test
    void testSave(){
        Book book = new Book();
        book.setType("测试数据123");
        book.setName("测试数据123");
        book.setDescription("测试数据123");
        bookDao.insert(book);
    }
 
    @Test
    void testUpdate(){
        Book book = new Book();
        book.setId(17);
        book.setType("测试数据abcdefg");
        book.setName("测试数据123");
        book.setDescription("测试数据123");
        bookDao.updateById(book);
    }
 
    @Test
    void testDelete(){
        bookDao.deleteById(16);
    }
 
    @Test
    void testGetAll(){
        bookDao.selectList(null);
    }
}
```

**配置 id 生成策略**

MyBatisPlus 技术默认的主键生成策略为雪花算法，生成的主键 ID 长度较大，和目前的数据库设定规则不相符，需要配置一下使 MyBatisPlus 使用数据库的主键生成策略。在 application.yml 中添加对应配置即可，具体如下

```
server:
  port: 80
 
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
      username: root
      password: root
 
mybatis-plus:
  global-config:
    db-config:
      table-prefix: tbl_		#设置表名通用前缀
      id-type: auto				#设置主键id字段的生成策略为参照数据库设定的策略，当前数据库设置id生成策略为自增
```

**清理控制台没用日志**

yml

```
mybatis-plus:
  global-config:
    banner: false
spring:
  main:
    banner-mode: off
```

resources 下创建 logback.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
</configuration>
```

**查看 MyBatisPlus 运行日志**

在进行数据层测试的时候，因为基础的 CRUD 操作均由 MyBatisPlus 给我们提供了，所以就出现了一个局面，开发者不需要书写 SQL 语句了，这样程序运行的时候总有一种感觉，一切的一切都是黑盒的，作为开发者我们啥也不知道就完了。如果程序正常运行还好，如果报错了，这个时候就很崩溃，你甚至都不知道从何下手，因为传递参数、封装 SQL 语句这些操作完全不是你开发出来的，所以查看执行期运行的 SQL 语句就成为当务之急。

SpringBoot 整合 MyBatisPlus 的时候充分考虑到了这点，通过配置的形式就可以查阅执行期 SQL 语句，配置如下

```
mybatis-plus:
  global-config:
    db-config:
      table-prefix: tbl_
      id-type: auto
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

再来看运行结果，此时就显示了运行期执行 SQL 的情况。

```
Creating a new SqlSession
SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@2c9a6717] was not registered for synchronization because synchronization is not active
JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@6ca30b8a] will not be managed by Spring
==>  Preparing: SELECT id,type,name,description FROM tbl_book
==> Parameters: 
<==    Columns: id, type, name, description
<==        Row: 1, 计算机理论, Spring实战 第5版, Spring入门经典教程，深入理解Spring原理技术内幕
<==        Row: 2, 计算机理论, Spring 5核心原理与30个类手写实战, 十年沉淀之作，手写Spring精华思想
<==        Row: 3, 计算机理论, Spring 5 设计模式, 深入Spring源码剖析Spring源码中蕴含的10大设计模式
<==        Row: 4, 计算机理论, Spring MVC+MyBatis开发从入门到项目实战, 全方位解析面向Web应用的轻量级框架，带你成为Spring MVC开发高手
<==        Row: 5, 计算机理论, 轻量级Java Web企业应用实战, 源码级剖析Spring框架，适合已掌握Java基础的读者
<==        Row: 6, 计算机理论, Java核心技术 卷I 基础知识（原书第11版）, Core Java 第11版，Jolt大奖获奖作品，针对Java SE9、10、11全面更新
<==        Row: 7, 计算机理论, 深入理解Java虚拟机, 5个维度全面剖析JVM，大厂面试知识点全覆盖
<==        Row: 8, 计算机理论, Java编程思想（第4版）, Java学习必读经典,殿堂级著作！赢得了全球程序员的广泛赞誉
<==        Row: 9, 计算机理论, 零基础学Java（全彩版）, 零基础自学编程的入门图书，由浅入深，详解Java语言的编程思想和核心技术
<==        Row: 10, 市场营销, 直播就该这么做：主播高效沟通实战指南, 成长为网红的秘密都在书中
<==        Row: 11, 市场营销, 直播销讲实战一本通, 和秋叶一起学系列网络营销书籍
<==        Row: 12, 市场营销, 直播带货：淘宝、天猫直播从新手到高手, 一本教你如何玩转直播的书，10堂课轻松实现带货月入3W+
<==        Row: 13, 测试类型, 测试数据, 测试描述数据
<==        Row: 14, 测试数据update, 测试数据update, 测试数据update
<==        Row: 15, -----------------, 测试数据123, 测试数据123
<==      Total: 15
```

其中清晰的标注了当前执行的 SQL 语句是什么，携带了什么参数，对应的执行结果是什么，所有信息应有尽有。

此处设置的是日志的显示形式，当前配置的是控制台输出，当然还可以由更多的选择，根据需求切换即可

![](<assets/1740015950592.png>)

**总结**

1.  手工导入 starter 坐标（2 个），mysql 驱动（1 个）
    
2.  配置数据源与 MyBatisPlus 对应的配置
    
3.  开发 Dao 接口（继承 BaseMapper）
    
4.  制作测试类测试 Dao 功能是否有效
    
5.  使用配置方式开启日志，设置日志输出方式为标准输出即可查阅 SQL 执行日志
    

### 3. 数据层开发——分页功能制作

前面仅仅是使用了 MyBatisPlus 提供的基础 CRUD 功能，实际上 MyBatisPlus 给我们提供了几乎所有的基础操作，这一节说一下如何实现数据库端的分页操作。

MyBatisPlus 提供的分页操作 API 如下：

```
@Test
void testGetPage(){
    IPage page = new Page(2,5);
    bookDao.selectPage(page, null);
    System.out.println(page.getCurrent());
    System.out.println(page.getSize());
    System.out.println(page.getTotal());
    System.out.println(page.getPages());
    System.out.println(page.getRecords());
}
```

其中 selectPage 方法需要传入一个封装分页数据的对象，可以通过 new 的形式创建这个对象，当然这个对象也是 MyBatisPlus 提供的，别选错包了。创建此对象时需要指定两个分页的基本数据

*   当前显示第几页
*   每页显示几条数据

可以通过创建 Page 对象时利用构造方法初始化这两个数据。

```
IPage page = new Page(2,5);


```

将该对象传入到查询方法 selectPage 后，可以得到查询结果，但是我们会发现当前操作查询结果返回值仍然是一个 IPage 对象，这又是怎么回事？

```
IPage page = bookDao.selectPage(page, null);


```

原来这个 IPage 对象中封装了若干个数据，而查询的结果作为 IPage 对象封装的一个数据存在的，可以理解为查询结果得到后，又塞到了这个 IPage 对象中，其实还是为了高度的封装，一个 IPage 描述了分页所有的信息。下面 5 个操作就是 IPage 对象中封装的所有信息了。

```
@Test
void testGetPage(){
    IPage page = new Page(2,5);
    bookDao.selectPage(page, null);
    System.out.println(page.getCurrent());		//当前页码值
    System.out.println(page.getSize());			//每页显示数
    System.out.println(page.getTotal());		//数据总量
    System.out.println(page.getPages());		//总页数
    System.out.println(page.getRecords());		//详细数据
}
```

到这里就知道这些数据如何获取了，但是当你去执行这个操作时，你会发现并不像我们分析的这样，实际上这个分页功能当前是无效的。为什么这样呢？这个要源于 MyBatisPlus 的内部机制。

对于 MySQL 的分页操作使用 limit 关键字进行，而并不是所有的数据库都使用 limit 关键字实现的，这个时候 MyBatisPlus 为了制作的兼容性强，将分页操作设置为基础查询操作的升级版，你可以理解为 IPhone6 与 IPhone6S-PLUS 的关系。

基础操作中有查询全部的功能，而在这个基础上只需要升级一下（PLUS）就可以得到分页操作。所以 MyBatisPlus 将分页操作做成了一个开关，你用分页功能就把开关开启，不用就不需要开启这个开关。而我们现在没有开启这个开关，所以分页操作是没有的。这个开关是通过 MyBatisPlus 的拦截器的形式存在的，其中的原理这里不分析了，有兴趣的小伙伴可以学习 MyBatisPlus 这门课程进行详细解读。具体设置方式如下：

**定义 MyBatisPlus 拦截器并将其设置为 Spring 管控的 bean**

```
@Configuration
public class MPConfig {
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return interceptor;
    }
}
```

上述代码第一行是创建 MyBatisPlus 的拦截器栈，这个时候拦截器栈中没有具体的拦截器，第二行是初始化了分页拦截器，并添加到拦截器栈中。如果后期开发其他功能，需要添加全新的拦截器，按照第二行的格式继续 add 进去新的拦截器就可以了。

**总结**

1.  使用 IPage 封装分页数据
2.  分页操作依赖 MyBatisPlus 分页拦截器实现功能
3.  借助 MyBatisPlus 日志查阅执行 SQL 语句

### 4. 数据层开发——条件查询功能制作

除了分页功能，MyBatisPlus 还提供有强大的条件查询功能。以往我们写条件查询要自己动态拼写复杂的 SQL 语句，现在简单了，MyBatisPlus 将这些操作都制作成 API 接口，调用一个又一个的方法就可以实现各种条件的拼装。这里给大家普及一下基本格式，详细的操作还是到 MyBatisPlus 的课程中查阅吧。

下面的操作就是执行一个模糊匹配对应的操作，由 like 条件书写变为了 like 方法的调用。

```
@Test
void testGetBy(){
    QueryWrapper<Book> qw = new QueryWrapper<>();
    qw.like("name","Spring");
    bookDao.selectList(qw);
}
```

其中第一句 QueryWrapper 对象是一个用于封装查询条件的对象，该对象可以动态使用 API 调用的方法添加条件，最终转化成对应的 SQL 语句。第二句就是一个条件了，需要什么条件，使用 QueryWapper 对象直接调用对应操作即可。比如做大于小于关系，就可以使用 lt 或 gt 方法，等于使用 eq 方法，等等，此处不做更多的解释了。

这组 API 使用还是比较简单的，但是关于属性字段名的书写存在着安全隐患，比如查询字段 name，当前是以字符串的形态书写的，万一写错，编译器还没有办法发现，只能将问题抛到运行器通过异常堆栈告诉开发者，不太友好。

MyBatisPlus 针对字段检查进行了功能升级，全面支持 Lambda 表达式，就有了下面这组 API。由 QueryWrapper 对象升级为 LambdaQueryWrapper 对象，这下就避免了上述问题的出现。

```
@Test
void testGetBy2(){
    String name = "1";
    LambdaQueryWrapper<Book> lqw = new LambdaQueryWrapper<Book>();
    lqw.like(Book::getName,name);
    bookDao.selectList(lqw);
}
```

为了便于开发者动态拼写 SQL，防止将 null 数据作为条件使用，MyBatisPlus 还提供了动态拼装 SQL 的快捷书写方式。

```
@Test
void testGetBy2(){
    String name = "1";
    LambdaQueryWrapper<Book> lqw = new LambdaQueryWrapper<Book>();
    //if(name != null) lqw.like(Book::getName,name);		//方式一：JAVA代码控制
    lqw.like(name != null,Book::getName,name);				//方式二：API接口提供控制开关
    bookDao.selectList(lqw);
}
```

其实就是个格式，没有区别。关于 MyBatisPlus 的基础操作就说到这里吧，如果这一块知识不太熟悉的小伙伴建议还是完整的学习一下 MyBatisPlus 的知识吧，这里只是蜻蜓点水的用了几个操作而已。

[MyBatisPlus 基础_vincewm 的博客 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/126470047?spm=1001.2014.3001.5501 "MyBatisPlus基础_vincewm的博客-CSDN博客")

**总结**

1.  使用 QueryWrapper 对象封装查询条件
    
2.  推荐使用 LambdaQueryWrapper 对象
    
3.  所有查询操作封装成方法调用
    
4.  查询条件支持动态条件拼装
    

### 5. 业务层开发

业务层是**组织业务逻辑功能，并根据业务需求，对数据持久层发起调用**。目标是为了组织出符合需求的业务逻辑功能，至于调不调用数据层还真不好说，有需求就调用，没有需求就不调用。

**区分数据层和业务层：**

业务层的方法名定义一定要与业务有关，例如登录操作

```
login(String username,String password);


```

而数据层的方法名定义一定与业务无关，是一定，不是可能，也不是有可能，例如根据用户名密码查询

```
selectByUserNameAndPassword(String username,String password);


```

我们在开发的时候是可以根据完成的工作不同划分成不同职能的开发团队的。比如一个哥们制作数据层，他就可以不知道业务是什么样子，拿到的需求文档要求可能是这样的

```
接口：传入用户名与密码字段，查询出对应结果，结果是单条数据
接口：传入ID字段，查询出对应结果，结果是单条数据
接口：传入离职字段，查询出对应结果，结果是多条数据
```

但是进行业务功能开发的哥们，拿到的需求文档要求差别就很大

```
接口：传入用户名与密码字段，对用户名字段做长度校验，4-15位，对密码字段做长度校验，8到24位，对密码字段做特殊字符校验，不允许存在空格，查询结果为对象。如果为null，返回BusinessException，封装消息码INFO_LOGON_USERNAME_PASSWORD_ERROR


```

你比较一下，能是一回事吗？差别太大了，所以说业务层方法定义与数据层方法定义差异化很大，只不过有些入门级的开发者手懒或者没有使用过公司相关的 ISO 标准化文档而已。

**代码实现：**

业务层接口：

```
public interface BookService {
    Boolean save(Book book);
    Boolean update(Book book);
    Boolean delete(Integer id);
    Book getById(Integer id);
    List<Book> getAll();
    IPage<Book> getPage(int currentPage,int pageSize);
}
```

业务层实现类如下，转调数据层即可：

```
@Service
public class BookServiceImpl implements BookService {
 
    @Autowired
    private BookDao bookDao;
 
    @Override
    public Boolean save(Book book) {
        return bookDao.insert(book) > 0;
    }
 
    @Override
    public Boolean update(Book book) {
        return bookDao.updateById(book) > 0;
    }
 
    @Override
    public Boolean delete(Integer id) {
        return bookDao.deleteById(id) > 0;
    }
 
    @Override
    public Book getById(Integer id) {
        return bookDao.selectById(id);
    }
 
    @Override
    public List<Book> getAll() {
        return bookDao.selectList(null);
    }
 
    @Override
    public IPage<Book> getPage(int currentPage, int pageSize) {
        IPage page = new Page(currentPage,pageSize);
        bookDao.selectPage(page,null);
        return page;
    }
}
```

别忘了对业务层接口进行测试，测试类如下：

```
@SpringBootTest
public class BookServiceTest {
    @Autowired
    private IBookService bookService;
 
    @Test
    void testGetById(){
        System.out.println(bookService.getById(4));
    }
    @Test
    void testSave(){
        Book book = new Book();
        book.setType("测试数据123");
        book.setName("测试数据123");
        book.setDescription("测试数据123");
        bookService.save(book);
    }
    @Test
    void testUpdate(){
        Book book = new Book();
        book.setId(17);
        book.setType("-----------------");
        book.setName("测试数据123");
        book.setDescription("测试数据123");
        bookService.updateById(book);
    }
    @Test
    void testDelete(){
        bookService.removeById(18);
    }
 
    @Test
    void testGetAll(){
        bookService.list();
    }
 
    @Test
    void testGetPage(){
        IPage<Book> page = new Page<Book>(2,5);
        bookService.page(page);
        System.out.println(page.getCurrent());
        System.out.println(page.getSize());
        System.out.println(page.getTotal());
        System.out.println(page.getPages());
        System.out.println(page.getRecords());
    }
 
}
```

**总结**

1.  Service 接口名称定义成业务名称，并与 Dao 接口名称进行区分
2.  制作测试类测试 Service 功能是否有效

#### 业务层快速开发（慎用）

其实 MyBatisPlus 技术不仅提供了数据层快速开发方案，业务层 MyBatisPlus 也给了一个通用接口，个人观点不推荐使用，凑合能用吧，其实就是一个封装 + 继承的思想，代码给出，实际开发慎用。

业务层接口快速开发

```
public interface IBookService extends IService<Book> {
    //添加非通用操作API接口
}
```

业务层接口实现类快速开发，关注继承的类需要传入两个泛型，一个是数据层接口，另一个是实体类。

```
@Service
public class BookServiceImpl extends ServiceImpl<BookDao, Book> implements IBookService {
    @Autowired
    private BookDao bookDao;
	//添加非通用操作API
}
```

如果感觉 MyBatisPlus 提供的功能不足以支撑你的使用需要（其实是一定不能支撑的，因为需求不可能是通用的），在原始接口基础上接着定义新的 API 接口就行了，此处不再说太多了，就是自定义自己的操作了，但是不要和已有的 API 接口名冲突即可。

**总结**

1.  使用通用接口（ISerivce）快速开发 Service
2.  使用通用实现类（ServiceImpl<M,T>）快速开发 ServiceImpl
3.  可以在通用接口基础上做功能重载或功能追加
4.  注意重载时不要覆盖原始操作，避免原始提供的功能丢失

### 6. 表现层开发

终于做到表现层了，做了这么多都是基础工作。其实你现在回头看看，哪里还有什么 SpringBoot 的影子？前面 1,2 步就搞完了。继续完成表现层制作吧，咱们表现层的开发使用基于 Restful 的表现层接口开发，功能测试通过 Postman 工具进行。

表现层接口如下:

```
@RestController
@RequestMapping("/books")
public class BookController2 {
 
    @Autowired
    private IBookService bookService;
 
    @GetMapping
    public List<Book> getAll(){
        return bookService.list();
    }
 
    @PostMapping
    public Boolean save(@RequestBody Book book){
        return bookService.save(book);
    }
 
    @PutMapping
    public Boolean update(@RequestBody Book book){
        return bookService.modify(book);
    }
 
    @DeleteMapping("{id}")
    public Boolean delete(@PathVariable Integer id){
        return bookService.delete(id);
    }
 
    @GetMapping("{id}")
    public Book getById(@PathVariable Integer id){
        return bookService.getById(id);
    }
    //当前页码和每页数量由前端传
    @GetMapping("{currentPage}/{pageSize}")
    public IPage<Book> getPage(@PathVariable int currentPage,@PathVariable int pageSize){
        return bookService.getPage(currentPage,pageSize);
    }
}
```

在使用 Postman 测试时关注提交类型，对应上即可，不然就会报 405 的错误码了。

**普通 GET 请求**

![](<assets/1740015950847.png>)

**PUT 请求传递 json 数据，后台实用 @RequestBody 接收数据**

![](<assets/1740015951152.png>)

**GET 请求传递路径变量，后台实用 @PathVariable 接收数据**

![](<assets/1740015951538.png>)

**总结**

1.  基于 Restful 制作表现层接口
    
    *   新增：POST
    *   删除：DELETE
    *   修改：PUT
    *   查询：GET
2.  接收参数
    
    *   实体数据：@RequestBody
    *   路径变量：@PathVariable

### 7. 表现层消息一致性处理

目前我们通过 Postman 测试后业务层接口功能是通的，但是这样的结果给到前端开发者会出现一个小问题。不同的操作结果所展示的数据格式差异化严重。

**增删改操作结果**

```
true


```

**查询单个数据操作结果**

```
{
    "id": 1,
    "type": "计算机理论",
    "name": "Spring实战 第5版",
    "description": "Spring入门经典教程"
}
```

**查询全部数据操作结果**

```
[
    {
        "id": 1,
        "type": "计算机理论",
        "name": "Spring实战 第5版",
        "description": "Spring入门经典教程"
    },
    {
        "id": 2,
        "type": "计算机理论",
        "name": "Spring 5核心原理与30个类手写实战",
        "description": "十年沉淀之作"
    }
]
```

每种不同操作返回的数据格式都不一样，而且还不知道以后还会有什么格式，这样的结果让前端人员看了是很容易让人崩溃的，必须将所有操作的操作结果数据格式统一起来，需要设计表现层返回结果的模型类，用于后端与前端进行数据格式统一，也称为**前后端数据协议**

```
@Data
public class R {
    //完整形态应该把flag改integer型code，加上String的错误信息msg，再创建一个code类定义所有成功失败的码，
    private Boolean flag;
    private Object data;
}
```

其中 flag 用于标识操作是否成功，data 用于封装操作数据，现在的数据格式就变了

```
{
    "flag": true,
    "data":{
        "id": 1,
        "type": "计算机理论",
        "name": "Spring实战 第5版",
        "description": "Spring入门经典教程"
    }
}
```

表现层开发格式也需要转换一下

![](<assets/1740015951801.png>)

![](<assets/1740015952194.png>)

![](<assets/1740015952424.png>)

结果这么一折腾，全格式统一，现在后端发送给前端的数据格式就统一了，免去了不少前端解析数据的烦恼。

**总结**

1.  设计统一的返回值结果类型便于前端开发读取数据
    
2.  返回值结果类型可以根据需求自行设定，没有固定格式
    
3.  返回值结果模型类用于后端与前端进行数据格式统一，也称为前后端数据协议
    

### 8. 前后端联通性测试

后端的表现层接口开发完毕，就可以进行前端的开发了。

将前端人员开发的页面保存到 lresources 目录下的 static 目录中，建议执行 maven 的 clean 生命周期，避免缓存的问题出现。

![](<assets/1740015952651.png>)

在进行具体的功能开发之前，先做联通性的测试，通过页面发送异步提交（axios），这一步调试通过后再进行进一步的功能开发。

```
//列表
getAll() {
	axios.get("/books").then((res)=>{
		console.log(res.data);
	});
},
```

只要后台代码能够正常工作，前端能够在日志中接收到数据，就证明前后端是通的，也就可以进行下一步的功能开发了。

**总结**

1.  单体项目中页面放置在 resources/static 目录下
2.  created 钩子函数用于初始化页面时发起调用
3.  页面使用 axios 发送异步请求获取数据后确认前后端是否联通

### 9. 页面基础功能开发

#### F-1. 列表功能（非分页版）

列表功能主要操作就是加载完数据，将数据展示到页面上，此处要利用 VUE 的数据模型绑定，发送请求得到数据，然后页面上读取指定数据即可。

**页面数据模型定义**

```
data:{
	dataList: [],		//当前页要展示的列表数据
	...
},
```

异步请求获取数据

```
//列表
getAll() {
    axios.get("/books").then((res)=>{
        this.dataList = res.data.data;
    });
},
```

这样在页面加载时就可以获取到数据，并且由 VUE 将数据展示到页面上了。

总结：

1.  将查询数据返回到页面，利用前端数据绑定进行数据展示

#### F-2. 添加功能

添加功能用于收集数据的表单是通过一个弹窗展示的，因此在添加操作前首先要进行弹窗的展示，添加后隐藏弹窗即可。因为这个弹窗一直存在，因此当页面加载时首先设置这个弹窗为不可显示状态，需要展示，切换状态即可。

**默认状态**

```
data:{
	dialogFormVisible: false,	//添加表单是否可见
	...
},
```

**切换为显示状态**

```
//弹出添加窗口
handleCreate() {
	this.dialogFormVisible = true;
},
```

由于每次添加数据都是使用同一个弹窗录入数据，所以每次操作的痕迹将在下一次操作时展示出来，需要在每次操作之前清理掉上次操作的痕迹。

**定义清理数据操作**

```
//重置表单
resetForm() {
    this.formData = {};
},
```

**切换弹窗状态时清理数据**

```
//弹出添加窗口
handleCreate() {
    this.dialogFormVisible = true;
    this.resetForm();
},
```

至此准备工作完成，下面就要调用后台完成添加操作了。

**添加操作**

```
//添加
handleAdd () {
    //发送异步请求
    axios.post("/books",this.formData).then((res)=>{
        //如果操作成功，关闭弹层，显示数据
        if(res.data.flag){
            this.dialogFormVisible = false;
            this.$message.success("添加成功");
        }else {
            this.$message.error("添加失败");
        }
    }).finally(()=>{
        this.getAll();
    });
},
```

1.  将要保存的数据传递到后台，通过 post 请求的第二个参数传递 json 数据到后台
    
2.  根据返回的操作结果决定下一步操作
    
    *   如何是 true 就关闭添加窗口，显示添加成功的消息
    *   如果是 false 保留添加窗口，显示添加失败的消息
3.  无论添加是否成功，页面均进行刷新，动态加载数据（对 getAll 操作发起调用）
    

**取消添加操作**

```
//取消
cancel(){
    this.dialogFormVisible = false;
    this.$message.info("操作取消");
},
```

**总结**

1.  请求方式使用 POST 调用后台对应操作
2.  添加操作结束后动态刷新页面加载数据
3.  根据操作结果不同，显示对应的提示信息
4.  弹出添加 Div 时清除表单数据

#### F-3. 删除功能

模仿添加操作制作删除功能，差别之处在于删除操作仅传递一个待删除的数据 id 到后台即可。

**删除操作**

```
// 删除
handleDelete(row) {
    axios.delete("/books/"+row.id).then((res)=>{
        if(res.data.flag){
            this.$message.success("删除成功");
        }else{
            this.$message.error("删除失败");
        }
    }).finally(()=>{
        this.getAll();
    });
},
```

**删除操作提示信息**

```
// 删除
handleDelete(row) {
    //1.弹出提示框
    this.$confirm("此操作永久删除当前数据，是否继续？","提示",{
        type:'info'
    }).then(()=>{
        //2.做删除业务
        axios.delete("/books/"+row.id).then((res)=>{
       		if(res.data.flag){
            	this.$message.success("删除成功");
        	}else{
            	this.$message.error("删除失败");
        	}
        }).finally(()=>{
            this.getAll();
        });
    }).catch(()=>{
        //3.取消删除
        this.$message.info("取消删除操作");
    });
}，	
```

**总结**

1.  请求方式使用 Delete 调用后台对应操作
2.  删除操作需要传递当前行数据对应的 id 值到后台
3.  删除操作结束后动态刷新页面加载数据
4.  根据操作结果不同，显示对应的提示信息
5.  删除操作前弹出提示框避免误操作

#### F-4. 修改功能

修改功能可以说是列表功能、删除功能与添加功能的合体。几个相似点如下：

1.  页面也需要有一个弹窗用来加载修改的数据，这一点与添加相同，都是要弹窗
    
2.  弹出窗口中要加载待修改的数据，而数据需要通过查询得到，这一点与查询全部相同，都是要查数据
    
3.  查询操作需要将要修改的数据 id 发送到后台，这一点与删除相同，都是传递 id 到后台
    
4.  查询得到数据后需要展示到弹窗中，这一点与查询全部相同，都是要通过数据模型绑定展示数据
    
5.  修改数据时需要将被修改的数据传递到后台，这一点与添加相同，都是要传递数据
    
    所以整体上来看，修改功能就是前面几个功能的大合体
    
    **查询并展示数据**
    

```
//弹出编辑窗口
handleUpdate(row) {
    axios.get("/books/"+row.id).then((res)=>{
        if(res.data.flag){
            //展示弹层，加载数据
            this.formData = res.data.data;
            this.dialogFormVisible4Edit = true;
        }else{
            this.$message.error("数据同步失败，自动刷新");
        }
    });
},
```

**修改操作**

```
//修改
handleEdit() {
    axios.put("/books",this.formData).then((res)=>{
        //如果操作成功，关闭弹层并刷新页面
        if(res.data.flag){
            this.dialogFormVisible4Edit = false;
            this.$message.success("修改成功");
        }else {
            this.$message.error("修改失败，请重试");
        }
    }).finally(()=>{
        this.getAll();
    });
},
```

**总结**

1.  加载要修改数据通过传递当前行数据对应的 id 值到后台查询数据（同删除与查询全部）
2.  利用前端双向数据绑定将查询到的数据进行回显（同查询全部）
3.  请求方式使用 PUT 调用后台对应操作（同新增传递数据）
4.  修改操作结束后动态刷新页面加载数据（同新增）
5.  根据操作结果不同，显示对应的提示信息（同新增）

### 10. 业务消息一致性处理

目前的功能制作基本上达成了正常使用的情况，什么叫正常使用呢？也就是这个程序不出 BUG，如果我们搞一个 BUG 出来，你会发现程序马上崩溃掉。比如后台手工抛出一个异常，看看前端接收到的数据什么样子。

```
{
    "timestamp": "2021-09-15T03:27:31.038+00:00",
    "status": 500,
    "error": "Internal Server Error",
    "path": "/books"
}
```

面对这种情况，前端的同学又不会了，这又是什么格式？怎么和之前的格式不一样？

```
{
    "flag": true,
    "data":{
        "id": 1,
        "type": "计算机理论",
        "name": "Spring实战 第5版",
        "description": "Spring入门经典教程"
    }
}
```

看来不仅要对正确的操作数据格式做处理，还要对错误的操作数据格式做同样的格式处理。

首先在当前的数据结果中添加消息字段，用来兼容后台出现的操作消息。

```
@Data
public class R{
    //完整形态是flag换Integer型的状态码code
    private Boolean flag;
    private Object data;
    private String msg;		//用于封装消息
}
```

后台代码也要根据情况做处理，当前是模拟的错误。

```
@PostMapping
public R save(@RequestBody Book book) throws IOException {
    Boolean flag = bookService.insert(book);
    return new R(flag , flag ? "添加成功^_^" : "添加失败-_-!");
}
```

然后在表现层做统一的异常处理，使用 SpringMVC 提供的异常处理器做统一的异常处理。

```
@RestControllerAdvice
public class ProjectExceptionAdvice {
    @ExceptionHandler(Exception.class)
    public R doOtherException(Exception ex){
        //记录日志
        //发送消息给运维
        //发送邮件给开发人员,ex对象发送给开发人员
        ex.printStackTrace();
        return new R(false,null,"系统错误，请稍后再试！");
    }
}
```

页面上得到数据后，先判定是否有后台传递过来的消息，标志就是当前操作是否成功，如果返回操作结果 false，就读取后台传递的消息。

```
//添加
handleAdd () {
	//发送ajax请求
    axios.post("/books",this.formData).then((res)=>{
        //如果操作成功，关闭弹层，显示数据
        if(res.data.flag){
            this.dialogFormVisible = false;
            this.$message.success("添加成功");
        }else {
            this.$message.error(res.data.msg);			//消息来自于后台传递过来，而非固定内容
        }
    }).finally(()=>{
        this.getAll();
    });
},
```

**总结**

1.  使用注解 @RestControllerAdvice 定义 SpringMVC 异常处理器用来处理异常的
2.  异常处理器必须被扫描加载，否则无法生效
3.  表现层返回结果的模型类中添加消息属性用来传递消息到页面

### 11. 页面功能开发

#### F-5. 分页功能

分页功能的制作用于替换前面的查询全部，其中要使用到 elementUI 提供的分页组件。

```
<!--分页组件-->
<div class="pagination-container">
    <el-pagination
		class="pagiantion"
		@current-change="handleCurrentChange"
		:current-page="pagination.currentPage"
		:page-size="pagination.pageSize"
		layout="total, prev, pager, next, jumper"
		:total="pagination.total">
    </el-pagination>
</div>
```

为了配合分页组件，封装分页对应的数据模型。

```
data:{
	pagination: {	
		//分页相关模型数据
		currentPage: 1,	//当前页码
		pageSize:10,	//每页显示的记录数
		total:0,		//总记录数
	}
},
```

修改查询全部功能为分页查询，通过路径变量传递页码信息参数。

```
getAll() {
    axios.get("/books/"+this.pagination.currentPage+"/"+this.pagination.pageSize).then((res) => {
    });
},
```

后台提供对应的分页功能。

```
@GetMapping("/{currentPage}/{pageSize}")
public R getAll(@PathVariable Integer currentPage,@PathVariable Integer pageSize){
    IPage<Book> pageBook = bookService.getPage(currentPage, pageSize);
    return new R(null != pageBook ,pageBook);
}
```

页面根据分页操作结果读取对应数据，并进行数据模型绑定。

```
getAll() {
    axios.get("/books/"+this.pagination.currentPage+"/"+this.pagination.pageSize).then((res) => {
        this.pagination.total = res.data.data.total;
        this.pagination.currentPage = res.data.data.current;
        this.pagination.pagesize = res.data.data.size;
        this.dataList = res.data.data.records;
    });
},
```

对切换页码操作设置调用当前分页操作。

```
//切换页码
handleCurrentChange(currentPage) {
    this.pagination.currentPage = currentPage;
    this.getAll();
},
```

**总结**

1.  使用 el 分页组件
2.  定义分页组件绑定的数据模型
3.  异步调用获取分页数据
4.  分页数据页面回显

#### F-6. 删除功能维护

由于使用了分页功能，当最后一页只有一条数据时，删除操作就会出现 BUG，最后一页无数据但是独立展示，对分页查询功能进行后台功能维护，如果当前页码值大于最大页码值，重新执行查询。其实这个问题解决方案很多，这里给出比较简单的一种处理方案。

```
@GetMapping("{currentPage}/{pageSize}")
public R getPage(@PathVariable int currentPage,@PathVariable int pageSize){
    IPage<Book> page = bookService.getPage(currentPage, pageSize);
    //如果当前页码值大于了总页码值，那么重新执行查询操作，使用最大页码值作为当前页码值
    if( currentPage > page.getPages()){
        page = bookService.getPage((int)page.getPages(), pageSize);
    }
    return new R(true, page);
}
```

#### F-7. 条件查询功能

最后一个功能来做条件查询，其实条件查询可以理解为分页查询的时候除了携带分页数据再多带几个数据的查询。这些多带的数据就是查询条件。比较一下不带条件的分页查询与带条件的分页查询差别之处，这个功能就好做了

*   页面封装的数据：带不带条件影响的仅仅是一次性传递到后台的数据总量，由传递 2 个分页相关数据转换成 2 个分页数据加若干个条件
    
*   后台查询功能：查询时由不带条件，转换成带条件，反正不带条件的时候查询条件对象使用的是 null，现在换成具体条件，差别不大
    
*   查询结果：不管带不带条件，出来的数据只是有数量上的差别，其他都差别，这个可以忽略
    
    经过上述分析，看来需要在页面发送请求的格式方面做一定的修改，后台的调用数据层操作时发送修改，其他没有区别。
    
    页面发送请求时，两个分页数据仍然使用路径变量，其他条件采用动态拼装 url 参数的形式传递。
    
    **页面封装查询条件字段**
    
    ```
    pagination: {		
    //分页相关模型数据
    	currentPage: 1,		//当前页码
    	pageSize:10,		//每页显示的记录数
    	total:0,			//总记录数
    	name: "",
    	type: "",
    	description: ""
    },
    ```
    
    页面添加查询条件字段对应的数据模型绑定名称
    
    ```
    <div>
        <el-input placeholder="图书类别" v-model="pagination.type"/>
        <el-input placeholder="图书名称" v-model="pagination.name"/>
        <el-input placeholder="图书描述" v-model="pagination.description"/>
        <el-button @click="getAll()">查询</el-button>
        <el-button type="primary" @click="handleCreate()">新建</el-button>
    </div>
    ```
    
    将查询条件组织成 url 参数，添加到请求 url 地址中，这里可以借助其他类库快速开发，当前使用手工形式拼接，降低学习要求
    
    ```
    getAll() {
        //1.获取查询条件,拼接查询条件
        param = "?name="+this.pagination.name;
        param += "&type="+this.pagination.type;
        param += "&description="+this.pagination.description;
        console.log("-----------------"+ param);
        axios.get("/books/"+this.pagination.currentPage+"/"+this.pagination.pageSize+param).then((res) => {
            this.dataList = res.data.data.records;
        });
    },
    ```
    
    后台代码中定义实体类封查询条件
    
    ```
    @GetMapping("{currentPage}/{pageSize}")
    public R getAll(@PathVariable int currentPage,@PathVariable int pageSize,Book book) {
        System.out.println("参数=====>"+book);
        IPage<Book> pageBook = bookService.getPage(currentPage,pageSize);
        return new R(null != pageBook ,pageBook);
    }
    ```
    
    对应业务层接口与实现类进行修正
    
    ```
    public interface IBookService extends IService<Book> {
        IPage<Book> getPage(Integer currentPage,Integer pageSize,Book queryBook);
    }
    ```
    
    ```
    @Service
    public class BookServiceImpl2 extends ServiceImpl<BookDao,Book> implements IBookService {
        public IPage<Book> getPage(Integer currentPage,Integer pageSize,Book queryBook){
            IPage page = new Page(currentPage,pageSize);
            LambdaQueryWrapper<Book> lqw = new LambdaQueryWrapper<Book>();
            lqw.like(Strings.isNotEmpty(queryBook.getName()),Book::getName,queryBook.getName());
            lqw.like(Strings.isNotEmpty(queryBook.getType()),Book::getType,queryBook.getType());
            lqw.like(Strings.isNotEmpty(queryBook.getDescription()),Book::getDescription,queryBook.getDescription());
            return bookDao.selectPage(page,lqw);
        }
    }
    ```
    
    页面回显数据
    
    ```
    getAll() {
        //1.获取查询条件,拼接查询条件
        param = "?name="+this.pagination.name;
        param += "&type="+this.pagination.type;
        param += "&description="+this.pagination.description;
        console.log("-----------------"+ param);
        axios.get("/books/"+this.pagination.currentPage+"/"+this.pagination.pageSize+param).then((res) => {
            this.pagination.total = res.data.data.total;
            this.pagination.currentPage = res.data.data.current;
            this.pagination.pagesize = res.data.data.size;
            this.dataList = res.data.data.records;
        });
    },
    ```
    

**总结**

1.  定义查询条件数据模型（当前封装到分页数据模型中）
2.  异步调用分页功能并通过请求参数传递数据到后台