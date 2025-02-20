---
url: https://blog.csdn.net/qq_40991313/article/details/126137805
title: JavaWeb 基础 7——会话技术 Cookie&Session_sendredirect cookie-CSDN 博客
date: 2025-02-20 09:37:44
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[一、会话技术](#t0)

[1.1 会话和跟踪技术介绍](#t1)

[1.2 Cookie](#t2)

[1.2.1 简介](#t3)

[1.2.2 Cookie 的发送和获取](#t4)

[1.2.3 Cookie 的原理](#t5)

[1.2.4 Cookie 的存活时间](#t6)

[1.2.5 Cookie 存储中文](#t7)

[1.3 Session](#t8)

[1.3.1 简介](#t9)

[1.3.2 Session 的基本使用](#t10)

[1.3.3 Session 的原理分析](#t11)

[1.3.4 钝化与活化、销毁](#t12)

[1.4 总结，Cookie 和 Session 区别和应用场景](#t13)

[1.5 案例，用户登录注册 “记住我” 和“验证码”](#t14)

[1.5.1 需求分析](#t15)

[1.5.2 用户登录功能](#t16)

[1.5.3 记住我 - 设置 Cookie](#t17)

[1.5.4 记住我 - 获取 Cookie](#t18)

[1.5.5 用户注册功能](#t19)

[1.5.6 验证码 - 展示](#t20)

[1.5.7 验证码 - 校验](#t21)

## 一、会话技术

### 1.1 会话和跟踪技术介绍

**会话:** 会话指的是客户端与服务器交互的一个**时间段**。

用户打开浏览器，访问 web 服务器的资源，会话建立，直到有一方断开连接，会话结束。在一次会话中可以包含**多次**请求和响应。

用实际场景来理解下会话，比如在我们访问京东的时候，当打开浏览器进入京东首页后，浏览器和京东的服务器之间就建立了一次会话，后面的搜索商品, 查看商品的详情, 加入购物车等都是在这一次会话中完成。

**思考:** 下图中总共建立了几个会话?

![](<assets/1740015464614.png>)

每个浏览器都会与服务端建立了一个会话，加起来总共是 **3** 个会话。

**会话跟踪:** 一种维护浏览器状态的方法，服务器需要**识别**多次请求是否来自于**同一浏览器**，以便在同一次会话的多次请求间**共享数据**。

**会话跟踪技术的应用**

*   服务器会收到多个请求，这多个请求可能来自多个浏览器，如上图中的 6 个请求来自 3 个浏览器
    
*   服务器需要用来识别请求是否来自同一个浏览器
    
*   服务器用来识别浏览器的过程，这个过程就是**会话跟踪**
    
*   服务器识别浏览器后就可以在同一个会话中多次请求之间来共享数据
    
*   **购物车:** `加入购物车`和`去购物车结算`是两次请求，但是后面这次请求要想展示前一次请求所添加的商品，就需要用到数据共享。
    
    ![](<assets/1740015464806.png>)
    
*   **页面展示用户登录信息:** 很多网站，登录后访问多个功能发送多次请求后，浏览器上都会有当前登录用户的信息 [用户名]。
    
    ![](<assets/1740015465113.png>)
    
*   网站登录页面的**`记住我`**功能: 当用户登录成功后，勾选`记住我`按钮后下次再登录的时候，网站就会自动填充用户名和密码，简化用户的登录操作。多次登录就会有多次请求，他们之间也涉及到共享数据
    
    ![](<assets/1740015465372.png>)
    
*   登录页面的**验证码功能:** 生成验证码和输入验证码点击注册这也是**两次请求**，这两次请求的数据之间要进行**对比**，相同则允许注册，不同则拒绝注册，该功能的实现也需要在同一次会话中共享数据。
    
    ![](<assets/1740015465594.png>)
    

**思考: 为什么现在浏览器和服务器不支持数据共享呢?**

*   浏览器和服务器之间使用的是 HTTP 请求来进行数据传输
    
*   HTTP 协议是**无状态**的，每次浏览器向服务器请求时，服务器都会将该请求视为**新的**请求
    
*   HTTP 协议设计成无状态的目的是让**每次请求之间相互独立，互不影响**
    
*   请求与请求之间独立后，就无法实现多次请求之间的数据共享
    

**会话跟踪技术的实现方式:**

(1) 客户端会话跟踪技术：**Cookie**

(2) 服务端会话跟踪技术：**Session**

它们之间最大的**区别: Cookie 是存储在浏览器端而 Session 是存储在服务器端。**

Session 译为会议，会话。

**小结**

*   HTTP 协议是无状态的，靠 HTTP 协议是无法实现会话跟踪
    
*   想要实现会话跟踪，就需要用到 Cookie 和 Session
    

### 1.2 Cookie

#### 1.2.1 简介

**Cookie**：

客户端会话技术。将数据保存到客户端，以后**每次请求都携带 Cookie 数据**进行访问。

**Cookie 的工作流程**

![](<assets/1740015465819.png>)

*   服务端提供了两个 Servlet，分别是 ServletA 和 ServletB
    
*   浏览器发送 HTTP 请求 1 给服务端，服务端 ServletA 接收请求并进行业务处理
    
*   服务端 ServletA 在处理的过程中可以**创建一个 Cookie 对象**并将`name=zs`的数据存入 Cookie
    
*   服务端 ServletA 在响应数据的时候，会把 Cookie 对象**响应**给浏览器
    
*   浏览器接收到响应数据，会把 Cookie 对象中的数据**存储在浏览器内存**中，此时浏览器和服务端就**建立了一次会话**
    
*   **在同一次会话**中浏览器**再次发送** HTTP 请求 2 给服务端 ServletB，浏览器会**携带 Cookie 对象**中的所有数据
    
*   ServletB 接收到请求和数据后，就可以获取到存储在 Cookie 对象中的数据，这样同一个会话中的多次请求之间就实现了数据共享
    

#### 1.2.2 Cookie 的发送和获取

![](<assets/1740015466043.png>)

**发送 Cookie**

*   创建 Cookie 对象，并设置数据
    

```
Cookie cookie = new Cookie("key","value");

```

*   发送 Cookie 到客户端（浏览器）：使用 **response** 对象
    

```
response.addCookie(cookie);

```

**案例完成 Cookie 的发送：**

**需求:** 在 Servlet 中生成 Cookie 对象并存入数据，然后将数据发送给浏览器

1. 创建 Maven 项目, 项目名称为 cookie-demo，并在 pom.xml 添加依赖

2. 编写 Servlet 类，名称为 AServlet

3. 在 AServlet 中创建 Cookie 对象，存入数据，发送给前端

4. 启动测试，在浏览器查看 Cookie 对象中的值

(1) 创建 Maven 项目 cookie-demo，并在 pom.xml 添加依赖 Servlet、jsp、jstl

```
<properties>
    <maven.compiler.source>8</maven.compiler.source>
    <maven.compiler.target>8</maven.compiler.target>
</properties>
 
<dependencies>
    <!--servlet-->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>
    <!--jsp-->
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.2</version>
        <scope>provided</scope>
    </dependency>
    <!--jstl-->
    <dependency>
        <groupId>jstl</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency>
    <dependency>
        <groupId>taglibs</groupId>
        <artifactId>standard</artifactId>
        <version>1.1.2</version>
    </dependency>
</dependencies>
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.2</version>
        </plugin>
    </plugins>
</build>
```

(2) 在 Servlet 中创建 Cookie 对象，存入数据，发送给前端

```
@WebServlet("/aServlet")
public class AServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //发送Cookie
        //1. 创建Cookie对象
        Cookie cookie = new Cookie("username","zs");
        //2. 发送Cookie，response
        response.addCookie(cookie);
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

（3）启动测试，在浏览器查看 Cookie 对象中的值

访问

```
http://localhost:8080/cookie-demo/aServlet

```

**chrome 浏览器查看 Cookie 的值**，有两种方式:

**方式一:**

![](<assets/1740015466356.png>)

**方式二:** 选中打开开发者工具或者 使用快捷键 F12 或者 Ctrl+Shift+I

![](<assets/1740015466816.png>)

**获取 Cookie**

*   **获取客户端携带的所有 Cookie**，使用 **request** 对象
    

```
Cookie[] cookies = request.getCookies();

```

*   遍历数组，获取每一个 Cookie 对象：for
    
*   使用 Cookie 对象方法**获取数据**
    

```
cookie.getName();
cookie.getValue();
```

**案例完成 Cookie 的获取:**

需求: 在 Servlet 中获取前一个案例存入在 Cookie 对象中的数据

1. 编写一个新 Servlet 类，名称为 BServlet

2. 在 BServlet 中使用 request 对象获取 Cookie 数组，遍历数组，从数据中获取指定名称对应的值

3. 启动测试，在控制台打印出获取的值

（1）在 BServlet 中使用 request 对象获取 Cookie 数组，遍历数组，从数据中获取指定名称对应的值

```
@WebServlet("/bServlet")
public class BServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取Cookie
        //1. 获取Cookie数组
        Cookie[] cookies = request.getCookies();
        //2. 遍历数组
        for (Cookie cookie : cookies) {
            //3. 获取数据
            String name = cookie.getName();
            if("username".equals(name)){
                String value = cookie.getValue();
                System.out.println(name+":"+value);
                break;
            }
        }
 
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

（2）启动测试，在控制台打印出获取的值

访问`http://localhost:8080/cookie-demo/bServlet`

![](<assets/1740015467191.png>)

在 IDEA 控制台就能看到输出的结果:

![](<assets/1740015468505.png>)

**小结**

*   发送 Cookie:
    
    *   创建 Cookie 对象，并设置值: Cookie cookie = new Cookie("key","value");
        
    *   发送 Cookie 到客户端使用的是 Reponse 对象: response.addCookie(cookie);
        
*   获取 Cookie:
    
    *   使用 Request 对象获取 Cookie 数组: Cookie[] cookies = request.getCookies();
        
    *   遍历数组
        
    *   获取数组中每个 Cookie 对象的值: cookie.getName() 和 cookie.getValue()
        

介绍完 Cookie 的基本使用之后，那么 Cookie 的底层到底是如何实现一次会话两次请求之间的数据共享呢?

#### 1.2.3 Cookie 的原理

对于 Cookie 的实现原理是**基于 HTTP 协议**的:

*   **响应头: set-cookie**
    
*   **请求头: cookie**
    

![](<assets/1740015468741.png>)

**过程：** 

*   前面的案例中已经能够实现，AServlet 给前端发送 Cookie,BServlet 从 request 中获取 Cookie 的功能
    
*   对于 AServlet 响应数据的时候，Tomcat 服务器都是**基于 HTTP 协议来响应数据**
    
*   当 Tomcat 发现后端要返回的是一个 Cookie 对象之后，Tomcat 就会在**响应头**中添加一行数据**`Set-Cookie:username=zs`**
    
*   浏览器获取到响应结果后，从响应头中就可以获取到`Set-Cookie`对应值`username=zs`, 并将数据**存储在浏览器的内存**中
    
*   浏览器再次发送请求给 BServlet 的时候，**浏览器会自动在请求头中添加`**Cookie**: username=zs`**发送给服务端 BServlet
    
*   Request 对象会把请求头中 cookie 对应的值封装成一个个 Cookie 对象，最终形成一个数组
    
*   BServlet 通过 Request 对象获取到 Cookie[] 后，就可以从中获取自己需要的数据
    

**案例验证:**

(1) 访问 AServlet 对应的地址`http://localhost:8080/cookie-demo/aServlet`

使用 Chrom 浏览器打开开发者工具 (F12 或 Crtl+Shift+I) 进行查看**响应头**中的数据

![](<assets/1740015469056.png>)

（2）访问 BServlet 对应的地址 `[http://localhost:8080/cookie-demo/bServlet](http://localhost:8080/cookie-demo/bServlet "http://localhost:8080/cookie-demo/bServlet")

使用 Chrom 浏览器打开开发者工具 (F12 或 Crtl+Shift+I) 进行查看**请求头**中的数据

![](<assets/1740015469348.png>)

#### 1.2.4 Cookie 的存活时间

cookie 响应发送和请求获取过程:

![](<assets/1740015469653.png>)

(1) 浏览器发送请求给 AServlet,AServlet 会响应一个存有`usernanme=zs`的 Cookie 对象给浏览器

(2) 浏览器接收到响应数据将 cookie 存入到浏览器内存中

(3) 当浏览器再次发送请求给 BServlet,BServlet 就可以使用 Request 对象获取到 Cookie 数据

**思考：**在发送请求到 BServlet 之前，如果把**浏览器关闭再打开**进行访问，BServlet 能否获取到 Cookie 数据?

注意：浏览器关闭再打开不是指打开一个新的选显卡，而且必须是先关闭再打开，顺序不能变。

答案：不能。BServlet 中无法再获取到 Cookie 数据，浏览器关闭默认销毁 cookie

*   **默认情况下，Cookie 存储在浏览器内存中，当浏览器关闭，内存释放，则 Cookie 被销毁**
    

此时，网站登录在重启浏览器后 “记着我” 功能就无法实现了。

**如何将 Cookie 持久化存储?**

*   **设置 Cookie 存活时间**
    

```
setMaxAge(int seconds)

```

参数值为:

1. 正数：将 Cookie 写入浏览器所在电脑的硬盘，持久化存储。到时间自动删除

2. **负数：默认值**，Cookie 在当前浏览器内存中，当浏览器关闭，则 Cookie 被销毁

3. 零：删除对应 Cookie

**设置 cookie 周期案例：**

```
@WebServlet("/aServlet")
public class AServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //发送Cookie
        //1. 创建Cookie对象
        Cookie cookie = new Cookie("username","zs");
        //设置存活时间   ，1周 7天
        cookie.setMaxAge(60*60*24*7); //易阅读，需程序计算
		//cookie.setMaxAge(604800); //不易阅读(可以使用注解弥补)，程序少进行一次计算
        //2. 发送Cookie，response
        response.addCookie(cookie);
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

修改完代码后，启动测试，访问`http://localhost:8080/cookie-demo/aServlet`

*   访问一个 AServlet 后，把浏览器关闭重启后，再去访问`http://localhost:8080/cookie-demo/bServet`, 能在控制台打印出`username:zs`, 说明 Cookie 没有随着浏览器关闭而被销毁
    
*   通过浏览器查看 Cookie 的内容，会发现 Cookie 的相关信息
    
    ![](<assets/1740015469985.png>)
    

#### 1.2.5 Cookie 存储中文

**Cookie 不能直接存储中文，并且会报错，要进行 URL 编码间接存储中文。** 

测试是否直接支持中文：

```
@WebServlet("/aServlet")
public class AServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
 		//发送Cookie
        String value = "张三";
        Cookie cookie = new Cookie("username",value);
        //设置存活时间   ，1周 7天
        cookie.setMaxAge(60*60*24*7);
        //2. 发送Cookie，response
        response.addCookie(cookie);
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

启动访问测试，访问`http://localhost:8080/cookie-demo/aServlet`会发现浏览器会提示错误信息

![](<assets/1740015470316.png>)

*   **结论: Cookie 不能直接存储中文，并且会报错**
    

**cookie 间接存储中文方法：进行** URL 编码解码

1. 在 AServlet 中对中文进行 URL 编码，采用 URLEncoder.encode()，将编码后的值存入 Cookie 中

2. 在 BServlet 中获取 Cookie 中的值, 获取的值为 URL 编码后的值

3. 将获取的值在进行 URL 解码, 采用 URLDecoder.decode()，就可以获取到对应的中文值

(1) 在 AServlet 中对中文进行 URL 编码

```
@WebServlet("/aServlet")
public class AServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //发送Cookie
        Cookie cookie = new Cookie("username", URLEncoder.encode("张三","utf-8"));
        //设置存活时间   ，1周 7天
        cookie.setMaxAge(60*60*24*7);
        //2. 发送Cookie，response
        response.addCookie(cookie);
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

(2) 在 BServlet 中获取值，并对值进行解码

```
@WebServlet("/bServlet")
public class BServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取Cookie
        //1. 获取Cookie数组
        Cookie[] cookies = request.getCookies();
        //2. 遍历数组
        for (Cookie cookie : cookies) {
            //3. 获取数据
            String name = cookie.getName();
            if("username".equals(name)){
                String value = cookie.getValue();//获取的是URL编码后的值 %E5%BC%A0%E4%B8%89
                //URL解码
                value = URLDecoder.decode(value,"UTF-8");
                System.out.println(name+":"+value);//value解码后为 张三
                break;
            }
        }
 
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

至此，我们就可以将中文存入 Cookie 中进行使用。

**小结**

Cookie 的使用细节中，我们讲了 Cookie 的`存活时间`和`存储中文`:

*   存活时间，需要掌握 setMaxAage()API 的使用
    
*   存储中文，需要掌握 URL 编码和解码的使用
    

### 1.3 Session

#### **1.3.1 简介**

**Session**：服务端会话跟踪技术：将数据保存到服务端。

*   Session 是存储在服务端而 Cookie 是存储在客户端
    
*   存储在客户端的数据容易被窃取和截获，存在很多不安全的因素
    
*   存储在服务端的数据相比于客户端来说就**更安全**
    

**特点：**

*   Session 的 setAttribute 支持中文，不存在乱码、报错等问题。 
*   Session 基于 cookie 实现的，服务端把 session 唯一标识 id 当成 cookie 响应给浏览器，再次请求时直接通过浏览器 cookie 里的 id 找到服务器内存中存储的 session 对象。
*   同一会话多次请求获取到的 session 对象是同一个。
*   关闭浏览器后 Session 销毁。

**Session 的工作流程**

![](<assets/1740015470611.png>)

*   在服务端的 AServlet 获取一个 Session 对象，把数据存入其中
    
*   在服务端的 BServlet 获取到相同的 Session 对象，从中取出数据
    
*   就可以实现一次会话中多次请求之间的数据共享了
    

#### **1.3.2** Session 的基本使用

在 JavaEE 中提供了 **HttpSession 接口**，来实现一次会话的多次请求之间数据共享功能。

具体的使用步骤为:

*   **获取 Session 对象**, 使用的是 request 对象
    

```
HttpSession session = request.getSession();

```

*   Session 对象提供的**功能**，类似于 request 域对象方法:
    
    *   **存储数据到 session 域中**
        
        ```
        void setAttribute(String name, Object o)
        
        ```
        
    *   **根据 key，获取值**
        
        ```
        Object getAttribute(String name)
        
        ```
        
    *   **根据 key，删除该键值对**
        
        ```
        void removeAttribute(String name)
        
        ```
        

**案例：**

介绍完 Session 相关的 API 后，接下来通过一个案例来完成对 Session 的使用，具体实现步骤为:

需求: 在一个 Servlet 中往 Session 中存入数据，在另一个 Servlet 中获取 Session 中存入的数据

1. 创建名为 SessionDemo1 的 Servlet 类

2. 创建名为 SessionDemo2 的 Servlet 类

3. 在 SessionDemo1 的方法中: 获取 Session 对象、存储数据

4. 在 SessionDemo2 的方法中: 获取 Session 对象、获取数据

5. 启动测试

(1)SessionDemo1: 获取 Session 对象、存储数据

```
@WebServlet("/demo1")
public class SessionDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    	//存储到Session中
        //1. 获取Session对象
        HttpSession session = request.getSession();
        //2. 存储数据
        session.setAttribute("username","zs");
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

(2)SessionDemo2: 获取 Session 对象、获取数据

```
@WebServlet("/demo2")
public class SessionDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取数据，从session中
        //1. 获取Session对象
        HttpSession session = request.getSession();
        //2. 获取数据
        Object username = session.getAttribute("username");
        System.out.println(username);
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

(3) 启动测试，

*   先访问`http://localhost:8080/cookie-demo/demo1`, 将数据存入 Session
    
*   在访问`http://localhost:8080/cookie-demo/demo2`, 从 Session 中获取数据
    
*   查看控制台
    
    ![](<assets/1740015470893.png>)
    

通过案例的效果，能看到 Session 是能够在一次会话中两次请求之间共享数据。

**小结**

*   **Session 的获取**
    
    ```
    HttpSession session = request.getSession();
    
    ```
    
*   **Session 常用方法的使用**
    
    ```
    void setAttribute(String name, Object o)
    Object getAttribute(String name)
    ```
    
    **注意:**Session 中可以存储的是一个 **Object 类型**的数据，也就是说 Session 中可以存储任意数据类型。
    

#### **1.3.3** Session 的原理分析

*   **Session 是基于 Cookie 实现的**
    
*   **一个会话里多次请求获取的 Session 对象是同一个。不同浏览器或者重新打开浏览器后获取的 Session 对象不是同一个。**

**验证：**

![](<assets/1740015471154.png>)

Session 要想实现一次会话多次请求之间的数据共享，就必须要保证多次请求获取 Session 的对象是同一个。

SessionDemo1

```
@WebServlet("/demo1")
public class SessionDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    	//存储到Session中
        //1. 获取Session对象
        HttpSession session = request.getSession();
        System.out.println(session);
        //2. 存储数据
        session.setAttribute("username","zs");
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

SessionDemo2

```
@WebServlet("/demo2")
public class SessionDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取数据，从session中
        //1. 获取Session对象
        HttpSession session = request.getSession();
        System.out.println(session);
        //2. 获取数据
        Object username = session.getAttribute("username");
        System.out.println(username);
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

启动测试，分别访问

`http://localhost:8080/cookie-demo/demo1`

`http://localhost:8080/cookie-demo/demo2`

![](<assets/1740015471382.png>)

**结论:**

*   两个 Servlet 类中获取的 Session 对象是**同一个**
    
*   把 demo1 和 demo2 请求刷新多次，控制台最终打印的结果都是同一个
    

注意：**如果是不同浏览器或者重新打开浏览器后，打印的 Session 就不一样了。**

Session 实现的也是一次会话中的多次请求之间的数据共享。

**Session 是如何保证在一次会话中获取的 Session 对象是同一个呢?**

![](<assets/1740015471640.png>)

(1)demo1 在第一次获取 session 对象的时候，session 对象会有一个**唯一的标识**，假如是`id:10`

(2)demo1 在 session 中存入其他数据并处理完成所有业务后，需要通过 Tomcat 服务器响应结果给浏览器

(3)Tomcat 服务器发现业务处理中使用了 session 对象，就会**把 session 的唯一标识`id:10`当做一个 cookie**，添加**`Set-Cookie:JESSIONID=10`**到**响应头**中，并响应给浏览器

(4) 浏览器接收到响应结果后，会把响应头中的 coookie 数据存储到浏览器的内存中

(5) 浏览器在同一会话中访问 demo2 的时候，会把 cookie 中的数据按照**`cookie: JESSIONID=10`**的格式添加到**请求头**中并发送给服务器 Tomcat

(6)demo2 获取到请求后，从请求头中就读取 cookie 中的 JSESSIONID 值为 10，然后就会到**服务器内存**中**寻找**`id:10`的 **session 对象**，如果找到了，就直接返回该对象，如果没有则新创建一个 session 对象

(7) 关闭打开浏览器后，因为浏览器的 cookie 已被销毁，所以就没有 JESSIONID 的数据，服务端获取到的 session 就是一个全新的 session 对象

**通过实例来演示下:**

(1) 使用 chrome 浏览器访问`http://localhost:8080/cookie-demo/demo1`, 打开开发者模式 (F12 或 Ctrl+Shift+I), 查看**响应头 (Response Headers)** 数据:

![](<assets/1740015471974.png>)

(2) 使用 chrome 浏览器再次访问`http://localhost:8080/cookie-demo/demo2`，查看**请求头 (Request Headers)** 数据:

![](<assets/1740015472251.png>)

**小结**

*   Session 是基于 Cookie 来实现的
    

#### **1.3.4** 钝化与活化、销毁

![](<assets/1740015472581.png>)

**注意：**

*   这里说的关闭服务器是正常关闭，不是强制关闭
*   在服务器正常关闭后，session 写入服务器的硬盘里，不是客户端的硬盘里。

*   session 数据存储在服务端，服务器重启后，session 数据会被保存在服务器硬盘
    
*   浏览器被关闭启动后，重新建立的连接就已经是一个全新的会话，获取的 session 数据也是一个新的对象
    
*   session 的数据要想共享，浏览器不能关闭，所以 session 数据不能长期保存数据
    
*   cookie 是存储在客户端，是可以长期保存
    

**Session 钝化与活化**

*   思考：服务器重启后，Session 中的数据是否还在?
    

要想回答这个问题，我们可以先看下下面这幅图，

![](<assets/1740015472787.png>)

(1) 服务器端 AServlet 和 BServlet 共用的 session 对象应该是存储在服务器的内存中

(2) 服务器重新启动后，内存中的数据应该是已经被释放，对象也应该都销毁了

所以 session 数据应该也已经不存在了。但是**如果 session 不存在会引发什么问题呢?**

**举个例子说明下：**

(1) 用户把需要购买的商品添加到购物车，因为要实现同一个会话多次请求数据共享，所以假设把数据存入 Session 对象中

(2) 用户正要付钱的时候接到一个电话，付钱的动作就搁浅了

(3) 正在用户打电话的时候，购物网站因为某些原因需要重启

(4) 重启后 session 数据被销毁，购物车中的商品信息也就会随之而消失

(5) 用户想再次发起支付，就会出现问题

所以说对于 session 的数据，我们应该做到就算服务器重启了，也应该能把数据保存下来才对。

**案例：**

**注意:** 这里所说的关闭和启动应该要确保是正常的关闭和启动。

**正常关闭 Tomcat 服务器的方法**

需要使用命令行的方式来启动和停止 Tomcat 服务器:

**启动**: 进入到项目 pom.xml 所在目录，执行`tomcat7:run`

![](<assets/1740015473015.png>)

**停止**: 在启动的命令行界面，输入`ctrl+c`

![](<assets/1740015473384.png>)

接下来的测试流程是:

(1) 先启动 Tomcat 服务器

(2) 访问`http://localhost:8080/cookie-demo/demo1`将数据存入 session 中

(3) 正确停止 Tomcat 服务器

(4) 再次重新启动 Tomcat 服务器

(5) 访问`http://localhost:8080/cookie-demo/demo2` 查看是否能获取到 session 中的数据

![](<assets/1740015473724.png>)

经过测试，会发现**只要服务器是正常关闭和启动，session 中的数据是可以被保存下来的。**

那么 Tomcat 服务器到底是如何做到的呢?

具体的原因就是:

**Session 的钝化和活化**

*   **钝化：**在服务器**正常关闭**后，Tomcat 会自动将 Session 数据写入**硬盘的文件**中
    
    *   钝化的数据路径为:`项目目录\target\tomcat\work\Tomcat\localhost\项目名称\SESSIONS.ser`
        
        ![](<assets/1740015473970.png>)
        
*   **活化：**再次启动服务器后，从**文件**中加载数据到 Session 中
    
    *   数据加载到 Session 中后，路径中的`SESSIONS.ser`文件会被删除掉
        

对于上述的整个过程，大家只需要**了解下即可**。因为所有的过程都是 Tomcat 自己完成的，不需要我们参与。

**小结**

*   session 数据存储在服务端，服务器重启后，session 数据会被保存
    
*   浏览器被关闭启动后，重新建立的连接就已经是一个全新的会话，获取的 session 数据也是一个新的对象
    
*   session 的数据要想共享，浏览器不能关闭，所以 session 数据不能长期保存数据
    
*   cookie 是存储在客户端，是可以长期保存
    

**Session 销毁**

session 的销毁会有两种方式:

*   **默认情况下，无操作，30 分钟自动销毁**
    
    *   对于这个失效时间，是可以通过**配置进行修改**的
        
        *   在项目的 web.xml 中配置
            
            ```
            <?xml version="1.0" encoding="UTF-8"?>
            <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                     xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
                     version="3.1">
             
                <session-config>
                    <session-timeout>100</session-timeout>
                </session-config>
            </web-app>
            ```
            
        *   如果没有配置，默认是 30 分钟，默认值是在 Tomcat 的 web.xml 配置文件中写死的
            
            ![](<assets/1740015474283.png>)
            
*   调用 Session 对象的 **invalidate()** 进行**手动销毁**
    
    *   在 SessionDemo2 类中添加 session 销毁的方法
        
        ```
        @WebServlet("/demo2")
        public class SessionDemo2 extends HttpServlet {
            @Override
            protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
                //获取数据，从session中
         
                //1. 获取Session对象
                HttpSession session = request.getSession();
                System.out.println(session);
         
                // 销毁
                session.invalidate();
                //2. 获取数据
                Object username = session.getAttribute("username");
                System.out.println(username);
            }
         
            @Override
            protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
                this.doGet(request, response);
            }
        }
        ```
        
    *   启动访问测试，先访问 demo1 将数据存入到 session，再次访问 demo2 从 session 中获取数据
        
        ![](<assets/1740015474544.png>)
        
    *   该销毁方法一般会在**用户手动退出**的时候，需要将 session 销毁掉。
        

### **1.4 总结，Cookie 和 Session 区别和应用场景**

**Cookie 和 Session 小结**

*   Cookie 和 Session 都是来完成一次会话内多次请求间**数据共享**的。
    
*   **区别:**
    
    *   **存储位置：**Cookie 是将数据存储在客户端，Session 将数据存储在服务端。关闭浏览器，cookie 看 setMaxAge，session 销毁。关闭服务器后两者都依然存在没销毁。
        
    *   **安全性：**Cookie 不安全，Session 安全
        
    *   **数据大小：**Cookie 最大 3KB，Session 无大小限制
        
    *   **存储时间：**Cookie 可以通过 setMaxAge() 长期存储，Session 默认 30 分钟
        
    *   **服务器性能：**Cookie 不占服务器资源，Session 占用服务器资源
        
*   **应用场景:**
    
    *   **购物车:** 使用 **Cookie** 来存储，因为要长期存储，且数据不敏感。
        
    *   **已登录用户的个人信息展示:** 使用 **Session** 来存储，因为数据敏感
        
    *   **记住我功能:** 使用 **Cookie** 来存储，因为要长期存储，牺牲安全提升体验
        
    *   **验证码:** 使用 **Session** 来存储，因为安全重要。发验证码请求和提交验证码请求的验证码只需要短期存储、且安全。
        
*   **结论**
    
    *   **Cookie** 是用来保证用户在**未登录情况下**的身份识别
        
    *   **Session** 是用来保存用户**登录后**的数据
        

### **1.5 案例，**用户登录注册 “记住我” 和“验证码”

#### 1.5.1 需求分析

1. 完成用户登录功能，如果用户勾选 “记住用户” ，则下次访问登录页面**自动**填充用户名密码

2. 完成注册功能，并实现**验证码**功能

![](<assets/1740015474794.png>)

#### 1.5.2 用户登录功能

![](<assets/1740015475022.png>)

用户登录成功后，跳转到列表页面，并在页面上展示当前登录的用户名称

用户登录失败后，跳转回登录页面，并在页面上展示对应的错误信息

**实现流程分析**

![](<assets/1740015475309.png>)

(1) 前端通过表单发送请求和数据给 Web 层的 LoginServlet

(2) 在 LoginServlet 中接收请求和数据 [用户名和密码]

(3)LoginServlet 接收到请求和数据后，调用 Service 层完成根据用户名和密码查询用户对象

(4) 在 Service 层需要编写 UserService 类，在类中实现 login 方法，方法中调用 Dao 层的 UserMapper

(5) 在 UserMapper 接口中，声明一个根据用户名和密码查询用户信息的方法

(6)Dao 层把数据查询出来以后，将返回数据封装到 User 对象，将对象交给 Service 层

(7)Service 层将数据返回给 Web 层

(8)Web 层获取到 User 对象后，判断 User 对象，如果为 Null, 则将错误信息响应给登录页面，如果不为 Null，则跳转到列表页面，并把当前登录用户的信息存入 Session 携带到列表页面。

**具体实现**

**(1) 完成 Dao 层的代码编写**

`UserMapper.java`放到 mapper 包下:

```
public interface UserMapper {
    /**
     * 根据用户名和密码查询用户对象
     * @param username
     * @param password
     * @return
     */
    @Select("select * from tb_user where username = #{username} and password = #{password}")
    User select(@Param("username") String username,@Param("password")  String password);
 
    /**
     * 根据用户名查询用户对象
     * @param username
     * @return
     */
    @Select("select * from tb_user where username = #{username}")
    User selectByUsername(String username);
 
    /**
     * 添加用户
     * @param user
     */
    @Insert("insert into tb_user values(null,#{username},#{password})")
    void add(User user);
}
```

`User.java`放到`pojo`包下:

```
public class User {
 
    private Integer id;
    private String username;
    private String password;
 
    public Integer getId() {
        return id;
    }
 
    public void setId(Integer id) {
        this.id = id;
    }
 
    public String getUsername() {
        return username;
    }
 
    public void setUsername(String username) {
        this.username = username;
    }
 
    public String getPassword() {
        return password;
    }
 
    public void setPassword(String password) {
        this.password = password;
    }
 
    @Override
    toString方法
}
```

`UserMapper.xml`放入到 resources/com/itheima/mapper` 目录下:

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.mapper.UserMapper">
 
</mapper>
```

**(2) 完成 Service 层的代码编写**

(2.1) 在`com.itheima.service`包下，创建 UserService 类

```
public class UserService {
    //1.使用工具类获取SqlSessionFactory
    SqlSessionFactory factory = SqlSessionFactoryUtils.getSqlSessionFactory();
    /**
     * 登录方法，输入用户名密码，返回包含用户所有信息的对象
     * @param username
     * @param password
     * @return
     */
    public User login(String username,String password){
        //2. 获取SqlSession
        SqlSession sqlSession = factory.openSession();
        //3. 获取UserMapper
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        //4. 调用方法
        User user = mapper.select(username, password);
        //释放资源
        sqlSession.close();
 
        return  user;
    }
}
```

(3) 完成页面和 Web 层的代码编写

(3.1)`webapp`目录:

![](<assets/1740015475677.png>)

(3.2) 将 login.html 内容修改成 login.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!DOCTYPE html>
<html lang="en">
 
<head>
    <meta charset="UTF-8">
    <title>login</title>
    <link href="css/login.css" rel="stylesheet">
</head>
 
<body>
<div id="loginDiv" style="height: 350px">
    <form action="/brand-demo/loginServlet" method="post" id="form">
        <h1 id="loginMsg">LOGIN IN</h1>
        <div id="errorMsg">用户名或密码不正确</div>
        <p>Username:<input id="username" name="username" type="text"></p>
        <p>Password:<input id="password" name="password" type="password"></p>
        <p>Remember:<input id="remember" name="remember" type="checkbox"></p>
        <div id="subDiv">
            <input type="submit" class="button" value="login up">
            <input type="reset" class="button" value="reset">   
            <a href="register.html">没有账号？</a>
        </div>
    </form>
</div>
</body>
</html>
```

(3.3) 创建 LoginServlet 类

```
@WebServlet("/loginServlet")
public class LoginServlet extends HttpServlet {
    private UserService service = new UserService();
 
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 获取用户名和密码
        String username = request.getParameter("username");
        String password = request.getParameter("password");
   
        //2. 调用service查询
        User user = service.login(username, password);
 
        //3. 判断
        if(user != null){
            //登录成功，跳转到查询所有的BrandServlet
            
            //将登陆成功后的user对象，存储到session，以便于在商品界面能展示用户信息。因为是敏感信息，用session不用cookie
            HttpSession session = request.getSession();
            session.setAttribute("user",user);
            
            String contextPath = request.getContextPath();
            //重定向到查询所有商品页面，不同业务间，不用携带参数，并且地址栏跳转到新页面，使用重定向。如果使用转发的话，就要用setAttribute，跳转后地址栏还是登录地址，刷新后就又要重新登录了。
            response.sendRedirect(contextPath+"/selectAllServlet");
        }else {
            // 登录失败,
            // 存储错误信息到request
            request.setAttribute("login_msg","用户名或密码错误");
            request.setAttribute("username",username);
            request.setAttribute("password",password);
            // 跳转到login.jsp，同一业务并且要携带登陆失败参数，用转发，转发页面刷新浏览器会提示“确认重新提交表单”
            request.getRequestDispatcher("/login.jsp").forward(request,response);
 
        }
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

(3.4) 在 brand.jsp 中 < body > 标签下添加欢迎当前用户的提示信息:

```
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>${user.username}，欢迎你</h1>
<hr>
<input type="button" id="add" value="add"><br>
<table border="1" cellspacing="0" width="80%">
    <tr>
        <th>序号</th>
        <th>品牌名称</th>
        <th>企业名称</th>
        <th>排序</th>
        <th>品牌介绍</th>
        <th>状态</th>
        <th>操作</th>
    </tr>
 
    <c:forEach items="${brands}" var="brand" varStatus="status">
        <tr align="center">
                <%--<td>${brand.id}</td>--%>
            <td>${status.count}</td>
            <td>${brand.brandName}</td>
            <td>${brand.companyName}</td>
            <td>${brand.ordered}</td>
            <td>${brand.description}</td>
            <c:if test="${brand.status == 1}">
                <td>启用</td>
            </c:if>
            <c:if test="${brand.status != 1}">
                <td>禁用</td>
            </c:if>
            <td><a href="/brand-demo/SelectByIdServlet?id=${brand.id}">修改</a> <a href="#">删除</a></td>
        </tr>
    </c:forEach>
</table>
<script>
    document.getElementById("add").onclick = function (){
        location.href = "/brand-demo/addBrand.jsp";
    }
</script>
</body>
</html>
```

(3.5) 修改 login.jsp，将错误信息使用 EL 表达式来获取，回填错误的用户名密码

```
        <c:if test="${login_msg==null}">
            <div id="errorMsg">请输入账号密码</div>
        </c:if>
        <c:if test="${login_msg!=null}">
            <div id="errorMsg">${login_msg}</div>
        </c:if>
```

```
        <p>Username:<input id="username" name="username" type="text" value="${username}"></p>
 
        <p>Password:<input id="password" name="password" type="password" value="${password}"></p>
```

**(4) 启动，访问测试**

(4.1) 进入登录页面，输入错误的用户名或密码

![](<assets/1740015475930.png>)

(4.2) 输入正确的用户和密码信息

![](<assets/1740015476226.png>)

**小结**

*   在 LoginServlet 中，将登录成功的**用户数据存入 session** 中，方法在列表页面中获取当前登录用户信息进行展示
    
*   在 LoginServlet 中，将**登录失败的错误信息存入到 request** 中，如果存入到 session 中就会出现这次会话的所有请求都有登录失败的错误信息，这个是不需要的，所以不用存入到 session 中
    

#### 1.5.3 记住我 - 设置 Cookie

**1. 需求:**

如果用户勾选 “记住用户” ，则下次访问登陆页面自动填充用户名密码。这样可以提升用户的体验。

![](<assets/1740015476589.png>)

对应上面这个需求，最大的问题就是: 如何自动填充用户名和密码?

**2. 实现流程分析**

**如何自动填充用户名和密码？**

1. 将用户名和密码写入 **Cookie** 中，并且持久化存储 Cookie, 下次访问浏览器会自动携带 Cookie

2. 在页面获取 Cookie 数据后，设置到用户名和密码框中

**何时写入 Cookie?**

1. 用户必须**登陆成功后**才需要写

2. 用户必须在登录页面勾选了`记住我`的复选框

![](<assets/1740015476831.png>)

(1) 前端需要在发送请求和数据的时候，多携带一个用户是否勾选`Remember`的数据

(2)LoginServlet 获取到数据后，调用 Service 完成用户名和密码的判定

(3) 登录成功，并且用户在前端勾选了`记住我`，需要往 Cookie 中写入用户名和密码的数据，并设置 Cookie 存活时间

(4) 设置成功后，将数据响应给前端

**3. 具体实现**

(1) 在 login.jsp 为复选框设置值

```
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2022/8/3
  Time: 21:14
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html lang="en">
 
<head>
    <meta charset="UTF-8">
    <title>login</title>
    <link href="css/login.css" rel="stylesheet">
</head>
 
<body>
<div id="loginDiv" style="height: 350px">
 
    <form action="/brand-demo/LoginServlet" id="form" method="post">
        <h1 id="loginMsg">LOGIN IN</h1>
        <c:if test="${login_msg==null}">
            <div id="errorMsg">请输入账号密码</div>
        </c:if>
        <c:if test="${login_msg!=null}">
            <div id="errorMsg">${login_msg}</div>
        </c:if>
        <p>Username:<input id="username" name="username" type="text" value="${username}"></p>
 
        <p>Password:<input id="password" name="password" type="password" value="${password}"></p>
        <p>Remember:<input id="remember" name="remember" value="1" type="checkbox"></p>
        <div id="subDiv">
            <input type="submit" class="button" value="login up">
<%--             表示空格--%>
            <input type="reset" class="button" value="reset">   
            <a href="register.html">没有账号？</a>
        </div>
    </form>
</div>
 
</body>
</html>
```

(2) 在 LoginServlet 获取复选框的值并在登录成功后进行设置 Cookie

```
@WebServlet("/loginServlet")
public class LoginServlet extends HttpServlet {
    private UserService service = new UserService();
 
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 获取用户名和密码
        String username = request.getParameter("username");
        String password = request.getParameter("password");
 
        //获取复选框数据
        String remember = request.getParameter("remember");
 
        //2. 调用service查询
        User user = service.login(username, password);
 
        //3. 判断
        if(user != null){
            //登录成功，跳转到查询所有的BrandServlet
 
            //判断用户是否勾选记住我，字符串写前面是为了避免出现空指针异常
            if("1".equals(remember)){
                //勾选了，发送Cookie
                //1. 创建Cookie对象
                Cookie c_username = new Cookie("username",username);
                Cookie c_password = new Cookie("password",password);
                // 设置Cookie的存活时间
                c_username.setMaxAge( 60 * 60 * 24 * 7);
                c_password.setMaxAge( 60 * 60 * 24 * 7);
                //2. 发送
                response.addCookie(c_username);
                response.addCookie(c_password);
            }
 
            //将登陆成功后的user对象，存储到session
            HttpSession session = request.getSession();
            session.setAttribute("user",user);
 
            String contextPath = request.getContextPath();
            response.sendRedirect(contextPath+"/selectAllServlet");
        }else {
            // 登录失败,
 
            // 存储错误信息到request
            request.setAttribute("login_msg","用户名或密码错误");
 
            // 跳转到login.jsp
            request.getRequestDispatcher("/login.jsp").forward(request,response);
 
        }
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

(3) 启动访问测试，

只有当前用户名和密码输入正确，并且勾选了 Remeber 的复选框，在响应头中才可以看得 cookie 的相关数据

![](<assets/1740015477095.png>)

#### 1.5.4 记住我 - 获取 Cookie

**1. 需求**

登录成功并勾选了 Remeber 后，后端返回给前端的 Cookie 数据就已经存储好了，接下来就需要在页面获取 Cookie 中的数据，并把数据设置到登录页面的用户名和密码框中。

![](<assets/1740015477311.png>)

如何在页面直接获取 Cookie 中的值呢?

**2. 实现流程分析**

在页面可以使用 EL 表达式，${cookie.**key**.value}

key: 指的是存储在 cookie 中的键名称

![](<assets/1740015477540.png>)

(1) 在 login.jsp 用户名的表单输入框使用 value 值给表单元素添加默认值，value 可以使用`${cookie.username.value}`

(2) 在 login.jsp 密码的表单输入框使用 value 值给表单元素添加默认值，value 可以使用`${cookie.password.value}`

**3. 具体实现**

(1) 修改 login.jsp 页面

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!DOCTYPE html>
<html lang="en">
 
<head>
    <meta charset="UTF-8">
    <title>login</title>
    <link href="css/login.css" rel="stylesheet">
</head>
 
<body>
<div id="loginDiv" style="height: 350px">
    <form action="/brand-demo/loginServlet" method="post" id="form">
        <h1 id="loginMsg">LOGIN IN</h1>
        <div id="errorMsg">${login_msg}</div>
        <p>Username:<input id="username" name="username" value="${cookie.username.value}" type="text"></p>
 
        <p>Password:<input id="password" name="password" value="${cookie.password.value}" type="password"></p>
        <p>Remember:<input id="remember" name="remember" value="1" type="checkbox"></p>
        <div id="subDiv">
            <input type="submit" class="button" value="login up">
            <input type="reset" class="button" value="reset">   
            <a href="register.html">没有账号？</a>
        </div>
    </form>
</div>
</body>
</html>
```

4. 访问测试，重新访问登录页面，就可以看得用户和密码已经被填充。

![](<assets/1740015477808.png>)

#### 1.5.5 用户注册功能

**需求**

注册功能：保存用户信息到数据库

验证码功能

展示验证码：展示验证码图片，并可以点击切换

校验验证码：验证码填写不正确，则注册失败

![](<assets/1740015478065.png>)

**2. 实现流程分析**

![](<assets/1740015478353.png>)

(1) 前端通过表单发送请求和数据给 Web 层的 RegisterServlet

(2) 在 RegisterServlet 中接收请求和数据 [用户名和密码]

(3)RegisterServlet 接收到请求和数据后，调用 Service 层完成用户信息的保存

(4) 在 Service 层需要编写 UserService 类，在类中实现 register 方法，需要判断用户是否已经存在，如果不存在，则完成用户数据的保存

(5) 在 UserMapper 接口中，声明两个方法，一个是根据用户名查询用户信息方法，另一个是保存用户信息方法

(6) 在 UserService 类中保存成功则返回 true，失败则返回 false, 将数据返回给 Web 层

(7)Web 层获取到结果后，如果返回的是 true, 则提示`注册成功`，并转发到登录页面，如果返回 false 则提示`用户名已存在`并转发到注册页面

**3. 具体实现**

(1)Dao 层代码参考资料中的内容完成

(2) 编写 Service 层代码

```
public class UserService {
    //1.使用工具类获取SqlSessionFactory
    SqlSessionFactory factory = SqlSessionFactoryUtils.getSqlSessionFactory();
    /**
     * 注册方法
     * @return
     */
 
    public boolean register(User user){
        //2. 获取SqlSession
        SqlSession sqlSession = factory.openSession();
        //3. 获取UserMapper
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        //4. 判断用户名是否存在
        User u = mapper.selectByUsername(user.getUsername());
 
        if(u == null){
            // 用户名不存在，注册
            mapper.add(user);
            sqlSession.commit();
        }
        sqlSession.close();
 
        return u == null;
 
    }
}
```

(3) 完成页面和 Web 层的代码编写

(3.1) 将 register.html 内容修改成 register.jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>欢迎注册</title>
    <link href="css/register.css" rel="stylesheet">
</head>
<body>
<div class="form-div">
    <div class="reg-content">
        <h1>欢迎注册</h1>
        <span>已有帐号？</span> <a href="login.jsp">登录</a>
    </div>
    <form id="reg-form" action="/brand-demo/registerServlet" method="post">
        <table>
            <tr>
                <td>用户名</td>
                <td class="inputs">
                    <input name="username" type="text" id="username">
                    <br>
                    <span id="username_err" class="err_msg" style="display:none">用户名不太受欢迎</span>
                </td>
            </tr>
            <tr>
                <td>密码</td>
                <td class="inputs">
                    <input name="password" type="password" id="password">
                    <br>
                    <span id="password_err" class="err_msg" style="display: none">密码格式有误</span>
                </td>
            </tr>
            <tr>
                <td>验证码</td>
                <td class="inputs">
                    <input name="checkCode" type="text" id="checkCode">
                    <img src="imgs/a.jpg">
                    <a href="#" id="changeImg" >看不清？</a>
                </td>
            </tr>
        </table>
        <div class="buttons">
            <input value="注 册" type="submit" id="reg_btn">
        </div>
        <br class="clear">
    </form>
</div>
</body>
</html>
```

(3.2) 编写 RegisterServlet

```
@WebServlet("/registerServlet")
public class RegisterServlet extends HttpServlet {
    private UserService service = new UserService();
 
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
       //1. 获取用户名和密码数据
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        
        User user = new User();
        user.setUsername(username);
        user.setPassword(password);
 
        //2. 调用service 注册
        boolean flag = service.register(user);
        //3. 判断注册成功与否
        if(flag){
             //注册功能，跳转登陆页面
            request.setAttribute("register_msg","注册成功，请登录");
            request.getRequestDispatcher("/login.jsp").forward(request,response);
        }else {
            //注册失败，跳转到注册页面
 
            request.setAttribute("register_msg","用户名已存在");
            request.getRequestDispatcher("/register.jsp").forward(request,response);
        }
 
 
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

(3.3) 需要在页面上展示后台返回的错误信息，需要修改 register.jsp

```
修改前:<span id="username_err" class="err_msg" style="display:none">用户名不太受欢迎</span>
修改后:<span id="username_err" class="err_msg">${register_msg}</span>
```

(3.4) 如果注册成功，需要把成功信息展示在登录页面，所以也需要修改 login.jsp

```
修改前:<div id="errorMsg">${login_msg}</div>
修改后:<div id="errorMsg">${login_msg} ${register_msg}</div>
```

(3.5) 修改 login.jsp，将注册跳转地址修改为 register.jsp

```
修改前：<a href="register.html">没有账号？</a>
修改后: <a href="register.jsp">没有账号？</a>
```

(3.6) 启动测试，

如果是注册的用户信息已经存在:

![](<assets/1740015478707.png>)

如果注册的用户信息不存在，注册成功:

![](<assets/1740015478927.png>)

#### 1.5.6 验证码 - 展示

**1. 需求分析**

展示验证码：展示验证码图片，并可以点击切换

![](<assets/1740015479208.png>)

验证码的生成是通过工具类来实现的，具体的工具类参考

`CheckCodeUtil.java`

```
package package1.util;
 
import javax.imageio.ImageIO;
import java.awt.*;
import java.awt.geom.AffineTransform;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.util.Arrays;
import java.util.Random;
 
/**
 * 生成验证码工具类
 */
public class CheckCodeUtil {
 
    public static final String VERIFY_CODES = "123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    private static Random random = new Random();
 
 
 
    /**
     * 输出随机验证码图片流,并返回验证码值（一般传入输出流，响应response页面端，Web项目用的较多）
     *
     * @param w
     * @param h
     * @param os
     * @param verifySize
     * @return
     * @throws IOException
     */
    public static String outputVerifyImage(int w, int h, OutputStream os, int verifySize) throws IOException {
        String verifyCode = generateVerifyCode(verifySize);
        outputImage(w, h, os, verifyCode);
        return verifyCode;
    }
 
    /**
     * 使用系统默认字符源生成验证码
     *
     * @param verifySize 验证码长度
     * @return
     */
    public static String generateVerifyCode(int verifySize) {
        return generateVerifyCode(verifySize, VERIFY_CODES);
    }
 
    /**
     * 使用指定源生成验证码
     *
     * @param verifySize 验证码长度
     * @param sources    验证码字符源
     * @return
     */
    public static String generateVerifyCode(int verifySize, String sources) {
        // 未设定展示源的字码，赋默认值大写字母+数字
        if (sources == null || sources.length() == 0) {
            sources = VERIFY_CODES;
        }
        int codesLen = sources.length();
        Random rand = new Random(System.currentTimeMillis());
        StringBuilder verifyCode = new StringBuilder(verifySize);
        for (int i = 0; i < verifySize; i++) {
            verifyCode.append(sources.charAt(rand.nextInt(codesLen - 1)));
        }
        return verifyCode.toString();
    }
 
    /**
     * 生成随机验证码文件,并返回验证码值 (生成图片形式，用的较少)
     *
     * @param w
     * @param h
     * @param outputFile
     * @param verifySize
     * @return
     * @throws IOException
     */
    public static String outputVerifyImage(int w, int h, File outputFile, int verifySize) throws IOException {
        String verifyCode = generateVerifyCode(verifySize);
        outputImage(w, h, outputFile, verifyCode);
        return verifyCode;
    }
 
 
 
    /**
     * 生成指定验证码图像文件
     *
     * @param w
     * @param h
     * @param outputFile
     * @param code
     * @throws IOException
     */
    public static void outputImage(int w, int h, File outputFile, String code) throws IOException {
        if (outputFile == null) {
            return;
        }
        File dir = outputFile.getParentFile();
        //文件不存在
        if (!dir.exists()) {
            //创建
            dir.mkdirs();
        }
        try {
            outputFile.createNewFile();
            FileOutputStream fos = new FileOutputStream(outputFile);
            outputImage(w, h, fos, code);
            fos.close();
        } catch (IOException e) {
            throw e;
        }
    }
 
    /**
     * 输出指定验证码图片流
     *
     * @param w
     * @param h
     * @param os
     * @param code
     * @throws IOException
     */
    public static void outputImage(int w, int h, OutputStream os, String code) throws IOException {
        int verifySize = code.length();
        BufferedImage image = new BufferedImage(w, h, BufferedImage.TYPE_INT_RGB);
        Random rand = new Random();
        Graphics2D g2 = image.createGraphics();
        g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
 
        // 创建颜色集合，使用java.awt包下的类
        Color[] colors = new Color[5];
        Color[] colorSpaces = new Color[]{Color.WHITE, Color.CYAN,
                Color.GRAY, Color.LIGHT_GRAY, Color.MAGENTA, Color.ORANGE,
                Color.PINK, Color.YELLOW};
        float[] fractions = new float[colors.length];
        for (int i = 0; i < colors.length; i++) {
            colors[i] = colorSpaces[rand.nextInt(colorSpaces.length)];
            fractions[i] = rand.nextFloat();
        }
        Arrays.sort(fractions);
        // 设置边框色
        g2.setColor(Color.GRAY);
        g2.fillRect(0, 0, w, h);
 
        Color c = getRandColor(200, 250);
        // 设置背景色
        g2.setColor(c);
        g2.fillRect(0, 2, w, h - 4);
 
        // 绘制干扰线
        Random random = new Random();
        // 设置线条的颜色
        g2.setColor(getRandColor(160, 200));
        for (int i = 0; i < 20; i++) {
            int x = random.nextInt(w - 1);
            int y = random.nextInt(h - 1);
            int xl = random.nextInt(6) + 1;
            int yl = random.nextInt(12) + 1;
            g2.drawLine(x, y, x + xl + 40, y + yl + 20);
        }
 
        // 添加噪点
        // 噪声率
        float yawpRate = 0.05f;
        int area = (int) (yawpRate * w * h);
        for (int i = 0; i < area; i++) {
            int x = random.nextInt(w);
            int y = random.nextInt(h);
            // 获取随机颜色
            int rgb = getRandomIntColor();
            image.setRGB(x, y, rgb);
        }
        // 添加图片扭曲
        shear(g2, w, h, c);
 
        g2.setColor(getRandColor(100, 160));
        int fontSize = h - 4;
        Font font = new Font("Algerian", Font.ITALIC, fontSize);
        g2.setFont(font);
        char[] chars = code.toCharArray();
        for (int i = 0; i < verifySize; i++) {
            AffineTransform affine = new AffineTransform();
            affine.setToRotation(Math.PI / 4 * rand.nextDouble() * (rand.nextBoolean() ? 1 : -1), (w / verifySize) * i + fontSize / 2, h / 2);
            g2.setTransform(affine);
            g2.drawChars(chars, i, 1, ((w - 10) / verifySize) * i + 5, h / 2 + fontSize / 2 - 10);
        }
 
        g2.dispose();
        ImageIO.write(image, "jpg", os);
    }
 
    /**
     * 随机颜色
     *
     * @param fc
     * @param bc
     * @return
     */
    private static Color getRandColor(int fc, int bc) {
        if (fc > 255) {
            fc = 255;
        }
        if (bc > 255) {
            bc = 255;
        }
        int r = fc + random.nextInt(bc - fc);
        int g = fc + random.nextInt(bc - fc);
        int b = fc + random.nextInt(bc - fc);
        return new Color(r, g, b);
    }
 
    private static int getRandomIntColor() {
        int[] rgb = getRandomRgb();
        int color = 0;
        for (int c : rgb) {
            color = color << 8;
            color = color | c;
        }
        return color;
    }
 
    private static int[] getRandomRgb() {
        int[] rgb = new int[3];
        for (int i = 0; i < 3; i++) {
            rgb[i] = random.nextInt(255);
        }
        return rgb;
    }
 
    private static void shear(Graphics g, int w1, int h1, Color color) {
        shearX(g, w1, h1, color);
        shearY(g, w1, h1, color);
    }
 
    private static void shearX(Graphics g, int w1, int h1, Color color) {
 
        int period = random.nextInt(2);
 
        boolean borderGap = true;
        int frames = 1;
        int phase = random.nextInt(2);
 
        for (int i = 0; i < h1; i++) {
            double d = (double) (period >> 1)
                    * Math.sin((double) i / (double) period
                    + (6.2831853071795862D * (double) phase)
                    / (double) frames);
            g.copyArea(0, i, w1, 1, (int) d, 0);
            if (borderGap) {
                g.setColor(color);
                g.drawLine((int) d, i, 0, i);
                g.drawLine((int) d + w1, i, w1, i);
            }
        }
 
    }
 
    private static void shearY(Graphics g, int w1, int h1, Color color) {
 
        int period = random.nextInt(40) + 10; // 50;
 
        boolean borderGap = true;
        int frames = 20;
        int phase = 7;
        for (int i = 0; i < w1; i++) {
            double d = (double) (period >> 1)
                    * Math.sin((double) i / (double) period
                    + (6.2831853071795862D * (double) phase)
                    / (double) frames);
            g.copyArea(i, 0, 1, h1, 0, (int) d);
            if (borderGap) {
                g.setColor(color);
                g.drawLine(i, (int) d, i, 0);
                g.drawLine(i, (int) d + h1, i, h1);
            }
 
        }
 
    }
}
 
```

在该工具类中编写 main 方法进行测试:

```
public static void main(String[] args) throws IOException {
    //生成验证码的图片位置
    OutputStream fos = new FileOutputStream("d://a.jpg");
    //checkCode为最终验证码的数据
    String checkCode = CheckCodeUtil.outputVerifyImage(100, 50, fos, 4);
    System.out.println(checkCode);
}
```

生成完验证码以后，我们就可以知晓:

*   验证码就是使用 Java 代码生成的一张图片
    
*   验证码的作用: 防止机器自动注册，攻击服务器
    

**2. 实现流程分析**

![](<assets/1740015479407.png>)

(1) 前端发送请求给 CheckCodeServlet

(2)CheckCodeServlet 接收到请求后，生成验证码图片，将图片用 Reponse 对象的输出流写回到前端

思考: 如何将图片写回到前端浏览器呢?

(1)Java 中已经有工具类生成验证码图片，测试类中只是把图片生成到磁盘上 (2) 生成磁盘的过程中使用的是 OutputStream 流，如何把这个图片生成在页面呢? (3) 前面在将 Reponse 对象的时候，它有一个方法可以获取其字节输出流，getOutputStream() (4) 综上所述，我们可以把写往磁盘的流对象更好成 Response 的字节流，即可完成图片响应给前端

**3. 具体实现**

(1) 修改 Register.jsp 页面，将验证码的图片从后台获取

```
<tr>
    <td>验证码</td>
        <td class="inputs">
        <input name="checkCode" type="text" id="checkCode">
        <img id="checkCodeImg" src="/brand-demo/checkCodeServlet">
<%--                    用a链接href="#"是为了让“看不清”带下划线，显示的像个链接--%>
        <a href="#" id="changeImg" >看不清？</a>
    </td>
</tr>
 
<script>
    document.getElementById("changeImg").onclick = function () {
       	//路径后面添加时间戳的目的是避免浏览器进行缓存静态资源，不加的话每次刷新图片看起来不变
        document.getElementById("checkCodeImg").src = "/brand-demo/checkCodeServlet?"+new Date().getMilliseconds();
    }
</script>
```

(2) 编写 CheckCodeServlet 类，用来接收请求生成验证码

```
@WebServlet("/checkCodeServlet")
public class CheckCodeServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 生成验证码
        ServletOutputStream os = response.getOutputStream();
        String checkCode = CheckCodeUtil.outputVerifyImage(100, 50, os, 4);
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

#### 1.5.7 验证码 - 校验

**1. 需求**

*   判断程序生成的验证码 和 用户输入的验证码 是否一样，如果不一样，则阻止注册
    
*   验证码图片访问和提交注册表单是**两次**请求，所以要将程序生成的验证码存入 Session 中
    

![](<assets/1740015479672.png>)

思考: 为什么要把验证码数据存入到 Session 中呢?

*   生成验证码和校验验证码是两次请求，此处就需要在一个会话的两次请求之间共享数据
    
*   验证码属于安全数据类的，所以我们选中 Session 来存储验证码数据。
    

**2. 实现流程分析**

(1) 在 CheckCodeServlet 中生成验证码的时候，将验证码数据存入 Session 对象

(2) 前端将验证码和注册数据提交到后台，交给 RegisterServlet 类

(3)RegisterServlet 类接收到请求和数据后，其中就有验证码，和 Session 中的验证码进行对比

(4) 如果一致，则完成注册，如果不一致，则提示错误信息

**3. 具体实现**

(1) 修改 CheckCodeServlet 类，将验证码存入 Session 对象

```
@WebServlet("/checkCodeServlet")
public class CheckCodeServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
 
        // 生成验证码
        ServletOutputStream os = response.getOutputStream();
        String checkCode = CheckCodeUtil.outputVerifyImage(100, 50, os, 4);
 
        // 存入Session
        HttpSession session = request.getSession();
        session.setAttribute("checkCodeGen",checkCode);
 
 
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

(2) 在 RegisterServlet 中，获取页面的和 session 对象中的验证码，进行对比

```
package com.itheima.web;
 
import com.itheima.pojo.User;
import com.itheima.service.UserService;
 
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.*;
import java.io.IOException;
 
@WebServlet("/registerServlet")
public class RegisterServlet extends HttpServlet {
    private UserService service = new UserService();
 
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
       //1. 获取用户名和密码数据
        String username = request.getParameter("username");
        String password = request.getParameter("password");
 
        User user = new User();
        user.setUsername(username);
        user.setPassword(password);
 
        // 获取用户输入的验证码
        String checkCode = request.getParameter("checkCode");
 
        // 程序生成的验证码，从Session获取
        HttpSession session = request.getSession();
        String checkCodeGen = (String) session.getAttribute("checkCodeGen");
 
        // 比对
        if(!checkCodeGen.equalsIgnoreCase(checkCode)){
 
            request.setAttribute("register_msg","验证码错误");
            request.getRequestDispatcher("/register.jsp").forward(request,response);
 
            // 不允许注册
            return;
        }
        //2. 调用service 注册
        boolean flag = service.register(user);
        //3. 判断注册成功与否
        if(flag){
             //注册功能，跳转登陆页面
 
            request.setAttribute("register_msg","注册成功，请登录");
            request.getRequestDispatcher("/login.jsp").forward(request,response);
        }else {
            //注册失败，跳转到注册页面
 
            request.setAttribute("register_msg","用户名已存在");
            request.getRequestDispatcher("/register.jsp").forward(request,response);
        }
 
 
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

至此，用户的注册登录功能就已经完成了。