---
url: https://blog.csdn.net/qq_40991313/article/details/126795212?spm=1001.2014.3001.5501
title: SpringCloud 基础 3——Docker_springcloud docker-CSDN 博客
date: 2025-02-20 09:59:23
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1. 初识 Docker](#t0)

[1.1. 什么是 Docker](#t1)

[1.1.1. 应用部署的环境问题](#t2)

[1.1.2.Docker 解决依赖兼容问题](#t3)

[1.1.3.Docker 解决操作系统环境差异](#t4)

[1.1.4. 小结](#t5)

[1.2.Docker 和虚拟机的区别](#t6)

[1.3.Docker 架构](#t7)

[1.3.1. 镜像和容器](#t8)

[1.3.2.DockerHub](#t9)

[1.3.3.Docker 架构](#t10)

[1.3.4. 小结镜像、容器、结构、dockerhub](#t11)

[1.4.CentOS 安装 Docker](#t12)

[1.4.1. 卸载（可选）](#t13)

[1.4.2. 安装 docker](#t14)

[1.4.3. 启动 docker](#t15)

[1.4.4. 配置镜像加速](#t16)

[1.4.5 设置开机启动 docker](#t17)

[2.Docker 的基本操作](#t18)

[2.1. 镜像操作](#t19)

[2.1.1. 镜像名称](#t20)

[2.1.2. 镜像命令](#t21)

[2.1.3. 案例 1 - 拉取、查看镜像](#t22)

[2.1.4. 案例 2 - 保存、导入镜像](#t23)

[2.2. 容器操作](#t24)

[2.2.1. 容器相关命令](#t25)

[2.2.2. 案例 - 创建并运行容器，run](#t26)

[2.2.2.5. 查看容器状态、日志](#t27)

[2.2.3. 案例 - 进入容器，修改文件（不建议）](#t28)

[2.2.4. 停止、删除容器](#t29)

[2.2.5. 创建 redis 容器](#t30)

[2.2.5. 小结](#t31)

[2.3. 数据卷（容器数据管理）](#t32)

[2.3.1. 什么是数据卷](#t33)

[2.3.2. 数据卷操作命令](#t34)

[2.3.3. 创建和查看数据卷](#t35)

[2.3.4. 挂载数据卷、宿主机目录，-v](#t36)

[2.3.5. 案例 - 给 nginx 挂载数据卷](#t37)

[2.3.6.docker 安装 MySQL，挂载本地目录](#t38)

[2.3.7. 小结](#t39)

[3.Dockerfile 自定义镜像](#t40)

[3.1. 镜像结构](#t41)

[3.2.Dockerfile 语法](#t42)

[3.3. 使用 Dockerfile 构建 Java 项目的镜像](#t43)

[3.3.1. 基于 Ubuntu 构建 Java 项目（不推荐）](#t44)

[3.3.2. 基于 java8 构建 Java 项目（推荐）](#t45)

[3.4. 小结](#t46)

[4.Docker-Compose，部署分布式项目](#t47)

[4.1. 初识 DockerCompose](#t48)

[4.2. 安装 DockerCompose](#t49)

[4.2.1. 下载](#t50)

[4.2.2. 修改文件权限](#t51)

[4.2.3.Base 自动补全命令：](#t52)

[4.3. 部署微服务集群](#t53)

[4.3.1. 初始 compose、Dockerfile 介绍](#t54)

[4.3.2. 修改微服务配置，localhost 改服务名](#t55)

[4.3.3. 所有服务打包成 app.jar](#t56)

[4.3.4. 拷贝 jar 包到部署目录](#t57)

[4.3.5. 部署](#t58)

[5.Docker 镜像仓库](#t59)

[5.1. 搭建私有镜像仓库](#t60)

[5.1.1. 简化版镜像仓库](#t61)

[5.1.2. 带有图形化界面版本](#t62)

[5.2. 推送、拉取镜像](#t63)

## 1. 初识 Docker

### 1.1. 什么是 Docker

docker 译为容器。

**Docker 是一个**开源的**应用容器引擎**，让**开发者可以打包**他们的**应用以及依赖包到**一个可移植的**镜像**中，然后发布到任何流行的 [Linux](https://baike.baidu.com/item/Linux?fromModule=lemma_inlink "Linux") 或 [Windows](https://baike.baidu.com/item/Windows/165458?fromModule=lemma_inlink "Windows") 操作系统的机器上，也可以实现[虚拟化](https://baike.baidu.com/item/%E8%99%9A%E6%8B%9F%E5%8C%96/547949?fromModule=lemma_inlink "虚拟化")。容器是完全使用[沙箱](https://baike.baidu.com/item/%E6%B2%99%E7%AE%B1/393318?fromModule=lemma_inlink "沙箱")机制，相互之间不会有任何接口。 

微服务虽然具备各种各样的优势，但服务的拆分通用给部署带来了很大的麻烦。

*   **分布式系统中，依赖的组件非常多**，不同组件之间部署时往往会产生一些**冲突**。
*   在数百上千台服务中**重复部署，环境**不一定一致，会遇到各种问题

#### 1.1.1. 应用部署的环境问题

大型项目组件较多，运行环境也较为复杂，部署时会碰到一些问题：

*   依赖关系复杂，容易出现兼容性问题
    
*   开发、测试、生产环境有差异
    

![](<assets/1740016763748.png>)

例如一个项目中，部署时需要依赖于 node.js、Redis、RabbitMQ、MySQL 等，这些服务部署时所需要的函数库、依赖项各不相同，甚至会有冲突。给部署带来了极大的困难。

#### 1.1.2.Docker 解决依赖兼容问题

而 **Docker 巧妙的解决了这些问题**，Docker 是如何实现的呢？

Docker 为了**解决依赖的兼容问题**的，采用了两个手段：

*   **将应用的 Libs（函数库）、Deps（依赖）、配置与应用一起打包**
    
*   **将每个应用放到一个隔离容器去运行，避免互相干扰**
    

![](<assets/1740016764024.png>)

这样打包好的应用包中，既包含应用本身，也保护应用所需要的 Libs、Deps，无需再操作系统上安装这些，自然就不存在不同应用之间的兼容问题了。

虽然解决了不同应用的兼容问题，但是开发、测试等环境会存在差异，操作系统版本也会有差异，怎么解决这些问题呢？

#### 1.1.3.Docker 解决操作系统环境差异

要解决不同操作系统环境差异问题，必须先了解操作系统结构。以一个 Ubuntu 操作系统为例，结构如下：

![](<assets/1740016764337.png>)

结构包括：

*   **计算机硬件：**例如 CPU、内存、磁盘等
*   **系统内核：**所有 Linux 发行版的内核都是 Linux，例如 CentOS、Ubuntu、Fedora 等。内核可以与计算机硬件交互，对外提供**内核指令**，用于**操作计算机硬件**。
*   **系统应用：**操作系统本身提供的应用、函数库。这些函数库是对内核指令的封装，使用更加方便。

应用于计算机交互的流程如下：

1）应用调用**操作系统应用**（函数库），实现各种功能

**2）系统函数库**是对内核指令集的封装，会调用内核指令

3）**内核指令**操作计算机硬件

**Ubuntu 和 CentOSpringBoot** 都是基于 Linux 内核，无非是**系统应用不同，提供的函数库有差异：**

![](<assets/1740016764611.png>)

此时，**如果**将一个 **Ubuntu 版本的 MySQL** 应用**安装到 CentOS 系统**，MySQL 在调用 Ubuntu 函数库时，会发现找不到或者不匹配，就会**报错**了：

![](<assets/1740016764848.png>)

**Docker 如何解决不同系统环境的问题？**

Docker 将**用户程序与**所需要调用的**系统** (比如 Ubuntu) **函数库一起打包**，这样**每个程序调用自己打包好的函数库**就不会出现环境问题。Docker 运行到不同操作系统时，直接基于打包的函数库，借助于操作系统的 Linux 内核来运行。

因此，**docker 打包好的程序包，可以运行在任何 Linux 内核的操作系统上。**

如图：

![](<assets/1740016765257.png>)

#### 1.1.4. 小结

Docker 如何解决大型项目依赖关系复杂，不同组件依赖的兼容性问题？

*   Docker 允许开发中将应用、依赖、函数库、配置一起**打包**，形成可移植镜像
*   Docker 应用运行在容器中，使用沙箱机制，相互**隔离**

Docker 如何解决开发、测试、生产环境有差异的问题？

*   **Docker 镜像中包含完整运行环境，包括系统函数库**，仅依赖系统的 Linux 内核，因此可以在任意 Linux 操作系统上运行

Docker 是一个快速交付应用、运行应用的技术，具备下列**优势：**

*   可以将程序及其依赖、运行环境一起**打包为一个镜像**，可以**迁移**到**任意 Linux** 操作系统
*   运行时利用**沙箱机制形成隔离容器**，各个应用互不干扰
*   启动、移除都可以通过一行命令完成，方便快捷

### 1.2.Docker 和虚拟机的区别

Docker 可以让一个应用在任何操作系统中非常方便的运行。而以前我们接触的**虚拟机**，**也能在一个操作系统中，运行另外一个操作系统**，保护系统中的任何应用。

两者有什么**差异**呢？

*   **虚拟机**（virtual machine）是在操作系统中**模拟硬件设备**，然后**运行另一个操作系统**，比如在 Windows 系统里面运行 Ubuntu 系统，这样就可以运行任意的 Ubuntu 应用了。
*   **Docker 仅仅是封装函数库**，并没有模拟完整的操作系统，如图：

![](<assets/1740016765537.png>)

对比来看：

![](<assets/1740016765832.png>)

小结：

**Docker 和虚拟机的差异：**

*   **docker 是一个系统进程**；**虚拟机是**在操作系统中的**操作系统**
    
*   **docker 体积小、启动速度快、性能好**；虚拟机体积大、启动速度慢、性能一般
    

### 1.3.Docker 架构

#### 1.3.1. 镜像和容器

Docker 中有几个重要的概念：

**镜像（Image）**：Docker 将**应用程序**及其所需的**依赖、函数库、环境、配置**等文件**打包**在一起，称为镜像。

**容器（Container）**：**镜像中的应用程序运行后形成的进程**就是**容器**，只是 Docker 会给容器进程做隔离，**对外不可见**。

一切应用最终都是代码组成，都是硬盘中的一个个的字节形成的**文件**。只有运行时，才会加载到内存，形成进程。

而**镜像**，就是把一个应用在硬盘上的文件、及其运行环境、部分系统函数库文件一起打包形成的文件包。

**每个文件包（镜像）是只读的，每个容器是可读写的，所以镜像不会被干扰。**可以基于镜像去创建容器，然后在每个容器里记录日志等数据。

**容器**，就是**将这些文件中**编写的**程序、函数加载到内存中**允许，形成进程，只不过要**隔离**起来。因此**一个镜像可以启动多次**，形成多个容器进程。

![](<assets/1740016766060.png>)

例如你下载了一个 QQ，如果我们将 QQ 在磁盘上的运行**文件**及其运行的操作系统依赖打包，形成 QQ 镜像。然后你可以启动多次，双开、甚至三开 QQ，跟多个妹子聊天。

#### 1.3.2.DockerHub

开源应用程序非常多，打包这些应用往往是重复的劳动。为了避免这些重复劳动，人们就会将自己打包的应用镜像，例如 Redis、MySQL 镜像放到网络上，共享使用，就像 GitHub 的代码共享一样。

*   **DockerHub：**DockerHub 是一个官方的 **Docker 镜像的托管平台**。**这样的平台称为 Docker 注册 Docker Registry。**
    
*   国内也有类似于 DockerHub 的公开服务，比如 [网易云镜像服务](https://c.163yun.com/hub "网易云镜像服务")、[阿里云镜像库](https://cr.console.aliyun.com/ "阿里云镜像库")等。
    

我们一方面可以将自己的镜像共享到 DockerHub，另一方面也可以从 DockerHub 拉取镜像：

![](<assets/1740016766350.png>)

#### 1.3.3.Docker 架构

我们要使用 Docker 来操作镜像、容器，就必须要安装 Docker。

Docker 是一个 CS 架构的程序，由两部分组成：

*   **服务端 (server)：**Docker **守护进程**，负责**处理 Docker 指令**，管理镜像、容器等
    
*   **客户端 (client)：**通过命令或 RestAPI **向 Docker 服务端发送指令**。可以在本地或远程向服务端发送指令。
    

如图：

![](<assets/1740016766645.png>)

docker build 是构建镜像，被 docker daemon 接收处理构建成镜像 images。

docker pull 拉取镜像，docker daemon 从接收指令后，从 registry 拉取镜像。

docker run 创建容器。

#### 1.3.4. 小结镜像、容器、结构、dockerhub

**镜像：**

*   将应用程序及其依赖、环境、配置打包在一起

**容器：**

*   镜像运行起来就是容器，一个镜像可以运行多个容器

**Docker 结构：**

*   服务端：接收命令或远程请求，操作镜像或容器
    
*   客户端：发送命令或者请求到 Docker 服务端
    

**DockerHub：**

*   一个镜像托管的服务器，类似的还有阿里云镜像服务，统称为 DockerRegistry

### 1.4.CentOS 安装 Docker

Docker 分为 CE 和 EE 两大版本。CE 即社区版（免费，支持周期 7 个月），EE 即企业版，强调安全，付费使用，支持周期 24 个月。

Docker CE 分为 `stable` `test` 和 `nightly` 三个更新频道。

官方网站上有各种环境下的 [安装指南](https://docs.docker.com/install/ "安装指南")，这里主要介绍 Docker CE 在 CentOS 上的安装。

Docker CE 支持 64 位版本 CentOS 7，并且要求内核版本不低于 3.10， CentOS 7 满足最低内核的要求，所以我们在 CentOS 7 安装 Docker。

#### 1.4.1. 卸载（可选）

如果之前安装过旧版本的 Docker，可以使用下面命令卸载：

```
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine \
                  docker-ce
```

#### 1.4.2. 安装 docker

首先需要大家虚拟机联网，安装 yum 工具

```
yum install -y yum-utils \
           device-mapper-persistent-data \
           lvm2 --skip-broken
```

然后更新本地镜像源：

```
## 1.4.设置docker镜像源
yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    
sed -i 's/download.docker.com/mirrors.aliyun.com\/docker-ce/g' /etc/yum.repos.d/docker-ce.repo
 
yum makecache fast
```

然后输入命令：

```
## 1.4.关闭
systemctl stop firewalld
## 1.4.禁止开机启动防火墙
systemctl disable firewalld
```

docker-ce 为社区免费版本。稍等片刻，docker 即可安装成功。

#### 1.4.3. 启动 docker

Docker 应用需要用到各种端口，逐一去修改防火墙设置。非常麻烦，因此建议大家直接关闭防火墙！

启动 docker 前，**一定要关闭防火墙**后！！

启动 docker 前，一定要关闭防火墙后！！

启动 docker 前，一定要关闭防火墙后！！

```
systemctl start docker  ## 1.4.启动docker服务
 
systemctl stop docker  ## 1.4.停止docker服务
 
systemctl restart docker  ## 1.4.重启docker服务
```

通过命令启动 docker：

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://tfa5975m.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

然后输入命令，可以查看 docker 版本：

```
#docker run --name 容器名称 -p 主机端口:需要映射的容器端口 -d 镜像名称
docker run --name mn -p 80:80 -d nginx
```

#### 1.4.4. 配置镜像加速

docker 官方镜像仓库网速较差，我们需要设置国内镜像服务：

参考阿里云的镜像加速文档：[阿里云登录 - 欢迎登录阿里云，安全稳定的云计算服务平台](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors "阿里云登录 - 欢迎登录阿里云，安全稳定的云计算服务平台")

通过修改 daemon 配置文件 / etc/docker/daemon.json 来使用加速器。

**在 ssh 输入下面命令：**

```
#docker stop 容器名
docker stop mn
```

可以看见，多了个 / etc/docker/daemon.json 文件：

![](<assets/1740016766932.png>)

#### 1.4.5 设置开机启动 docker

1、查看已经启动的服务

```
docker run --name mr -p 6379:6379 -d redis redis-server --appendonly yes
docker exec -it mr bash
redis-cli
```

2、查看是否设置开机启动

```
docker run \
  --name mn \
  -v html:/usr/share/nginx/html \
  -p 8080:80 \
  -d nginx 
#docker run --name mn -v html:/usr/share/nginx/html -p 80:80 -d nginx
```

3、设置开机启动

```
# 查看html数据卷的位置
docker volume inspect html
# 进入该目录
cd /var/lib/docker/volumes/html/_data
# 修改文件
vi index.html
```

**设置容器自启**

```
cd /tmp/
mkdir mysql
cd mysql
mkdir data
mkdir conf
cd conf
touch my.cnf
vi my.cnf
```

例如：

```
[mysqld]
skip-name-resolve
character_set_server=utf8
datadir=/var/lib/mysql
server-id=1000
```

## 2.Docker 的基本操作

### 2.1. 镜像操作

#### 2.1.1. 镜像名称

首先来看下镜像的名称组成：

*   镜名称一般分两部分组成：**[repository]:[tag]**。
*   在没有指定 **tag** 时，**默认是 latest**，代表**最新版本**的镜像

如图：

![](<assets/1740016767187.png>)

这里的 mysql 就是 repository，5.7 就是 tag，合一起就是镜像名称，代表 5.7 版本的 MySQL 镜像。

#### 2.1.2. 镜像命令

常见的镜像操作命令如图：

![](<assets/1740016767455.png>)

```
cd /tmp/
mkdir docker-demo
cd docker-demo
```

```
# 指定基础镜像
FROM ubuntu:16.04
# 配置环境变量，JDK的安装目录
ENV JAVA_DIR=/usr/local
 # 拷贝jdk和java项目的包
COPY ./jdk8.tar.gz $JAVA_DIR/
COPY ./docker-demo.jar /tmp/app.jar
 # 安装JDK
RUN cd $JAVA_DIR \
 && tar -xf ./jdk8.tar.gz \
 && mv ./jdk1.8.0_144 ./java8
 # 配置环境变量
ENV JAVA_HOME=$JAVA_DIR/java8
ENV PATH=$PATH:$JAVA_HOME/bin
 # 暴露端口
EXPOSE 8090
# 入口，java项目的启动命令
ENTRYPOINT java -jar /tmp/app.jar
```

```
#最后面的空格点别忘了
docker build -t javaweb:1.0 .
```

```
FROM java:8-alpine
COPY ./docker-demo.jar /tmp/app.jar
EXPOSE 8090
ENTRYPOINT java -jar /tmp/app.jar
```

```
#最后面的空格点别忘了，表示在当前目录下构建镜像
docker build -t javaweb:2.0 .
```

#### 2.1.3. 案例 1 - 拉取、查看镜像

**需求：从 DockerHub 中拉取一个 nginx 镜像并查看**

**1）搜索镜像名称。**首先去镜像仓库搜索 nginx 镜像，比如 [DockerHub](https://hub.docker.com/ "DockerHub"):

![](<assets/1740016767752.png>)

**2）拉取镜像。**根据查看到的镜像名称，这里拉取官方镜像，通过命令：docker pull nginx

![](<assets/1740016767932.png>)

```
version: "3.8"
 services:
 mysql:
 image: mysql:5.7.25
 environment:
 MYSQL_ROOT_PASSWORD: 123 
 volumes:
 - "/tmp/mysql/data:/var/lib/mysql"
 - "/tmp/mysql/conf/hmy.cnf:/etc/mysql/conf.d/hmy.cnf"
#临时构建镜像并运行
 web:
 build: .
 ports:
 - "8090:8090"
```

![](<assets/1740016768201.png>)

**3）查看镜像。**通过命令：docker images 查看拉取到的镜像

```
## 1.4.安装
curl -L https://github.com/docker/compose/releases/download/1.23.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

![](<assets/1740016768522.png>)

#### 2.1.4. 案例 2 - 保存、导入镜像

**需求：**利用 docker save 将 **nginx 镜像导出磁盘，然后再通过 load 加载回来**

**1）查看命令用法。**利用 **docker xx --help** 命令查看 docker save 和 docker load 的语法

例如，查看 save 命令用法，可以输入命令：

```
## 1.4.修改权限
chmod +x /usr/local/bin/docker-compose
```

结果：

![](<assets/1740016768713.png>)

命令格式：

```
## 1.4.补全命令
curl -L http://raw.githubusercontent.com/docker/compose/1.29.1/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose
```

**2）保存镜像。**使用 docker save 导出镜像到磁盘

运行命令：

```
version: "3.2"
 
services:
 nacos:
 image: nacos/nacos-server
 environment:
#nacos启动命令startup.cmd -m standalone
 MODE: standalone
 ports:
 - "8848:8848"
 mysql:
 image: mysql:5.7.25
 environment:
 MYSQL_ROOT_PASSWORD: 123
 volumes:
 - "$PWD/mysql/data:/var/lib/mysql"
 - "$PWD/mysql/conf:/etc/mysql/conf.d/"
 userservice:
 build: ./user-service
 orderservice:
 build: ./order-service
 gateway:
 build: ./gateway
 ports:
 - "10010:10010"
```

结果如图：

![](<assets/1740016768970.png>)

**3）导入镜像。**使用 docker load 加载镜像

先删除本地的 nginx 镜像：

```
FROM java:8-alpine
COPY ./app.jar /tmp/app.jar
ENTRYPOINT java -jar /tmp/app.jar
```

rmi 是 remove image 的缩写。

然后运行命令，加载本地文件：

```
spring:
 datasource:
#这里mysql还是有端口的，仅用于服务内调用，docker-compose就不配置端口
 url: jdbc:mysql://mysql:3306/cloud_order?useSSL=false
 username: root
 password: 123
 driver-class-name: com.mysql.jdbc.Driver
 application:
 name: orderservice
 cloud:
 nacos:
 server-addr: nacos:8848 # nacos服务地址
```

结果：

![](<assets/1740016769226.png>)

### 2.2. 容器操作

#### 2.2.1. 容器相关命令

容器操作的命令如图：

![](<assets/1740016769560.png>)

exec 是 execute 的缩写。 

容器保护三个状态：

*   运行：进程正常运行
*   **暂停：**进程暂停，CPU 不再运行，**并不释放内存**
*   停止：进程终止，回收进程占用的内存、CPU 等资源

其中：

*   **docker run：创建并运行一个容器，处于运行状态**
    
*   **docker pause：让一个运行的容器暂停**
    
*   **docker unpause：让一个容器从暂停状态恢复运行**
    
*   **docker stop：停止一个运行的容器**
    
*   **docker start：让一个停止的容器再次运行**
    
*   **docker rm：删除一个容器**
    

#### 2.2.2. 案例 - 创建并运行容器，run

可以看官方方法启动容器 

[Docker Hub](https://hub.docker.com/_/nginx "Docker Hub")

![](<assets/1740016769847.png>)

**创建并运行 nginx 容器的命令：**

```
<build>
  <!-- 服务打包的最终名称 -->
  <finalName>app</finalName>
  <plugins>
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
    </plugin>
  </plugins>
</build>
```

命令解读：

*   docker run ：创建并运行一个容器
*   –name : 给容器起一个名字，比如叫做 mn（my_nginx 缩写）
*   **-p ：将宿主机端口给容器端口映射**，冒号左侧是宿主机端口，右侧是容器端口
*   **-d：后台运行容器**
*   nginx：镜像名称，例如 nginx

**端口映射详解：**

这里的**`-p`**参数，是**将容器端口映射到宿主机端口**。

默认情况下，**容器是隔离环境**，我们**不能直接访问到容器的 80 端口**。

现在，将容器的 80 与宿主机的 80 关联起来，当我们**访问宿主机的 80 端口时**，就会被**映射到容器的 80**，**这样访问主机的 80 端口，就相当于访问容器的 80 端口**。

![](<assets/1740016770135.png>)

**启动容器后生成唯一 ID  ：**

![](<assets/1740016770317.png>)

**访问主机 80 端口可以看到 Nginx 已经运行：**

![](<assets/1740016770543.png>)

#### 2.2.2.5. 查看容器状态、日志

**查看所有容器运行状态：**

```
docker run -d \
    --restart=always \
    --name registry	\
    -p 5000:5000 \
    -v registry-data:/var/lib/registry \
    registry
```

![](<assets/1740016770874.png>)

**查看日志：**

```
## 1.4.打开要修改的文件
vi /etc/docker/daemon.json
## 1.4.添加内容：
"insecure-registries":["http://192.168.150.101:8080"]
## 1.4.重加载
systemctl daemon-reload
## 1.4.重启docker
systemctl restart docker
```

**跟踪日志：**

```
cd /root
mkdir registry-ui
cd registry-ui
touch docker-compose.yml
vi docker-compose.yml
```

![](<assets/1740016771191.png>)

#### 2.2.3. 案例 - 进入容器，修改文件（不建议）

**不建议进入容器修改文件，建议用下面 2.3 的数据卷。** 

**需求**：进入 Nginx 容器，修改 HTML 文件内容，添加 “传智教育欢迎您”

**提示**：进入容器要用到 docker exec 命令。

**步骤**：

**1）进入容器。**进入我们刚刚创建的 nginx 容器的命令为：

```
version: '3.0'
services:
  registry:
    image: registry
    volumes:
      - ./registry-data:/var/lib/registry
  ui:
    image: joxit/docker-registry-ui:static
    ports:
      - 8080:80
    environment:
      - REGISTRY_TITLE=传智教育私有仓库
      - REGISTRY_URL=http://registry:5000
    depends_on:
      - registry
```

exec 是 execute 执行的缩写。 

 **命令解读：**

*   docker exec ：进入容器内部，执行一个命令
    
*   **-it :** 给当前进入的**容器创建一个标准输入、输出终端**，允许我们与容器交互
    
*   mn ：要进入的容器的名称
    
*   **bash：进入容器后执行的命令**，bash 是一个 linux 终端交互命令
    

![](<assets/1740016771477.png>)

 **退出容器**

```
exit
```

docker 容器里命令是阉割版 Linux 命令，没有 ll 和 vim，有 ls。

**2）进入 nginx 的 HTML 所在目录** /usr/share/nginx/html

![](<assets/1740016771766.png>)

容器内部会模拟一个独立的 Linux 文件系统，看起来如同一个 linux 服务器一样：

![](<assets/1740016771941.png>)

nginx 的环境、配置、运行文件全部都在这个文件系统中，包括我们要修改的 html 文件。

查看 DockerHub 网站中的 nginx 页面，可以知道 nginx 的 html 目录位置在`/usr/share/nginx/html`

**进入 html 目录：**

```
cd /usr/share/nginx/html
```

查看目录下文件：

![](<assets/1740016772035.png>)

**3）修改 index.html 的内容**

容器内**没有 vi 命令**，无法直接修改，我们用下面的命令来修改：

```
sed -i -e 's#Welcome to nginx#传智教育欢迎您#g' -e 's#<head>#<head><meta charset="utf-8">#g' index.html
```

在浏览器访问自己的虚拟机地址，例如我的是：http://192.168.150.101，即可看到结果：

![](<assets/1740016772363.png>)

**4）退出容器**

```
exit
```

![](<assets/1740016772618.png>)

#### 2.2.4. 停止、删除容器

**停止容器：** 

```
#docker stop 容器名
docker stop mn
```

**查看所有容器（包括停止的） ：**

```
docker ps -a
```

![](<assets/1740016772875.png>)

**强制删除容器**

```
docker rm -f mn
```

![](<assets/1740016773115.png>)

#### 2.2.5. 创建 redis 容器

[Docker Hub](https://hub.docker.com/_/redis "Docker Hub")

![](<assets/1740016773406.png>)

```
docker run --name mr -p 6379:6379 -d redis redis-server --appendonly yes
docker exec -it mr bash
redis-cli
```

#### 2.2.5. 小结

docker run 命令的常见参数有哪些？

*   –name：指定容器名称
*   -p：指定端口映射
*   -d：让容器后台运行

查看容器日志的命令：

*   docker logs
*   添加 -f 参数可以持续查看日志

查看容器状态：

*   docker ps
*   docker ps -a 查看所有容器，包括已经停止的

### 2.3. 数据卷（容器数据管理）

在之前的 nginx 案例中，修改 nginx 的 html 页面时，需要进入 nginx 内部。并且因为没有编辑器，修改文件也很麻烦。

这就是因为**容器与数据（容器内文件）耦合**带来的后果。

![](<assets/1740016773613.png>)

要解决这个问题，**必须将数据与容器解耦**，这就要用到**数据卷**了。

#### 2.3.1. 什么是数据卷

**数据卷（volume 译为卷，容积，音量）**是一个**虚拟目录，指向宿主机文件系统中的某个目录。**

![](<assets/1740016773907.png>)

一旦完成数据卷挂载，**对容器的一切操作都会作用在数据卷对应的宿主机目录了。**

这样，我们操作宿主机的 / var/lib/docker/volumes/html 目录，就等于操作容器内的 / usr/share/nginx/html 目录了。只要数据卷对应的宿主机目录在，删除再创建容器，数据依然不变。

#### 2.3.2. 数据卷操作命令

数据卷操作的基本语法如下：

```
docker volume [COMMAND]
```

docker volume 命令是数据卷操作，根据命令后跟随的 command 来确定下一步的操作：

*   create 创建一个 volume
*   inspect 显示一个或多个 volume 的信息
*   ls 列出所有的 volume
*   prune 删除未使用的 volume
*   rm 删除一个或多个指定的 volume

![](<assets/1740016774191.png>)

#### 2.3.3. 创建和查看数据卷

**需求**：创建一个数据卷，并查看数据卷在宿主机的目录位置

**① 创建数据卷**

```
docker volume create html
```

**② 查看所有数据**

```
docker volume ls
```

结果：

![](<assets/1740016774517.png>)

**③ 查看数据卷详细信息卷**

```
docker volume inspect html
```

结果：

![](<assets/1740016774773.png>)

可以看到，我们创建的 html 这个数据卷关联的宿主机目录为`/var/lib/docker/volumes/html/_data`目录。

**小结**：

**数据卷的作用：**

*   将容器与数据分离，解耦合，方便操作容器内数据，保证数据安全

**数据卷操作：**

*   docker volume create：创建数据卷
*   docker volume ls：查看所有数据卷
*   docker volume inspect：查看数据卷详细信息，包括关联的宿主机目录位置
*   docker volume rm：删除指定数据卷
*   docker volume prune：删除所有未使用的数据卷

#### 2.3.4. 挂载数据卷、宿主机目录，-v

**挂载宿主机目录：**

```
docker run --name mn -v /root/nginx/html:/usr/share/nginx/html -p 80:80 -d nginx
```

**挂载数据卷** 

我们在创建容器时，可以通过 -v 参数来**挂载一个数据卷到某个容器内目录**，命令格式如下：

```
docker run \
  --name mn \
  -v html:/usr/share/nginx/html \
  -p 8080:80 \
  -d nginx 
#docker run --name mn -v html:/usr/share/nginx/html -p 80:80 -d nginx
```

这里的 - v 就是挂载数据卷的命令：

*   **`-v html:`/usr/share/nginx/html：左边是数据卷名，右边是容器内目录。**把 html **数据卷挂载到容器内**的 / usr/share/nginx/html 这个目录中

可以看到容器内数据通过挂载数据卷指向的目录里，已经出现初始文件。

![](<assets/1740016775063.png>)

数据卷挂载与目录直接挂载的

*   **数据卷挂载耦合度低**，由 docker 来管理目录，但是目录较深，**不好找**
*   **目录挂载耦合度高**，需要我们自己管理目录，不过目录容易**寻找查看**

**查看数据卷 html 的信息 ：**

```
docker volume inspect html
```

![](<assets/1740016775373.png>)

#### 2.3.5. 案例 - 给 nginx 挂载数据卷

**需求**：创建一个 nginx 容器，修改容器内的 html 目录内的 index.html 内容

**分析**：上个案例中，我们进入 nginx 容器内部，已经知道 nginx 的 html 目录所在位置 / usr/share/nginx/html ，我们需要把这个目录挂载到 html 这个数据卷上，方便操作其中的内容。

**提示**：运行容器时使用 -v 参数挂载数据卷

步骤：

**① 创建容器并挂载数据卷到容器内的 HTML 目录**

```
docker run --name mn -v html:/usr/share/nginx/html -p 80:80 -d nginx
```

**② 进入 html 数据卷指向的宿主机位置，并修改 HTML 内容**

```
# 查看html数据卷的位置
docker volume inspect html
# 进入该目录
cd /var/lib/docker/volumes/html/_data
# 修改文件
vi index.html
```

![](<assets/1740016775719.png>)

#### 2.3.6.docker 安装 MySQL，挂载本地目录

**docker 安装 mysql 和直接安装的区别：**

1、docker 安装快速，效率高;

2、docker 隔离性好，**可以安装无数个 mysql 实例**，互相不干扰，只要映射主机端口不同即可;

3、占用资源少，MB 级别，而服务器安装 GB 级别;

4、启动速度秒级，而服务器安装启动分钟级别;

5、性能接近原生，而服务器安装较低;

6、数据备份、迁移，docker 更方便强大;

7、卸载管理更方便和干净，直接删除容器和镜像即可;

8、稳定性，只要保证 docker 环境没问题，mysql 就没问题。

容器不仅仅可以挂载数据卷，也可以直接挂载到宿主机目录上。关联关系如下：

*   **带数据卷模式：宿主机目录 --> 数据卷 —> 容器内目录**
*   **直接挂载模式：宿主机目录 —> 容器内目录**

如图：

![](<assets/1740016775971.png>)

**语法**：

目录挂载与数据卷挂载的语法是类似的：

*   **-v [宿主机目录]:[容器内目录]**
*   **-v [宿主机文件]:[容器内文件]**

**需求**：创建并运行一个 MySQL 容器，将宿主机目录直接挂载到容器

实现思路如下：

1）在将课前资料中的 mysql.tar 文件上传到虚拟机，通过 load 命令加载为镜像

```
docker load -i mysql.tar
```

 也可以拉取

```
sudo docker pull mysql:5.7.25
```

2）创建目录 / tmp/mysql/data 、/tmp/mysql/conf，将课前资料提供的 hmy.cnf 上传到 / tmp/mysql/conf

```
cd /tmp/
mkdir mysql
cd mysql
mkdir data
mkdir conf
cd conf
touch my.cnf
vi my.cnf
```

```
[mysqld]
skip-name-resolve
character_set_server=utf8
datadir=/var/lib/mysql
server-id=1000
```

4）去 DockerHub 查阅资料，创建并运行 MySQL 容器，要求：

[Docker Hub](https://hub.docker.com/_/mysql "Docker Hub")

**注意端口 3306 占用问题。** 

```
docker run --name mysql -e MYSQL_ROOT_PASSWORD=1234 -p 3306:3306 -v /tmp/mysql/conf/hmy.cnf:/etc/mysql/conf.d/hmy.cnf -v /tmp/mysql/data:/var/lib/mysql -d mysql:5.7.25
```

![](<assets/1740016776217.png>)

![](<assets/1740016776482.png>)

使用 Navicat 可以连接：

![](<assets/1740016776665.png>)

① 挂载 / tmp/mysql/data 到 mysql 容器内数据存储目录

② 挂载 / tmp/mysql/conf/hmy.cnf 到 mysql 容器的配置文件

③ 设置 MySQL 密码

#### 2.3.7. 小结

docker run 的命令中通过 -v 参数挂载文件或目录到容器中：

*   -v volume 名称: 容器内目录
*   -v 宿主机文件: 容器内文
*   -v 宿主机目录: 容器内目录

数据卷挂载与目录直接挂载的

*   **数据卷挂载耦合度低**，由 docker 来管理目录，但是目录较深，**不好找**
*   **目录挂载耦合度高**，需要我们自己管理目录，不过目录容易寻找查看

## 3.Dockerfile 自定义镜像

常见的镜像在 DockerHub 就能找到，但是我们自己写的项目就必须自己构建镜像了。

而要自定义镜像，就必须先了解镜像的结构才行。

### 3.1. 镜像结构

**镜像是将应用程序及其需要的系统函数库、环境、配置、依赖打包而成。**

我们以 MySQL 为例，来看看镜像的组成结构：

![](<assets/1740016776863.png>)

**分层操作可以提高更新版本时的复用性。** 

简单来说，镜像就是在系统函数库、运行环境基础上，添加应用程序文件、配置文件、依赖文件等组合，然后编写好启动脚本打包在一起形成的文件。

我们要构建镜像，其实就是实现上述打包的过程。

### 3.2.Dockerfile 语法

构建自定义的镜像时，并不需要一个个文件去拷贝，打包。

我们只需要告诉 Docker，我们的镜像的组成，需要哪些 BaseImage、需要拷贝什么文件、需要安装什么依赖、启动脚本是什么，将来 Docker 会帮助我们构建镜像。

而描述上述信息的文件就是 Dockerfile 文件。

**Dockerfile 就是一个文本文件**，其中**包含**一个个的**指令 (Instruction)**，用指令来说明要执行什么操作来构建镜像。**每一个指令都会形成一层 Layer**。

![](<assets/1740016777176.png>)

更新详细语法说明，请参考官网文档： https://docs.docker.com/engine/reference/builder

### 3.3. 使用 **Dockerfile** 构建 Java 项目的镜像

#### 3.3.1. 基于 Ubuntu 构建 Java 项目（不推荐）

需求：基于 Ubuntu 镜像构建一个新镜像，运行一个 java 项目

*   **步骤 1：新建一个空文件夹 docker-demo**
    
    ```
    cd /tmp/
    mkdir docker-demo
    cd docker-demo
    ```
    
*   **步骤 2：**拷贝课前资料中的 docker-demo.jar 文件到 docker-demo 这个目录
    
    ![](<assets/1740016777445.png>)
    
*   **步骤 3：**拷贝课前资料中的 jdk8.tar.gz 文件到 docker-demo 这个目录
    
    ![](<assets/1740016777650.png>)
    
*   步骤 4：拷贝课前资料提供的 Dockerfile 到 docker-demo 这个目录
    
    ![](<assets/1740016777866.png>)
    
    其中的内容如下：
    
    ```
    # 指定基础镜像
    FROM ubuntu:16.04
    # 配置环境变量，JDK的安装目录
    ENV JAVA_DIR=/usr/local
     # 拷贝jdk和java项目的包
    COPY ./jdk8.tar.gz $JAVA_DIR/
    COPY ./docker-demo.jar /tmp/app.jar
     # 安装JDK
    RUN cd $JAVA_DIR \
     && tar -xf ./jdk8.tar.gz \
     && mv ./jdk1.8.0_144 ./java8
     # 配置环境变量
    ENV JAVA_HOME=$JAVA_DIR/java8
    ENV PATH=$PATH:$JAVA_HOME/bin
     # 暴露端口
    EXPOSE 8090
    # 入口，java项目的启动命令
    ENTRYPOINT java -jar /tmp/app.jar
    ```
    
*   步骤 5：进入 docker-demo
    
    将准备好的 docker-demo 上传到虚拟机任意目录，然后进入 docker-demo 目录下
    
*   步骤 6：运行命令：
    
    ```
    #最后面的空格点别忘了
    docker build -t javaweb:1.0 .
    ```
    

创建容器 

```
docker run --name web -p 8090:8090 -d javaweb:1.0
```

最后访问 http:// 虚拟机 ip 地址: 8090/hello/count，其中的 ip 改成你的虚拟机 ip

#### 3.3.2. 基于 java8 构建 Java 项目（推荐）

虽然我们可以基于 Ubuntu 基础镜像，添加任意自己需要的安装包，构建镜像，但是却比较麻烦。所以大多数情况下，我们都可以在一些安装了部分软件的基础镜像上做改造。

例如，构建 java 项目的镜像，可以在已经准备了 JDK 的基础镜像基础上构建。

**需求：基于 java:8-alpine 镜像，将一个 Java 项目构建为镜像**

实现思路如下：

*   ① 新建一个空的目录，然后在目录中新建一个文件，命名为 Dockerfile
    
*   ② 拷贝课前资料提供的 docker-demo.jar 到这个目录中
    
*   ③ 编写 Dockerfile 文件：
    
    *   a ）基于 java:8-alpine 作为基础镜像
        
    *   b ）将 app.jar 拷贝到镜像中
        
    *   c ）暴露端口
        
    *   d ）编写入口 ENTRYPOINT
        
        内容如下：
        
        ```
        FROM java:8-alpine
        COPY ./docker-demo.jar /tmp/app.jar
        EXPOSE 8090
        ENTRYPOINT java -jar /tmp/app.jar
        ```
        
*   ④ 使用 docker build 命令构建镜像
    
    ```
    #最后面的空格点别忘了，表示在当前目录下构建镜像
    docker build -t javaweb:2.0 .
    ```
    
*   ⑤ 使用 docker run 创建容器并运行
    
    ```
    docker run --name web -p 8090:8090 -d javaweb:2.0
    ```
    

### 3.4. 小结

小结：

1.  Dockerfile 的本质是一个文件，通过指令描述镜像的构建过程
    
2.  Dockerfile 的第一行必须是 FROM，从一个基础镜像来构建
    
3.  基础镜像可以是基本操作系统，如 Ubuntu。也可以是其他人制作好的镜像，例如：java:8-alpine
    

## 4.Docker-Compose，部署分布式项目

Docker Compose 可以基于 Compose 文件帮我们快速的部署分布式应用，而无需手动一个个创建和运行容器！

![](<assets/1740016778084.png>)

### 4.1. 初识 DockerCompose

compose 译为生成，组成，写作。 

Compose 文件是一个文本文件，通过指令定义集群中的每个容器如何运行。格式如下：

```
version: "3.8"
 services:
 mysql:
 image: mysql:5.7.25
 environment:
 MYSQL_ROOT_PASSWORD: 123 
 volumes:
 - "/tmp/mysql/data:/var/lib/mysql"
 - "/tmp/mysql/conf/hmy.cnf:/etc/mysql/conf.d/hmy.cnf"
#临时构建镜像并运行
 web:
 build: .
 ports:
 - "8090:8090"
```

上面的 Compose 文件就描述一个项目，其中包含两个容器：

*   mysql：一个基于`mysql:5.7.25`镜像构建的容器，并且挂载了两个目录。这里不需要暴露端口，因为微服务集群部署，集群内部能访问即可，不用暴露。
*   web：一个基于`docker build`临时构建的镜像容器，映射端口时 8090

DockerCompose 的详细语法参考官网：https://docs.docker.com/compose/compose-file/

其实 DockerCompose 文件可以看做是将多个 docker run 命令写到一个文件，只是语法稍有差异。

### 4.2. 安装 DockerCompose

#### 4.2.1. 下载

Linux 下需要通过命令下载：

```
## 1.4.安装
curl -L https://github.com/docker/compose/releases/download/1.23.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

如果下载速度较慢，或者下载失败，可以使用课前资料提供的 docker-compose 文件：

上传到`/usr/local/bin/`目录也可以。

#### 4.2.2. 修改文件权限

修改文件权限，给执行权：

```
## 1.4.修改权限
chmod +x /usr/local/bin/docker-compose
```

#### 4.2.3.Base 自动补全命令：

```
## 1.4.补全命令
curl -L http://raw.githubusercontent.com/docker/compose/1.29.1/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose
```

如果这里出现错误，需要修改自己的 hosts 文件然后重启 systemctl restart docke、新建 shell 窗口：

```
echo "199.232.68.133 raw.githubusercontent.com" >> /etc/hosts
```

### 4.3. 部署微服务集群

**需求**：将之前学习的 cloud-demo 微服务集群利用 DockerCompose 部署

**实现思路**：

① 查看课前资料提供的 cloud-demo 文件夹，里面已经编写好了 docker-compose 文件

② 修改自己的 cloud-demo 项目，将数据库、nacos 地址都命名为 docker-compose 中的服务名

③ 使用 maven 打包工具，将项目中的每个微服务都打包为 app.jar

④ 将打包好的 app.jar 拷贝到 cloud-demo 中的每一个对应的子目录中

⑤ 将 cloud-demo 上传至虚拟机，利用 docker-compose up -d 来部署

#### 4.3.1. 初始 compose、Dockerfile 介绍

查看课前资料提供的 cloud-demo 文件夹，里面已经编写好了 docker-compose 文件，而且每个微服务都准备了一个独立的目录：

![](<assets/1740016778378.png>)

**docker-compose.yml** 内容如下：

```
version: "3.2"
 
services:
 nacos:
 image: nacos/nacos-server
 environment:
#nacos启动命令startup.cmd -m standalone
 MODE: standalone
 ports:
 - "8848:8848"
 mysql:
 image: mysql:5.7.25
 environment:
 MYSQL_ROOT_PASSWORD: 123
 volumes:
 - "$PWD/mysql/data:/var/lib/mysql"
 - "$PWD/mysql/conf:/etc/mysql/conf.d/"
 userservice:
 build: ./user-service
 orderservice:
 build: ./order-service
 gateway:
 build: ./gateway
 ports:
 - "10010:10010"
```

可以看到，其中包含 5 个 service 服务：

*   `nacos`：作为注册中心和配置中心
    *   `image: nacos/nacos-server`： 基于 nacos/nacos-server 镜像构建
    *   `environment`：环境变量
        *   `MODE: standalone`：单点模式启动
    *   `ports`：端口映射，这里暴露了 8848 端口
*   `mysql`：数据库
    *   `image: mysql:5.7.25`：镜像版本是 mysql:5.7.25
    *   `environment`：环境变量
        *   `MYSQL_ROOT_PASSWORD: 123`：设置数据库 root 账户的密码为 123
    *   `volumes`：数据卷挂载，这里挂载了 mysql 的 data、conf 目录，其中有我提前准备好的数据
*   `userservice`、`orderservice`、`gateway`：都是基于 Dockerfile 临时构建的

查看 mysql 目录，可以看到其中已经准备好了 cloud_order、cloud_user 表：

![](<assets/1740016778570.png>)

查看微服务目录，可以看到都包含 Dockerfile 文件：

![](<assets/1740016778981.png>)

内容如下：

```
FROM java:8-alpine
COPY ./app.jar /tmp/app.jar
ENTRYPOINT java -jar /tmp/app.jar
```

#### 4.3.2. 修改微服务配置，localhost 改服务名

用 **docker-compose** 部署微服务时，**所有服务之间用服务名互相访问。**

因为微服务将来要部署为 docker 容器，而容器之间互联不是通过 IP 地址，而是通过容器名。这里我们将 order-service、user-service、gateway 服务的 mysql、nacos 地址都修改为基于容器名的访问。

如下所示：

```
spring:
 datasource:
#这里mysql还是有端口的，仅用于服务内调用，docker-compose就不配置端口
 url: jdbc:mysql://mysql:3306/cloud_order?useSSL=false
 username: root
 password: 123
 driver-class-name: com.mysql.jdbc.Driver
 application:
 name: orderservice
 cloud:
 nacos:
 server-addr: nacos:8848 # nacos服务地址
```

#### 4.3.3. 所有服务打包成 app.jar

接下来需要将我们的每个微服务都打包。因为之前查看到 Dockerfile 中的 jar 包名称都是 app.jar，因此我们的每个微服务都需要用这个名称。

可以通过修改 pom.xml 中的打包名称来实现，**除了父工程和 feign-api 每个微服务的 pom 都需要修改：** 

```
<build>
  <!-- 服务打包的最终名称 -->
  <finalName>app</finalName>
  <plugins>
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
    </plugin>
  </plugins>
</build>
```

打包后：

![](<assets/1740016779287.png>)

#### 4.3.4. 拷贝 jar 包到部署目录

编译打包好的 app.jar 文件，需要放到 Dockerfile 的同级目录中。注意：每个微服务的 app.jar 放到与服务名称对应的目录，别搞错了。

user-service：

![](<assets/1740016779586.png>)

order-service：

![](<assets/1740016779942.png>)

gateway：

![](<assets/1740016780217.png>)

#### 4.3.5. 部署

最后，我们需要将文件整个 cloud-demo 文件夹上传到虚拟机中，由 DockerCompose 部署。

上传到任意目录：

![](<assets/1740016780513.png>)

**部署：**

进入 cloud-demo 目录后，运行下面的命令：

```
docker-compose up -d
```

如果 nacos 没有第一个启动，会报错，此时要重启 nacos 外的服务 

![](<assets/1740016780752.png>)

```
docker-compose restart gateway userservice orderservice
```

查看日志：

```
docker-compose logs -f
```

查看命令

```
docker-compose --help
```

重启：

```
docker-compose restart -d
```

终止：

```
docker-compose stop
```

## 5.Docker 镜像仓库

### 5.1. 搭建私有镜像仓库

搭建镜像仓库可以基于 Docker 官方提供的 DockerRegistry 来实现。

官网地址：[Docker Hub](https://hub.docker.com/_/registry "Docker Hub")

#### 5.1.1. 简化版镜像仓库

Docker 官方的 Docker Registry 是一个基础版本的 Docker 镜像仓库，具备仓库管理的完整功能，但是没有图形化界面。

搭建方式比较简单，命令如下：

```
docker run -d \
    --restart=always \
    --name registry	\
    -p 5000:5000 \
    -v registry-data:/var/lib/registry \
    registry
```

命令中挂载了一个数据卷 registry-data 到容器内的 / var/lib/registry 目录，这是私有镜像库存放数据的目录。

访问 [http://YourIp:5000/v2/_catalog](http://YourIp:5000/v2/_catalog "http://YourIp:5000/v2/_catalog") 可以查看当前私有镜像服务中包含的镜像

#### 5.1.2. 带有图形化界面版本

**1. 配置 Docker 信任地址**

我们的私服采用的是 http 协议，默认不被 Docker 信任，所以需要做一个配置：

```
## 1.4.打开要修改的文件
vi /etc/docker/daemon.json
## 1.4.添加内容：
"insecure-registries":["http://192.168.150.101:8080"]
## 1.4.重加载
systemctl daemon-reload
## 1.4.重启docker
systemctl restart docker
```

**2. 启动**

 使用 DockerCompose 部署带有图象界面的 DockerRegistry，命令如下：

```
cd /root
mkdir registry-ui
cd registry-ui
touch docker-compose.yml
vi docker-compose.yml
```

将下面内容复制粘贴进去： 

```
version: '3.0'
services:
  registry:
    image: registry
    volumes:
      - ./registry-data:/var/lib/registry
  ui:
    image: joxit/docker-registry-ui:static
    ports:
      - 8080:80
    environment:
      - REGISTRY_TITLE=传智教育私有仓库
      - REGISTRY_URL=http://registry:5000
    depends_on:
      - registry
```

然后启动

```
docker-compose up -d
```

**3. 浏览器访问**

http://centos 的 ip 地址: 8080

![](<assets/1740016781019.png>)

### 5.2. 推送、拉取镜像

推送镜像到私有镜像服务**必须先 tag 重命名，给镜像前加 "ip 地址: 8080/"**，步骤如下：

**① 重新 tag 本地镜像**，名称前缀为私有仓库的地址：192.168.150.101:8080/

```
docker tag nginx:latest 192.168.150.101:8080/nginx:1.0
```

**② 推送镜像（确保 tag 过）**

```
docker push 192.168.150.101:8080/nginx:1.0
```

**③ 拉取镜像**

```
docker pull 192.168.150.101:8080/nginx:1.0
```

![](<assets/1740016781292.png>)