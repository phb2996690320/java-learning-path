---
url: https://blog.csdn.net/qq_40991313/article/details/143023093
title: 【架构设计】从设计模式入手，设计一个符合设计原则的积分兑换系统_批量解析积分文件计算发放逻辑业务设计 - CSDN 博客
date: 2025-02-20 13:58:04
tag: 
summary: 
---
**导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

推荐专栏：

[设计模式之美_设计模式_代码重构 - 极客时间](https://time.geekbang.org/column/intro/100039001?utm_campaign=geektime_search&utm_content=geektime_search&utm_medium=geektime_search&utm_source=geektime_search&utm_term=geektime_search "设计模式之美_设计模式_代码重构-极客时间")

**目录**

[一、基本介绍](#t0)

[1.1 积分规则](#t1)

[1.2 需求](#t2)

[二、模块划分](#t3)

[三、交互关系](#t4)

[四、系统设计](#t5)

[4.1 基本介绍](#t6)

[4.2 数据库设计](#t7)

[4.2.1 设计原则](#t8) 

[4.2.2 设计方案](#t9)

[4.3 接口设计](#t10)

[4.4 业务模型设计](#t11)

[五、部署方式](#t12)

## 一、基本介绍

### 1.1 积分规则

通过淘宝、移动商城了解积分规则。

**积分规则**：

*   **赚取积分**：下订单、每日签到、评论等
*   **消费积分**：抵扣订单金额、兑换优惠券、积分换购、参与活动扣积分等。

**案例：** 

以京东的京豆为例：

**赚取积分**：

*   **下单返积分**：下单按成交金额比例返积分，在促销时会返回更高比例的积分。
    *   例如京东里，每消费 1 元人民币获得 1 京豆，1 京豆相当于 0.01 元。在 618 期间，双倍返回积分，每消费 1 元获得 2 京豆
*   **每日签到**：每日签到获取一定积分，连续签到奖励增多。
    *   例如京东每天签到可获得 1-10 京豆。连续签到满 7 天、15 天或 30 天时，京东可能会发放额外的京豆作为奖励。
*   **评论**：评论越优质，获得积分越多。
    *   例如京东文字评价可获得 10 京豆；文字 + 图片评价可获得 30 京豆；文字 + 视频评价可获得 50 京豆。
*   **活动**：通过东东农场等活动可以获得京豆奖励。

**消费积分**：

*   **抵扣订单金额**：下单时，每 100 京豆能抵扣 1 元
*   **兑换优惠券**：1000 京豆可以兑换一张满 200 元减 20 元
*   **积分换购**：在活动期间，一个价值 100 元的商品，可以通过 10000 京豆秒杀。
*   **参与活动扣积分**：在转盘活动中，每天可以旋转一次，每次消耗 10 京豆，随机获得礼品。

### **1.2 需求**

*   **获取积分**：用户在获取积分的时候，会告知积分的有效期；
    
*   **使用积分**：用户在使用积分的时候，会优先使用快过期的积分；
    
*   **查询所有积分**：用户在查询积分明细的时候，会显示积分的有效期和状态（是否过期）；
    
*   **查询可用积分**：用户在查询总可用积分的时候，会排除掉过期的积分。
    

## 二、模块划分

三种模块划分方法：

*   **合并到上级系统**：积分的所有业务不直接抽取成独立系统，而是合并到上级 “营销系统” 中。
    *   **耦合度**：中等
    *   **示例**：订单系统下订单后，发 MQ 到营销系统，执行计算并增加积分的业务。
*   **分散到各业务系统（推荐）**：积分的所有业务不直接抽取成独立系统，而是分散到订单、评论等系统中。
    *   **耦合度**：最低。因为在业务层面，各系统独立管理自己的积分规则，彼此之间相对独立。符合高内聚、低耦合的思想。
    *   **示例**：积分系统只提供简单的增删改查接口，下订单和根据规则计算积分都由订单系统完成，然后调用积分系统增加积分。
*   **拆分积分系统**：增删改查、计算积分等所有积分相关的业务都拆成一个独立的系统。
    *   **耦合度**：最高。因为下层系统（被调用方）包含了太多上层系统（调用方）的业务数据，导致职责不清晰。积分系统的研发人员不仅需要清楚订单、活动等业务，还需要跨部门合作，跟订单、优惠券等系统进行跨部门沟通和协作。《人月神话》讲过，公司规模越大，沟通成本越低。
    *   **示例**：订单系统下订单后，发 MQ 到营销系统，执行计算并增加积分的业务。

## 三、交互关系

交互关系有两种：

*   **同步**：新订单落库后，需要同步等待 “增加积分” 操作返回结果后，整个下订单业务才算完成。
*   **异步**：新订单落库后，直接发 MQ 异步增加积分，此时订单业务已经完成，增加积分的操作在消息队列中消费。

具体选用哪种需要考虑具体业务场景，如果需要强一致性就选同步，如果需要高性能就选异步。

## 四、系统设计

### 4.1 基本介绍

系统设计三要素：

*   **数据库设计**：最重要，在设计完成后改动影响最大，改动后需要迁移数据库数据、调整 controller、service、dao 代码。
*   **接口设计**：改动影响中等，修改后会需要跟着改动前端接口的调用。
*   **业务逻辑设计**：改动影响较低。改动时一般只需要修改 service 中的代码，由代码开发者自己就能改，对其他人影响较低。

### 4.2 数据库设计

#### 4.2.1 设计原则 

**数据库设计三范式和反范式**：

*   **第一二三范式（属性、主键、非主键）：**
    *   每个属性不可再分
    *   表必须有且只有一个主键
    *   非主键列必须直接依赖于主键
*   **反范式（使用冗余字段）**：为提高查询效率，可添加不常更新的字段为冗余字段。例如 “商品信息表” 里，已有 “分类 id” 字段，我们可以加上 “分类名” 字段（商品所属分类一般不会更改）。虽然违反了第一范式，但提高了查询效率（因为不用再关联查询分类名）。

**阿里规约——数据库篇**：

[【阿里规约】阿里开发手册解读——数据库和 ORM 篇 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/135678947 "【阿里规约】阿里开发手册解读——数据库和ORM篇-CSDN博客")

#### 4.2.2 设计方案

遵循数据库三范式，我们这样设计《积分明细表》，包括数据库表三要素，

积分明细表 credit_transaction

<table><thead><tr><th>字段名称</th><th>英文名</th><th>数据类型</th><th>说明</th></tr></thead><tbody><tr><td>明细 ID</td><td>detail_id</td><td>BIGINT</td><td>主键，唯一标识积分交易记录</td></tr><tr><td>渠道 ID</td><td>channel_id</td><td>BIGINT</td><td>赚取或消费的渠道 ID（如订单、评论、签到等）</td></tr><tr><td>事件 ID</td><td>event_id</td><td>BIGINT</td><td>相关事件 ID（如订单 ID、评论 ID、优惠券换购交易 ID 等）</td></tr><tr><td>积分</td><td>credit</td><td>DECIMAL</td><td>积分数量，赚取为正值，消费为负值</td></tr><tr><td>创建时间</td><td>create_time</td><td>DATETIME</td><td>积分赚取或消费的时间</td></tr><tr><td>过期时间</td><td>expired_time</td><td>DATETIME</td><td>积分过期时间</td></tr></tbody></table>

```
CREATE TABLE credit_transaction (
    detail_id BIGINT AUTO_INCREMENT PRIMARY KEY COMMENT '明细ID，唯一标识积分交易记录',
    channel_id BIGINT NOT NULL COMMENT '赚取或消费渠道ID',
    event_id BIGINT NOT NULL COMMENT '相关事件ID，比如订单ID、评论ID、优惠券换购交易ID',
    credit DECIMAL(10, 2) NOT NULL COMMENT '积分，赚取为正值，消费为负值',
    create_time DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '积分赚取或消费时间',
    expired_time DATETIME DEFAULT NULL COMMENT '积分过期时间',
    INDEX idx_channel_id (channel_id),
    INDEX idx_event_id (event_id),
    INDEX idx_create_time (create_time),
    INDEX idx_expired_time (expired_time)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='积分明细表';
```

### 4.3 接口设计

**设计原则**：

[设计模式——设计模式简介和七大原则 - CSDN 博客](https://blog.csdn.net/qq_40991313/article/details/130403757?spm=1001.2014.3001.5502 "设计模式——设计模式简介和七大原则-CSDN博客")

**方案**：单一职责 + 外观设计模式 

*   **单一职责**：一个类应该只负责一项职责。优点是复用性高，缺点是性能可能变差。因为本来一个接口能返回完整数据，需要拆分成多个接口，从而需要多次访问数据库，对性能造成影响。
*   **外观模式**：：也叫过程模式 / 门面模式，是一种结构型设计模式。外观模式通常创建一个外观类，将子系统类的多个流程方法组装起来。

为了兼顾易用性和性能，我们选择单一职责 + 外观设计模式，在职责单一的细粒度接口之上，再封装一层粗粒度的接口（ **外观类**）给外部使用。

**接口设计**：

<table><thead><tr><th>接口</th><th>参数</th><th>返回</th></tr></thead><tbody><tr><td>赚取积分</td><td><code onclick="mdcp.copyCode(event)">userId</code>, <code onclick="mdcp.copyCode(event)">channelId</code>, <code onclick="mdcp.copyCode(event)">eventId</code>, <code onclick="mdcp.copyCode(event)">credit</code>, <code onclick="mdcp.copyCode(event)">expiredTime</code></td><td>积分明细 ID</td></tr><tr><td>消费积分</td><td><code onclick="mdcp.copyCode(event)">userId</code>, <code onclick="mdcp.copyCode(event)">channelId</code>, <code onclick="mdcp.copyCode(event)">eventId</code>, <code onclick="mdcp.copyCode(event)">credit</code>, <code onclick="mdcp.copyCode(event)">expiredTime</code></td><td>积分明细 ID</td></tr><tr><td>查询总积分明细</td><td><code onclick="mdcp.copyCode(event)">userId</code></td><td>总可用积分</td></tr><tr><td>查询赚取积分明细</td><td><code onclick="mdcp.copyCode(event)">userId</code> + 分页参数</td><td><code onclick="mdcp.copyCode(event)">id</code>, <code onclick="mdcp.copyCode(event)">userId</code>, <code onclick="mdcp.copyCode(event)">channelId</code>, <code onclick="mdcp.copyCode(event)">eventId</code>, <code onclick="mdcp.copyCode(event)">credit</code>, <code onclick="mdcp.copyCode(event)">createTime</code>, <code onclick="mdcp.copyCode(event)">expiredTime</code></td></tr><tr><td>查询消费积分明细</td><td><code onclick="mdcp.copyCode(event)">userId</code> + 分页参数</td><td><code onclick="mdcp.copyCode(event)">id</code>, <code onclick="mdcp.copyCode(event)">userId</code>, <code onclick="mdcp.copyCode(event)">channelId</code>, <code onclick="mdcp.copyCode(event)">eventId</code>, <code onclick="mdcp.copyCode(event)">credit</code>, <code onclick="mdcp.copyCode(event)">createTime</code>, <code onclick="mdcp.copyCode(event)">expiredTime</code></td></tr><tr><td>查询积分</td><td><code onclick="mdcp.copyCode(event)">userId</code> + 分页参数</td><td><code onclick="mdcp.copyCode(event)">id</code>, <code onclick="mdcp.copyCode(event)">userId</code>, <code onclick="mdcp.copyCode(event)">channelId</code>, <code onclick="mdcp.copyCode(event)">eventId</code>, <code onclick="mdcp.copyCode(event)">credit</code>, <code onclick="mdcp.copyCode(event)">createTime</code>, <code onclick="mdcp.copyCode(event)">expiredTime</code></td></tr></tbody></table>

### 4.4 业务模型设计

**方案**：

大部分业务模型都可以拆分成三层架构。 

*   **三层架构**：Controller 层负责接口暴露，Repository 层负责数据读写  
    Service 层负责核心业务逻辑。
*   **DDD 模式**：基于贫血模型的传统开发模式和基于充血模型的  
    DDD 开发模式。

**三层架构：** 

开发过程中，我们把后端服务器 Servlet 拆分成三层，分别是 web、service 和 dao，这也是程序员常提到的 **“Java 味”**：

*   **web 层（表现层）**：直接与用户交互，负责接收用户输入和呈现数据
*   **service 层（业务层）**：处理具体的业务逻辑。也称为服务层或应用层。
*   **dao 层（数据访问层）**：负责与数据库交互，执行数据的增删改查

![](<assets/1740031084134.png>)

​

**优点**：

*   **代码复用**：同一个 dao 可能被多个 service 调用，分层后能避免重复编写 dao 代码，从而避免违反 DRY 原则（Don't Repeat Yourself，即不要写重复的代码）。
*   **低耦合**：各层的职责明确，页面交互、业务逻辑、数据库操作三层分离，降低了系统模块间的耦合。例如 service 只需要关注业务，编写时直接调用 dao 层，而不需要知道它具体依赖的哪个数据库。
*   **易维护**：每层的功能独立，业务逻辑更改后只需修改 Service，数据库更改后只需修改 dao。
*   **可扩展**：当增加新功能后，可以在各层扩展相关功能的类。例如如果将所有逻辑都写在 controller 里，这会因为需求变化而导致这个方法里的代码越来越多，可读性和可维护性都变差，这就需要进行水平（即分模块，按业务拆分模块）和垂直拆分（即使用三层架构）。

## 五、部署方式

部署方式主要有以下两种，具体选择需要考虑服务器资源、并发量、代码量等实际场景：

*   **单体部署**：跟其他系统作为一整个单体项目，单体部署到服务器
*   **独立部署**：将积分系统或者大的营销系统作为一个微服务独立部署。更推荐后者，因为积分系统代码量不大。