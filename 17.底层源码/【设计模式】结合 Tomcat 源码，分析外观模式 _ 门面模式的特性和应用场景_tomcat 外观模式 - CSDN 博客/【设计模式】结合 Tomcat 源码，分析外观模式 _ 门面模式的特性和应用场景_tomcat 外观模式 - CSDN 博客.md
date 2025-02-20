---
url: https://blog.csdn.net/qq_40991313/article/details/143056317
title: 【设计模式】结合 Tomcat 源码，分析外观模式 / 门面模式的特性和应用场景_tomcat 外观模式 - CSDN 博客
date: 2025-02-20 13:59:29
tag: 
summary: 
---
**导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289 "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[一、经典的组建家庭影院流程](#t0)

[二、传统方式解决影院管理](#t1)

[2.1 实现方案：客户端直接调用各流程](#t2)

[2.2 优缺点和改进思路](#t3)

[三、外观模式](#t4)

[3.1 外观模式 / 门面模式](#t5)

[3.2 外观类、子系统和客户端](#t6)

[3.3 优缺点和适用场景](#t7)

[四、外观模式解决影院管理](#t8)

[4.1 实现方案：将各流程组装到外观类](#t9)

[4.2 核心代码](#t10)

[4.2.1 子系统：投影仪、DVD 等类](#t11)

[4.2.2 外观类：家庭影院外观类](#t12)

[4.2.3 客户端：家庭影院的准备、观看、结束](#t13)

[五、外观模式在三层架构中的应用](#t14)

[5.1 三层架构和 MVC 设计模式](#t15)

[5.1.1 三层架构](#t16)

[5.1.2 MVC 设计模式](#t17)

[5.2 外观模式在三层架构中的应用](#t18)

[六、外观模式在 Tomcat 源码中的应用](#t19) 

[6.1 外观类：ApplicationContextFacade](#t20)

[6.2 子系统：Map](#t21)

[6.3 客户端：ApplicationContext](#t22)

[6.4 Tomcat 和 Spring 的 ApplicationContext 区别](#t23)

## 一、经典的组建家庭影院流程

**问题描述：**

*   **组建家庭影院过程**：
    1.  直接用遥控器：统筹各设备开关
    2.  开爆米花机
    3.  放下屏幕
    4.  开投影仪
    5.  开音响
    6.  开 DVD，选 dvd
    7.  去拿爆米花
    8.  调暗灯光
    9.  播放
    10.  观影结束后，关闭各种设备
*   **需求**：组建一个电影院，要求完成上述流程

## **二、传统方式解决影院管理**

### 2.1 实现方案：客户端直接调用各流程

![](<assets/1740031169673.png>)

**各个电器类（子系统）**：包括打开、关闭等功能

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/23
     * @Description: DVD类
     * @Version: 1.0
     */
    public class DVDPlayer{
        /**
         * 播放
         */
        public void play(){
            System.out.println("播放DVD");
        }
 
        /**
         * 打开
         */
        public void on(){
            System.out.println("打开DVD");
        }
        /**
         * 关闭
         */
        public void off(){
            System.out.println("关闭DVD");
        }
 
    }
 
    /**
     * @Author: vince
     * @CreateTime: 2024/10/23
     * @Description: 投影仪
     * @Version: 1.0
     */
    public class Projector{
        public void on() {
            System.out.println("打开投影仪");
        }
    }
    // ...
```

**客户端**：

```
/**
 * @Author: vince
 * @CreateTime: 2024/10/23
 * @Description: 客户端测试类
 * @Version: 1.0
 */
public class ClientTest{
    public static void main(String[] args){
        // 1、创建对象：创建各电器类对象
        // 2、打开电器：调用各对象的on()方法
        // 3、放映：调用DVDPlayer对象的play方法
        // 4.关闭：关闭所有电器
    }
 
}
```

### 2.2 优缺点和改进思路

**优点：**

*   **简单：**比较好理解，简单易操作

**缺点：**

*   **调用过程混乱**：客户端创建各个子系统的对象，并直接去调用子系统（对象）相关方法，会造成调用过程混乱，没有清晰的过程
*   **无法直接维护子系统**：不利于在 ClientTest 中去维护对子系统的操作

**改进思路分析：** 

*   **抽取界面类**：定义一个高层接口（界面类），给子系统接口提供一致的界面（例如 ready，play，pause，end 等方法），屏蔽内部子系统的细节。

## 三、外观模式

### 3.1 外观模式 / 门面模式

**外观模式（Facade）**：也叫过程模式 / 门面模式，是一种结构型设计模式。外观模式通常创建一个外观类，将子系统类的多个流程方法组装起来。

外观模式通过封装复杂的子系统接口，提供一个简化的统一接口，减少了客户端与子系统的直接依赖，降低了耦合性。

 **UML 类图**

![](<assets/1740031169778.png>)

**结构型模式**（Structural Patterns）：

主要用于描述对象之间的**组合**关系**。**包括 “代理模式”、“适配器模式”、“桥接模式”、“装饰者模式”、“外观模式”、“享元模式” 和“组合模式”等。这些模式可以帮助我们更好地**设计程序结构**，提高代码的灵活性和可维护性。

外观模式将多个子系统通过界面接口统一访问，改变了各个子系统与客户端之间的组合关系，所以是结构型设计模式。

### 3.2 外观类、子系统和客户端

**外观模式的三个角色**：

*   **外观类**（Facade）：提供统一的界面类，组装子系统的各个流程。例如软件安装外观类有个 “一键安装” 方法，组装了选择安装目录、选择组件、开启开机自启动等子流程。
*   **子系统**：各功能的实际执行者。例如系统服务类设置开机自启动。
*   **客户端**（Client）：调用者。客户端通过调用外观类提供的简化接口，与各子系统交互，从而实现功能。例如点击外观对象的 “一键安装” 方法，一键安装软件。

**举例**：

*   **一键安装**：在 PC 上安装软件的时候经常有一键安装选项，省去选择安装目录、安装的组件、选择开机自启动、选择是否生成快捷方式等步骤
*   **重启手机**：手机的关机选项，就是把关机和启动组合为一个操作

### 3.3 优缺点和适用场景

**优点：**

*   **简化用户操作**：将具体的各个流程组装到外观类中，这样客户端调用时，直接调用外观类的方法即可。
*   **高内聚低耦合**：将子系统的实现细节隐藏在外观类之后，客户端只与外观类交互，降低了系统各部分之间的耦合性。
*   **分层结构**：外观模式可以作为上层系统与下层子系统交互的中间层，简化每一层之间的依赖。
*   **性能高**：将各个子系统流程的分别调用，改为外观类一次性调用，减少网络通信成本，提高 了客户端的响应速度。
*   **易于维护**：当需要修改流程时，只需要维护外观类的方法，调换各个流程，不需要关注各个子系统的具体实现。

**缺点：**

*   **掩盖子系统复杂性**：将子系统的实现细节隐藏在外观类之后，开发者可能对各个子系统的具体实现细节了解不够深刻。 
*   **过度设计**：一些场景子系统并不多，而且流程简单，未来也不需要扩展新的流程，用外观模式就会有些过度设计。这也是众多设计模式的缺点，需要充分考虑适用场景

**使用场景：**

*   **子系统各流程复杂**：当子系统很多时，为了防止遗留流程，并且方便管理各个流程的前后顺序，可以使用外观模式
*   **兼容旧系统功能**：在维护一个遗留的大型系统时，可能这个系统已经变得非常难以维护和扩展，此时可以考虑为新系统开发一个 Facade 类，来提供遗留系统的比较清晰简单的接口，让新系统与 Facade 类交互，提高复用性
*   **三层架构**：Java 项目中，我们常常将 Dao 注入到 Service，Service 将多个 Dao 组合在一起，这其实也用到了外观模式。

## 四、外观模式解决影院管理

### **4.1 实现方案：将各流程组装到外观类**

![](<assets/1740031169863.png>)

### **4.2 核心代码**

#### **4.2.1** 子系统：投影仪、DVD 等类

子系统类主要包括投影仪、DVD 等类，拥有打开、关闭等功能（普通方法） ：

**投影仪类：** 

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/23
     * @Description: 投影仪
     * @Version: 1.0
     */
    public class Projector {
        private static Projector projector = new Projector();
 
        public static Projector getInstance() {
            return projector;
        }
 
        public void on() {
            System.out.println("打开投影仪...");
        }
 
        public void off() {
            System.out.println("关闭投影仪...");
        }
 
        public void focus() {
            System.out.println("投影仪聚焦...");
        }
 
        public void zoom() {
            System.out.println("投影仪放大...");
        }
    }
```

**DVD 播放器类：**

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/23
     * @Description: DVD类
     * @Version: 1.0
     */
    public class DVDPlayer {
        private static DVDPlayer player = new DVDPlayer();
 
        public static DVDPlayer getInstance() {
            return player;
        }
 
        public void on() {
            System.out.println("打开DVD播放器...");
        }
 
        public void off() {
            System.out.println("关闭DVD播放器...");
        }
 
        public void play() {
            System.out.println("播放DVD播放器...");
        }
 
        public void pause() {
            System.out.println("暂停DVD播放器...");
        }
 
        public void setDvd(String dvd) {
            System.out.println("选dvd：" + dvd + "...");
        }
    }
```

**荧幕**

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/24
     * @Description: 荧幕类
     * @Version: 1.0
     */
    public class Screen {
        private static Screen screen = new Screen();
 
        public static Screen getInstance() {
            return screen;
        }
 
        public void up() {
            System.out.println("升起荧幕...");
        }
 
        public void down() {
            System.out.println("拉下荧幕...");
        }
    }
```

**音响**

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/24
     * @Description: 音响类
     * @Version: 1.0
     */
    public class Stereo {
        private static Stereo stereo = new Stereo();
 
        public static Stereo getInstance() {
            return stereo;
        }
 
        public void on() {
            System.out.println("打开立体声...");
        }
 
        public void off() {
            System.out.println("关闭立体声...");
        }
 
        public void setVolume(Integer volume) {
            System.out.println("立体声音量+" + volume + "...");
        }
    }
```

**灯光**

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/24
     * @Description: 灯光类
     * @Version: 1.0
     */
    public class TheaterLights {
        private static TheaterLights lights = new TheaterLights();
 
        public static TheaterLights getInstance() {
            return lights;
        }
 
        public void on() {
            System.out.println("打开灯光...");
        }
 
        public void off() {
            System.out.println("关闭灯光...");
        }
 
        public void dim() {
            System.out.println("调暗灯光...");
        }
 
        public void bright() {
            System.out.println("调亮灯光...");
        }
    }
```

**爆米花机器**

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/24
     * @Description: 爆米花机
     * @Version: 1.0
     */
    public class Popcorn {
        private static Popcorn popcorn = new Popcorn();
 
        public static Popcorn getInstance() {
            return popcorn;
        }
 
        public void on() {
            System.out.println("打开爆米花机器...");
        }
 
        public void off() {
            System.out.println("关闭爆米花机器...");
        }
 
        public void pop() {
            System.out.println("取出爆米花...");
        }
    }
```

#### **4.2.2** 外观类：家庭影院外观类

外观类包括一个或多个组装方法，将各个流程（子系统类的方法）组装起来。 

家庭影院 Facade

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/24
     * @Description: 外观类：家庭影院外观类，包括准备、观看、暂停、结束等大流程
     * @Version: 1.0
     */
    public class HomeTheaterFacade {
        private Popcorn popcorn;
        private Screen screen;
        private Stereo stereo;
        private TheaterLights lights;
        private Projector projector;
        private DVDPlayer player;
 
        public HomeTheaterFacade() {
            this.popcorn = Popcorn.getInstance();
            this.screen = Screen.getInstance();
            this.stereo = Stereo.getInstance();
            this.lights = TheaterLights.getInstance();
            this.projector = Projector.getInstance();
            this.player = DVDPlayer.getInstance();
        }
 
        /**
         * 准备看电影：调用开灯、放屏幕、开投影仪、调暗灯等流程
         */
        public void ready() {
            // 打开灯光
            lights.on();
            // 开爆米花机
            popcorn.on();
            // 放下屏幕
            screen.down();
            // 开投影仪，聚焦、放大
            projector.on();
            projector.focus();
            projector.zoom();
            // 开音响，设置音量
            stereo.on();
            stereo.setVolume(8);
            // 开DVD，选dvd
            player.on();
            player.setDvd("坦塔尼克号");
            // 取爆米花，关闭爆米花机器
            popcorn.pop();
            popcorn.off();
            // 调暗灯光
            lights.dim();
        }
 
        /**
         * 看电影
         */
        public void play() {
            player.play();
        }
 
        /**
         * 暂停电影
         */
        public void pause() {
            player.pause();
        }
 
        /**
         * 关闭电影：调用关投影仪、关音响、开灯、收屏幕等流程
         */
        public void end() {
            player.off();
            projector.off();
            stereo.off();
            lights.bright();
            screen.up();
        }
    }
```

#### **4.2.3** 客户端：家庭影院的准备、观看、结束

客户端类创建外观类对象，调用各个组装方法。

```
/**
     * @Author: vince
     * @CreateTime: 2024/10/24
     * @Description: 客户端：调用影院外观类的准备、观看、结束等流程
     * @Version: 1.0
     */
    public class ClientTest {
        public static void main(String[] args) throws InterruptedException {
            HomeTheaterFacade homeTheaterFacade = new HomeTheaterFacade();
            System.out.println("===========家庭影院初始化============");
            homeTheaterFacade.ready();
            System.out.println("===========家庭影院沉浸式播放============");
            homeTheaterFacade.play();
            Thread.sleep(1000);
            System.out.println("===========家庭影院暂停============");
            homeTheaterFacade.pause();
            Thread.sleep(1000);
            System.out.println("===========家庭影院沉浸式播放============");
            homeTheaterFacade.play();
            Thread.sleep(1000);
            System.out.println("===========家庭影院结束============");
            homeTheaterFacade.end();
        }
    }
```

## 五、外观模式在三层架构中的应用

### 5.1 三层架构和 MVC 设计模式

#### **5.1.1 三层架构**

开发过程中，我们把后端服务器 Servlet 拆分成三层，分别是 web、service 和 dao，这也是程序员常提到的 **“Java 味”**：

*   **web 层（表现层）**：直接与用户交互，负责接收用户输入和呈现数据
*   **service 层（业务层）**：处理具体的业务逻辑。也称为服务层或应用层。
*   **dao 层（数据访问层）**：负责与数据库交互，执行数据的增删改查

![](<assets/1740031169942.png>)

**优点**：

*   **低耦合**：各层的职责明确，页面交互、业务逻辑、数据库操作三层分离，降低了系统模块间的耦合。并且各个类功能一目了然，例如 OrderController 可以直接看出它是控制器。
*   **易维护**：每层的功能独立，业务逻辑更改后只需修改 Service，数据库更改后只需修改 dao。
*   **可扩展**：当增加新功能后，可以在各层扩展相关功能的类。

#### **5.1.2 MVC 设计模式**

**MVC 设计模式**：将后端 Servlet 设计为控制器 controller、视图 view、业务模型 Model。

*   **视图（View）**：显示 UI 页面数据，并与用户交互。前后端分离项目中视图就是前端代码，一体化项目中视图是 JSP、Thymeleaf 等框架渲染成的 HTML。
*   **控制器（Controller）**：负责接收浏览器发送过来的请求，然后响应给浏览器
*   **模型（Model）**：封装后端业务逻辑和应用的核心数据，与数据层交互。

**流程**：

1.  控制器（例如 serlvlet）用来接收浏览器发送过来的请求
2.  控制器调用模型（例如 JavaBean）来获取数据，比如从数据库查询数据；
3.  控制器获取到数据后再交由视图（例如 JSP）进行数据展示。  

![](<assets/1740031170033.png>)

**优点**：

*   **低耦合**：将数据、UI 和控制逻辑分离，便于开发、维护和扩展。
*   **可扩展性**：将逻辑处理和视图渲染分开，各个小组件能直接复用。

### 5.2 外观模式在三层架构中的应用

外观模式像模板模式一样，都是很通用的设计模式，不只是 JDK 和常用框架的源码，我们开发过程中也会经常用到他们。

例如我们写 Service 时候，将 Dao 注入到 Service，然后调用 Dao 的各个方法，这其实也用到外观模式的思想。

下面订单 Service，注入了订单 dao、商品 dao 和客户 dao，然后对流程进行组装

```
/**
 * @Author: vince
 * @CreateTime: 2024/10/29
 * @Description: 订单业务实现
 * @Version: 1.0
 */
@Service
public class OrderServiceImpl implements OrderService{
 
    /**
     * 订单dao
     */
    @Autowired
    private OrderDao orderDao;
 
    /**
     * 商品dao
     */
    @Autowired
    private ProductDao productDao;
 
    /**
     * 客户dao
     */
    @Autowired
    private CustomerDao customerDao;
 
    public Order placeOrder(Customer customer, Product product) {
        // 外观模式思想：调用多个DAO方法，完成下单业务逻辑
        // 1.保存客户信息
        customerDao.save(customer);
        // 2.更新商品库存
        productDao.updateStock(product);
        Order order = new Order(customer, product);
        // 3.保存订单
        orderDao.save(order);
        return order;
    }
}
```

## 六、外观模式在 Tomcat 源码中的应用 

Tomcat 源码使用了外观模式，以 ApplicationContextFacade 为例

### 6.1 外观类：ApplicationContextFacade

ApplicationContextFacade 是 Apache Tomcat 框架中的外观类，位于 org.apache.catalina.core 包下。是 ApplicationContext 的代理接口，主要用于屏蔽 ApplicationContext 的复杂内部实现。

其中，initClassCache() 方法用于初始化 classCache 变量：

```
package org.apache.catalina.core;
/**
 * 外观类：用于屏蔽 ApplicationContext 的复杂内部实现。
 *
 * @author Remy Maucherat
 */
public class ApplicationContextFacade implements ServletContext {
 
    // ---------------------------------------------------------- Attributes
    /**
     * 缓存类对象：用于后续通过反射获取类的信息
     */
    private final Map<String, Class<?>[]> classCache;
 
 
    /**
     * 缓存方法对象
     */
    private final Map<String, Method> objectCache;
 
 
    // ----------------------------------------------------------- Constructors
 
    /**
     * 构造方法：构造一个新对象实例，并初始化类对象缓存
     * @param context The associated Context instance
     */
    public ApplicationContextFacade(ApplicationContext context) {
        super();
        this.context = context;
 
        classCache = new HashMap<>();
        objectCache = new ConcurrentHashMap<>();
        initClassCache();
    }
 
 
    /**
     * 初始化类缓存：将ApplicationContext各方法加入类缓存中，value统一初始化成String类对象
     */
    private void initClassCache() {
        // 1.创建String的类对象
        Class<?>[] clazz = new Class[] { String.class };
        // 2.将该类对象作为value，key是ApplicationContext各方法，统一存到类缓存中
        // 获取上下文
        classCache.put("getContext", clazz);
        // 获取 MIME 类型
        classCache.put("getMimeType", clazz);
        // 获取资源路径
        classCache.put("getResourcePaths", clazz);
        // 获取资源
        classCache.put("getResource", clazz);
        // 获取资源的输入流
        classCache.put("getResourceAsStream", clazz);
        // 获取请求分发器
        classCache.put("getRequestDispatcher", clazz);
        // 获取指定名称的分发器
        classCache.put("getNamedDispatcher", clazz);
        // 获取 Servlet
        classCache.put("getServlet", clazz);
 
        // 设置初始化参数
        classCache.put("setInitParameter", new Class[] { String.class, String.class });
        // 创建 Servlet 实例
        classCache.put("createServlet", new Class[] { Class.class });
        // 添加 Servlet
        classCache.put("addServlet", new Class[] { String.class, String.class });
 
        // 创建过滤器实例
        classCache.put("createFilter", new Class[] { Class.class });
        // 添加过滤器
        classCache.put("addFilter", new Class[] { String.class, String.class });
        // 创建监听器
        classCache.put("createListener", new Class[] { Class.class });
        // 添加监听器
        classCache.put("addListener", clazz);
        // 获取过滤器注册信息
        classCache.put("getFilterRegistration", clazz);
        // 获取 Servlet 注册信息
        classCache.put("getServletRegistration", clazz);
        // 获取初始化参数
        classCache.put("getInitParameter", clazz);
        // 设置属性
        classCache.put("setAttribute", new Class[] { String.class, Object.class });
        // 移除属性
        classCache.put("removeAttribute", clazz);
        // 获取资源的真实路径
        classCache.put("getRealPath", clazz);
        // 获取属性
        classCache.put("getAttribute", clazz);
        // 记录日志
        classCache.put("log", clazz);
        // 设置会话跟踪模式
        classCache.put("setSessionTrackingModes", new Class[] { Set.class });
        // 添加 JSP 文件
        classCache.put("addJspFile", new Class[] { String.class, String.class });
        // 声明角色
        classCache.put("declareRoles", new Class[] { String[].class });
        // 设置会话超时时间
        classCache.put("setSessionTimeout", new Class[] { int.class });
        // 设置请求字符编码
        classCache.put("setRequestCharacterEncoding", new Class[] { String.class });
        // 设置响应字符编码
        classCache.put("setResponseCharacterEncoding", new Class[] { String.class });
    }
    // ...
}
```

### 6.2 子系统：Map

 子系统是 Map 类，外观类创建了 Map 对象 classCache 作为变量，存储各个类的类对象，之后通过反射获取这些类的信息

```
/**
 * 外观类：用于屏蔽 ApplicationContext 的复杂内部实现。
 *
 * @author Remy Maucherat
 */
public class ApplicationContextFacade implements ServletContext {
 
    // ---------------------------------------------------------- Attributes
    /**
     * 子系统：类缓存对象：用于后续通过反射获取类的信息
     */
    private final Map<String, Class<?>[]> classCache;
 
    // ...
}
```

### 6.3 客户端：ApplicationContext

 ApplicationContext 是 Apache Tomcat 中的一个核心类，主要功能是表示一个 Web 应用程序的执行环境。它实现了 ServletContext 接口，负责获取响应字符编码、添加过滤器等 Servlet 基础功能。

```
package org.apache.catalina.core;
/**
 * 应用上下文类：ServletContext的标准实现，用于获取响应字符编码、添加过滤器等Servlet基础功能
 *
 * @author Craig R. McClanahan
 * @author Remy Maucherat
 */
public class ApplicationContext implements ServletContext {
 
    /**
     * The facade around this object.
     */
    private final ServletContext facade = new org.apache.catalina.core.ApplicationContextFacade(this);
 
    /**
     * 获取上下文
     * @return {@link StandardContext }
     */
    protected StandardContext getContext() {
        return this.context;
    }
    /**
     * 获取文件类型
     *
     * @param file 文件名
     */
    @Override
    public String getMimeType(String file) {
 
        if (file == null) {
            return null;
        }
        int period = file.lastIndexOf('.');
        if (period < 0) {
            return null;
        }
        String extension = file.substring(period + 1);
        if (extension.length() < 1) {
            return null;
        }
        return context.findMimeMapping(extension);
 
    }
 
    // ...其他方法
}
```

### 6.4 Tomcat 和 Spring 的 ApplicationContext 区别

**相同点：** 两者都是应用程序上下文，用于管理和获取容器的环境信息。

**不同点：**

*   **Tomcat 的 ApplicationContext**：Web 容器相关的应用上下文，用于获取和管理 Web 容器的环境信息；
*   **Spring 的 ApplicationContext**：Spring 的 IOC 容器相关的应用上下文，用于获取和管理 IOC 容器的环境信息；

**IOC 容器**：

*   **IOC 控制反转思想：**创建对象的控制权由内部（即 new 实例化）反转到外部（即 IOC 容器）。
*   **Bean：**IOC 容器中存放的一个个对象
*   **DI 依赖注入：**绑定 IOC 容器中 bean 与 bean 之间的依赖关系。例如将 dao 层对象注入到 service 层对象。

**使用示例：**

```
@SpringBootTest
class DemoApplicationTests {
    @Autowired
    private ApplicationContext applicationContext;
    /**
     * web容器：ApplicationContext是ServletContext接口的实现类
     */
    @Autowired
    private ServletContext servletContext;
 
    /**
     * 获取所有Bean
     */
    @Test
    void contextLoads() {
        String[] beanDefinitionNames = applicationContext.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            System.out.println(beanDefinitionName);
        }
    }
 
    /**
     * 获取所有web容器的属性
     */
    @Test
    void servletTest() {
        // 获取所有web容器的属性
        Enumeration<String> attributeNames = servletContext.getAttributeNames();
        while (attributeNames.hasMoreElements()) {
            String name = attributeNames.nextElement();
            System.out.println(name);
        }
    }
 
}
```

![](<assets/1740031170121.png>)

![](<assets/1740031170218.png>)