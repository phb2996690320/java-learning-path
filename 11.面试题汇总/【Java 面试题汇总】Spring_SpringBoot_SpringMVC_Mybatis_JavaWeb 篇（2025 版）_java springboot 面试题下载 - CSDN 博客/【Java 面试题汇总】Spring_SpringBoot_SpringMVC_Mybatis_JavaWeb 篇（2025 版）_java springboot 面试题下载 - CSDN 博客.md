---
url: https://blog.csdn.net/qq_40991313/article/details/130263968
title: 【Java 面试题汇总】Spring,SpringBoot,SpringMVC,Mybatis,JavaWeb 篇（2025 版）_java springboot 面试题下载 - CSDN 博客
date: 2025-02-20 13:45:51
tag: 
summary: 
---
**导航：** 

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[一、Spring](#t0)

[二、SpringBoot](#t1) 

[2.1 说说你对 Spring Boot 的理解, 以及它和 Spring 的区别？](#t2)

[2.2 说说 Soring Boot 的起步依赖](#t3)

[2.3 说说 Spring Boot 的启动流程](#t4)

[2.4 说说 SpringBoot 的自动装配](#t5)

[2.5 说说 Spring Boot、SSM 常用的注解](#t6)

[2.6 SpringBoot 启动速度优化](#t7)

[三、SpringMVC](#t8) 

[3.1 说说你对 MVC 的理解](#t9)

[3.2 为什么 service 层要写接口？](#t10)

[3.3 介绍一下 Spring MVC 的执行流程](#t11)

[3.4 说说 RestFull 风格](#t12)

[四、Mybatis](#t13) 

[4.1 介绍一下 MyBatis 的缓存机制](#t14)

[4.2 在 MyBatis 中 $ 和 #有什么区别](#t15)

[4.3 resultType 和 resultMap 区别](#t16)

[五、JavaWeb](#t17)

[5.1 cookie 和 session 的区别是什么？](#t18)

[5.2 cookie 和 session 各自适合的场景是什么？](#t19)

[5.3 请介绍 session 的工作原理](#t20)

[5.4 get 请求与 post 请求有什么区别？](#t21)

[5.5 get 请求的参数能放到请求体吗？](#t22)

[5.6 页面报 400 错误是什么意思？](#t23)

[5.8 Maven 生命周期](#t24)

[5.9 socket 的过程](#t25) 

## 一、Spring

[【Java 常见面试题】Spring 篇](https://blog.csdn.net/qq_40991313/article/details/131207135 "【Java常见面试题】Spring篇")

## 二、SpringBoot 

### 2.1 说说你对 Spring Boot 的理解, 以及它和 Spring 的区别？

**得分点**

Spring Boot 与 Spring 的关系 、Spring Boot 的主要功能 、Spring Boot 的优点

**标准回答**

**与 Spring 的关系：**Spring Boot 是 Spring 的脚手架框架。

**优点：**快速构建项目、对主流框架的无配置集成、项目可独立运行，无需外部依赖 Servlet 容器、提供运行时的应用监控

**主要功能：**自动配置、起步依赖（一组关联度较高的依赖集合，并自动配置）、监控项目（Spring Boot Admin，监控性能、环境等信息，需要导入服务端和客户端的依赖和配置）

其实从本质上来说，**Spring Boot 就是 Spring**，它帮你完成了一些 Spring Bean 配置。Spring Boot 使用 **“习惯优于配置” 的理念**让你的项目快速地运行起来，使用 Spring Boot 能很快的创建一个能独立运行、准生产级别、基于 Spring 框架的项目。

但 Spring Boot 本身不提供 Spring 的核心功能，而是作为 **Spring 的脚手架框架**，达到**快速构建项目，预设第三方配置，开箱即用**的目的。Spring Boot 有很多优点，具体如下：

- 可以快速构建项目

- 可以对主流开发框架的无配置集成

- 项目可独立运行，无需外部依赖 Servlet 容器

- 提供运行时的应用监控

- 可以极大地提高开发、部署效率

- 可以与云计算天然集成

**加分回答**

Spring Boot 的核心功能：

1. 自动配置

针对很多 Spring 应用程序常见的应用功能，Spring Boot 能自动提供相关配置。

2. 起步依赖

Spring Boot 通过起步依赖为项目的依赖管理提供帮助。起步依赖其实就是**特殊的 Maven 依赖**和 Gradle 依赖，利用了**传递依赖解析**，把**常用库聚合在一起**，组成了几个为特定功能而定制的依赖。

3. 端点监控 Spring Boot 可以对正在运行的项目提供监控。

### 2.2 说说 Soring Boot 的起步依赖

**得分点**

starter 配置，约定大于配置

**标准回答**

**起步依赖：**一组关联度较高的依赖集合，用于快速搭建应用程序环境。 

**作用：**简化依赖 jar 包的导入、提供自动配置功能。

一些 starter 依赖的版本信息是由 spring-boot-starter-parent（版本仲裁中心） 统一控制，例如 spring-boot-starter 不用指定版本。

**约定大于配置：**

*   **自动装配：**springboot 会使用一些约定俗成的配置作为默认配置。例如自动扫描启动类所在包的 bean、自动加载类路径下的配置类、自动装配 starter 依赖（例如 redis 默认端口 6379，RabbitMQ 客户端是 5672 服务端 15672）。
*   **自动扫描引导类所在包下的 Bean：**Spring Boot 在启动时会自动扫描 @SpringBootApplication 注解所在的包及其子包，并自动注册所有标注了 @Bean 注解的 Bean 实例。不需要再配置 @ComponentScan("xx.xx");
*   **自动加载类路径下的配置文件：**Spring Boot 默认支持 application.properties 或 application.yml 格式的配置文件，并且只要这些文件放置到了 classpath 下，就会被自动识别并加载进来。

Spring Boot 的起步依赖是一种特殊的依赖关系，它可以**简化项目中依赖 jar 包的导入过程**，并提供了许多自动配置的功能。

所谓 "起步依赖"，指的是一组预先定义好的、常用的、**关联度较高的依赖集合**，通过引入这些起步依赖，就能方便地快速搭建一个特定类型的**应用程序环境**。

这个依赖集合**包含了许多与 Web 开发有关的 jar 包**（如 Spring MVC、Tomcat 等），并且还会自动进行一些**默认的配置**（如设置视图解析器、注册消息转换器等），从而使得我们无需手动集成这些 jar 包和进行这些配置操作，就能够快速地启动一个基础的 Web 应用程序。

Spring Boot 提供了多种起步依赖，覆盖了不同的应用程序场景，开发人员可以根据实际需要来选择引入不同的依赖集合。同时，Spring Boot 也支持自定义起步依赖，开发人员可以基于现有的依赖集合进行扩展，从而快速搭建出适合自己特定应用场景的开发环境。

Spring Boot 将日常企业应用研发中的各种场景都抽取出来，做成一个个的 starter（启动器），starter 中整合了该场景下各种可能用到的依赖，用户只需要在 Maven 中**引入 starter 依赖**，**SpringBoot 就能自动扫描到要加载的信息并启动相应的默认配置。**starter 提供了大量的自动配置，让用户摆脱了处理各种依赖和配置的困扰。所有这些 starter 都遵循着**约定成俗的默认配置**，并允许用户调整这些配置，即遵循 **“约定大于配置”** 的原则。

**约定大于配置**

Spring Boot 的核心设计理念就是 "约定大于配置"（Convention Over Configuration），即通过一些默认的配置和约定来减少开发人员在项目中进行复杂的配置操作，从而提高开发效率。

具体来讲，Spring Boot 在很多地方都使用了约定大于配置的思想：

1.  启动类的位置。Spring Boot 在启动时会自动扫描 @SpringBootApplication 注解所在的包及其子包，并自动注册所有标注了 @Bean 注解的 Bean 实例。
    
2.  默认配置文件。Spring Boot 默认支持 application.properties 或 application.yml 格式的配置文件，并且只要这些文件放置到了 classpath 下，就会被自动识别并加载进来。
    
3.  自动装配。Spring Boot 提供了自动配置功能，只需要引入相应的 starter 依赖，就可以由 Spring Boot 自动装配相关的依赖项、创建必要的 Bean 实例等。
    
4.  命名规范。Spring Boot 通过一些常见的命名规范，如目录布局规范、类 / 接口命名规范，使得程序的结构更加清晰，易于维护。
    

总之，Spring Boot 通过对常见应用场景的默认设置和约定，将大量的配置工作隐藏起来，提高了开发人员的生产力和开发效率。同时，如果需要进行定制化的配置，Spring Boot 也提供了丰富的扩展机制和自定义配置选项。

那么我们看构建的项目的 pom.xml 文件中的 starter 配置。

```
<dependency>
    <groupid>org.springframework.boot</groupid>
    <artifactid>spring-boot-starter-web</artifactid>
</dependency>
```

以 spring-boot-starter-web 为例，它能够为提供 Web 开发场景所需要的几乎所有依赖，因此在使用 Spring Boot 开发 Web 项目时，只需要引入该 Starter 即可，而不需要额外导入 Web 服务器和其他的 Web 依赖。

**加分回答**

有时在引入 starter 时，我们并不需要指明版本（version），这是因为 starter 版本信息是由 spring-boot-starter-parent（版本仲裁中心） 统一控制的。

### 2.3 说说 Spring Boot 的启动流程

**得分点**

调用 run 方法，run 方法执行流程

**标准回答**

Spring Boot 的启动流程：

1.  **准备环境**：
    1.  **启动监听器**： SpringBoot 包中 META-INF/spring.factories 文件中的一批 XxxApplicationListenner，包括清缓存、日志、文件编码等启动监听器
    2.  **默认不跳过 BeanInfo 属性**：spring.beaninfo.ignore 默认 true，即 Spring 在创建 Bean 时不会尝试加载每个类的 BeanInfo 信息（每个 Bean 可以对应一个实现 BeanInfo 接口的信息扩展类）
2.  **创建应用上下文**：如果是 web 环境，则创建注解上下文 AnnotationConfigApplicationContext，如果是非 web 环境，则创建泛型上下文 GenericWebApplicationContext
3.  **准备上下文**：给 Spring 容器注册 Bean 名生成器、启动参数等类，设置是否允许同名 Bean、懒加载的配置参数，加载 resources 目录下配置文件
    1.  **注册 Bean 名生成器、类型转换器**：注册用于生成 Bean 名的 Bean 和类型转换的 Bean。BeanNameGenerator（用于生成 Bean 的名称），ConversionService（用于类型转换）为 Bean。
    2.  **执行所有初始化方法**：即所有实现了 ApplicationContextInitializer 接口的类，执行他们的初始化方法。
    3.  **注册启动参数为 Bean**：将 main 函数中的 args 参数封装成单例 Bean，注册进 Spring 容器
    4.  **加载 resources**：获取并加载 resources 目录下所有资源的 Spring 上下文
4.  **刷新上下文**：调用 Spring 的 refresh() 方法，刷新上下文
    1.  **初始化引导类所在包下所有 Bean：**扫描引导类所在包和子包下的 Bean，基于类加载机制加载所有组件（如 Spring Cloud）和第三方库（如 Mybatis）
    2.  **自动配置：**@EnableAutoConfiguration 注册了一个 Selector 类，查找 classpath:/META-INF/spring.factories 文件中的相关配置信息，找到第三方 jar 包里的配置类，实现自动配置
    3.  **执行 Task 任务：**引导类 @EnableScheduling 后，该注解注册了定时任务 PostProcessor，和任务方法 @Scheduled(cron ="xx") 实现定时任务
    4.  **实例化非懒加载的单例 Bean**：获取所有的 bean，如果是单例且非懒加载的，就实例化
    5.  **完成刷新、启动服务器**：调用 getLifecycleProcessor().onRefresh()，启动 Tomcat 或其他嵌入式服务器
5.  **执行刷新后钩子**：留给用户扩展，目前 afterRefresh() 方法里面是空的。扩展方法是重写启动类，即 SpringApplication 子类重写这个方法
6.  **执行启动后任务**：执行用户自定义的实现了 ApplicationRunner 接口或 CommandLineRunner 接口的类的 run() 方法。

Spring Boot 的启动流程

1.  创建 Spring 应用程序上下文（ApplicationContext）：Spring Boot 使用内嵌的 Tomcat 或者 Jetty 服务器，所以启动时会自动创建一个 applicationContext。首先使用`Application.class`引导应用程序并创建 Spring 应用程序上下文。
    
2.  加载 Spring 组件和第三方库：Spring Boot 自动加载了大量常见依赖项，并对它们进行了管理。当应用程序启动时，Spring Boot 会通过基于条件的类加载机制（根据 classpath 路径、JVM 系统属性和环境变量判断是否加载某个类）加载所有需要的组件和第三方库。
    
3.  执行 auto-configuration：Spring Boot 通过查找 classpath 中的 META-INF/spring.factories 文件中的相关配置信息来实现自动配置，自动配置的类本质上是实现了 Spring 的`Condition`接口，该接口判断某些条件是否满足从而执行自动配置脚本，在初始化时检查条件后，将所判断的 Bean 注入到 Spring 容器中。
    
4.  进行 scheduled task、initializers 等相关操作：通过在 SpringBootConfig 类中加入 @EnableScheduling 来开启 scheduled task，在应用程序上下文加载时，我们还可以添加一些 ApplicationContextInitialize 类来进行必要的初始化操作。
    
5.  启动服务：最后，Spring Boot 使用内嵌的 Tomcat 或者 Jetty 服务器来启动正在运行的应用程序。
    

当 Spring Boot 项目创建完成后会默认生成一个 **Application 的入口类**，这个类中的 main 方法可以启动 Spring Boot 项目，在 **main 方法**中，通过 SpringApplication 的静态方法，即 **run 方法**进行 **SpringApplication 的实例化**操作，然后再针对实例化对象调用另外一个 run 方法来完成整个项目的初始化和启动。SpringApplication 调用的 run 方法重点做了以下操作：

- 获取监听器参数配置

- 打印 Banner 信息

- 创建并初始化容器

- 监听器发送通知

**加分回答**

SpringApplication 实例化过程中相对重要的配置：

- 项目启动类 SpringbootDemoApplication.class 设置为属性存储起来 this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources))；

- 设置应用类型是 SERVLET 应用还是 REACTIVE 应用

```
/**
 * @Author: vince
 * @CreateTime: 2024/05/14
 * @Description: 计算Bean加载时间处理器
 * @Version: 1.0
 */
@Component
public class BeanLoadingTimePostProcessor implements BeanPostProcessor {
    private static final Logger logger = LoggerFactory.getLogger(BeanLoadingTimePostProcessor.class);
 
    private Map<String, Long> startTimeMap = new ConcurrentHashMap<>();
 
    @Override
    public Object postProcessBeforeInitialization(@NotNull Object bean, @NotNull String beanName) {
        // 记录 Bean 初始化开始时间
        startTimeMap.put(beanName, System.currentTimeMillis());
        return bean;
    }
 
    @Override
    public Object postProcessAfterInitialization(@NotNull Object bean, @NotNull String beanName) {
        // 记录 Bean 初始化结束时间
        Long startTime = startTimeMap.get(beanName);
        if (startTime != null) {
            Long endTime = System.currentTimeMillis();
            long loadTime = endTime - startTime;
            // 打印 Bean 加载时间
            logger.info("Bean '" + beanName + "' 加载时间为 " + loadTime + " ms");
        }
        return bean;
    }
}
```

- 设置初始化器 (Initializer)，最后会调用这些初始化器

- 所谓的初始化器就是 org.springframework.context.ApplicationContextInitializer 的实现类，在 Spring 上下文被刷新之前进行初始化的操作

```
#yml开启二级缓存
mybatis:
 configuration:
 cache-enabled: true
```

- 设置监听器 (Listener)

```
<!-- 配置本mapper所在namespace的二级缓存，下面都是默认值 -->
<cache eviction="LRU" flushInterval="0" size="1024" readOnly="false"/>
```

- 初始化 mnApplicationClass 属性：用于推断并设置项目 mn() 方法启动的主程序启动类 this.mnApplicationClass = deduceMnApplicationClass()；

### 2.4 说说 SpringBoot 的自动装配

**得分点**

自动装配概念，自动装配流程

**标准回答**

**自动装配：**springboot 会自动把第三方组件 jar 包里的 Bean 装载到 IOC 器里面， 并配置初始化参数。

**@EnableAutoConfiguration：**在引导类注解 @SpringBootApplication 里，作用是开启自动配置。包括 @AutoConfigurationPackage、@Import（AutoConfigurationImportSelector.class）两个注解。

*   **@AutoConfigurationPackage：**扫描引导类所在包和子包下的所有 Bean，加载到 Spring 容器。里面有注解 @Import(AutoConfigurationPackages.Registrar)
*   **@Import（AutoConfigurationImportSelector.class）：导入所有自动配置类。**AutoConfigurationImportSelector 类实现了 ImportSelector 接口，它的 loadSpringFactories() 方法会找出 META-INF/spring.factories 文件里所有 key 为 "EnableAutoConfiguration" 的自动配置类，以列表的形式返回。

**自动配置过程：**

@EnableAutoConfiguration 的 @Import() 注解导入了 AutoConfigurationImportSelector 类，这个类实现了 ImportSelector 接口，它内部会调用 loadSpringFactories() 方法，加载 spring.factories 中注册的所有 AutoConfiguration 类到 Spring 容器。

使用 Spring Boot 时，我们需要引入对应的 Starters，Spring Boot 启动时便会**自动加载相关依赖**，配置相应的**初始化参数**，以最快捷、简单的形式对第三方软件进行集成，这便是 Spring Boot 的自动配置功能。

整个自动装配的过程是：Spring Boot 通过 **@EnableAutoConfiguration** 注解**开启自动配置**，加载 spring.factories 中注册的各种 **AutoConfiguration 类**，当某个 AutoConfiguration 类满足其注解 **@Conditional** 指定的生效条件（Starters 提供的依赖、配置或 Spring 容器中是否存在某个 Bean 等）时，实例化该 AutoConfiguration 类中定义的 **Bean**（组件等），并注入 Spring 容器，就可以完成依赖框架的自动配置。

**加分回答**

@EnableAutoConfiguration 作用

从 classpath 中搜索所有 META-INF/spring.factories 配置文件然后，将其中 org.springframework.boot.autoconfigure.EnableAutoConfiguration key 对应的配置项加载到 spring 容器 只有 spring.boot.enableautoconfiguration 为 true（默认为 true）的时候，才启用自动配置 @EnableAutoConfiguration 还可以根据 class 来排除（exclude），或是根据 class name（excludeName）来排除

其内部实现的关键点有

1. ImportSelector 该接口的方法的返回值都会被纳入到 spring 容器管理中

2. SpringFactoriesLoader 该类可以从 classpath 中搜索所有 META-INF/spring.factories 配置文件，并读取配置

### 2.5 说说 Spring Boot、SSM 常用的注解

**得分点**

Spring Boot 常用注解的作用

**标准回答**

启动、配置相关注解： 

*   @SpringBootApplication 里的 @EnableAutoConfiguration 开启自动配置
*   @Import 是自动配置的核心实现者
*   @Conditional 指定 AutoConfiguration 类里 bean 实例化的生效条件；
*   @Configuration 标注当前类是配置类
*   @ComponentScan 组件扫描和自动装配

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

Spring 相关：

*   @Component 注解为 Bean；
*   @Scope 设置 Bean 的作用域；
*   @Autowired 自动注入
*   @Transactional 事务注解

导入配置文件：

*   @PropertySource 导入 properties 文件；
*   @ImportResource 导入 xml 配置文件；
*   **@Import：**
    *   **导入普通类：**将该类作为配置类加载到容器；
    *   **导入实现了 ImportSelector 接口的类：**重写 selectImports 方法，返回 String[] 数组，数组里的每个全限定名对应的类都会注入到 spring 容器。
    *   **导入实现了 ImportBeanDefinitionRegister 接口的类：**重写 registerBeanDefinitions() 方法，手动往 beanDefinitionMap 中注册 beanDefinition。

关于 Spring Boot 常用注解：

**@SpringBootApplication 注解：**

在 Spring Boot 入口类中, 唯一的一个注解就是 @SpringBootApplication。它是 Spring Boot 项目的核心注解, 用于**开启自动配置**, 准确说是通过该注解内组合的 **@EnableAutoConfiguration** 开启了自动配置。

**@EnableAutoConfiguration 注解：**

@EnableAutoConfiguration 的主要功能是启动 Spring 应用程序上下文时进行**自动配置**, 它会尝试猜测并配置项目可能需要的 Bean。自动配置通常是基于项目 classpath 中引入的类和已定义的 Bean 来实现的。在此过程中, **被自动配置的组件**来自项目自身和项目依赖的 **jar 包**中。

**@Import 注解：**

@EnableAutoConfiguration 的关键功能是通过 @Import 注解导入的 ImportSelector 来完成的。从源代码得知 @Import(AutoConfigurationImportSelector.class) 是 @EnableAutoConfiguration 注解的组成部分, 也是**自动配置功能的核心实现者**。

**@Conditional 注解：**

@Conditional 注解是由 Spring 4.0 版本引入的新特性, 可**根据是否满足指定的条件来决定是否进行 Bean 的实例化及装配**, 比如, 设定当类路径下包含某个 jar 包的时候才会对注解的类进行实例化操作。总之, 就是根据一些特定条件来控制 Bean 实例化的行为。

### 2.6 SpringBoot 启动速度优化

**概述：**

*   **发现问题：**项目启动慢，需要四分钟；
*   **分析问题：**使用 IDEA 自带的火焰图，发现 Spring 容器的 refresh() 方法占用了 54% 的时长。refresh() 方法主要负责 Bean 的创建和初始化。
*   **定位问题：**实现 BeanPostProcessor 接口，自定义 Bean 的前后置处理器，统计所有 Bean 的加载时间，发现有个依赖的第三方 Dubbo 服务在项目启动过程中，饿汉式地注册了所有订阅的服务。
*   **解决问题：** 这些订阅的服务有些项目里用不到，所以我本地将这个配置类覆盖（基于双亲委派模型），只注入项目中用到的 Service。

**火焰图原理：**

1.  **采样分析：**在应用程序运行期间定期采样（例如每毫秒一次）调用栈，记录当前执行的函数。
2.  **整理数据：**将采样数据整理成调用栈的汇总信息，计算每个函数出现在调用栈中的次数。每个函数出现的次数表示其占用的 CPU 时间比例。
3.  **生成火焰图：**将整理好的数据以层级结构展示，每个矩形表示一个函数。矩形的宽度表示函数占用的 CPU 时间比例。矩形的纵向排列表示调用关系，上层矩形是下层矩形的调用者。

**详细：** 

**发现问题：**项目启动慢，需要四分钟；

**分析问题：**使用 IDEA 自带的火焰图，发现 Spring 容器的 refresh() 方法占用了 54% 的时长。refresh() 方法主要负责 Bean 的创建和初始化。

![](<assets/1740030351860.png>)

**定位问题：**实现 BeanPostProcessor 接口，自定义 Bean 的前后置处理器，统计所有 Bean 的加载时间，发现有个依赖的第三方 Dubbo 服务在项目启动过程中，饿汉式地注册了所有订阅的服务。

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--        映射配置文件 UserMapper.xml-->
<!--namespace名称空间，该命名空间和对应mapper接口的全限定名一致-->
<mapper namespace="package1.mapper.UserMapper">
    <select id="selectAll" resultType="package1.pojo.User">
        select * from tb_user;
    </select>
</mapper>
```

控制台打印出所有 Bean 的加载时间：

![](<assets/1740030352270.png>)

**解决问题：** 这些订阅的服务有些项目里用不到，所以我本地将这个配置类覆盖（基于双亲委派模型），只注入项目中用到的 Service。

## 三、SpringMVC 

### 3.1 说说你对 MVC 的理解

**得分点**

mvc 概念，model、view、controller 模块功能

**MVC：** MVC（Model-View-Controller）是一种 web 项目的设计模式，把软件分为模型（数据）、视图（用户界面）、控制器（处理模型和视图间请求和响应）三层，让前端和业务逻辑代码和数据分开。 

**SpringMVC：**SpringMVC 是在 Servlet 基础上构建并且使用 MVC 模式设计的一个 Web 框架，也只使用在 Web 中。

1.  **拆分 Controller：**控制器 Controller 拆分成前端控制器 DispatcherServlet 和后端控制器 Controller；
2.  **拆分 Model：**把模型 Model 拆分成业务层 Service 和数据访问层 Repository
3.  支持 JSP、Freemark 等视图；

**三层架构：**

*   **表现层 (UI)：**与用户交互的界面。可以用于接收用户输入的数据和显示处理后用户需要的数据。
*   **业务逻辑层 (BLL)：**是表现层和数据访问层的桥梁，实现业务逻辑处理。
*   **数据访问层 (DAL)：**关联着数据库。实现对数据的增删改查

**三层架构和 MVC 的区别：**

**从功能上看：**

*   三层架构是一个分层式的软件体系架构设计，适用于所有的项目。
*   MVC 模式是为了让前端和业务逻辑代码和数据分开，只使用在 web 项目中。

**从目的上看：**

*   三层架构侧重点是实现项目整体的解耦。
*   MVC 设计模式侧重点是 web 项目中前端页面和业务逻辑的解耦。

**从层次上看：**软件要先确定好框架，再进行设计模式。

*   三层架构是框架层面上的。
*   MVC 设计模式是设计模式层面上的。

**标准回答**

MVC 是一种设计模式，在这种模式下软件被分为三层，即 Model（模型）、View（视图）、Controller（控制器）。Model 代表的是数据，View 代表的是用户界面，Controller 代表的是数据的处理逻辑，Controller 是 Model 和 View 这两层的桥梁。将软件分层的好处是，可以将对象之间的**耦合度降低**，便于代码的维护。

**Model：**指从现实世界中**抽象出来的对象模型**，是应用逻辑的反应；它封装了数据和对数据的操作，是实际进行数据处理的地方（模型层与数据库才有交互）。在 MVC 的三个部件中，模型拥有最多的处理任务。被模型返回的数据是中立的，模型与数据格式无关，这样一个模型能为多个视图提供数据，由于应用于模型的代码只需写一次就可以被多个视图重用，所以减少了代码的重复性。

**View：**负责进行**模型的展示**，一般就是我们见到的用户界面。

**Controller：**控制器负责视图和模型之间的**交互**，控制对用户输入的响应、响应方式和流程；它主要负责两方面的动作，一是把用户的**请求分发**到相应的模型，二是把模型的改变及时地反映到视图上。

**加分回答 - 表现层、业务层、数据访问层、SpringMvc**

为了解耦以及提升代码的可维护性，服务端开发一般会对代码进行分层，服务端代码一般会分为三层：表现层、业务层、数据访问层。在浏览器访问服务器时，请求会先到达表现层

最典型的 MVC 就是 jsp+servlet+javabean 模式。

以 JavaBean 作为模型，既可以作为数据模型来封装业务数据，又可以作为业务逻辑模型来包含应用的业务操作。

JSP 作为视图层，负责提供页面为用户展示数据，提供相应的表单（Form）来用于用户的请求，并在适当的时候（点击按钮）向控制器发出请求来请求模型进行更新。

Serlvet 作为控制器，用来接收用户提交的请求，然后获取请求中的数据，将之转换为业务模型需要的数据模型，然后调用业务模型相应的业务方法进行更新，同时根据业务执行结果来选择要返回的视图。

当然，这种方式现在已经不那么流行了，**SpringMVC 框架**已经成为了 **MVC 模式的最主流实现**。

SpringMVC 框架是基于 Java **实现了 MVC 框架模式**的请求驱动类型的轻量级框架。前端控制器是 DispatcherServlet 接口实现类，映射处理器是 HandlerMapping 接口实现类，视图解析器是 ViewResolver 接口实现类，页面控制器是 Controller 接口实现类

### 3.2 为什么 service 层要写接口？

**依赖倒转原则：**高层模块不应该直接依赖于低层模块，两者都应该依赖于抽象。即详细应该依赖于抽象。在调用链上，调用者属于高层，被调用者属于低层。

因为 controller 需要调用 service，所以 controller 是高层模块，service 是低层模块。

所以 controller 层不能直接依赖于 service 层，他们都应该依赖于抽象，即 service 接口。

### 3.3 介绍一下 Spring MVC 的执行流程

**得分点**

浏览器、DispatcherServlet、Controller、ViewResolver 

SpringMvc 所有组件不能直接互相发送信息，都要经过前端控制器 DispatcherServlet 统一转发。

Spring MVC 的执行流程：

1.  **浏览器发请求**到 DispatcherServlet，DispatcherServlet 分发给后端控制器 **Controller**；
2.  **Controller** 里面处理完业务逻辑之后，把 **ModelAndView 对象**分发给 DispatcherServlet，DispatcherServlet 分发给**视图解析器** ViewResolver 。
3.  **视图解析器**把 ModelAndView 对象解析成**视图** View，发给 DispatcherServlet，DispatcherServlet 将模型填充到视图，分发给**浏览器**。

**标准回答**

SpringMVC 的执行流程如下。

1. 用户点击某个请求路径，发起一个 HTTP request 请求，该请求会被提交到前端控制器 (DispatcherServlet)；

2. 由 DispatcherServlet 请求一个或多个处理器映射器 (HandlerMapping)，并返回一个执行链 (HandlerExecutionChn)。

3. DispatcherServlet 将执行链返回的 Handler 信息发送给处理器适配器 (HandlerAdapter)；

4. HandlerAdapter 根据 Handler 信息找到并执行相应的 **Handler(常称为 Controller)；**

5. Handler 执行完毕后会返回给 HandlerAdapter 一个 **ModelAndView 对象** (Spring MVC 的底层对象，**包括 Model 数据模型和 View 视图信息**)；

6. HandlerAdapter 接收到 ModelAndView 对象后，将其返回给 DispatcherServlet ；

7. DispatcherServlet 接收到 ModelAndView 对象后，会请求**视图解析器** (ViewResolver) 对视图进行**解析**；

8. ViewResolver 根据 View 信息匹配到相应的视图结果，并返回给 DispatcherServlet；

9. DispatcherServlet 接收到具体的 View 视图后，进行视图渲染，将 Model 中的模型数据填充到 View 视图中的 request 域，生成最终的 View(视图)；

10. 视图负责将结果显示到浏览器 (客户端)。

**加分回答**

Spring MVC 涉及到的**组件**有 DispatcherServlet（前端控制器）、HandlerMapping（处理器映射器）、HandlerAdapter（处理器适配器）、Handler（处理器）、ViewResolver（视图解析器）和 View（视图）。

下面对各个组件的功能说明如下：

**DispatcherServlet**

DispatcherServlet 是**前端控制器**，从图 1 可以看出，Spring MVC 的**所有请求**都要经过 DispatcherServlet 来**统一分发**。DispatcherServlet 相当于一个转发器或**中央处理器**，控制整个流程的执行，对各个组件进行统一调度，以降低组件之间的耦合性，有利于组件之间的拓展。

**HandlerMapping**

HandlerMapping 是**处理器映射器**，其作用是根据请求的 URL 路径，通过注解或者 XML 配置，**寻找匹配的处理器**（Handler）**信息**。

**HandlerAdapter**

HandlerAdapter 是**处理器适配器**，其作用是根据映射器找到的处理器（Handler）信息，按照特定规则**执行**相关的**处理器**（Handler）。

**Handler（Controller）**

Handler 是**处理器**，和 Java Servlet 扮演的角色一致。其作用是**执行**相关的**请求处理逻辑**，并**返回**相应的数据和视图信息，将其封装至 **ModelAndView 对象**中。

**View Resolver**

View Resolver 是**视图解析器**，其作用是进行解析操作，通过 **ModelAndView 对象**中的 View 信息将逻辑视图名**解析成**真正的视图 **View**（如通过一个 JSP 路径返回一个真正的 JSP 页面）。

**View**

View 是**视图**，其本身是一个接口，实现类支持不同的 View 类型（JSP、FreeMarker、Excel 等）。

以上组件中，需要**开发人员**进行开发的是**处理器**（Handler，常称 Controller）和**视图**（View）。通俗的说，要开发处理该请求的具体代码逻辑，以及最终展示给用户的界面。

### 3.4 说说 RestFull 风格

**Rest：**一种接口设计风格，隐藏资源的访问行为。示例 http://localhost/v1/user/1。

**RESTful：**遵循 REST 风格访问资源。

*   **URI 命名：**模块名用复数、名词，命名时对应数据库表名。增删改查行为隐藏，用请求方式区分。
*   **请求方式：**增删改查分别用 POST,DELETE,PUT,GET。
*   **版本号：**将 API 的版本号放入 URL 中。示例 http://localhost/v1/user/1。
*   **搜索条件：**搜索条件的请求参数放在请求头，例如 http://localhost/v1/user?page=2&pageSize=100

### 3.5 Restful 风格和一律用 POST 请求，哪个更好？

存在分歧，POST 更安全，Restful 更符合规范。

微信支付统一 REST 风格，而支付宝支付统一 POST 请求；

“**Get 请求可以通过构造 img 等标签发起，造成 CSRF、 CSRF 漏洞。**”，img 标签可以发起一次 GET 请求，无法发起 POST 请求。

CSRF 漏洞：跨站请求伪造 (Cross-Site Request Forgery, CSRF)，恶意网站通过脚本向当前用户浏览器打开的其它页面的 URL 发起恶意请求，由于同一浏览器进程下 Cookie 可见性，导致用户身份被盗用，完成恶意网站脚本中指定的操作。

不用 GET 请求可以避免部分的恶意请求，当然也不是说 POST 就不会有 CSRF，有个非常经典的案例：Gmail, 在 2007 年底也因 CSRF 漏洞而被黑客攻击，这个就是 POST 做的。为了避免 CSRF 漏洞，通常会采用 token、referer、验证码这些手段。

![](<assets/1740030352586.png>)

![](<assets/1740030352907.png>)

## 四、Mybatis 

### 4.1 介绍一下 MyBatis 的缓存机制

**得分点**

一、二级缓存概念

**两级缓存都不是线程安全的**，推荐分布式场景下使用 Redis 等分布式缓存。 默认情况下只开启一级缓存。

**一级缓存：**

*   **缓存级别：**SqlSession 级别（单个会话）的缓存，仅支持单个会话内缓存。SqlSession 将查询结果缓存在内存中，再次相同查询时 MyBatis 会从缓存中获取数据。
*   **生命周期：**生命周期和 SqlSession 一致。
*   **失效模式：**增删改或 SqlSession 改变时，一级缓存会失效。
*   **底层：**一个简单的没有容量限制的 HashMap。每次查询时 SqlSession 对象里的 Executor 先从 Local Cache 里查，查不到再查数据库写到缓存里和缓存。

**二级缓存：**

*   **缓存级别：**SqlSessionFactory 级别（跨会话）、mapper 级别、namespace 级别的缓存，要手动开启。会缓存当前映射配置文件下的所有 SqlSession 查到的数据，可以指定缓存容量，多个 SqlSession 之间缓存可以共享。
    
    ```
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE mapper
            PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
     
    <mapper namespace="com.itheima.mapper.BrandMapper">
        <!--给列名起别名，让列名和成员变量一致，实现赋值-->
    <resultMap id="brandResultMap" type="brand">
        <!--
                id：完成主键字段的映射
                    column：表的列名
                    property：实体类的属性名
                result：完成一般字段的映射
                    column：表的列名
                    property：实体类的属性名
            -->
        <result column="brand_name" property="brandName"/>
        <result column="company_name" property="companyName"/>
    </resultMap>
        <select id="selectAll" resultMap="brandResultMap">
            select *
            from tb_brand;
        </select>
    </mapper>
    ```
    
*   **配置：**
    *   **刷新间隔：**默认是 0，即增删改后立即刷新缓存。
    *   **淘汰策略：**LRU（默认）、FIFO、SOFT（软，内存快慢时回收）、WEAK（弱，下次垃圾回收时回收）。
    *   **大小：**默认 1M
    *   **是否只读：**默认 false。
        *   **只读：**给所有调用者返回同一个缓存实例。只要一个调用者改了实例，其他调用者访问的实例也更改了。性能高但不安全。
        *   **关闭只读：**给每个调用者返回缓存实例的拷贝。本调用者修改自己的拷贝不会影响到其他调用者。性能差但安全，所以默认。
    *   ```
        <!-- 配置本mapper所在namespace的二级缓存，下面都是默认值 -->
        <cache eviction="LRU" flushInterval="0" size="1024" readOnly="false"/>
        ```
        
*   **粒度：**粒度更加的细，能够到 namespace 级别。也就是说缓存映射语句文件中所有查询语句。namespace 在映射配置文件 <mapper namespace="package1.mapper.UserMapper">，用来找到对应的 mapper 接口。
*   **可控性：**可以通过 Cache 接口实现自定义缓存。实现方法是新建 @Component 注解的实现类，重写 get(),put(),putIfAbsent(),evict(),clear(),getName() 等方法。 
*   **底层：**先通过 SqlSession 里的 CachingExecutor 查全局的二级缓存，如果查不到再查一级缓存。

![](<assets/1740030353193.png>)

**标准回答**

**SqlSession** 负责管理持久层的一次**数据库操作会话**。在使用 MyBatis 进行数据库操作时，每个线程都应该拥有自己的 SqlSession 实例。SqlSession 对象不是线程安全的。 

```
List<User> users = sqlSession.selectList("namespace.selectAll");
```

**一级缓存也称为本地缓存**，它**默认启用且不能关闭**。一级缓存存在于 SqlSession 的生命周期中，即它是 **SqlSession 级别的缓存**。在同一个 SqlSession 中查询时，MyBatis 会把执行的方法和参数通过算法生成缓存的键值，将键值和查询结果存入一个 Map 对象中。如果同一个 SqlSession 中执行的方法和参数完全一致，那么通过算法会生成相同的键值，当 Map 缓存对象中己经存在该键值时，则会返回缓存中的对象。

SqlSessionFactory 是用于创建 SqlSession 对象的工厂。

```
SqlSession sqlSession = sqlSessionFactory.openSession();
```

**二级缓存**存在于 SqlSessionFactory 的生命周期中，即它是 **SqlSessionFactory 级别的缓存**。若想使用二级缓存，需要在如下两处进行配置：

- 在 MyBatis 的**全局配置 settings** 中有一个**参数 cacheEnabled**，这个参数是二级缓存的全局开关，默认值是 true ，初始状态为启用状态。

- MyBatis 的二级缓存是**和命名空间绑定的**，即二级缓存需要**配置在 Mapper.xml 映射文件**中。

**配置二级缓存：**

mybatis-config.xml

```
<setting name="cacheEnabled" value="true"/>
```

Mapper.xml

```
<cache eviction="LRU" flushInterval="60000" size="512" readOnly="true"/>
```

其中，eviction 属性用来指定缓存淘汰策略，flushInterval 属性表示缓存刷新时间间隔，size 属性表示缓存的大小，readOnly 属性表示缓存是否只读等等。 

**二级缓存具有如下效果：**

- 映射语句文件中的**所有 SELECT 语句将会被缓存**。

- 映射语句文件中的所有 INSERT 、UPDATE 、DELETE 语句会**刷新缓存**。

- 缓存会使用 Least Recently Used ( LRU ，**最近最少使用的**）算法来**收回**。

- 根据时间表（如 no Flush Interval ，没有刷新间隔），缓存不会以任何时间顺序来刷新。

- 缓存会存储集合或对象（无论查询方法返回什么类型的值）的 1024 个引用。

- 缓存会被视为 read/write（可读／可写）的，意味着对象检索不是共享的，而且可以安全地被调用者修改，而不干扰其他调用者或线程所做的潜在修改。

**加分回答** -**Mybatis 一级缓存失效的四种情况：**

- **sqlsession 变了 缓存失效**

- sqlsession 不变，查询条件不同，一级缓存失效

**-** sqlsession 不变，中间发生了**增删改操作，一级缓存失败**

- sqlsession 不变，手动清除缓存，一级缓存失败

**两级缓存不是线程安全的**

MyBatis 的二级缓存相对于一级缓存来说，实现了 SqlSession 之间缓存数据的共享，同时粒度更加的细，能够到 namespace 级别，通过 Cache 接口实现类不同的组合，对 Cache 的可控性也更强。

MyBatis 在多表查询时，极大可能会出现脏数据，这导致安全使用二级缓存的条件比较苛刻。 由于默认的 MyBatis Cache 实现都是基于本地的，这导致在分布式环境下，一定会出现读取到脏数据的情况，且开发成本较高，所以开发环境中一般都会直接使用 Redis，Memcached 等分布式缓存.

### 4.2 在 MyBatis 中 $ 和 #有什么区别

**得分点**

使用传参方式 MyBatis 创建 SQL 的不同，安全和效率问题

使用 **$** 设置参数时，MyBatis 会创建**普通的 SQL 语句**，然后在执行 SQL 语句时将**参数拼入 SQL**；

使用**#**设置参数时，MyBatis 会创建**预编译的 SQL 语句**，然后在执行 SQL 时 MyBatis 会为预编译 SQL 中的**占位符赋值**。

**优缺点：** 

**预编译**的 SQL 语句执行**效率高**，并且可以**防止注入攻击**，效率和安全性都大大优于前者。

但在解决一些特殊问题，如在一些根据不同的条件产生不同的动态列中，我们要传递 SQL 的列名，根据某些列进行排序，或者传递列名给 SQL 就只能使用 $ 了。

### 4.3 resultType 和 resultMap 区别

resultType 和 resultMap 都用于设置 mybatis 增删改查后返回的数据类型，两者只能使用一个。 

resultType：指定实体类的全限定名或首字母小写的类名，MyBatis 会自动将查询的结果映射成对应实体类。适用于数据库字段名和实体类属性名一致的情况。

resultMap： 给列起别名并查询所有。适用于数据库字段名和实体类属性名不同的情况。mybatis 默认会进行驼峰转换。

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--        映射配置文件 UserMapper.xml-->
<!--namespace名称空间，该命名空间和对应mapper接口的全限定名一致-->
<mapper namespace="package1.mapper.UserMapper">
    <select resultType="package1.pojo.User">
        select * from tb_user;
    </select>
</mapper>
```

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
 
<mapper namespace="com.itheima.mapper.BrandMapper">
    <!--给列名起别名，让列名和成员变量一致，实现赋值-->
<resultMap type="brand">
    <!--
            id：完成主键字段的映射
                column：表的列名
                property：实体类的属性名
            result：完成一般字段的映射
                column：表的列名
                property：实体类的属性名
        -->
    <result column="brand_name" property="brandName"/>
    <result column="company_name" property="companyName"/>
</resultMap>
    <select resultMap="brandResultMap">
        select *
        from tb_brand;
    </select>
</mapper>
```

## 五、[JavaWeb](https://so.csdn.net/so/search?q=JavaWeb&spm=1001.2101.3001.7020)

### 5.1 cookie 和 session 的区别是什么？

*   **存储位置不同：**cookie 存放于客户端；session 存放于服务端。
    
*   **存储容量不同：**单个 cookie 保存的数据 <=**4KB**，一个站点最多保存 **20 个** cookie；而 session 并没有上限。
    
*   **存储方式不同：**cookie 只能保存 ASCII 字符串，并需要通过编码当时存储为 Unicode 字符或者二进制数据；session 中能够存储任何类型的数据，例如字符串、整数、集合等。
    
*   **隐私策略不同：**cookie 对客户端是可见的，别有用心的人可以分析存放在本地的 cookie 并进行 cookie 欺骗，所以它是不安全的；session 存储在服务器上，对客户端是透明的，不存在敏感信息泄露的风险。
    
*   **生命周期不同：**可以通过设置 cookie 的属性，达到 cookie 长期有效的效果；session 依赖于名为 JSESSIONID 的 cookie，而该 cookie 的默认过期时间为 - 1，只需关闭窗口该 session 就会失效，因此 session 不能长期有效。
    
*   **服务器压力不同：**cookie 保存在客户端，不占用服务器资源；session 保管在服务器上，**每个用户都会产生一个 session**，如果并发量大的话，则会消耗大量的服务器内存。
    
*   **浏览器支持不同：**cookie 是需要浏览器支持的，如果客户端禁用了 cookie，则会话跟踪就会失效；运用 session 就需要使用 URL 重写的方式，所有用到 session 的 URL 都要进行重写，否则 session 会话跟踪也会失效。
    
*   **跨域支持不同：**cookie 支持跨域访问，session 不支持跨域访问。
    

### 5.2 cookie 和 session 各自适合的场景是什么？

**session：**对于敏感数据，应存放在 session 里，因为 cookie 不安全。

**cookie：**对于普通数据，优先考虑存放在 cookie 里，这样会减少对服务器资源的占用。

### 5.3 请介绍 session 的工作原理

1.  客户端首次访问服务器，服务器创建具有唯一标识 SESSIONID 的 session 对象；
2.  响应阶段，服务器创建一个携带 SESSIONID 的 cookie，并响应给客户端；
3.  客户端获取响应到的 cookie，再次访问服务器时携带 SESSIONID，获取对应 session 对象里的信息。

### 5.4 get 请求与 post 请求有什么区别？

*   **书签标记：**GET 产生的 URL 地址可以被 Bookmark（书签标记），而 POST 不可以。
    
*   **参数历史记录保留：**GET 请求参数会被完整保留在浏览器历史记录里，而 POST 中的参数不会被保留。
    
*   **浏览器主动缓存：**GET 请求会被浏览器主动 cache，而 POST 不会，除非手动设置。
    
*   **编码格式：**GET 请求只能进行 url 编码，而 POST 支持多种编码方式。
    
*   **参数长度限制：**GET 请求在 URL 中传送的参数是有长度限制的，而 POST 没有。
    
*   **参数类型限制：**对参数的数据类型，GET 只接受 ASCII 字符，而 POST 没有限制。
    
*   **安全性：**GET 比 POST 更不安全，因为参数直接暴露在 URL 上，所以不能用来传递敏感信息。
    
*   **请求参数位置：**GET 参数通过 URL 传递，URL 在请求行里；POST 在请求体里。
    
*   **幂等性：**post 不幂等，而 GET，PUT，DELETE 都是幂等的。因为 post 是保存新记录，post 几次就创建几次，而 get 只读不会影响数据，put 是修改，多次修改都是修改成固定的值，delete 是删除，多次删除只是删除已经被删除的数据。
    

### 5.5 get 请求的参数能放到请求体吗？

GET 请求是可以将参数放到 BODY 里面的，**官方并没有明确禁止**，但给出的建议是这样**不符合规范**，无法保证所有的实现都支持。这就意味着，如果你试图这样做，可能出现各种未知的问题，所以应该当避免。

### 5.6 页面报 400 错误是什么意思？

**参考答案**

400 状态码标识请求的语义有误，当前**请求无法被服务器理解**。除非进行修改，否则客户端不应该重复提交这个请求。通常情况下，是本次请求中**包含有错误的参数**，此时应该排查前端传递的参数。

### 5.8 Maven 生命周期

*   **clean：**清理上次构建生成的 jar 包
*   **validate：**校验工程信息是否正确
*   **comple：**编译工程
*   **test：**使用 JUnit 等单元测试工具测试工程 (标注的 test 的程序)
*   **package：**打包工程成 jar 包
*   **verify：**验证 jar 包是否符合规范
*   **install：**安装 jar 包到本地仓库
*   **deploy：**推送 jar 包到远程仓库

### 5.9 socket 的过程 

Socket 是网络通信中使用的一种机制，它可以实现两个不同的**进程之间的通信**。

**Socket 的通信过程：**

1.  **服务器绑定端口号：**服务器端启动，绑定一个端口号，等待客户端的连接请求。
2.  **客户端指定端口号发请求：**客户端启动，向服务器端发送连接请求，并指定服务器的 IP 地址和端口号。
3.  **服务器创建 Socket 对象：**服务器端接收到客户端的连接请求后，为该连接创建一个新的 Socket 对象，同时服务器端继续等待其他客户端的连接请求。
4.  **客户端创建 Socket 对象：**客户端接收到服务器端的响应后，为该连接创建一个新的 Socket 对象，然后通过该 Socket 对象与服务器端进行通信。
5.  **通信：**服务器端和客户端通过各自的 Socket 对象进行通信，直到通信结束。
6.  **关闭 Socket 对象：**通信结束后，客户端和服务器端分别关闭自己的 Socket 对象，释放网络资源。

在通信过程中，Socket 对象扮演着重要的角色，它通过网络传输数据并完成通信。在 Java 中，可以使用 Java Socket API 实现 Socket 通信。