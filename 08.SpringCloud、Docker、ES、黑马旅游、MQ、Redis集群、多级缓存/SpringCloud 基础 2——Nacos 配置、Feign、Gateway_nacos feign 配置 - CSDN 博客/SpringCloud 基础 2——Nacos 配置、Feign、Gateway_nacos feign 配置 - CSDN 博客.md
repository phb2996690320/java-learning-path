---
url: https://blog.csdn.net/qq_40991313/article/details/126772669?spm=1001.2014.3001.5501
title: SpringCloud 基础 2——Nacos 配置、Feign、Gateway_nacos feign 配置 - CSDN 博客
date: 2025-02-20 09:58:55
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1.Nacos 配置管理](#t0)

[1.1. 统一配置管理](#t1)

[1.1.1. 在 Nacos 中添加配置文件](#t2)

[1.1.2. 从微服务拉取配置，@Value("${xxx}")](#t3)

[1.2. 配置热更新](#t4)

[1.2.1. 方式一（推荐），类注解 @RefreshScope](#t5)

[1.2.2. 方式二，@ConfigurationProperties，@Component](#t6)

[1.3. 配置共享](#t7)

[1.3.1. 创建共享配置和读取](#t8)

[1.3.2. 配置共享的优先级](#t9)

[1.4. 搭建 Nacos 集群](#t10)

[1.5. 其他](#t11)

[1.5.1. 命名空间](#t12)

[1.5.2. 根据服务创建命名空间，各命名空间根据环境分组](#t13)

[1.5.3. 按类型抽取配置，加载多配置集](#t14) 

[2.Feign 远程调用](#t15)

[2.0.RestTemplate 存在问题](#t16)

[2.1.Feign 替代 RestTemplate](#t17)

[2.1.1. 引入依赖](#t18)

[2.1.2. 开启 feign，添加注解 @EnableFeignClients](#t19)

[2.1.3. 编写 Feign 的客户端，@FeignClient](#t20)

[2.1.4. 远程调用](#t21)

[2.2. 自定义配置](#t22)

[2.2.1. 配置文件方式自定义配置](#t23)

[2.2.2.Java 代码方式自定义配置](#t24)

[2.3.Feign 性能优化，HttpClient](#t25)

[2.4. 最佳实践](#t26)

[2.4.1. 继承方式（高耦合不推荐）](#t27)

[2.4.2. 抽取方式（推荐）](#t28)

[3.Gateway 服务网关](#t29)

[3.1. 网关概述](#t30)

[3.2.gateway 快速入门](#t31)

[3.2.1. 创建 gateway 服务，引入依赖](#t32)

[3.2.2. 编写启动类](#t33)

[3.2.3. 编写基础配置和路由规则](#t34)

[3.2.4. 重启测试](#t35)

[5）网关路由的流程图](#t36)

[3.3. 断言工厂](#t37)

[3.4. 网关过滤器工厂](#t38)

[3.4.1. 路由过滤器的种类](#t39)

[3.4.2. 请求头过滤器，AddRequestHeader](#t40)

[3.4.3. 默认过滤器，default-filters](#t41)

[3.4.4. 总结](#t42)

[3.5. 全局过滤器，GlobalFilter](#t43)

[3.5.1. 全局过滤器作用](#t44)

[3.5.2. 自定义全局过滤器，过滤未登录](#t45)

[3.5.3. 过滤器执行顺序](#t46)

[3.6. 跨域问题](#t47)

[3.6.1. 什么是跨域问题](#t48)

[3.6.2. 模拟跨域问题](#t49)

[3.6.3. 解决跨域问题，CORS](#t50)

## 1.Nacos 配置管理

Nacos 除了可以做注册中心，同样可以做配置管理来使用。

### 1.1. 统一配置管理

当微服务部署的实例越来越多，达到数十、数百时，逐个修改微服务配置就会让人抓狂，而且很容易出错。我们需要一种统一配置管理方案，可以集中管理所有实例的配置。

![](<assets/1740016736014.png>)

**Nacos** 一方面可以**将配置集中管理**，另一方面可以在**配置变更时**，及时**通知微服务**，实现**配置的热更新。**

#### 1.1.1. 在 Nacos 中添加配置文件

如何在 nacos 中管理配置呢？

![](<assets/1740016736372.png>)

然后在弹出的表单中，填写配置信息：

![](<assets/1740016736690.png>)

这里分组名称默认 DEFAULT_GROUP，我们看服务列表也可以看到所有服务默认都在这个分组：

![](<assets/1740016736975.png>)

**注意：**

*   项目的核心配置，需要**热更新的配置才有放到 nacos 管理的必要**。基本**不会变更的**一些**配置**还是**保存在微服务本地**比较好。
*   配置写 yaml，而不是 yml。

#### 1.1.2. 从微服务拉取配置，@Value("${xxx}")

微服务要拉取 nacos 中管理的配置，并且与本地的 application.yml 配置合并，才能完成项目启动。

**问题：nacos 配置要先于 yml，但如果尚未读取 yml，又如何得知 nacos 地址并获取 nacos 配置呢？**

**答案：**因此 spring 引入了一种新的配置文件：**bootstrap.yaml 文件**，优先级高于 application.yml，**会在 application.yml 之前被读取**。流程如下：

bootstrap 译为引导

![](<assets/1740016737236.png>)

**1）引入 nacos-config 依赖**

首先，在 user-service 服务中，引入 nacos-config 的客户端依赖：

```
<!--nacos配置管理依赖-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
<!--        在SpringBoot 2.4.x的版本之后，对于bootstrap.properties/bootstrap.yaml配置文件(我们合起来成为Bootstrap配置文件)的支持，需要导入如下的依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-bootstrap</artifactId>
            <version>3.1.4</version>
        </dependency>
```

**2）添加 bootstrap.yaml**

然后，在 user-service 中添加一个 bootstrap.yaml 文件，需要**配置服务名、开发环境、nacos 地址、后缀名**，对应 nacos 添加的配置文件名。内容如下：

```
spring:
 application:
 name: userservice # 服务名称
 profiles:
 active: dev #开发环境，这里是dev 
 cloud:
 nacos:
 server-addr: localhost:8848 # Nacos地址
 config:
 file-extension: yaml # 文件后缀名
```

这里会根据 spring.cloud.nacos.server-addr 获取 nacos 地址，再根据

`${spring.application.name}-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension}`作为文件 id，来读取配置。

本例中，就是去读取`userservice-dev.yaml`：

![](<assets/1740016737680.png>)

**2.5）删除 application.yml 中的重复配置**，包括服务名称、nacos 地址、开发环境

**3）读取 nacos 配置，@Value**

在 user-service 中的 UserController 中添加业务逻辑，读取 pattern.dateformat 配置：

![](<assets/1740016737952.png>)

**完整代码：**

```
@Slf4j
@RestController
@RequestMapping("/user")
public class UserController {
 
    @Autowired
    private UserService userService;
 
    @Value("${pattern.dateformat}")
    private String dateformat;
    
    @GetMapping("now")
    public String now(){
        return LocalDateTime.now().format(DateTimeFormatter.ofPattern(dateformat));
    }
    // ...略
}
```

在页面访问，可以看到效果：

![](<assets/1740016738245.png>)

### 1.2. 配置热更新

我们最终的目的，是修改 nacos 中的配置后，微服务中无需重启即可让配置生效，也就是**配置热更新**。

要实现配置热更新，可以使用两种方式：

#### 1.2.1. 方式一（推荐），类注解 @RefreshScope

在 @Value 注入的变量所在类上添加注解 @RefreshScope：

![](<assets/1740016738442.png>)

#### 1.2.2. 方式二，@ConfigurationProperties，@Component

在 user-service 服务中，添加一个类，读取 patterrn.dateformat 属性：

```
@Component
@Data
@ConfigurationProperties(prefix = "pattern")
public class PatternProperties {
    private String dateformat;
}
```

在 UserController 中使用这个类代替 @Value：

![](<assets/1740016738658.png>)

完整代码：

```
@Slf4j
@RestController
@RequestMapping("/user")
public class UserController {
 
    @Autowired
    private UserService userService;
 
    @Autowired
    private PatternProperties patternProperties;
 
    @GetMapping("now")
    public String now(){
        return LocalDateTime.now().format(DateTimeFormatter.ofPattern(patternProperties.getDateformat()));
    }
 
    // 略
}
```

### 1.3. 配置共享

#### 1.3.1. 创建共享配置和读取

其实微服务启动时，会去 nacos 读取多个配置文件，例如：

*   `[spring.application.name]-[spring.profiles.active].yaml`，例如：userservice-dev.yaml
*   **`[spring.application.name].yaml`，例如：userservice.yaml**

而`[spring.application.name].yaml`不包含环境，因此可以被多个环境共享。

下面我们通过案例来测试配置共享

**1）添加一个环境共享配置**

我们在 nacos 中添加一个 userservice.yaml 文件：

![](<assets/1740016738919.png>)

**2）在 user-service 中读取共享配置**

在 user-service 服务中，修改 PatternProperties 类，读取新添加的属性：

![](<assets/1740016739154.png>)

在 user-service 服务中，修改 UserController，添加一个方法：

![](<assets/1740016739469.png>)

**3）测试。运行两个 UserApplication，使用不同的 profile**

修改 UserApplication2 这个启动项，改变其 profile 值：

![](<assets/1740016739743.png>)

![](<assets/1740016740105.png>)

这样，**UserApplication(8081) 使用的 profile 是 dev，UserApplication2(8082) 使用的 profile 是 test**。

启动 UserApplication 和 UserApplication2

访问 **dev 环境的** [http://localhost:8081/user/prop](http://localhost:8081/user/prop "http://localhost:8081/user/prop")，结果：**共享配置和 dev 环境的配置都可以读到**

![](<assets/1740016740344.png>)

访问 **test 环境**的 [http://localhost:8082/user/prop](http://localhost:8082/user/prop "http://localhost:8082/user/prop")，结果：读不到 dev 环境配置，能读到共享配置

![](<assets/1740016740548.png>)

可以看出来，不管是 dev，还是 test 环境，**都读取到了 envSharedValue 这个属性的值**。

#### 1.3.2. 配置共享的优先级

当 nacos、服务本地同时出现相同属性时，优先级有高低之分：

**nacos 的环境 > nacos 的共享 > 本地**

![](<assets/1740016740937.png>)

### 1.4. 搭建 Nacos 集群

Nacos 生产环境下一定要部署为集群状态

![](<assets/1740016741177.png>)

**搭建集群的基本步骤：**

*   **搭建数据库，初始化数据库表结构**
    
*   **下载 nacos 安装包**
    
*   **配置 nacos**
    

进入 nacos 的 conf 目录，修改配置文件 cluster.conf.example，重命名为 cluster.conf：

```
127.0.0.1:8845
127.0.0.1.8846
127.0.0.1.8847
```

修改 application.properties 文件

```
spring.datasource.platform=mysql
 
db.num=1
 
db.url.0=jdbc:mysql://127.0.0.1:3306/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useUnicode=true&useSSL=false&serverTimezone=UTC
db.user.0=root
db.password.0=123
```

*   **启动 nacos 集群**
    

将 nacos 文件夹复制三份，分别命名为：nacos1、nacos2、nacos3

![](<assets/1740016741471.png>)

然后分别修改三个文件夹中的 application.properties，

nacos1:

```
upstream nacos-cluster {
    server 127.0.0.1:8845;
	server 127.0.0.1:8846;
	server 127.0.0.1:8847;
}
 
server {
    listen       80;
    server_name  localhost;
 
    location /nacos {
        proxy_pass http://nacos-cluster;
    }
}
```

nacos2:

```
spring:
 cloud:
 nacos:
 server-addr: localhost:80 # Nacos地址
```

nacos3:

```
spring:
 application:
 name: gulimall-coupon
 cloud:
 nacos:
 config:
 server-addr: 127.0.0.1:8848
 file-extension: yaml
 namespace: xxxx
 extension-configs:
 - data-id: datasource.yml
 group: dev
 refresh: true
```

然后分别启动三个 nacos 节点：

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

*   **nginx 反向代理**
    

修改 conf/nginx.conf 文件，配置如下：

```
@MapperScan("cn.itcast.order.mapper")
@SpringBootApplication
@EnableFeignClients
public class OrderApplication {
 
    public static void main(String[] args) {
        SpringApplication.run(OrderApplication.class, args);
    }
//    @Bean
//    @LoadBalanced
//    public RestTemplate restTemplate(){
//        return new RestTemplate();
//    }
 
}
```

而后在浏览器访问：[http://localhost/nacos](http://localhost/nacos "http://localhost/nacos") 即可。

代码中 application.yml 文件配置如下：

```
package cn.itcast.order.clients;
 
@FeignClient("userservice")
public interface UserClient {
    @GetMapping("/user/{id}")
    User findById(@PathVariable("id") Long id);
}
```

### 1.5. 其他

#### 1.5.1. 命名空间

![](<assets/1740016741805.png>)

命名空间用于将开发测试生产三种**环境**、**或者**各**微服务**之间**配置隔离**。默认命名空间是 public：

![](<assets/1740016742050.png>)

**不同命名空间设置不同配置：** 

![](<assets/1740016742318.png>)

**指定命名空间：** 

![](<assets/1740016742644.png>)

#### 1.5.2. 根据服务创建命名空间，各命名空间根据环境分组

![](<assets/1740016742927.png>)

![](<assets/1740016743214.png>)

![](<assets/1740016743523.png>)

#### 1.5.3. 按类型抽取配置，加载多配置集 

![](<assets/1740016743808.png>)

开发时为了方便可以将配置文件写在项目中，等发布后再抽取到 nacos 中。 

**将 datasource 相关配置抽取成一个配置**，在 coupon 命名空间下，分组名为 dev（根据环境分组） ：

![](<assets/1740016744049.png>)

使用：

![](<assets/1740016744274.png>)

**或者 bootstrap.yml** 

```
feign:  
 client:
 config: 
 userservice: # 针对某个微服务的配置
 loggerLevel: FULL #  日志级别
```

## 2.Feign 远程调用

### 2.0.RestTemplate 存在问题

先来看我们以前利用 **RestTemplate** 发起远程调用的代码：

![](<assets/1740016744588.png>)

**存在问题：**

• 代码可读性差，编程体验不统一

**• 参数复杂 URL 难以维护**

**Feign 是一个声明式的 http 客户端**，官方地址：[https://github.com/OpenFeign/feign](https://github.com/OpenFeign/feign "https://github.com/OpenFeign/feign")

其作用就是帮助我们**优雅的实现 http 请求的发送**，解决上面提到的问题。

![](<assets/1740016744808.png>)

### 2.1.Feign 替代 RestTemplate

Fegin 的使用步骤如下：

#### 2.1.1. 引入依赖

我们在 **order-service** 服务的 pom 文件中引入 feign 的依赖：

```
feign:  
 client:
 config: 
 default: # 这里用default就是全局配置，如果是写服务名称，则是针对某个微服务的配置
 loggerLevel: FULL #  日志级别
```

#### 2.1.2. 开启 feign，添加注解 @EnableFeignClients

在 **order-service** 的启动类添加注解开启 Feign 的功能：

```
public class DefaultFeignConfiguration {
    @Bean
    public Logger.Level feignLogLevel(){
        return Logger.Level.BASIC; // 日志级别为BASIC
    }
}
```

#### 2.1.3. 编写 Feign 的客户端，@FeignClient

在 **order-service** 中新建一个接口，内容如下：

```
<!--httpClient的依赖 -->
<dependency>
    <groupId>io.github.openfeign</groupId>
    <artifactId>feign-httpclient</artifactId>
</dependency>
```

**声明远程调用的需要的信息**，比如：

*   **服务名称：userservice**
*   **请求方式：GET**
*   **请求路径：/user/{id}**
*   **请求参数：Long id**
*   **返回值类型：User**

![](<assets/1740016745104.png>) 

这样，Feign 就可以帮助我们发送 http 请求，无需自己使用 RestTemplate 来发送了。

#### 2.1.4. 远程调用

修改 order-service 中的 **OrderService** 类中的 queryOrderById 方法，使用 Feign 客户端代替 RestTemplate：

![](<assets/1740016745193.png>)

是不是看起来优雅多了。

**总结**

**使用 Feign 的步骤：**

① 引入依赖

② 添加 @EnableFeignClients 注解

③ 编写 FeignClient 接口

④ 使用 FeignClient 中定义的方法代替 RestTemplate

### 2.2. 自定义配置

Feign 可以支持很多的自定义配置，如下表所示：

<table><thead><tr><th>类型</th><th>作用</th><th>说明</th></tr></thead><tbody><tr><td><strong>feign.Logger.Level</strong></td><td>修改日志级别</td><td>包含四种不同的级别：NONE、BASIC、HEADERS、FULL</td></tr><tr><td>feign.codec.Decoder</td><td>响应结果的解析器</td><td>http 远程调用的结果做解析，例如解析 json 字符串为 java 对象</td></tr><tr><td>feign.codec.Encoder</td><td>请求参数编码</td><td>将请求参数编码，便于通过 http 请求发送</td></tr><tr><td>feign. Contract</td><td>支持的注解格式</td><td>默认是 SpringMVC 的注解</td></tr><tr><td>feign. Retryer</td><td>失败重试机制</td><td>请求失败的重试机制，默认是没有，不过会使用 Ribbon 的重试</td></tr></tbody></table>

一般情况下，**默认值就能满足我们使用，一般只需要配置日志级别为 basic。**如果要自定义时，只需要创建自定义的 @Bean 覆盖默认 Bean 即可。

下面以日志为例来演示如何自定义配置。

#### 2.2.1. 配置文件方式自定义配置

基于配置文件修改 feign 的日志级别可以针对单个服务：

```
feign:
 client:
 config:
 default: # default全局的配置
 loggerLevel: BASIC # 日志级别，BASIC就是基本的请求和响应信息
 httpclient:
 enabled: true # 开启feign对HttpClient的支持
 max-connections: 200 # 最大的连接数
 max-connections-per-route: 50 # 每个路径的最大连接数
```

也可以针对所有服务：

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

日志的级别分为四种：

*   **NONE（默认）：**不记录任何日志信息，这是**默认值**。
*   **BASIC：**仅记录请求的方法，URL 以及响应状态码和执行时间
*   **HEADERS：**在 BASIC 的基础上，额外记录了请求和响应的头信息
*   **FULL：**记录所有请求和响应的明细，包括头信息、请求体、元数据。

#### 2.2.2.Java 代码方式自定义配置

也可以基于 Java 代码来修改日志级别，先声明一个类，然后声明一个 Logger.Level 的对象：

```
<dependency>
    <groupId>cn.itcast.demo</groupId>
    <artifactId>feign-api</artifactId>
    <version>1.0</version>
</dependency>
```

如果要**全局生效**，将其放到启动类的 @EnableFeignClients 这个注解中：

```
<!--网关-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
<!--nacos服务发现依赖-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

如果是**局部生效**，则把它放到对应的 @FeignClient 这个注解中：

```
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-alibaba-dependencies</artifactId>
    <version>2.2.6.RELEASE</version>
    <type>pom</type>
    <scope>import</scope>
</dependency>
```

### 2.3.Feign 性能优化，HttpClient

Feign 底层发起 http 请求，依赖于其它的框架。

**Feign 底层客户端实现包括：**

**•URLConnection（默认，不推荐）：默认实现，不支持连接池，三次握手伤害性能**

**•Apache HttpClient ：支持连接池**

**•OKHttp：支持连接池**

因此提高 Feign 的性能主要手段就是使用**连接池**代替默认的 URLConnection。

这里我们用 Apache 的 HttpClient 来演示。

**1）引入依赖**

在 order-service 的 pom 文件中引入 Apache 的 HttpClient 依赖：

```
@SpringBootApplication
public class GatewayApplication {
 
	public static void main(String[] args) {
		SpringApplication.run(GatewayApplication.class, args);
	}
}
```

**2）配置连接池**

在 order-service 的 application.yml 中添加配置：

```
server:
 port: 10010 # 网关端口
spring:
 application:
 name: gateway # 服务名称
 cloud:
    #网关需要被nacos注册中心管理
 nacos:
 server-addr: localhost:8848 # nacos地址
 gateway:
 routes: # 网关路由配置
        #第一组网关路由配置，针对于服务user-service
 - id: userservice # 路由id，自定义，只要唯一即可
 uri: lb://userservice # 路由的目标地址。lb就是负载均衡，后面跟服务名称。
 predicates: # 路由断言。也就是判断请求是否符合路由规则的条件。predicates译为谓语、断言
 - Path=/user/** # 路径断言。这个是按照路径匹配，只要以/user/开头就符合要求
#之后访问http://localhost:10010/user/1就路由（转发）到lb://userservice/1，注册拉取并负载均衡后为http://localhost:8081
        #第二组网关路由配置，针对于服务order-service
 - id: order-service
 uri: lb://order-service
 predicates:
 - Path=/order/**
```

实际应用中，需要不断调试测出最合适的连接数。

接下来，在 FeignClientFactoryBean 中的 loadBalance 方法中打断点：

![](<assets/1740016745401.png>)

Debug 方式启动 order-service 服务，可以看到这里的 client，底层就是 Apache HttpClient：

![](<assets/1740016745640.png>)

**总结，Feign 的优化：**

*   **1. 日志级别尽量用 basic**
*   **2. 使用 HttpClient 或 OKHttp** 代替 URLConnection

① 引入 feign-httpClient 依赖

② 配置文件开启 httpClient 功能，设置连接池参数

### 2.4. 最佳实践

所谓最近实践，就是使用过程中总结的经验，最好的一种使用方式。

观察可以发现，**Feign 的客户端与服务提供者的 controller 代码非常相似**：

feign 客户端：

![](<assets/1740016745910.png>)

UserController：

![](<assets/1740016746314.png>)

有没有一种办法**简化这种重复的代码编**写呢？

#### 2.4.1. 继承方式（高耦合不推荐）

一样的代码可以通过**继承来共享：**

1）定义一个 API 接口，利用定义方法，并基于 SpringMVC 注解做声明。

2）Feign 客户端和 Controller 都集成改接口

![](<assets/1740016746602.png>)

**优点：**

*   简单
*   实现了代码共享

**缺点：**

*   服务提供方、服务消费方**紧耦合**
*   参数列表中的注解映射并不会继承，因此 Controller 中必须再次声明方法、参数列表、注解

#### 2.4.2. 抽取方式（推荐）

**将 Feign 的 Client 抽取为独立模块**，并且**把接口有关的 POJO**、**默认的 Feign 配置**都**放到这个模块**中，提供给所有消费者使用。

**例如**，将 order-service 模块的 UserClient、User 实体类、Feign 的默认配置都**抽取到一个 feign-api 包中**，所有微服务引用该依赖包，即可直接使用。

![](<assets/1740016746798.png>)

**抽取方式代码实现，包扫描问题**

**1）抽取**

首先创建一个 module，命名为 feign-api：

![](<assets/1740016747111.png>)

项目结构：cn.itcast.feign

![](<assets/1740016747349.png>)

在 feign-api 中然后**引入 feign 的 starter 依赖**

```
spring:
 cloud:
 gateway:
 routes:
 - id: user-service 
 uri: lb://userservice 
 predicates: 
 - Path=/user/** 
 filters: # 过滤器
 - AddRequestHeader=Truth, Itcast is freaking awesome! # 添加请求头
```

然后，order-service 中编写的 **UserClient、User、yml 的 DefaultFeignConfiguration** 都复制到 feign-api 项目中

![](<assets/1740016747568.png>)

**2）在 order-service 中使用 feign-api**

首先，**删除 order-service 中的 UserClient、User、DefaultFeignConfiguration 等类或接口**。

在 order-service 的 pom 文件中中**引入 feign-api 的依赖：**

```
spring:
 cloud:
 gateway:
 routes:
 - id: user-service 
 uri: lb://userservice 
 predicates: 
 - Path=/user/**
 default-filters: # 默认过滤项
 - AddRequestHeader=Truth, Itcast is freaking awesome!
```

修改 order-service 中的所有与上述三个组件有关的导包部分，改成导入 feign-api 中的包

**3）bean 扫描问题导致测试报错**

重启后，发现服务报错了：

![](<assets/1740016747810.png>)

这是**因为 UserClient 现在在 cn.itcast.feign.clients 包下**，

而 order-service 的 **@EnableFeignClients 注解是在 cn.itcast.order 包下**，**不在同一个包，无法扫描到 UserClient。**

**4）解决扫描包问题**

**方式一：扫描整个包**

**指定 Feign 应该扫描的包：**

```
public interface GlobalFilter {
    /**
     *  处理当前请求，有必要的话通过{@link GatewayFilterChain}将请求交给下一个过滤器处理
     *
     * @param exchange 请求上下文，里面可以获取Request、Response等信息
     * @param chain 用来把请求委托给下一个过滤器 
     * @return {@code Mono<Void>} 返回标示当前过滤器业务结束
     */
    Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain);
}
```

**方式二（推荐）：扫描指定 clients**

**指定需要加载的 Client 接口：**

```
package cn.itcast.gateway.filters;
//注解@Order设置过滤优先级，越小优先级越高。也可以实现Ordered接口的getOrder()方法
@Order(-1)
@Component
public class AuthorizeFilter implements GlobalFilter {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        // 1.获取请求参数。 MultiValueMap的add方法是一个key对应多个值，set方法是一个key对应一个值
        MultiValueMap<String, String> params = exchange.getRequest().getQueryParams();
        // 2.获取authorization参数
        String auth = params.getFirst("authorization");
        // 3.校验
        if ("admin".equals(auth)) {
            // 放行
            return chain.filter(exchange);
        }
        // 4.拦截
        // 4.1.禁止访问，设置状态码，401未登录unauthorized，
        //exchange.getResponse().setRawStatusCode(401);
        exchange.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED);
        // 4.2.结束处理
        return exchange.getResponse().setComplete();
    }
}
```

## 3.Gateway 服务网关

Spring Cloud Gateway 是 Spring Cloud 的一个全新项目，该项目是基于 Spring 5.0，Spring Boot 2.0 和 Project Reactor 等响应式编程和事件流技术开发的网关，它**旨在为微服务架构提供一种简单有效的统一的 API 路由管理方式。**

### 3.1. 网关概述

Gateway 网关是我们**服务的守门神**，**所有微服务的统一入口**。

gateway 译为网关，门口，通道 

网关的**核心功能特性**：

*   **请求路由**
*   **权限控制**
*   **限流**

架构图：

![](<assets/1740016748015.png>)

**权限控制**：网关作为微服务入口，需要校验用户是是否有请求资格，如果没有则进行拦截。

**路由和负载均衡**：一切请求都必须先经过 gateway，但网关不处理业务，而是根据某种规则，把请求转发到某个微服务，这个过程叫做路由。当然路由的目标服务有多个时，还需要做负载均衡。

**限流**：当请求流量过高时，在网关中按照下流的微服务能够接受的速度来放行请求，避免服务压力过大。

在 **SpringCloud 中网关的实现包括两种：**

*   **zuul（阻塞式编程）**
*   **gateway（推荐，响应式编程）**

Zuul 是基于 Servlet 的实现，属于阻塞式编程。而 SpringCloudGateway 则是基于 Spring5 中提供的 WebFlux，属于响应式编程的实现，具备更好的性能。

**扩展：阻塞式编程和响应式编程**

阻塞式编程和响应式编程是两种不同的编程范式，它们在处理异步操作时有着不同的理念和实现方式。

**阻塞式编程：**

1.  **同步阻塞模型：** 在阻塞式编程中，程序执行会被阻塞（暂停）直到某个操作完成。这意味着当一个线程执行一个可能耗时的操作时，整个程序会被阻塞，直到该操作完成。
    
2.  **等待模型：** 阻塞式编程通常采用等待模型，即程序等待外部资源（例如文件、网络请求、数据库查询等）返回结果。在等待期间，线程处于阻塞状态，不能执行其他任务。
    

**响应式编程：**

1.  **异步非阻塞模型：** 响应式编程采用异步非阻塞的模型。异步意味着操作不会阻塞程序的执行，而是在**后台执行**。非阻塞意味着在等待结果的同时，程序可以继续执行其他任务。
    
2.  **事件驱动：** 响应式编程通常是事件驱动的。程序通过订阅事件或观察者模式来响应数据变化或操作完成的通知，而不是通过主动轮询或等待。
    

**多线程和异步的区别**

*   多线程涉及在一个进程中创建多个线程，而异步编程涉及以非阻塞方式执行任务。因此，异步可以是单线程异步（例如洗盘子，虽然我同时只能洗一个盘子，但我一个个洗完后把所有废水统一倒掉，因此在单个任务中完成了多个子任务，时间得到了优化。），也可以是多线程异步。
*   多线程需要显式地管理线程同步和通信，而异步编程可以使用回调或承诺 (callbacks/promises) 等编程结构来处理异步操作。）

**对比：**

*   **并发性：**
    *   阻塞式编程通常需要使用多线程来处理并发，而响应式编程通过异步处理来支持高并发。
*   **资源利用率：**
    *   阻塞式编程可能会导致线程资源浪费，因为每个线程在等待时都不能执行其他任务。响应式编程通过异步非阻塞模型更有效地利用系统资源。
*   **复杂性：**
    *   阻塞式编程可能导致代码复杂性增加，因为需要处理线程同步和阻塞的问题。响应式编程通常可以更简洁地处理异步操作。
*   **适用场景：**
    *   阻塞式编程适用于简单的同步操作，而响应式编程更适用于处理大量异步事件，例如网络请求、实时数据流等。

### 3.2.gateway 快速入门

**网关的基本路由功能，基本步骤如下：**

1.  创建 SpringBoot 工程 gateway，引入网关依赖
2.  编写启动类
3.  编写基础配置和路由规则
4.  启动网关服务进行测试

#### 3.2.1. 创建 gateway 服务，引入依赖

创建服务：

![](<assets/1740016748414.png>)

**引入依赖：**

```
@WebFilter(filterName = "loginCheckFilter",urlPatterns = "/*")
@Slf4j
public class LoginCheckFilter implements Filter {
    public static final AntPathMatcher PATH_MATCHER = new AntPathMatcher();
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;
        String[] notFilter = new String[]{
                "/employee/login",
                "/employee/logout",
                "/backend/**",
                "/front/**",
                "/common/**",
                "/user/sendMsg",
                "/user/login"
        };
        String nowUri = request.getRequestURI();
//        放行不需要拦截的uri
        for(String url:notFilter){
            if(PATH_MATCHER.match(url, nowUri)){
                filterChain.doFilter(request,response);
                return;
            }
        }
//        session里有employee则已登录，放行。针对于后端
        if(request.getSession().getAttribute("employee")!=null){
            BaseContext.setThreadId((Long) request.getSession().getAttribute("employee"));
            filterChain.doFilter(request,response);
 
            return;
        }
        //session里有user则已登录，放行。针对于前端
        if(request.getSession().getAttribute("user")!=null){
            BaseContext.setThreadId((Long) request.getSession().getAttribute("user"));
            filterChain.doFilter(request,response);
            return;
        }
//        未登录返回错误信息，前端设置跳转
        response.getWriter().write(JSON.toJSONString(R.error("NOTLOGIN")));
    }
}
```

父工程管理依赖 <dependencyManagement> 有：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
<pre>
spring:
  cloud:
    gateway:
      globalcors: # 全局的跨域处理
        add-to-simple-url-handler-mapping: true # 解决options请求被拦截问题
        corsConfigurations:
          '[/**]':
            allowedOrigins: # 允许哪些网站的跨域请求
              - "http://localhost:8090"
              - "http://www.leyou.com"
            allowedMethods: # 允许的跨域ajax的请求方式
              - "GET"
              - "POST"
              - "DELETE"
              - "PUT"
              - "OPTIONS"
            allowedHeaders: "*" # 允许在请求中携带的头信息
            allowCredentials: true # 是否允许携带cookie
            maxAge: 360000 # 这次跨域检测的有效期
</pre>
</body>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script> axios.get("http://localhost:10010/user/1?authorization=admin")
  .then(resp => console.log(resp.data))
  .catch(err => console.log(err)) </script>
</html>
```

所以子模块 getway 引入 spring-cloud-starter-alibaba-nacos-discovery 不需要指定版本

#### 3.2.2. 编写启动类

cn.itcast.gateway 包下 

```
spring:
 cloud:
 gateway:
      # 。。。
 globalcors: # 全局的跨域处理
 add-to-simple-url-handler-mapping: true # 解决options请求被拦截问题
 corsConfigurations:
          '[/**]':
 allowedOrigins: # 允许哪些网站的跨域请求 
 - "http://localhost:8090"
 allowedMethods: # 允许的跨域ajax的请求方式
 - "GET"
 - "POST"
 - "DELETE"
 - "PUT"
 - "OPTIONS"
 allowedHeaders: "*" # 允许在请求中携带的头信息
 allowCredentials: true # 是否允许携带cookie
 maxAge: 360000 # 这次跨域检测的有效期
```

#### 3.2.3. 编写基础配置和路由规则

创建 application.yml 文件，内容如下：

```
server:
 port: 10010 # 网关端口
spring:
 application:
 name: gateway # 服务名称
 cloud:
    #网关需要被nacos注册中心管理
 nacos:
 server-addr: localhost:8848 # nacos地址
 gateway:
 routes: # 网关路由配置
        #第一组网关路由配置，针对于服务user-service
 - id: userservice # 路由id，自定义，只要唯一即可
 uri: lb://userservice # 路由的目标地址。lb就是负载均衡，后面跟服务名称。
 predicates: # 路由断言。也就是判断请求是否符合路由规则的条件。predicates译为谓语、断言
 - Path=/user/** # 路径断言。这个是按照路径匹配，只要以/user/开头就符合要求
#之后访问http://localhost:10010/user/1就路由（转发）到lb://userservice/1，注册拉取并负载均衡后为http://localhost:8081
        #第二组网关路由配置，针对于服务order-service
 - id: order-service
 uri: lb://order-service
 predicates:
 - Path=/order/**
```

我们将符合`Path` 规则的一切请求，都代理到 `uri`参数指定的地址。

本例中，我们将 `/user/**`开头的请求，代理到`lb://userservice`，lb 是负载均衡，根据服务名拉取服务列表，实现负载均衡。

#### 3.2.4. 重启测试

启动 gateway 和 userservice，访问 [http://localhost:10010/user/1](http://localhost:10010/user/1 "http://localhost:10010/user/1") 时，符合`/user/**`规则，请求转发到 uri：[http://userservice/user/1](http://userservice/user/1 "http://userservice/user/1")，得到了结果：

![](<assets/1740016748639.png>)

#### 5）网关路由的流程图

整个访问的流程如下：

![](<assets/1740016748885.png>)

**总结：**

**网关搭建步骤：**

1.  创建项目，引入 nacos 服务发现和 gateway 依赖
2.  配置 application.yml，包括服务基本信息、nacos 地址、路由

**路由配置包括：**

1.  路由 id：路由的唯一标示
2.  路由目标（uri）：路由的目标地址，http 代表固定地址，lb 代表根据服务名负载均衡
3.  路由断言（predicates）：判断路由的规则，
4.  路由过滤器（filters）：对请求或响应做处理

接下来，就重点来学习路由断言和路由过滤器的详细知识

### 3.3. 断言工厂

**我们只需要掌握 Path 这种路由工程就可以了。**

predicates 译为断言、谓语。

**断言**是程序中的一阶逻辑，目的为了表示与验证软件开发者预期的结果——当程序执行到断言的位置时，对应的断言应该为真。若断言不为真时，程序会中止执行，并给出错误信息。

**路由断言。**也就是判断请求是否符合路由规则的条件。

**断言工厂 Predicate Factory 的作用：**

**读取并处理 yml 配置的断言规则**，**将断言规则解析为路由判断的条件**

例如 Path=/user/** 是按照路径匹配，这个规则是由

`org.springframework.cloud.gateway.handler.predicate.**PathRoutePredicateFactory**`类来

处理的，像这样的断言工厂在 SpringCloudGateway 还有十几个:

**断言工厂包括：** 

<table><thead><tr><th><strong>名称</strong></th><th><strong>说明</strong></th><th><strong>示例</strong></th></tr></thead><tbody><tr><td>After</td><td>是某个时间点后的请求</td><td>- After=2037-01-20T17:42:47.789-07:00[America/Denver]</td></tr><tr><td>Before</td><td>是某个时间点之前的请求</td><td>- Before=2031-04-13T15:14:47.433+08:00[Asia/Shanghai]</td></tr><tr><td>Between</td><td>是某两个时间点之前的请求</td><td>- Between=2037-01-20T17:42:47.789-07:00[America/Denver], 2037-01-21T17:42:47.789-07:00[America/Denver]</td></tr><tr><td>Cookie</td><td>请求必须包含某些 cookie</td><td>- Cookie=chocolate, ch.p</td></tr><tr><td>Header</td><td>请求必须包含某些 header</td><td>- Header=X-Request-Id, \d+</td></tr><tr><td>Host</td><td>请求必须是访问某个 host（域名）</td><td>- Host=<strong>.somehost.org,</strong>.anotherhost.org</td></tr><tr><td>Method</td><td>请求方式必须是指定方式</td><td>- Method=GET,POST</td></tr><tr><td><strong>Path</strong></td><td><strong>请求路径</strong>必须符合指定规则</td><td><strong>- Path=</strong>/red/{segment},/blue/**</td></tr><tr><td>Query</td><td>请求参数必须包含指定参数</td><td>- Query=name, Jack 或者 - Query=name</td></tr><tr><td>RemoteAddr</td><td>请求者的 ip 必须是指定范围</td><td>- RemoteAddr=192.168.1.1/24</td></tr><tr><td>Weight</td><td>权重处理</td><td></td></tr></tbody></table>

示例：

![](<assets/1740016749160.png>)

**我们只需要掌握 Path 这种路由工程就可以了。**

### 3.4. 网关过滤器工厂

**网关过滤器 GatewayFilter** 是网关中提供的一种过滤器，**可以对进入网关的请求和微服务返回的响应做处理：**

![](<assets/1740016749473.png>)

#### 3.4.1. 路由过滤器的种类

Spring 提供了 31 种不同的路由过滤器工厂。例如：

<table><thead><tr><th><strong>名称</strong></th><th><strong>说明</strong></th></tr></thead><tbody><tr><td>AddRequestHeader</td><td>给当前请求添加一个请求头</td></tr><tr><td>RemoveRequestHeader</td><td>移除请求中的一个请求头</td></tr><tr><td>AddResponseHeader</td><td>给响应结果中添加一个响应头</td></tr><tr><td>RemoveResponseHeader</td><td>从响应结果中移除有一个响应头</td></tr><tr><td>RequestRateLimiter</td><td>限制请求的流量</td></tr></tbody></table>

#### 3.4.2. 请求头过滤器，AddRequestHeader

下面我们以 AddRequestHeader 为例来讲解。

**需求**：给所有进入 userservice 的请求添加一个请求头：Truth=itcast is freaking awesome!

**添加请求头过滤器（局部）：**

只需要修改 gateway 服务的 application.yml 文件，**给 user-service 添加路由过滤**即可：

```
spring:
 cloud:
 gateway:
 routes:
 - id: user-service 
 uri: lb://userservice 
 predicates: 
 - Path=/user/** 
 filters: # 过滤器
 - AddRequestHeader=Truth, Itcast is freaking awesome! # 添加请求头
```

当前过滤器写在 userservice 路由下，因此仅仅对访问 userservice 的请求有效。

参数注解 **@RequestHeader** 能**获取请求头内容**： 

![](<assets/1740016749719.png>)

#### 3.4.3. 默认过滤器，default-filters

如果要对所有的路由都生效，则可以将过滤器工厂写到 default 下。格式如下：

```
spring:
 cloud:
 gateway:
 routes:
 - id: user-service 
 uri: lb://userservice 
 predicates: 
 - Path=/user/**
 default-filters: # 默认过滤项
 - AddRequestHeader=Truth, Itcast is freaking awesome!
```

#### 3.4.4. 总结

**过滤器的作用是什么？**

*   ① **对路由的请求或响应做加工处理**，比如添加请求头
*   ② 配置在路由下的过滤器只对当前路由的请求生效

**defaultFilters 的作用是什么？**

*   ① 对所有路由都生效的过滤器

### 3.5. 全局过滤器，GlobalFilter

上一节学习的过滤器，网关提供了 31 种，但每一种过滤器的作用都是固定的。如果我们希望**拦截请求，做自己的业务逻辑**则没办法实现。

#### 3.5.1. 全局过滤器作用

全局过滤器的作用也是**处理一切进入网关的请求和响应**，**与 GatewayFilter 的作用一样**。

**区别在于 GatewayFilter 通过配置定义，处理逻辑是固定的；而 GlobalFilter 的逻辑需要自己写代码实现。**

定义方式是**实现 GlobalFilter 接口**。

```
public interface GlobalFilter {
    /**
     *  处理当前请求，有必要的话通过{@link GatewayFilterChain}将请求交给下一个过滤器处理
     *
     * @param exchange 请求上下文，里面可以获取Request、Response等信息
     * @param chain 用来把请求委托给下一个过滤器 
     * @return {@code Mono<Void>} 返回标示当前过滤器业务结束
     */
    Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain);
}
```

**在 filter 中编写自定义逻辑，可以实现下列功能：**

*   **登录状态判断**
*   **权限校验**
*   **请求限流等**

#### 3.5.2. 自定义全局过滤器，过滤未登录

**需求：**定义全局过滤器，拦截请求，判断请求的参数是否满足下面条件：

*   **参数中是否有授权 authorization**，
*   authorization 参数**值是否为 admin**

如果同时满足则放行，否则拦截

实际开发中一般是从 session 或缓存等方式中获取登录信息，这里仅做学习全局过滤器。

**实现：**

在 gateway 中定义一个过滤器：

```
package cn.itcast.gateway.filters;
//注解@Order设置过滤优先级，越小优先级越高。也可以实现Ordered接口的getOrder()方法
@Order(-1)
@Component
public class AuthorizeFilter implements GlobalFilter {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        // 1.获取请求参数。 MultiValueMap的add方法是一个key对应多个值，set方法是一个key对应一个值
        MultiValueMap<String, String> params = exchange.getRequest().getQueryParams();
        // 2.获取authorization参数
        String auth = params.getFirst("authorization");
        // 3.校验
        if ("admin".equals(auth)) {
            // 放行
            return chain.filter(exchange);
        }
        // 4.拦截
        // 4.1.禁止访问，设置状态码，401未登录unauthorized，
        //exchange.getResponse().setRawStatusCode(401);
        exchange.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED);
        // 4.2.结束处理
        return exchange.getResponse().setComplete();
    }
}
```

**回顾过滤器 Filter：**

```
@WebFilter(filterName = "loginCheckFilter",urlPatterns = "/*")
@Slf4j
public class LoginCheckFilter implements Filter {
    public static final AntPathMatcher PATH_MATCHER = new AntPathMatcher();
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;
        String[] notFilter = new String[]{
                "/employee/login",
                "/employee/logout",
                "/backend/**",
                "/front/**",
                "/common/**",
                "/user/sendMsg",
                "/user/login"
        };
        String nowUri = request.getRequestURI();
//        放行不需要拦截的uri
        for(String url:notFilter){
            if(PATH_MATCHER.match(url, nowUri)){
                filterChain.doFilter(request,response);
                return;
            }
        }
//        session里有employee则已登录，放行。针对于后端
        if(request.getSession().getAttribute("employee")!=null){
            BaseContext.setThreadId((Long) request.getSession().getAttribute("employee"));
            filterChain.doFilter(request,response);
 
            return;
        }
        //session里有user则已登录，放行。针对于前端
        if(request.getSession().getAttribute("user")!=null){
            BaseContext.setThreadId((Long) request.getSession().getAttribute("user"));
            filterChain.doFilter(request,response);
            return;
        }
//        未登录返回错误信息，前端设置跳转
        response.getWriter().write(JSON.toJSONString(R.error("NOTLOGIN")));
    }
}
```

#### 3.5.3. 过滤器执行顺序

请求进入网关会碰到三类过滤器：**当前路由的过滤器、DefaultFilter、GlobalFilter**

请求路由后，会将当前路由过滤器和 DefaultFilter、GlobalFilter，合并到一个过滤器链（集合）中，排序后依次执行每个过滤器：

![](<assets/1740016749993.png>)

**排序的规则**是什么呢？

*   每一个过滤器都必须指定一个 int 类型的 order 值，**order 值越小，优先级越高，执行顺序越靠前**。**order 值一样时，defaultFilter > 路由过滤器 > GlobalFilte**r。
*   **GlobalFilter** 通过实现 Ordered 接口，或者添加 **@Order** 注解来指定 order 值，由我们自己指定
*   **路由过滤器和 defaultFilter** 的 order 由 Spring 指定，默认是**按照声明顺序从 1 递增**。
*   当过滤器的 **order 值一样时**，会按照 **defaultFilter > 路由过滤器 > GlobalFilte**r 的顺序执行。

详细内容，可以查看源码：

`org.springframework.cloud.gateway.route.RouteDefinitionRouteLocator#getFilters()`方法是先加载 defaultFilters，然后再加载某个 route 的 filters，然后合并。

`org.springframework.cloud.gateway.handler.FilteringWebHandler#handle()`方法会加载全局过滤器，与前面的过滤器合并后根据 order 排序，组织过滤器链

### 3.6. 跨域问题

#### 3.6.1. 什么是跨域问题

**跨域：**域名不一致就是跨域，主要包括：

*   **域名不同**： [www.taobao.com](http://www.taobao.com "www.taobao.com") 和 [www.taobao.org](http://www.taobao.org "www.taobao.org") 和 [www.jd.com](http://www.jd.com "www.jd.com") 和 miaosha.jd.com
*   域名相同，**端口不同**：localhost:8080 和 localhost8081

**跨域问题：浏览器禁止请求的发起者与服务端发生跨域 ajax 请求，请求被浏览器拦截的问题**

**解决方案：CORS**，[跨域资源共享 CORS 详解 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2016/04/cors.html "跨域资源共享 CORS 详解 - 阮一峰的网络日志")

CORS，全称 Cross-Origin Resource Sharing，即**跨域资源共享** ，是一种**允许当前域（domain）的资源（比如 html/js/web service）被其他域（domain）的脚本请求访问的机**制，通常由于同域安全策略（the same-origin security policy）浏览器会禁止这种跨域请求。

#### 3.6.2. 模拟跨域问题

页面文件：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
<pre>
spring:
  cloud:
    gateway:
      globalcors: # 全局的跨域处理
        add-to-simple-url-handler-mapping: true # 解决options请求被拦截问题
        corsConfigurations:
          '[/**]':
            allowedOrigins: # 允许哪些网站的跨域请求
              - "http://localhost:8090"
              - "http://www.leyou.com"
            allowedMethods: # 允许的跨域ajax的请求方式
              - "GET"
              - "POST"
              - "DELETE"
              - "PUT"
              - "OPTIONS"
            allowedHeaders: "*" # 允许在请求中携带的头信息
            allowCredentials: true # 是否允许携带cookie
            maxAge: 360000 # 这次跨域检测的有效期
</pre>
</body>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script> axios.get("http://localhost:10010/user/1?authorization=admin")
  .then(resp => console.log(resp.data))
  .catch(err => console.log(err)) </script>
</html>
```

放入 gateway 的 tomcat 或者 nginx 这样的 web 服务器中，启动并访问。

可以在浏览器控制台看到下面的错误：

![](<assets/1740016750180.png>)

从 localhost:8090 访问 localhost:10010，端口不同，显然是跨域的请求。

#### 3.6.3. 解决跨域问题，CORS

在 gateway 服务的 application.yml 文件中，添加下面的配置：

```
spring:
 cloud:
 gateway:
      # 。。。
 globalcors: # 全局的跨域处理
 add-to-simple-url-handler-mapping: true # 解决options请求被拦截问题
 corsConfigurations:
          '[/**]':
 allowedOrigins: # 允许哪些网站的跨域请求 
 - "http://localhost:8090"
 allowedMethods: # 允许的跨域ajax的请求方式
 - "GET"
 - "POST"
 - "DELETE"
 - "PUT"
 - "OPTIONS"
 allowedHeaders: "*" # 允许在请求中携带的头信息
 allowCredentials: true # 是否允许携带cookie
 maxAge: 360000 # 这次跨域检测的有效期
```