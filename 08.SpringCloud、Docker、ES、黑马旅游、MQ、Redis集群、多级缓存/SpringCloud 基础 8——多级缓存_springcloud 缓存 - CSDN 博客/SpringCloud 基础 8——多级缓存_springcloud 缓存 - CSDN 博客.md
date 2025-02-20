---
url: https://blog.csdn.net/qq_40991313/article/details/126922987?spm=1001.2014.3001.5501
title: SpringCloud 基础 8——多级缓存_springcloud 缓存 - CSDN 博客
date: 2025-02-20 09:59:32
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1. 多级缓存流程](#t0)

[2.JVM 进程缓存](#t1)

[2.1.docker 安装 MySQL](#t2)

[2.1.1. 准备目录](#t3)

[2.1.2. 创建 mysql 容器](#t4)

[2.1.3. 修改配置](#t5)

[2.1.4. 重启](#t6)

[2.2. 导入 SQL](#t7)

[2.3. 导入 Demo 工程](#t8)

[2.3.0.pom 和 yml](#t9)

[2.3.1. 分页查询商品](#t10)

[2.3.2. 新增商品](#t11)

[2.3.3. 修改商品](#t12)

[2.3.4. 修改库存](#t13)

[2.3.5. 删除商品](#t14)

[2.3.6. 根据 id 查询商品](#t15)

[2.3.7. 根据 id 查询库存](#t16)

[2.3.8. 启动](#t17)

[2.4. 导入商品查询页面](#t18)

[2.4.1. 运行 nginx 服务](#t19)

[2.4.2. 反向代理](#t20)

[2.5.Caffeine 简介，本地缓存](#t21)

[2.6. 实现 Caffeine 的 JVM 进程缓存](#t22)

[2.6.1. 需求](#t23)

[2.6.2. 实现](#t24)

[3.Lua 语法入门](#t25)

[3.1. 初识 Lua](#t26)

[3.1.HelloWorld](#t27)

[3.2. 变量和循环](#t28)

[3.2.1.Lua 的数据类型](#t29)

[3.2.2. 声明变量](#t30)

[3.2.3. 循环](#t31)

[3.3. 条件控制、函数](#t32)

[3.3.1. 函数](#t33)

[3.3.2. 条件控制](#t34)

[3.3.3. 案例，自定义函数判断数组参数是否为 nil](#t35)

[4. 实现多级缓存](#t36)

[4.1. 安装 OpenResty](#t37)

[4.1.0.OpenResty 简介，基于 Linux 集成 Lua 库](#t38)

[4.1.1. 安装](#t39)

[4.1.2. 启动和运行](#t40)

[4.1.3. 备注](#t41)

[4.2.OpenResty 快速入门](#t42)

[4.2.1. 反向代理流程](#t43)

[4.2.2.OpenResty 监听请求、响应 lua 文件的 JSON](#t44)

[4.2.3.openresty 响应 item.lua 假数据](#t45)

[4.3.OpenResty 请求参数处理](#t46)

[4.3.1. 获取参数的 API](#t47)

[4.3.2.lua 获取参数中 id 并响应](#t48)

[4.4. 查询 Tomcat](#t49)

[4.4.1.lua 发送 http 请求的 API](#t50)

[4.4.2. 封装 http 工具](#t51)

[4.4.3.CJSON 工具类](#t52)

[4.4.4. 实现 Tomcat 查询](#t53)

[4.4.5. 基于 ID 负载均衡](#t54)

[4.5.Redis 缓存预热](#t55)

[4.6. 查询 Redis 缓存](#t56)

[4.6.1. 封装 Redis 工具](#t57)

[4.6.2. 实现 Redis 查询](#t58)

[4.7.Nginx 本地缓存](#t59)

[4.7.1. 本地缓存 API](#t60)

[4.7.2. 实现本地缓存查询](#t61)

[5. 缓存同步](#t62)

[5.1. 数据同步策略](#t63)

[5.2. 安装 Canal](#t64)

[5.2.1. 认识 Canal](#t65)

[5.2.2. 安装 Canal](#t66)

[5.3. 监听 Canal](#t67)

[5.3.1. 引入依赖：](#t68)

[5.3.2. 编写配置：](#t69)

[5.3.3. 修改 Item 实体类](#t70)

[5.3.4. 编写监听器](#t71)

## 1. 多级缓存流程

**传统的缓存策略**一般是请求到达 Tomcat 后，先查询 Redis，如果未命中则查询数据库，如图：

![](<assets/1740016784513.png>)

**存在下面的问题：**

**• 请求要经过 Tomcat 处理，Tomcat 的性能成为整个系统的瓶颈**

**•Redis 缓存失效时，会对数据库产生冲击**

**多级缓存**就是充分利用请求处理的每个环节，**分别添加缓存，减轻 Tomcat 压力**，提升服务性能：

*   浏览器访问静态资源时，优先读取**浏览器本地缓存**
*   访问非静态资源（ajax 查询数据）时，访问服务端
*   请求到达 Nginx 后，优先读取 **Nginx 本地缓存**
*   如果 Nginx 本地缓存未命中，则去直接**查询 Redis（不经过 Tomcat）**
*   如果 Redis 查询未命中，则**查询 Tomcat**
*   请求进入 Tomcat 后，优先查询 **JVM 本地进程缓存（如 Caffeine）**
*   如果 JVM 进程缓存未命中，则**查询数据库**

![](<assets/1740016784702.png>)

在多级缓存架构中，**Nginx 内部需要编写本地缓存查询、Redis 查询、Tomcat 查询的业务逻辑**，因此这样的 **nginx** 服务不再是一个反向代理服务器，而是一个**编写业务的 Web 服务器**了。

因此这样的**业务 Nginx 服务**也需要**搭建集群**来提高并发，再有**专门的 nginx 服务来做反向代理**，如图：

![](<assets/1740016784926.png>)

另外，我们的 Tomcat 服务将来也会部署为集群模式：

![](<assets/1740016785123.png>)

可见，**多级缓存的关键有两个：**

*   一个是在 **nginx 中编写业务**，实现 nginx 本地缓存、Redis、Tomcat 的查询
    
*   另一个就是在 **Tomcat 中实现 JVM 进程缓存**
    

其中 Nginx 编程则会用到 OpenResty 框架结合 Lua 这样的语言。

这也是今天课程的难点和重点。

## 2.JVM 进程缓存

为了演示多级缓存的案例，我们先准备一个商品查询的业务。

为了演示多级缓存，我们**先导入一个商品管理的案例，其中包含商品的 CRUD 功能**。我们将来会给查询商品添加多级缓存。

### 2.1.docker 安装 MySQL

后期做数据同步需要用到 MySQL 的主从功能，所以需要大家在虚拟机中，利用 Docker 来运行一个 MySQL 容器。

#### 2.1.1. 准备目录

为了方便后期配置 MySQL，我们先准备两个目录，用于挂载容器的数据和配置文件目录：

```
## 2.进入/tmp目录
cd /tmp
## 2.创建文件夹
mkdir mysql
## 2.进入mysql目录
cd mysql
```

#### 2.1.2. 创建 mysql 容器

进入 mysql 目录后，执行下面的 Docker 命令，**注意改端口和数据库账号密码**： 

```
docker run \
 -p 3306:3306 \
 --name mysql \
 -v $PWD/conf:/etc/mysql/conf.d \
 -v $PWD/logs:/logs \
 -v $PWD/data:/var/lib/mysql \
 -e MYSQL_ROOT_PASSWORD=123 \
 --privileged \
 -d \
 mysql:5.7.25
```

#### 2.1.3. 修改配置

在 / tmp/mysql/conf 目录添加一个 my.cnf 文件，作为 mysql 的配置文件：

```
## 2.创建文件
vi /tmp/mysql/conf/my.cnf
```

文件的内容如下：

```
[mysqld]
#跳过域名解析
skip-name-resolve    
character_set_server=utf8
#跟容器指定的数据目录名保持一致
datadir=/var/lib/mysql
#服务id
server-id=1000
```

#### 2.1.4. 重启

配置修改后，必须重启容器：

```
/*
 Navicat Premium Data Transfer

 Source Server         : 192.168.150.101
 Source Server Type    : MySQL
 Source Server Version : 50725
 Source Host           : 192.168.150.101:3306
 Source Schema         : heima

 Target Server Type    : MySQL
 Target Server Version : 50725
 File Encoding         : 65001

 Date: 16/08/2021 14:45:07
*/
 
SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;
 
-- ----------------------------
-- Table structure for tb_item
-- ----------------------------
DROP TABLE IF EXISTS `tb_item`;
CREATE TABLE `tb_item`  (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '商品id',
  `title` varchar(264) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '商品标题',
  `name` varchar(128) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL DEFAULT '' COMMENT '商品名称',
  `price` bigint(20) NOT NULL COMMENT '价格（分）',
  `image` varchar(200) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '商品图片',
  `category` varchar(200) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '类目名称',
  `brand` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '品牌名称',
  `spec` varchar(200) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '规格',
  `status` int(1) NULL DEFAULT 1 COMMENT '商品状态 1-正常，2-下架，3-删除',
  `create_time` datetime NULL DEFAULT NULL COMMENT '创建时间',
  `update_time` datetime NULL DEFAULT NULL COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `status`(`status`) USING BTREE,
  INDEX `updated`(`update_time`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 50002 CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '商品表' ROW_FORMAT = COMPACT;
 
-- ----------------------------
-- Records of tb_item
-- ----------------------------
INSERT INTO `tb_item` VALUES (10001, 'RIMOWA 21寸托运箱拉杆箱 SALSA AIR系列果绿色 820.70.36.4', 'SALSA AIR', 16900, 'https://m.360buyimg.com/mobilecms/s720x720_jfs/t6934/364/1195375010/84676/e9f2c55f/597ece38N0ddcbc77.jpg!q70.jpg.webp', '拉杆箱', 'RIMOWA', '{\"颜色\": \"红色\", \"尺码\": \"26寸\"}', 1, '2019-05-01 00:00:00', '2019-05-01 00:00:00');
INSERT INTO `tb_item` VALUES (10002, '安佳脱脂牛奶 新西兰进口轻欣脱脂250ml*24整箱装*2', '脱脂牛奶', 68600, 'https://m.360buyimg.com/mobilecms/s720x720_jfs/t25552/261/1180671662/383855/33da8faa/5b8cf792Neda8550c.jpg!q70.jpg.webp', '牛奶', '安佳', '{\"数量\": 24}', 1, '2019-05-01 00:00:00', '2019-05-01 00:00:00');
INSERT INTO `tb_item` VALUES (10003, '唐狮新品牛仔裤女学生韩版宽松裤子 A款/中牛仔蓝（无绒款） 26', '韩版牛仔裤', 84600, 'https://m.360buyimg.com/mobilecms/s720x720_jfs/t26989/116/124520860/644643/173643ea/5b860864N6bfd95db.jpg!q70.jpg.webp', '牛仔裤', '唐狮', '{\"颜色\": \"蓝色\", \"尺码\": \"26\"}', 1, '2019-05-01 00:00:00', '2019-05-01 00:00:00');
INSERT INTO `tb_item` VALUES (10004, '森马(senma)休闲鞋女2019春季新款韩版系带板鞋学生百搭平底女鞋 黄色 36', '休闲板鞋', 10400, 'https://m.360buyimg.com/mobilecms/s720x720_jfs/t1/29976/8/2947/65074/5c22dad6Ef54f0505/0b5fe8c5d9bf6c47.jpg!q70.jpg.webp', '休闲鞋', '森马', '{\"颜色\": \"白色\", \"尺码\": \"36\"}', 1, '2019-05-01 00:00:00', '2019-05-01 00:00:00');
INSERT INTO `tb_item` VALUES (10005, '花王（Merries）拉拉裤 M58片 中号尿不湿（6-11kg）（日本原装进口）', '拉拉裤', 38900, 'https://m.360buyimg.com/mobilecms/s720x720_jfs/t24370/119/1282321183/267273/b4be9a80/5b595759N7d92f931.jpg!q70.jpg.webp', '拉拉裤', '花王', '{\"型号\": \"XL\"}', 1, '2019-05-01 00:00:00', '2019-05-01 00:00:00');
 
-- ----------------------------
-- Table structure for tb_item_stock
-- ----------------------------
DROP TABLE IF EXISTS `tb_item_stock`;
CREATE TABLE `tb_item_stock`  (
  `item_id` bigint(20) NOT NULL COMMENT '商品id，关联tb_item表',
  `stock` int(10) NOT NULL DEFAULT 9999 COMMENT '商品库存',
  `sold` int(10) NOT NULL DEFAULT 0 COMMENT '商品销量',
  PRIMARY KEY (`item_id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci ROW_FORMAT = COMPACT;
 
-- ----------------------------
-- Records of tb_item_stock
-- ----------------------------
INSERT INTO `tb_item_stock` VALUES (10001, 99996, 3219);
INSERT INTO `tb_item_stock` VALUES (10002, 99999, 54981);
INSERT INTO `tb_item_stock` VALUES (10003, 99999, 189);
INSERT INTO `tb_item_stock` VALUES (10004, 99999, 974);
INSERT INTO `tb_item_stock` VALUES (10005, 99999, 18649);
 
SET FOREIGN_KEY_CHECKS = 1;
```

### 2.2. 导入 SQL

先创建 heima 数据库，导入 sql： 

```
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <!--mybatis-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.2</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>com.github.ben-manes.caffeine</groupId>
            <artifactId>caffeine</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>
```

其中包含两张表： 

*   **tb_item：商品表**，包含商品的基本信息

![](<assets/1740016785328.png>)

*   **tb_item_stock：商品库存表**，包含商品的库存信息

![](<assets/1740016785551.png>)

之所以**将库存分离出来**，是因为**库存是更新比较频繁**的信息，**写操作较多**。而**其他信息修改的频率非常低。频繁更新数据清除缓存就没有缓存意义了。**

### 2.3. 导入 Demo 工程

下面导入课前资料提供的工程：

![](<assets/1740016785786.png>)

项目结构如图所示：

![](<assets/1740016785972.png>)

其中的业务包括：

*   分页查询商品
*   新增商品
*   修改商品
*   修改库存
*   删除商品
*   根据 id 查询商品
*   根据 id 查询库存

业务全部使用 mybatis-plus 来实现，如有需要请自行修改业务逻辑。

#### 2.3.0.pom 和 yml

```
server:
 port: 8081
spring:
 application:
 name: itemservice
 datasource:
 url: jdbc:mysql://自己的ip地址:3306/heima?useSSL=false
 username: root
 password: 自己的密码
 driver-class-name: com.mysql.jdbc.Driver
mybatis-plus:
 type-aliases-package: com.heima.item.pojo
 configuration:
 map-underscore-to-camel-case: true
 global-config:
 db-config:
#更新策略只更新非null，默认值
 update-strategy: not_null
#id策略自增
 id-type: auto
logging:
 level:
    com.heima: debug
 pattern:
 dateformat: HH:mm:ss:SSS
```

```
#user  nobody;
worker_processes  1;
 
events {
    worker_connections  1024;
}
 
http {
    include       mime.types;
    default_type  application/octet-stream;
 
    sendfile        on;
    #tcp_nopush     on;
    keepalive_timeout  65;
 
#upstream指令可以定义一组服务器。默认是轮询算法，即几个服务器轮流工作
    upstream nginx-cluster{
#这个地址是虚拟机OpenResty的地址端口、OpenResty会在下面第四小节配置
        server 192.168.150.101:8081;
    }
    server {
        listen       80;
        server_name  localhost;
 
#反向代理并负载均衡配置，将请求转发到指定网址的服务器，这里写了upstream 定义的nginx-cluster集群，将在集群里的服务器之间轮询负载均衡
	location /api {
            proxy_pass http://nginx-cluster;
        }
 
        location / {
            root   html;
            index  index.html index.htm;
        }
 
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

#### 2.3.1. 分页查询商品

在`com.heima.item.web`包的`ItemController`中可以看到接口定义：

![](<assets/1740016786232.png>)

#### 2.3.2. 新增商品

在`com.heima.item.web`包的`ItemController`中可以看到接口定义：

![](<assets/1740016786414.png>)

#### 2.3.3. 修改商品

在`com.heima.item.web`包的`ItemController`中可以看到接口定义：

![](<assets/1740016786654.png>)

#### 2.3.4. 修改库存

在`com.heima.item.web`包的`ItemController`中可以看到接口定义：

![](<assets/1740016786886.png>)

#### 2.3.5. 删除商品

在`com.heima.item.web`包的`ItemController`中可以看到接口定义：

![](<assets/1740016787065.png>)

这里是采用了逻辑删除，将商品状态修改为 3

#### 2.3.6. 根据 id 查询商品

在`com.heima.item.web`包的`ItemController`中可以看到接口定义：

![](<assets/1740016787320.png>)

这里只返回了商品信息，不包含库存

#### 2.3.7. 根据 id 查询库存

在`com.heima.item.web`包的`ItemController`中可以看到接口定义：

![](<assets/1740016787586.png>)

#### 2.3.8. 启动

注意修改 application.yml 文件中配置的 mysql 地址信息：

![](<assets/1740016787995.png>)

需要修改为自己的虚拟机地址信息、还有账号和密码。

修改后，启动服务，访问：**http://localhost:8081/item/10001** 即可查询数据

### 2.4. 导入商品查询页面

商品查询是购物页面，与商品管理的页面是分离的。

部署方式如图：

![](<assets/1740016788329.png>)

我们需要准备一个反向代理的 nginx 服务器，如上图红框所示，将静态的商品页面放到 nginx 目录中。

页面需要的数据通过 ajax 向服务端（nginx 业务集群）查询。

#### 2.4.1. 运行 nginx 服务

这里我已经给大家准备好了 nginx 反向代理服务器和静态资源。

我们找到课前资料的 nginx 目录：

![](<assets/1740016788691.png>)

将其拷贝到一个**非中文或空格目录下**，运行这个 nginx 服务。

运行命令：

```
@Test
void testBasicOps() {
    // 构建cache对象
    Cache<String, String> cache = Caffeine.newBuilder().build();
 
    // 存数据
    cache.put("gf", "迪丽热巴");
 
    // 取数据
    String gf = cache.getIfPresent("gf");
    System.out.println("gf = " + gf);
 
    // 取数据，包含两个参数：
    // 参数一：缓存的key
    // 参数二：Lambda表达式，表达式参数就是缓存的key，方法体是查询数据库的逻辑
    // 优先根据key查询JVM缓存，如果未命中，则执行参数二的Lambda表达式
    String defaultGF = cache.get("defaultGF", key -> {    //这里key就是"defaultGF"，只是起别名
        // 根据key去数据库查询数据，这里省略数据库查询操作，直接返回
        return "柳岩";
    });
    System.out.println("defaultGF = " + defaultGF);
}
```

然后访问 **http://localhost/item.html?id=10001** 即可：

![](<assets/1740016788920.png>)

**管理端页面：**[http://localhost:8081/](http://localhost:8081/ "http://localhost:8081/") 

![](<assets/1740016789141.png>)

#### 2.4.2. 反向代理

**现在，页面是假数据展示的。**我们需要向服务器**发送 ajax 请求**，查询商品数据。

打开控制台，可以看到页面有发起 ajax 查询数据：

![](<assets/1740016789426.png>)

而这个**请求地址**同样是 **80 端口**，**而后台端口是 8081 端口**，所以**请求被当前的 nginx 反向代理了**。

查看 nginx 的 conf 目录下的 nginx.conf 文件的**反向代理配置：**

![](<assets/1740016789655.png>)

其中的关键配置如下：

![](<assets/1740016789902.png>)

其中的 192.168.150.101 是我的虚拟机 IP，也就是我的 Nginx 业务集群要部署的地方：

![](<assets/1740016790323.png>)

完整内容如下：

```
// 创建缓存对象
Cache<String, String> cache = Caffeine.newBuilder()
    .maximumSize(1) // 设置缓存数量上限为 1，只能存一个key，存第二个后，caffeine会找空闲时间或一次读写后清理
    .build();
```

### 2.5.Caffeine 简介，本地缓存

缓存在日常开发中启动至关重要的作用，由于是存储在内存中，数据的读取速度是非常快的，能大量减少对数据库的访问，减少数据库的压力。我们把缓存分为两类：

*   **分布式缓存**，例如 Redis：
    *   **优点：**存储容量更大、可靠性更好、可以在集群间共享
    *   **缺点：**访问缓存有**网络开销**
    *   **场景：缓存数据量较大、可靠性要求较高、需要在集群间共享**
*   **进程本地缓存**，例如 HashMap、GuavaCache：
    *   优点：读取本地内存，没有网络开销，**速度更快**
    *   缺点：存储容量有限、可靠性较低、无法共享
    *   **场景：性能要求较高，缓存数据量较小**

我们今天会利用 Caffeine 框架来实现 JVM 进程缓存。

**Caffeine** 是一个基于 Java8 开发的，提供了近乎**最佳命中率**的高性能的**本地缓存库。**目前 Spring 内部的缓存使用的就是 Caffeine。GitHub 地址：https://github.com/ben-manes/caffeine

Caffeine 的性能非常好，下图是官方给出的性能对比：

![](<assets/1740016790617.png>)

可以看到 Caffeine 的性能遥遥领先！

缓存使用的基本 API：

```
// 创建缓存对象
Cache<String, String> cache = Caffeine.newBuilder()
    // 设置缓存有效期为 10 秒，从最后一次写入开始计时 
    .expireAfterWrite(Duration.ofSeconds(10)) 
    .build();
```

**注意导包**，不是 spring 的包，是 GitHub 的包：

![](<assets/1740016790952.png>)

Caffeine 既然是缓存的一种，肯定需要有缓存的清除策略，不然的话内存总会有耗尽的时候。

Caffeine 提供了**三种缓存驱逐策略：**

*   **基于容量**：设置缓存的**数量上限**
    
    ```
    package com.heima.item.config;
     
    import com.github.benmanes.caffeine.cache.Cache;
    import com.github.benmanes.caffeine.cache.Caffeine;
    import com.heima.item.pojo.Item;
    import com.heima.item.pojo.ItemStock;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
     
    @Configuration
    public class CaffeineConfig {
     
        @Bean
        public Cache<Long, Item> itemCache(){
            return Caffeine.newBuilder()
                    .initialCapacity(100)    //初始的缓存空间大小
                    .maximumSize(10_000)
                    .build();
        }
     
        @Bean
        public Cache<Long, ItemStock> stockCache(){
            return Caffeine.newBuilder()
                    .initialCapacity(100)
                    .maximumSize(10_000)
                    .build();
        }
    }
    ```
    
*   **基于时间**：设置缓存的有效时间
    
    ```
    @RestController
    @RequestMapping("item")
    public class ItemController {
     
        @Autowired
        private IItemService itemService;
        @Autowired
        private IItemStockService stockService;
     
        @Autowired
        private Cache<Long, Item> itemCache;
        @Autowired
        private Cache<Long, ItemStock> stockCache;
        
        // ...其它略
        
        @GetMapping("/{id}")
        public Item findById(@PathVariable("id") Long id) {
            return itemCache.get(id, key -> itemService.query()
                    .ne("status", 3).eq("id", key)
                    .one()
            );
        }
     
        @GetMapping("/stock/{id}")
        public ItemStock findStockById(@PathVariable("id") Long id) {
            return stockCache.get(id, key -> stockService.getById(key));
        }
    }
    ```
    
*   **基于引用**：设置缓存为软引用或弱引用，利用 GC 来回收缓存数据。性能较差，不建议使用。
    

**注意**：在默认情况下，当**一个缓存元素过期的时候**，**Caffeine 不会自动立即将其清理**和驱逐。而是**在一次读或写操作后**，或者**在空闲时间完**成对失效数据的驱逐。

### 2.6. 实现 **Caffeine 的** JVM 进程缓存

#### 2.6.1. 需求

利用 Caffeine 实现下列需求：

*   给根据 id 查询商品的业务添加缓存，缓存未命中时查询数据库
*   给根据 id 查询商品库存的业务添加缓存，缓存未命中时查询数据库
*   **缓存初始大小为 100**
*   缓存**上限为 10000 个**

#### 2.6.2. 实现

首先，我们需要定义两个 Caffeine 的缓存对象，**分别保存商品、库存的缓存数据**。

在 item-service 的`com.heima.item.**config**`**包**下定义`CaffeineConfig`类：

```
-- 声明字符串，可以用单引号或双引号，
local str = 'hello'
-- 字符串拼接可以使用 ..
local str2 = 'hello' .. 'world'
-- 声明数字
local num = 21
-- 声明布尔类型
local flag = true
```

然后，修改 item-service 中的`com.heima.item.web`包下的 ItemController 类，添加缓存逻辑：

```
-- 声明数组 ，key为角标的 table
local arr = {'java', 'python', 'lua'}
-- 声明table，类似java的map
local map =  {name='Jack', age=21}
```

## 3.Lua 语法入门

Nginx 编程需要用到 Lua 语言，因此我们必须先入门 Lua 的基本语法。

### 3.1. 初识 Lua

Lua 是一种轻量小巧的**脚本语言**，用标准 C 语言编写并以源代码形式开放， 其**设计目的是为了嵌入应用程序中**，从而为应用程序提供灵活的扩展和定制功能。官网：https://www.lua.org/

![](<assets/1740016791161.png>)

Lua 经常嵌入到 C 语言开发的程序中，例如游戏开发、游戏插件等。

Nginx 本身也是 C 语言开发，因此也允许基于 Lua 做拓展。

### 3.1.HelloWorld

**CentOS7 默认已经安装了 Lua 语言环境**，所以可以直接运行 Lua 代码。

1）在 Linux 虚拟机的任意目录下，新建一个 hello.lua 文件

![](<assets/1740016791445.png>)

2）添加下面的内容

```
-- 访问数组，lua数组的角标从1开始
print(arr[1])
```

3）运行

![](<assets/1740016791648.png>)

另外，还可以直接输入 lua，**命令行模式：**

![](<assets/1740016791900.png>)

### 3.2. 变量和循环

学习任何语言必然离不开变量，而变量的声明必须先知道数据的类型。

#### 3.2.1.Lua 的数据类型

Lua 中支持的常见数据类型包括：

![](<assets/1740016792212.png>)

另外，Lua 提供了 **type()** 函数来**判断一个变量的数据类型：**

![](<assets/1740016792546.png>)

#### 3.2.2. 声明变量

**字符串单引号双引号都可以，每个语句结束回车或空格即可，不用加 “；”**

Lua 声明变量的时候**无需指定数据类型**，而是用 **local 来声明**变量为**局部变量：**

```
-- 访问table
print(map['name'])
print(map.name)
```

Lua 中的 **table 类型既可以作为数组**，**又可以**作为 Java 中的 **map** 来使用。数组就是特殊的 table，key 是数组角标而已：

```
-- 声明数组 key为索引的 table
local arr = {'java', 'python', 'lua'}
-- 遍历数组
for index,value in ipairs(arr) do
    print(index, value) 
end
```

Lua 中的**数组角标是从 1 开始**，访问的时候与 Java 中类似：

```
-- 声明map，也就是table
local map = {name='Jack', age=21}
-- 遍历table
for key,value in pairs(map) do
   print(key, value) 
end
```

Lua 中的 table 可以用 **key 来访问：**

```
function 函数名( argument1, argument2..., argumentn)
    -- 函数体
    return 返回值
end
```

#### 3.2.3. 循环

对于 table，我们可以利用 for 循环来遍历。不过数组和普通 table 遍历略有差异。

**遍历数组：**

```
function printArr(arr)
    for index, value in ipairs(arr) do
        print(value)
    end
end
```

**注意 ipairs** 

**遍历普通 table（map）**

```
if(布尔表达式)
then
   --[ 布尔表达式为 true 时执行该语句块 --]
else
   --[ 布尔表达式为 false 时执行该语句块 --]
end
```

**注意 pairs** 

### 3.3. 条件控制、函数

Lua 中的条件控制和函数声明与 Java 类似。

#### 3.3.1. 函数

定义函数的语法：

```
function printArr(arr)
    if not arr then
        print('数组不能为空！')
    end
    for index, value in ipairs(arr) do
        print(value)
    end
end
```

例如，定义一个函数，用来打印数组：

```
export NGINX_HOME=/usr/local/openresty/nginx
export PATH=${NGINX_HOME}/sbin:$PATH
```

#### 3.3.2. 条件控制

类似 Java 的条件控制，例如 if、else 语法：

```
### 启动nginx
nginx
### 重新加载配置
nginx -s reload
### 停止
nginx -s stop
```

与 java 不同，布尔表达式中的逻辑运算是基于英文单词：

![](<assets/1740016792766.png>)

#### 3.3.3. 案例，自定义函数判断数组参数是否为 nil

需求：自定义一个函数，可以打印 table，当参数为 nil 时，打印错误信息

```
#user  nobody;
worker_processes  1;
error_log  logs/error.log;
 
events {
    worker_connections  1024;
}
 
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
 
    server {
        listen       8081;
        server_name  localhost;
        location / {
            root   html;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

## 4. 实现多级缓存

多级缓存的实现离不开 Nginx 编程，而 **Nginx 编程又离不开 OpenResty。**

### 4.1. 安装 OpenResty

#### 4.1.0.OpenResty 简介，基于 Linux 集成 Lua 库

OpenResty® 是一个基于 Nginx 的**高性能 Web 平台**，用于方便地搭建能够处理超高并发、扩展性极高的动态 Web 应用、Web 服务和动态网关。具备下列特点：

*   **基于 Linux**，具备 Nginx 的完整功能
*   **基于 Lua 语言进行扩展**，集成了大量精良的 **Lua 库**、第三方模块
*   允许**使用 Lua 自定义业务逻辑**、**自定义库**

官方网站：

```
#lua 模块
lua_package_path "/usr/local/openresty/lualib/?.lua;;";
#c模块 
lua_package_cpath "/usr/local/openresty/lualib/?.so;;";
```

![](<assets/1740016793016.png>)

#### 4.1.1. 安装

首先你的 Linux 虚拟机必须联网

**1）安装依赖开发库**

首先要安装 OpenResty 的依赖开发库，执行命令：

```
-- 封装函数，发送http请求，并解析响应
local function read_http(path, params)
    local resp = ngx.location.capture(path,{
        method = ngx.HTTP_GET,
        args = params,
    })
    if not resp then
        -- 记录错误信息，返回404
        ngx.log(ngx.ERR, "http not found, path: ", path , ", args: ", args)
        ngx.exit(404)
    end
    return resp.body
end
-- 将方法导出
local _M = {  
    read_http = read_http
}  
return _M
```

**2）安装 OpenResty 仓库**

你可以在你的 CentOS 系统中添加 `openresty` 仓库，这样就可以便于未来安装或更新我们的软件包（通过 `yum check-update` 命令）。运行下面的命令就可以添加我们的仓库：

```
-- 关闭redis连接的工具方法，其实是放入连接池
local function close_redis(red)
    local pool_max_idle_time = 10000 -- 连接的空闲时间，单位是毫秒
    local pool_size = 100 --连接池大小
    local ok, err = red:set_keepalive(pool_max_idle_time, pool_size)
    if not ok then
        ngx.log(ngx.ERR, "放入redis连接池失败: ", err)
    end
end
```

如果提示说命令不存在，则运行：

```
-- 查询redis的方法 ip和port是redis地址，key是查询的key
local function read_redis(ip, port, key)
    -- 获取一个连接
    local ok, err = red:connect(ip, port)
    if not ok then
        ngx.log(ngx.ERR, "连接redis失败 : ", err)
        return nil
    end
    -- 查询redis
    local resp, err = red:get(key)
    -- 查询失败处理
    if not resp then
        ngx.log(ngx.ERR, "查询Redis失败: ", err, ", key = " , key)
    end
    --得到的数据为空处理
    if resp == ngx.null then
        resp = nil
        ngx.log(ngx.ERR, "查询Redis数据为空, key = ", key)
    end
    close_redis(red)
    return resp
end
```

然后再重复上面的命令

**3）安装 OpenResty**

然后就可以像下面这样安装软件包，比如 `openresty`：

```
### 共享字典，也就是本地缓存，名称叫做：item_cache，大小150m
lua_shared_dict item_cache 150m; 
```

**4）安装 opm 工具**

opm 是 OpenResty 的一个管理工具，可以帮助我们安装一个第三方的 Lua 模块。

如果你想安装命令行工具 `opm`，那么可以像下面这样安装 `openresty-opm` 包：

```
#lua 模块
lua_package_path "/usr/local/openresty/lualib/?.lua;;";
#c模块 
lua_package_cpath "/usr/local/openresty/lualib/?.so;;";
```

**5）目录结构**

**默认**情况下，**OpenResty 安装的目录是：/usr/local/openresty**

![](<assets/1740016793329.png>)

看到里面的 nginx 目录了吗，OpenResty 就是在 Nginx 基础上集成了一些 Lua 模块。

**6）配置 nginx 的环境变量**

打开配置文件：

```
location  /api/item {
    # 默认的响应类型
    default_type application/json;
    # 响应结果由lua/item.lua文件来决定
    content_by_lua_file lua/item.lua;
}
```

在最下面加入两行：

```
#user  nobody;
worker_processes  1;
error_log  logs/error.log;
 
events {
    worker_connections  1024;
}
 
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
	 #lua 模块
	lua_package_path "/usr/local/openresty/lualib/?.lua;;";
	#c模块 
	lua_package_cpath "/usr/local/openresty/lualib/?.so;;";  
    server {
        listen       8081;
        server_name  localhost;
        location / {
            root   html;
            index  index.html index.htm;
        }
        location  /api/item {
   		 # 默认的响应类型
	    default_type application/json;
	    # 响应结果由lua/item.lua文件来决定
	    content_by_lua_file lua/item.lua;
		}
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

NGINX_HOME：后面是 OpenResty 安装目录下的 nginx 的目录

然后让配置生效：

```
cd /usr/local/openresty/nginx
mkdir lua
```

#### 4.1.2. 启动和运行

OpenResty 底层是基于 Nginx 的，查看 OpenResty 目录的 nginx 目录，结构与 windows 中安装的 nginx 基本一致：

![](<assets/1740016793574.png>)

所以运行方式与 nginx 基本一致：

```
cd lua
touch item.lua
```

nginx 的默认配置文件注释太多，影响后续我们的编辑，这里将 nginx.conf 中的注释部分删除，保留有效部分。

修改`/usr/local/openresty/nginx/conf/**nginx.conf**`**文件**，**主要监听端口号从 80 改成 8081**：

```
location ~ /api/item/(\d+) {
    # 默认的响应类型
    default_type application/json;
    # 响应结果由lua/item.lua文件来决定
    content_by_lua_file lua/item.lua;
}
```

在 Linux 的控制台输入命令以启动 nginx：

```
-- 获取商品id
local id = ngx.var[1]
-- 拼接并返回
ngx.say('{"id":' .. id .. ',"name":"SALSA AIR","title":"RIMOWA 21寸托运箱拉杆箱 SALSA AIR系列果绿色 820.70.36.4","price":17900,"image":"https://m.360buyimg.com/mobilecms/s720x720_jfs/t6934/364/1195375010/84676/e9f2c55f/597ece38N0ddcbc77.jpg!q70.jpg.webp","category":"拉杆箱","brand":"RIMOWA","spec":"","status":1,"createTime":"2019-04-30T16:00:00.000+00:00","updateTime":"2019-04-30T16:00:00.000+00:00","stock":2999,"sold":31290}')
```

然后访问页面：http:// 虚拟机 ip 地址: 8081，注意 ip 地址替换为你自己的虚拟机 IP：

#### 4.1.3. 备注

加载 OpenResty 的 lua 模块：

```
local resp = ngx.location.capture("/path",{
    method = ngx.HTTP_GET,   -- 请求方式
    args = {a=1,b=2},  -- get方式传参数
})
```

common.lua

```
location /path {
     # 这里是windows电脑的ip和Java服务端口，需要确保windows防火墙处于关闭状态
     proxy_pass http://192.168.150.1:8081; 
 }
```

释放 Redis 连接 API：

```
location /item {
    proxy_pass http://192.168.150.1:8081;
}
```

读取 Redis 数据的 API：

```
-- 封装函数，发送http请求，并解析响应
local function read_http(path, params)
    local resp = ngx.location.capture(path,{
        method = ngx.HTTP_GET,
        args = params,
    })
    if not resp then
        -- 记录错误信息，返回404
        ngx.log(ngx.ERR, "http请求查询失败, path: ", path , ", args: ", args)
        ngx.exit(404)
    end
    return resp.body
end
-- 将方法导出
local _M = {  
    read_http = read_http
}  
return _M
```

开启共享词典：

```
-- 引入自定义common工具模块，返回值是common中返回的 _M
local common = require("common")
-- 从 common中获取read_http这个函数
local read_http = common.read_http
-- 获取路径参数
local id = ngx.var[1]
-- 根据id查询商品。第一个参数是路径，第二个参数是请求参数
local itemJSON = read_http("/item/".. id, nil)
-- 根据id查询商品库存
local itemStockJSON = read_http("/item/stock/".. id, nil)
```

### 4.2.OpenResty 快速入门

我们希望达到的多级缓存架构如图：

![](<assets/1740016793760.png>)

其中：

*   windows 上的 nginx 用来做反向代理服务，将前端的查询商品的 ajax 请求代理到 OpenResty 集群
    
*   OpenResty 集群用来编写多级缓存业务
    

#### 4.2.1. 反向代理流程

现在，商品详情页使用的是假的商品数据。不过在浏览器中，可以看到页面有发起 ajax 请求查询真实商品数据。

这个请求如下：

![](<assets/1740016794075.png>)

**请求**地址是 localhost，端口是 80，就被 windows 上安装的 Nginx 服务给接收到了。然后**代理给了虚拟机的 OpenResty 集群：**

![](<assets/1740016794289.png>)

我们需要在 OpenResty 中编写业务，查询商品数据并返回到浏览器。

但是这次，我们先在 OpenResty 接收请求，返回假的商品数据。

#### 4.2.2.OpenResty 监听请求、响应 lua 文件的 JSON

OpenResty 的很多功能都依赖于其目录下的 Lua 库，需要在 nginx.conf 中指定依赖库的目录，并导入依赖：

**1）添加对 OpenResty 的 Lua 模块的加载**

这些模块要加载进来才能用 lua。 

修改虚拟机`/usr/local/openresty/nginx/conf/**nginx.conf**`文件，在其中的 http 下面，添加下面代码：

```
local obj = {
    name = 'jack',
    age = 21
}
-- 把 table 序列化为 json
local json = cjson.encode(obj)
```

**2）监听 / api/item 路径**

修改`/usr/local/openresty/nginx/conf/nginx.conf`文件，在 nginx.conf 的 server 下面，添加对 / api/item 这个路径的监听：

```
local json = '{"name": "jack", "age": 21}'
-- 反序列化 json为 table
local obj = cjson.decode(json);
print(obj.name)
```

这个监听，就类似于 SpringMVC 中的`@GetMapping("/api/item")`做路径映射。

而`content_by_lua_file lua/item.lua`则相当于调用 item.lua 这个文件，执行其中的业务，把结果返回给用户。相当于 java 中调用 service。

**最终的 nginx.conf 文件：**

```
-- 导入common函数库
local common = require('common')
local read_http = common.read_http
-- 导入cjson库
local cjson = require('cjson')
 
-- 获取路径参数
local id = ngx.var[1]
-- 根据id查询商品
local itemJSON = read_http("/item/".. id, nil)
-- 根据id查询商品库存
local itemStockJSON = read_http("/item/stock/".. id, nil)
 
-- JSON转化为lua的table
local item = cjson.decode(itemJSON)
local stock = cjson.decode(stockJSON)
 
-- 组合数据
item.stock = stock.stock
item.sold = stock.sold
 
-- 把item序列化为json 返回结果
ngx.say(cjson.encode(item))
```

#### 4.2.3.**`openresty`**响应 item.lua 假数据

**1）在`/usr/local/openresty/nginx`目录创建文件夹：lua**

```
upstream tomcat-cluster {
    hash $request_uri;
    server 192.168.150.1:8081;
    server 192.168.150.1:8082;
}
```

**2）在`/usr/loca/openresty/nginx/lua`文件夹下，新建文件：item.lua**

```
location /item {
    proxy_pass http://tomcat-cluster;
}
```

item.lua 的路径和 4.2.2 里设置的响应 lua 文件路径一致。

 **3）编写 item.lua，返回假数据**

item.lua 中，利用 **ngx.say() 函数**返回数据到 **Response** 中

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

lua 的 **ngx.say() 函数**就相当于 java 中 response.getWriter().writer(xxx);

**4）重新加载配置**

```
spring:
  redis:
    host: 192.168.150.101
```

刷新商品页面：http://localhost/item.html?id=10001，即可看到效果：

![](<assets/1740016794524.png>)

![](<assets/1740016794802.png>)

### 4.3.OpenResty 请求参数处理

上一节中，我们在 OpenResty 接收前端请求，但是返回的是假数据。

要返回真实数据，必须根据前端传递来的商品 id，查询商品信息才可以。

那么如何获取前端传递的商品参数呢？

#### 4.3.1. 获取参数的 API

OpenResty 中提供了一些 API 用来获取不同类型的前端请求参数：

![](<assets/1740016795066.png>)

**location 后有个 “~” 符号，表示正则表达式匹配。**

除了 JSON 参数，其他请求参数类型都是数组或 table 类型，因为参数不止一个。 

#### 4.3.2.lua 获取参数中 id 并响应

在前端发起的 ajax 请求如图：

![](<assets/1740016795378.png>)

可以看到商品 id 是以路径占位符方式传递的，因此可以利用正则表达式匹配的方式来获取 ID

**1）获取商品 id**

修改`/usr/loca/openresty/nginx/**nginx.conf**`文件中监听 / api/item 的代码，利用正则表达式获取 ID：

```
package com.heima.item.config;
 
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.heima.item.pojo.Item;
import com.heima.item.pojo.ItemStock;
import com.heima.item.service.IItemService;
import com.heima.item.service.IItemStockService;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.stereotype.Component;
 
import java.util.List;
 
@Component
public class RedisHandler implements InitializingBean {
 
    @Autowired
    private StringRedisTemplate redisTemplate;
 
    @Autowired
    private IItemService itemService;
    @Autowired
    private IItemStockService stockService;
 
    private static final ObjectMapper MAPPER = new ObjectMapper();
 
    @Override
    public void afterPropertiesSet() throws Exception {
        // 初始化缓存
        // 1.查询商品信息
        List<Item> itemList = itemService.list();
        // 2.放入缓存
        for (Item item : itemList) {
            // 2.1.item序列化为JSON
            String json = MAPPER.writeValueAsString(item);
            // 2.2.存入redis
            redisTemplate.opsForValue().set("item:id:" + item.getId(), json);
        }
 
        // 3.查询商品库存信息
        List<ItemStock> stockList = stockService.list();
        // 4.放入缓存
        for (ItemStock stock : stockList) {
            // 2.1.item序列化为JSON
            String json = MAPPER.writeValueAsString(stock);
            // 2.2.存入redis
            redisTemplate.opsForValue().set("item:stock:id:" + stock.getId(), json);
        }
    }
}
```

**注意：**

*   **location 后有个 “~” 符号，表示正则表达式匹配。**
*    虚拟机 openresty 的 Nginx 监听的是 localhost 的 8081 端口，正是项目的端口

**2）拼接 ID 并返回**

修改`/usr/loca/openresty/nginx/lua/item.lua`文件，获取 id 并拼接到结果中返回：

```
-- 导入redis
local redis = require('resty.redis')
-- 初始化redis
local red = redis:new()
red:set_timeouts(1000, 1000, 1000)
```

 **此时响应的结果仅 id 是真数据**

![](<assets/1740016795635.png>)

**3）重新加载并测试**

运行命令以重新加载 OpenResty 配置：

```
-- 关闭redis连接的工具方法，其实是放入连接池
local function close_redis(red)
    local pool_max_idle_time = 10000 -- 连接的空闲时间，单位是毫秒
    local pool_size = 100 --连接池大小
    local ok, err = red:set_keepalive(pool_max_idle_time, pool_size)
    if not ok then
        ngx.log(ngx.ERR, "放入redis连接池失败: ", err)
    end
end
```

刷新页面可以看到结果中已经带上了 ID：

![](<assets/1740016795794.png>)

### 4.4. 查询 Tomcat

拿到商品 ID 后，本应去缓存中查询商品信息，不过目前我们还未建立 nginx、redis 缓存。因此，这里我们先根据商品 id 去 tomcat 查询商品信息。我们实现如图部分：

![](<assets/1740016796021.png>)

需要注意的是，我们的 OpenResty 是在虚拟机，Tomcat 是在 Windows 电脑上。两者 IP 一定不要搞错了。

![](<assets/1740016796329.png>)

#### 4.4.1.lua 发送 http 请求的 API

nginx 提供了内部 API 用以**发送 http 请求：**

```
-- 查询redis的方法 ip和port是redis地址，key是查询的key
local function read_redis(ip, port, key)
    -- 获取一个连接
    local ok, err = red:connect(ip, port)
    if not ok then
        ngx.log(ngx.ERR, "连接redis失败 : ", err)
        return nil
    end
    -- 查询redis
    local resp, err = red:get(key)
    -- 查询失败处理
    if not resp then
        ngx.log(ngx.ERR, "查询Redis失败: ", err, ", key = " , key)
    end
    --得到的数据为空处理
    if resp == ngx.null then
        resp = nil
        ngx.log(ngx.ERR, "查询Redis数据为空, key = ", key)
    end
    close_redis(red)
    return resp
end
```

**返回的响应内容包括：**

*   resp.status：响应状态码
*   resp.header：响应头，是一个 table
*   resp.body：响应体，就是响应数据

**注意：**这里的 path 是路径，并不包含 IP 和端口。这个 **/path 请求会被 nginx 内部的 server 监听并处理。**

但是我们希望这个请求发送到 Tomcat 服务器，所以还需要在虚拟机 nginx.conf 编写一个 server 来**对这个路径做反向代理：**

```
-- 将方法导出
local _M = { 
    read_http = read_http,
    read_redis = read_redis
}  
return _M
```

原理如图：

![](<assets/1740016796532.png>)

#### 4.4.2. 封装 http 工具

下面，我们封装一个发送 Http 请求的工具，基于 ngx.location.capture 来实现查询 tomcat。

**1）添加反向代理，到 windows 的 Java 服务**

因为 item-service 中的接口都是 / item 开头，所以我们监听 / item 路径，代理到 windows 上的 tomcat 服务。

修改 `/usr/local/openresty/nginx/conf/nginx.conf`文件，添加一个 location：

```
-- 导入redis
local redis = require('resty.redis')
-- 初始化redis
local red = redis:new()
red:set_timeouts(1000, 1000, 1000)
 
-- 关闭redis连接的工具方法，其实是放入连接池
local function close_redis(red)
    local pool_max_idle_time = 10000 -- 连接的空闲时间，单位是毫秒
    local pool_size = 100 --连接池大小
    local ok, err = red:set_keepalive(pool_max_idle_time, pool_size)
    if not ok then
        ngx.log(ngx.ERR, "放入redis连接池失败: ", err)
    end
end
 
-- 查询redis的方法 ip和port是redis地址，key是查询的key
local function read_redis(ip, port, key)
    -- 获取一个连接
    local ok, err = red:connect(ip, port)
    if not ok then
        ngx.log(ngx.ERR, "连接redis失败 : ", err)
        return nil
    end
    -- 查询redis
    local resp, err = red:get(key)
    -- 查询失败处理
    if not resp then
        ngx.log(ngx.ERR, "查询Redis失败: ", err, ", key = " , key)
    end
    --得到的数据为空处理
    if resp == ngx.null then
        resp = nil
        ngx.log(ngx.ERR, "查询Redis数据为空, key = ", key)
    end
    close_redis(red)
    return resp
end
 
-- 封装函数，发送http请求，并解析响应
local function read_http(path, params)
    local resp = ngx.location.capture(path,{
        method = ngx.HTTP_GET,
        args = params,
    })
    if not resp then
        -- 记录错误信息，返回404
        ngx.log(ngx.ERR, "http查询失败, path: ", path , ", args: ", args)
        ngx.exit(404)
    end
    return resp.body
end
-- 将方法导出
local _M = {  
    read_http = read_http,
    read_redis = read_redis
}  
return _M
```

以后，只要我们调用`ngx.location.capture("/item")`，就一定能发送请求到 windows 的 tomcat 服务。

**2）封装工具类**

之前我们说过，OpenResty 启动时会加载以下两个目录中的工具文件：

![](<assets/1740016796839.png>)

所以，自定义的 http 工具也需要放到这个目录下。

在`/usr/local/openresty/lualib`目录下，新建一个 common.lua 文件：

```
-- 导入common函数库
local common = require('common')
local read_http = common.read_http
local read_redis = common.read_redis
-- 封装查询函数
function read_data(key, path, params)
    -- 查询本地缓存
    local val = read_redis("127.0.0.1", 6379, key)
    -- 判断查询结果
    if not val then
        ngx.log(ngx.ERR, "redis查询失败，尝试查询http， key: ", key)
        -- redis查询失败，去查询http
        val = read_http(path, params)
    end
    -- 返回数据
    return val
end
```

内容如下:

```
-- 导入common函数库
local common = require('common')
local read_http = common.read_http
local read_redis = common.read_redis
-- 导入cjson库
local cjson = require('cjson')
 
-- 封装查询函数
function read_data(key, path, params)
    -- 查询本地缓存
    local val = read_redis("127.0.0.1", 6379, key)
    -- 判断查询结果
    if not val then
        ngx.log(ngx.ERR, "redis查询失败，尝试查询http， key: ", key)
        -- redis查询失败，去查询http
        val = read_http(path, params)
    end
    -- 返回数据
    return val
end
 
-- 获取路径参数
local id = ngx.var[1]
 
-- 查询商品信息
local itemJSON = read_data("item:id:" .. id,  "/item/" .. id, nil)
-- 查询库存信息
local stockJSON = read_data("item:stock:id:" .. id, "/item/stock/" .. id, nil)
 
-- JSON转化为lua的table
local item = cjson.decode(itemJSON)
local stock = cjson.decode(stockJSON)
-- 组合数据
item.stock = stock.stock
item.sold = stock.sold
 
-- 把item序列化为json 返回结果
ngx.say(cjson.encode(item))
```

这个工具将 read_http 函数封装到_M 这个 table 类型的变量中，并且返回，这类似于导出。

使用的时候，可以利用`require('common')`来导入该函数库，这里的 common 是函数库的文件名。

**3）实现商品查询**

最后，我们修改`/usr/local/openresty/lua/item.lua`文件，利用刚刚封装的函数库实现对 tomcat 的查询：

```
# 共享字典，也就是本地缓存，名称叫做：item_cache，大小150m
 lua_shared_dict item_cache 150m;
```

这里查询到的结果是 json 字符串，并且包含商品、库存两个 json 字符串，页面最终需要的是把两个 json 拼接为一个 json：

![](<assets/1740016797069.png>)

这就需要我们先把 JSON 变为 lua 的 table，完成数据整合后，再转为 JSON。

#### 4.4.3.CJSON 工具类

OpenResty 提供了一个 cjson 的模块用来**处理 JSON 的序列化和反序列化。**

官方地址： https://github.com/openresty/lua-cjson/

**1）引入 cjson 模块：**

```
-- 获取本地缓存对象
local item_cache = ngx.shared.item_cache
-- 存储, 指定key、value、过期时间，单位s，默认为0代表永不过期
item_cache:set('key', 'value', 1000)
-- 读取
local val = item_cache:get('key')
```

**2）序列化：**

```
-- 导入共享词典，本地缓存
local item_cache = ngx.shared.item_cache
 
-- 封装查询函数
function read_data(key, expire, path, params)
    -- 查询本地缓存
    local val = item_cache:get(key)
    if not val then
        ngx.log(ngx.ERR, "本地缓存查询失败，尝试查询Redis， key: ", key)
        -- 查询redis
        val = read_redis("127.0.0.1", 6379, key)
        -- 判断查询结果
        if not val then
            ngx.log(ngx.ERR, "redis查询失败，尝试查询http， key: ", key)
            -- redis查询失败，去查询http
            val = read_http(path, params)
        end
    end
    -- 查询成功，把数据写入本地缓存
    item_cache:set(key, val, expire)
    -- 返回数据
    return val
end
```

**3）反序列化：**

```
-- 导入common函数库
local common = require('common')
local read_http = common.read_http
local read_redis = common.read_redis
-- 导入cjson库
local cjson = require('cjson')
-- 导入共享词典，本地缓存
local item_cache = ngx.shared.item_cache
 
-- 封装查询函数
function read_data(key, expire, path, params)
    -- 查询本地缓存
    local val = item_cache:get(key)
    if not val then
        ngx.log(ngx.ERR, "本地缓存查询失败，尝试查询Redis， key: ", key)
        -- 查询redis
        val = read_redis("127.0.0.1", 6379, key)
        -- 判断查询结果
        if not val then
            ngx.log(ngx.ERR, "redis查询失败，尝试查询http， key: ", key)
            -- redis查询失败，去查询http
            val = read_http(path, params)
        end
    end
    -- 查询成功，把数据写入本地缓存
    item_cache:set(key, val, expire)
    -- 返回数据
    return val
end
 
-- 获取路径参数
local id = ngx.var[1]
 
-- 查询商品信息
local itemJSON = read_data("item:id:" .. id, 1800,  "/item/" .. id, nil)
-- 查询库存信息
local stockJSON = read_data("item:stock:id:" .. id, 60, "/item/stock/" .. id, nil)
 
-- JSON转化为lua的table
local item = cjson.decode(itemJSON)
local stock = cjson.decode(stockJSON)
-- 组合数据
item.stock = stock.stock
item.sold = stock.sold
 
-- 把item序列化为json 返回结果
ngx.say(cjson.encode(item))
```

#### 4.4.4. 实现 Tomcat 查询

下面，我们修改之前的 item.lua 中的业务，添加 json 处理功能：

```
<dependency>
    <groupId>top.javatool</groupId>
    <artifactId>canal-spring-boot-starter</artifactId>
    <version>1.2.1-RELEASE</version>
</dependency>
```

#### 4.4.5. 基于 ID 负载均衡

刚才的代码中，我们的 tomcat 是单机部署。而实际开发中，tomcat 一定是集群模式：

![](<assets/1740016797297.png>)

因此，OpenResty 需要对 tomcat 集群做负载均衡。

而默认的负载均衡规则是轮询模式，当我们查询 / item/10001 时：

*   第一次会访问 8081 端口的 tomcat 服务，在该服务内部就形成了 JVM 进程缓存
*   第二次会访问 8082 端口的 tomcat 服务，该服务内部没有 JVM 缓存（因为 JVM 缓存无法共享），会查询数据库
*   …

你看，因为轮询的原因，第一次查询 8081 形成的 JVM 缓存并未生效，直到下一次再次访问到 8081 时才可以生效，缓存命中率太低了。

怎么办？

如果能让同一个商品，每次查询时都访问同一个 tomcat 服务，那么 JVM 缓存就一定能生效了。

也就是说，我们需要根据商品 id 做负载均衡，而不是轮询。

**1）原理**

nginx 提供了基于请求路径做负载均衡的算法：

nginx 根据请求路径做 hash 运算，把得到的数值对 tomcat 服务的数量取余，余数是几，就访问第几个服务，实现负载均衡。

**例如：**

*   我们的请求路径是 /item/10001
*   **tomcat 总数为 2 台**（8081、8082）
*   对请求路径 / item/1001 做 hash 运算求余的结果为 1
*   则访问第一个 tomcat 服务，也就是 8081

只要 id 不变，每次 hash 运算结果也不会变，那就可以保证同一个商品，一直访问同一个 tomcat 服务，确保 JVM 缓存生效。

**2）实现**

修改`**/usr**/local/openresty/nginx/conf/**nginx.conf**`**文件**，实现基于 ID 做负载均衡。

首先，**定义 tomcat 集群**，并设置基于路径做负载均衡：

```
canal:
 destination: heima # canal的集群名字，要与安装canal时设置的名称一致
 server: 192.168.150.101:11111 # canal服务地址
```

然后，修改**对 tomcat 服务的反向代理**，目标指向 **tomcat 集群：**

```
package com.heima.item.pojo;
 
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableField;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import lombok.Data;
import org.springframework.data.annotation.Id;
import org.springframework.data.annotation.Transient;
 
import javax.persistence.Column;
import java.util.Date;
 
@Data
@TableName("tb_item")
public class Item {
    @TableId(type = IdType.AUTO)
    @Id
    private Long id;//商品id
    @Column(name = "name")
    private String name;//商品名称
    private String title;//商品标题
    private Long price;//价格（分）
    private String image;//商品图片
    private String category;//分类名称
    private String brand;//品牌名称
    private String spec;//规格
    private Integer status;//商品状态 1-正常，2-下架
    private Date createTime;//创建时间
    private Date updateTime;//更新时间
    @TableField(exist = false)
    @Transient
    private Integer stock;
    @TableField(exist = false)
    @Transient
    private Integer sold;
}
```

重新加载 OpenResty

```
package com.heima.item.canal;
 
import com.github.benmanes.caffeine.cache.Cache;
import com.heima.item.config.RedisHandler;
import com.heima.item.pojo.Item;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import top.javatool.canal.client.annotation.CanalTable;
import top.javatool.canal.client.handler.EntryHandler;
 
@CanalTable("tb_item")
@Component
public class ItemHandler implements EntryHandler<Item> {
 
    @Autowired
    private RedisHandler redisHandler;
    @Autowired
    private Cache<Long, Item> itemCache;
 
    @Override
    public void insert(Item item) {
        // 写数据到JVM进程缓存
        itemCache.put(item.getId(), item);
        // 写数据到redis
        redisHandler.saveItem(item);
    }
 
    @Override
    public void update(Item before, Item after) {
        // 写数据到JVM进程缓存
        itemCache.put(after.getId(), after);
        // 写数据到redis
        redisHandler.saveItem(after);
    }
 
    @Override
    public void delete(Item item) {
        // 删除数据到JVM进程缓存
        itemCache.invalidate(item.getId());
        // 删除数据到redis
        redisHandler.deleteItemById(item.getId());
    }
}
```

**3）测试**

启动两台 tomcat 服务：

![](<assets/1740016797665.png>)

同时启动：

![](<assets/1740016797913.png>)

清空日志后，再次访问页面，可以看到不同 id 的商品，访问到了不同的 tomcat 服务：

![](<assets/1740016798156.png>)

![](<assets/1740016798386.png>)

### 4.5.Redis 缓存预热

Redis 缓存会面临冷启动问题：

**冷启动**：服务刚刚启动时，Redis 中并没有缓存，如果所有商品数据都在第一次查询时添加缓存，可能会给数据库带来较大压力。

**缓存预热**：在实际开发中，我们可以利用大数据统计用户访问的热点数据，在项目启动时将这些热点数据提前查询并保存到 Redis 中。

我们数据量较少，并且没有数据统计相关功能，目前可以在启动时将所有数据都放入缓存中。

1）利用 Docker 安装 Redis

```
package com.heima.item.config;
 
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.heima.item.pojo.Item;
import com.heima.item.pojo.ItemStock;
import com.heima.item.service.IItemService;
import com.heima.item.service.IItemStockService;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.stereotype.Component;
 
import java.util.List;
 
@Component
public class RedisHandler implements InitializingBean {
 
    @Autowired
    private StringRedisTemplate redisTemplate;
 
    @Autowired
    private IItemService itemService;
    @Autowired
    private IItemStockService stockService;
 
    private static final ObjectMapper MAPPER = new ObjectMapper();
 
    @Override
    public void afterPropertiesSet() throws Exception {
        // 初始化缓存
        // 1.查询商品信息
        List<Item> itemList = itemService.list();
        // 2.放入缓存
        for (Item item : itemList) {
            // 2.1.item序列化为JSON
            String json = MAPPER.writeValueAsString(item);
            // 2.2.存入redis
            redisTemplate.opsForValue().set("item:id:" + item.getId(), json);
        }
 
        // 3.查询商品库存信息
        List<ItemStock> stockList = stockService.list();
        // 4.放入缓存
        for (ItemStock stock : stockList) {
            // 2.1.item序列化为JSON
            String json = MAPPER.writeValueAsString(stock);
            // 2.2.存入redis
            redisTemplate.opsForValue().set("item:stock:id:" + stock.getId(), json);
        }
    }
 
    public void saveItem(Item item) {
        try {
            String json = MAPPER.writeValueAsString(item);
            redisTemplate.opsForValue().set("item:id:" + item.getId(), json);
        } catch (JsonProcessingException e) {
            throw new RuntimeException(e);
        }
    }
 
    public void deleteItemById(Long id) {
        redisTemplate.delete("item:id:" + id);
    }
}
```

2）在 item-service 服务中引入 Redis 依赖

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

3）配置 Redis 地址

```
spring:
  redis:
    host: 192.168.150.101
```

4）编写初始化类

缓存预热需要在项目启动时完成，并且必须是拿到 RedisTemplate 之后。

这里我们利用 InitializingBean 接口来实现，因为 InitializingBean 可以在对象被 Spring 创建并且成员变量全部注入后执行。

```
package com.heima.item.config;
 
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.heima.item.pojo.Item;
import com.heima.item.pojo.ItemStock;
import com.heima.item.service.IItemService;
import com.heima.item.service.IItemStockService;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.stereotype.Component;
 
import java.util.List;
 
@Component
public class RedisHandler implements InitializingBean {
 
    @Autowired
    private StringRedisTemplate redisTemplate;
 
    @Autowired
    private IItemService itemService;
    @Autowired
    private IItemStockService stockService;
 
    private static final ObjectMapper MAPPER = new ObjectMapper();
 
    @Override
    public void afterPropertiesSet() throws Exception {
        // 初始化缓存
        // 1.查询商品信息
        List<Item> itemList = itemService.list();
        // 2.放入缓存
        for (Item item : itemList) {
            // 2.1.item序列化为JSON
            String json = MAPPER.writeValueAsString(item);
            // 2.2.存入redis
            redisTemplate.opsForValue().set("item:id:" + item.getId(), json);
        }
 
        // 3.查询商品库存信息
        List<ItemStock> stockList = stockService.list();
        // 4.放入缓存
        for (ItemStock stock : stockList) {
            // 2.1.item序列化为JSON
            String json = MAPPER.writeValueAsString(stock);
            // 2.2.存入redis
            redisTemplate.opsForValue().set("item:stock:id:" + stock.getId(), json);
        }
    }
}
```

### 4.6. 查询 Redis 缓存

现在，Redis 缓存已经准备就绪，我们可以再 OpenResty 中实现查询 Redis 的逻辑了。如下图红框所示：

![](<assets/1740016798619.png>)

当请求进入 OpenResty 之后：

*   优先查询 Redis 缓存
*   如果 Redis 缓存未命中，再查询 Tomcat

#### 4.6.1. 封装 Redis 工具

OpenResty 提供了操作 Redis 的模块，我们只要引入该模块就能直接使用。但是为了方便，我们将 Redis 操作封装到之前的 common.lua 工具库中。

修改`/usr/local/openresty/lualib/common.lua`文件：

1）引入 Redis 模块，并初始化 Redis 对象

```
-- 导入redis
local redis = require('resty.redis')
-- 初始化redis
local red = redis:new()
red:set_timeouts(1000, 1000, 1000)
```

2）封装函数，用来释放 Redis 连接，其实是放入连接池

```
-- 关闭redis连接的工具方法，其实是放入连接池
local function close_redis(red)
    local pool_max_idle_time = 10000 -- 连接的空闲时间，单位是毫秒
    local pool_size = 100 --连接池大小
    local ok, err = red:set_keepalive(pool_max_idle_time, pool_size)
    if not ok then
        ngx.log(ngx.ERR, "放入redis连接池失败: ", err)
    end
end
```

3）封装函数，根据 key 查询 Redis 数据

```
-- 查询redis的方法 ip和port是redis地址，key是查询的key
local function read_redis(ip, port, key)
    -- 获取一个连接
    local ok, err = red:connect(ip, port)
    if not ok then
        ngx.log(ngx.ERR, "连接redis失败 : ", err)
        return nil
    end
    -- 查询redis
    local resp, err = red:get(key)
    -- 查询失败处理
    if not resp then
        ngx.log(ngx.ERR, "查询Redis失败: ", err, ", key = " , key)
    end
    --得到的数据为空处理
    if resp == ngx.null then
        resp = nil
        ngx.log(ngx.ERR, "查询Redis数据为空, key = ", key)
    end
    close_redis(red)
    return resp
end
```

4）导出

```
-- 将方法导出
local _M = { 
    read_http = read_http,
    read_redis = read_redis
}  
return _M
```

完整的 common.lua：

```
-- 导入redis
local redis = require('resty.redis')
-- 初始化redis
local red = redis:new()
red:set_timeouts(1000, 1000, 1000)
 
-- 关闭redis连接的工具方法，其实是放入连接池
local function close_redis(red)
    local pool_max_idle_time = 10000 -- 连接的空闲时间，单位是毫秒
    local pool_size = 100 --连接池大小
    local ok, err = red:set_keepalive(pool_max_idle_time, pool_size)
    if not ok then
        ngx.log(ngx.ERR, "放入redis连接池失败: ", err)
    end
end
 
-- 查询redis的方法 ip和port是redis地址，key是查询的key
local function read_redis(ip, port, key)
    -- 获取一个连接
    local ok, err = red:connect(ip, port)
    if not ok then
        ngx.log(ngx.ERR, "连接redis失败 : ", err)
        return nil
    end
    -- 查询redis
    local resp, err = red:get(key)
    -- 查询失败处理
    if not resp then
        ngx.log(ngx.ERR, "查询Redis失败: ", err, ", key = " , key)
    end
    --得到的数据为空处理
    if resp == ngx.null then
        resp = nil
        ngx.log(ngx.ERR, "查询Redis数据为空, key = ", key)
    end
    close_redis(red)
    return resp
end
 
-- 封装函数，发送http请求，并解析响应
local function read_http(path, params)
    local resp = ngx.location.capture(path,{
        method = ngx.HTTP_GET,
        args = params,
    })
    if not resp then
        -- 记录错误信息，返回404
        ngx.log(ngx.ERR, "http查询失败, path: ", path , ", args: ", args)
        ngx.exit(404)
    end
    return resp.body
end
-- 将方法导出
local _M = {  
    read_http = read_http,
    read_redis = read_redis
}  
return _M
```

#### 4.6.2. 实现 Redis 查询

接下来，我们就可以去修改 item.lua 文件，实现对 Redis 的查询了。

查询逻辑是：

*   根据 id 查询 Redis
*   如果查询失败则继续查询 Tomcat
*   将查询结果返回

1）修改`/usr/local/openresty/lua/item.lua`文件，添加一个查询函数：

```
-- 导入common函数库
local common = require('common')
local read_http = common.read_http
local read_redis = common.read_redis
-- 封装查询函数
function read_data(key, path, params)
    -- 查询本地缓存
    local val = read_redis("127.0.0.1", 6379, key)
    -- 判断查询结果
    if not val then
        ngx.log(ngx.ERR, "redis查询失败，尝试查询http， key: ", key)
        -- redis查询失败，去查询http
        val = read_http(path, params)
    end
    -- 返回数据
    return val
end
```

2）而后修改商品查询、库存查询的业务：

![](<assets/1740016798884.png>)

3）完整的 item.lua 代码：

```
-- 导入common函数库
local common = require('common')
local read_http = common.read_http
local read_redis = common.read_redis
-- 导入cjson库
local cjson = require('cjson')
 
-- 封装查询函数
function read_data(key, path, params)
    -- 查询本地缓存
    local val = read_redis("127.0.0.1", 6379, key)
    -- 判断查询结果
    if not val then
        ngx.log(ngx.ERR, "redis查询失败，尝试查询http， key: ", key)
        -- redis查询失败，去查询http
        val = read_http(path, params)
    end
    -- 返回数据
    return val
end
 
-- 获取路径参数
local id = ngx.var[1]
 
-- 查询商品信息
local itemJSON = read_data("item:id:" .. id,  "/item/" .. id, nil)
-- 查询库存信息
local stockJSON = read_data("item:stock:id:" .. id, "/item/stock/" .. id, nil)
 
-- JSON转化为lua的table
local item = cjson.decode(itemJSON)
local stock = cjson.decode(stockJSON)
-- 组合数据
item.stock = stock.stock
item.sold = stock.sold
 
-- 把item序列化为json 返回结果
ngx.say(cjson.encode(item))
```

### 4.7.Nginx 本地缓存

现在，整个多级缓存中只差最后一环，也就是 nginx 的本地缓存了。如图：

![](<assets/1740016799064.png>)

#### 4.7.1. 本地缓存 API

OpenResty 为 Nginx 提供了 **shard dict** 的功能，可以在 nginx 的多个 worker 之间共享数据，实现缓存功能。

1）开启共享字典，在 nginx.conf 的 http 下添加配置：

```
# 共享字典，也就是本地缓存，名称叫做：item_cache，大小150m
 lua_shared_dict item_cache 150m;
```

2）操作共享字典：

```
-- 获取本地缓存对象
local item_cache = ngx.shared.item_cache
-- 存储, 指定key、value、过期时间，单位s，默认为0代表永不过期
item_cache:set('key', 'value', 1000)
-- 读取
local val = item_cache:get('key')
```

#### 4.7.2. 实现本地缓存查询

1）修改`/usr/local/openresty/lua/item.lua`文件，修改 read_data 查询函数，添加本地缓存逻辑：

```
-- 导入共享词典，本地缓存
local item_cache = ngx.shared.item_cache
 
-- 封装查询函数
function read_data(key, expire, path, params)
    -- 查询本地缓存
    local val = item_cache:get(key)
    if not val then
        ngx.log(ngx.ERR, "本地缓存查询失败，尝试查询Redis， key: ", key)
        -- 查询redis
        val = read_redis("127.0.0.1", 6379, key)
        -- 判断查询结果
        if not val then
            ngx.log(ngx.ERR, "redis查询失败，尝试查询http， key: ", key)
            -- redis查询失败，去查询http
            val = read_http(path, params)
        end
    end
    -- 查询成功，把数据写入本地缓存
    item_cache:set(key, val, expire)
    -- 返回数据
    return val
end
```

2）修改 item.lua 中查询商品和库存的业务，实现最新的 read_data 函数：

![](<assets/1740016799354.png>)

其实就是多了缓存时间参数，过期后 nginx 缓存会自动删除，下次访问即可更新缓存。

这里给商品基本信息设置超时时间为 30 分钟，库存为 1 分钟。

因为库存更新频率较高，如果缓存时间过长，可能与数据库差异较大。

3）完整的 item.lua 文件：

```
-- 导入common函数库
local common = require('common')
local read_http = common.read_http
local read_redis = common.read_redis
-- 导入cjson库
local cjson = require('cjson')
-- 导入共享词典，本地缓存
local item_cache = ngx.shared.item_cache
 
-- 封装查询函数
function read_data(key, expire, path, params)
    -- 查询本地缓存
    local val = item_cache:get(key)
    if not val then
        ngx.log(ngx.ERR, "本地缓存查询失败，尝试查询Redis， key: ", key)
        -- 查询redis
        val = read_redis("127.0.0.1", 6379, key)
        -- 判断查询结果
        if not val then
            ngx.log(ngx.ERR, "redis查询失败，尝试查询http， key: ", key)
            -- redis查询失败，去查询http
            val = read_http(path, params)
        end
    end
    -- 查询成功，把数据写入本地缓存
    item_cache:set(key, val, expire)
    -- 返回数据
    return val
end
 
-- 获取路径参数
local id = ngx.var[1]
 
-- 查询商品信息
local itemJSON = read_data("item:id:" .. id, 1800,  "/item/" .. id, nil)
-- 查询库存信息
local stockJSON = read_data("item:stock:id:" .. id, 60, "/item/stock/" .. id, nil)
 
-- JSON转化为lua的table
local item = cjson.decode(itemJSON)
local stock = cjson.decode(stockJSON)
-- 组合数据
item.stock = stock.stock
item.sold = stock.sold
 
-- 把item序列化为json 返回结果
ngx.say(cjson.encode(item))
```

## 5. 缓存同步

大多数情况下，浏览器查询到的都是缓存数据，如果缓存数据与数据库数据存在较大差异，可能会产生比较严重的后果。

所以我们必须保证数据库数据、缓存数据的一致性，这就是缓存与数据库的同步。

### 5.1. 数据同步策略

缓存数据同步的常见方式有三种：

**设置有效期**：给缓存设置有效期，到期后自动删除。再次查询时更新

*   优势：简单、方便
*   缺点：时效性差，缓存过期之前可能不一致
*   场景：更新频率较低，时效性要求低的业务

**同步双写**：在修改数据库的同时，直接修改缓存

*   优势：时效性强，缓存与数据库强一致
*   缺点：有代码侵入，耦合度高；
*   场景：对一致性、时效性要求较高的缓存数据

** 异步通知：** 修改数据库时发送事件通知，相关服务监听到通知后修改缓存数据

*   优势：低耦合，可以同时通知多个缓存服务
*   缺点：时效性一般，可能存在中间不一致状态
*   场景：时效性要求一般，有多个服务需要同步

而异步实现又可以基于 MQ 或者 Canal 来实现：

1）基于 MQ 的异步通知：

![](<assets/1740016799713.png>)

解读：

*   商品服务完成对数据的修改后，只需要发送一条消息到 MQ 中。
*   缓存服务监听 MQ 消息，然后完成对缓存的更新

依然有少量的代码侵入。

2）基于 Canal 的通知

![](<assets/1740016800045.png>)

解读：

*   商品服务完成商品修改后，业务直接结束，没有任何代码侵入
*   Canal 监听 MySQL 变化，当发现变化后，立即通知缓存服务
*   缓存服务接收到 canal 通知，更新缓存

代码零侵入

### 5.2. 安装 Canal

#### 5.2.1. 认识 Canal

**Canal [kə’næl]**，译意为水道 / 管道 / 沟渠，canal 是阿里巴巴旗下的一款开源项目，基于 Java 开发。基于数据库增量日志解析，提供增量数据订阅 & 消费。GitHub 的地址：https://github.com/alibaba/canal

Canal 是基于 mysql 的主从同步来实现的，MySQL 主从同步的原理如下：

![](<assets/1740016800356.png>)

*   1）MySQL master 将数据变更写入二进制日志 ( binary log），其中记录的数据叫做 binary log events
*   2）MySQL slave 将 master 的 binary log events 拷贝到它的中继日志 (relay log)
*   3）MySQL slave 重放 relay log 中事件，将数据变更反映它自己的数据

而 Canal 就是把自己伪装成 MySQL 的一个 slave 节点，从而监听 master 的 binary log 变化。再把得到的变化信息通知给 Canal 的客户端，进而完成对其它数据库的同步。

![](<assets/1740016800668.png>)

#### 5.2.2. 安装 Canal

安装和配置 Canal 参考课前资料文档：

![](<assets/1740016800887.png>)

### 5.3. 监听 Canal

Canal 提供了各种语言的客户端，当 Canal 监听到 binlog 变化时，会通知 Canal 的客户端。

![](<assets/1740016801072.png>)

我们可以利用 Canal 提供的 Java 客户端，监听 Canal 通知消息。当收到变化的消息时，完成对缓存的更新。

不过这里我们会使用 GitHub 上的第三方开源的 canal-starter 客户端。地址：https://github.com/NormanGyllenhaal/canal-client

与 SpringBoot 完美整合，自动装配，比官方客户端要简单好用很多。

#### 5.3.1. 引入依赖：

```
<dependency>
    <groupId>top.javatool</groupId>
    <artifactId>canal-spring-boot-starter</artifactId>
    <version>1.2.1-RELEASE</version>
</dependency>
```

#### 5.3.2. 编写配置：

```
canal:
 destination: heima # canal的集群名字，要与安装canal时设置的名称一致
 server: 192.168.150.101:11111 # canal服务地址
```

#### 5.3.3. 修改 Item 实体类

通过 @Id、@Column、等注解完成 Item 与数据库表字段的映射：

```
package com.heima.item.pojo;
 
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableField;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import lombok.Data;
import org.springframework.data.annotation.Id;
import org.springframework.data.annotation.Transient;
 
import javax.persistence.Column;
import java.util.Date;
 
@Data
@TableName("tb_item")
public class Item {
    @TableId(type = IdType.AUTO)
    @Id
    private Long id;//商品id
    @Column(name = "name")
    private String name;//商品名称
    private String title;//商品标题
    private Long price;//价格（分）
    private String image;//商品图片
    private String category;//分类名称
    private String brand;//品牌名称
    private String spec;//规格
    private Integer status;//商品状态 1-正常，2-下架
    private Date createTime;//创建时间
    private Date updateTime;//更新时间
    @TableField(exist = false)
    @Transient
    private Integer stock;
    @TableField(exist = false)
    @Transient
    private Integer sold;
}
```

#### 5.3.4. 编写监听器

通过实现`EntryHandler<T>`接口编写监听器，监听 Canal 消息。注意两点：

*   实现类通过`@CanalTable("tb_item")`指定监听的表信息
*   EntryHandler 的泛型是与表对应的实体类

```
package com.heima.item.canal;
 
import com.github.benmanes.caffeine.cache.Cache;
import com.heima.item.config.RedisHandler;
import com.heima.item.pojo.Item;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import top.javatool.canal.client.annotation.CanalTable;
import top.javatool.canal.client.handler.EntryHandler;
 
@CanalTable("tb_item")
@Component
public class ItemHandler implements EntryHandler<Item> {
 
    @Autowired
    private RedisHandler redisHandler;
    @Autowired
    private Cache<Long, Item> itemCache;
 
    @Override
    public void insert(Item item) {
        // 写数据到JVM进程缓存
        itemCache.put(item.getId(), item);
        // 写数据到redis
        redisHandler.saveItem(item);
    }
 
    @Override
    public void update(Item before, Item after) {
        // 写数据到JVM进程缓存
        itemCache.put(after.getId(), after);
        // 写数据到redis
        redisHandler.saveItem(after);
    }
 
    @Override
    public void delete(Item item) {
        // 删除数据到JVM进程缓存
        itemCache.invalidate(item.getId());
        // 删除数据到redis
        redisHandler.deleteItemById(item.getId());
    }
}
```

在这里对 Redis 的操作都封装到了 RedisHandler 这个对象中，是我们之前做缓存预热时编写的一个类，内容如下：

```
package com.heima.item.config;
 
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.heima.item.pojo.Item;
import com.heima.item.pojo.ItemStock;
import com.heima.item.service.IItemService;
import com.heima.item.service.IItemStockService;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.stereotype.Component;
 
import java.util.List;
 
@Component
public class RedisHandler implements InitializingBean {
 
    @Autowired
    private StringRedisTemplate redisTemplate;
 
    @Autowired
    private IItemService itemService;
    @Autowired
    private IItemStockService stockService;
 
    private static final ObjectMapper MAPPER = new ObjectMapper();
 
    @Override
    public void afterPropertiesSet() throws Exception {
        // 初始化缓存
        // 1.查询商品信息
        List<Item> itemList = itemService.list();
        // 2.放入缓存
        for (Item item : itemList) {
            // 2.1.item序列化为JSON
            String json = MAPPER.writeValueAsString(item);
            // 2.2.存入redis
            redisTemplate.opsForValue().set("item:id:" + item.getId(), json);
        }
 
        // 3.查询商品库存信息
        List<ItemStock> stockList = stockService.list();
        // 4.放入缓存
        for (ItemStock stock : stockList) {
            // 2.1.item序列化为JSON
            String json = MAPPER.writeValueAsString(stock);
            // 2.2.存入redis
            redisTemplate.opsForValue().set("item:stock:id:" + stock.getId(), json);
        }
    }
 
    public void saveItem(Item item) {
        try {
            String json = MAPPER.writeValueAsString(item);
            redisTemplate.opsForValue().set("item:id:" + item.getId(), json);
        } catch (JsonProcessingException e) {
            throw new RuntimeException(e);
        }
    }
 
    public void deleteItemById(Long id) {
        redisTemplate.delete("item:id:" + id);
    }
}
```