---
url: https://blog.csdn.net/qq_40991313/article/details/126393653
title: 【Java 笔记 + 踩坑】SpringMVC 基础_随着互联网的发展, 上面的模式因为是同步调用, 性能慢慢的跟不是需求, 所以异步调用 - CSDN 博客
date: 2025-02-20 09:40:12
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[1，SpringMVC 简介](#t0)

[1.1 三层架构、MVC、异步回顾](#t1)

[1.1.1 三层架构](#t2)

[1.1.2 MVC 模式](#t3)

[1.1.3 异步取代同步](#t4)

[1.2 SpringMVC 概述](#t5)

[1.3 所有 SpringMvc 注解整理](#t6)

[2，SpringMVC 入门案例](#t7)

[2.1 需求分析](#t8)

[2.2 代码实现](#t9)

[知识点：@Controller@RequestMapping@ResponseBody](#t10)

[2.3 案例总结](#t11)

[2.4 工作流程解析](#t12)

[2.4.1 启动服务器初始化过程](#t13)

[2.4.2 单次请求过程](#t14)

[2.5 bean 加载控制，spring 排除加载表现层的 bean](#t15)

[2.5.1 问题分析](#t16)

[2.5.2 思路分析](#t17)

[2.5.3 环境准备](#t18)

[2.5.5 设置 bean 加载控制](#t19)

[知识点 @ComponentScan 的 excludeFilters](#t20)

[3，PostMan 工具的使用](#t21)

[3.1 PostMan 简介](#t22)

[3.2 PostMan 安装](#t23)

[3.3 PostMan 使用](#t24)

[3.3.1 创建 WorkSpace 工作空间](#t25)

[3.3.2 发送请求](#t26)

[3.3.3 保存当前请求](#t27)

[4，请求与响应](#t28)

[4.1 设置请求映射路径](#t29)

[4.1.1 环境准备](#t30)

[4.1.2 请求冲突问题分析](#t31)

[4.1.3 设置映射路径](#t32)

[4.2 普通请求参数传递入门](#t33)

[4.2.1 环境准备](#t34)

[4.2.2 请求参数传递和中文乱码解决](#t35)

[4.3 请求头的五种类型参数传递](#t36)

[4.3.1 普通请求参数，起别名，@RequestParam](#t37)

[4.3.2 POJO 数据类型](#t38)

[4.3.3 嵌套 POJO 类型参数](#t39)

[4.3.4 数组类型参数](#t40)

[4.3.5 集合类型参数，@RequestParam](#t41)

[知识点 @RequestParam](#t42)

[4.4 请求体的 JSON 数据传输参数，@RequestBody](#t43)

[4.4.1 概述](#t44) 

[4.4.2 JSON 普通数组，@EnableWebMvc](#t45)

[4.4.3 JSON 对象数据](#t46)

[4.4.4 JSON 对象数组](#t47)

[知识点 @EnableWebMvc@RequestBody](#t48)

[4.4.5 @RequestBody 与 @RequestParam 区别](#t49)

[4.4.5 @RequestParam 和 @RequestPart 的区别](#t50)

[4.5 日期类型参数传递 @DateTimeFormat](#t51)

[4.5.1 具体代码](#t52)

[知识点 @DateTimeFormat](#t53)

[4.5.2 类型转换内部实现原理](#t54)

[4.6 响应](#t55)

[4.6.1 环境准备，jackson-databind 依赖](#t56)

[4.6.2 响应 jsp 页面 [了解]](#t57)

[4.6.3 返回文本数据 [了解]](#t58)

[4.6.4 响应 JSON 数据](#t59)

[知识点 @ResponseBody](#t60)

[5，Rest 风格](#t61)

[5.1 REST 简介](#t62)

[5.2 RESTful 入门案例](#t63)

[5.2.1 环境准备](#t64)

[5.2.2 思路分析](#t65)

[5.2.3 代码实现，@RequestMapping 的 method 属性，@PathVariable](#t66)

[5.2.4 目前学的参数占位符汇总](#t67)

[知识点 @PathVariable](#t68)

[5.2.5 区别：@RequestBody、@RequestParam、@PathVariable](#t69)

[5.3 RESTful 优化，快速开发](#t70)

[5.3.1 概述，@RestController，@GetMapping](#t71) 

[知识点 @RestController@GetMapping @PostMapping @PutMapping @DeleteMapping](#t72)

[5.4 RESTful 案例](#t73)

[5.4.1 需求分析](#t74)

[5.4.2 环境准备](#t75)

[5.4.2 后台接口开发](#t76)

[5.4.3 放行静态页面，WebMvcConfigurer 配置类添加资源处理器](#t77)

## 1，SpringMVC 简介

### 1.1 三层架构、MVC、异步回顾

#### **1.1.1 三层架构**

开发过程中，我们把后端服务器 Servlet 拆分成三层，分别是 web、service 和 dao，这也是程序员常提到的 **“Java 味”**：

*   **web 层（表现层）**：直接与用户交互，负责接收用户输入和呈现数据
*   **service 层（业务层）**：处理具体的业务逻辑。也称为服务层或应用层。
*   **dao 层（数据访问层）**：负责与数据库交互，执行数据的增删改查

![](<assets/1740015613159.png>)

**优点**：

*   **代码复用**：同一个 dao 可能被多个 service 调用，分层后能避免重复编写 dao 代码，从而避免违反 DRY 原则（Don't Repeat Yourself，即不要写重复的代码）。
*   **低耦合**：各层的职责明确，页面交互、业务逻辑、数据库操作三层分离，降低了系统模块间的耦合。例如 service 只需要关注业务，编写时直接调用 dao 层，而不需要知道它具体依赖的哪个数据库。
*   **易维护**：每层的功能独立，业务逻辑更改后只需修改 Service，数据库更改后只需修改 dao。
*   **可扩展**：当增加新功能后，可以在各层扩展相关功能的类。例如如果将所有逻辑都写在 controller 里，这会因为需求变化而导致这个方法里的代码越来越多，可读性和可维护性都变差，这就需要进行水平（即分模块，按业务拆分模块）和垂直拆分（即使用三层架构）。

**servlet 存在问题：**

servlet 处理请求和数据的时候，**一个 servlet 只能处理一个请求**

**MVC 模式是对三层架构中的 web 层的优化。** 

#### **1.1.2 MVC 模式**

 **MVC 设计模式**：将后端 Servlet 设计为控制器 controller、视图 view、业务模型 Model。

*   **视图（View）**：显示 UI 页面数据，并与用户交互。前后端分离项目中视图就是前端代码，一体化项目中视图是 JSP、Thymeleaf 等框架渲染成的 HTML。
*   **控制器（Controller）**：负责接收浏览器发送过来的请求，然后响应给浏览器
*   **模型（Model）**：封装后端业务逻辑和应用的核心数据，与数据层交互。

**流程**：

1.  控制器（例如 serlvlet）用来接收浏览器发送过来的请求
2.  控制器调用模型（例如 JavaBean）来获取数据，比如从数据库查询数据；
3.  控制器获取到数据后再交由视图（例如 JSP）进行数据展示。  

**优点**：

*   **低耦合**：将数据、UI 和控制逻辑分离，便于开发、维护和扩展。
*   **可扩展性**：将逻辑处理和视图渲染分开，各个小组件能直接复用。

![](<assets/1740015615710.png>)

#### **1.1.3 异步取代同步**

随着互联网的发展，**MVC 模式**因为是**同步调用**，性能慢慢的跟不是需求，所以**异步调用**慢慢的走到了前台，是现在比较流行的一种处理方式。

![](<assets/1740015618143.png>)

**vue 异步调用的特点：** 

*   因为是**异步调用**，所以**后端不需要返回 view 视图**，将其去除。（回顾 jsp 就是 Servlet 响应到 jsp 页面展示；而 vue 是异步请求 Servlet，通过 JSON 数据前后端交互的）
*   前端如果通过**异步调用**的方式进行交互，后台就需要将返回的数据转换成 **json 格式**进行返回

### 1.2 SpringMVC 概述

**SpringMVC 是隶属于 Spring 框架的一部**分，主要是用来进行 **Web 开发**，是**对 Servlet 进行了封装**。

springmvc 功能和 Servlet 是一样的，只是更简洁。 

**SpringMVC 主要负责的是：**

*   **controller** 如何接收请求和数据
*   如何将请求和数据转发给业务层
*   如何将响应数据转换成 json 发回到前端

SpringMVC 是处于 **Web 层的框架**，**主要作用：接收前端发过来的请求和数据然后经过处理并将处理的结果响应给前端**。

REST 是一种软件架构风格，可以降低开发的复杂性，提高系统的可伸缩性，后期的应用也是非常广泛。

SSM 整合是把咱们所学习的 SpringMVC+Spring+Mybatis 整合在一起来完成业务开发，是对我们所学习这三个框架的一个综合应用。

**学习目标:**

1.  **掌握基于 SpringMVC 获取请求参数和响应 json 数据操作**
2.  **熟练应用基于 REST 风格的请求路径设置与参数传递**
3.  能够根据实际业务建立前后端开发通信协议并进行实现
4.  **基于 SSM 整合技术开发任意业务模块功能**

介绍了这么多，对 SpringMVC 进行一个定义

*   SpringMVC 是一种基于 Java 实现 MVC 模型的轻量级 Web 框架
    
*   优点
    
    *   使用简单、开发便捷 (相比于 Servlet)
    *   灵活性强
    
    这里所说的优点，就需要我们在使用的过程中慢慢体会。
    

### 1.3 所有 SpringMvc 注解整理

Controller 相关注解：

*   @Controller 标注类为控制器，处理 http 请求。
*   @RequestMapping 将请求路径映射到方法；
*   @RestController 相当于 @ResponseBody+@Controller；
*   @ResponseBody 将方法返回的对象相映成 JSON 格式；
*   @RequestBody 接收请求体 JSON 数据，并转为对象或对象数组。
*   @PathVariable 获取请求路径中的数据；
*   @RequestParam 根据 value 获取同名请求参数的值、获取集合类型参数；
*   @ControllerAdvice 统一处理异常；
*   @RestControllerAdvice 注解 Rest 风格统一处理异常；
*   @ExceptionHandler 声明捕获异常类型；

## 2，SpringMVC 入门案例

因为 SpringMVC 是一个 Web 框架，将来是要替换 Servlet, 所以先来回顾下以前 Servlet 是如何进行开发的?

1. 创建 web 工程 (Maven 结构)

2. 设置 tomcat 服务器，加载 web 工程 (tomcat 插件)

3. 导入坐标 (Servlet)

4. 定义处理请求的功能类 (UserServlet)

5. 设置请求映射 (配置映射关系)

SpringMVC 的制作过程和上述流程几乎是一致的，具体的实现流程是什么?

1. 创建 web 工程 (Maven 结构)

2. 设置 tomcat 服务器，加载 web 工程 (tomcat 插件)

3. 导入坐标 (**SpringMVC**+Servlet)

4. 定义处理请求的功能类 (**UserController**)

5. **设置请求映射 (配置映射关系)**

6. **将 SpringMVC 设定加载到 Tomcat 容器中**

### 2.1 需求分析

### 2.2 代码实现

**步骤 1: 创建 Maven 项目**

打开 IDEA, 创建一个新的 web 项目

![](<assets/1740015620108.png>)

**步骤 2: 补全目录结构**

因为使用骨架创建的项目结构不完整，需要手动补全

![](<assets/1740015621891.png>)

**步骤 3: 导入 jar 包**

将 pom.xml 中多余的内容删除掉，再添加依赖 spring-webmvc 和 javax.servlet-api

**无需再导入 spring-context，因为依赖 spring-webmvc 里包括了 spring-context.**

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.itheima</groupId>
  <artifactId>springmvc_01_quickstart</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>
 
  <dependencies>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.10.RELEASE</version>
    </dependency>
  </dependencies>
 
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.1</version>
        <configuration>
          <port>80</port>
          <path>/</path>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

**回顾: servlet 的坐标为什么需要添加`<scope>provided</scope>`?**

*   scope 是 maven 中 jar 包依赖作用范围的描述，
*   如果不设置默认是`compile`在在编译、运行、测试时均有效
*   如果运行有效的话就会和 tomcat 中的 servlet-api 包发生冲突，导致启动报错
    
*   provided 代表的是该包只在编译和测试的时候用，运行的时候无效直接使用 tomcat 中的，就避免冲突
    

**步骤 4: 创建配置类**

```
@Configuration
//扫描范围设为controller即可，因为springmvc容器只用管理表现层的bean即可，业务、数据层由spring管理
@ComponentScan("com.itheima.controller")
public class SpringMvcConfig {
}
```

**步骤 5: 创建 Controller 类**

```
//设置springmvc的核心控制器bean
@Controller
public class UserController {
    //设置当前控制器方法请求访问路径
    @RequestMapping("/save")
    //@ResponseBody设置当前控制器方法响应内容为当前返回值，无需解析
    @ResponseBody
    public String save(){
        System.out.println("user save ...");
        //如果不加@ResponseBody，系统会根据返回值字符串寻找路径，例如return "index.jsp";会跳转到jsp页面
        //返回真正json只需要返回对象，在pom引入jackson，springmvcconfig配置类注解@@EnableWebMvc，下面注释的语句就是返回JSON数据
        //return new User("zhangsan",23);
        return "{'info':'springmvc'}";
    }
}
```

*   当**方法上有 @ReponseBody** 注解后
    *   方法的**返回值为字符串**，会将其作为**文本内容**直接**响应给前端**
    *   方法的**返回值为对象**，会将对象**转换成 JSON 响应给前端**

**步骤 6: 使用 Servlet 容器配置类替换 web.xml**

**方法一：复杂底层**

将 web.xml 删除，换成 ServletContainersInitConfig，继承抽象转发 Servlet 初始化器类 **AbstractDispatcherServletInitializer**，**alt+insert 实现三个方法**。

```
public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer {
    //创建Servlet容器时，加载springmvc的配置。将springmvc对应的bean并放入WebApplicationContext对象范围中。
    //最终返回web容器WebApplicationContext ，web容器就是springmvc最终体现的容器。ApplicationContext应用上下文，容器
    protected WebApplicationContext createServletApplicationContext() {
        //创建注解配置的web容器。使用AnnotationConfigWebApplicationContext，比前面Spring创建注解容器的类AnnotationConfigApplicationContext中间多了Web
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        //加载springmvc的配置类字节码到web容器中，从而可以让web容器管理加载配置类管理的bean
        //因为AnnotationConfigWebApplicationContext类没有带参构造方法，所以要手动为容器加载指定配置类字节码文件
        ctx.register(SpringMvcConfig.class);
        return ctx;
    }
 
    //设置由springmvc拦截并处理的请求路径，即springmvc拦截哪些请求。
    //返回值String数组只有一个"/"，代表所有路径都由springmvc处理
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
 
    //加载spring配置类，方法和加载springmvc方法相同，只是加载的配置类改成spring的配置文件
    //如果创建Servlet容器时需要加载非表现层的bean,使用当前方法进行，使用方式和createServletApplicationContext相同。
    protected WebApplicationContext createRootApplicationContext() {
      AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        ctx.register(SpringConfig.class);
        return ctx;
    }
}
```

**方法二：简单快速**

**继承 Abstract**AnnotationConfig**DispatcherServletInitializer** ，跟之前抽象类外表区别是中间多了 AnnotationConfig

```
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
 
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }
 
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }
 
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```

**步骤 7: 配置 Tomcat 环境**

我这里用的是 Tomcat9，因为我 Servlet 和 springmvc 依赖都导入了最新的，和 Tomcat7 不兼容。

**步骤 8: 启动运行项目**

![](<assets/1740015623832.png>)

**步骤 9: 浏览器访问**

浏览器输入`http://localhost/save`进行访问

![](<assets/1740015625989.png>)

至此 SpringMVC 的入门案例就已经完成。

**注意事项**

*   **SpringMVC 是基于 Spring 的**，在 pom.xml **只导入了`spring-webmvc`**jar 包的原因是它会**自动依赖 spring 相关坐标**
*   **AbstractDispatcherServletInitializer** 类是 SpringMVC 提供的**快速初始化 Web3.0 容器**的抽象类
*   AbstractDispatcherServletInitializer 提供了**三个接口方法**供用户实现
    *   **createServletApplicationContext** 方法，创建 Servlet 容器时，加载 SpringMVC 对应的 bean 并放入 WebApplicationContext 对象范围中，而 WebApplicationContext 的作用范围为 ServletContext 范围，即整个 web 容器范围
    *   **getServletMappings** 方法，设定 SpringMVC 对应的请求映射路径，即 SpringMVC 拦截哪些请求
    *   **createRootApplicationContext** 方法，如果创建 Servlet 容器时需要加载非 SpringMVC 对应的 bean, 使用当前方法进行，使用方式和 createServletApplicationContext 相同。
    *   createServletApplicationContext 用来加载 SpringMVC 环境，createRootApplicationContext 用来加载 Spring 环境

### 知识点：@Controller@RequestMapping@ResponseBody

**知识点 1：@Controller**

<table><thead><tr><th>名称</th><th>@Controller</th></tr></thead><tbody><tr><td>类型</td><td>类注解</td></tr><tr><td>位置</td><td>SpringMVC 控制器类定义上方</td></tr><tr><td>作用</td><td>设定 SpringMVC 的核心控制器 bean</td></tr></tbody></table>

**知识点 2：@RequestMapping**

<table><thead><tr><th>名称</th><th>@RequestMapping</th></tr></thead><tbody><tr><td>类型</td><td>类注解或方法注解</td></tr><tr><td>位置</td><td>SpringMVC 控制器类或方法定义上方</td></tr><tr><td>作用</td><td>设置当前控制器方法请求访问路径</td></tr><tr><td>相关属性</td><td>value(默认)，请求访问路径</td></tr></tbody></table>

**知识点 3：@ResponseBody**

<table><thead><tr><th>名称</th><th>@ResponseBody</th></tr></thead><tbody><tr><td>类型</td><td>类注解或方法注解</td></tr><tr><td>位置</td><td>SpringMVC 控制器类或方法定义上方</td></tr><tr><td>作用</td><td><strong>设置当前控制器方法响应内容为当前返回值，无需解析</strong></td></tr></tbody></table>

### 2.3 案例总结

*   一次性工作
    *   创建工程，设置服务器，加载工程
    *   导入坐标
    *   创建 **web 容器启动类**，**加载 SpringMVC 配置**，并设置 SpringMVC **请求拦截路径**
    *   SpringMVC 核心配置类（设置配置类，扫描 controller 包，加载 Controller 控制器 bean）
*   多次工作
    *   定义处理请求的控制器类
    *   定义处理请求的控制器方法，并配置映射路径（@RequestMapping）与返回 json 数据（@ResponseBody）

### 2.4 工作流程解析

为了更好的使用 SpringMVC, 我们将 **SpringMVC 的使用过程**总共分**两个阶段**来分析，分别是**`启动服务器初始化过程`和`单次请求过程`**

**web 容器、Servletcontext、webApplicationContext、UserController 关系：** 

![](<assets/1740015628170.png>)

#### 2.4.1 启动服务器初始化过程

1.  服务器启动，执行 **ServletContainersInitConfig** 类，**初始化 web 容器**。功能类似于以前的 web.xml
    
2.  执行 **createServletApplicationContext** 方法，创建了 **WebApplicationContext** 对象。该方法加载 SpringMVC 的配置类 SpringMvcConfig 来**初始化 SpringMVC 的容器**
    
3.  加载 SpringMvcConfig 配置类
    
    ![](<assets/1740015629255.png>)
    
4.  执行 @**ComponentScan** 加载对应的 bean。扫描指定包及其子包下所有类上的注解，如 Controller 类上的 @Controller 注解
    
5.  加载 **UserController**，每个 @RequestMapping 的名称对应一个具体的方法。此时就建立了 `/save` 和 save 方法的对应关系
    
    ![](<assets/1740015629446.png>)
    

6. 执行 **getServletMappings** 方法，设定 SpringMVC 拦截请求的路径规则。`/`代表所拦截请求的路径规则，只有被拦截后才能交给 SpringMVC 来处理请求

![](<assets/1740015629709.png>)

#### 2.4.2 单次请求过程

1.  发送请求`http://localhost/save`
2.  web 容器发现该请求满足 SpringMVC 拦截规则，将请求交给 SpringMVC 处理
3.  解析请求路径 / save
4.  由 / save 匹配执行对应的方法 save(）
    *   上面的第五步已经将请求路径和方法建立了对应关系，通过 / save 就能找到对应的 save 方法
5.  **执行 save**()
6.  检测到有 @**ResponseBody** 直接将 save() 方法的返回值作为响应体返回给请求方

### 2.5 bean 加载控制，spring 排除加载表现层的 bean

#### 2.5.1 问题分析

**项目目录结构:**

![](<assets/1740015630064.png>)

**各目录存放内容：** 

*   **config** 目录存入的是配置类, 写过的配置类有:
    
    *   ServletContainersInitConfig
    *   SpringConfig
    *   SpringMvcConfig
    *   JdbcConfig
    *   MybatisConfig
*   **controller** 目录存放的是 SpringMVC 的 controller 类
*   **service** 目录存放的是 service 接口和实现类
*   **dao** 目录存放的是 dao/Mapper 接口

**问题：controller、service 和 dao 这些类都需要被容器管理成 bean 对象**，那么到底是该让 SpringMVC 加载还是让 Spring 加载呢?

**答案：**

*   **SpringMVC** 加载其相关 bean(表现层 bean), 也就是 **controller 包下的类**
*   **Spring 控制的 bean**
    *   **业务** bean(Service)
    *   **功能** bean(DataSource,SqlSessionFactoryBean,MapperScannerConfigurer 等)

分析清楚谁该管哪些 bean 以后，接下来要解决的问题是如何让 Spring 和 SpringMVC 分开加载各自的内容。

在 **SpringMVC** 的配置类**`SpringMvcConfig`**中使用注解**`@ComponentScan`**，我们**只需要将其扫描范围**设置到 **controller** 即可，如

![](<assets/1740015630389.png>)

在 Spring 的配置类**`SpringConfig`**中使用注解**`@ComponentScan`**, 当时扫描的范围中其实是已经包含了 controller, 如:

![](<assets/1740015630652.png>)

**现在的问题：**

**因为功能不同，如何避免 Spring 错误加载到 SpringMVC 的 bean?**

 **答案：**

加载 **Spring** 控制的 bean 的时候**排除掉 SpringMVC 控制的 bean**。Spring 加载的 bean 设定扫描范围为 com.itheima, 排除掉 controller 包中的 bean

#### 2.5.2 思路分析

针对上面的问题，解决方案也比较简单，就是:

*   加载 Spring 控制的 bean 的时候排除掉 SpringMVC 控制的 bean

具体该如何排除：

*   方式一: Spring 加载的 bean 设定扫描范围为精准范围，例如 service 包、dao 包等
*   方式二: Spring 加载的 bean 设定扫描范围为 com.itheima, 排除掉 controller 包中的 bean
*   方式三: 不区分 Spring 与 SpringMVC 的环境，加载到同一个环境中 [了解即可]

#### 2.5.3 环境准备

*   创建一个 Web 的 Maven 项目
    
*   pom.xml 添加 Spring 依赖 springmvc 相关 javax.servlet-api,spring-webmvc（里面包括了 spring-context），数据库相关 mysql-java-connector,spring-jdbc,druid,mybatis,mybatis-spring
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
     
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
     
      <groupId>com.itheima</groupId>
      <artifactId>springmvc_02_bean_load</artifactId>
      <version>1.0-SNAPSHOT</version>
      <packaging>war</packaging>
     
      <dependencies>
        <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>javax.servlet-api</artifactId>
          <version>3.1.0</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>5.2.10.RELEASE</version>
        </dependency>
        <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>druid</artifactId>
          <version>1.1.16</version>
        </dependency>
     
        <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis</artifactId>
          <version>3.5.6</version>
        </dependency>
     
        <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>5.1.47</version>
        </dependency>
     
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-jdbc</artifactId>
          <version>5.2.10.RELEASE</version>
        </dependency>
     
        <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis-spring</artifactId>
          <version>1.3.0</version>
        </dependency>
      </dependencies>
     
      <build>
        <plugins>
          <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.1</version>
            <configuration>
              <port>80</port>
              <path>/</path>
            </configuration>
          </plugin>
        </plugins>
      </build>
    </project>
    ```
    
*   创建 servlet 配置类
    
    ```
    public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer {
        protected WebApplicationContext createServletApplicationContext() {
            AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
            ctx.register(SpringMvcConfig.class);
            return ctx;
        }
        protected String[] getServletMappings() {
            return new String[]{"/"};
        }
        protected WebApplicationContext createRootApplicationContext() {
          return null;
        }
    }
     
    @Configuration
    @ComponentScan("com.itheima.controller")
    public class SpringMvcConfig {
    }
     
    @Configuration
    @ComponentScan("com.itheima")
    public class SpringConfig {
    }
    ```
    
*   编写 Controller，Service，Dao，Domain 类
    
    ```
    @Controller
    public class UserController {
     
        @RequestMapping("/save")
        @ResponseBody
        public String save(){
            System.out.println("user save ...");
            return "{'info':'springmvc'}";
        }
    }
     
    public interface UserService {
        public void save(User user);
    }
     
    @Service
    public class UserServiceImpl implements UserService {
        public void save(User user) {
            System.out.println("user service ...");
        }
    }
     
    public interface UserDao {
        @Insert("insert into tbl_user(name,age)values(#{name},#{age})")
        public void save(User user);
    }
    public class User {
        private Integer id;
        private String name;
        private Integer age;
        //setter..getter..toString略
    }
    ```
    

最终创建好的项目结构如下:

![](<assets/1740015630839.png>)

#### 2.5.5 设置 bean 加载控制

**方式一:** 修改 **Spring** 配置类，设定**扫描范围为精准**范围。

```
@Configuration
@ComponentScan({"com.itheima.service","comitheima.dao"})
public class SpringConfig {
}
```

**说明:**

上述只是通过例子说明可以精确指定让 Spring 扫描对应的包结构，真正在做开发的时候，因为 Dao 最终是交给`MapperScannerConfigurer`对象来进行扫描处理的，我们只需要将其扫描到 service 包即可。

**方式二:** 修改 **Spring** 配置类，设定扫描范围为 com.itheima, **排除掉 controller 包中的 bean**

@ComponentScan 的 excludeFilters 属性。

```
@Configuration
@ComponentScan(value="com.itheima",
    excludeFilters=@ComponentScan.Filter(
        type = FilterType.ANNOTATION,
        classes = Controller.class
    )
)
public class SpringConfig {
}
```

*   **excludeFilters 属性**：设置扫描加载 bean 时，排除的**过滤规则**
    
*   **type 属性**：设置**排除规则**，当前使用按照 bean 定义时的注解类型进行排除
    
    *   **ANNOTATION**：**按照注解排除**
    *   ASSIGNABLE_TYPE: 按照指定的类型过滤
    *   ASPECTJ: 按照 Aspectj 表达式排除，基本上不会用
    *   **REGEX**: 按照**正则表达式**排除
    *   CUSTOM: 按照自定义规则排除
    
    大家**只需要知道第一种 ANNOTATION 即可**
    
*   **classes 属性：**设置**排除的具体注解类**，当前设置排除 @Controller 定义的 bean
    

**测试 controller 类是否被排除掉了：创建 spring 容器获取表现层 bean**

```
public class App{
    public static void main (String[] args){
        AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        System.out.println(ctx.getBean(UserController.class));
    }
}
```

如果被排除了，该方法执行就会报 bean 未被定义的错误

![](<assets/1740015631012.png>)

**注意: 测试**的时候，需要把 **SpringMvcConfig** 配置类上的 **@ComponentScan 注解注释**掉，否则不会报错

出现问题的原因是，

*   Spring 配置类扫描的包是`com.itheima`
*   SpringMVC 的配置类，**`SpringMvcConfig`**上有一个 **@Configuration 注解**，也**会被 Spring 扫描到**
*   SpringMvcConfig 上又有一个 @ComponentScan，把 controller 类又给扫描进来了
*   所以如果不把 @ComponentScan 注释掉，Spring 配置类将 Controller 排除，但是因为扫描到 SpringMVC 的配置类，又将其加载回来，演示的效果就出不来
*   解决方案，也简单，把 SpringMVC 的配置类移出 Spring 配置类的扫描范围即可。

**Servlet 容器配置类加载 Spring 配置类：** 

最后一个问题，有了 Spring 的配置类，要想在 tomcat 服务器启动将其加载，我们需要**修改 ServletContainersInitConfig 的 createRootApplicationContext 方法**

```
public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer {
    protected WebApplicationContext createServletApplicationContext() {
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        ctx.register(SpringMvcConfig.class);
        return ctx;
    }
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
    protected WebApplicationContext createRootApplicationContext() {
      AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        ctx.register(SpringConfig.class);
        return ctx;
    }
}
```

**Servlet 容器配置类简单方法：**

对于上述的配置方式，Spring 还提供了一种更简单的配置方式，可以不用再去创建`AnnotationConfigWebApplicationContext`对象，不用手动`register`对应的配置类。

```
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
 
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }
 
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }
 
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```

#### 知识点 @ComponentScan 的 excludeFilters

<table><thead><tr><th>名称</th><th>@ComponentScan</th></tr></thead><tbody><tr><td>类型</td><td>类注解</td></tr><tr><td>位置</td><td>类定义上方</td></tr><tr><td>作用</td><td>设置 spring 配置类扫描路径，用于加载使用注解格式定义的 bean</td></tr><tr><td>相关属性</td><td>excludeFilters: 排除扫描路径中加载的 bean, 需要指定类别 (type) 和具体项(classes)<br>includeFilters: 加载指定的 bean，需要指定类别 (type) 和具体项(classes)</td></tr></tbody></table>

## 3，PostMan 工具的使用

### 3.1 PostMan 简介

代码编写完后，我们要想测试，只需要打开浏览器直接输入地址发送请求即可。发送的是`GET`请求可以直接使用浏览器，但是如果要发送的是`POST`请求呢?

**如果要求发送的是 post 请求**，我们就得准备页面在页面上准备 form 表单，测试起来比较**麻烦**。所以我们就需要**借助一些第三方工具，如 PostMan.**

*   **PostMan** 是一款功能强大的**网页调试与发送网页 HTTP 请求的 Chrome 插件**。
    
    ![](<assets/1740015631274.png>)
    
*   **作用：常用于进行接口测试，模拟浏览器发送请求**
    
*   特征
    
    *   简单
    *   实用
    *   美观
    *   大方

### 3.2 PostMan 安装

**下载地址：**

[Download Postman | Get Started for Free](https://www.postman.com/downloads/ "Download Postman | Get Started for Free")

**9.1.2 版本下载地址：**

[https://dl.pstmn.io/download/version/9.12.2/win64](https://dl.pstmn.io/download/version/9.12.2/win64 "https://dl.pstmn.io/download/version/9.12.2/win64")

**9.1.2 汉化包地址：**

[Releases · hlmd/Postman-cn · GitHub](https://github.com/hlmd/Postman-cn/releases?page=1 "Releases · hlmd/Postman-cn · GitHub")

![](<assets/1740015631482.png>)

双击`Postman-win64-8.3.1-Setup.exe`即可自动安装，

汉化包：[Releases · hlmd/Postman-cn · GitHub](https://github.com/hlmd/Postman-cn/releases "Releases · hlmd/Postman-cn · GitHub") 

安装汉化包后解压到 postman 的 app-resources 目录下，再次打开就已经汉化成功。

![](<assets/1740015631734.png>)

安装完成后，如果需要注册，可以按照提示进行注册，如果底部有跳过测试的链接也可以点击跳过注册

![](<assets/1740015631936.png>)

看到如下界面，就说明已经安装成功。

![](<assets/1740015632136.png>)

### 3.3 PostMan 使用

#### 3.3.1 创建 WorkSpace 工作空间

工作空间是可以云备份的。 

![](<assets/1740015632389.png>)

#### 3.3.2 发送请求

点击 + 号新建请求：

![](<assets/1740015632609.png>)

![](<assets/1740015632830.png>)

#### 3.3.3 保存当前请求

![](<assets/1740015633032.png>)

**注意:** 第一次请求需要创建一个新的集合目录，后面就不需要创建新目录，直接保存到已经创建好的目录即可。

## 4，请求与响应

之前我们提到过，**SpringMVC 是 web 层的框架**，**主要的作用**是**接收请求、接收数据、响应结果**，所以这一章节是学习 SpringMVC 的**重点**内容，我们主要会讲解四部分内容:

*   请求映射路径
*   请求参数
*   日期类型参数传递
*   响应 json 数据

### 4.1 设置请求映射路径

#### 4.1.1 环境准备

*   创建一个 Web 的 Maven 项目
    
*   pom.xml 添加 Spring 依赖 javax.servlet,spring-webmvc
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
     
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
     
      <groupId>com.itheima</groupId>
      <artifactId>springmvc_03_request_mapping</artifactId>
      <version>1.0-SNAPSHOT</version>
      <packaging>war</packaging>
     
      <dependencies>
        <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>javax.servlet-api</artifactId>
          <version>3.1.0</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>5.2.10.RELEASE</version>
        </dependency>
      </dependencies>
     
      <build>
        <plugins>
          <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.1</version>
            <configuration>
              <port>80</port>
              <path>/</path>
            </configuration>
          </plugin>
        </plugins>
      </build>
    </project>
    ```
    
*   创建对应的配置类
    
    ```
    public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
     
        protected Class<?>[] getServletConfigClasses() {
            return new Class[]{SpringMvcConfig.class};
        }
        protected String[] getServletMappings() {
            return new String[]{"/"};
        }
        protected Class<?>[] getRootConfigClasses() {
            return new Class[0];
        }
    }
     
    @Configuration
    @ComponentScan("com.itheima.controller")
    public class SpringMvcConfig {
    }
    ```
    
*   编写 **BookController 和 UserController**，**都有一个 / save 路径**
    
    ```
    @Controller
    public class UserController {
     
        @RequestMapping("/save")
        @ResponseBody
        public String save(){
            System.out.println("user save ...");
            return "{'module':'user save'}";
        }
     
        @RequestMapping("/delete")
        @ResponseBody
        public String save(){
            System.out.println("user delete ...");
            return "{'module':'user delete'}";
        }
    }
     
    @Controller
    public class BookController {
     
        @RequestMapping("/save")
        @ResponseBody
        public String save(){
            System.out.println("book save ...");
            return "{'module':'book save'}";
        }
    }
    ```
    

项目结构如下:

![](<assets/1740015633315.png>)

#### 4.1.2 请求冲突问题分析

因为 **BookController 和 UserController**，**都有一个 / save 路径**，启动 Tomcat 服务器，后台会报错:

![](<assets/1740015633496.png>)

从错误信息可以看出:

*   UserController 有一个 save 方法，访问路径为`http://localhost/save`
*   BookController 也有一个 save 方法，访问路径为`http://localhost/save`
*   当访问`http://localhost/saved`的时候，到底是访问 UserController 还是 BookController?

**问题：团队多人开发，每人设置不同的请求路径，冲突问题该如何解决?**

**解决思路:** 为不同模块设置**模块名作为请求路径前置**

对于 Book 模块的 save, 将其访问路径设置`http://localhost/book/save`

对于 User 模块的 save, 将其访问路径设置`http://localhost/user/save`

这样在同一个模块中出现命名冲突的情况就比较少了。

#### 4.1.3 设置映射路径

**方法一: 笨方法，直接修改 Controller 方法上的 @RequestMapping 值**

```
@Controller
public class UserController {
 
    @RequestMapping("/user/save")
    @ResponseBody
    public String save(){
        System.out.println("user save ...");
        return "{'module':'user save'}";
    }
 
    @RequestMapping("/user/delete")
    @ResponseBody
    public String save(){
        System.out.println("user delete ...");
        return "{'module':'user delete'}";
    }
}
 
@Controller
public class BookController {
 
    @RequestMapping("/book/save")
    @ResponseBody
    public String save(){
        System.out.println("book save ...");
        return "{'module':'book save'}";
    }
}
```

问题是解决了，但是每个方法前面都需要进行修改，写起来比较麻烦而且还有很多重复代码，如果 / user 后期发生变化，所有的方法都需要改，**耦合度太高**。

**方法二: 优化版，给 controller 类另外配置 @RequestMapping，值为模块名**

**优化方案:**

```
@Controller
@RequestMapping("/user")
public class UserController {
 
    @RequestMapping("/save")
    @ResponseBody
    public String save(){
        System.out.println("user save ...");
        return "{'module':'user save'}";
    }
 
    @RequestMapping("/delete")
    @ResponseBody
    public String save(){
        System.out.println("user delete ...");
        return "{'module':'user delete'}";
    }
}
 
@Controller
@RequestMapping("/book")
public class BookController {
 
    @RequestMapping("/save")
    @ResponseBody
    public String save(){
        System.out.println("book save ...");
        return "{'module':'book save'}";
    }
}
```

**注意:**

*   当**类上和方法上都添加了`@RequestMapping`注解**，前端发送请求的时候，要和两个注解的 value 值相加匹配才能访问到。
*   @RequestMapping 注解 value 属性前面**加不加`/`都可以**

扩展小知识: 对于 PostMan 如何觉得字小不好看，可以使用`ctrl+=`调大，`ctrl+-`调小。

### 4.2 普通请求参数传递入门

了解即可，更多用 JSON 传数据。 

请求路径设置好后，只要确保页面发送请求地址和后台 Controller 类中配置的路径一致，就可以接收到前端的请求，接收到请求后，**如何接收页面传递的参数?**

关于请求参数的传递与接收是和请求方式有关系的，目前比较常见的两种请求方式为：

*   **GET**
*   **POST**

#### 4.2.1 环境准备

*   创建一个 Web 的 Maven 项目
    
*   pom.xml 添加 Spring 依赖 javax.servlet,spring-webmvc
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
     
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
     
      <groupId>com.itheima</groupId>
      <artifactId>springmvc_03_request_mapping</artifactId>
      <version>1.0-SNAPSHOT</version>
      <packaging>war</packaging>
     
      <dependencies>
        <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>javax.servlet-api</artifactId>
          <version>3.1.0</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>5.2.10.RELEASE</version>
        </dependency>
      </dependencies>
     
      <build>
        <plugins>
          <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.1</version>
            <configuration>
              <port>80</port>
              <path>/</path>
            </configuration>
          </plugin>
        </plugins>
      </build>
    </project>
    ```
    
*   创建对应的配置类
    
    ```
    public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
     
        protected Class<?>[] getServletConfigClasses() {
            return new Class[]{SpringMvcConfig.class};
        }
        protected String[] getServletMappings() {
            return new String[]{"/"};
        }
        protected Class<?>[] getRootConfigClasses() {
            return new Class[0];
        }
    }
     
    @Configuration
    @ComponentScan("com.itheima.controller")
    public class SpringMvcConfig {
    }
    ```
    
*   **编写 UserController**
    
    ```
    @Controller
    public class UserController {
     
        @RequestMapping("/commonParam")
        @ResponseBody
        public String commonParam(){
            return "{'module':'commonParam'}";
        }
    }
    ```
    
*   **编写模型类，User 和 Address**
    
    ```
    public class Address {
        private String province;
        private String city;
        //setter...getter...略
    }
    public class User {
        private String name;
        private int age;
        //setter...getter...略
    }
    ```
    

最终创建好的项目结构如下:

![](<assets/1740015633749.png>)

#### 4.2.2 请求参数传递和中文乱码解决

**中文乱码问题：**

*   不管是 **GET 还是 POST**，controller **代码都是一样的**，**形参传递**，不作区分。只是对于**中文乱码解决不同**，get 是在 Tomcat 插件里配置编码 utf-8，post 是 **Servlet 容器配置类**使用**过滤器**设置编码。
*   **json 格式**传递**无中文乱码问题**，注意参数要加 @RequestBody，返回值类型是对象和 List <类>。return "{'save':'success'}" ; 返回的是普通类型 String。

**GET 发送单个参数**

```
http://localhost/commonParam?name=itcast


```

![](<assets/1740015634054.png>)

**GET 接收单个参数：**

controller 里方法**形参名必须和请求参数名一致**：

```
@Controller
public class UserController {
 
    @RequestMapping("/commonParam")
    @ResponseBody
    public String commonParam(String name){
        System.out.println("普通参数传递 name ==> "+name);
        return "{'module':'commonParam'}";
    }
}
```

**GET 发送多个参数**

```
http://localhost/commonParam?name=itcast&age=15


```

![](<assets/1740015634423.png>)

**GET 接收多个参数：**

```
@Controller
public class UserController {
 
    @RequestMapping("/commonParam")
    @ResponseBody
    public String commonParam(String name,int age){
        System.out.println("普通参数传递 name ==> "+name);
        System.out.println("普通参数传递 age ==> "+age);
        return "{'module':'commonParam'}";
    }
}
```

**GET 请求中文乱码**

如果 Tomcat8.5 之前版本，我们传递的参数中有中文，你会发现接收到的参数会出现中文乱码问题。

发送请求:`http://localhost/commonParam?name=张三&age=18`

控制台:

![](<assets/1740015634688.png>)

**解决乱码：**

**Tomcat8.5 以后的版本已经处理了中文乱码的问题**，但是 IDEA 中的 Tomcat 插件目前只到 Tomcat7，所以需要修改 pom.xml 来解决 GET 请求中文乱码问题

```
<build>
    <plugins>
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.1</version>
        <configuration>
          <port>80</port><!--tomcat端口号-->
          <path>/</path> <!--虚拟目录-->
          <uriEncoding>UTF-8</uriEncoding><!--访问路径编解码字符集-->
        </configuration>
      </plugin>
    </plugins>
  </build>
```

**POST 发送参数**

*   一定别忘了地址栏左边选择 POST 请求，不然会 500 报错。
*   选择 x-www-form-urlencoded，对比之下，form-data 表单是既能发表单，又能发文件

![](<assets/1740015635063.png>)

**POST 接收参数：**

和 GET 一致，不用做任何修改

```
@Controller
public class UserController {
 
    @RequestMapping("/commonParam")
    @ResponseBody
    public String commonParam(String name,int age){
        System.out.println("普通参数传递 name ==> "+name);
        System.out.println("普通参数传递 age ==> "+age);
        return "{'module':'commonParam'}";
    }
}
```

**POST 请求中文乱码：所有 Tomcat 版本都中文乱码**

发送请求与参数:

![](<assets/1740015635349.png>)

接收参数:

控制台打印，会发现有中文乱码问题。

![](<assets/1740015635639.png>)

**解决方案: 配置过滤器**

在 **Servlet 容器配置类**里，alt+insert **重写 getServletFilters** 过滤器方法： 

```
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
    protected Class<?>[] getRootConfigClasses() {
        return new Class[0];
    }
 
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }
 
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
 
    //重写getServletFilters过滤器方法，返回数组传入CharacterEncodingFilter过滤器对象，乱码处理
    @Override
    protected Filter[] getServletFilters() {
        //创建字符编码过滤器CharacterEncodingFilter对象
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        //设置编码
        filter.setEncoding("UTF-8");
        //返回过滤器数组，元素为CharacterEncodingFilter对象
        return new Filter[]{filter};
    }
}
```

**注意：**

*   **过滤器方法只适用于 post** 请求方式的中文乱码，get 中文乱码还是要 Tomcat8 以上，或 Tomcat7 的 pom 里设置 Tomcat 插件编码
*   CharacterEncodingFilter 是在 spring-webmvc 包中，所以用之前要确保导入对应的 jar 包。

### 4.3 请求头的五种类型参数传递

前面我们已经能够使用 GET 或 POST 来发送请求和数据，所携带的数据都是比较简单的数据，接下来在这个基础上，我们来研究一些比较复杂的参数传递，常见的参数种类有:

*   **普通参数**
*   **POJO 类型参数**
*   **嵌套 POJO 类型参数**
*   **数组类型参数**
*   **集合类型参数**

#### 4.3.1 普通请求参数，起别名，**@RequestParam**

*   普通参数: url 地址传参，地址参数名与形参变量名相同，定义形参即可接收参数。

![](<assets/1740015635909.png>)

**问题：如果形参与地址参数名不一致该如何解决?**

**解决方案: 使用 @RequestParam 注解，用法跟 dao 层传参 @Param 类似** 

发送请求与参数:

```
http://localhost/commonParamDifferentName?name=张三&age=18


```

后台接收参数:

```
@RequestMapping("/commonParamDifferentName")
@ResponseBody
public String commonParamDifferentName(String userName , int age){
    System.out.println("普通参数传递 userName ==> "+userName);
    System.out.println("普通参数传递 age ==> "+age);
    return "{'module':'common param different name'}";
}
```

因为前端给的是`name`, 后台接收使用的是`userName`, 两个名称对不上，导致接收数据失败:

![](<assets/1740015636095.png>)

**解决方案: 使用 @RequestParam 注解，用法跟 dao 层传参 @Param 类似**

```
@RequestMapping("/commonParamDifferentName")
    @ResponseBody
    public String commonParamDifferentName(@RequestParam("name") String userName , int age){
        System.out.println("普通参数传递 userName ==> "+userName);
        System.out.println("普通参数传递 age ==> "+age);
        return "{'module':'common param different name'}";
    }
```

**注意: 写上 @RequestParam 注解框架就不需要自己去解析注入，能提升框架处理性能**

#### 4.3.2 POJO 数据类型

适用于参数多的情况，将参数封装成一个实体类中。 

简单数据类型一般处理的是参数个数比较少的请求，如果参数比较多，那么后台接收参数的时候就比较复杂，这个时候我们可以考虑使用 POJO 实体类数据类型。

*   **POJO 实体类参数：请求参数名与形参对象的属性名相同**，定义 POJO 类型形参即可接收参数

此时需要使用前面准备好的 POJO 类，先来看下 User

```
public class User {
    private String name;
    private int age;
    //setter...getter...略
}
```

发送请求和参数:

![](<assets/1740015636309.png>)

后台接收参数:

```
//POJO参数：请求参数与形参对象中的属性对应即可完成参数传递
@RequestMapping("/pojoParam")
@ResponseBody
public String pojoParam(User user){
    System.out.println("pojo参数传递 user ==> "+user);
    return "{'module':'pojo param'}";
}
```

**注意:**

*   POJO 参数接收，前端 GET 和 POST 发送请求数据的方式不变。
*   **请求参数 key 的名称要和 POJO 中属性的名称一致，否则无法封装。**

#### 4.3.3 嵌套 POJO 类型参数

如果 POJO 对象中嵌套了其他的 POJO 类，如

```
public class Address {
    private String province;
    private String city;
    //setter...getter...略
}
public class User {
    private String name;
    private int age;
    private Address address;
    //setter...getter...略
}
```

*   **嵌套 POJO 参数：请求参数名与形参对象属性名相同**，注意内嵌的 pojo 类在发送请求时参数要设成**内嵌类对象. 属性名**，**形参**依然是传**外层类的对象**，按照对象层次结构关系即可接收嵌套 POJO 属性参数

发送请求和参数:

![](<assets/1740015636500.png>)

后台接收参数:

```
//POJO参数嵌套：请求参数与形参对象中的属性对应即可完成参数传递
@RequestMapping("/pojoParam")
@ResponseBody
public String pojoParam(User user){
    System.out.println("pojo参数传递 user ==> "+user);
    return "{'module':'pojo param'}";
}
```

**注意:**

**请求参数 key 的名称要和 POJO 中属性的名称一致，否则无法封装**

#### 4.3.4 数组类型参数

举个简单的例子，如果前端需要获取用户的爱好，爱好绝大多数情况下都是多个，如何发送请求数据和接收数据呢?

*   **数组参数：请求参数名与形参数组名相同，且请求参数为多个**，定义数组类型即可接收参数

发送请求和参数:

![](<assets/1740015636803.png>)

后台接收参数:

```
  //数组参数：同名请求参数可以直接映射到对应名称的形参数组对象中
    @RequestMapping("/arrayParam")
    @ResponseBody
    public String arrayParam(String[] likes){
        System.out.println("数组参数传递 likes ==> "+ Arrays.toString(likes));
        return "{'module':'array param'}";
    }
```

#### 4.3.5 集合类型参数，**`@RequestParam`**

**结论：**请求参数跟上面数组一样，controller 形参为**`@RequestParam`**注解的同名集合对象。

**集合直接做形参会报错，报错演示：**

发送请求和参数:

![](<assets/1740015637039.png>)

后台接收参数:

```
//集合参数：同名请求参数可以使用@RequestParam注解映射到对应名称的集合对象中作为数据
@RequestMapping("/listParam")
@ResponseBody
public String listParam(List<String> likes){
    System.out.println("集合参数传递 likes ==> "+ likes);
    return "{'module':'list param'}";
}
```

运行会报错，

![](<assets/1740015637282.png>)

**错误的原因是:**SpringMVC 将 **List 看做**是一个 **POJO 对象**来处理，将其创建一个对象并准备把前端的数据封装到对象中，但是 List 是一个接口无法创建对象，所以报错。

**解决方案是: 使用`@RequestParam`注解绑定参数关系**

**使用`@RequestParam`注解绑定参数关系**

```
//集合参数：同名请求参数可以使用@RequestParam注解映射到对应名称的集合对象中作为数据
@RequestMapping("/listParam")
@ResponseBody
public String listParam(@RequestParam List<String> likes){
    System.out.println("集合参数传递 likes ==> "+ likes);
    return "{'module':'list param'}";
}
```

**注意：**

*   **集合保存普通参数：请求参数名与形参集合对象名相同**且请求参数为多个，@RequestParam 绑定参数关系
*   对于简单数据类型使用**数组会比集合更简单**些。

#### 知识点 @RequestParam

<table><thead><tr><th>名称</th><th>@RequestParam</th></tr></thead><tbody><tr><td>类型</td><td>形参注解</td></tr><tr><td>位置</td><td>SpringMVC 控制器方法形参定义前面</td></tr><tr><td>作用</td><td>绑定请求参数与处理器方法形参间的关系</td></tr><tr><td>相关参数</td><td>required：是否为必传参数<br>defaultValue：参数默认值</td></tr></tbody></table>

### 4.4 请求体的 JSON 数据传输参数，**@RequestBody**

#### 4.4.1 概述 

**@RequestBody 接收请求体 JSON 数据，并转为对象或对象数组。**

前面我们说过，现在比较流行的开发方式为**异步调用**。前后台以异步方式进行交换，传输的数据使用的是 **JSON**, 所以前端如果发送的是 JSON 数据，后端该如何接收?

对于 **JSON 数据类型**，我们常见的有三种:

*   **json 普通数组：**  ["value1","value2","value3",...]
*   **json 对象：** {key1:value1,key2:value2,...}
*   **json 对象数组：** [{key1:value1,...},{key2:value2,...}]

使用 josn 数据传输参数要先导入 **Jackson 依赖**和 **@EnableWebMvc 注解**

 **pom.xml 添加依赖 jackson-databind**

SpringMVC 默认使用的是 **jackson 来处理 json 的转换**，所以需要在 pom.xml 添加 jackson 依赖

```
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.0</version>
</dependency>
```

 **开启 SpringMVC 注解支持 @EnableWebMvc**

在 SpringMVC 的配置类中开启 SpringMVC 的注解支持 **@EnableWebMvc**，这个注解功能很强大，里面**包含了将 JSON 转换成对象的功能**。

```
@Configuration
@ComponentScan("com.itheima.controller")
//开启json数据类型自动转换
@EnableWebMvc
public class SpringMvcConfig {
}
```

#### 4.4.2 JSON 普通数组，**@EnableWebMvc**

 剧透，后面使用 springboot 和 Rest 风格后，给 controller 注解 @RestController 后就不用导入 Jackson、@EnableWebMvc，直接参数前 @RequestBody 即可

**controller 接收前端请求头的 JSON 数组并转为 List：**

**步骤 1:pom.xml 添加依赖 jackson-databind**

SpringMVC 默认使用的是 **jackson 来处理 json 的转换**，所以需要在 pom.xml 添加 jackson 依赖

```
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.0</version>
</dependency>
```

**步骤 2:PostMan 发送 JSON 数据**

![](<assets/1740015637487.png>)

**步骤 3: 开启 SpringMVC 注解支持 @EnableWebMvc**

在 SpringMVC 的配置类中开启 SpringMVC 的注解支持，这里面就包含了将 JSON 转换成对象的功能。

```
@Configuration
@ComponentScan("com.itheima.controller")
//开启json数据类型自动转换
@EnableWebMvc
public class SpringMvcConfig {
}
```

**步骤 4:controller 形参前添加 @RequestBody**

```
 
@RequestMapping("/listParamForJson")
@ResponseBody
//使用@RequestBody注解将前端请求体传递的json数组数据，传递并转换到形参的集合对象中作为数据
public String listParamForJson(@RequestBody List<String> likes){
    System.out.println("list common(json)参数传递 list ==> "+likes);
    return "{'module':'list common for json param'}";
}
```

**步骤 5: 启动运行程序**

![](<assets/1740015637656.png>)

#### 4.4.3 JSON 对象数据

 剧透，后面使用 springboot 和 Rest 风格后，给 controller 注解 @RestController 后就不用导入 Jackson、@EnableWebMvc，直接参数前 @RequestBody 即可

**回顾：**

**Servlet 接收 JSON 对象：**

1.Servlet 的 getParameter 获取 String 形式的 json 对象

2.fastjson 的 JSON.parseObject 方法将 json 对象转为实体类对象

**Servlet 响应 JSON 对象到前端：**

1. 将实体类对象通过 fastjson 的 JSON.toJSONString 方法转为 String 类型

2.response.getWriter().write(jsonStr)

**controller 接收前端 JSON 对象并转为实体类对象：**

**前端发送 JSON 对象数据:**

```
{
    "name":"itcast",
    "age":15
}
```

![](<assets/1740015638027.png>)

**controller 接收 JSON 对象数据：** 

带 @RequestBody 实体类对象的形参直接转换 JSON 对象

```
@RequestMapping("/pojoParamForJson")
@ResponseBody
public String pojoParamForJson(@RequestBody User user){
    System.out.println("pojo(json)参数传递 user ==> "+user);
    return "{'module':'pojo for json param'}";
}
```

 **记得开启 SpringMVC 注解 @EnableWebMvc**

在 SpringMVC 的配置类中开启 SpringMVC 的注解支持，这里面就包含了**将 JSON 转换成对象**的功能。

```
@Configuration
@ComponentScan("com.itheima.controller")
//开启json数据类型自动转换
@EnableWebMvc
public class SpringMvcConfig {
}
```

启动程序访问测试

![](<assets/1740015638243.png>)

**说明:**

address 为 null 的原因是前端没有传递数据给后端。

如果想要 address 也有数据，我们需求修改前端传递的数据内容:

```
{
    "name":"itcast",
    "age":15,
    "address":{
        "province":"beijing",
        "city":"beijing"
    }
}
```

再次发送请求，就能看到 address 中的数据

![](<assets/1740015638432.png>)

#### 4.4.4 JSON 对象数组

 剧透，后面使用 springboot 和 Rest 风格后，给 controller 注解 @RestController 后就不用导入 Jackson、@EnableWebMvc，直接参数前 @RequestBody 即可 

**controller 接收前端 JSON 数组并转为 List：**

前端发送 JSON 对象数组:

```
[
    {"name":"itcast","age":15},
    {"name":"itheima","age":12}
]
```

![](<assets/1740015638959.png>)

后端接收数据:

```
@RequestMapping("/listPojoParamForJson")
@ResponseBody
public String listPojoParamForJson(@RequestBody List<User> list){
    System.out.println("list pojo(json)参数传递 list ==> "+list);
    return "{'module':'list pojo for json param'}";
}
```

启动程序访问测试

![](<assets/1740015639191.png>)

**小结**

SpringMVC 接收 JSON 数据的实现步骤为:

(1) 导入 jackson 包

(2) 使用 PostMan 发送 JSON 数据

(3) 开启 SpringMVC 注解驱动，在配置类上添加 @EnableWebMvc 注解

(4)Controller 方法的参数前添加 @RequestBody 注解

#### 知识点 @EnableWebMvc@RequestBody

**知识点 1：@EnableWebMvc**

<table><thead><tr><th>名称</th><th>@EnableWebMvc</th></tr></thead><tbody><tr><td>类型</td><td><strong>配置类注解</strong></td></tr><tr><td>位置</td><td>SpringMVC 配置类定义上方</td></tr><tr><td>作用</td><td>开启 SpringMVC 多项辅助功能</td></tr></tbody></table>

**知识点 2：@RequestBody**

<table><thead><tr><th>名称</th><th>@RequestBody</th></tr></thead><tbody><tr><td>类型</td><td><strong>形参注解</strong></td></tr><tr><td>位置</td><td>SpringMVC 控制器方法形参定义前面</td></tr><tr><td>作用</td><td>将请求中请求体所包含的数据传递给请求参数，此注解一个处理器方法只能使用一次</td></tr></tbody></table>

#### **4.4.5 @RequestBody 与 @RequestParam 区别**

*   **区别**
    
    *   @RequestParam 用于集合类型传参，用于起别名【application/x-www-form-urlencoded】
    *   @RequestBody 用于接收 json 数据【application/json】
*   **应用**
    
    *   后期开发中，发送 json 格式数据为主，@RequestBody 应用较广
    *   如果发送非 json 格式数据，选用 @RequestParam 接收请求参数

#### **4.4.5** @RequestParam 和 @RequestPart 的区别

当请求头中指定 Content-Type:multipart/form-data 时，传递的 json 参数，@RequestPart 注解可以用对象来接收，@RequestParam 只能用字符串接收

*   `@RequestPart`这个注解用在`multipart/form-data`表单提交请求的方法上。
*   支持的请求方法的方式`MultipartFile`，属于 Spring 的`MultipartResolver`类。这个请求是通过`http协议`传输的

### 4.5 日期类型参数传递 @DateTimeFormat

前面我们处理过简单数据类型、POJO 数据类型、数组和集合数据类型以及 JSON 数据类型，接下来我们还得处理一种开发中比较常见的一种数据类型，**`日期类型`**

日期类型比较特殊，因为对于**日期的格式有 N 多中输入方式**，比如:

*   2088-08-18
*   2088/08/18
*   08/18/2088
*   ......

#### 4.5.1 具体代码

20xx/xx/xx 格式日期，controller 可以自动转换成 Date 对象，其他格式日期要注解 **@DateTimeFormat(pattern="yyyy-MM-dd")** ，pattern 译为模式，图案

**步骤 1: 编写方法接收日期数据**

在 UserController 类中添加方法，把参数设置为日期类型

```
@RequestMapping("/dataParam")
@ResponseBody
public String dataParam(Date date)
    System.out.println("参数传递 date ==> "+date);
    return "{'module':'data param'}";
}
```

**步骤 2: 启动 Tomcat 服务器**

查看控制台是否报错，如果有错误，先解决错误。

**步骤 3: 使用 PostMan 发送请求**

使用 PostMan 发送 GET 请求，并设置 date 参数

```
http://localhost/dataParam?date=2088/08/08

```

![](<assets/1740015639477.png>)

**步骤 4: 查看控制台**

![](<assets/1740015639706.png>)

通过打印，我们发现 SpringMVC 可以接收日期数据类型，并将其打印在控制台。

这个时候，我们就想如果把日期参数的格式改成其他的，SpringMVC 还能处理么?

**步骤 5: 更换日期格式**

为了能更好的看到程序运行的结果，我们在方法中多添加一个日期参数

```
@RequestMapping("/dataParam")
@ResponseBody
public String dataParam(Date date,Date date1)
    System.out.println("参数传递 date ==> "+date);
    return "{'module':'data param'}";
}
```

使用 PostMan 发送请求，携带两个不同的日期格式，

```
http://localhost/dataParam?date=2088/08/08&date1=2088-08-08

```

![](<assets/1740015639960.png>)

发送请求和数据后，页面会报 400，控制台会报出一个错误

Resolved [org.springframework.web.method.annotation.**MethodArgumentTypeMismatchException**: Failed to convert value of type 'java.lang.String' to required type 'java.util.Date'; nested exception is org.springframework.core.convert.**ConversionFailedException**: Failed to convert from type [java.lang.String] to type [java.util.Date] for value '2088-08-08'; nested exception is java.lang.IllegalArgumentException]

从错误信息可以看出，错误的原因是在将`2088-08-08`转换成日期类型的时候失败了，原因是 SpringMVC 默认支持的字符串转日期的格式为`yyyy/MM/dd`, 而我们现在传递的不符合其默认格式，SpringMVC 就无法进行格式转换，所以报错。

解决方案也比较简单，需要使用`@DateTimeFormat`

```
@RequestMapping("/dataParam")
@ResponseBody
public String dataParam(Date date,
                        @DateTimeFormat(pattern="yyyy-MM-dd") Date date1)
    System.out.println("参数传递 date ==> "+date);
    System.out.println("参数传递 date1(yyyy-MM-dd) ==> "+date1);
    return "{'module':'data param'}";
}
```

重新启动服务器，重新发送请求测试，SpringMVC 就可以正确的进行日期转换了

![](<assets/1740015640160.png>)

**步骤 6: 携带时间的日期**

接下来我们再来发送一个携带时间的日期，看下 SpringMVC 该如何处理?

先修改 UserController 类，添加第三个参数

```
@RequestMapping("/dataParam")
@ResponseBody
public String dataParam(Date date,
                        @DateTimeFormat(pattern="yyyy-MM-dd") Date date1,
                        @DateTimeFormat(pattern="yyyy/MM/dd HH:mm:ss") Date date2)
    System.out.println("参数传递 date ==> "+date);
    System.out.println("参数传递 date1(yyyy-MM-dd) ==> "+date1);
    System.out.println("参数传递 date2(yyyy/MM/dd HH:mm:ss) ==> "+date2);
    return "{'module':'data param'}";
}
```

使用 PostMan 发送请求，携带两个不同的日期格式，

```
http://localhost/dataParam?date=2088/08/08&date1=2088-08-08&date2=2088/08/08 8:08:08

```

![](<assets/1740015640454.png>)

重新启动服务器，重新发送请求测试，SpringMVC 就可以将日期时间的数据进行转换

![](<assets/1740015640675.png>)

#### 知识点 @DateTimeFormat

<table><thead><tr><th>名称</th><th>@DateTimeFormat</th></tr></thead><tbody><tr><td>类型</td><td><strong>形参注解</strong></td></tr><tr><td>位置</td><td>SpringMVC 控制器方法形参前面</td></tr><tr><td>作用</td><td>设定日期时间型数据格式</td></tr><tr><td>相关属性</td><td>pattern：指定日期时间格式字符串</td></tr></tbody></table>

#### 4.5.2 类型转换内部实现原理

讲解内部原理之前，我们需要先思考个问题:

*   前端传递字符串，后端使用日期 Date 接收
*   前端传递 JSON 数据，后端使用对象接收
*   前端传递字符串，后端使用 Integer 接收
*   后台需要的数据类型有很多中
*   在数据的传递过程中存在很多类型的转换

问: 谁来做这个类型转换?

答: SpringMVC

问: SpringMVC 是如何实现类型转换的?

答: SpringMVC 中提供了很多类型转换接口和实现类

在框架中，有一些类型转换接口，其中有:

*   **(1) Converter 接口**

```
/**
*    S: the source type
*    T: the target type
*/
public interface Converter<S, T> {
    @Nullable
    //该方法就是将从页面上接收的数据(S)转换成我们想要的数据类型(T)返回
    T convert(S source);
}
```

**注意: Converter 所属的包为`org.springframework.core.convert.converter`**

Converter 接口的实现类

![](<assets/1740015640994.png>)

框架中有提供很多对应 Converter 接口的实现类，用来实现不同数据类型之间的转换, 如:

请求参数年龄数据（String→Integer）

日期格式转换（String → Date）

*   **(2) HttpMessageConverter 接口**

该接口是实现**对象与 JSON 之间的转换工作**

****注意: SpringMVC 的配置类把 @EnableWebMvc 当做标配配置上去，不要省略****

### 4.6 响应

SpringMVC 接收到请求和数据后，进行一些了的处理，当然这个处理可以是转发给 Service，Service 层再调用 Dao 层完成的，不管怎样，**处理完以后，都需要将结果告知给用户。**

**比如: 根据用户 ID 查询用户信息、查询用户列表、新增用户等。**

对于响应，主要就包含两部分内容：

*   **响应页面**
*   **响应数据**
    *   **文本数据**
    *   **json 数据**

因为**异步调用**是目前常用的主流方式，所以我们需要**更关注**的就是如何返回 **JSON 数据**，对于其他只需要认识了解即可。

#### 4.6.1 环境准备，jackson-databind 依赖

*   创建一个 Web 的 Maven 项目
    
*   pom.xml 添加 Spring 依赖 javax.servlet-api,spring-webmvc,jackson-databind
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
     
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
     
      <groupId>com.itheima</groupId>
      <artifactId>springmvc_05_response</artifactId>
      <version>1.0-SNAPSHOT</version>
      <packaging>war</packaging>
     
      <dependencies>
        <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>javax.servlet-api</artifactId>
          <version>3.1.0</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>5.2.10.RELEASE</version>
        </dependency>
    <!--json解析工具-->
        <dependency>
          <groupId>com.fasterxml.jackson.core</groupId>
          <artifactId>jackson-databind</artifactId>
          <version>2.9.0</version>
        </dependency>
      </dependencies>
     
      <build>
        <plugins>
          <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.1</version>
            <configuration>
              <port>80</port>
              <path>/</path>
            </configuration>
          </plugin>
        </plugins>
      </build>
    </project>
    ```
    
*   创建 servlet 容器的配置类和 springmvc 配置类
    
    ```
    public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
        protected Class<?>[] getRootConfigClasses() {
            return new Class[0];
        }
     
        protected Class<?>[] getServletConfigClasses() {
            return new Class[]{SpringMvcConfig.class};
        }
     
        protected String[] getServletMappings() {
            return new String[]{"/"};
        }
     
        //乱码处理
        @Override
        protected Filter[] getServletFilters() {
            CharacterEncodingFilter filter = new CharacterEncodingFilter();
            filter.setEncoding("UTF-8");
            return new Filter[]{filter};
        }
    }
     
    @Configuration
    @ComponentScan("com.itheima.controller")
    //开启json数据类型自动转换
    @EnableWebMvc
    public class SpringMvcConfig {
    }
     
    ```
    
*   编写模型类 User
    

```
public class User {
    private String name;
    private int age;
    //getter...setter...toString省略
}
```

*   webapp 下创建 page.jsp
    
    ```
    <html>
    <body>
    <h2>Hello Spring MVC!</h2>
    </body>
    </html>
    ```
    

最终创建好的项目结构如下:

![](<assets/1740015641286.png>)

#### 4.6.2 响应 jsp 页面 [了解]

**步骤 1: 设置返回页面**

```
@Controller
public class UserController {
 
    @RequestMapping("/toJumpPage")
    //注意
    //1.此处不能添加@ResponseBody,如果加了该注入，会直接将page.jsp当字符串返回前端
    //2.方法需要返回String
    public String toJumpPage(){
        System.out.println("跳转页面");
        return "page.jsp";
    }
 
}
```

**注意：**

*   **方法不能添加 @ResponseBody**, 如果加了该注入，会直接将 page.jsp 当字符串返回前端。
*   方法需要返回 String

**步骤 2: 启动程序测试**

此处涉及到页面跳转，所以不适合采用 PostMan 进行测试，直接打开浏览器，输入

`http://localhost/toJumpPage`

![](<assets/1740015641495.png>)

#### 4.6.3 返回文本数据 [了解]

**步骤 1: 设置返回文本内容**

```
@Controller
public class UserController {
 
       @RequestMapping("/toText")
    //注意此处该注解就不能省略，如果省略了,会把response text当前页面名称去查找，如果没有回报404错误
    @ResponseBody
    public String toText(){
        System.out.println("返回纯文本数据");
        return "response text";
    }
 
}
```

**步骤 2: 启动程序测试**

此处不涉及到页面跳转，因为我们现在发送的是 GET 请求，可以使用浏览器也可以使用 PostMan 进行测试，输入地址`http://localhost/toText`访问

![](<assets/1740015641690.png>)

#### 4.6.4 响应 JSON 数据

**响应 POJO 对象：直接返回对象**

```
@Controller
public class UserController {
 
    @RequestMapping("/toJsonPOJO")
    @ResponseBody
    @JsonFormat(pattern="yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")    //导入jackson-databind后可以格式化日期格式
    public User toJsonPOJO(){
        System.out.println("返回json对象数据");
        User user = new User();
        user.setName("itcast");
        user.setAge(15);
        return user;
    }
 
}
```

返回值为实体类对象，设置返回值为实体类类型，即可实现返回对应对象的 json 数据，需要依赖 **@ResponseBody** 注解和 **@EnableWebMvc** 注解

重新启动服务器，访问

```
http://localhost/toJsonPOJO

```

![](<assets/1740015641932.png>)

如果 return 字符串 "{'zhang 张三':32}" 会存在中文乱码问题，所以返回对象自动转 JSON 比较好。

**响应 POJO 集合对象：直接返回集合类型**

```
@Controller
public class UserController {
 
    @RequestMapping("/toJsonList")
    @ResponseBody
    public List<User> toJsonList(){
        System.out.println("返回json集合数据");
        User user1 = new User();
        user1.setName("传智播客");
        user1.setAge(15);
 
        User user2 = new User();
        user2.setName("黑马程序员");
        user2.setAge(12);
 
        List<User> userList = new ArrayList<User>();
        userList.add(user1);
        userList.add(user2);
 
        return userList;
    }
 
}
```

重新启动服务器，访问`http://localhost/toJsonList`

![](<assets/1740015642214.png>)

#### 知识点 @ResponseBody

<table><thead><tr><th>名称</th><th>@ResponseBody</th></tr></thead><tbody><tr><td>类型</td><td><strong>方法 \ 类注解</strong></td></tr><tr><td>位置</td><td>SpringMVC 控制器方法定义上方和控制类上</td></tr><tr><td>作用</td><td><strong>设置当前控制器返回值作为响应体,</strong><br>写在类上，该类的所有方法都有该注解功能</td></tr><tr><td>相关属性</td><td>pattern：指定日期时间格式字符串</td></tr></tbody></table>

**说明:**

*   该注解可以写在类上或者方法上
*   写在类上就是该类下的所有方法都有 @ReponseBody 功能
*   当**方法上有 @ReponseBody** 注解后
    *   方法的**返回值为字符串**，会将其作为**文本内容**直接**响应给前端**
    *   方法的**返回值为对象**，会将对象**转换成 JSON 响应给前端**

此处又使用到了类型转换，内部还是通过 Converter 接口的实现类完成的，所以 Converter 除了前面所说的功能外，它还可以实现:

*   对象转 Json 数据 (POJO -> json)
*   集合转 Json 数据 (Collection -> json)

## 5，Rest 风格

### 5.1 REST 简介

SpringMVC 的 Rest 风格指的是以 RESTful API 实现在 Web 上，对外提供一个面向资源（resource）的接口设计。该风格下的 API 需要遵循如下约定：

*   使用 HTTP 协议中的 GET、POST、PUT、DELETE 方法来表示对资源的增删改查操作；
*   遵循 URI 只建议由小写字母、数字、连字符和点号`/`组成、“/” 为层级关系的分隔符、名词使用复数形式等命名规则；
*   避免使用已经过期的 HTTP Header 信息，如 “Accept-Charset”、“Accept-Encoding”、“Connection”。推荐参照 HTTP1.1 版本，使用应答首部文件中的 "Content-Type"。

示例：

<table><thead><tr><th>请求类型</th><th>REST URL</th><th>描述</th></tr></thead><tbody><tr><td>GET</td><td>/users</td><td>获取用户列表</td></tr><tr><td>POST</td><td>/users</td><td>创建新用户</td></tr><tr><td>PUT</td><td>/users/{id}</td><td>修改指定 ID 的用户</td></tr><tr><td>DELETE</td><td>/users/{id}</td><td>删除指定 ID 的用户</td></tr></tbody></table>

采用 Rest 风格的 API 其好处在于：可以简化接口调用，降低了与客户端之间开发的耦合度，同时也让数据更加直观易懂。同时，有助于降低服务端的开发制约，支持微服务架构，并且能够更好地支持各种移动设备和网络应用，可以更好地满足 API 设计的不断迭代、演进的需求。

*   **REST**（Representational State Transfer），**表现性状态转换**, 它是一种**软件架构风格**
    
    当我们想**表示一个网络资源**的时候，可以使用两种方式:
    
    *   **传统风格资源描述形式**
        *   `http://localhost/user/getById?id=1` 查询 id 为 1 的用户信息
        *   `http://localhost/user/saveUser` 保存用户信息
    *   **REST 风格描述形式**
        *   `http://localhost/user/1`
        *   `http://localhost/user`

传统方式一般是一个请求 url 对应一种操作，这样做不仅麻烦，也不安全，因为会程序的人读取了你的请求 url 地址，就大概知道该 url 实现的是一个什么样的操作。

查看 REST 风格的描述，你会发现请求地址变的简单了，并且**光看请求 URL 并不是很能猜出来该 URL 的具体功能**

所以 **REST 的优点**有:

*   **隐藏资源的访问行为**，无法通过地址得知对资源是何种操作
*   **书写简化**

**问题：**一个相同的 **url 地址**即可以是新增也可以是修改或者查询，我们该**如何区分**？

**答案： 使用请求方式区分，例如`GET`,`POST`,`PUT`,`DELETE。`**

*   按照 REST 风格访问资源时使用**请求方式**区分对资源进行了何种操作
    *   `http://localhost/users` 查询全部用户信息 **GET（查询）**
    *   `http://localhost/users/1` 查询指定用户信息 GET（查询）
    *   `http://localhost/users` 添加用户信息 **POST（新增 / 保存）**
    *   `http://localhost/users` 修改用户信息 **PUT（修改 / 更新）**
    *   `http://localhost/users/1` 删除用户信息 **DELETE（删除）**

请求的方式比较多，但是比较常用的就 4 种，分别是**`GET`,`POST`,`PUT`,`DELETE`**。

**常用的四种请求方式：**

*   发送 **GET 请求**是用来做**查询**
*   发送 **POST 请求**是用来做**新增**
*   发送 **PUT 请求**是用来做**修改**
*   发送 **DELETE 请求**是用来做**删除**

**注意**:

*   上述行为是**约定方式，约定不是规范，可以打破**，所以称 REST 风格，而不是 REST 规范。虽然不是规范，但现在**基本都用 rest 风格**。
    *   **REST 提供了对应的架构方式**，按照这种架构设计项目可以降低开发的复杂性，提高系统的可伸缩性
    *   REST 中规定 GET/POST/PUT/DELETE 针对的是查询 / 新增 / 修改 / 删除，但是我们如果非要用 GET 请求做删除，这点在程序上运行是可以实现的
    *   但是如果绝大多数人都遵循这种风格，你写的代码让别人读起来就有点莫名其妙了。
*   描述**模块的名称**通常使用**复数**，也就是加 s 的格式描述，表示此类资源，而非单个资源，例如: users、books、accounts......

 **RESTful：**

清楚了什么是 REST 风格后，我们后期会经常提到一个概念叫`RESTful`，那什么又是 RESTful 呢?

*   **根据 REST 风格对资源进行访问**称为 **RESTful**。

后期我们在进行开发的过程中，大多是都是遵从 REST 风格来访问我们的后台服务，所以可以说咱们以后都是**基于 RESTful 来进行开发**的。

### 5.2 RESTful 入门案例

#### 5.2.1 环境准备

*   创建一个 Web 的 Maven 项目
    
*   pom.xml 添加依赖 javax.servlet-api,spring-webmvc,jackson-databind
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
     
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
     
      <groupId>com.itheima</groupId>
      <artifactId>springmvc_06_rest</artifactId>
      <version>1.0-SNAPSHOT</version>
      <packaging>war</packaging>
     
      <dependencies>
        <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>javax.servlet-api</artifactId>
          <version>3.1.0</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>5.2.10.RELEASE</version>
        </dependency>
        <dependency>
          <groupId>com.fasterxml.jackson.core</groupId>
          <artifactId>jackson-databind</artifactId>
          <version>2.9.0</version>
        </dependency>
      </dependencies>
     
      <build>
        <plugins>
          <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.1</version>
            <configuration>
              <port>80</port>
              <path>/</path>
            </configuration>
          </plugin>
        </plugins>
      </build>
    </project>
    ```
    
*   **创建对应的配置类**
    

```
  public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
      protected Class<?>[] getRootConfigClasses() {
          return new Class[0];
      }
 
      protected Class<?>[] getServletConfigClasses() {
          return new Class[]{SpringMvcConfig.class};
      }
 
      protected String[] getServletMappings() {
          return new String[]{"/"};
      }
 
      //乱码处理
      @Override
      protected Filter[] getServletFilters() {
          CharacterEncodingFilter filter = new CharacterEncodingFilter();
          filter.setEncoding("UTF-8");
          return new Filter[]{filter};
      }
  }
 
  @Configuration
  @ComponentScan("com.itheima.controller")
  //开启json数据类型自动转换
  @EnableWebMvc
  public class SpringMvcConfig {
  }
```

编写模型类 User 和 Book

```
  public class User {
      private String name;
      private int age;
      //getter...setter...toString省略
  }
 
  public class Book {
      private String name;
      private double price;
       //getter...setter...toString省略
  }
```

**编写 UserController 和 BookController**

```
  @Controller
  public class UserController {
      @RequestMapping("/save")
      @ResponseBody
      public String save(@RequestBody User user) {
          System.out.println("user save..."+user);
          return "{'module':'user save'}";
      }
 
      @RequestMapping("/delete")
      @ResponseBody
      public String delete(Integer id) {
          System.out.println("user delete..." + id);
          return "{'module':'user delete'}";
      }
 
      @RequestMapping("/update")
      @ResponseBody
      public String update(@RequestBody User user) {
          System.out.println("user update..." + user);
          return "{'module':'user update'}";
      }
 
      @RequestMapping("/getById")
      @ResponseBody
      public String getById(Integer id) {
          System.out.println("user getById..." + id);
          return "{'module':'user getById'}";
      }
 
      @RequestMapping("/findAll")
      @ResponseBody
      public String getAll() {
          System.out.println("user getAll...");
          return "{'module':'user getAll'}";
      }
  }
 
 
  @Controller
  public class BookController {
 
      @RequestMapping(value = "/books",method = RequestMethod.POST)
      @ResponseBody
      public String save(@RequestBody Book book){
          System.out.println("book save..." + book);
          return "{'module':'book save'}";
      }
 
      @RequestMapping(value = "/books/{id}",method = RequestMethod.DELETE)
      @ResponseBody
      public String delete(@PathVariable Integer id){
          System.out.println("book delete..." + id);
          return "{'module':'book delete'}";
      }
 
      @RequestMapping(value = "/books",method = RequestMethod.PUT)
      @ResponseBody
      public String update(@RequestBody Book book){
          System.out.println("book update..." + book);
          return "{'module':'book update'}";
      }
 
      @RequestMapping(value = "/books/{id}",method = RequestMethod.GET)
      @ResponseBody
      public String getById(@PathVariable Integer id){
          System.out.println("book getById..." + id);
          return "{'module':'book getById'}";
      }
 
      @RequestMapping(value = "/books",method = RequestMethod.GET)
      @ResponseBody
      public String getAll(){
          System.out.println("book getAll...");
          return "{'module':'book getAll'}";
      }
 
  }
```

最终创建好的项目结构如下:

![](<assets/1740015642399.png>)

#### 5.2.2 思路分析

**需求:** 将之前的增删改查替换成 RESTful 的开发方式。

1. 之前不同的请求有不同的路径, 现在要将其修改为统一的请求路径

修改前: 新增: /save , 修改: /update, 删除 /delete...

修改后: 增删改查: /users

2. 根据 GET 查询、POST 新增、PUT 修改、DELETE 删除对方法的请求方式进行限定

3. 发送请求的过程中如何设置请求参数?

#### 5.2.3 代码实现，@RequestMapping 的 method 属性，@PathVariable

**了解即可**，主要用 5.3 的优化方案 

**新增**

```
@Controller
public class UserController {
    //设置当前请求方法为POST，表示REST风格中的添加操作
    @RequestMapping(value = "/users",method = RequestMethod.POST)
    @ResponseBody
    public String save() {
        System.out.println("user save...");
        return "{'module':'user save'}";
    }
}
```

*   将请求路径更改为`/users`
    
    *   访问该方法使用 POST: `http://localhost/users`
*   使用 method 属性限定该方法的访问方式为`POST`
    
    *   **如果发送的不是 POST 请求**，比如发送 GET 请求，**则会报错**
        
        ![](<assets/1740015642590.png>)
        

**删除**

**先不传递路径参数删除**

```
@Controller
public class UserController {
    //设置当前请求方法为DELETE，表示REST风格中的删除操作
    @RequestMapping(value = "/users",method = RequestMethod.DELETE)
    @ResponseBody
    public String delete(Integer id) {
        System.out.println("user delete..." + id);
        return "{'module':'user delete'}";
    }
}
```

*   将请求路径更改为`/users`
    *   访问该方法使用 DELETE: `http://localhost/users`

访问成功，但是删除方法没有携带所要删除数据的 id, 所以针对 RESTful 的开发，如何携带数据参数?

**传递路径参数删除，@PathVariable**

**@PathVariable** 译作可变路径，**作用是声明这个参数来自于路径。**

前端发送请求的时候使用:`http://localhost/users/1`, 路径中的`1`就是我们想要传递的参数。

后端获取参数，需要做如下修改:

*   修改 @RequestMapping 的 value 属性，将其中修改为`/users/{id}`，目的是和路径匹配
*   在方法的形参前添加 @PathVariable 注解

```
@Controller
public class UserController {
    //设置当前请求方法为DELETE，表示REST风格中的删除操作
    @RequestMapping(value = "/users/{id}",method = RequestMethod.DELETE)
    @ResponseBody
    //@PathVariable声明此参数来自于路径。使参数和@RequestMapping中路径中的参数绑定。如果名称不一样，需要起别名再绑定
    public String delete(@PathVariable Integer id) {
        System.out.println("user delete..." + id);
        return "{'module':'user delete'}";
    }
}
```

**思考:**

**(1) 如果方法形参的名称和路径`{}`中的值不一致，该怎么办?**

**答：起别名。**

![](<assets/1740015642890.png>)

**(2) 如果有多个参数需要传递该如何编写?**

**答：路径后加斜杠新增参数。**

前端发送请求的时候使用:`http://localhost/users/1/tom`, 路径中的`1`和`tom`就是我们想要传递的两个参数。

后端获取参数，需要做如下修改:

```
@Controller
public class UserController {
    //设置当前请求方法为DELETE，表示REST风格中的删除操作
    @RequestMapping(value = "/users/{id}/{name}",method = RequestMethod.DELETE)
    @ResponseBody
    public String delete(@PathVariable Integer id,@PathVariable String name) {
        System.out.println("user delete..." + id+","+name);
        return "{'module':'user delete'}";
    }
}
```

**修改**

```
@Controller
public class UserController {
    //设置当前请求方法为PUT，表示REST风格中的修改操作
    @RequestMapping(value = "/users",method = RequestMethod.PUT)
    @ResponseBody
    public String update(@RequestBody User user) {
        System.out.println("user update..." + user);
        return "{'module':'user update'}";
    }
}
```

*   将请求路径更改为`/users`
    
    *   访问该方法使用 PUT: `http://localhost/users`
*   访问并携带参数:
    
    ![](<assets/1740015643196.png>)
    

**根据 ID 查询**

```
@Controller
public class UserController {
    //设置当前请求方法为GET，表示REST风格中的查询操作
    @RequestMapping(value = "/users/{id}" ,method = RequestMethod.GET)
    @ResponseBody
    public String getById(@PathVariable Integer id){
        System.out.println("user getById..."+id);
        return "{'module':'user getById'}";
    }
}
```

将请求路径更改为`/users`

*   访问该方法使用 GET: `http://localhost/users/666`

**查询所有**

```
@Controller
public class UserController {
    //设置当前请求方法为GET，表示REST风格中的查询操作
    @RequestMapping(value = "/users" ,method = RequestMethod.GET)
    @ResponseBody
    public String getAll() {
        System.out.println("user getAll...");
        return "{'module':'user getAll'}";
    }
}
```

将请求路径更改为`/users`

*   访问该方法使用 GET: `http://localhost/users`

**小结**

RESTful 入门案例，我们需要学习的内容如下:

(1) 设定 Http **请求动作** (动词)

**@RequestMapping**(value="",**method** = RequestMethod.**POST|GET|PUT|DELETE**)

(2) 设定**请求参数** (路径变量)

@RequestMapping(value="/users/**{id}**",method = RequestMethod.DELETE)

@ReponseBody

public String delete(**@PathVariable** Integer **id**){

}

#### 5.2.4 目前学的参数占位符汇总

*   **mysql：**参数占位符 ${}（有 sql 注入问题）和 #{} 
    
    ```
        @Insert("insert into tbl_book values (null,#{type},#{name},#{description})")
        public void save(Book book);
    ```
    
*    **jsp 中 EL 表达式：**参数占位符 ${}
    
    ```
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>Title</title>
    </head>
    <body>
        ${brands}
    </body>
    </html>
    ```
    
*   **Vue：**参数占位符 {{}}
    
    ```
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
    <div>
        <input v-model="username"><br>
        <!--插值表达式-->
        input里的值是：{{username}}
    </div>
    <script src="js/vue.js"></script>
    <script>
        //1. 创建Vue核心对象
        new Vue({
            el:"#app",
            data(){  // data() 是 ECMAScript 6 版本的新的写法
                return {
                    username:"默认值"
                }
            }
     
            /*data: function () {
                return {
                    username:""
                }
            }*/
        });
     
    </script>
    </body>
    </html>
    ```
    
*   **Spring 的 JdbcConfig 注入 properties：**参数占位符 ${}。记得 SpringConfig 的 @PropertySource("jdbc.properties")
    
    ```
        @Value("${jdbc.driver}")
        private String driver;
    ```
    
*   **SpringMVC 的 @RequestMapping 或 @PostMapping 路径：**参数占位符 {}
    
    ```
        @DeleteMapping("/{id}")
        public String delete(@PathVariable Integer id){
            System.out.println("book delete..." + id);
            return "{'module':'book delete'}";
        }
    ```
    

#### 知识点 @PathVariable

<table><thead><tr><th>名称</th><th>@PathVariable</th></tr></thead><tbody><tr><td>类型</td><td><strong>形参注解</strong></td></tr><tr><td>位置</td><td>SpringMVC 控制器方法形参定义前面</td></tr><tr><td>作用</td><td>绑定路径参数与处理器方法形参间的关系，要求路径参数名与形参名一一对应</td></tr></tbody></table>

### 5.2.5 区别：`@RequestBody`、`@RequestParam`、`@PathVariable`

关于接收参数，我们学过三个注解`@RequestBody`、`@RequestParam`、`@PathVariable`, 这三个注解之间的区别和应用分别是什么?

*   **联系：**都是参数注解，都用于前端到 controller 的参数传递。 
*   **区别**
    *   **@RequestParam** 用于**接收非 JSON 参数，除集合外其他类型都可以省略**，可以**起别名**。如果实参是集合则必须加这个注解。
        
        ```
        @RequestMapping("/commonParamDifferentName")
            @ResponseBody
            public String commonParamDifferentName(@RequestParam("name") String userName , int age){
                System.out.println("普通参数传递 userName ==> "+userName);
                System.out.println("普通参数传递 age ==> "+age);
                return "{'module':'common param different name'}";
            }
        ```
        
        ```
        //集合参数：同名请求参数可以使用@RequestParam注解映射到对应名称的集合对象中作为数据
        @RequestMapping("/listParam")
        @ResponseBody
        public String listParam(@RequestParam List<String> likes){
            System.out.println("集合参数传递 likes ==> "+ likes);
            return "{'module':'list param'}";
        }
        ```
        
    *   **@RequestBody** 用于**接收 json 数据**（请求体参数），必须加上才能获取到前端传来的 JSON 数据。
        
        ```
        @RequestMapping("/pojoParamForJson")
        @ResponseBody
        public String pojoParamForJson(@RequestBody User user){
            System.out.println("pojo(json)参数传递 user ==> "+user);
            return "{'module':'pojo for json param'}";
        }
        ```
        
    *   **@PathVariable** 用于**接收路径参数**，**REST 风格。**使用 {参数名称} 描述路径参数
        
        ```
        @Controller
        public class UserController {
            //设置当前请求方法为GET，表示REST风格中的查询操作
            @RequestMapping(value = "/users/{id}" ,method = RequestMethod.GET)
            @ResponseBody
            public String getById(@PathVariable Integer id){
                System.out.println("user getById..."+id);
                return "{'module':'user getById'}";
            }
        }
        ```
        
*   **应用**
    *   后期开发中，发送请求参数超过 1 个时，**以 json 格式为主**，**@RequestBody** 应用较广
    *   如果发送非 json 格式数据，选用 @RequestParam 接收请求参数
    *   采用 **RESTful** 进行开发，当参数数量较少时，例如 1 个，可以采用 **@PathVariable** 接收请求路径变量，通常用于**传递 id 值**

### 5.3 RESTful 优化，快速开发

#### 5.3.1 概述，@RestController，@GetMapping  

**结论：**

**@RestController 合并** @Controller 和 @ResponseBody

@GetMapping  @PostMapping  @PutMapping  @DeleteMapping **代替** @RequestMapping

**RESTful 开发麻烦的地方：**

![](<assets/1740015643578.png>)

**问题 1：**每个方法的 @RequestMapping 注解中都定义了访问路径 **/books**，**重复**性太高。

**问题 2：**每个方法的 @RequestMapping 注解中都要使用 **method** 属性定义请求方式，**重复**性太高。

**问题 3：**每个方法响应 json 都需要加上 **@ResponseBody** 注解，**重复**性太高。

**问题解决：**

```
@RestController //@Controller + ReponseBody
@RequestMapping("/books")
public class BookController {
 
    //@RequestMapping(method = RequestMethod.POST)
    @PostMapping
    public String save(@RequestBody Book book){
        System.out.println("book save..." + book);
        return "{'module':'book save'}";
    }
 
    //@RequestMapping(value = "/{id}",method = RequestMethod.DELETE)
    @DeleteMapping("/{id}")
    public String delete(@PathVariable Integer id){
        System.out.println("book delete..." + id);
        return "{'module':'book delete'}";
    }
 
    //@RequestMapping(method = RequestMethod.PUT)
    @PutMapping
    public String update(@RequestBody Book book){
        System.out.println("book update..." + book);
        return "{'module':'book update'}";
    }
 
    //@RequestMapping(value = "/{id}",method = RequestMethod.GET)
    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println("book getById..." + id);
        return "{'module':'book getById'}";
    }
 
    //@RequestMapping(method = RequestMethod.GET)
    @GetMapping
    public String getAll(){
        System.out.println("book getAll...");
        return "{'module':'book getAll'}";
    }
 
}
```

对于刚才的问题，我们都有对应的解决方案：

**问题 1：**每个方法的 @RequestMapping 注解中都定义了**访问路径 / books**，**重复**性太高。

```
将@RequestMapping提到类上面，将@Controller和@RequestMapping合并为@RestController，用来定义所有方法共同的访问路径。


```

**问题 2：**每个方法的 @RequestMapping 注解中都要使用 **method** 属性定义**请求方式**，**重复**性太高。

```
使用@GetMapping  @PostMapping  @PutMapping  @DeleteMapping代替


```

**问题 3：**每个方法响应 json 都需要加上 **@ResponseBody 注解**，**重复**性太高。

```
1.将ResponseBody提到类上面，让所有的方法都有@ResponseBody的功能，即返回值是响应体，而不是路径
2.使用@RestController注解替换@Controller与@ResponseBody注解，简化书写
```

#### **知识点 @RestController@GetMapping @PostMapping @PutMapping @DeleteMapping**

**知识点 1：@RestController**

<table><thead><tr><th>名称</th><th>@RestController</th></tr></thead><tbody><tr><td>类型</td><td><strong>类注解</strong></td></tr><tr><td>位置</td><td>基于 SpringMVC 的 RESTful 开发控制器类定义上方</td></tr><tr><td>作用</td><td>设置当前控制器类为 RESTful 风格，<br>等同于 @Controller 与 @ResponseBody 两个注解组合功能</td></tr></tbody></table>

**知识点 2：@GetMapping @PostMapping @PutMapping @DeleteMapping**

<table><thead><tr><th>名称</th><th>@GetMapping @PostMapping @PutMapping @DeleteMapping</th></tr></thead><tbody><tr><td>类型</td><td><strong>方法注解</strong></td></tr><tr><td>位置</td><td>基于 SpringMVC 的 RESTful 开发控制器方法定义上方</td></tr><tr><td>作用</td><td>设置当前控制器方法请求访问路径与请求动作，每种对应一个请求动作，<br>例如 @GetMapping 对应 GET 请求</td></tr><tr><td>相关属性</td><td>value（默认）：请求访问路径</td></tr></tbody></table>

### 5.4 RESTful 案例

#### 5.4.1 需求分析

**需求一: 假数据图片列表查询**，从后台返回数据，将数据展示在页面上

![](<assets/1740015643943.png>)

**需求二: 新增图片**，将新增图书的数据传递到后台，并在控制台打印

![](<assets/1740015644161.png>)

**说明:** 此次案例的重点是在 SpringMVC 中如何使用 RESTful 实现前后台交互，所以本案例并没有和数据库进行交互，所有数据使用**`假`数据**来完成开发。

步骤分析:

1. 搭建项目导入 jar 包

2. 编写 Controller 类，提供两个方法，一个用来做列表查询，一个用来做新增

3. 在方法上使用 RESTful 进行路径设置

4. 完成请求、参数的接收和结果的响应

5. 使用 PostMan 进行测试

6. 将前端页面拷贝到项目中

7. 页面发送 ajax 请求

8. 完成页面数据的展示

#### 5.4.2 环境准备

*   创建一个 Web 的 Maven 项目
    
*   pom.xml 添加 SpringMVC 三大依赖 javax.servlet-api,spring-webmvc,jackson-databind
    
    ```
    <?xml version="1.0" encoding="UTF-8"?>
     
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
     
      <groupId>com.itheima</groupId>
      <artifactId>springmvc_07_rest_case</artifactId>
      <version>1.0-SNAPSHOT</version>
      <packaging>war</packaging>
     
      <dependencies>
        <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>javax.servlet-api</artifactId>
          <version>3.1.0</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>5.2.10.RELEASE</version>
        </dependency>
        <dependency>
          <groupId>com.fasterxml.jackson.core</groupId>
          <artifactId>jackson-databind</artifactId>
          <version>2.9.0</version>
        </dependency>
      </dependencies>
     
      <build>
        <plugins>
          <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.1</version>
            <configuration>
              <port>80</port>
              <path>/</path>
            </configuration>
          </plugin>
        </plugins>
      </build>
    </project>
    ```
    

创建对应的配置类

```
  public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
      protected Class<?>[] getRootConfigClasses() {
          return new Class[0];
      }
 
      protected Class<?>[] getServletConfigClasses() {
          return new Class[]{SpringMvcConfig.class};
      }
 
      protected String[] getServletMappings() {
          return new String[]{"/"};
      }
 
      //乱码处理
      @Override
      protected Filter[] getServletFilters() {
          CharacterEncodingFilter filter = new CharacterEncodingFilter();
          filter.setEncoding("UTF-8");
          return new Filter[]{filter};
      }
  }
 
  @Configuration
  @ComponentScan("com.itheima.controller")
  //开启json数据类型自动转换
  @EnableWebMvc
  public class SpringMvcConfig {
  }
```

编写模型类 Book

```
  public class Book {
      private Integer id;
      private String type;
      private String name;
      private String description;
      //setter...getter...toString略
  }
```

**编写 BookController**

```
  @Controller
  public class BookController {
 
 
  }
```

最终创建好的项目结构如下:

![](<assets/1740015644465.png>)

#### 5.4.2 后台接口开发

**步骤 1: 编写 Controller 类并使用 RESTful 进行配置**

```
@RestController
@RequestMapping("/books")
public class BookController {
 
    @PostMapping
    public String save(@RequestBody Book book){
        System.out.println("book save ==> "+ book);
        return "{'module':'book save success'}";
    }
 
     @GetMapping
    public List<Book> getAll(){
        System.out.println("book getAll is running ...");
        List<Book> bookList = new ArrayList<Book>();
 
        Book book1 = new Book();
        book1.setType("计算机");
        book1.setName("SpringMVC入门教程");
        book1.setDescription("小试牛刀");
        bookList.add(book1);
 
        Book book2 = new Book();
        book2.setType("计算机");
        book2.setName("SpringMVC实战教程");
        book2.setDescription("一代宗师");
        bookList.add(book2);
 
        Book book3 = new Book();
        book3.setType("计算机丛书");
        book3.setName("SpringMVC实战教程进阶");
        book3.setDescription("一代宗师呕心创作");
        bookList.add(book3);
 
        return bookList;
    }
 
}
```

**步骤 2：先测试后端，再开发前端。PostMan 测试**

测试新增

```
{
    "type":"计算机丛书",
    "name":"SpringMVC终极开发",
    "description":"这是一本好书"
}
```

![](<assets/1740015645005.png>)

测试查询

![](<assets/1740015645261.png>)

#### 5.4.3 放行静态页面，WebMvcConfigurer 配置类添加资源处理器

**步骤 1: 拷贝静态页面**

将资料 \ 功能页面 拷贝到项目的`webapp`目录下

![](<assets/1740015645732.png>)

**步骤 2: 访问 pages 目录下的 books.html，springmvc 对静态资源放行，SpringMvcSupport 配置类**

打开浏览器输入`http://localhost/pages/books.html`

![](<assets/1740015645967.png>)

**(1) 出现错误的原因?**

![](<assets/1740015646318.png>)

**报错原因：SpringMVC 拦截了静态资源**，根据 / pages/books.html 去 controller 找对应的方法，找不到所以会报 404 的错误。静态资源应该交给 Tomcat 处理，而不是 springmvc 处理。

**(2)SpringMVC 为什么会拦截静态资源呢?**

![](<assets/1740015646500.png>)

**(3) 解决方案：**SpringMVC 将**静态资源进行放行**。

使用资源处理器，当访问**请求路径** /pages/???? 时候，从**文件** /pages 目录下查找内容。

两种方法，主要用方法二。

**方法一：麻烦底层（了解）**

**①创建 SpringMvcSupport 配置类**，设置静态资源访问过滤。

继承 WebMvcConfigurationSupport 类，重写 addResourceHandlers 方法。

```
//注意有@Configuration，注解设为配置类。现在学到的配置类就它和springmvc和spring配置类有@Configuration
@Configuration
//继承webmvc配置支持类WebMvcConfigurationSupport
public class SpringMvcSupport extends WebMvcConfigurationSupport {
    //设置静态资源访问过滤，当前类需要设置为配置类，并被扫描加载
    @Override
    //addResourceHandlers译为添加资源处理器
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        //addResourceHandler("/pages/**")是拦截请求路径，addResourceLocations("/pages/")是映射到文件真实地址。这样就可以让别人访问服务器的本地文件了
        //当访问请求路径/pages/????时候，从文件/pages目录下查找内容。注意addResourceHandler里两个通配符，addResourceLocations后面是斜杠没通配符
        registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
        //当访问/js/????时候，从/js目录下查找内容
        registry.addResourceHandler("/js/**").addResourceLocations("/js/");
        registry.addResourceHandler("/css/**").addResourceLocations("/css/");
        registry.addResourceHandler("/plugins/**").addResourceLocations("/plugins/");
    }
}
```

**②SpringMvcConfig 扫描 SpringMvcSupport 配置类**

注意：**SpringMvcSupport** 配置类是在 config 目录下，SpringMVC 扫描的是 controller 包，所以该配置类还未生效，要想生效需要将 SpringMvcConfig 配置类进行修改

```
@Configuration
@ComponentScan({"com.itheima.controller","com.itheima.config"})
@EnableWebMvc
public class SpringMvcConfig {
}
 
或者
 
@Configuration
@ComponentScan("com.itheima")
@EnableWebMvc
public class SpringMvcConfig {
}
```

 **方法二：简单快速（推荐）**

 **SpringMvcConfig 实现 WebMvcConfigurer 接口**，既**可以添加资源处理器**，也可以**添加拦截器**，拦截器在下一章 ssm 整合中具体讲。既然不用 config 的 SpringMvcSupport 了，springmvc 配置类也就不用扫描 config 目录了

```
@Configuration
@ComponentScan({"package1.controller"})
@EnableWebMvc
public class SpringMvcConfig implements WebMvcConfigurer {
    @Autowired
    private ProjectInterception projectInterception;
    //添加拦截器，配置本地资源映射路径，在访问A（虚拟的）的时候，需要到B（实际的）的位置去访问。
//    @Override
//    public void addInterceptors(InterceptorRegistry registry) {
//        registry.addInterceptor(projectInterception).addPathPatterns("/books","/books/*");
        //如果只拦截/books，发送http://localhost/books/100后会发现拦截器没有被执行
        //registry.addInterceptor(projectInterceptor).addPathPatterns("/books");
//    }
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

使用资源处理器，当访问**请求路径** /pages/???? 时候，从**文件** /pages 目录下查找内容。

**步骤 3: 修改 books.html 页面**

```
<!DOCTYPE html>
 
<html>
    <head>
        <!-- 页面meta -->
        <meta charset="utf-8">
        <title>SpringMVC案例</title>
        <!-- 引入样式 -->
        <link rel="stylesheet" href="../plugins/elementui/index.css">
        <link rel="stylesheet" href="../plugins/font-awesome/css/font-awesome.min.css">
        <link rel="stylesheet" href="../css/style.css">
    </head>
 
    <body class="hold-transition">
 
        <div id="app">
 
            <div class="content-header">
                <h1>图书管理</h1>
            </div>
 
            <div class="app-container">
                <div class="box">
                    <div class="filter-container">
                        <el-input placeholder="图书名称" style="width: 200px;" class="filter-item"></el-input>
                        <el-button class="dalfBut">查询</el-button>
                        <el-button type="primary" class="butT" @click="openSave()">新建</el-button>
                    </div>
 
                    <el-table size="small" current-row-key="id" :data="dataList" stripe highlight-current-row>
                        <el-table-column type="index" align="center" label="序号"></el-table-column>
                        <el-table-column prop="type" label="图书类别" align="center"></el-table-column>
                        <el-table-column prop="name" label="图书名称" align="center"></el-table-column>
                        <el-table-column prop="description" label="描述" align="center"></el-table-column>
                        <el-table-column label="操作" align="center">
                            <template slot-scope="scope">
                                <el-button type="primary" size="mini">编辑</el-button>
                                <el-button size="mini" type="danger">删除</el-button>
                            </template>
                        </el-table-column>
                    </el-table>
 
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
 
                    <!-- 新增标签弹层 -->
                    <div class="add-form">
                        <el-dialog title="新增图书" :visible.sync="dialogFormVisible">
                            <el-form ref="dataAddForm" :model="formData" :rules="rules" label-position="right" label-width="100px">
                                <el-row>
                                    <el-col :span="12">
                                        <el-form-item label="图书类别" prop="type">
                                            <el-input v-model="formData.type"/>
                                        </el-form-item>
                                    </el-col>
                                    <el-col :span="12">
                                        <el-form-item label="图书名称" prop="name">
                                            <el-input v-model="formData.name"/>
                                        </el-form-item>
                                    </el-col>
                                </el-row>
                                <el-row>
                                    <el-col :span="24">
                                        <el-form-item label="描述">
                                            <el-input v-model="formData.description" type="textarea"></el-input>
                                        </el-form-item>
                                    </el-col>
                                </el-row>
                            </el-form>
                            <div slot="footer" class="dialog-footer">
                                <el-button @click="dialogFormVisible = false">取消</el-button>
                                <el-button type="primary" @click="saveBook()">确定</el-button>
                            </div>
                        </el-dialog>
                    </div>
 
                </div>
            </div>
        </div>
    </body>
 
    <!-- 引入组件库 -->
    <script src="../js/vue.js"></script>
    <script src="../plugins/elementui/index.js"></script>
    <script type="text/javascript" src="../js/jquery.min.js"></script>
    <script src="../js/axios-0.18.0.js"></script>
 
    <script>
        var vue = new Vue({
 
            el: '#app',
 
            data:{
                dataList: [],//当前页要展示的分页列表数据
                formData: {},//表单数据
                dialogFormVisible: false,//增加表单是否可见
                dialogFormVisible4Edit:false,//编辑表单是否可见
                pagination: {},//分页模型数据，暂时弃用
            },
 
            //钩子函数，VUE对象初始化完成后自动执行
            created() {
                this.getAll();
            },
 
            methods: {
                // 重置表单
                resetForm() {
                    //清空输入框
                    this.formData = {};
                },
 
                // 弹出添加窗口
                openSave() {
                    this.dialogFormVisible = true;
                    this.resetForm();
                },
 
                //添加
                saveBook () {
                    axios.post("/books",this.formData).then((res)=>{
 
                    });
                },
 
                //主页列表查询
                getAll() {
                    axios.get("/books").then((res)=>{
                        this.dataList = res.data;
                    });
                },
 
            }
        })
    </script>
</html>
```