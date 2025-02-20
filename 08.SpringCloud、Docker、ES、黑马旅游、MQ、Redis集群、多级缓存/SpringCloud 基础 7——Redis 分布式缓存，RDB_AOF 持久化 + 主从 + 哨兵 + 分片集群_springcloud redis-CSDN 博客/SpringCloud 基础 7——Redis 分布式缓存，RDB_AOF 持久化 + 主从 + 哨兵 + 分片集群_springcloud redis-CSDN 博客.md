---
url: https://blog.csdn.net/qq_40991313/article/details/126912298?spm=1001.2014.3001.5501
title: SpringCloud 基础 7——Redis 分布式缓存，RDB,AOF 持久化 + 主从 + 哨兵 + 分片集群_springcloud redis-CSDN 博客
date: 2025-02-20 09:59:30
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1.Redis 持久化](#t0)

[1.0.Redis 持久化的意义](#t1) 

[1.1. 数据备份文件 RDB 持久化方案](#t2)

[1.1.1. 执行时机](#t3)

[1.1.2.RDB 原理](#t4)

[1.1.3. 小结，bgsave 流程、执行时间、缺点](#t5)

[1.2. 追加文件 AOF 持久化方案](#t6)

[1.2.1.AOF 原理](#t7)

[1.2.2.AOF 配置](#t8)

[1.2.3.AOF 文件重写](#t9)

[1.3.RDB 与 AOF 对比](#t10)

[2.Redis 主从](#t11)

[2.0. 介绍](#t12)

[2.1. 搭建主从集群](#t13)

[2.1.1. 集群结构](#t14)

[2.1.2. 准备实例和配置](#t15)

[2.1.3. 启动](#t16)

[2.1.4. 开启主从关系](#t17)

[2.1.5. 测试读写分离](#t18)

[2.2. 主从数据同步原理](#t19)

[2.2.1. 全量同步](#t20)

[2.2.2. 增量同步](#t21)

[2.2.3.repl_backlog 原理](#t22)

[2.3. 主从同步优化](#t23)

[2.4. 小结，全量同步和增量同步区别、执行时间](#t24)

[3.Redis 哨兵](#t25)

[3.1. 哨兵原理](#t26)

[3.1.1. 集群结构和作用](#t27)

[3.1.2. 集群监控原理](#t28)

[3.1.3. 集群故障恢复原理，选举、切换新 master](#t29)

[3.1.4. 小结，Sentinel 健康、故障转移、通知](#t30)

[3.2. 搭建哨兵集群](#t31)

[3.2.1. 集群结构](#t32)

[3.2.2. 准备实例和配置](#t33)

[3.2.3. 启动 3 个 redis 实例](#t34)

[3.2.4. 测试](#t35)

[3.3.RedisTemplate](#t36)

[3.3.1. 导入 Demo 工程](#t37)

[3.3.2. 引入 redis 的 starter 依赖](#t38)

[3.3.3. 配置 Redis 地址](#t39)

[3.3.4. 配置读写分离](#t40)

[3.3.5. 测试读写分离、主从自动切换](#t41)

[4.Redis 分片集群](#t42)

[4.0. 分片集群概述](#t43)

[4.1. 搭建分片集群](#t44) 

[4.1.1. 集群结构](#t45)

[4.1.2. 准备实例和配置](#t46)

[4.1.3. 启动](#t47)

[4.1.4. 创建集群](#t48)

[4.1.5. 集群常用命令](#t49)

[4.1.6. 集群模式下连接节点必须加 “-c”](#t50)

[4.2. 散列插槽](#t51)

[4.2.1. 插槽原理](#t52)

[4.2.1. 小结，插槽流程、同类数据通过 {} 插槽绑定到同实例](#t53)

[4.3. 集群伸缩](#t54)

[4.3.0. 操作集群的命令](#t55) 

[4.3.1. 需求分析，添加节点到集群、分配插槽](#t56)

[4.3.2. 创建新的 redis 实例](#t57)

[4.3.3. 添加新节点到 redis](#t58)

[4.3.4. 转移插槽](#t59)

[4.4. 故障转移](#t60)

[4.4.1. 自动故障转移](#t61)

[4.4.2. 手动故障转移、指定 master 实例](#t62)

[4.5.RedisTemplate 访问分片集群](#t63)

## 1.Redis 持久化

### 1.0.Redis 持久化的意义 

redis 持久化的意义，在于**数据备份和故障恢复**。

比如你部署了一个 redis，作为 cache 缓存，当然也可以保存一些较为重要的数据。Redis 数据存在内容中，如果没有持久化的话，redis 遇到灾难性故障的时候，就会丢失所有的数据。如果通过**将数据持久化在磁盘上**，然后**定期同步和备份到一些云存储服务**上去，那么就可以保证数据不丢失全部，还是可以恢复一部分数据回来的。

![](<assets/1740016781555.png>)

**redis 持久化 + 备份：**一般将 redis 数据从内存存储到磁盘。然后将磁盘数据备份一份即将数据上传到云服务器 S3 或 ODPS 上即可。如果左边的 redis 进程坏了并且磁盘也坏了，此时可以在另一台服务器启动该 redis，然后将云服务器上的数据 copy 一份到磁盘上，redis 进程在启动过程中会从磁盘加载到内存中。

Redis 有**两种持久化方案：**

*   **RDB 持久化**
*   **AOF 持久化**

### 1.1. **数据备份文件** RDB 持久化方案

RDB 全称 **Redis Database Backup file（Redis 数据备份文件**，backup 译为备份，支援**）**，也被叫做 **Redis 数据快照**。简单来说就是把内存中的所有**数据都记录到磁盘中**。当 Redis 实例**故障重启**后，**从磁盘读取快照文件**，恢复数据。快照文件称为 RDB 文件，**默认是保存在当前运行目录**。

#### 1.1.1. 执行时机

RDB 持久化在四种情况下会执行：

*   执行 save 命令
*   执行 bgsave 命令
*   Redis 停机时
*   触发 RDB 条件时

**1）save 命令**

执行下面的命令，可以立即执行一次 RDB：

![](<assets/1740016781841.png>)

save 命令会导致**主进程执行 RDB**，这个过程中**其它所有命令都会被阻塞**。只有在数据迁移时可能用到。

**2）bgsave 命令**

下面的命令可以**异步**执行 RDB：

![](<assets/1740016782180.png>)

这个命令执行后会**开启独立进程完成 RDB**，**主进程**可以持续处理用户请求，**不受影响**。

**3）停机时**

Redis **停机时会执行一次 save 命令**，实现 RDB 持久化。

**4）触发 RDB 条件**

Redis **内部有触发 RDB 的机制**，可以在 redis.conf 文件中找到，格式如下：

```
cd /usr/local/redis-4.0.0
vim redis.conf
```

```
# 900秒内，如果至少有1个key被修改，则执行bgsave ， 如果是save "" 则表示禁用RDB
save 900 1 
save 300 10 
save 60 10000
```

redis 服务端自动 RDB： 

![](<assets/1740016782402.png>)

 **RDB 的其它配置**也可以在 redis.conf 文件中设置： 

```
# 是否压缩 ,建议不开启，压缩也会消耗cpu，磁盘的话不值钱
rdbcompression yes
 # RDB文件名称
dbfilename dump.rdb 
 # 文件保存的路径目录
dir ./
```

#### 1.1.2.RDB 原理

**bgsave 开始时会 fork 主进程得到子进程**，**子进程共享主进程的内存数据**。完成 **fork 后**读取内存数据并**写入 RDB 文件**。在 fork 时主进程是阻塞的。

**fork** 采用的是 **copy-on-write 技术：**

*   当主进程执行读操作时，访问共享内存。在 linux 中，主进程只能操作**虚拟内存**，不能操作物理内存，操作系统会维护**虚拟内存到物理内存的映射关系表（即页表）**。当进行写操作时，物理内存会拷贝**数据副本**，主进程在数据副本实行读写。
*   当主进程执行写操作时，则会**拷贝一份页表到子进程**，实现了内存空间共享，根据页表的映射关系执行写操作。

![](<assets/1740016782669.png>)

#### 1.1.3. 小结，bgsave 流程、执行时间、缺点

**RDB 方式 bgsave 的基本流程？**

*   **fork 主进程得到一个子进程**，共享内存空间
*   子进程读取内存数据并**写入新的 RDB 文件**
*   用新 RDB 文件**替换旧的 RDB 文件**

**RDB 会在什么时候执行？save 60 1000 代表什么含义？**

*   默认是服务停止时
*   代表 60 秒内至少执行 1000 次修改则触发 RDB

**RDB 的缺点？**

*   RDB 执行**间隔时间长**，**两次 RDB 之间写入数据有丢失的风险**
*   fork 子进程、压缩、写出 RDB 文件都比较**耗时**

### 1.2. 追加文件 AOF 持久化方案

#### 1.2.1.AOF 原理

AOF 全称为 **Append Only File（追加文件）**。**Redis** 处理的**每一个写命令都会记录在 AOF 文件**，可以看做是**命令日志文件**。

![](<assets/1740016782964.png>)

#### 1.2.2.AOF 配置

AOF **默认是关闭的**，需要修改 redis.conf 配置文件来开启 AOF：

```
# 是否开启AOF功能，默认是no
appendonly yes
# AOF文件的名称
appendfilename "appendonly.aof"
```

AOF 的命令记录的**频率**也可以通过 redis.conf 文件来配：

```
# 表示每执行一次写命令，立即记录到AOF文件。牺牲性能绝对保证数据安全性
#appendfsync always 
# 默认。写命令执行完先放入AOF缓冲区，然后表示每隔1秒将缓冲区数据写到AOF文件，是默认方案。内存方式读写，对性能有帮助，最多会丢失一秒钟内数据
appendfsync everysec 
# 写命令执行完先放入AOF缓冲区，由操作系统决定何时将缓冲区内容写回磁盘。安全性最差，还不如RDB用快照文件存读数据安全。
#appendfsync no
```

**三种策略对比：**

![](<assets/1740016783283.png>)

**实现 AOF：**

redis.conf 先禁用 RDB、删除 RDB 文件：

![](<assets/1740016783612.png>)

 

![](<assets/1740016783839.png>)

 配置 AOF：

![](<assets/1740016784065.png>)

![](<assets/1740016784273.png>)

 存数据：

![](<assets/1740016784513.png>)

出现了 aof 文件 

![](<assets/1740016784706.png>)

aof 是记录所有命令、rdb 是记录各记录的值，所以 **aof 文件会比 rdb 文件大很多**。 

#### 1.2.3.AOF 文件重写

因为是**记录命令**，**AOF 文件会比 RDB 文件大的多**。而且 AOF 会记录**对同一个 key 的多次写操作**，但**只有最后一次写操作才有意义**。通过**执行 bgrewriteaof 命令**，可以**让 AOF 文件执行重写功能**，用最少的命令达到相同效果。

![](<assets/1740016784948.png>)

 如图，AOF 原本有三个命令，但是`set num 123 和 set num 666`都是对 num 的操作，第二次会覆盖第一次的值，因此第一个命令记录下来没有意义。

所以**重写命令**后，AOF 文件内容就是：**`mset name jack num 666`**

**Redis** 也会在触发**阈值**时**自动去重写 AOF 文件**。阈值也可以在 redis.conf 中配置：

```
# AOF文件比上次文件 增长超过多少百分比则触发重写
auto-aof-rewrite-percentage 100
# AOF文件体积最小多大以上才触发重写 
auto-aof-rewrite-min-size 64mb
```

### 1.3.RDB 与 AOF 对比

RDB 和 AOF 各有自己的优缺点，如果对数据安全性要求较高，在实际开发中往往会**结合**两者来使用。

![](<assets/1740016785222.png>)

数据恢复优先级是两个方案同时用会优先使用 aof 文件恢复。 

## 2.Redis 主从

### 2.0. 介绍

**主从的好处：** 

1.  **数据备份**，主从复制实现了数据的热备，是除了持久化机制之外的另外一种数据备份方式。
2.  **读写分离**，使数据库能支撑更大的并发。在报表中尤其重要。由于部分报表 sql 语句非常的慢，导致锁表，影响前台服务。如果前台使用 master，报表使用 slave，那么报表 sql 将不会造成前台锁，保证了前台速度。
3.  **负载均衡**，在主从复制的基础上，配合读写分离机制，可以由主节点提供写服务，从节点提供服务。在读多写少的场景中，可以增加从节点来分担 redis-server 读操作的负载能力，从而大大提高 redis-server 的并发量
4.  **保证高可用**，作为后备数据库，如果主节点出现故障后，可以切换到从节点继续工作，保证 redisserver 的高可用。

### 2.1. 搭建主从集群

#### 2.1.1. 集群结构

我们搭建的主从集群结构如图：

![](<assets/1740016785488.png>)

共包含三个节点，一个主节点，两个从节点。

这里我们会在同一台虚拟机中开启 3 个 redis 实例，模拟主从集群，信息如下：

<table border="1" cellspacing="0"><thead><tr><th><span><span>IP</span></span></th><th><span><span>PORT</span></span></th><th><span><span>角色</span></span></th></tr></thead><tbody><tr><td><span><span><span>192.168.150.101</span></span></span></td><td><span><span><span>7001</span></span></span></td><td><span><span><span>master</span></span></span></td></tr><tr><td><span><span><span>192.168.150.101</span></span></span></td><td><span><span><span>7002</span></span></span></td><td><span><span><span>slave</span></span></span></td></tr><tr><td><span><span><span>192.168.150.101</span></span></span></td><td><span><span><span>7003</span></span></span></td><td><span><span><span>slave</span></span></span></td></tr></tbody></table>

#### 2.1.2. 准备实例和配置

要在同一台虚拟机开启 3 个实例，必须准备三份不同的配置文件和目录，配置文件所在目录也就是工作目录。

**1）创建目录**

我们在 **Linux 的 tmp 目录下创建三个文件夹**，名字分别叫 **7001、7002、7003：**

```
# 进入/tmp目录
cd /tmp
# 创建目录
mkdir 7001 7002 7003
```

如图：

![](<assets/1740016785761.png>)

**2）恢复原始配置**

修改 redis-6.2.4/**redis.conf 文件**，将其中的持久化模式改为**默认的 RDB 模式**，**AOF 保持关闭状态**。

```
# 开启RDB
# save ""
save 3600 1
save 300 100
save 60 10000
 # 关闭AOF
appendonly no
```

**3）拷贝配置文件到每个实例目录**

然后将 redis-6.2.4/redis.conf 文件拷贝到三个目录中（在 / tmp 目录执行下列命令）：

```
# 方式一：逐个拷贝
cp redis-6.2.4/redis.conf 7001
cp redis-6.2.4/redis.conf 7002
cp redis-6.2.4/redis.conf 7003
# 方式二：管道组合命令，一键拷贝
#echo 7001 7002 7003 | xargs -t -n 1 cp redis-6.2.4/redis.conf
```

**4）修改每个实例的端口、工作目录**

修改每个文件夹内的配置文件，将**端口分别修改**为 7001、7002、7003，将 **rdb 文件保存位置**都修改为自己所在目录（在 / tmp 目录执行下列命令）：

```
sed -i -e 's/6379/7001/g' -e 's/dir .\//dir \/tmp\/7001\//g' 7001/redis.conf
sed -i -e 's/6379/7002/g' -e 's/dir .\//dir \/tmp\/7002\//g' 7002/redis.conf
sed -i -e 's/6379/7003/g' -e 's/dir .\//dir \/tmp\/7003\//g' 7003/redis.conf
```

**sed** 是快捷的对文档操作的命令。 

**5）修改每个实例的声明 IP**

虚拟机本身有多个 IP，为了避免将来混乱，我们需要在 redis.conf 文件中指定每一个实例的绑定 ip 信息，格式如下：

```
# redis实例的声明 IP
replica-announce-ip 192.168.150.101
```

每个目录都要改，我们一键完成修改（在 / tmp 目录执行下列命令）：

```
# 逐一执行
sed -i '1a replica-announce-ip 192.168.150.101' 7001/redis.conf
sed -i '1a replica-announce-ip 192.168.150.101' 7002/redis.conf
sed -i '1a replica-announce-ip 192.168.150.101' 7003/redis.conf
 
# 或者一键修改
#printf '%s\n' 7001 7002 7003 | xargs -I{} -t sed -i '1a replica-announce-ip 192.168.150.101' {}/redis.conf
```

#### 2.1.3. 启动

为了方便查看日志，我们打开 3 个 ssh 窗口，分别启动 3 个 redis 实例，启动命令：

```
# 第1个
redis-server 7001/redis.conf
# 第2个
redis-server 7002/redis.conf
# 第3个
redis-server 7003/redis.conf
```

启动后：

![](<assets/1740016785967.png>)

如果要一键停止，可以运行下面命令：

```
# 连接 7002
redis-cli -p 7002
# 执行slaveof，将7002的主机设为7001
slaveof 192.168.150.101 7001
```

#### 2.1.4. 开启主从关系

现在三个实例还没有任何关系，要配置主从可以使用 **replicaof 或者 slaveof（5.0 以前）命令**。

**有临时和永久两种模式：**

*   修改配置文件（永久生效）
    
    *   在 redis.conf 中添加一行配置：`slaveof <masterip> <masterport>`
*   使用 redis-cli 客户端连接到 redis 服务，执行 slaveof 命令（重启后失效）：
    
    ```
    # 连接 7003
    redis-cli -p 7003
    # 执行slaveof
    slaveof 192.168.150.101 7001
    ```
    

**注意：**在 5.0 以后新增命令 replicaof，与 salveof 效果一致。

这里我们为了演示方便，使用**方式二命令行**。

通过 redis-cli 命令连接 7002，执行下面命令：

```
# 连接 7001
redis-cli -p 7001
# 查看状态
info replication
```

通过 redis-cli 命令连接 7003，执行下面命令：

```
# 进入/tmp目录
cd /tmp
# 创建目录
mkdir s1 s2 s3
```

然后连接 7001 节点，查看集群状态：

```
port 27001
sentinel announce-ip 192.168.150.101
#mymaster：主节点名称，自定义，任意写
#192.168.150.101 7001：主节点的ip和端口
#2：选举master时的quorum值，也就是3台实例超过两台被主观下线则整个redis集群客观下线。
sentinel monitor mymaster 192.168.150.101 7001 2
#Sentinel 在多长时间内没有从主服务器（mymaster）中收到响应时，将该主服务器视为不可用。
sentinel down-after-milliseconds mymaster 5000
#指定 Sentinel 在多长时间后将尝试执行故障转移以恢复故障
sentinel failover-timeout mymaster 60000
dir "/tmp/s1"
```

**结果：查看 7001 的从库：**

![](<assets/1740016786172.png>)

#### 2.1.5. 测试读写分离

执行下列操作以测试：

*   利用 redis-cli 连接**主库 7001**，执行`set num 123`
    
*   利用 redis-cli 连接 7002，执行`get num`，再执行`set num 666`
    
*   利用 redis-cli 连接 7003，执行`get num`，再执行`set num 888。发现只能读不能写。`
    

![](<assets/1740016786431.png>)

可以发现，只有在 7001 这个 master 节点上可以执行写操作，7002 和 7003 这两个 slave 节点只能执行读操作。

### 2.2. 主从数据同步原理

#### 2.2.1. 全量同步

主从**第一次建立连接时**，会执行**全量同步**，将 master 节点的所有数据都拷贝给 slave 节点，流程：

![](<assets/1740016786803.png>)

repl_backlog 译为复制积压文件，底层是双向链表，存储发送 rdb 文件和加载 rdb 文件期间的所有命令。确保主从库永远一致。

这里有一个问题，**master 如何得知 salve 是第一次来连接呢？**

有几个概念，可以作为判断依据：

*   **Replication Id**：简称 **replid**，是**数据集的标记**，id 一致则说明是同一数据集。**每一个 master 都有唯一的 replid**，slave 在**第一次同步**时两者 replid 不同，要做**全量同步**，**从库会继承 master 节点的 replid**；以后同步两者 replid 相同，是同一数据集，做增量同步。replication 译为复制、重复。
*   **offset**：**偏移量**，**随着**记录在 **repl_backlog 中的数据增多而逐渐增大**。slave 完成同步时也会记录当前同步的 offset**。如果 slave 的 offset 小于 master 的 offset**，说明 slave **数据落后**于 master，**需要更新**。

因此 slave 做数据同步，必须向 master 声明自己的 replication id 和 offset，master 才可以判断到底需要同步哪些数据。

因为 slave 原本也是一个 master，有自己的 replid 和 offset，当第一次变成 slave，与 master 建立连接时，发送的 replid 和 offset 是自己的 replid 和 offset。

master 判断发现 slave 发送来的 replid 与自己的不一致，说明这是一个全新的 slave，就知道要做全量同步了。

master 会将自己的 replid 和 offset 都发送给这个 slave，slave 保存这些信息。以后 slave 的 replid 就与 master 一致了。

因此，**master 判断一个节点是否是第一次同步的依据，就是看 replid 是否一致**。

如图：

![](<assets/1740016786989.png>)

**完整流程描述：**

*   slave 节点请求增量同步
*   master 节点判断 replid，发现不一致，拒绝增量同步
*   master 将完整内存数据生成 RDB，发送 RDB 到 slave
*   slave 清空本地数据，加载 master 的 RDB
*   master 将 RDB 期间的命令记录在 repl_backlog，并持续将 log 中的命令发送给 slave
*   slave 执行接收到的命令，保持与 master 之间的同步

#### 2.2.2. 增量同步

全量同步需要先做 RDB，然后将 RDB 文件通过网络传输个 slave，成本太高了。因此除了第一次做全量同步，其它大多数时候 slave 与 master 都是做**增量同步**。

什么是增量同步？就是**根据偏移量 offset 只更新 slave 与 master 存在差异的部分数据**。如图：

![](<assets/1740016787260.png>)

那么 master 怎么知道 slave 与自己的数据差异在哪里呢?

#### 2.2.3.repl_backlog 原理

master 怎么知道 slave 与自己的数据差异在哪里呢?

这就要说到全量同步时的 repl_backlog 文件了。

这个文件是一个循环链表，也就是说**角标到达链表末尾后，会再次从 0 开始读写**，这样数组头部的数据就会被覆盖。

repl_backlog 中会记录 Redis 处理过的命令日志及 offset，包括 master 当前的 offset，和 slave 已经拷贝到的 offset：

![](<assets/1740016787481.png>)

slave 与 master 的 offset 之间的差异，就是 salve 需要增量拷贝的数据了。

随着不断有数据写入，master 的 offset 逐渐变大，slave 也不断的拷贝，追赶 master 的 offset：

![](<assets/1740016787690.png>)

直到链表被填满：

![](<assets/1740016788081.png>)

此时，如果有新的数据写入，就会覆盖链表中的旧数据。不过，旧的数据只要是绿色的，说明是已经被同步到 slave 的数据，即便被覆盖了也没什么影响。因为未同步的仅仅是红色部分。

但是，如果 slave 出现网络阻塞，导致 master 的 offset 远远超过了 slave 的 offset：

![](<assets/1740016788317.png>)

如果 master 继续写入新数据，其 offset 就会覆盖旧的数据，直到将 slave 现在的 offset 也覆盖：

![](<assets/1740016788498.png>)

棕色框中的红色部分，就是尚未同步，但是却已经被覆盖的数据。此时如果 slave 恢复，需要同步，却发现自己的 offset 都没有了，无法完成增量同步了。只能做全量同步。

![](<assets/1740016788723.png>)

### 2.3. 主从同步优化

主从同步可以保证主从数据的一致性，非常重要。

可以从以下几个方面来**优化 Redis 主从就集群：**

*   在 master 中配置 repl-diskless-sync yes 启用**无磁盘复制**，**避免全量同步时的磁盘 IO。**
*   Redis **单节点上的内存占用不要太大**，减少 RDB 导致的过多磁盘 IO
*   适当**提高 repl_backlog 的大小**，发现 **slave** 宕机时**尽快实现故障恢复**，尽可能避免全量同步
*   限制一个 master 上的 slave 节点数量，如果实在是太多 slave，则可以采用**主 - 从 - 从链式结构**，减少 master 压力

**主从从架构图：**

![](<assets/1740016788989.png>)

### 2.4. 小结，全量同步和增量同步区别、执行时间

**简述全量同步和增量同步区别？**

*   **全量同步：**master 将完整内存数据生成 **RDB**，发送 RDB 到 slave。后续命令则记录在 **repl_backlog**，逐个发送给 slave。
*   **增量同步：**slave 提交自己的 **offset** 到 master，master 获取 **repl_backlog** 中从 **offset** 之后的命令给 slave

**什么时候执行全量同步？**

*   slave 节点**第一次连接** master 节点时
*   **slave 节点断开时间太久，repl_backlog 中的 offset 已经被覆盖时**

**什么时候执行增量同步？**

*   slave 节点断开又恢复，并且在 repl_backlog 中**能找到 offset 时**

## 3.Redis 哨兵

Redis 提供了哨兵（Sentinel）机制来实现主从集群的自动故障恢复。

### 3.1. 哨兵原理

#### 3.1.1. 集群结构和作用

哨兵的结构如图：

![](<assets/1740016789327.png>)

**哨兵的作用如下：**

*   **监控**：Sentinel 会不断**检查**您的 master 和 slave **是否按预期工作**
*   **自动故障恢复**：如果 **master 故障**，Sentinel 会**将一个 slave 提升为 master**。当故障实例恢复后也以新的 master 为主
*   **通知**：Sentinel 充当 Redis 客户端的服务发现来源，当**集群发生故障转移时**，会将**最新信息推送给 Redis 的客户端**

#### 3.1.2. 集群监控原理

**Sentinel** 基于**心跳机制监测服务状态**，**每隔 1 秒**向集群的每个实例**发送 ping 命令**：

**• 主观下线：**如果某 sentinel 节点发现某实例**未在规定时间响应**，则认为该实例**主观下线**。

**• 客观下线：**若**超过指定数量** quorum 值（quorum 译为法定人数、多数派）**的 sentinel** 都**认为**该实例**主观下线**，则该实例**客观下线**。quorum 值最好超过 Sentinel 实例数量的一半。

![](<assets/1740016789661.png>)

#### 3.1.3. 集群故障恢复原理，选举、切换新 master

**1. 选举新 master 的依据：** 

一旦发现 master 故障，**sentinel 需要在 salve 中选择一个作为新的 master**，选择依据是这样的：

*   首先会判断 **slave 节点**与 master 节点**断开时间**长短，如果**超过指定值**（down-after-milliseconds * 10）则会**排除该 slave 节点**
*   然后判断 slave 节点的 **slave-priority 值，越小优先级越高**，如果是 0 则永不参与选举
*   如果 slave-prority 一样，则判断 slave 节点的 **offset 值，越大说明数据越新，优先级越高**
*   最后是判断 slave 节点的**运行 id** 大小（这个运行 id 就很随机了，是 Redis 启动时自动生成的 id），**越小优先级越高。**

**2. 切换新 master 的方法：** 

*   **sentinel** 给备选的 **slave1 节点发送 slaveof no one 命令**，让该节点**成为 master**
*   **sentinel 给所有其它 slave 发送 slaveof 192.168.150.101 7002 命令**，让这些 slave 成为新 master 的从节点，开始从新的 master 上同步数据。
*   最后，sentinel 将**故障节点标记为 slave**，当故障节点恢复后会自动成为新的 master 的 slave 节点

![](<assets/1740016789974.png>)

#### 3.1.4. 小结，Sentinel 健康、故障转移、通知

**Sentinel 对 redis 集群的三个作用是什么？**

*   监控
*   故障转移
*   通知

 **Sentinel 其他作用：**

[微服务保护、流量控制、隔离和降级、授权规则、规则持久化](https://blog.csdn.net/qq_40991313/article/details/126882045 "微服务保护、流量控制、隔离和降级、授权规则、规则持久化")

**Sentinel 如何判断一个 redis 实例是否健康？**

*   **每隔 1 秒发送一次 ping 命令**，如果超过一定时间没有相向则认为是主观下线
*   如果**大多数 sentinel 都认为该实例主观下线**，则判定该实例客观下线
    

**故障转移步骤有哪些？**

*   首先选定一个 slave 作为**新的 master**，执行 **slaveof no one**
*   然后让**所有节点**都执行 **slaveof 新 master**
*   修改**故障的旧主节点**配置，添加 **slaveof 新 master**

### 3.2. 搭建哨兵集群

#### 3.2.1. 集群结构

这里我们**搭建一个三节点形成的 Sentinel 集群**，来**监管**之前的 **Redis 主从集群**。如图：

![](<assets/1740016790169.png>)

三个 sentinel 实例信息如下：

<table><thead><tr><th>节点</th><th>IP</th><th>PORT</th></tr></thead><tbody><tr><td>s1</td><td>192.168.150.101</td><td>27001</td></tr><tr><td>s2</td><td>192.168.150.101</td><td>27002</td></tr><tr><td>s3</td><td>192.168.150.101</td><td>27003</td></tr></tbody></table>

#### 3.2.2. 准备实例和配置

要在**同一台虚拟机开启 3 个实例**，必须准备**三份不同的配置文件和目录**，配置文件所在目录也就是工作目录。

我们创建三个文件夹，名字分别叫 s1、s2、s3：

```
# 方式一：逐个拷贝
cp s1/sentinel.conf s2
cp s1/sentinel.conf s3
# 方式二：管道组合命令，一键拷贝
echo s2 s3 | xargs -t -n 1 cp s1/sentinel.conf
```

如图：

![](<assets/1740016790378.png>)

然后我们在 **s1 目录创建一个 sentinel.conf 文件**，添加下面的内容：

```
sed -i -e 's/27001/27002/g' -e 's/s1/s2/g' s2/sentinel.conf
sed -i -e 's/27001/27003/g' -e 's/s1/s3/g' s3/sentinel.conf
```

**解读：**

*   `port 27001`：是当前 sentinel 实例的端口
*   `sentinel monitor mymaster 192.168.150.101 7001 2`：指定主节点信息，**只指定主节点信息 sentinel 就能监控到整个集群**
    *   `mymaster`：主节点名称，自定义，任意写
    *   `192.168.150.101 7001`：主节点的 ip 和端口
    *   **`2`：**选举 master 时的 **quorum 值**，也就是 3 台实例超过两台被主观下线则整个 redis 集群客观下线。

然后将 **s1/sentinel.conf 文件拷贝到 s2、s3 两个目录中**（在 **/tmp 目录**执行下列命令）：

```
# 第1个
redis-sentinel s1/sentinel.conf
# 第2个
redis-sentinel s2/sentinel.conf
# 第3个
redis-sentinel s3/sentinel.conf
```

**修改 s2、s3** 两个文件夹内的**配置文件**，**将端口分别修改为 27002、27003：**

```
@RestController
public class HelloController {
 
    @Autowired
    private StringRedisTemplate redisTemplate;
 
    @GetMapping("/get/{key}")
    public String hi(@PathVariable String key) {
        return redisTemplate.opsForValue().get(key);
    }
 
    @GetMapping("/set/{key}/{value}")
    public String hi(@PathVariable String key, @PathVariable String value) {
        redisTemplate.opsForValue().set(key, value);
        return "success";
    }
}
```

#### 3.2.3. 启动 3 个 redis 实例

为了方便查看日志，我们打开 3 个 ssh 窗口，分别**启动 3 个 redis 实例**，启动命令：

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

**启动后：**

![](<assets/1740016790556.png>)

#### 3.2.4. 测试

尝试让 **master 节点 7001 宕机（ctrl+c）**，查看 sentinel 日志：认为主库主观下线

![](<assets/1740016790758.png>)

**查看 7003 的日志：**认为主库主观下线

![](<assets/1740016791075.png>)

 也认为主库主观下线，超过 2 个实例认为 7001 主观下线，于是 7001 客观下线。**7002 被选成 master、让其他实例认主**

![](<assets/1740016791349.png>)

超过 2 个 sentinel 认为主库 7001 主观下线了，那么认为 7001 客观下线，进行故障修复。

### 3.3.RedisTemplate

在 **Sentinel** 集群监管下的 **Redis 主从集群**，其**节点会因为自动故障转移而发生变化**，Redis 的客户端必须感知这种变化，及时更新连接信息。Spring 的 **RedisTemplate 底层利用 lettuce 实现了节点的感知和自动切换。**

下面，我们通过一个测试来**实现 RedisTemplate 集成哨兵机制。**

#### 3.3.1. 导入 Demo 工程

首先，我们引入课前资料提供的 Demo 工程：

![](<assets/1740016791597.png>)

![](<assets/1740016791927.png>)

```
spring:
  redis:
    sentinel:
      master: mymaster    #与sentinel里的sentinel.conf配置文件的主库名一致
      nodes:
        - 192.168.150.101:27001
        - 192.168.150.101:27002
        - 192.168.150.101:27003
```

#### 3.3.2. 引入 redis 的 starter 依赖

在项目的 pom 文件中引入依赖：

```
port 27001
sentinel announce-ip 192.168.150.101
sentinel monitor mymaster 192.168.150.101 7001 2    #主节点名称，自定义，任意写
sentinel down-after-milliseconds mymaster 5000
sentinel failover-timeout mymaster 60000
dir "/tmp/s1"
```

#### 3.3.3. 配置 Redis 地址

然后在配置文件 application.yml 中指定 **redis 下的 sentinel 相关信息**，因为主库 ip、端口、quorum 值信息在 sentinel 的 sentinel.conf 里指定过了，所以**只用配置 sentinel 的相关信息：**

```
@Bean
public LettuceClientConfigurationBuilderCustomizer clientConfigurationBuilderCustomizer(){
    return clientConfigurationBuilder -> clientConfigurationBuilder.readFrom(ReadFrom.REPLICA_PREFERRED);
}
```

我们以前在三个 sentinel 目录下都创建一个 sentinel.conf 文件，添加了下面的内容，指定了主库信息：

```
# 进入/tmp目录
cd /tmp
# 删除旧的，避免配置干扰
rm -rf 7001 7002 7003
# 创建目录
mkdir 7001 7002 7003 8001 8002 8003
```

#### 3.3.4. 配置读写分离

在项目的**启动类**中，添加一个**新的 bean：**

```
port 6379
# 开启集群功能
cluster-enabled yes
# 集群的配置文件名称，不需要我们创建，由redis自己维护
cluster-config-file /tmp/6379/nodes.conf
# 节点心跳失败的超时时间
cluster-node-timeout 5000
# 持久化文件存放目录
dir /tmp/6379
# 绑定地址
bind 0.0.0.0
# 让redis后台运行
daemonize yes
# 注册的实例ip
replica-announce-ip 192.168.150.101
# 保护模式
protected-mode no
# 数据库数量
databases 1
# 日志
logfile /tmp/6379/run.log
```

这个 bean 中配置的就是**读写策略**，包括四种：

*   MASTER：从主节点读取
*   MASTER_PREFERRED：优先从 master 节点读取，master 不可用才读取 replica
*   REPLICA：从 slave（replica）节点读取
*   **REPLICA _PREFERRED（推荐）：**优先从 slave（replica）节点读取，所有的 slave 都不可用才读取 master

#### 3.3.5. 测试读写分离、主从自动切换

**测试读：** 

![](<assets/1740016792241.png>)

看 idea 日志：

先获取 sentinel 连接

![](<assets/1740016792633.png>)

挑中 27001 

![](<assets/1740016792859.png>)

得到主库信息：

![](<assets/1740016793124.png>)

 

得到从库信息：

![](<assets/1740016793446.png>)

 

连接主从节点：

![](<assets/1740016793667.png>)

 

可以看到查询请求给到了 7003：

![](<assets/1740016793979.png>)

 

**测试写：**

![](<assets/1740016794242.png>)

交给了 7002 主节点处理：

![](<assets/1740016794410.png>)

**测试故障修复：**

宕机 7002

![](<assets/1740016794718.png>)

 

sentinel 日志显示切换主节点到 7001：

![](<assets/1740016795010.png>)

 

## 4.Redis 分片集群

### 4.0. 分片集群概述

**主从和哨兵**可以解决高可用、高并发读的问题。但是依然有**两个问题没有解决：**

*   **海量数据存储问题**
    
*   **高并发写的问题**
    

使用**分片集群可以解决上述问题**，如图:

![](<assets/1740016795273.png>)

**分片集群特征：**

*   **集群中有多个 master，每个 master 保存不同数据**
    
*   每个 master 都可以有多个 slave 节点
    
*   **master 之间通过 ping 监测彼此健康状态**
    
*   客户端请求可以访问集群任意节点，最终都会被转发到正确节点
    

### 4.1. 搭建分片集群 

#### 4.1.1. 集群结构

分片集群需要的节点数量较多，这里我们搭建一个最小的分片集群，包含 3 个 master 节点，每个 master 包含一个 slave 节点，结构如下：

![](<assets/1740016795560.png>)

这里我们会在同一台虚拟机中开启 6 个 redis 实例，模拟分片集群，信息如下：

<table><thead><tr><th>IP</th><th>PORT</th><th>角色</th></tr></thead><tbody><tr><td>192.168.150.101</td><td>7001</td><td>master</td></tr><tr><td>192.168.150.101</td><td>7002</td><td>master</td></tr><tr><td>192.168.150.101</td><td>7003</td><td>master</td></tr><tr><td>192.168.150.101</td><td>8001</td><td>slave</td></tr><tr><td>192.168.150.101</td><td>8002</td><td>slave</td></tr><tr><td>192.168.150.101</td><td>8003</td><td>slave</td></tr></tbody></table>

#### 4.1.2. 准备实例和配置

删除之前的 7001、7002、7003 这几个目录，**重新创建出 7001、7002、7003、8001、8002、8003 目录：**

```
# 进入/tmp目录
cd /tmp
# 执行拷贝
echo 7001 7002 7003 8001 8002 8003 | xargs -t -n 1 cp redis.conf
```

在 / tmp 下准备一个新的 redis.conf 文件，内容如下：

```
# 进入/tmp目录
cd /tmp
# 修改配置文件
printf '%s\n' 7001 7002 7003 8001 8002 8003 | xargs -I{} -t sed -i 's/6379/{}/g' {}/redis.conf
```

将这个文件拷贝到每个目录下：

```
# 进入/tmp目录
cd /tmp
# 一键启动所有服务
printf '%s\n' 7001 7002 7003 8001 8002 8003 | xargs -I{} -t redis-server {}/redis.conf
```

修改每个目录下的 redis.conf，将其中的 6379 修改为与所在目录一致：

```
# 安装依赖
yum -y install zlib ruby rubygems
gem install redis
```

#### 4.1.3. 启动

因为已经配置了后台启动模式，所以可以直接启动服务：

```
# 进入redis的src目录
cd /tmp/redis-6.2.4/src
# 创建集群
./redis-trib.rb create --replicas 1 192.168.150.101:7001 192.168.150.101:7002 192.168.150.101:7003 192.168.150.101:8001 192.168.150.101:8002 192.168.150.101:8003
```

通过 ps 查看状态：

```
redis-cli --cluster help
Cluster Manager Commands:
  create         host1:port1 ... hostN:portN   #创建集群
                 --cluster-replicas <arg>      #从节点个数
  check          host:port                     #检查集群
                 --cluster-search-multiple-owners #检查是否有槽同时被分配给了多个节点
  info           host:port                     #查看集群状态
  fix            host:port                     #修复集群
                 --cluster-search-multiple-owners #修复槽的重复分配问题
  reshard        host:port                     #指定集群的任意一节点进行迁移slot，重新分slots
                 --cluster-from <arg>          #需要从哪些源节点上迁移slot，可从多个源节点完成迁移，以逗号隔开，传递的是节点的node id，还可以直接传递--from all，这样源节点就是集群的所有节点，不传递该参数的话，则会在迁移过程中提示用户输入
                 --cluster-to <arg>            #slot需要迁移的目的节点的node id，目的节点只能填写一个，不传递该参数的话，则会在迁移过程中提示用户输入
                 --cluster-slots <arg>         #需要迁移的slot数量，不传递该参数的话，则会在迁移过程中提示用户输入。
                 --cluster-yes                 #指定迁移时的确认输入
                 --cluster-timeout <arg>       #设置migrate命令的超时时间
                 --cluster-pipeline <arg>      #定义cluster getkeysinslot命令一次取出的key数量，不传的话使用默认值为10
                 --cluster-replace             #是否直接replace到目标节点
  rebalance      host:port                                      #指定集群的任意一节点进行平衡集群节点slot数量 
                 --cluster-weight <node1=w1...nodeN=wN>         #指定集群节点的权重
                 --cluster-use-empty-masters                    #设置可以让没有分配slot的主节点参与，默认不允许
                 --cluster-timeout <arg>                        #设置migrate命令的超时时间
                 --cluster-simulate                             #模拟rebalance操作，不会真正执行迁移操作
                 --cluster-pipeline <arg>                       #定义cluster getkeysinslot命令一次取出的key数量，默认值为10
                 --cluster-threshold <arg>                      #迁移的slot阈值超过threshold，执行rebalance操作
                 --cluster-replace                              #是否直接replace到目标节点
  add-node       new_host:new_port existing_host:existing_port  #添加节点，把新节点加入到指定的集群，默认添加主节点
                 --cluster-slave                                #新节点作为从节点，默认随机一个主节点
                 --cluster-master-id <arg>                      #给新节点指定主节点
  del-node       host:port node_id                              #删除给定的一个节点，成功后关闭该节点服务
  call           host:port command arg arg .. arg               #在集群的所有节点执行相关命令
  set-timeout    host:port milliseconds                         #设置cluster-node-timeout
  import         host:port                                      #将外部redis数据导入集群
                 --cluster-from <arg>                           #将指定实例的数据导入到集群
                 --cluster-copy                                 #migrate时指定copy
                 --cluster-replace                              #migrate时指定replace
  help           
 
For check, fix, reshard, del-node, set-timeout you can specify the host and port of any working node in the cluster.
```

发现服务都已经正常启动，此时互相之间还没有集群关系：

![](<assets/1740016795894.png>)

如果要关闭所有进程，可以执行命令：

```
# 连接
redis-cli -p 7001
# 存储数据
set num 123
# 读取数据
get num
# 再次存储
set a 1
```

或者（推荐这种方式）：

```
redis-cli --cluster add-node  192.168.150.101:7004 192.168.150.101:7001
#后面这个ip：端口是集群中任意已存在的实例地址
```

#### 4.1.4. 创建集群

虽然服务启动了，但是目前每个服务之间都是独立的，没有任何关联。

我们需要执行命令来创建集群，在 Redis5.0 之前创建集群比较麻烦，5.0 之后集群管理命令都集成到了 redis-cli 中。

**1）Redis5.0 之前**

Redis5.0 之前集群命令都是用 redis 安装包下的 src/redis-trib.rb 来实现的。因为 redis-trib.rb 是有 ruby 语言编写的所以需要安装 ruby 环境。

```
redis-cli --cluster add-node  192.168.150.101:7004 192.168.150.101:7001
#后面这个ip：端口是集群中任意已存在的实例地址
```

然后通过命令来管理集群：

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

**2）Redis5.0 以后**

我们使用的是 Redis6.2.4 版本，集群管理以及集成到了 redis-cli 中，格式如下：

```
spring:
  redis:
    cluster:
      nodes:
        - 192.168.150.101:7001
        - 192.168.150.101:7002
        - 192.168.150.101:7003
        - 192.168.150.101:8001
        - 192.168.150.101:8002
        - 192.168.150.101:8003
```

**命令说明：**

*   `redis-cli --cluster`或者`./redis-trib.rb`：代表集群操作命令
*   `create`：代表是创建集群
*   `--replicas 1`或者`--cluster-replicas 1` ：指定集群中每个 master 的副本个数为 1，此时`节点总数 ÷ (replicas + 1)` 得到的就是 master 的数量。因此节点列表中的前 n 个就是 master，其它节点都是 slave 节点，随机分配到不同 master

运行后的样子：

![](<assets/1740016796192.png>)

这里输入 yes，则集群开始创建：

![](<assets/1740016796467.png>)

通过命令可以查看集群状态：

```
spring:
  redis:
    sentinel:
      master: mymaster    #与sentinel里的sentinel.conf配置文件的主库名一致
      nodes:
        - 192.168.150.101:27001
        - 192.168.150.101:27002
        - 192.168.150.101:27003
```

![](<assets/1740016796767.png>)

#### 4.1.5. 集群常用命令

redis-cli --cluster 命令：

*   **redis-cli --cluster create：**创建集群
*   **info：**查看集群状态
*   **fix：**修复集群
*   **reshard：**迁移插槽
*   **add-node：**添加新节点
*   **del-node：**删除指定节点
*   **import：**导入数据到集群

```
@Bean
public LettuceClientConfigurationBuilderCustomizer clientConfigurationBuilderCustomizer(){
    return clientConfigurationBuilder -> clientConfigurationBuilder.readFrom(ReadFrom.REPLICA_PREFERRED);
}
```

#### 4.1.6. 集群模式下连接节点必须加 “-c”

集群模式下，连接节点时必须加 “-c” 。例如

```
redis-cli -c -p 7001
```

尝试连接 7001 节点，存储一个数据：

```
# 连接
redis-cli -p 7001
# 存储数据
set num 123
# 读取数据
get num
# 再次存储
set a 1
```

结果悲剧了：

![](<assets/1740016796958.png>)

集群操作时，需要给`redis-cli`加上`-c`参数才可以：

```
redis-cli -c -p 7001
```

这次可以了：

![](<assets/1740016797219.png>)

### 4.2. 散列插槽

#### 4.2.1. 插槽原理

**Redis 会把每一个 master 节点映射到 0~16383 共 16384 个插槽（hash slot）**上，查看集群信息时就能看到：

![](<assets/1740016797426.png>)

**数据 key 不是与节点绑定，而是与插槽绑定。**redis 会根据 **key 的有效部分计算插槽值**，分两种情况：

*   **key 中包含 "{}"，且 “{}” 中至少包含 1 个字符，“{}”中的部分是有效部分**
*   **key 中不包含 “{}”**，**整个 key 都是有效部分**

例如：key 是 num，那么就根据 num 计算，如果是 {itcast}num，则根据 itcast 计算。计算方式是利用 CRC16 **算法得到一个 hash 值**，然后**对 16384 取余**，得到的结果就是 slot 值。

![](<assets/1740016797687.png>)

**注意，集群下打开 redis 客户端要加 - c** 

如图，在 7001 这个节点执行 **set** a 1 时，对 a 做 **hash 运算**，对 16384 **取余**，得到的结果是 15495，因此要**存储**到 103 节点。

到了 **7003 节点**后，执行`**get** num`时，对 num 做 **hash 运算**，对 16384 **取余**，得到的结果是 2765，因此需要**切换到 7001 节点**

#### 4.2.1. 小结，插槽流程、同类数据通过 {} 插槽绑定到同实例

**Redis 如何判断某个 key 应该在哪个实例？**

*   将 16384 个**插槽分配**到不同的实例
*   根据 key 的有效部分**计算哈希值**，对 16384 取余
*   **余数作为插槽**，寻找插槽所在实例即可

**如何将同一类数据固定的保存在同一个 Redis 实例？**

*   这一类数据使用相同的有效部分，**例如 key 都以 {typeId} 为前缀，有效值一样的 key 会存储在同一个插槽。**

 **例如将 num 存到 a 所在插槽：**

![](<assets/1740016797935.png>)

### 4.3. 集群伸缩

#### 4.3.0. 操作集群的命令 

**redis-cli --cluster** 提供了很多**操作集群的命令**，可以通过下面方式查看：

![](<assets/1740016798210.png>)

比如，**添加节点的命令：**

![](<assets/1740016798471.png>)

```
redis-cli --cluster add-node  192.168.150.101:7004 192.168.150.101:7001
#后面这个ip：端口是集群中任意已存在的实例地址
```

**插槽命令：**

![](<assets/1740016798710.png>)

 

#### 4.3.1. 需求分析，添加节点到集群、分配插槽

**需求：向集群中添加一个新的 master 节点，并向其中存储 num = 10**

*   启动一个新的 redis 实例，端口为 **7004**
*   添加 7004 到之前的集群，并作为一个 master 节点
*   给 7004 节点**分配插槽**，**使得 num 这个 key 可以存储到 7004 实例（num 插槽算出来在 7001）**

这里需要两个新的功能：

*   **添加一个节点到集群中**
*   **将部分插槽分配到新插槽**

#### 4.3.2. 创建新的 redis 实例

创建一个文件夹：

```
mkdir 7004
```

拷贝配置文件：

```
cp redis.conf /7004
```

修改配置文件：

```
sed /s/6379/7004/g 7004/redis.conf
```

启动 7004

```
redis-server 7004/redis.conf
```

#### 4.3.3. 添加新节点到 redis

添加节点的语法如下：

![](<assets/1740016799031.png>)

**执行添加新节点命令：**

```
redis-cli --cluster add-node  192.168.150.101:7004 192.168.150.101:7001
#后面这个ip：端口是集群中任意已存在的实例地址
```

通过命令**查看集群状态：**

```
redis-cli -p 7001 cluster nodes
```

如图，7004 加入了集群，并且默认是一个 master 节点：

![](<assets/1740016799123.png>)

但是，可以看到 7004 节点的插槽数量为 0，因此没有任何数据可以存储到 7004 上

#### 4.3.4. 转移插槽

我们要将 num 存储到 7004 节点，因此需要**先看看 num 的插槽**是多少：

![](<assets/1740016799348.png>)

如上图所示，**num 的插槽为 2765.**

我们可以将 0~3000 的插槽从 7001 转移到 7004，命令格式如下：

![](<assets/1740016799605.png>)

reshard 译为重新切分。 

具体命令如下：

**重新切分 7001 插槽**

![](<assets/1740016799862.png>)

得到下面的反馈：

![](<assets/1740016800052.png>)

**询问要移动多少个插槽**，我们计划是 3000 个：

新的问题来了：

![](<assets/1740016800322.png>)

**哪个 node 来接收这些插槽？**

显然是 7004，那么 7004 节点的 id 是多少呢？

![](<assets/1740016800526.png>)

**复制这个 id，然后拷贝到刚才的控制台后：**

![](<assets/1740016800783.png>)

这里询问，你的插槽是从哪里移动过来的？

*   all：代表全部，也就是三个节点各转移一部分
*   具体的 id：目标节点的 id
*   done：没有了

这里我们要**从 7001 获取，因此填写 7001 的 id：**

![](<assets/1740016801093.png>)

填完后，**点击 done**，这样插槽转移就准备好了：

![](<assets/1740016801392.png>)

确认要转移吗？输入 **yes：**

然后，**通过命令查看结果：**

![](<assets/1740016801628.png>)

可以看到：

![](<assets/1740016801803.png>)

目的达成。

### 4.4. 故障转移

集群初识状态是这样的：

![](<assets/1740016802089.png>)

其中 7001、7002、7003 都是 master，我们计划让 7002 宕机。

#### 4.4.1. 自动故障转移

当集群中有一个 master 宕机会发生什么呢？

直接停止一个 redis 实例，例如 7002：

```
redis-cli -p 7002 shutdown
```

1）首先是该实例与其它实例失去连接

2）然后是疑似宕机：

![](<assets/1740016802331.png>)

3）最后是确定下线，自动提升一个 slave 为新的 master：

![](<assets/1740016802670.png>)

4）当 7002 再次启动，就会变为一个 slave 节点了：

![](<assets/1740016802915.png>)

#### 4.4.2. 手动故障转移、指定 master 实例

利用 **cluster failover 命令**可以**手动让集群中的某个 master 宕机**，切换到执行 cluster failover 命令的这个 slave 节点，实现无感知的数据迁移。其流程如下：

![](<assets/1740016803339.png>)

这种 failover 命令可以指定三种模式：

*   缺省：默认的流程，如图 1~6 歩
*   force：省略了对 offset 的一致性校验
*   takeover：直接执行第 5 歩，忽略数据一致性、忽略 master 状态和其它 master 的意见

**案例需求**：在 7002 这个 slave 节点执行**手动故障转移，重新夺回 master 地位**

步骤如下：

1）利用 redis-cli 连接 7002 这个节点

2）执行 cluster failover 命令

**如图：**

![](<assets/1740016803585.png>)

**效果：**

![](<assets/1740016803867.png>)

### 4.5.RedisTemplate 访问分片集群

RedisTemplate 底层同样基于 lettuce 实现了分片集群的支持，而使用的步骤与哨兵模式基本一致：

**1）引入 redis 的 starter 依赖**

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

**2）配置分片集群地址**

与哨兵模式相比，其中只有分片集群的配置方式略有差异，**配置分片集群地址如下：**

```
spring:
  redis:
    cluster:
      nodes:
        - 192.168.150.101:7001
        - 192.168.150.101:7002
        - 192.168.150.101:7003
        - 192.168.150.101:8001
        - 192.168.150.101:8002
        - 192.168.150.101:8003
```

 哨兵模式的配置：

```
spring:
  redis:
    sentinel:
      master: mymaster    #与sentinel里的sentinel.conf配置文件的主库名一致
      nodes:
        - 192.168.150.101:27001
        - 192.168.150.101:27002
        - 192.168.150.101:27003
```

**3）配置读写分离**

```
@Bean
public LettuceClientConfigurationBuilderCustomizer clientConfigurationBuilderCustomizer(){
    return clientConfigurationBuilder -> clientConfigurationBuilder.readFrom(ReadFrom.REPLICA_PREFERRED);
}
```