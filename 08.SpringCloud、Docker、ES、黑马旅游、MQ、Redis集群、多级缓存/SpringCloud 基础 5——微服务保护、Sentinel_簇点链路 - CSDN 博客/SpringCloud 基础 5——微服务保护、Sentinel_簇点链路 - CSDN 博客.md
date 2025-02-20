---
url: https://blog.csdn.net/qq_40991313/article/details/126882045?spm=1001.2014.3001.5501
title: SpringCloud 基础 5——微服务保护、Sentinel_簇点链路 - CSDN 博客
date: 2025-02-20 09:59:29
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1. 初识 Sentinel](#t0)

[1.1. 雪崩问题及解决方案](#t1)

[1.1.1. 雪崩问题](#t2)

[1.1.2. 方案 1：超时处理](#t3)

[1.1.3. 方案 2：仓壁模式 / 线程隔离](#t4)

[1.1.4. 方案 3：断路器](#t5)

[1.1.5. 方案 4（预防）：限流](#t6)

[1.1.6. 总结，雪崩问题](#t7)

[1.2. 服务保护技术对比](#t8)

[1.3.Sentinel 介绍和安装](#t9)

[1.3.1. 初识 Sentinel](#t10)

[1.3.2. 安装 Sentinel](#t11)

[1.4. 微服务整合 Sentinel](#t12)

[2. 流量控制](#t13)

[2.1. 簇点链路](#t14)

[2.1. 流量控制示例（直接模式）](#t15)

[2.1.1. 示例](#t16)

[2.1.2.jmeter 压力测试](#t17)

[2.2. 流控模式](#t18)

[2.2.1. 关联模式](#t19)

[2.2.2. 链路模式，@SentinelResource](#t20)

[2.2.3. 总结](#t21)

[2.3. 流控效果](#t22)

[2.3.1.warm up](#t23)

[2.3.2. 排队等待](#t24)

[2.3.3. 总结，三种流控效果](#t25)

[2.4. 热点参数限流](#t26)

[2.4.1. 全局参数限流](#t27)

[2.4.2. 热点参数限流](#t28)

[2.4.4. 案例](#t29)

[3. 隔离和降级](#t30)

[3.0. 线程隔离和熔断降级回顾](#t31)

[3.1.FeignClient 整合 Sentinel](#t32)

[3.1.1. 修改配置，开启 sentinel 功能](#t33)

[3.1.2. 编写失败后的降级逻辑](#t34)

[3.1.3. 总结整合步骤](#t35)

[3.2. 线程隔离（舱壁模式）](#t36)

[3.2.1. 线程隔离的实现方式](#t37)

[3.2.2.sentinel 的线程隔离](#t38)

[3.2.3. 总结](#t39)

[3.3. 熔断降级](#t40)

[3.3.1. 慢调用](#t41)

[3.3.2. 异常比例、异常数](#t42)

[4. 授权规则](#t43)

[4.1. 授权规则](#t44)

[4.1.1. 基本规则](#t45)

[4.1.2. 如何获取 origin（用来识别请求来自网关）](#t46)

[4.1.3. 给网关添加请求头](#t47)

[4.1.4. 配置授权规则](#t48)

[4.2. 自定义异常结果](#t49)

[4.2.1. 异常类型](#t50)

[4.2.2. 自定义异常处理，BlockExceptionHandler 接口](#t51)

[4.2.3. 测试自定义的限流和授权异常](#t52)

[5. 规则持久化，避免规则丢失](#t53)

[5.1. 规则管理模式](#t54)

[5.1.1.pull 模式](#t55)

[5.1.2.push 模式](#t56)

[5.2. 实现 push 模式](#t57)

[5.2.1. 修改 order-service 服务](#t58)

[1. 引入依赖](#t59)

[2. 配置 nacos 地址](#t60)

[5.2.2. 修改 sentinel-dashboard 源码](#t61)

[1. 解压](#t62)

[2. 修改 nacos 依赖](#t63)

[3. 添加 nacos 支持](#t64)

[4. 修改 nacos 地址](#t65)

[5. 配置 nacos 数据源](#t66)

[6. 修改前端页面](#t67)

[7. 重新编译、打包项目](#t68)

[8. 启动](#t69)

## 1. 初识 Sentinel

### 1.1. 雪崩问题及解决方案

#### 1.1.1. 雪崩问题

微服务中，服务间调用关系错综复杂，一个微服务往往依赖于多个其它微服务。

![](<assets/1740016770254.png>)

如图，如果服务提供者 I 发生了故障，当前的应用的部分业务因为依赖于服务 I，因此也会被阻塞。此时，其它不依赖于服务 I 的业务似乎不受影响。

![](<assets/1740016770524.png>)

但是，依赖服务 I 的业务请求被阻塞，用户不会得到响应，则 tomcat 的这个线程不会释放，于是越来越多的用户请求到来，越来越多的线程会阻塞：

![](<assets/1740016770797.png>)

**服务器支持的线程和并发数有限，请求一直阻塞，会导致服务器资源耗尽，从而导致所有其它服务都不可用**，那么当前服务也就不可用了。

那么，依赖于当前服务的其它服务随着时间的推移，最终也**都会变的不可用**，形成级联失败，雪崩就发生了：

![](<assets/1740016771072.png>)

**解决办法：**超时处理、线程隔离、降级熔断。

**预防办法：**流量控制 

#### 1.1.2. 方案 1：超时处理

解决雪崩问题的常见方式有四种：

**• 超时处理：**设定超时时间，**请求超过一定时间没有响应就返回错误信息**，不会无休止等待

![](<assets/1740016771289.png>)

#### 1.1.3. 方案 2：仓壁模式 / 线程隔离

仓壁模式来源于船舱的设计：

![](<assets/1740016771540.png>)

船舱都会被隔板分离为多个独立空间，当船体破损时，只会导致部分空间进入，将故障控制在一定范围内，避免整个船体都被淹没。

于此类似，我们可以**限定每个业务能使用的线程数**，避免耗尽整个 tomcat 的资源，因此**也叫线程隔离。**

![](<assets/1740016771739.png>)

#### 1.1.4. 方案 3：断路器

**断路器模式：**由**断路器**统计业务执行的**异常比例**，如果**超出阈值则会熔断该业务**，**拦截**访问**该业务**的**一切请求**。

断路器会统计访问某个服务的请求数量，异常比例：

![](<assets/1740016772005.png>)

当发现访问服务 D 的请求**异常比例**过高时，认为服务 D 有导致雪崩的风险，会拦截访问服务 D 的一切请求，形成熔断：

![](<assets/1740016772268.png>)

#### 1.1.5. 方案 4（预防）：限流

**流量控制**：**限制**业务访问的 **QPS（**Queries-per-second **每秒查询的数量）**，**避免瞬间高并发的流量**导致服务故障。

前面方案都是出现雪崩风险后采取措施，限流方案是预防雪崩。 

这里就用到 **sentinel（译为哨兵、守卫）**了， 它可以**按照目标服务能承受的频率释放请求**。

![](<assets/1740016772521.png>)

#### 1.1.6. 总结，雪崩问题

**什么是雪崩问题？**

*   微服务之间相互调用，因为调用链中的一个服务故障，引起整个链路都无法访问的情况。

可以认为：

**限流**是对服务的保护，避免因瞬间高并发流量而导致服务故障，进而避免雪崩。是一种**预防**措施。

**超时处理、线程隔离、降级熔断**是在部分服务故障时，将故障控制在一定范围，避免雪崩。是一种**补救**措施。

### 1.2. 服务保护技术对比

在 SpringCloud 当中支持多种服务保护技术：

*   [Netfix Hystrix](https://github.com/Netflix/Hystrix "Netfix Hystrix")
*   [Sentinel](https://github.com/alibaba/Sentinel "Sentinel")
*   [Resilience4J](https://github.com/resilience4j/resilience4j "Resilience4J")

早期比较流行的是 Hystrix 框架，但目前国内使用**最广泛的还是阿里巴巴的 Sentinel 框架**，这里我们做下对比：

<table><thead><tr><th></th><th><strong>Sentinel</strong></th><th><strong>Hystrix</strong></th></tr></thead><tbody><tr><td>隔离策略</td><td>信号量隔离</td><td>线程池隔离 / 信号量隔离</td></tr><tr><td>熔断降级策略</td><td>基于慢调用比例或异常比例</td><td>基于失败比率</td></tr><tr><td>实时指标实现</td><td>滑动窗口</td><td>滑动窗口（基于 RxJava）</td></tr><tr><td>规则配置</td><td>支持多种数据源</td><td>支持多种数据源</td></tr><tr><td>扩展性</td><td>多个扩展点</td><td>插件的形式</td></tr><tr><td>基于注解的支持</td><td>支持</td><td>支持</td></tr><tr><td>限流</td><td>基于 QPS，支持基于调用关系的限流</td><td>有限的支持</td></tr><tr><td>流量整形</td><td>支持慢启动、匀速排队模式</td><td>不支持</td></tr><tr><td>系统自适应保护</td><td>支持</td><td>不支持</td></tr><tr><td>控制台</td><td>开箱即用，可配置规则、查看秒级监控、机器发现等</td><td>不完善</td></tr><tr><td>常见框架的适配</td><td>Servlet、Spring Cloud、Dubbo、gRPC 等</td><td>Servlet、Spring Cloud Netflix</td></tr></tbody></table>

**线程池隔离：**当前业务给每个被隔离的业务都创建一个独立的线程池。优点隔离性好，缺点线程数量成倍增长，cpu 压力增大。

**信号隔离：**限制每个业务能使用线程的数量，超过限制不再创建新线程。**比线程池隔离更好**，优点是不影响性能，缺点是隔离性稍差于线程池隔离，因为所有业务在一个池子里，而线程池每个业务有独立的线程池。

### 1.3.Sentinel 介绍和安装

#### 1.3.1. 初识 Sentinel

Sentinel 是阿里巴巴开源的一款微服务流量控制组件。官网地址：https://sentinelguard.io/zh-cn/index.html

Sentinel 具有以下特征:

• **丰富的应用场景**：Sentinel 承接了阿里巴巴近 10 年的双十一大促流量的核心场景，例如秒杀（即突发流量控制在系统容量可以承受的范围）、消息削峰填谷、集群流量控制、实时熔断下游不可用应用等。

• **完备的实时监控**：Sentinel 同时提供实时的监控功能。您可以在控制台中看到接入应用的单台机器秒级数据，甚至 500 台以下规模的集群的汇总运行情况。

• **广泛的开源生态**：Sentinel 提供开箱即用的与其它开源框架 / 库的整合模块，例如与 Spring Cloud、Dubbo、gRPC 的整合。您只需要引入相应的依赖并进行简单的配置即可快速地接入 Sentinel。

• **完善的** **SPI** **扩展点**：Sentinel 提供简单易用、完善的 SPI 扩展接口。您可以通过实现扩展接口来快速地定制逻辑。例如定制规则管理、适配动态数据源等。

#### 1.3.2. 安装 Sentinel

**1）下载**

sentinel 官方提供了 UI 控制台，方便我们对系统做限流设置。大家可以在 [GitHub](https://github.com/alibaba/Sentinel/releases "GitHub") 下载。

课前资料也提供了下载好的 jar 包：

![](<assets/1740016772723.png>)

**2）运行**

将 jar 包放到任意**非中文目录**，在地址栏 cmd 回车打开命令行，执行下面命令：

```
server:
 port: 8080
spring:
 cloud:
 nacos:
 discovery:
 server-addr: nacos:8848
 cluster-name: HZ
 application:
 name: order-service
 datasource:
 url: jdbc:mysql://mysql:3306/cloud_order?useSSL=false
 username: root
 password: 1234
 driver-class-name: com.mysql.jdbc.Driver
mybatis:
 type-aliases-package: cn.itcast.user.pojo
 configuration:
 map-underscore-to-camel-case: true
logging:
 level:
    cn.itcast: debug
 pattern:
 dateformat: MM-dd HH:mm:ss:SSS
#eureka:
#  client:
#    service-url:
#      defaultZone: http://127.0.0.1:10086/eureka
```

如果要修改 Sentinel 的默认端口、账户、密码，可以通过下列配置：

<table><thead><tr><th><strong>配置项</strong></th><th><strong>默认值</strong></th><th><strong>说明</strong></th></tr></thead><tbody><tr><td>server.port</td><td>8080</td><td>服务端口</td></tr><tr><td>sentinel.dashboard.auth.username</td><td>sentinel</td><td>默认用户名</td></tr><tr><td>sentinel.dashboard.auth.password</td><td>sentinel</td><td>默认密码</td></tr></tbody></table>

例如，如果想要**修改端口：**

```
<!--sentinel-->
<dependency>
    <groupId>com.alibaba.cloud</groupId> 
    <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>
```

**3）访问**

访问 **http://localhost:8080** 页面，就可以看到 sentinel 的控制台了：

![](<assets/1740016772976.png>)

需要输入账号和密码，默认都是：sentinel

登录后，发现一片空白，什么都没有：

![](<assets/1740016773184.png>)

这是因为我们还没有与微服务整合。

### 1.4. 微服务整合 Sentinel

我们在 cloud-demo 的 **order-service** 中整合 sentinel，并连接 sentinel 的控制台，步骤如下：

 cloud-demo 是学网关、feign、微服务、nacos 等时候的学习项目

yml：

```
server:
 port: 8088
spring:
 cloud: 
 sentinel:
 transport:
 dashboard: localhost:8080    #译为控制面板、仪表盘
```

使用前先打开 nacos 服务器：

双击 bin 目录下

```
@GetMapping("/query")
public String queryOrder() {
    return "查询订单成功";
}
```

**1）在 order-service 引入 sentinel 依赖**

```
@GetMapping("/update")
public String updateOrder() {
    return "更新订单成功";
}
```

**2）配置控制台**

修改 application.yaml 文件，添加下面内容：

```
public void queryGoods(){
    System.err.println("查询商品");
}
```

**3）重启服务后访问 order-service 的任意端点**

打开浏览器，访问 http://localhost:8088/order/101，这样才能触发 sentinel 的监控。

然后再访问 sentinel 的控制台，查看效果：

![](<assets/1740016773417.png>)

**注意：**

*   **如果刷新后实时监控无数据，那就是你的 http://localhost:8088/order/101 是假刷新，ctrl+f5 清理缓存刷新就好了**

## 2. 流量控制

雪崩问题虽然有四种方案，但是限流是避免服务因突发的流量而发生故障，是对微服务雪崩问题的预防。我们先学习这种模式。

### 2.1. 簇点链路

当请求进入微服务时，首先会访问 DispatcherServlet，然后进入 Controller、Service、Mapper，这样的一个调用链就叫做**簇点链路**。簇点链路中**被监控的每一个接口**就是一个**资源**。

**默认**情况下 sentinel 会**监控** SpringMVC 的**每一个端点**（Endpoint，也就是 **controller 中的方法**），因此 SpringMVC 的**每一个端点**（Endpoint）**就是调用链路中的一个资源**。

例如，我们刚才访问的 order-service 中的 OrderController 中的端点：/order/{orderId}

![](<assets/1740016773722.png>)

**流控、熔断等都是针对簇点链路中的资源来设置的**，因此我们可以点击对应资源后面的按钮来设置规则：

*   **流控：**流量控制
*   **降级：**降级熔断
*   **热点：**热点参数限流，是限流的一种
*   **授权：**请求的权限控制

### 2.1. 流量控制示例（直接模式）

#### 2.1.1. 示例

点击资源 / order/{orderId} 后面的流控按钮，就可以弹出表单。

![](<assets/1740016774032.png>)

表单中可以填写限流规则，如下：

![](<assets/1740016774125.png>)

单机阈值为 QPS 类型的 1，含义是限制 /order/{orderId} 这个资源的单机 **QPS(queries per second) 为 1**，即**每秒只允许 1 次请求**，超出的请求会被拦截并报错。

#### 2.1.2.**jmeter 压力测试**

需求：给 /order/{orderId} 这个资源设置流控规则，QPS 不能超过 5，然后测试。

**1）首先在 sentinel 控制台添加限流规则**

![](<assets/1740016774327.png>)

**2）利用 jmeter 测试**

如果没有用过 jmeter，可以参考课前资料提供的文档《Jmeter 快速入门. md》

课前资料提供了编写好的 Jmeter 测试样例：

![](<assets/1740016774653.png>)

打开 jmeter，导入课前资料提供的测试样例：

![](<assets/1740016774834.png>)

选择：

![](<assets/1740016775079.png>)

**20 个用户，2 秒内运行完**，QPS 是 10，超过了 5.

选中`流控入门，QPS<5`右键运行：

![](<assets/1740016775288.png>)

**注意，**不要点击菜单中的执行按钮来运行。

**结果：****成功的请求每次只有 5 个，而 QPS 值正好是 5：**

![](<assets/1740016775476.png>)

### 2.2. 流控模式

在添加**限流规则**时，点击**高级选项**，可以选择**三种流控模式**：

*   **直接（默认）：**统计当前资源的请求，**触发阈值**时对当前资源**直接限流**，也是**默认**的模式
*   **关联：**统计与当前资源**相关的另一个资源，触发阈值**时，对**当前资源限流**
*   **链路：**统计从指定链路访问到本资源的请求，触发阈值时，对**指定链路限流。**例如 a、b 访问资源 c，只对指定的 a 到 c 的请求链路限流，b 到 c 没指定就不管。

![](<assets/1740016775765.png>)

![](<assets/1740016775975.png>)

2.1 流量控制测试的就是直接模式。

#### 2.2.1. 关联模式

**关联模式**：统计与当前资源相关的另一个资源，触发阈值时，对当前资源限流

**配置规则**：

![](<assets/1740016776213.png>)

**语法说明**：当 / write 资源访问量触发阈值时，就会对 / read 资源限流，避免影响 / write 资源。

**使用场景**：比如用户支付时需要修改订单状态，同时用户要查询订单。查询和修改操作会争抢数据库锁，产生竞争。业务需求是优先支付和更新订单的业务，因此当修改订单业务触发阈值时，需要对查询订单业务限流。

**需求说明**：

*   在 OrderController 新建两个端点：**/order/query 和 / order/update**，无需实现业务
    
*   配置流控规则，当 **/order/ update** 资源被访问的 **QPS 超过 5** 时，**对 / order/query 请求限流**
    

**1）定义 / order/query 端点，模拟订单查询**

```
@GetMapping("/query")
public String queryOrder() {
    // 查询商品
    orderService.queryGoods();
    // 查询订单
    System.out.println("查询订单");
    return "查询订单成功";
}
```

**2）定义 / order/update 端点，模拟订单更新**

```
@GetMapping("/save")
public String saveOrder() {
    // 查询商品
    orderService.queryGoods();
    // 查询订单
    System.err.println("新增订单");
    return "新增订单成功";
}
```

**重启服务，查看 sentinel 控制台的簇点链路：**

![](<assets/1740016776442.png>)

**3）配置流控规则**

对哪个端点限流，就点击哪个端点后面的按钮。我们是对订单查询 / order/query 限流，因此点击它后面的按钮：

![](<assets/1740016776683.png>)

在表单中填写流控规则：

![](<assets/1740016776878.png>)

**4）在 Jmeter 测试**

选择《流控模式 - 关联》：

![](<assets/1740016777114.png>)

可以看到 1000 个用户，100 秒，因此 QPS 为 10，超过了我们设定的阈值：5

查看 http 请求：

![](<assets/1740016777298.png>)

**请求的目标是 / order/update**，这样这个断点就会触发阈值。

**但限流的目标是 / order/query****：**

![](<assets/1740016777515.png>)

**5）总结**

![](<assets/1740016777881.png>)

#### 2.2.2. 链路模式，**@SentinelResource**

**链路模式**：只针对从指定链路访问到本资源的请求做统计，判断是否超过阈值。

**配置示例**：

例如有两条请求链路：

*   /test1 --> /common
    
*   /test2 --> /common
    

如果只希望统计从 / test2 进入到 / common 的请求，则可以这样配置：

![](<assets/1740016778189.png>)

**实战案例**

**需求：**有查询订单和创建订单业务，两者都需要查询商品。针对从查询订单进入到查询商品的请求统计，并设置限流。

**步骤：**

1.  在 OrderService 中添加一个 queryGoods 方法，不用实现业务
    
2.  在 OrderController 中，改造 / order/query 端点，调用 OrderService 中的 queryGoods 方法
    
3.  在 OrderController 中添加一个 / order/save 的端点，调用 OrderService 的 queryGoods 方法
    
4.  给 queryGoods 设置限流规则，从 / order/query 进入 queryGoods 的方法限制 QPS 必须小于 2
    

**实现：**

**1）添加查询商品方法**

在 order-service 服务中，给 OrderService 类添加一个 queryGoods 方法：

```
@SentinelResource("goods")
public void queryGoods(){
    System.err.println("查询商品");
}
```

**2）查询订单时，查询商品**

在 order-service 的 OrderController 中，修改 / order/query 端点的业务逻辑：

```
spring:
 cloud:
 sentinel:
 web-context-unify: false # 关闭context整合
```

**3）新增订单，查询商品**

在 order-service 的 OrderController 中，修改 / order/save 端点，模拟新增订单：

```
feign:
 sentinel:
 enabled: true # 开启feign对sentinel的支持
```

**4）给查询商品添加资源标记，并关闭对 SpringMVC 的资源聚合**

默认情况下，OrderService 中的方法是不被 Sentinel 监控的，需要我们自己通过注解来标记要监控的方法。

给 OrderService 的 queryGoods 方法添加 **@SentinelResource** 注解：

```
package cn.itcast.feign.clients.fallback;
 
import cn.itcast.feign.clients.UserClient;
import cn.itcast.feign.pojo.User;
import feign.hystrix.FallbackFactory;
import lombok.extern.slf4j.Slf4j;
 
@Slf4j
public class UserClientFallbackFactory implements FallbackFactory<UserClient> {
    @Override
    public UserClient create(Throwable throwable) {
        return new UserClient() {
            //匿名内部类，重写UserClient里的方法
            @Override
            public User findById(Long id) {
                log.error("查询用户异常", throwable);
                return new User();
            }
        };
    }
}
```

链路模式中，是对不同来源的两个链路做监控。但是 **sentinel 默认会给进入 SpringMVC 的所有请求设置同一个 root 资源，会导致链路模式失效。**

我们需要**关闭这种对 SpringMVC 的资源聚合**，修改 order-service 服务的 application.yml 文件：

```
@FeignClient("user-service")
public interface UserClient {
    @GetMapping("/user/{id}")
    public User findById(@PathVariable Long id);
}
```

**重启服务**，访问 / order/query 和 / order/save，可以查看到 sentinel 的簇点链路规则中，出现了**新的资源：**

![](<assets/1740016778452.png>)

**5）添加流控规则**

点击 goods 资源后面的流控按钮，在弹出的表单中填写下面信息：

![](<assets/1740016778756.png>)

只统计从 / order/query 进入 / goods 的资源，QPS 阈值为 2，超出则被限流。

6）Jmeter 测试

选择《流控模式 - 链路》：

![](<assets/1740016779088.png>)

可以看到这里 200 个用户，50 秒内发完，QPS 为 4，超过了我们设定的阈值 2

一个 http 请求是访问 / order/save：

![](<assets/1740016779344.png>)

运行的结果：

![](<assets/1740016779557.png>)

完全不受影响。

另一个是访问 / order/query：

![](<assets/1740016779767.png>)

运行结果：

![](<assets/1740016780245.png>)

每次只有 2 个通过。

#### 2.2.3. 总结

流控模式有哪些？

• 直接：对当前资源限流

• 关联：高优先级资源触发阈值，对低优先级资源限流。

• 链路：阈值统计时，只统计从指定资源进入当前资源的请求，是对请求来源的限流

### 2.3. 流控效果

在流控的高级选项中，还有一个**流控效果选项：**

![](<assets/1740016780571.png>)

流控效果是指请求达到流控阈值时应该采取的措施，包括三种：

*   **快速失败（默认）：**达到阈值后，新的请求会被立即拒绝并抛出 FlowException 异常，状态码 429。是默认的处理方式。
    
*   **warm up 预热模式：**对超出阈值的请求同样是拒绝并抛出异常。但这种模式**阈值会动态变化**，从一个较小值**逐渐增加到最大阈值，阈值初始值是 “最大阈值 / 冷启动因子”，**默认冷启动因子 coldFactor 是 3，也就是阈值默认会从 1/3 最大阈值到最大阈值。
    
*   **排队等待：**让所有的**请求**按照**先后次序排队执行**，两个请求的间隔不能小于指定时长
    

#### 2.3.1.warm up

阈值一般是一个微服务能承担的最大 QPS，但是一个**服务刚刚启动时，一切资源尚未初始化**（**冷启动**），如果**直接将 QPS 跑到最大值**，**可能导致服务瞬间宕机**。

warm up 也叫**预热模式**，是**应对服务冷启动**的一种方案。请求**阈值初始值**是 **maxThreshold / coldFactor**，持续指定时长后，逐渐提高到 maxThreshold 值。而**冷启动因子 coldFactor 的默认值是 3.**

**例如**，我设置 QPS 的 maxThreshold 为 10，预热时间为 5 秒，那么初始阈值就是 10 / 3 ，也就是 3，然后在 5 秒后逐渐增长到 10.

![](<assets/1740016780750.png>)

**案例**

**需求：**给 / order/{orderId} 这个资源设置限流，最大 QPS 为 10，利用 warm up 效果，预热时长为 5 秒

**1）配置流控规则：**

![](<assets/1740016781103.png>)

**2）Jmeter 测试**

选择《流控效果，warm up》：

![](<assets/1740016781388.png>)

QPS 为 10.

**刚刚启动时，大部分请求失败，成功的只有 3 个，说明 QPS 被限定在 3：**

![](<assets/1740016781630.png>)

**随着时间推移，成功比例越来越高：**

![](<assets/1740016781860.png>)

到 Sentinel 控制台查看实时监控：

![](<assets/1740016782127.png>)

**一段时间（5 秒）后：**

![](<assets/1740016782470.png>)

#### 2.3.2. 排队等待

当请求超过 QPS 阈值时，快速失败和 warm up 会拒绝新的请求并抛出异常。

而排队等待则是让**所有请求进入一个队列中**，然后按照**阈值允许的时间间隔**依次执行。后来的请求必须等待前面执行完成，如果请求**预期的等待时间超出最大时长**，则会**被拒绝**。

**工作原理**

**例如：**QPS = 5，每秒处理 5 个请求，即每 200ms 处理一个队列中的请求；设置超时时间 timeout = 2000，意味着**预期等待时长**超过 2000ms 的请求会被拒绝并抛出异常。

![](<assets/1740016782783.png>)

**那什么叫做预期等待时长呢？**

比如现在一下子来了 12 个请求，因为每 200ms 执行一个请求，那么：

*   第 6 个请求的**预期等待时长** = 200 * （6 - 1） = 1000ms
*   第 12 个请求的预期等待时长 = 200 * （12-1） = 2200ms

现在，第 1 秒同时接收到 10 个请求，但第 2 秒只有 1 个请求，此时 QPS 的曲线这样的：

![](<assets/1740016783164.png>)

如果**使用队列模式做流控**，所有进入的请求都要**排队**，以固定的 200ms 的间隔执行，**QPS 会变的很平滑：**

![](<assets/1740016783347.png>)

平滑的 QPS 曲线，对于**服务器来说是更友好的**。

**案例**

**需求：**给 / order/{orderId} 这个资源设置限流，最大 QPS 为 10，利用排队的流控效果，超时时长设置为 5s

**1）添加流控规则**

![](<assets/1740016783583.png>)

**2）Jmeter 测试**

选择《流控效果，队列》：

![](<assets/1740016783888.png>)

**QPS 为 15，已经超过了我们设定的 10。**

如果是之前的 快速失败、warmup 模式，超出的请求应该会直接报错。

但是我们看看队列模式的运行结果：

![](<assets/1740016784162.png>)

**全部都通过了。**

再去 sentinel 查看实时监控的 QPS 曲线：

![](<assets/1740016784439.png>)

**QPS 非常平滑，一致保持在 10**，但是**超出的请求没有被拒绝**，而是放入队列。因此**响应时间**（等待时间）**会越来越长**。

当**队列满了以后**，才会有**部分请求失败**：

![](<assets/1740016784627.png>)

#### 2.3.3. 总结，三种流控效果

**流控效果有哪些？**

*   **快速失败：**QPS 超过阈值时，拒绝新的请求
    
*   **warm up：** QPS 超过阈值时，拒绝新的请求；QPS 阈值是逐渐提升的，可以**避免冷启动时高并发导致服务宕机。**
    
*   **排队等待：**请求会进入**队列**，按照阈值允许的时间间隔依次执行请求；如果请求预期等待时长大于超时时间，直接拒绝
    

### 2.4. 热点参数限流

之前的限流是统计访问某个资源的所有请求，判断是否超过 QPS 阈值。而热点参数限流是**分别统计参数值相同的请求**，**判断是否超过 QPS 阈值。**

例如一堆请求里，大部分请求携带的参数都是 id=1，id=1 就是热点参数，我们对它进行限流。

#### 2.4.1. 全局参数限流

例如，一个根据 id 查询商品的接口：

![](<assets/1740016784811.png>)

访问 / goods/{id} 的请求中，id 参数值会有变化，热点参数限流会根据参数值分别统计 QPS，统计结果：

![](<assets/1740016785058.png>)

当 id=1 的请求触发阈值被限流时，id 值不为 1 的请求不受影响。

配置示例：

![](<assets/1740016785262.png>)

代表的含义是：对 hot 这个资源的 0 号参数（第一个参数）做统计，每 1 秒**相同参数值**的请求数不能超过 5

#### 2.4.2. 热点参数限流

刚才的配置中，对查询商品这个接口的所有商品一视同仁，QPS 都限定为 5.

而在实际开发中，可能部分商品是热点商品，例如秒杀商品，我们希望这部分商品的 QPS 限制与其它商品不一样，高一些。那就需要配置热点参数限流的高级选项了：

![](<assets/1740016785425.png>)

![](<assets/1740016785723.png>)

结合上一个配置，这里的含义是对 0 号的 long 类型参数限流，每 1 秒相同参数的 QPS 不能超过 5，有两个例外：

• 如果参数值是 100，则每 1 秒允许的 QPS 为 10

• 如果参数值是 101，则每 1 秒允许的 QPS 为 15

#### 2.4.4. 案例

**案例需求**：给 / order/{orderId} 这个资源添加热点参数限流，规则如下：

• 默认的热点参数规则是每 1 秒请求量不超过 2

• 给 102 这个参数设置例外：每 1 秒请求量不超过 4

• 给 103 这个参数设置例外：每 1 秒请求量不超过 10

**注意事项**：热点参数限流对默认的 SpringMVC 资源无效，需要利用 **@SentinelResource 注解**标记资源

**1）标记资源**

给 order-service 中的 OrderController 中的 / order/{orderId} 资源添加注解：

![](<assets/1740016785976.png>)

**2）热点参数限流规则**

访问该接口，可以看到我们标记的 hot 资源出现了：

![](<assets/1740016786256.png>)

这里不要点击 hot 后面的按钮，页面有 BUG

**点击左侧菜单中热点规则菜单：**

![](<assets/1740016786543.png>)

点击新增，填写表单：

![](<assets/1740016786778.png>)

**3）Jmeter 测试**

选择《热点参数限流 QPS1》：

![](<assets/1740016786982.png>)

这里发起请求的 QPS 为 5.

包含 3 个 http 请求：

**普通参数，QPS 阈值为 2**

![](<assets/1740016787179.png>)

运行结果：

![](<assets/1740016787457.png>)

**例外项 102，QPS 阈值为 4**

![](<assets/1740016787771.png>)

运行结果：

![](<assets/1740016788019.png>)

**例外项 103，QPS 阈值为 10**

![](<assets/1740016788271.png>)

运行结果：

![](<assets/1740016788478.png>)

## 3. 隔离和降级

### 3.0. 线程隔离和熔断降级回顾

**限流是一种预防措施**，虽然限流可以尽量避免因高并发而引起的服务故障，但服务还会因为其它原因而故障。

而要将这些故障控制在一定范围，避免雪崩，就要靠**线程隔离**（舱壁模式）和**熔断降级**手段了。

**线程隔离**之前讲到过：调用者在调用服务提供者时，给每个调用的请求分配独立线程池，出现故障时，最多消耗这个线程池内资源，避免把调用者的所有资源耗尽。

![](<assets/1740016788786.png>)

**熔断降级**：是在调用方这边加入断路器，统计对服务提供者的调用，如果调用的失败比例过高，则熔断该业务，不允许访问该服务的提供者了。

![](<assets/1740016789035.png>)

可以看到，**不管是线程隔离还是熔断降级，都是对客户端（调用方）的保护**。需要在**客户端**发起远程调用时做线程隔离、或者服务熔断，防止客户端因阻塞崩溃。

而我们的微服务远程调用都是基于 Feign 来完成的，因此我们需要将 Feign 与 Sentinel 整合，在 Feign 里面实现线程隔离和服务熔断。

### 3.1.FeignClient 整合 Sentinel

SpringCloud 中，微服务调用都是通过 Feign 来实现的，因此做客户端保护必须整合 Feign 和 Sentinel。

#### 3.1.1. 修改配置，开启 sentinel 功能

修改 OrderService 的 application.yml 文件，开启 Feign 的 Sentinel 功能：

```
@Configuration
public class DefaultFeignConfiguration {
    @Bean
    public UserClientFallbackFactory userClientFallbackFactory(){
        return new UserClientFallbackFactory();
    }
 
}
```

#### 3.1.2. 编写失败后的降级逻辑

**失败逻辑：业务失败后，不能直接报错，而应该返回用户一个友好提示或者默认结果，这个就是失败降级逻辑。**

给 FeignClient 编写失败后的降级逻辑

**①方式一：**FallbackClass，无法对远程调用的异常做处理

**②方式二（推荐）：FallbackFactory**，**可以对远程调用的异常做处理**，我们选择这种

这里我们演示方式二的失败降级处理。

**步骤一**：在 **feing-api 项目**中定义类，**实现 FallbackFactory<> 接口：**

![](<assets/1740016789126.png>)

```
import cn.itcast.feign.clients.fallback.UserClientFallbackFactory;
import cn.itcast.feign.pojo.User;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
 
@FeignClient(value = "userservice", fallbackFactory = UserClientFallbackFactory.class)
public interface UserClient {
 
    @GetMapping("/user/{id}")
    User findById(@PathVariable("id") Long id);
}
```

UserClient

```
public interface RequestOriginParser {
    /**
     * 从请求request对象中获取origin，获取方式自定义
     */
    String parseOrigin(HttpServletRequest request);
}
```

**步骤二**：在 **feing-api 项目中**的 config.DefaultFeignConfiguration 配置类中**将 UserClientFallbackFactory 注册为一个 Bean：**

```
package cn.itcast.order.sentinel;
 
import com.alibaba.csp.sentinel.adapter.spring.webmvc.callback.RequestOriginParser;
import org.springframework.stereotype.Component;
import org.springframework.util.StringUtils;
 
import javax.servlet.http.HttpServletRequest;
 
@Component
public class HeaderOriginParser implements RequestOriginParser {
    @Override
    public String parseOrigin(HttpServletRequest request) {
        // 1.获取请求头。来自网关过滤器的请求请求头origin值为gateway
        String origin = request.getHeader("origin");
        // 2.非空判断。如果origin为空则设定值为“blank”
        if (StringUtils.isEmpty(origin)) {
            origin = "blank";
        }
        return origin;
    }
}
```

**步骤三**：在 feing-api 项目中的 **UserClient 接口中使用 UserClientFallbackFactory：**

```
spring:
 cloud:
 gateway:
 default-filters:
 - AddRequestHeader=origin,gateway
 routes:
       # ...略
```

**重启 order-service** 后，访问一次订单查询业务，然后查看 sentinel 控制台，可以看到新的簇点链路：

![](<assets/1740016789343.png>)

#### 3.1.3. 总结整合步骤

Sentinel 支持的雪崩解决方案：

*   线程隔离（仓壁模式）
*   降级熔断

Feign 整合 Sentinel 的步骤：

*   在 application.yml 中配置：feign.sentienl.enable=true
*   给 FeignClient 编写 FallbackFactory 并注册为 Bean
*   将 FallbackFactory 配置到 FeignClient

### 3.2. 线程隔离（舱壁模式）

#### 3.2.1. 线程隔离的实现方式

线程隔离有两种方式实现：

*   **线程池隔离**
    
*   **信号量隔离（Sentinel 默认采用）**
    

如图：

![](<assets/1740016789716.png>)

**线程池隔离**：给每个服务调用业务分配一个线程池，利用线程池本身实现隔离效果

**信号量隔离**：不创建线程池，而是计数器模式，记录业务使用的**线程数量**，达到**信号量上限**时，**禁止新的请求。**

两者的优缺点：

![](<assets/1740016789949.png>)

线程池隔离：

**主动超时**是线程池能主动终止线程池中的某线程。

**异步：**请求不同服务时调用不同线程池，所以能异步调用。

**低扇出：**此服务依赖的服务数量少，就是扇出低，需要创建的线程池数量也少。

#### 3.2.2.sentinel 的线程隔离

**用法说明**：

在**添加限流规则时**，可以选择两种阈值类型：

![](<assets/1740016790265.png>)

*   QPS：就是每秒的请求数，在快速入门中已经演示过
    
*   线程数：是该资源能使用用的 tomcat 线程数的最大值。也就是通过限制线程数量，实现**线程隔离**（舱壁模式）。
    

**案例需求**：给 order-service 服务中的 UserClient 的查询用户接口设置流控规则，线程数不能超过 2。然后利用 jemeter 测试。

**1）配置隔离规则**

选择 feign 接口后面的流控按钮：

![](<assets/1740016790513.png>)

填写表单：

![](<assets/1740016790708.png>)

**2）Jmeter 测试**

选择《阈值类型 - 线程数 < 2》：

![](<assets/1740016791101.png>)

一次发生 10 个请求，有较大概率并发线程数超过 2，而超出的请求会走之前定义的失败降级逻辑。

查看运行结果：

![](<assets/1740016791345.png>)

发现**虽然结果都是通过了，不过部分请求得到的响应是降级返回的 null 信息。**

#### 3.2.3. 总结

线程隔离的两种手段是？

*   信号量隔离
    
*   线程池隔离
    

信号量隔离的特点是？

*   基于计数器模式，简单，开销小

线程池隔离的特点是？

*   基于线程池模式，有额外开销，但隔离控制更强

### 3.3. 熔断降级

熔断降级是解决雪崩问题的重要手段。其思路是由**断路器统计服务调用的异常比例、慢请求比例，**如果**超出阈值**则会**熔断**该服务。即拦截访问该服务的一切请求；而当**服务恢复时**，断路器会**放行**访问该服务的请求。

断路器**控制熔断和放行**是通过**状态机**来完成的：

![](<assets/1740016791615.png>)

**状态机包括三个状态：**

*   **closed：关闭状态**，断路器**放行所有请求**，并开始统计异常比例、慢请求比例。超过阈值则切换到 open 状态
*   **open：打开状态**，服务调用被**熔断**，访问被熔断服务的请求会被拒绝，快速失败，直接走降级逻辑。Open 状态 5 秒后会进入 half-open 状态
*   **half-open：半开状态**，放行一次请求，根据执行结果来判断接下来的操作。
    *   **请求成功：**则切换到 closed 状态
    *   **请求失败：**则切换到 open 状态

断路器熔断策略有三种：慢调用、异常比例、异常数

#### 3.3.1. 慢调用

**慢调用**：业务的**响应时长**（RT，ResponseTime）**大于指定时长的请求**认定为**慢调用请求**。在指定时间内，如果请求数量超过设定的最小数量，**慢调用比例大于设定的阈值，则触发熔断**。

**例如：**

![](<assets/1740016791892.png>)

**解读：**RT 超过 500ms 的调用是慢调用，统计最近 10000ms 内的请求，如果请求量超过 10 次，并且慢调用比例不低于 0.5，则触发熔断，熔断时长为 5 秒。然后进入 half-open 状态，放行一次请求做测试。

**案例**

**需求：**给 UserClient 的查询用户接口设置降级规则，慢调用的 RT 阈值为 50ms，统计时间为 1 秒，最小请求数量为 5，失败阈值比例为 0.4，熔断时长为 5

**1）设置慢调用**

修改 user-service 中的 / user/{id} 这个接口的业务。通过休眠模拟一个延迟时间：

![](<assets/1740016792119.png>)

此时，orderId=101 的订单，关联的是 id 为 1 的用户，调用时长为 60ms：

![](<assets/1740016792463.png>)

orderId=102 的订单，关联的是 id 为 2 的用户，调用时长为非常短；

![](<assets/1740016792739.png>)

**2）设置熔断规则**

下面，给 feign 接口设置降级规则：

![](<assets/1740016793025.png>)

规则：

![](<assets/1740016793291.png>)

超过 50ms 的请求都会被认为是慢请求

**3）测试**

在浏览器访问：http://localhost:8088/order/101，快速刷新 5 次，可以发现：

![](<assets/1740016793497.png>)

触发了熔断，请求时长缩短至 5ms，快速失败了，并且走降级逻辑，返回的 null

在浏览器访问：http://localhost:8088/order/102，竟然也被熔断了：

![](<assets/1740016793746.png>)

#### 3.3.2. 异常比例、异常数

**异常比例或异常数**：统计指定时间内的调用，如果**调用次数超过指定请求数**，并且出现**异常的比例达到设定的比例阈值**（或超过指定异常数），则**触发熔断**。

例如，一个异常比例设置：

![](<assets/1740016794146.png>)

解读：统计最近 1000ms 内的请求，如果请求量超过 10 次，并且异常比例不低于 0.4，则触发熔断。

一个异常数设置：

![](<assets/1740016794506.png>)

解读：统计最近 1000ms 内的请求，如果请求量超过 10 次，并且异常比例不低于 2 次，则触发熔断。

**案例**

需求：给 UserClient 的查询用户接口设置降级规则，统计时间为 1 秒，最小请求数量为 5，失败阈值比例为 0.4，熔断时长为 5s

**1）设置异常请求**

首先，修改 user-service 中的 / user/{id} 这个接口的业务。手动抛出异常，以触发异常比例的熔断：

![](<assets/1740016794778.png>)

也就是说，id 为 2 时，就会触发异常

**2）设置熔断规则**

下面，给 feign 接口设置降级规则：

![](<assets/1740016795015.png>)

规则：

![](<assets/1740016795112.png>)

在 5 次请求中，只要异常比例超过 0.4，也就是有 2 次以上的异常，就会触发熔断。

**3）测试**

在浏览器快速访问：http://localhost:8088/order/102，快速刷新 5 次，触发熔断：

![](<assets/1740016795323.png>)

此时，我们去访问本来应该正常的 103：

![](<assets/1740016795623.png>)

## 4. 授权规则

授权规则可以对请求方来源做判断和控制。

### 4.1. 授权规则

#### 4.1.1. 基本规则

授权规则可以**对调用方的来源做控制**，有白名单和黑名单两种方式。

*   **白名单：**来源（origin）在白名单内的调用者允许访问
    
*   **黑名单：**来源（origin）在黑名单内的调用者不允许访问
    

点击左侧菜单的授权，可以看到授权规则：

![](<assets/1740016795948.png>)

*   **资源名：**就是受保护的资源，例如 / order/{orderId}
    
*   **流控应用：**是来源者的名单，
    
    *   如果是勾选白名单，则名单中的来源被许可访问。
    *   如果是勾选黑名单，则名单中的来源被禁止访问。

比如：

![](<assets/1740016796216.png>)

我们允许请求从 gateway 到 order-service，不允许浏览器访问 order-service，那么白名单中就要填写**网关的来源名称（origin）**。

#### 4.1.2. 如何获取 origin（用来识别请求来自网关）

Sentinel 是通过 RequestOriginParser 这个接口的 parseOrigin 来获取请求的来源的。

```
public interface BlockExceptionHandler {
    /**
     * 处理请求被限流、降级、授权拦截时抛出的异常：BlockException
     */
    void handle(HttpServletRequest request, HttpServletResponse response, BlockException e) throws Exception;
}
```

这个方法的作用就是从 request 对象中，获取请求者的 origin 值并返回。

**默认**情况下，**sentinel** 不管请求者从哪里来，返回值永远是 default，也就是说**一切请求的来源都被认为是一样的值 default。**

因此，我们需要**自定义这个接口的实现**，让**不同的请求，返回不同的 origin**。

例如 **order-service 服务**中，我们定义一个 **RequestOriginParser 的实现类：**

```
package cn.itcast.order.sentinel;
 
import com.alibaba.csp.sentinel.adapter.spring.webmvc.callback.BlockExceptionHandler;
import com.alibaba.csp.sentinel.slots.block.BlockException;
import com.alibaba.csp.sentinel.slots.block.authority.AuthorityException;
import com.alibaba.csp.sentinel.slots.block.degrade.DegradeException;
import com.alibaba.csp.sentinel.slots.block.flow.FlowException;
import com.alibaba.csp.sentinel.slots.block.flow.param.ParamFlowException;
import org.springframework.stereotype.Component;
 
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
 
@Component
public class SentinelExceptionHandler implements BlockExceptionHandler {
    @Override
    public void handle(HttpServletRequest request, HttpServletResponse response, BlockException e) throws Exception {
        String msg = "未知异常";
        int status = 429;
 
        if (e instanceof FlowException) {
            msg = "请求被限流了";
        } else if (e instanceof ParamFlowException) {
            msg = "请求被热点参数限流";
        } else if (e instanceof DegradeException) {
            msg = "请求被降级了";
        } else if (e instanceof AuthorityException) {
            msg = "没有权限访问";
            status = 401;
        }
 
        response.setContentType("application/json;charset=utf-8");
        response.setStatus(status);
        response.getWriter().println("{\"msg\": " + msg + ", \"status\": " + status + "}");
    }
}
```

我们会尝试从 request-header 中获取 origin 值。

#### 4.1.3. 给网关添加请求头

既然获取请求 origin 的方式是从 reques-header 中获取 origin 值，我们必须让**所有从 gateway 路由到微服务的请求都带上 origin 头**。

这个需要利用之前学习的一个 GatewayFilter 来实现，AddRequestHeaderGatewayFilter。

修改 gateway 服务中的 application.yml，添加一个 defaultFilter：

```
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
</dependency>
```

这样，从 gateway 路由的所有请求都会带上 origin 头，值为 gateway。而从其它地方到达微服务的请求则没有这个头。

#### 4.1.4. 配置授权规则

接下来，我们添加一个授权规则，放行 origin 值为 gateway 的请求。

![](<assets/1740016796405.png>)

配置如下：

![](<assets/1740016796619.png>)

现在，我们直接跳过网关，访问 order-service 服务：

![](<assets/1740016796877.png>)

通过网关访问：

![](<assets/1740016797126.png>)

这里 authorization 是前面学网关时添加的自定义全局过滤器 

### 4.2. 自定义异常结果

**默认情况下，发生限流、降级、授权拦截时，都会抛出异常到调用方**。**异常结果都是 flow limmiting（限流）。**这样不够友好，无法得知是限流还是降级还是授权拦截。

#### 4.2.1. 异常类型

而如果要自定义异常时的返回结果，**需要实现 BlockExceptionHandler 接口：**

```
spring:
 cloud:
 sentinel:
 datasource:
 flow:
 nacos:
 server-addr: localhost:8848 # nacos地址
 dataId: orderservice-flow-rules
 groupId: SENTINEL_GROUP
 rule-type: flow # 还可以是：degrade、authority、param-flow
```

这个方法有三个参数：

*   HttpServletRequest request：request 对象
*   HttpServletResponse response：response 对象
*   **BlockException e：被 sentinel 拦截时抛出的异常**

这里的 **BlockException 包含多个不同的子类：**

<table><thead><tr><th><strong>异常</strong></th><th><strong>说明</strong></th></tr></thead><tbody><tr><td>FlowException</td><td>限流异常</td></tr><tr><td>ParamFlowException</td><td>热点参数限流的异常</td></tr><tr><td>DegradeException</td><td>降级异常</td></tr><tr><td>AuthorityException</td><td>授权规则异常</td></tr><tr><td>SystemBlockException</td><td>系统规则异常</td></tr></tbody></table>

#### 4.2.2. 自定义异常处理，BlockExceptionHandler 接口

下面，我们就在 order-service 定义一个自定义异常处理类：

```
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
</dependency>
```

#### 4.2.3. 测试自定义的限流和授权异常

**重启测试**，在不同场景下，会返回不同的异常消息.

**限流：**

![](<assets/1740016797528.png>)

![](<assets/1740016797758.png>)

**授权拦截时：**

![](<assets/1740016798010.png>)

![](<assets/1740016798222.png>)

## 5. 规则持久化，避免规则丢失

现在，**sentinel 的所有规则默认都是内存存储**，**重启后所有规则都会丢失**。在生产环境下，我们必须确保这些规则的**持久化，避免丢失**。

### 5.1. 规则管理模式

规则是否能持久化，取决于规则管理模式，sentinel 支持三种规则管理模式：

*   **原始模式：**Sentinel 的默认模式，将规则保存在内存，重启服务会丢失。
*   **pull 模式**
*   **push 模式**

#### 5.1.1.pull 模式

**pull 模式：**控制台将配置的规则推送到 Sentinel 客户端，而客户端会**将配置规则保存在本地文件或数据库中**。以后会**定时**去本地文件或数据库中**查询规则变化情况**（微服务环境下多个服务都会更改 sentinel 规则），**更新本地规则**。

**缺点**就是定时查询规则变化情况，可能会在规则变化后**不及时更新本地规则**。

![](<assets/1740016798524.png>)

#### 5.1.2.push 模式

**push 模式：**控制台将配置**规则推送到远程配置中心**，例如 Nacos。Sentinel 客户端监听 Nacos，获取配置变更的推送消息，完成本地配置更新。

![](<assets/1740016798768.png>)

### 5.2. 实现 push 模式

### 5.2.1. 修改 order-service 服务

修改 OrderService，让其监听 Nacos 中的 sentinel 规则配置。

具体步骤如下：

#### 1. 引入依赖

在 order-service 中引入 sentinel 监听 nacos 的依赖：

```
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
</dependency>
```

#### 2. 配置 nacos 地址

在 order-service 中的 application.yml 文件配置 nacos 地址及监听的配置信息：

```
spring:
 cloud:
 sentinel:
 datasource:
 flow:
 nacos:
 server-addr: localhost:8848 # nacos地址
 dataId: orderservice-flow-rules
 groupId: SENTINEL_GROUP
 rule-type: flow # 还可以是：degrade、authority、param-flow
```

### 5.2.2. 修改 sentinel-dashboard 源码

SentinelDashboard 默认不支持 nacos 的持久化，需要修改源码。

#### 1. 解压

解压课前资料中的 sentinel 源码包：

![](<assets/1740016798992.png>)

然后并用 IDEA 打开这个项目，结构如下：

![](<assets/1740016799208.png>)

#### 2. 修改 nacos 依赖

在 sentinel-dashboard 源码的 pom 文件中，nacos 的依赖默认的 scope 是 test，只能在测试时使用，这里要去除：

![](<assets/1740016799469.png>)

将 sentinel-datasource-nacos 依赖的 scope 去掉：

```
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
</dependency>
```

#### 3. 添加 nacos 支持

在 sentinel-dashboard 的 test 包下，已经编写了对 nacos 的支持，我们需要将其拷贝到 main 下。

![](<assets/1740016799755.png>)

#### 4. 修改 nacos 地址

然后，还需要修改测试代码中的 NacosConfig 类：

![](<assets/1740016799989.png>)

修改其中的 nacos 地址，让其读取 application.properties 中的配置：

![](<assets/1740016800196.png>)

在 sentinel-dashboard 的 application.properties 中添加 nacos 地址配置：

```
nacos.addr=localhost:8848
```

#### 5. 配置 nacos 数据源

另外，还需要修改 com.alibaba.csp.sentinel.dashboard.controller.v2 包下的 FlowControllerV2 类：

![](<assets/1740016800529.png>)

让我们添加的 Nacos 数据源生效：

![](<assets/1740016800779.png>)

#### 6. 修改前端页面

接下来，还要修改前端页面，添加一个支持 nacos 的菜单。

修改 src/main/webapp/resources/app/scripts/directives/sidebar / 目录下的 sidebar.html 文件：

![](<assets/1740016801132.png>)

将其中的这部分注释打开：

![](<assets/1740016801363.png>)

修改其中的文本：

![](<assets/1740016801711.png>)

#### 7. 重新编译、打包项目

运行 IDEA 中的 maven 插件，编译和打包修改好的 Sentinel-Dashboard：

![](<assets/1740016802045.png>)

#### 8. 启动

启动方式跟官方一样：

```
java -jar sentinel-dashboard.jar
```

如果要修改 nacos 地址，需要添加参数：

```
java -jar -Dnacos.addr=localhost:8848 sentinel-dashboard.jar
```