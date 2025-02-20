---
url: https://blog.csdn.net/qq_40991313/article/details/126734671
title: SpringCloud 基础 1——远程调用、Eureka,Nacos 注册中心、Ribbon 负载均衡_hoxton.sr10-CSDN 博客
date: 2025-02-20 09:58:48
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1. 微服务介绍](#t0)

[1.0. 微服务技术栈](#t1) 

[1.1. 单体架构](#t2)

[1.2. 分布式架构](#t3)

[1.3. 微服务](#t4)

[1.4.SpringCloud、SpringCloudAlibaba、springboot 版本兼容](#t5)

[1.5. 微服务、分布式、集群的区别](#t6)

[1.6. 总结](#t7)

[2. 服务拆分和远程调用](#t8)

[2.1. 服务拆分原则](#t9)

[2.2. 服务拆分示例](#t10)

[2.2.1. 导入 Sql 语句](#t11)

[2.2.2. 导入 demo 工程](#t12)

[2.3. 远程调用案例，RestTemplate](#t13)

[2.3.1. 案例需求](#t14)

[2.3.2. 注册 RestTemplate](#t15)

[2.3.3. 实现远程调用，RestTemplete 的 getForObject() 方法](#t16)

[2.4. 提供者与消费者](#t17)

[3.Eureka 注册中心](#t18)

[3.1.Eureka 原理](#t19)

[3.2. 搭建 eureka-server，@EnableEurekaServer](#t20)

[3.3. 服务注册](#t21)

[3.3.1. 代码实现](#t22) 

[3.3.2. 启动多个 user-service 实例](#t23)

[3.4. 服务发现 / 拉取和负载均衡，@LoadBalanced](#t24)

[4.Ribbon 负载均衡](#t25)

[4.1. 负载均衡原理](#t26)

[4.2. 源码跟踪](#t27)

[4.3. 负载均衡策略](#t28)

[4.3.1. 负载均衡策略](#t29)

[4.3.2. 自定义负载均衡策略为 RandomRule](#t30)

[4.4. 饥饿加载](#t31)

[5.Nacos 注册中心](#t32)

[5.1. 认识 Nacos](#t33)

[5.1.1.Nacos 原理](#t34)

[5.1.2. 下载安装配置启动](#t35) 

[5.2. 服务注册到 nacos](#t36)

[5.3. 服务分级存储模型](#t37)

[5.3.0. 概述](#t38)

[5.3.1. 给 user-service 配置集群](#t39)

[5.3.2. 同集群优先的负载均衡](#t40)

[5.4. 权重配置](#t41)

[5.5. 环境隔离，namespace](#t42)

[5.6.Nacos 与 Eureka 的区别](#t43)

## 1. 微服务介绍

### 1.0. 微服务技术栈 

随着互联网行业的发展，对服务的要求也越来越高，服务架构也从**单体架构**逐渐演变为现在流行的**微服务架构**。

![](<assets/1740016728360.png>)

![](<assets/1740016728599.png>)

**服务集群：**一个业务需要多个服务共同完成，这些服务组成服务集群。**远程调用：RestTemplate、Feign（推荐）**

**注册中心：** 记录每个服务的 ip、端口、用途等服务信息。当一个服务需要调用另一个服务时，只需要从注册中心拉取或注册服务信息即可。**ureka、nacos（推荐）**

**配置中心：**统一管理服务集群的配置信息，更改配置可以实现对应服务的热更新。**nacos**

**服务网关：**访问权限管理、将请求路由到具体的服务并负载均衡、请求限流。**zuul、gateway（推荐）**

**分布式缓存：**降低数据库高并发压力，请求先过缓存再查询数据库

**分布式搜索：**简单、安全性高的搜索可以用数据库，复杂搜索和统计需要用分布式搜索。

**消息队列：**异步通讯，服务之间通过异步消息实现互相调用。

**分布式日志服务：**统一服务集群中的日志。

**系统监控链路追踪：**实时监控每个服务运行状态。

**持续集成：**对微服务项目进行自动化编译、打包、镜像、自动化部署。 

### 1.1. 单体架构

**单体架构**：将业务的所有功能集中在一个项目中开发，打成一个包部署。

![](<assets/1740016728905.png>)

单体架构的优缺点如下：

**优点：**

*   架构简单
*   **部署成本低（打 jar 包、部署、负载均衡就完成了）**

**缺点：**

*   耦合度高（维护困难、升级困难，不利于大项目开发）

### 1.2. 分布式架构

**分布式架构**：根据业务功能对系统做拆分，**每个业务功能模块作为独立项目开发**，称为一个**服务**。

![](<assets/1740016729224.png>)

分布式架构的优缺点：

**优点：**

*   降低服务耦合
*   有利于服务升级和拓展

**缺点：**

*   服务调用关系错综复杂，**难度大**

**思考：**

分布式架构虽然降低了服务耦合，但是服务拆分时也有很多问题需要思考：

*   服务**拆分的粒度**如何界定？每个服务对应唯一的业务能力。
*   **服务之间如何调用**？注册中心、消息队列
*   服务的调用关系如何管理？消息队列

人们需要制定一套行之有效的标准来约束分布式架构。

### 1.3. 微服务

**微服务**是一种经过良好架构设计的**分布式架构方案** 。 

分布式架构：根据业务功能对系统做拆分，每个业务功能模块作为独立项目开发，称为一个服务。 

**微服务的架构特征：**

*   **单一职责：**微服务拆分粒度更小，**每一个服务都对应唯一的业务能力**，做到单一职责
*   **自治：**团队独立、技术独立、数据独立，独立部署和交付
*   **面向服务：**服务提供**统一标准的接口**，与语言和技术无关
*   **隔离性强：**服务调用做好**隔离**、容错、降级，避免出现级联问题

![](<assets/1740016729439.png>)

微服务的上述特性其实是在给分布式架构制定一个标准，进一步降低服务之间的耦合度，提供服务的独立性和灵活性。做到高内聚，低耦合。

因此，可以认为**微服务**是一种经过良好架构设计的**分布式架构方案** 。

全球的互联网公司都在积极尝试自己的微服务落地方案。其中在 Java 领域国内最知名的是 **SpringCloud 和 Dubbo**。

### 1.4.SpringCloud、SpringCloudAlibaba、springboot 版本兼容

SpringCloud 是目前国内**使用最广泛的微服务框架**。官网地址：[Spring Cloud](https://spring.io/projects/spring-cloud "Spring Cloud")。

SpringCloud 集成了各种微服务功能组件，并基于 SpringBoot 实现了这些组件的自动装配，从而提供了良好的开箱即用体验。

其中常见的组件包括：

![](<assets/1740016729770.png>)

另外，SpringCloud 底层是依赖于 SpringBoot 的，并且有版本的兼容关系，如下：

这里使用的版本是 **Hoxton.SR10**，因此对应的 **SpringBoot 版本是 2.3.x 版本**。

**SpringCloud 和 SpringBoot：**

<table><thead><tr><th>Release Train</th><th>Boot Version</th></tr></thead><tbody><tr><td><a href="https://github.com/spring-cloud/spring-cloud-release/wiki/Spring-Cloud-2021.0-Release-Notes" title="2021.0.x">2021.0.x</a> aka Jubilee</td><td>2.6.x, 2.7.x (Starting with 2021.0.3)</td></tr><tr><td><a href="https://github.com/spring-cloud/spring-cloud-release/wiki/Spring-Cloud-2020.0-Release-Notes" title="2020.0.x">2020.0.x</a> aka Ilford</td><td>2.4.x, 2.5.x (Starting with 2020.0.3)</td></tr><tr><td><a href="https://github.com/spring-cloud/spring-cloud-release/wiki/Spring-Cloud-Hoxton-Release-Notes" title="Hoxton">Hoxton</a></td><td>2.2.x, 2.3.x (Starting with SR5)</td></tr><tr><td><a href="https://github.com/spring-projects/spring-cloud/wiki/Spring-Cloud-Greenwich-Release-Notes" title="Greenwich">Greenwich</a></td><td>2.1.x</td></tr><tr><td><a href="https://github.com/spring-projects/spring-cloud/wiki/Spring-Cloud-Finchley-Release-Notes" title="Finchley">Finchley</a></td><td>2.0.x</td></tr><tr><td><a href="https://github.com/spring-projects/spring-cloud/wiki/Spring-Cloud-Edgware-Release-Notes" title="Edgware">Edgware</a></td><td>1.5.x</td></tr><tr><td><a href="https://github.com/spring-projects/spring-cloud/wiki/Spring-Cloud-Dalston-Release-Notes" title="Dalston">Dalston</a></td><td>1.5.x</td></tr></tbody></table>

<table><thead><tr><th>Spring Cloud Alibaba Version</th><th>Spring Cloud Version</th><th>Spring Boot Version</th></tr></thead><tbody><tr><td>2021.0.4.0*</td><td>Spring Cloud 2021.0.4</td><td>2.6.11</td></tr><tr><td>2021.0.1.0</td><td>Spring Cloud 2021.0.1</td><td>2.6.3</td></tr><tr><td>2021.1</td><td>Spring Cloud 2020.0.1</td><td>2.4.2</td></tr></tbody></table>

不兼容报错：Error creating bean with name 'configurationPropertiesBeans' d

```
​
2021-03-11 12:38:15.512 ERROR 16912 --- [           main] o.s.boot.SpringApplication               : Application run failed
 
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'configurationPropertiesBeans' defined in class path resource [org/springframework/cloud/autoconfigure/ConfigurationPropertiesRebinderAutoConfiguration.class]: Post-processing of merged bean definition failed; nested exception is java.lang.IllegalStateException: Failed to introspect Class [org.springframework.cloud.context.properties.ConfigurationPropertiesBeans] from ClassLoader [sun.misc.Launcher$AppClassLoader@18b4aac2]
 
​
```

**springcloud 对比 dubbo：**

![](<assets/1740016730002.png>)

**企业需求：**

![](<assets/1740016730308.png>)

### 1.5. 微服务、分布式、集群的区别

**微服务：**一种架构设计风格。**根据业务拆分成一个一个的服务**, 彻底去掉耦合, 每一个微服务提供单个业务功能, **一个服务只做一件事**。微服务是设计层面的东西，一般考虑如何将系统从逻辑上进行拆分，也就是垂直拆分。微服务可以是分布式的，即可以将不同服务部署在不同计算机上，当然如果量小**也可以部署在单机上**。

**集群：**一种部署方式。按照微服务架构风格拆分出来的**同一个业务系统部署到多个服务器上**，多个服务干一件事情，某一个服务宕机，用户基本无感知。主要为了保证**高可用**。我们通常讲的 tomcat 集群，nginx 集群，redis 集群都是为了确保系统的稳定性。

**分布式：**一种部署方式。将一个大系统中拆分出来的**子系统分别部署到不同的服务器上**，某一个服务宕机，其他未关联服务不受影响；重启某一个服务，其他服务也不受影响；某一个服务是瓶颈，则只针对这个服务提升性能做成集群，资源利用率高。分布式是部署层面的东西。

### 1.6. 总结

*   单体架构：简单方便，高度耦合，扩展性差，适合小型项目。例如：学生管理系统
    
*   分布式架构：松耦合，扩展性好，但架构复杂，难度大。适合大型互联网项目，例如：京东、淘宝
    
*   微服务：一种良好的分布式架构方案
    
    ①优点：拆分粒度更小、服务更独立、耦合度更低
    
    ②缺点：架构非常复杂，运维、监控、部署难度提高
    
*   SpringCloud 是微服务架构的一站式解决方案，集成了各种优秀微服务功能组件
    

## 2. 服务拆分和远程调用

任何分布式架构都离不开服务的拆分。

### 2.1. 服务拆分原则

微服务拆分时的原则：

*   不同微服务，**不要重复开发相同业务**。如果两个服务间有相同业务，那一定是设计有问题。
*   微服务**数据独立**，不要访问其它微服务的数据库
*   微服务可以将自己的**业务暴露为接口**，供其它微服务调用

![](<assets/1740016730539.png>)

### 2.2. 服务拆分示例

以课前资料中的微服务 cloud-demo 为例，其结构如下：

![](<assets/1740016730851.png>)

cloud-demo：父工程，管理依赖

*   order-service：订单微服务，负责订单相关业务
*   user-service：用户微服务，负责用户相关业务

**要求：**

*   订单微服务和用户微服务都必须有**各自的数据库**，相互独立
*   订单服务和用户服务都**对外暴露 Restful 风格的接口**
*   订单服务如果需要查询用户信息，只能调用用户服务的 Restful 接口，不能查询用户数据库

**回顾 RESTful：**

**REST**（Representational State Transfer），**表现性状态转换**, 它是一种**软件架构风格。**

*   **根据 REST 风格对资源进行访问**称为 **RESTful**。

按照 REST 风格访问资源时使用请求方式区分对资源进行了何种操作。  
http://localhost/users 查询全部用户信息 GET（查询）  
http://localhost/users/1 查询指定用户信息 GET（查询）  
http://localhost/users 添加用户信息 POST（新增 / 保存）  
http://localhost/users 修改用户信息 PUT（修改 / 更新）  
http://localhost/users/1 删除用户信息 DELETE（删除）

#### **2.2.1. 导入 Sql 语句**

首先，将课前资料提供的`cloud-order.sql`和`cloud-user.sql`导入到 cloud_order、cloud_user 数据库中：

![](<assets/1740016731089.png>)

![](<assets/1740016731378.png>)

cloud-user 表中初始数据如下：

![](<assets/1740016731555.png>)

cloud-order 表中初始数据如下：

![](<assets/1740016731761.png>)

cloud-order 表中持有 cloud-user 表中的 id 字段。

#### **2.2.2. 导入 demo 工程**

用 IDEA 导入课前资料提供的 Demo：

![](<assets/1740016731996.png>)

项目结构如下：

![](<assets/1740016732187.png>)

导入后，修改 yml 中数据库密码，会在 IDEA 右下角出现弹窗：

![](<assets/1740016732502.png>)

点击弹窗，然后按下图选择：

![](<assets/1740016732766.png>)

会出现这样的菜单：

![](<assets/1740016732984.png>)

配置下项目使用的 JDK：

![](<assets/1740016733266.png>)

访问：

![](<assets/1740016733557.png>)

![](<assets/1740016733808.png>)

**推荐个 json 格式化插件：**

[https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa/related?hl=zh-CN](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa/related?hl=zh-CN "https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa/related?hl=zh-CN")

### 2.3. 远程调用案例，RestTemplate

 **了解即可，推荐 Feign 远程调用。**

**RestTemplate 存在问题：**

• 代码可读性差，编程体验不统一

**• 参数复杂 URL 难以维护**

**Feign 是一个声明式的 http 客户端**，官方地址：[GitHub - OpenFeign/feign: Feign makes writing java http clients easier](https://github.com/OpenFeign/feign "GitHub - OpenFeign/feign: Feign makes writing java http clients easier")

其作用就是帮助我们**优雅的实现 http 请求的发送**，解决上面提到的问题。

[SpringCloud 基础 2——nacos 配置、Feign、Gateway_nacos feign 配置_vincewm 的博客 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/126772669?spm=1001.2014.3001.5501 "SpringCloud基础2——nacos配置、Feign、Gateway_nacos feign配置_vincewm的博客-CSDN博客")

#### 2.3.1. 案例需求

在 order-service 服务中，有一个根据 id 查询订单的接口：

![](<assets/1740016734053.png>)

根据 id 查询订单，返回值是 Order 对象，如图：

![](<assets/1740016734365.png>)

其中的 user 为 null

在 user-service 中有一个根据 id 查询用户的接口：

![](<assets/1740016734548.png>)

查询的结果如图：

![](<assets/1740016734906.png>)

修改 order-service 中的根据 id 查询订单业务，要求在查询订单的同时，根据订单中包含的 userId 查询出用户信息，一起返回。

![](<assets/1740016735312.png>)

因此，我们需要在 order-service 中 向 user-service 发起一个 http 的请求，调用 [http://localhost:8081/user/](http://localhost:8081/user/ "http://localhost:8081/user/"){userId} 这个接口。

大概的步骤是这样的：

*   注册一个 RestTemplate 的实例到 Spring 容器
*   修改 order-service 服务中的 OrderService 类中的 queryOrderById 方法，根据 Order 对象中的 userId 查询 User
*   将查询的 User 填充到 Order 对象，一起返回

#### 2.3.2. 注册 RestTemplate

首先，我们在 order-service 服务中的 OrderApplication 启动类中，注册 RestTemplate 实例：

```
package cn.itcast.order;
 
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.web.client.RestTemplate;
 
@MapperScan("cn.itcast.order.mapper")
@SpringBootApplication
public class OrderApplication {
 
    public static void main(String[] args) {
        SpringApplication.run(OrderApplication.class, args);
    }
 
    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}
```

#### 2.3.3. 实现远程调用，RestTemplete 的 getForObject() 方法

修改 order-service 服务中的 cn.itcast.order.service 包下的 OrderService 类中的 queryOrderById 方法：

![](<assets/1740016735519.png>)

### 2.4. 提供者与消费者

在服务调用关系中，会有两个不同的角色：

**服务提供者**：一次业务中，被其它微服务调用的服务。（提供接口给其它微服务）

**服务消费者**：一次业务中，调用其它微服务的服务。（调用其它微服务提供的接口）

![](<assets/1740016735787.png>)

**思考：**

但是，服务提供者与服务消费者的角色并不是绝对的，而是相对于业务而言。

如果服务 **A 调用了服务 B，而服务 B 又调用了服务 C**，服务 B 的角色是什么？

*   对于 A 调用 B 的业务而言：A 是服务消费者，B 是服务提供者
*   对于 B 调用 C 的业务而言：B 是服务消费者，C 是服务提供者

因此，**一个服务既可以是服务提供者，也可以是服务消费者。**

## 3.Eureka 注册中心

假如我们的服务提供者 user-service 部署了多个实例，如图：

![](<assets/1740016735979.png>)

**思考：**

*   order-service 在发起远程调用的时候，该如何得知 user-service 实例的 ip 地址和端口？
*   有多个 user-service 实例地址，order-service 调用时该如何选择？
*   order-service 如何得知某个 user-service 实例是否依然健康，是不是已经宕机？

### 3.1.Eureka 原理

**注册中心：** 记录每个服务的 ip、端口、用途等服务信息。当一个服务需要调用另一个服务时，只需要从注册中心拉取或注册服务信息即可。**ureka、nacos（推荐）** 

这些问题都需要利用 SpringCloud 中的注册中心来解决，其中最广为人知的注册中心就是 Eureka，其结构如下：

![](<assets/1740016736249.png>)

**注册过程：**

1.  **服务注册：**服务实例启动后，将自己的信息注册到注册中心。
    
2.  **保存服务映射关系：**注册中心保存（服务名称 ----> 服务实例地址列表）的映射关系
    
3.  **服务拉取：**服务拉取时，根据服务名称和映射关系，拉取实例地址列表。
    
4.  **负载均衡：**服务拉取后，利用负载均衡算法从实例地址列表里选中一个实例地址；
    

**心跳检测故障：**实例每隔一段时间（默认 30 秒）向注册中心发起请求，报告自己状态；当超过一定时间没有发送心跳时，eureka-server 会认为微服务实例故障，将该实例从服务列表中剔除。

**问题 1：order-service 如何得知 user-service 实例地址？**

获取地址信息的流程如下：

*   user-service 服务实例启动后，将自己的**信息注册到 eureka-server（Eureka 服务端）**。这个叫**服务注册**
*   **eureka-server 保存服务名称到服务实例地址列表的映射关系**
*   order-service 根据服务名称，**拉取实例地址列表**。这个叫服务发现或**服务拉取**

**问题 2：order-service 如何从多个 user-service 实例中选择具体的实例？**

*   order-service 从实例列表中利用**负载均衡算法**选中一个实例地址
*   向该实例地址发起远程调用

**问题 3：order-service 如何得知某个 user-service 实例是否依然健康，是不是已经宕机？**

*   user-service 会每隔一段时间**（默认 30 秒）**向 eureka-server 发起请求，报告自己状态，称为**心跳**
*   当超过一定时间**没有发送心跳**时，eureka-server 会认为微服务实例故障，将该实例从服务列表中**剔除**
*   order-service 拉取服务时，就能将**故障实例排除了**

**注意：**一个微服务，既可以是服务提供者，又可以是服务消费者，因此 eureka 将服务注册、服务发现等功能统一封装到了 eureka-client 端

因此，接下来我们动手实践的步骤包括：

![](<assets/1740016736467.png>)

### 3.2. 搭建 eureka-server，**@EnableEurekaServer**

首先大家注册中心服务端：eureka-server，这必须是一个独立的微服务

**3.2.1. 创建 eureka-server 服务**

在 cloud-demo 父工程下，创建一个子 Maven 模块：

![](<assets/1740016736684.png>)

填写模块信息：

![](<assets/1740016737020.png>)

然后填写服务信息：

![](<assets/1740016737263.png>)

**3.2.2. 引入 eureka 依赖**

引入 SpringCloud 为 eureka 提供的 starter 依赖：

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

**3.2.3. 编写启动类，@EnableEurekaServer**

给 eureka-server 服务编写一个启动类，一定要添加一个 @EnableEurekaServer 注解，开启 eureka 的注册中心功能：

```
package cn.itcast.eureka;
 
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;
 
@SpringBootApplication
@EnableEurekaServer
public class EurekaApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaApplication.class, args);
    }
}
```

**3.2.4. 编写配置文件**

编写一个 application.yml 文件，内容如下：

```
server:
 port: 10086
spring:
 application:
 name: eureka-server
#eureka也是微服务，自己也要注册到自己上
eureka:
 client:
 service-url: 
 defaultZone: http://127.0.0.1:10086/eureka
```

**3.2.5. 启动服务**

启动微服务，然后在浏览器访问：[http://127.0.0.1:10086](http://127.0.0.1:10086 "http://127.0.0.1:10086")

看到下面结果应该是成功了：

![](<assets/1740016737523.png>)

![](<assets/1740016737832.png>)

### 3.3. 服务注册

下面，我们**将 user-service 模块注册到 eureka-server 中**去。

#### 3.3.1. 代码实现 

**1）引入依赖**

在 user-service 的 pom 文件中，引入下面的 eureka-client 依赖：

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

**2）配置文件**

在 user-service 中，修改 application.yml 文件，添加服务名称、eureka 地址：

```
spring:
 application:
 name: userservice
eureka:
 client:
 service-url:
 defaultZone: http://127.0.0.1:10086/eureka
```

[http://127.0.0.1:10086/](http://127.0.0.1:10086/ "http://127.0.0.1:10086/") ：

![](<assets/1740016738074.png>)

#### **3.3.2. 启动多个 user-service 实例**

**复制并修改启动配置** 

为了演示一个服务有多个实例的场景，我们添加一个 SpringBoot 的**启动配置，再启动一个 user-service。**

首先，复制原来的 user-service 启动配置：

![](<assets/1740016738376.png>)

**在弹出的窗口中，填写信息：**

![](<assets/1740016738597.png>)

现在，SpringBoot 窗口会出现两个 user-service 启动配置：

![](<assets/1740016738879.png>)

不过，第一个是 8081 端口，第二个是 8082 端口。

启动两个 user-service 实例：

![](<assets/1740016739223.png>)

查看 eureka-server 管理页面，可以看见 user-server 两个实例：

![](<assets/1740016739407.png>)

### 3.4. 服务发现 / 拉取和负载均衡，**@LoadBalanced**

**0）概述**

**服务拉取是基于服务名称拉取服务列**表，然后对服务列表做负载均衡。 

这就需要前面 yml 配置了服务名称：

![](<assets/1740016739672.png>)

**实际上就是两个步骤：**

先确保导入 eureka 的 client 依赖、服务在 yml 中 spring.application.name 起了名

![](<assets/1740016740031.png>)

下面，我们将 order-service 的逻辑修改：向 eureka-server 拉取 user-service 的信息，实现服务发现。

**1）引入依赖**

之前说过，服务发现、服务注册统一都封装在 eureka-client 依赖，因此这一步与服务注册时一致。

在 order-service 的 pom 文件中，引入下面的 eureka-client 依赖：

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

**2）配置文件**

服务发现也需要知道 eureka 地址，因此第二步与服务注册一致，都是配置 eureka 信息：

在 order-service 中，修改 application.yml 文件，**添加服务名称**、eureka 地址：

```
spring:
 application:
 name: orderservice
eureka:
 client:
 service-url:
 defaultZone: http://127.0.0.1:10086/eureka
```

**3）服务拉取和负载均衡，注解 @LoadBalanced**

最后，我们要去 eureka-server 中拉取 user-service 服务的实例列表，并且实现负载均衡。

不过这些动作不用我们去做，只需要添加一些注解即可。

在 order-service 的 OrderApplication 中，给 RestTemplate 这个 Bean 添加一个 **@LoadBalanced 注解：**

```
@MapperScan("cn.itcast.order.mapper")
@SpringBootApplication
public class OrderApplication {
 
    public static void main(String[] args) {
        SpringApplication.run(OrderApplication.class, args);
    }
    @Bean
    @LoadBalanced
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
 
}
```

修改 order-service 服务中的 cn.itcast.order.service 包下的 OrderService 类中的 queryOrderById 方法。**修改访问的 url 路径，用服务名代替 ip、端口：**

```
@RestController
@RequestMapping("order")
public class OrderController {
 
   @Autowired
   private OrderService orderService;
   @Autowired
   private RestTemplate restTemplate;
 
    @GetMapping("{orderId}")
    public Order queryOrderByUserId(@PathVariable("orderId") Long orderId) {
        // 根据id查询订单并返回
        Order order = orderService.queryOrderById(orderId);
        User user = restTemplate.getForObject("http://user-server/user/" + order.getUserId(), User.class);
        order.setUser(user);
        return order;
    }
}
```

spring 会自动帮助我们从 eureka-server 端，根据 userservice 这个服务名称，获取实例列表，而后完成**负载均衡**。

**测试负载均衡：**

不断刷新 order-service 请求，发现两个 user-service 实例轮询调用数据库：

![](<assets/1740016740378.png>)

![](<assets/1740016740684.png>)

## 4.Ribbon 负载均衡

### 4.1. 负载均衡原理

**SpringCloud 底层**其实是利用了一个名为 **Ribbon** 的组件，来**实现负载均衡功能**的。

![](<assets/1740016741020.png>)

**思考：**

我们发出的请求明明是 [http://userservice/user/1](http://userservice/user/1 "http://userservice/user/1")，怎么变成了 [http://localhost:8081](http://localhost:8081 "http://localhost:8081") 的呢？

**答案：**

负载均衡拦截器 LoadBalancerInterceptor

![](<assets/1740016741216.png>)

### 4.2. 源码跟踪

为什么我们只输入了 service 名称就可以访问了呢？之前还要获取 ip 和端口。

显然有人帮我们根据 service 名称，获取到了服务实例的 ip 和端口。它就是`LoadBalancerInterceptor`，这个类会在对 RestTemplate 的请求进行拦截，然后从 Eureka 根据服务 id 获取服务列表，随后利用负载均衡算法得到真实的服务地址信息，替换服务 id。

我们进行源码跟踪：

**1）LoadBalancerIntercepor**

![](<assets/1740016741467.png>)

可以看到这里的 intercept 方法，拦截了用户的 HttpRequest 请求，然后做了几件事：

*   `request.getURI()`：获取请求 uri，本例中就是 [http://user-service/user/8](http://user-service/user/8 "http://user-service/user/8")
*   `originalUri.getHost()`：获取 uri 路径的主机名，其实就是服务 id，`user-service`
*   `this.loadBalancer.execute()`：处理服务 id，和用户请求。

这里的`this.loadBalancer`是`LoadBalancerClient`类型，我们继续跟入。

**2）LoadBalancerClient**

继续跟入 execute 方法：

![](<assets/1740016741686.png>)

代码是这样的：

*   getLoadBalancer(serviceId)：根据服务 id 获取 ILoadBalancer，而 ILoadBalancer 会拿着服务 id 去 eureka 中获取服务列表并保存起来。
*   getServer(loadBalancer)：利用内置的负载均衡算法，从服务列表中选择一个。本例中，可以看到获取了 8082 端口的服务

放行后，再次访问并跟踪，发现获取的是 8081：

![](<assets/1740016741892.png>)

果然实现了负载均衡。

**3）负载均衡策略 IRule**

在刚才的代码中，可以看到获取服务使通过一个`getServer`方法来做负载均衡:

![](<assets/1740016742224.png>)

我们继续跟入：

![](<assets/1740016742323.png>)

继续跟踪源码 chooseServer 方法，发现这么一段代码：

![](<assets/1740016742568.png>)

我们看看这个 rule 是谁：

![](<assets/1740016742937.png>)

这里的 rule 默认值是一个`RoundRobinRule`，看类的介绍：

![](<assets/1740016743242.png>)

这不就是轮询的意思嘛。

到这里，整个负载均衡的流程我们就清楚了。

**4）总结**

SpringCloudRibbon 的底层采用了一个拦截器，拦截了 RestTemplate 发出的请求，对地址做了修改。用一幅图来总结一下：

![](<assets/1740016743433.png>)

**基本流程如下：**

*   拦截我们的 RestTemplate 请求 [http://userservice/user/1](http://userservice/user/1 "http://userservice/user/1")
*   RibbonLoadBalancerClient 会从请求 url 中获取服务名称，也就是 user-service
*   DynamicServerListLoadBalancer 根据 user-service 到 eureka 拉取服务列表
*   eureka 返回列表，localhost:8081、localhost:8082
*   IRule 利用内置负载均衡规则，从列表中选择一个，例如 localhost:8081
*   RibbonLoadBalancerClient 修改请求地址，用 localhost:8081 替代 userservice，得到 [http://localhost:8081/user/1](http://localhost:8081/user/1 "http://localhost:8081/user/1")，发起真实请求

### 4.3. 负载均衡策略

#### 4.3.1. 负载均衡策略

负载均衡的规则都定义在 IRule 接口中，而 IRule 有很多不同的实现类：

![](<assets/1740016743692.png>)

不同规则的含义如下：

<table><thead><tr><th><strong>内置负载均衡规则类</strong></th><th><strong>规则描述</strong></th></tr></thead><tbody><tr><td><strong>RoundRobinRule（默认）</strong></td><td>简单<strong>轮询服务列表</strong>来选择服务器。它是<strong> Ribbon 默认的负载均衡规则</strong>。</td></tr><tr><td>AvailabilityFilteringRule</td><td>对以下两种服务器进行忽略： （1）在默认情况下，这台服务器如果 3 次连接失败，这台服务器就会被设置为 “短路” 状态。短路状态将持续 30 秒，如果再次连接失败，短路的持续时间就会几何级地增加。 （2）并发数过高的服务器。如果一个服务器的并发连接数过高，配置了 AvailabilityFilteringRule 规则的客户端也会将其忽略。并发连接数的上限，可以由客户端的..ActiveConnectionsLimit 属性进行配置。</td></tr><tr><td>WeightedResponseTimeRule</td><td>为每一个服务器赋予一个权重值。服务器响应时间越长，这个服务器的权重就越小。这个规则会随机选择服务器，这个权重值会影响服务器的选择。</td></tr><tr><td><strong>ZoneAvoidanceRule</strong></td><td>以区域可用的服务器为基础进行服务器的选择。使用 Zone 对服务器进行分类，这个 Zone 可以理解为一个机房、一个机架等。而后再对 Zone 内的多个服务做轮询。</td></tr><tr><td>BestAvailableRule</td><td>忽略那些短路的服务器，并选择并发数较低的服务器。</td></tr><tr><td>RandomRule</td><td>随机选择一个可用的服务器。</td></tr><tr><td>RetryRule</td><td>重试机制的选择逻辑</td></tr></tbody></table>

默认的实现就是 ZoneAvoidanceRule，是一种轮询方案

#### 4.3.2. 自定义负载均衡策略为 RandomRule

**注意，一般用默认的负载均衡规则，不做修改。** 

通过定义 IRule 实现可以修改负载均衡规则，有两种方式：

1.  **代码方式（全局，此模块调用所有模块的策略）：**在 order-service 中的 OrderApplication 类中，定义一个新的 IRule：

```
@Bean
public IRule randomRule(){
    return new RandomRule();
}
```

1.  **配置文件方式（局部，此模块调用某些模块的策略）：**在 order-service 的 application.yml 文件中，添加新的配置也可以修改规则：

```
userservice: # 给某个微服务配置负载均衡规则，这里是userservice服务
 ribbon:
 NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule # 负载均衡规则
```

测试：

访问 localhost:8080/order/1,localhost:8080/order/2,localhost:8080/order/3,localhost:8080/order/4

发现 4 次都随机到 8081 端口的 user-server 

**注意，一般用默认的负载均衡规则，不做修改。**

### 4.4. 饥饿加载

**Ribbon 默认是采用懒加载**，即**第一次访问时才会去创建负载均衡客户端 LoadBalanceClient，缓存服务列表**，请求时间会很长。

而**饥饿加载则会在项目启动时创建**，降低第一次访问的耗时，通过下面配置开启饥饿加载：

```
ribbon:
 eager-load:
#开启饥饿加载
 enabled: true
#指定饥饿加载的服务名称
 clients: 
 - userservice
```

## 5.Nacos 注册中心

国内公司一般都推崇阿里巴巴的技术，比如注册中心，SpringCloudAlibaba 也推出了一个名为 Nacos 的注册中心。

### 5.1. 认识 Nacos

#### 5.1.1.Nacos 原理

[Nacos](https://nacos.io/ "Nacos") 是阿里巴巴的产品，现在是 [SpringCloud](https://spring.io/projects/spring-cloud "SpringCloud") 中的一个组件。相比 [Eureka](https://github.com/Netflix/eureka "Eureka") 功能更加丰富，在国内受欢迎程度较高。

![](<assets/1740016744001.png>)

**注册过程：**

1.  **服务注册：**服务实例启动后，将自己的信息注册到注册中心。
    
2.  **保存服务映射关系：**注册中心保存（服务名称 ----> 服务实例地址列表）的映射关系
    
3.  **服务拉取：**服务拉取时，根据服务名称和映射关系，拉取实例地址列表。
    
4.  **负载均衡：**服务拉取后，利用负载均衡算法从实例地址列表里选中一个实例地址；
    

**心跳检测故障：**临时实例每隔一段时间（默认 30 秒）向注册中心发起请求，报告自己状态；当超过一定时间没有发送心跳时，eureka-server 会认为微服务实例故障，将该实例从服务列表中剔除。

**主动检测故障：**nacos 会主动发请求检测非临时实例是否故障，若故障则会等待其恢复健康而不是剔除。

**配置分临时实例：**

```
spring:
 cloud:
 nacos:
 discovery:
 ephemeral: false # 设置为非临时实例
```

#### 5.1.2. 下载安装配置启动 

这里下载 1.4.1. 版本的 Nacos  

**下载页：**[Releases · alibaba/nacos · GitHub](https://github.com/alibaba/nacos/releases "Releases · alibaba/nacos · GitHub")

**下载地址：**[https://github.com/alibaba/nacos/releases/download/1.4.1/nacos-server-1.4.1.zip](https://github.com/alibaba/nacos/releases/download/1.4.1/nacos-server-1.4.1.zip "https://github.com/alibaba/nacos/releases/download/1.4.1/nacos-server-1.4.1.zip") 

**目录说明：**

![](<assets/1740016744332.png>)

*   bin：启动脚本
    
*   conf：配置文件
    

**默认端口：8848**

**![](<assets/1740016744562.png>)**

**启动：**

在 bin 目录下：

```
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-alibaba-dependencies</artifactId>
    <version>2.2.6.RELEASE</version>
    <type>pom</type>
    <scope>import</scope>
</dependency>
```

![](<assets/1740016744782.png>)

账号密码都是 nacos。 

### 5.2. 服务注册到 nacos

Nacos 是 SpringCloudAlibaba 的组件，而 SpringCloudAlibaba 也遵循 SpringCloud 中定义的服务注册、服务发现规范。因此**使用 Nacos 和使用 Eureka 对于微服务来说，并没有太大区别。**

主要差异在于：

*   依赖不同
*   服务地址不同

**0）注册前先确保启动了 nacos：**

```
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

**1）引入依赖**

在 cloud-demo 父工程的 pom 文件中的`<dependencyManagement>`中引入 **SpringCloudAlibaba 的依赖：**

```
spring:
 cloud:
 nacos:
 server-addr: localhost:8848
```

 **注意：SpringCloud、SpringCloudAlibaba 和 springboot 地版本是需要互相对应的**，具体看文章上面 1.4 版本兼容。

**注释掉 eureka 的客户端依赖**，然后在 user-service 和 order-service 中的 pom 文件中**引入 nacos-discovery 依赖：**

```
@RestController
@RequestMapping("order")
public class OrderController {
 
   @Autowired
   private OrderService orderService;
   @Autowired
   private RestTemplate restTemplate;
 
    @GetMapping("{orderId}")
    public Order queryOrderByUserId(@PathVariable("orderId") Long orderId) {
        // 根据id查询订单并返回
        Order order = orderService.queryOrderById(orderId);
        User user = restTemplate.getForObject("http://user-server/user/" + order.getUserId(), User.class);
        order.setUser(user);
        return order;
    }
}
```

**注意**：**不要忘了注释掉 eureka 的客户端依赖。**

**2）配置 nacos 地址**

先**注释掉 eureka 的地址**，在 user-service 和 order-service 的 application.yml 中**添加 nacos 地址：**

```
spring:
 cloud:
 nacos:
 server-addr: localhost:8848
 discovery:
 cluster-name: HZ # 集群名称
```

**注意**：**不要忘了注释掉 eureka 的地址**

**2.5）拉取和 ureka 一样：**

```
spring:
 cloud:
 nacos:
 server-addr: localhost:8848
 discovery:
 cluster-name: HZ # 集群名称
```

**3）重启**

重启微服务后，登录 nacos 管理页面，可以看到微服务信息：

![](<assets/1740016745015.png>)

### 5.3. 服务分级存储模型

#### 5.3.0. 概述

**一个服务可以有多个实例**，例如我们的 user-service，可以有:

*   127.0.0.1:8081
*   127.0.0.1:8082
*   127.0.0.1:8083

假如这些实例分布于全国各地的不同机房，例如：

*   127.0.0.1:8081，在上海机房
*   127.0.0.1:8082，在上海机房
*   127.0.0.1:8083，在杭州机房

Nacos 就将**同一机房内的实例**划分为一个**集群**。

也就是说，user-service 是服务，**一个服务可以包含多个集群**，如杭州、上海，每个集群下可以有多个实例，形成分级模型，如图：

![](<assets/1740016745379.png>)

微服务互相访问时，应该**尽可能访问同集群实例**，因为本地访问**速度更快**。当本集群内不可用时，才访问其它集群。例如：

![](<assets/1740016745611.png>)

杭州机房内的 order-service 应该优先访问同机房的 user-service。

#### 5.3.1. 给 user-service 配置集群

修改 user-service 的 application.yml 文件，添加集群配置：

```
userservice:
  ribbon:
    NFLoadBalancerRuleClassName: com.alibaba.cloud.nacos.ribbon.NacosRule # 负载均衡规则
```

重启两个 user-service 实例后，我们可以在 nacos 控制台看到下面结果：

![](<assets/1740016745810.png>)

我们再次复制一个 user-service 启动配置，添加属性：

```
spring:
 cloud:
 nacos:
 server-addr: localhost:8848
 discovery:
 cluster-name: HZ
 namespace: 492a7d5d-237b-46a1-a99a-fa8e98e4b0f9 # 命名空间，填ID
```

配置如图所示：

![](<assets/1740016746073.png>)

启动 UserApplication3 后再次查看 nacos 控制台：

![](<assets/1740016746329.png>)

#### 5.3.2. 同集群优先的负载均衡

默认的`ZoneAvoidanceRule`并不能实现根据同集群优先来实现负载均衡。

因此 Nacos 中提供了一个`NacosRule`的实现，可以优先从同集群中挑选实例。

**1）给 order-service 配置集群信息**

修改 order-service 的 application.yml 文件，添加集群配置：

```
spring:
 cloud:
 nacos:
 discovery:
 ephemeral: false # 设置为非临时实例
```

**2）修改负载均衡规则**

修改 **order-service** 的 application.yml 文件，修改负载均衡规则：

```
userservice:
  ribbon:
    NFLoadBalancerRuleClassName: com.alibaba.cloud.nacos.ribbon.NacosRule # 负载均衡规则
```

**注意：**

*   **NacosRule** 是优先选择本地集群，在**本地集群内部是随机访问的**，如果本地集群找不到提供者，会**去其他集群寻找并报警告**。
*   对比 eureka 的 **ZoneAvoidanceRule** 是同区域内**轮询**

### 5.4. 权重配置

实际部署中会出现这样的场景：

服务器设备性能有差异，部分实例所在机器性能较好，另一些较差，我们希望性能好的机器承担更多的用户请求。

但默认情况下 NacosRule 是同集群内随机挑选，不会考虑机器的性能问题。

因此，**Nacos 提供了权重配置来控制访问频率，性能越好的机器权重越大则访问频率越高。**

在 nacos 控制台，找到 user-service 的实例列表，点击编辑，即可修改权重，**默认权重是 1**：

![](<assets/1740016746594.png>)

在弹出的编辑窗口，修改权重：

![](<assets/1740016746914.png>)

**注意**：如果权重修改为 0，则该实例永远不会被访问

### 5.5. 环境隔离，namespace

Nacos 提供了 **namespace 来实现环境隔离功能**。

*   nacos 中可以有多个 namespace
*   namespace 下可以有**组 group、服务存储 service / 数据存储 data** 等。业务相关度高的服务放在一个组 group，例如订单和支付。
*   **不同 namespace 之间相互隔离**，例如不同 namespace 的服务互相不可见

![](<assets/1740016747106.png>)

**5.5.1. 创建 namespace**

**默认**情况下，所有 **service、data、group 都在同一个 namespace**，名为 **public**：

![](<assets/1740016747401.png>)

![](<assets/1740016747767.png>)

我们可以点击页面新增按钮，**添加一个 namespace**：

![](<assets/1740016747960.png>)

然后，填写表单：

![](<assets/1740016748214.png>)

就能在页面看到一个新的 namespace：

![](<assets/1740016748431.png>)

**5.5.2. 给微服务配置 namespace**

给微服务配置 namespace 只能通过修改配置来实现。

例如，修改 order-service 的 application.yml 文件：

```
spring:
 cloud:
 nacos:
 server-addr: localhost:8848
 discovery:
 cluster-name: HZ
 namespace: 492a7d5d-237b-46a1-a99a-fa8e98e4b0f9 # 命名空间，填ID
```

重启 order-service 后，访问控制台，可以看到下面的结果：

![](<assets/1740016748616.png>)

![](<assets/1740016748861.png>)

此时访问 order-service，因为 namespace 不同，会导致找不到 userservice，控制台会报错：

![](<assets/1740016749111.png>)

### 5.6.Nacos 与 Eureka 的区别

**Nacos** 的服务实例分为两种 l 类型：

*   **临时实例（默认）：**如果实例宕机超过一定时间，会从服务列表剔除，默认的类型。
*   **非临时实例：**如果实例宕机，不会从服务列表剔除，也可以叫永久实例。

配置一个服务实例为永久实例：

```
spring:
 cloud:
 nacos:
 discovery:
 ephemeral: false # 设置为非临时实例
```

ephemeral 译为短暂的，短命的。

![](<assets/1740016749292.png>)

Nacos 和 Eureka 整体结构类似，服务注册、服务拉取、心跳等待，但是也存在一些差异：包括**给消费者主动推送变更消息，对非临时实例主动检测状态。**

![](<assets/1740016749573.png>)

*   **Nacos 与 eureka 的共同点**
    
    *   **注册拉取：**都支持服务注册和服务拉取
        
    *   **心跳：**都支持服务提供者心跳方式做健康检测
        
*   **Nacos 与 Eureka 的区别**
    
    *   **检测故障：**Nacos 临时实例采用心跳模式，非临时实例采用主动检测模式；eureka 是只能采用心跳模式。
        
    *   **故障实例剔除：**nacos 临时实例心跳不正常会被剔除，非临时实例则不会被剔除；eureka 只支持心跳。
        
    *   **及时更新：**Nacos 支持服务列表变更的消息推送模式，服务列表更新更及时
        
    *   **分布式指标：**Nacos 集群默认采用 AP 方式（最终一致），当集群中存在非临时实例时，采用 CP 模式（强一致）；Eureka 采用 AP 方式
        

**CAP：**分布式三个指标，不可能同时满足，只能满足 CP（一致性、分区容错性）或者 AP（可用性、分区容错性）。

*   **一致性 C：**Consistency，访问同一个集群里的多个实例，得到的数据必须一致。
*   **可用性 A：**Availability ，访问集群中的任意健康节点，必须能得到响应，而不是超时或拒绝。
*   **分区容错 P：**Partition Tolerance，节点故障导致集群分区后，系统依然对外提供服务。

**AP（可用 + 分区容错，最终一致）：**各子事务分别执行和提交，允许出现结果不一致，然后采用弥补措施恢复数据即可，实现最终一致。

**CP（一致 + 分区容错，强一致）：**各个子事务执行后互相等待，同时提交，同时回滚，达成强一致。但事务等待过程中，处于弱可用状态。

**集群：**一种部署方式。按照微服务架构风格拆分出来的**同一个业务系统部署到多个服务器上**，多个服务干一件事情，某一个服务宕机，用户基本无感知。主要为了保证**高可用**。我们通常讲的 tomcat 集群，nginx 集群，redis 集群都是为了确保系统的稳定性。