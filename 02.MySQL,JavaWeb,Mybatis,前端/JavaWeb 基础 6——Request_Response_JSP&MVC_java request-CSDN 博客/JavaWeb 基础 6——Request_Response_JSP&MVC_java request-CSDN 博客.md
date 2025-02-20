---
url: https://blog.csdn.net/qq_40991313/article/details/126024588
title: JavaWeb 基础 6——Request,Response,JSP&MVC_java request-CSDN 博客
date: 2025-02-20 09:37:40
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[一、Request&Response 概述](#t0)

[二、Request 对象](#t1)

[2.1 Request 继承体系](#t2)

[2.2 Request 获取请求数据的方法](#t3)

[2.2.0 简述](#t4) 

[2.2.1 获取请求行数据](#t5)

[2.2.2 获取请求头数据](#t6)

[2.2.3 获取请求体数据](#t7)

[2.2.4 获取请求参数的通用方式](#t8)

[2.3 IDEA 使用模板创建 Servlet（继承 HttpServlet 版）](#t9)

[2.4 请求参数中文乱码问题](#t10)

[2.4.1 乱码原因和解决思路](#t11) 

[2.4.2 POST 请求 getParameter 解决方案](#t12) 

[2.4.3 GET 请求 getParameter 解决方案](#t13)

[2.4.4 URL 编码解码](#t14)

[2.5 Request 请求转发](#t15)

[三、Response 对象](#t16)

[3.1 Response 设置响应数据](#t17)

[3.2 Respones 请求重定向](#t18)

[3.2.1 重定向](#t19)

[3.2.2 对比转发和重定向，区别和应用场景](#t20)

[3.3 是否加虚拟目录的依据，动态配置虚拟目录](#t21)

[3.4 Response 响应字符数据](#t22)

[3.5 Response 响应字节数据](#t23)

[四、Request、Response、Mybatis、前端注册登录小项目](#t24)

[4.1 代码](#t25)

[4.2 SqlSessionFactory 工具类抽取](#t26)

[五、JSP](#t27)

[5.1 JSP 概述](#t28)

[5.2 helloworld](#t29)

[5.3 JSP 原理](#t30)

[5.4 JSP 脚本](#t31)

[5.4.1 JSP 脚本分类](#t32)

[5.4.2 案例, 使用 JSP 脚本展示品牌数据](#t33)

[5.4.3 JSP 缺点](#t34)

[5.5 EL 表达式](#t35)

[5.5.1 概述](#t36)

[5.5.2 代码演示，把 Servlet 的 List 转发给 jsp 页面输出](#t37)

[5.5.3 域对象](#t38)

[5.6 JSTL（JSP 标准标签库）标签](#t39)

[5.6.1 概述](#t40)

[5.6.2 if 标签](#t41)

[5.6.3 forEach 标签](#t42)

[5.7 MVC 模式和三层架构](#t43)

[5.7.1 MVC 模式](#t44)

[5.7.2 三层架构](#t45)

[5.7.3 MVC 和三层架构](#t46)

[5.8 品牌数据增删改查案例](#t47)

[5.8.1 需求：完成品牌数据的增删改查操作](#t48)

[5.8.2 主要坑点](#t49)

[5.8.3 环境准备](#t50)

[5.8.4 查询所有​编辑](#t51)

[5.8.5 添加​编辑](#t52)

[5.8.6 修改（回显数据）​编辑](#t53)

## 一、Request&Response 概述

**Request 是请求对象，Response 是响应对象。**

思考：

这两个对象在我们使用 Servlet 的时候有看到，此时，我们就需要思考一个问题 request 和 response 这两个参数的作用是什么?

![](<assets/1740015460627.png>)

*   **request: 获取请求数据**
    
    *   浏览器会发送 HTTP 请求到后台服务器 [Tomcat]
        
    *   HTTP 的请求中会包含很多请求数据 [请求行 + 请求头 + 请求体]
        
    *   **后台服务器** [Tomcat] 会对 HTTP 请求中的数据进行**解析**并把解析结果存入到 **request** **对象**中
        
    *   我们可以从 request 对象中获取请求的相关参数
        
    *   获取到数据后就可以继续后续的业务，比如获取用户名和密码就可以实现登录操作的相关业务
        
*   **response: 设置响应数据**
    
    *   业务处理完后，**后台就需要给前端返回业务处理的结果**即响应数据
        
    *   把响应数据封装到 **response 对象**中
        
    *   后台服务器 [Tomcat] 会**解析** response 对象, 按照 [响应行 + 响应头 + 响应体] 格式拼接结果
        
    *   **浏览器**最终解析结果，把内容展示在浏览器给用户浏览
        

**案例：** 

对于上述所讲的内容，我们通过一个案例来初步体验下 request 和 response 对象的使用。

```
@WebServlet("/demo3")
public class ServletDemo3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //使用request对象 获取请求数据
        String name = request.getParameter("name");//url?name=zhangsan
 
        //使用response对象 设置响应数据
        response.setHeader("content-type","text/html;charset=utf-8");
        response.getWriter().write("<h1>"+name+",欢迎您！</h1>");
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("Post...");
    }
}
```

启动成功后就可以通过浏览器来访问，并且根据传入参数的不同就可以在页面上展示不同的内容:

![](<assets/1740015460926.png>)

**小结**

在这节中，我们主要认识了下 request 对象和 reponse 对象:

*   request 对象是用来封装请求数据的对象
    
*   response 对象是用来封装响应数据的对象
    

## 二、Request 对象

### 2.1 Request 继承体系

*   **Tomcat 需要解析请求数据，封装为 request 对象, 并且创建 request 对象传递到 service 方法**
    

**思考**

*   ServletRequest 和 HttpServletRequest 的关系是什么?
    
*   request 对象是有谁来创建的?
    
*   request 提供了哪些 API, 这些 API 从哪里查?
    

**Request 的继承体系:**

![](<assets/1740015461170.png>)

**`RequestFacade类`**:

*   该类是 Tomcat 定义的实现类，**实现了 HttpServletRequest 接口**，也间接实现了 ServletRequest 接口。
    
*   Servlet 类中的 service 方法、doGet 方法或者是 doPost 方法最终都是由 Web 服务器 [Tomcat] 来调用的，所以 **Tomcat 提供了方法参数接口的具体实现类`RequestFacade`**，并完成了对象的创建
    
*   要想了解 RequestFacade 中都提供了哪些方法，我们可以直接查看 JavaEE 的 API 文档中关于 ServletRequest 和 HttpServletRequest 的接口文档，因为 RequestFacade 实现了其接口就需要重写接口中的方法
    

### 2.2 Request 获取请求数据的方法

#### 2.2.0 简述 

HTTP 请求数据总共分为三部分内容，分别是**请求行、请求头、请求体**。

HTTP 请求数据中包含了`请求行`、`请求头`和`请求体`，针对这三部分内容，Request 对象都提供了对应的 API 方法来获取对应的值:

*   **请求行**
    
    *   getMethod() 获取请求方式
        
    *   getContextPath() 获取项目访问路径
        
    *   getRequestURL() 获取请求 URL
        
    *   getRequestURI() 获取请求 URI
        
    *   getQueryString() 获取 GET 请求方式的请求参数
        
*   **请求头**
    
    *   getHeader(String name) 根据请求头名称获取其对应的值
        
*   **请求体**
    
    *   注意: **POST 请求有请求体，GET 请求无请求体**
        
    *   如果是纯文本数据: getReader()
        
    *   如果是字节数据如文件数据: getInputStream()
        

#### **2.2.1 获取请求行数据**

请求行包含三块内容，分别是`请求方式`、`请求资源路径`、`HTTP协议及版本`

![](<assets/1740015461437.png>)

对于这三部分内容，request 对象都提供了对应的 API 方法来获取，具体如下:

![](<assets/1740015461655.png>)

*   **获取请求方式**: `GET`
    

```
String getMethod()

```

*   **获取虚拟目录** (项目名 / 项目访问路径): `/request-demo`
    

```
String getContextPath()

```

*   **获取 URL**(统一资源定位符): `http://localhost:8080/request-demo/req1`
    

```
StringBuffer getRequestURL()

```

*   **获取 URI**(统一资源标识符): `/request-demo/req1`
    

```
String getRequestURI()

```

*   **获取请求参数** (GET 方式): `username=zhangsan&password=123`
    

```
String getQueryString()

```

介绍完上述方法后，咱们通过代码把上述方法都使用下:

![](<assets/1740015462055.png>)

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form action="/request-demo/req1" method="get" target="_blank">
    <input name="username"/><input type="submit"/>
</form>
</body>
</html>
```

```
/**
 * request 获取请求数据
 */
@WebServlet("/req1")
public class RequestDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // String getMethod()：获取请求方式： GET
        String method = req.getMethod();
        System.out.println(method);//GET
        // String getContextPath()：获取虚拟目录(项目访问路径)：/request-demo
        String contextPath = req.getContextPath();
        System.out.println(contextPath);
        // StringBuffer getRequestURL(): 获取URL(统一资源定位符)：http://localhost:8080/request-demo/req1
        StringBuffer url = req.getRequestURL();
        System.out.println(url.toString());
        // String getRequestURI()：获取URI(统一资源标识符)： /request-demo/req1
        String uri = req.getRequestURI();
        System.out.println(uri);
        // String getQueryString()：获取请求参数（GET方式）： username=zhangsan
        String queryString = req.getQueryString();
        System.out.println(queryString);
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    }
}
```

启动服务器，访问`http://localhost:8080/request-demo/req1?username=zhangsan&passwrod=123`，获取的结果如下:

![](<assets/1740015462272.png>)

#### **2.2.2 获取请求头数据**

对于请求头的数据，格式为`key: value`如下:

```
Host: 表示请求的主机名
User-Agent: 浏览器版本,例如Chrome浏览器的标识类似Mozilla/5.0 ...Chrome/79，IE浏览器的标识类似Mozilla/5.0 (Windows NT ...)like Gecko；
Accept：表示浏览器能接收的资源类型，如text/*，image/*或者*/*表示所有；
Accept-Language：表示浏览器偏好的语言，服务器可以据此返回不同语言的网页；
Accept-Encoding：表示浏览器可以支持的压缩类型，例如gzip, deflate等。
```

![](<assets/1740015462480.png>)

  

所以根据请求头名称获取对应值的方法为:

```
String getHeader(String name)

```

接下来，在代码中如果想要获取客户端浏览器的版本信息，则可以使用

```
/**
 * request 获取请求数据
 */
@WebServlet("/req1")
public class RequestDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取请求头: user-agent: 浏览器的版本信息
        String agent = req.getHeader("user-agent");
		System.out.println(agent);
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    }
}
```

```
​

```

重新启动服务器后，`http://localhost:8080/request-demo/req1?username=zhangsan&passwrod=123`，获取的结果如下:

![](<assets/1740015462670.png>)

#### **2.2.3 获取请求体数据**

浏览器在发送 GET 请求的时候是没有请求体的，请求参数在请求行里。

所以需要把请求方式变更为 POST，请求参数 QueryString 在请求体里。请求体中的数据格式如下:

![](<assets/1740015462970.png>)

Request 对象**获取请求体数据方式：**

*   **获取字节输入流**，如果前端发送的是字节数据，比如传递的是文件数据，则使用该方法
    

```
ServletInputStream getInputStream()
该方法可以获取字节
```

*   **获取字符输入流**，如果前端发送的是纯文本数据，则使用该方法
    

```
BufferedReader getReader()

```

**示例：** 

1. 在项目的 webapp 目录下添加一个 html 页面，名称为：`req.html`

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!-- 
    action:form表单提交的请求地址
    method:请求方式，指定为post
-->
<form action="/request-demo/req1" method="post">
    <input type="text" name="username">
    <input type="password" name="password">
    <input type="submit">
</form>
</body>
</html>
```

2. 调用 getReader() 或者 getInputStream() 方法，因为目前前端传递的是纯文本数据，所以我们采用 getReader() 方法来获取

```
/**
 * request 获取请求数据
 */
@WebServlet("/req1")
public class RequestDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
         //获取post 请求体：请求参数
        //1. 获取字符输入流
        BufferedReader br = req.getReader();
        //2. 读取数据，读一行即可，因为请求体只有一行。所有请求参数会用＆符号拼接成一行
        String line = br.readLine();    
        System.out.println(line);
        //br.close();//可以不写，因为销毁request后br也就跟着销毁了
    }
}
```

**注意：**

BufferedReader 流是通过 request 对象来获取的，当**请求完成后 request 对象就会被销毁**，request 对象被销毁后，**BufferedReader 流就会自动关闭**，所以此处就不需要手动关闭流了。

3. 启动服务器，通过浏览器访问`http://localhost:8080/request-demo/req.html`

![](<assets/1740015463238.png>)

点击`提交`按钮后，就可以在控制台看到前端所发送的请求数据

![](<assets/1740015463518.png>)

#### 2.2.4 获取请求参数的通用方式

**1. 什么是请求参数?**

我们拿用户登录的例子来说明，**用户名和密码其实就是我们所说的请求参数**。

**2. 什么是请求数据?**

**请求数据则是包含请求行、请求头和请求体的所有数据**

获取请求参数的两种方法： 

**方法一：获取连体参数，get 和 post 不同**

*   GET 方式:
    

```
String getQueryString()

```

*   POST 方式:
    

```
BufferedReader getReader();

```

**示例：**

（1）发送一个 GET 请求并携带用户名，后台接收后打印到控制台

（2）发送一个 POST 请求并携带用户名，后台接收后打印到控制台

代码如下:

```
@WebServlet("/req1")
public class RequestDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
 
        String result = req.getQueryString();
        System.out.println(result);
 
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        BufferedReader br = req.getReader();
        String result = br.readLine();
        System.out.println(result);
    }
}
```

因为 get 和 post 请求参数分别在请求行和请求体，故存在代码重复问题

![](<assets/1740015463790.png>)

**方法二: 通用方法获取分割后的参数，get 和 post 通用**

request 对象把分割后的请求参数，存入到一个 Map 集合中:

![](<assets/1740015464159.png>)

**注意**: 因为参数的值可能是一个，也可能有多个，所以 Map 的值的类型为 String 数组。

基于上述理论，request 对象为我们提供了如下方法:

*   **获取所有参数 Map 集合**
    

```
Map<String,String[]> getParameterMap()    //标签的name：value。值是数组，因为有时传来的同name键对应多个value，例如checkbox。

```

*   **根据名称获取参数值（数组）**
    

```
String[] getParameterValues(String name)

```

*   **根据名称获取参数值 (单个值)，最常用**
    

```
String getParameter(String name)

```

**注意：**

用方法一获取后，再用方法一还能获取数值，再用方法二会返回不到数值。 

用方法二获取后，再用方法二还能获取数值，再用方法一会返回不到数值。

**案例演示：**

1. 修改 req.html 页面，添加爱好选项，爱好可以同时选多个

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form action="/request-demo/req2" method="get">
    <input type="text" name="username"><br>
    <input type="password" name="password"><br>
    <input type="checkbox" name="hobby" value="1"> 游泳
    <input type="checkbox" name="hobby" value="2"> 爬山 <br>
    <input type="submit">
 
</form>
</body>
</html>
```

![](<assets/1740015464502.png>)

2. 在 Servlet 代码中获取页面传递 GET 请求的参数值

2.1 获取 GET 方式的所有请求参数

```
/**
 * request 通用方式获取请求参数
 */
@WebServlet("/req2")
public class RequestDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //GET请求逻辑
        System.out.println("get....");
        //1. 获取所有参数的Map集合
        Map<String, String[]> map = req.getParameterMap();
        for (String key : map.keySet()) {
            // username:zhangsan lisi
            System.out.print(key+":");
 
            //获取值
            String[] values = map.get(key);
            for (String value : values) {
                System.out.print(value + " ");
            }
 
            System.out.println();
        }
    }
 
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req,resp);
    }
}
```

获取的结果为:

![](<assets/1740015464918.png>)

2.2 获取 GET 请求参数中的爱好，结果是数组值

```
/**
 * request 通用方式获取请求参数
 */
@WebServlet("/req2")
public class RequestDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //GET请求逻辑
        //...
        System.out.println("------------");
        String[] hobbies = req.getParameterValues("hobby");
        for (String hobby : hobbies) {
            System.out.println(hobby);
        }
    }
 
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    }
}
```

获取的结果为:

![](<assets/1740015465132.png>)

2.3 获取 GET 请求参数中的用户名和密码，结果是单个值

```
/**
 * request 通用方式获取请求参数
 */
@WebServlet("/req2")
public class RequestDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //GET请求逻辑
        //...
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        System.out.println(username);
        System.out.println(password);
    }
 
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    }
}
```

获取的结果为:

![](<assets/1740015465328.png>)

post 与 get 一样，在 Servlet 代码中获取页面传递 POST 请求的参数值；将 req.html 页面 form 表单的提交方式改成 post；将 doGet 方法中的内容**直接复制**到 doPost 方法中即可

**小结**

*   req.getParameter() 方法使用的频率会比较高
    
*   **以后**我们再写代码的时候，就**只需要按照如下格式来编写:**
    

```
public class RequestDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       //采用request提供的获取请求参数的通用方式来获取请求参数
       //编写其他的业务代码...
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req,resp);
    }
}
```

### 2.3 IDEA 使用模板创建 Servlet（继承 HttpServlet 版）

使用通用方式获取请求参数后，屏蔽了 GET 和 POST 的请求方式代码的不同，则代码可以定义如下格式:

![](<assets/1740015465679.png>)

由于格式固定，所以我们可以使用 IDEA 提供的模板来制作一个 Servlet 的模板，这样我们后期在创建 Servlet 的时候就会更高效，具体如何实现:

(1) 按照自己的需求，修改 Servlet 创建的模板内容。annotated 译为带注释的

![](<assets/1740015466073.png>)

 主要改动注解和加 this 使代码简洁：

```
@WebServlet("/${Entity_Name}")
public class ${Class_Name} extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
 
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request,response);
    }
}
```

（2）使用 servlet 模板创建 Servlet 类

![](<assets/1740015466298.png>)

**IDEA 中 new 没有 [servlet](https://so.csdn.net/so/search?q=servlet&spm=1001.2101.3001.7020 "servlet") 的选项解决方法：**

首先检查 Servlet 的依赖是否导入 pom.xml

```
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
    </dependency>
```

file - 项目结构（注意 main 文件夹下要有 java 和 resources 目录）：

![](<assets/1740015466578.png>)

### 2.4 请求参数中文乱码问题

#### 2.4.1 乱码原因和解决思路 

**getParameter 方法**

**post 乱码原因：**获取参数底层是获取请求体方法 getReader()，获取到的**参数编码为** I**SO-8859-1**。

**解决思路：设置请求编码为 utf-8**，或者将请求参数的字符串解码为 utf-8。

**get 乱码原因：**提交表单后，get 请求参数会进行 SO-8859-1 的 url 编码追加到地址栏链接后面，Tomcat7 获取到的**参数编码为** I**SO-8859-1 的 url 编码**，Tomcat8 获取到的参数编码为 **utf-8 的 URL 编码**。

**解决思路：**使用 Tomcat8、9、10，或者先 ISO-8859-1 的 url 解码成字符数组，再 utf-8 编码。

*   **POST 请求解决方案是:** 设置输入流的编码
    
    ```
    request.setCharacterEncoding("UTF-8");    //注意HTML页面head里设置编码也要是utf-8
    
    
    ```
    
*   **GET 请求解决方案：**使用 Tomcat8 及以上版本。
    
*   通用方式（GET/POST）：需要先解码，再编码
    
    ```
    new String(username.getBytes("ISO-8859-1"),"UTF-8");
    
    ```
    

**总结：Tomcat8 只 setCharacterEncoding 就可以解决所有编码中文问题。**

getQueryString 和 getReader 方法：

```
URLDecoder.decode(request.getQueryString(),"utf-8")

```

 **问题展示:**

(1) 将 req.html 页面的请求方式修改为 get

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form action="/request-demo/req2" method="get">
    <input type="text" name="username"><br>
    <input type="password" name="password"><br>
    <input type="checkbox" name="hobby" value="1"> 游泳
    <input type="checkbox" name="hobby" value="2"> 爬山 <br>
    <input type="submit">
 
</form>
</body>
</html>
```

(2) 在 Servlet 方法中获取参数，并打印

```
/**
 * 中文乱码问题解决方案
 */
@WebServlet("/req4")
public class RequestDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
       //1. 获取username
       String username = request.getParameter("username");
       System.out.println(username);
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

（3）启动服务器，页面上输入中文参数，提交后会发现控制台中文乱码：

![](<assets/1740015466807.png>)

#### 2.4.2 POST 请求 **getParameter** 解决方案 

*    设置输入流的编码
    
    ```
    request.setCharacterEncoding("UTF-8");
    注意:设置的字符集要和页面保持一致
    ```
    

*   分析出现中文乱码的**原因**：
    
    *   POST 的请求参数是通过 request 的 **getReader()** 来获取流中的数据
        
    *   在**获取流**的时候解码方式是 ISO-8859-1
        
    *   **ISO-8859-1** 编码是**不支持中文**的，所以会出现乱码
        
*   **解决方案：**
    
    *   页面设置的编码格式为 UTF-8
        
    *   把 TOMCAT 在获取流数据之前的编码设置为 UTF-8
        
    *   通过 request.setCharacterEncoding("UTF-8") 设置编码, UTF-8 也可以写成小写
        

**getParameter 代码:**

```
/**
 * 中文乱码问题解决方案
 */
@WebServlet("/req4")
public class RequestDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 解决乱码: POST getReader()
        //设置字符输入流的编码，设置的字符集要和页面保持一致
        request.setCharacterEncoding("UTF-8");    //Tomcat8可以注释这句
       //2. 获取username
       String username = request.getParameter("username");
       System.out.println(username);
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

**getReader 代码:** 

```
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
 
        BufferedReader br = request.getReader();
        String string=br.readLine();
        System.out.println(URLDecoder.decode(string,"utf-8"));
    }
```

至此 POST 请求中文乱码的问题就已经解决，但是这种方案**不适用于 GET 请求**，这个原因是什么呢，咱们下面再分析。

**如果还是中文乱码，检查 HTML，要设置编码为 utf-8，浏览器使用谷歌浏览器，要 ctrl+f5 清缓存刷新测试页面。**

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--
    action:form表单提交的请求地址
    method:请求方式，指定为post
-->
<form action="/untitled/servletDemo" method="post">
    <input type="text" name="username">
    <input type="password" name="password">
    <input type="submit">
</form>
</body>
</html>
```

#### **2.4.3 GET 请求 getParameter 解决方案**

在 Tomcat7 要 new String(username.getBytes("ISO-8859-1"),"UTF-8");**Tomcat8 直接用不会乱码**。

**`POST请求的中文乱码解决方案为什么不适用GET请求？`**

*   request.**setCharacterEncoding**("utf-8") 是设置 request 处理**流**的编码
    
*   **getQueryString** 方法并**没有**通过**流**的方式获取数据
    

所以 GET 请求不能用设置编码的方式来解决中文乱码问题。

**GET 请求中文参数乱码的原因**

*   浏览器把中文参数按照`UTF-8`进行 URL 编码（URL 编码示例：%E9%9D%9E。Chrome 地址栏会把 URL 编码转成 utf-8 方便阅读，i 传到服务器的还是 URL 编码）
    
*   Tomcat 对获取到的内容进行了`ISO-8859-1`的 URL 解码
    
*   在控制台就会出现类上`å¼ ä¸`的乱码，最后一位是个空格
    

**思考:**

如果把`req.html`页面的`<meta>`标签的 charset 属性改成`ISO-8859-1`, 后台不做操作，能解决中文乱码问题么?

答案是否定的，因为**`ISO-8859-1`本身是不支持中文展示**的，所以改了 <meta> 标签的 charset 属性后，会导致页面上的中文内容都无法正常展示。

#### **2.4.4 URL 编码解码**

这块知识了解即可。

具体编码过程分两步，分别是:

(1) 将字符串按照**编码**方式转为**二进制**

(2) 每个字节转为 2 个 16 进制数并在前边加上 %

`例如，张三`按照 UTF-8 的方式转换成二进制的结果为:

```
1110 0101 1011 1100 1010 0000 1110 0100 1011 1000 1000 1001

```

**计算 utf-8 字符串二进制方法**

使用`http://www.mytju.com/classcode/tools/encode_utf8.asp`，输入`张三`

![](<assets/1740015466982.png>)

就可以获取张和三分别对应的 16 进制。在计算的十六进制结果中，每两位前面加一个 %, 就可以获取到`%E5%BC%A0%E4%B8%89`。

但是对于上面的计算过程，如果没有工具，纯手工计算的话，相对来说还是比较复杂的，我们也不需要进行手动计算，在 Java 中已经为我们提供了编码和解码的 API 工具类可以让我们更快速的进行编码和解码:

Java 中 url **编码和解码的 API 工具类**

**编码:**

```
java.net.URLEncoder.encode("需要被编码的内容","字符集(UTF-8)")

```

**解码:**

```
java.net.URLDecoder.decode("需要被解码的内容","字符集(UTF-8)")

```

接下来咱们对`张三`来进行编码和解码

```
import java.io.UnsupportedEncodingException;
import java.net.URLDecoder;
import java.net.URLEncoder;
public class URLDemo {
 
  public static void main(String[] args) throws UnsupportedEncodingException {
        String username = "张三";
        //1. URL编码
        String encode = URLEncoder.encode(username, "utf-8");
        System.out.println(encode); //打印:%E5%BC%A0%E4%B8%89
 
       //2. URL解码
       //String decode = URLDecoder.decode(encode, "utf-8");//打印:张三
       String decode = URLDecoder.decode(encode, "ISO-8859-1");//打印:`å¼ ä¸ `
       System.out.println(decode);
    }
}
```

**分析解决乱码原理：**

*   在进行编码和解码的时候，不管使用的是哪个字符集，他们对应的`%E5%BC%A0%E4%B8%89`是一致的
    
*   那他们对应的二进制值也是一样的，为:
    
    *   ```
        1110 0101 1011 1100 1010 0000 1110 0100 1011 1000 1000 1001
        
        ```
        
*   所以我们可以考虑**把`å¼ ä¸`转换成字节，在把字节转换成`张三`**，使转换的过程中是它们的编码一致，就可以解决中文乱码问题。
    

**具体的实现步骤为:**

1. 按照 ISO-8859-1 编码获取乱码`å¼ ä¸`对应的字节数组

2. 按照 UTF-8 编码获取字节数组对应的字符串

**解决代码**

```
/**
 * 中文乱码问题解决方案
 */
@WebServlet("/req4")
public class RequestDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 解决乱码：POST，getReader()
        //request.setCharacterEncoding("UTF-8");//设置字符输入流的编码
 
 
        //3. GET,获取参数的方式：getQueryString
        // 乱码原因：tomcat进行URL解码，默认的字符集ISO-8859-1
       /* //3.1 先对乱码数据进行编码：转为字节数组
        byte[] bytes = username.getBytes(StandardCharsets.ISO_8859_1);
        //3.2 字节数组解码
        username = new String(bytes, StandardCharsets.UTF_8);*/
 
        username  = new String(username.getBytes(StandardCharsets.ISO_8859_1),StandardCharsets.UTF_8);
 
        System.out.println("解决乱码后："+username);
 
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

**注意**

把`request.setCharacterEncoding("UTF-8")`代码注释掉后，会发现 GET 请求参数乱码解决方案同时也可也把 POST 请求参数乱码的问题也解决了。

只不过对于 POST 请求参数一般都会比较多，采用这种方式解决乱码起来比较麻烦，所以对于 POST 请求还是建议使用设置编码的方式进行。

另外需要说明一点的是 **Tomcat8.0 之后，已将 GET 请求乱码问题解决，设置默认的解码方式为 UTF-8**

**小结**

**中文乱码解决方案**

POST 请求和 GET 请求的参数中如果有中文，后台接收数据就会出现中文乱码问题

GET 请求在 Tomcat8.0 以后的版本就不会出现了

POST 请求解决方案是: 设置输入流的编码

```
request.setCharacterEncoding("UTF-8");
注意:设置的字符集要和页面保持一致
```

所有 Tomcat 版本通用方式（GET/POST）：需要先解码，再编码

```
new String(username.getBytes("ISO-8859-1"),"UTF-8");

```

**URL 编码实现方式:**

*   编码:
    
    ```
    URLEncoder.encode(str,"UTF-8");
    
    ```
    
*   解码:
    
    ```
    URLDecoder.decode(s,"ISO-8859-1");
    
    ``` 

### 2.5 Request 请求转发

**请求转发 (forward):** 一种在**服务器内部**的资源跳转方式。

![](<assets/1740015467157.png>)

![](<assets/1740015467447.png>)

(1) 浏览器发送请求给服务器，服务器中对应的资源 A 接收到请求

(2) 资源 A 处理完请求后将请求**发给**资源 B

(3) 资源 B 处理完后将结果响应给浏览器，**继续执行 A 剩下代码**

(4) 请求从资源 A 到资源 B 的过程就叫**请求转发**

**请求转发的实现方式:**

```
req.getRequestDispatcher("资源B路径").forward(req,resp);

```

**请求转发的特点：**

*   **浏览器地址栏路径不发生变化**
    
    虽然后台从`/req5`转发到`/req6`, 但是浏览器的地址一直是`/req5`, 未发生变化
    
*   **只能转发到当前服务器的内部资源。**不能从一个服务器通过转发访问另一台服务器
    
*   **一次请求**，可以在转发资源间使用 **request 共享数据**
    
    虽然后台从`/req5`转发到`/req6`，但是这个只有一次请求
    

**案例：** 

具体如何来使用，我们先来看下需求:

![](<assets/1740015467695.png>)

针对上述需求，具体的实现步骤为:

(1) 创建 RequestDemo5 类

```
/**
 * 请求转发
 */
@WebServlet("/req5")
public class RequestDemo5 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("demo5...");
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

(2) 创建 RequestDemo6 类

```
/**
 * 请求转发
 */
@WebServlet("/req6")
public class RequestDemo6 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("demo6...");
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

(3) 在 RequestDemo5 的 doGet 方法中进行请求转发

```
/**
 * 请求转发
 */
@WebServlet("/req5")
public class RequestDemo5 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("demo5...");
        //请求转发
        request.getRequestDispatcher("/req6").forward(request,response);
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

(4) 启动测试

访问`http://localhost:8080/request-demo/req5`, 就可以在控制台看到如下内容:

![](<assets/1740015468283.png>)

说明请求已经转发到了`/req6`

**请求转发资源间共享数据: 使用 Request 对象**

此处主要解决的问题是把请求从`/req5`转发到`/req6`的时候，如何传递数据给`/req6`。

需要使用 **request 对象**提供的三个**方法**:

*   存储数据到 request 域 [范围, 数据是存储在 request 对象] 中
    

```
void setAttribute(String name,Object o);

```

*   根据 key 获取值
    

```
Object getAttribute(String name);

```

*   根据 key 删除该键值对
    

```
void removeAttribute(String name);

```

**代码示例：**

![](<assets/1740015468497.png>)

1. 在 RequestDemo5 的 doGet 方法中转发请求之前，将数据存入 request 域对象中

2. 在 RequestDemo6 的 doGet 方法从 request 域对象中获取数据，并将数据打印到控制台

3. 启动访问测试

(1) 修改 RequestDemo5 中的方法

```
@WebServlet("/req5")
public class RequestDemo5 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("demo5...");
        //存储数据
        request.setAttribute("msg","hello");
        //请求转发
        request.getRequestDispatcher("/req6").forward(request,response);
 
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

(2) 修改 RequestDemo6 中的方法

```
/**
 * 请求转发
 */
@WebServlet("/req6")
public class RequestDemo6 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("demo6...");
        //获取数据
        Object msg = request.getAttribute("msg");
        System.out.println(msg);
 
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

(3) 启动测试

访问`http://localhost:8080/request-demo/req5`, 就可以在控制台看到如下内容:

![](<assets/1740015469084.png>)

此时就可以实现在转发多个资源之间共享数据。

## 三、Response 对象

*   **Request:** 使用 request 对象来**获取请求数据**
    
*   **Response:** 使用 response 对象来**设置响应数据**
    

**Reponse 的继承体系:**

![](<assets/1740015469298.png>)

### 3.1 Response 设置响应数据

HTTP 响应数据总共分为三部分内容，分别是**响应行、响应头、响应体**，对于这三部分内容的数据，respone 对象都提供了哪些方法来进行设置?

**响应行**

![](<assets/1740015469697.png>)

常用的就是设置响应状态码:

```
void setStatus(int sc);

```

**响应头**

![](<assets/1740015469919.png>)

设置响应头键值对：

```
void setHeader(String name,String value);

```

**响应体**

![](<assets/1740015470096.png>)

对于响应体，是通过字符、字节**输出流**的方式往浏览器写，

获取字符输出流:

```
PrintWriter getWriter();

```

获取字节输出流

```
ServletOutputStream getOutputStream();

```

### 3.2 Respones 请求重定向

#### 3.2.1 重定向

**Response 重定向 (redirect): 一种资源跳转方式。**

重定向只是向 response 中添加了一条重定向地址，并不会立即响应给浏览器，只有当 doFilter() 或者 **service() 执行完毕**才会将响应信息封装起来发送给浏览器。

**省流：浏览器请求资源 A，A 处理不了说 B 能处理，浏览器再请求资源 B。**

![](<assets/1740015470365.png>)

![](<assets/1740015470603.png>)

(1) 浏览器发送请求给服务器，服务器中对应的资源 A 接收到请求

(2) 资源 A 现在无法处理该请求，就会先将 A 中代码运行完毕，再给浏览器响应一个 **302 的状态码 + location** 的一个访问资源 B 的路径

(3) **浏览器（对比，请求转发是直接资源 a 到资源 b）**接收到响应状态码为 302 就会重新**发送请求**到 location 对应的访问地址去**访问资源 B**

(4) 资源 B 接收到请求后进行处理并最终给浏览器响应结果，这整个过程就叫**重定向**

**状态码分类**    说明  
1xx    响应中——临时状态码，表示请求已经接受，告诉客户端应该继续请求或者如果它已经完成则忽略它  
2xx    成功——表示请求已经被成功接收，处理已完成  
3xx    重定向——重定向到其它地方：它让客户端再发起一个请求以完成整个处理。  
4xx    客户端错误——处理发生错误，责任在客户端，如：客户端的请求一个不存在的资源，客户端未被授权，禁止访问等  
5xx    服务器端错误——处理发生错误，责任在服务端，如：服务端抛出异常，路由出错，HTTP 版本不支持等

**重定向的实现方式一:**

```
resp.setStatus(302);
resp.setHeader("location","资源B的访问路径");
```

**重定向的实现方式二：推荐**

```
        resposne.sendRedirect("/request-demo/resp2")


```

```
//动态获取项目虚拟目录更好
        String contextPath = request.getContextPath();
        response.sendRedirect(contextPath+"/resp2");
```

**案例：**

![](<assets/1740015470800.png>)

针对上述需求，具体的实现步骤为:

1. 创建一个 ResponseDemo1 类，接收 / resp1 的请求，在 doGet 方法中打印`resp1....`

2. 创建一个 ResponseDemo2 类，接收 / resp2 的请求，在 doGet 方法中打印`resp2....`

3. 在 ResponseDemo1 的方法中使用

response.setStatus(302);

response.setHeader("Location","/request-demo/resp2") 来给前端响应结果数据

4. 启动测试

(1) 创建 ResponseDemo1 类

```
@WebServlet("/resp1")
public class ResponseDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("resp1....");
        //重定向
        //1.设置响应状态码 302
        response.setStatus(302);
        //2. 设置响应头 Location
        response.setHeader("Location","/request-demo/resp2");
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

(2) 创建 ResponseDemo2 类

```
@WebServlet("/resp2")
public class ResponseDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("resp2....");
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

(3) 启动测试

访问`http://localhost:8080/request-demo/resp1`, 就可以在控制台看到如下内容:

![](<assets/1740015471085.png>)

说明`/resp1`和`/resp2`都被访问到了。到这重定向就已经完成了。

**简化的重定向编写方式**为:

```
resposne.sendRedirect("/request-demo/resp2")

```

所以第 1 步中的代码就可以简化为：

```
@WebServlet("/resp1")
public class ResponseDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("resp1....");
        //重定向
        resposne.sendRedirect("/request-demo/resp2")；
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

**重定向的特点：（与请求转发正好相反）**

*   浏览器**地址栏**路径发送**变化**
    
    当进行重定向访问的时候，由于是由浏览器发送的两次请求，所以地址会发生变化
    
*   可以重定向到任何位置的资源 (服务**内容**、**外部**均可)
    
    因为第一次响应结果中包含了浏览器下次要跳转的路径，所以这个路径是可以任意位置资源。
    
*   **两次请求**，**不能**在多个资源使用 **request 共享**数据
    
    因为浏览器发送了两次请求，是两个不同的 request 对象，就无法通过 request 对象进行共享数据
    
*   **重定向刷新不会重新提交表单，效率低，适用于不同业务间。**
    

介绍完**请求重定向**和**请求转发**以后，接下来需要把这两个放在一块对比下:

![](<assets/1740015471298.png>)

以后到底用哪个，还是需要根据具体的业务来决定。

#### 3.2.2 对比转发和重定向，区别和应用场景

**区别** 

![](<assets/1740015471504.png>)

**1. 跳转的范围不同**  
由于转发是在服务器内部进行的，跳转的范围被限制在当前服务器的当前应用。

而重定向由浏览器发送 URL 请求完成，范围是所有服务器的所有应用

**2.“/” 标识符代表的含义不同**  
由于转发是在服务器内部进行的，在指定转发的 URL 中，“/” 代表当前应用

而重定向的 “/” 则代表当前服务器。

**3. 刷新后是否会重新提交表单 / 参数**  
转发：对于转发而言，通过 Request 对象携带的**参数会继续存在**，若刷新当前页面，则会导致已经提交的表单**再次提交**，对于主流浏览器而言，此时会弹出**是否重复提交表单**的确认弹窗。

重定向：而重定向**不会携带**这些数据，刷新时安全的。

**6. 效率不同**  
对于**转发**而言，过程发生在服务器内部，不经过重定向的二次网络过程和请求，因此**效率更高**

**7. 携带参数的方式不同**  
对于转发而言，携带参数跳转的方式是通过方法 request.setParameter 来绑定。

而对于重定向，一种可行的办法是用? key=value&key2=value2 的方式在 URL 后面增加显性参数。

**应用场景：**

**重定向刷新不会重新提交表单，效率低，适用于不同业务间。**  
建议一：

对于**竭力避免表单重复提交**的场景下选择**重定向**方式，防止不断刷新页面引起的重复提交表单。

建议二：

对于效率要求更高的场景选择服务器内转发

建议三：

若**携带较多参数**则选择服务器内**转发**，一般是同一业务内需要携带参数多。

请求转发偏向于、**为同一业务功能服务**  
重定向偏向于、**不同业务功能之间的跳转**

### 3.3 是否加虚拟目录的依据，动态配置虚拟目录

**问题 1：**

转发的时候路径上没有加虚拟目录 (项目访问路径)`/request-demo`而重定向加了虚拟目录。

那么到底**什么时候需要加，什么时候不需要加呢?**

![](<assets/1740015471595.png>)

**答案：是否加虚拟目录 (项目访问路径) 的依据**

*   **浏览器使用: 需要加虚拟目录 (项目访问路径)**
    
*   **服务端使用: 不需要加虚拟目录**
    

转发是只能在同一服务器内跳转，虚拟目录是不变的，不加编译器能猜出来；重定向可以是不同服务器跳转，就需要指定虚拟目录。 

对于转发来说，因为是在服务端直接从资源 a 发送到 b，所以不需要加虚拟目录

对于重定向来说，路径最终是由浏览器来发送请求到 location 地址去访问资源 B，就需要添加虚拟目录。

**练习：下面是否需要加虚拟目录**

*   `<a href='路劲'>`
    
*   `<form action='路径'>`
    
*   req.getRequestDispatcher("路径")
    
*   resp.sendRedirect("路径")
    

**答案:**

```
1.超链接，从浏览器发送，需要加
2.表单，从浏览器发送，需要加
3.转发，是从服务器内部跳转，不需要加
4.重定向，是由浏览器进行跳转，需要加。
```

目前学的也就转发不加虚拟目录。 

**问题 2：**

**如何动态配置项目虚拟目录。**

防止后期通过 Tomcat 插件配置了项目的访问路径，那么所有需要重定向的地方都需要重新修改

![](<assets/1740015471813.png>)

答案也比较简单，我们可以在代码中**动态去获取项目访问的虚拟目录**，使用 request 对象中的请求行获取虚拟目录方法 **getContextPath**()，修改后的代码如下:

```
@WebServlet("/resp1")
public class ResponseDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("resp1....");
 
        //简化方式完成重定向
        //动态获取虚拟目录
        String contextPath = request.getContextPath();
        response.sendRedirect(contextPath+"/resp2");    //使用指定的重定向位置URL向客户端发送一个临时重定向响应，并清除缓冲区。
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

重新启动访问测试，功能依然能够实现，此时就可以动态获取项目访问的虚拟路径，从而降低代码的耦合度。

### 3.4 Response 响应字符数据

了解即可，实际应用一般是把 request 转发到前端页面 JSP 中展示，每行用 write 写太复杂，并且前后端不分离。

**响应字符数据到浏览器**

*   先设置内容类型 contentType 和编码
    
    ```
    response.setContentType("text/html;charset=utf-8");  
    
    ```
    
*   通过 Response 对象获取字符输出流： PrintWriter writer = resp.**getWriter**();
    
*   通过字符输出流写数据: writer.write("aaa");
    

 PrintWriter 是字符打印流

**示例：**

1. 返回一个简单的字符串`aaa`

```
/**
 * 响应字符数据：设置字符数据的响应体
 */
@WebServlet("/resp3")
public class ResponseDemo3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //必须设置编码，否则中文会乱码
        response.setContentType("text/html;charset=utf-8");    
        //1. 获取字符输出流
        PrintWriter writer = response.getWriter();
		 writer.write("aaa");
    }
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

![](<assets/1740015472262.png>)

**返回一串 html 字符串**，并且能被浏览器解析

```
PrintWriter writer = response.getWriter();
//content-type，告诉浏览器返回的数据类型是HTML类型数据，这样浏览器才会解析HTML标签
//设置字符集防止乱码中文
response.setHeader("content-type","text/html;charset=utf-8");
//response.setContentType("text/html;charset=utf-8");//或者直接设置content-type
writer.write("<h1>aaa</h1>");
```

![](<assets/1740015472940.png>)

**注意:** 一次请求响应结束后，**response** 对象就会被**销毁**掉，所以不**要手动关闭流**。

2. 返回一个中文的字符串`你好`，需要注意设置响应数据的编码为`utf-8`

```
//设置响应的数据格式及数据的编码
response.setContentType("text/html;charset=utf-8");
writer.write("你好");
```

![](<assets/1740015473281.png>)

### 3.5 Response 响应字节数据

**响应字节数据到浏览器**

*   通过 Response 对象获取字节输出流：ServletOutputStream outputStream = resp.**getOutputStream**();
    
*   通过字节输出流写数据: outputStream.**write**(字节数据);
    

**示例：**

**返回一个图片文件到浏览器**

```
/**
 * 响应字节数据：设置字节数据的响应体
 */
@WebServlet("/resp4")
public class ResponseDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 读取文件
        FileInputStream fis = new FileInputStream("d://a.jpg");
        //2. 获取response字节输出流
        ServletOutputStream os = response.getOutputStream();
        //3.1. 方法一，完成流的copy
        byte[] buff = new byte[1024];
        int len = 0;
        while ((len = fis.read(buff))!= -1){
            os.write(buff,0,len);
        }
        //3.2.  方法二，工具类快速流的复制，需要pom导坐标commons-io
        //IOUtils.copy(fis,os);
        fis.close();
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

![](<assets/1740015473533.png>)

**使用 IOUtils.copy(fis,os) 进行流的复制（推荐）**

(1)pom.xml 添加依赖

```
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>
```

(2) 调用工具类方法实现复制

```
//fis:输入流
//os:输出流
IOUtils.copy(fis,os);
```

优化后的代码:

```
/**
 * 响应字节数据：设置字节数据的响应体
 */
@WebServlet("/resp4")
public class ResponseDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 读取文件
        FileInputStream fis = new FileInputStream("d://a.jpg");
        //2. 获取response字节输出流
        ServletOutputStream os = response.getOutputStream();
        //3. 完成流的copy
      	IOUtils.copy(fis,os);
        fis.close();
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

## 四、Request、Response、Mybatis、前端注册登录小项目

### 4.1 代码

**需求：**

![](<assets/1740015473803.png>)

![](<assets/1740015474046.png>)

**项目结构**

![](<assets/1740015474302.png>)

 **数据库准备**

```
-- 创建用户表，在db1数据库下创建
CREATE TABLE tb_user(
	id int primary key auto_increment,
	username varchar(20) unique,
	password varchar(32)
);
 
-- 添加数据
INSERT INTO tb_user(username,password) values('zhangsan','123'),('lisi','234');
 
SELECT * FROM tb_user;
 
 
```

 **代码**

[Request、Response、Mybatis、前端注册登录小项目 - Java 文档类资源 - CSDN 下载](https://download.csdn.net/download/qq_40991313/86267342 "Request、Response、Mybatis、前端注册登录小项目-Java文档类资源-CSDN下载")

### 4.2 SqlSessionFactory 工具类抽取

```
String resource = "mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new 
	SqlSessionFactoryBuilder().build(inputStream);
```

注册登录 Servlet.java 在使用 Mybatis 时候中都用到了上面代码，这些重复代码就会造成一些**问题:**

*   重复代码不利于后期的维护
    
*   **SqlSessionFactory 工厂类进行重复创建**
    
    *   就相当于每次买手机都需要重新创建一个手机生产工厂来给你制造一个手机一样，**资源消耗非常大但性能却非常低**。所以这么做是不允许的。
        

**解决方案**

*   代码重复可以**抽取工具类**
    
*   对指定代码只需要执行一次可以使用**静态代码块** 
    

**注意：**

```
SqlSession sqlSession = sqlSessionFactory.openSession();

```

 虽然上面语句也重复，但不能抽取到工具类里。因为 SqlSession 是一个连接、**会话**，每次连接数据库时候创建一次会话是合适，如果所有连接都共用一个会话会互相影响。

SqlSession 是一个**会话**，相当于 JDBC 中的一个 Connection 对象，Mybatis 中所有的数据库交互都由 SqlSession 来完成。

**代码示例：**

**抽取工具类**

```
public class SqlSessionFactoryUtils {
 
    private static SqlSessionFactory sqlSessionFactory;
 
    static {
        //静态代码块会随着类的加载而自动执行，且只执行一次
        //静态代码块不能抛异常，要用try-catch
        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
 
 
    public static SqlSessionFactory getSqlSessionFactory(){
        return sqlSessionFactory;
    }
}
```

**使用工具类**

```
SqlSessionFactory sqlSessionFactory =SqlSessionFactoryUtils.getSqlSessionFactory();

```

## 五、JSP

### 5.1 JSP 概述

**了解即可**，现在基本都不用 jsp 了，新技术都是用 vue、axios，前后端分离。jsp 算旧技术，但有些项目是用 jsp 开发的，所以还是学一下。

**JSP（全称：Java Server Pages）：Java 服务端页面。**

是一种**动态的网页技术**，其中既可以定义 HTML、JS、CSS 等静态内容，还可以定义 Java 代码的动态内容，也就是 `JSP = HTML + Java`。

JSP 和 JavaScript 是两码事，JSP 是 java 服务端页面，JavaScript 是脚本语言的一种。 

**jsp 代码示例：**

```
<html>
    <head>
        <title>Title</title>
    </head>
    <body>
        <h1>JSP,Hello World</h1>
        <%
        	System.out.println("hello,jsp~");
        %>
    </body>
</html>
```

上面代码 `h1` 标签内容是展示在页面上，而 Java 的输出语句是**输出在 idea 的控制台**。

**JSP 作用：**

简化开发，避免了在 Servlet 中直接输出 HTML 标签，前后端分离。

如果只用 `servlet`，实现页面动态显示用户名：

![](<assets/1740015474567.png>)

![](<assets/1740015474805.png>)

上面的代码有大量使用到 `writer` 对象向页面写标签内容，这样我们的代码就显得很麻烦；将来如果展示的效果出现了问题，排错也显得有点力不从心。

**用 JSP** 实现页面动态显示用户名：

![](<assets/1740015475026.png>)

上面代码可以看到里面基本都是 `HTML` 标签，而动态数据使用 Java 代码进行展示；这样操作看起来要比用 `servlet` 实现要舒服很多。

JSP 作用：简化开发，避免了在 Servlet 中直接输出 HTML 标签。

### 5.2 helloworld

接下来我们做一个简单的快速入门代码。

![](<assets/1740015475250.png>)

**创建 maven 的 web 项目**

![](<assets/1740015475507.png>)

**导入 JSP 依赖**

```
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>jsp-api</artifactId>
    <version>2.2</version>
    <scope>provided</scope>
</dependency>
```

`pom.xml` 文件内容如下：导入 Servlet 和 JSP 依赖，Tomcat 插件

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
 
    <groupId>org.example</groupId>
    <artifactId>jsp-demo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>
 
    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
 
    <dependencies>
      <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
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
</project>
```

该依赖的 `scope` 必须设置为 `provided`，因为 tomcat 中有这个 jar 包了，所以在打包时我们是不希望将该依赖打进到我们工程的 war 包中。

**创建 jsp 页面**

在项目的 `webapp` 下创建 jsp 页面

![](<assets/1740015475764.png>)

通过上面方式创建一个名为 `hello.jsp` 的页面。

**编写代码**

在 `hello.jsp` 页面中书写 `HTML` 标签和 `Java` 代码，如下

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h1>hello jsp</h1>
 
    <%
        System.out.println("hello,jsp~");
    %>
</body>
</html>
```

**测试**

启动服务器并在浏览器地址栏输入 `http://localhost:8080/jsp-demo/hello.jsp`，我们可以在页面上看到如下内容：

![](<assets/1740015476066.png>)

 **注意：**Tomcat 版本和 jdk 版本不能差太大，例如 jdk18、Tomcat8.5 运行 jsp 就会报错 HTTP 500-Unable to compile class for JSP，jdk18 和 Tomcat10 会 Servlet 页面打不开 400，**jdk18 和 Tomcat9 正常。**

同时也可以看到在 `idea` 的控制台看到输出的 `hello,jsp~` 内容。

### 5.3 JSP 原理

**JSP 本质上就是一个 Servlet，**所以可以写 `Java` 代码**。**

**访问 jsp 时的流程**

![](<assets/1740015476302.png>)

 最终对外提供服务的就是 jsp 转换、编译成的字节码文件。

1.  浏览器第一次访问 `hello.jsp` 页面
    
2.  `tomcat` 会将 `hello.jsp` 转换为名为 **`hello_jsp.java`** 的一个 `Servlet`
    
3.  `tomcat` 再将转换的 `servlet` 编译成字节码文件 **`hello_jsp.class`**
    
4.  `tomcat` 会执行该字节码文件，向外提供服务
    

**验证 jsp 文件转成 Servlet 类** 

我们可以到项目所在磁盘目录下找 `target\tomcat\work\Tomcat\localhost\jsp-demo\org\apache\jsp` 目录，而这个目录下就能看到转换后的 `servlet`

![](<assets/1740015476482.png>)

打开 **`hello_jsp.java`** 文件，来查看里面的代码：

![](<assets/1740015476692.png>)

可以看到这个由 jsp 转成的 **Servlet 类继承**了名为 **`HttpJspBase`** 这个类，而通过 Tomcat 源码可以发现**`HttpJspBase`** 继承了 `HttpServlet` ：

![](<assets/1740015476907.png>)

那么 `hello_jsp` 这个由 jsp 转换的类就间接的继承了 `HttpServlet` ，也就说明 **`hello_jsp` 是一个 `servlet`**。

`hello_jsp` 类有一个名为 **`_jspService()`** 的方法，该方法就是每次访问 `jsp` 时自动执行的方法，和 `servlet` 中的 `service` 方法一样 。

而在 `_jspService()` 方法中可以看到往浏览器写标签的代码：

![](<assets/1740015477127.png>)

以前我们自己写 `servlet` 时，这部分代码是由我们自己来写，现在有了 `jsp` 后，由 tomcat 完成这部分功能。

### 5.4 JSP 脚本

JSP 脚本用于在 JSP 页面内定义 Java 代码。

#### 5.4.1 JSP 脚本分类

JSP 脚本有如下三个分类：

*   **<%...%>            ：**内容会直接放到_jspService() 方法之中，中间内容在服务器运行
    
*   **<%="…"%>         ：**内容会放到 out.print() 中，作为 out.print() 的参数，中间内容会打印在网页
    
*   **<%!…%>           ：**内容会放到_jspService() 方法之外，被类直接包含，中间定义成员变量、方法
    

`jsp转成的Servlet类中的`**`_jspService()` 方法**就是每次访问 `jsp` 时自动执行的方法，和 `servlet` 中的 `service` 方法一样 。 

**代码演示：**

**<%...%>** 

在 `hello.jsp` 中书写

```
<%
    System.out.println("hello,jsp~");
    int i = 3;
%>
```

通过浏览器访问 `hello.jsp` 后，查看转换的 `hello_jsp.java` 文件，i 变量定义在了 `_jspService()` 方法中

![](<assets/1740015477460.png>)

 **<%=…%>**

在 `hello.jsp` 中书写

```
<%="hello"%>
<%=i%>
```

通过浏览器访问 `hello.jsp` 后，查看转换的 `hello_jsp.java` 文件，该脚本的内容作为参数放在了 `out.print()` 中：

![](<assets/1740015477647.png>)

 **<%!…%>**

在 `hello.jsp` 中书写

```
<%!
    void  show(){}
	String name = "zhangsan";
%>
```

通过浏览器访问 `hello.jsp` 后，查看转换的 `hello_jsp.java` 文件，该脚本的内容被放在了成员位置

![](<assets/1740015477865.png>)

#### 5.4.2 案例, 使用 JSP 脚本展示品牌数据

**需求**

使用 JSP 脚本展示品牌数据

![](<assets/1740015478071.png>)

**素材**

静态的 jsp

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
 
<%
    // 查询数据库
    List<Brand> brands = new ArrayList<Brand>();
    brands.add(new Brand(1,"三只松鼠","三只松鼠",100,"三只松鼠，好吃不上火",1));
    brands.add(new Brand(2,"优衣库","优衣库",200,"优衣库，服适人生",0));
    brands.add(new Brand(3,"小米","小米科技有限公司",1000,"为发烧而生",1));
 
%>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<input type="button" value="新增"><br>
<hr>
<table border="1" cellspacing="0" width="800">
    <tr>
        <th>序号</th>
        <th>品牌名称</th>
        <th>企业名称</th>
        <th>排序</th>
        <th>品牌介绍</th>
        <th>状态</th>
        <th>操作</th>
 
    </tr>
    <tr align="center">
        <td>1</td>
        <td>三只松鼠</td>
        <td>三只松鼠</td>
        <td>100</td>
        <td>三只松鼠，好吃不上火</td>
        <td>启用</td>
        <td><a href="#">修改</a> <a href="#">删除</a></td>
    </tr>
 
    <tr align="center">
        <td>2</td>
        <td>优衣库</td>
        <td>优衣库</td>
        <td>10</td>
        <td>优衣库，服适人生</td>
        <td>禁用</td>
 
        <td><a href="#">修改</a> <a href="#">删除</a></td>
    </tr>
 
    <tr align="center">
        <td>3</td>
        <td>小米</td>
        <td>小米科技有限公司</td>
        <td>1000</td>
        <td>为发烧而生</td>
        <td>启用</td>
 
        <td><a href="#">修改</a> <a href="#">删除</a></td>
    </tr>
 
 
</table>
 
</body>
</html>
```

**实体类 Brand.java**

```
package com.itheima.pojo;
 
/**
 * 品牌实体类
 */
 
public class Brand {
    // id 主键
    private Integer id;
    // 品牌名称
    private String brandName;
    // 企业名称
    private String companyName;
    // 排序字段
    private Integer ordered;
    // 描述信息
    private String description;
    // 状态：0：禁用  1：启用
    private Integer status;
 
 
    public Brand() {
    }
 
    public Brand(Integer id, String brandName, String companyName, String description) {
        this.id = id;
        this.brandName = brandName;
        this.companyName = companyName;
        this.description = description;
    }
 
    public Brand(Integer id, String brandName, String companyName, Integer ordered, String description, Integer status) {
        this.id = id;
        this.brandName = brandName;
        this.companyName = companyName;
        this.ordered = ordered;
        this.description = description;
        this.status = status;
    }
 
    public Integer getId() {
        return id;
    }
 
    public void setId(Integer id) {
        this.id = id;
    }
 
    public String getBrandName() {
        return brandName;
    }
 
    public void setBrandName(String brandName) {
        this.brandName = brandName;
    }
 
    public String getCompanyName() {
        return companyName;
    }
 
    public void setCompanyName(String companyName) {
        this.companyName = companyName;
    }
 
    public Integer getOrdered() {
        return ordered;
    }
 
    public void setOrdered(Integer ordered) {
        this.ordered = ordered;
    }
 
    public String getDescription() {
        return description;
    }
 
    public void setDescription(String description) {
        this.description = description;
    }
 
    public Integer getStatus() {
        return status;
    }
 
    public void setStatus(Integer status) {
        this.status = status;
    }
 
    @Override
    public String toString() {
        return "Brand{" +
                "id=" + id +
                ", brandName='" + brandName + '\'' +
                ", companyName='" + companyName + '\'' +
                ", ordered=" + ordered +
                ", description='" + description + '\'' +
                ", status=" + status +
                '}';
    }
}
```

**将 `brand.jsp` 页面改为动态的**

```
for循环后面可以用JSP标准标签库JSTL的foreach标签优化


``` 

```
<%@ page import="com.itheima.pojo.Brand" %>
<%@ page import="java.util.List" %>
<%@ page import="java.util.ArrayList" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
 
<%
    // 查询数据库
    List<Brand> brands = new ArrayList<Brand>();
    brands.add(new Brand(1,"三只松鼠","三只松鼠",100,"三只松鼠，好吃不上火",1));
    brands.add(new Brand(2,"优衣库","优衣库",200,"优衣库，服适人生",0));
    brands.add(new Brand(3,"小米","小米科技有限公司",1000,"为发烧而生",1));
 
%>
 
 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<input type="button" value="新增"><br>
<hr>
<table border="1" cellspacing="0" width="800">
    <tr>
        <th>序号</th>
        <th>品牌名称</th>
        <th>企业名称</th>
        <th>排序</th>
        <th>品牌介绍</th>
        <th>状态</th>
        <th>操作</th>
    </tr>
    <%
        for (int i = 0; i < brands.size(); i++) {
            Brand brand = brands.get(i);
    %>
    <tr align="center">
        <td><%=brand.getId()%></td>
        <td><%=brand.getBrandName()%></td>
        <td><%=brand.getCompanyName()%></td>
        <td><%=brand.getOrdered()%></td>
        <td><%=brand.getDescription()%></td>
		<td><%=brand.getStatus() == 1 ? "启用":"禁用"%></td>
        <td><a href="#">修改</a> <a href="#">删除</a></td>
    </tr>
    <%
        }
    %>
</table>
</body>
</html>
```

**测试**

在浏览器地址栏输入 `http://localhost:8080/jsp-demo/brand.jsp` ，页面展示效果如下

![](<assets/1740015478300.png>)

#### 5.4.3 JSP 缺点

通过上面的案例，我们可以看到 JSP 的很多缺点。

由于 JSP 页面内，既可以定义 HTML 标签，又可以定义 Java 代码，造成了以下问题：

难写难读难维护。

*   **书写麻烦**：特别是复杂的页面
    
    既要写 HTML 标签，还要写 Java 代码
    
*   阅读麻烦
    
    上面案例的代码，相信你后期再看这段代码时还需要花费很长的时间去梳理
    
*   复杂度高：运行需要依赖于各种环境，JRE，JSP 容器，JavaEE…
    
*   占内存和磁盘：JSP 会自动生成. java 和. class 文件占磁盘，运行的是. class 文件占内存
    
*   **调试困难**：出错后，需要找到自动生成的. java 文件进行调试
    
*   不利于团队协作：前端人员不会 Java，后端人员不精 HTML
    
    如果页面布局发生变化，前端工程师对静态页面进行修改，然后再交给后端工程师，由后端工程师再将该页面改为 JSP 页面
    

由于上述的问题， **JSP 已逐渐退出历史舞台，**以后开发更多的是使用 **HTML + Ajax** 来替代。Ajax 是异步的 JavaScript。有个这个技术后，前端工程师负责前端页面开发，而后端工程师只负责前端代码开发。

**技术的发展过程**

![](<assets/1740015478498.png>)

1.  第一阶段：使用 `servlet` 即实现逻辑代码编写，也对页面进行拼接。这种模式我们之前也接触过
    
2.  第二阶段：随着技术的发展，出现了 `JSP` ，人们发现 `JSP` 使用起来比 `Servlet` 方便很多，但是还是要在 `JSP` 中嵌套 `Java` 代码，也不利于后期的维护
    
3.  第三阶段：使用 `Servlet` 进行逻辑代码开发，而使用 `JSP` 进行数据展示
    
    ![](<assets/1740015478744.png>)
    
4.  第四阶段：使用 `servlet` 进行后端逻辑代码开发，而使用 `HTML` 进行数据展示。而这里面就存在问题，`HTML` 是静态页面，怎么进行动态数据展示呢？这就是 `ajax` 的作用了。
    

那既然 JSP 已经逐渐的退出历史舞台，那我们**为什么还要学习 `JSP` 呢**？原因有两点：

*   一些公司可能**有些老项目还在用** `**JS**P` ，所以要求我们必须动 `JSP`
    
*   我们如果不经历这些复杂的过程，就不能体现后面阶段开发的简单
    

接下来我们来学习第三阶段，使用 `EL表达式` 和 `JSTL` 标签库替换 `JSP` 中的 `Java` 代码。

### 5.5 EL 表达式

#### 5.5.1 概述

EL（全称 **Expression Language** ）表达式语言。

**作用：**

1. 用于简化 JSP 页面内的 Java 代码。

2. 主要作用是 **获取数据**。其实就是从**域对象**中获取数据，然后将数据展示在页面上。

**用法：**

page 标签设置不忽略 EI 表达式

```
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>

```

**${表达式}** 。

例如：${brands} 就是获取域中存储的 key 为 brands 的数据。

**回顾：**

${} ：学 Mybatis 时参数占位符：拼接 SQL。底层使用的是 `Statement`，会存在 SQL 注入问题。

#### 5.5.2 代码演示，**把 Servlet 的 List 转发给 jsp 页面输出**

*   定义 servlet，在 servlet 中封装一些数据并存储到 request 域对象中并转发到 `el-demo.jsp` 页面。
    
    ```
    @WebServlet("/demo1")
    public class ServletDemo1 extends HttpServlet {
        @Override
        protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            //1. 准备数据
            List<Brand> brands = new ArrayList<Brand>();
            brands.add(new Brand(1,"三只松鼠","三只松鼠",100,"三只松鼠，好吃不上火",1));
            brands.add(new Brand(2,"优衣库","优衣库",200,"优衣库，服适人生",0));
            brands.add(new Brand(3,"小米","小米科技有限公司",1000,"为发烧而生",1));
     
            //2. 存储到request域中
            request.setAttribute("brands",brands);
     
            //3. 转发到 el-demo.jsp
            request.getRequestDispatcher("/el-demo.jsp").forward(request,response);
        }
     
        @Override
        protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            this.doGet(request, response);
        }
    }
    ```
    
    **注意：** 此处需要用转发，因为**转发才可以使用 request 对象作为域对象进行数据共享**
    
*   在 `el-demo.jsp` 中通过 EL 表达式 获取数据
    
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
    
*   在浏览器的地址栏输入 `http://localhost:8080/jsp-demo/demo1` ，页面效果如下：
    
    ![](<assets/1740015479047.png>)
    

#### 5.5.3 域对象

[JavaWeb](https://so.csdn.net/so/search?q=JavaWeb&spm=1001.2101.3001.7020) 中有四大域对象，分别是：

*   page：当前页面有效
    
*   request：当前请求有效
    
*   session：当前会话有效
    
*   application：当前应用有效
    

el 表达式获取数据，会依次从这 4 个域中寻找，直到找到为止。而这四个域对象的作用范围如下图所示

![](<assets/1740015479403.png>)

例如： ${brands}，el 表达式获取数据，会先从 page 域对象中获取数据，如果没有再到 requet 域对象中获取数据，如果再没有再到 session 域对象中获取，如果还没有才会到 application 中获取数据。

### 5.6 JSTL（JSP 标准标签库）标签

#### 5.6.1 概述

JSP 标准标签库 (Jsp Standarded Tag Library) ，**使用标签取代 JSP 页面上的 Java 代码。**

**示例**

```
<c:if test="${flag == 1}">
    男
</c:if>
<c:if test="${flag == 2}">
    女
</c:if>
```

上面代码看起来比 JSP 中嵌套 Java 代码看起来舒服好了。

JSTL 提供了很多标签，如下图

![](<assets/1740015479643.png>)

**最常用的标签：**

`<c:forEach>` 标签和 `<c:if>` 标签。

**JSTL 使用**

*   导入坐标
    
    ```
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
    ```
    
*   在 JSP 页面上引入 JSTL 标签库
    
    ```
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> 
    
    ```
    
    要使用 EL 表达式记得 isELIgnored="false"
    
*   使用标签
    

#### 5.6.2 if 标签

`<c:if>`：相当于 if 判断，条件渲染

*   属性：test，用于定义条件表达式，**表达式要在 $ 里判断**。test="${flag == 1}" 可以，test="${flag }== 1" 不行。
    

```
<c:if test="${flag == 1}">
    男
</c:if>
<c:if test="${flag == 2}">
    女
</c:if>
```

**代码演示：**

*   定义一个 `servlet` ，在该 `servlet` 中向 request 域对象中添加 键是 `status` ，值为 `1` 的数据
    
    ```
    package web;
     
    import javax.servlet.*;
    import javax.servlet.http.*;
    import javax.servlet.annotation.*;
    import java.io.IOException;
     
    @WebServlet("/ServletDemo1")
    public class ServletDemo1 extends HttpServlet {
        @Override
        protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            //1. 存储数据到request域中
            request.setAttribute("status",1);
     
            //2. 转发到 jstl-if.jsp
            request.getRequestDispatcher("/hello.jsp").forward(request,response);
        }
     
        @Override
        protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            this.doGet(request, response);
        }
    }
    ```
    
*   定义 `hello.jsp` 页面，在该页面使用 `<c:if>` 标签
    
    ```
    <%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
    <%--<%@ page contentType="text/html;charset=UTF-8" language="java" %>--%>
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    <html>
    <head>
      <title>Title</title>
    </head>
    <body>
    <%--
        c:if：来完成逻辑判断，替换java  if else，别忘了isELIgnored="false"
    --%>
    <c:if test="${status ==1}">
      启用
    </c:if>
    查看${status}
    <%
      System.out.println(request.getAttribute("status"));;
    %>
    <c:if test="${status ==0}">
      禁用
    </c:if>
    </body>
    </html>
    ```
    
    **注意：** 在该页面已经要引入 JSTL 核心标签库
    
    `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`
    

#### 5.6.3 forEach 标签

`<c:forEach>`：相当于 for 循环。java 中有增强 for 循环和普通 for 循环。

**用法一**

类似于 Java 中的增强 for 循环。涉及到的 `<c:forEach>` 中的属性如下

*   items：被遍历的容器
    
*   var：遍历产生的临时变量
    
*   varStatus：遍历状态对象，用于自动生成序号，status.index 是从 0 开始，status.count 是从 1 开始
    

如下代码，是从域对象中获取名为 brands 数据，该数据是一个集合；遍历遍历，并给该集合中的每一个元素起名为 `brand`，是 Brand 对象。在循环里面使用 EL 表达式获取每一个 Brand 对象的属性值

```
<c:forEach items="${brands}" var="brand">
    <tr align="center">
        <td>${brand.id}</td>
        <td>${brand.brandName}</td>
        <td>${brand.companyName}</td>
        <td>${brand.description}</td>
    </tr>
</c:forEach>
```

```
    <c:forEach items="${brands}" var="brand" varStatus="status">
        <tr align="center">
            <td>${status.count}</td>
            <td>${brand.id}</td>
            <td>${brand.brandName}</td>
            <td>${brand.companyName}</td>
            <td>${brand.description}</td>
        </tr>
    </c:forEach>
```

**注意：** <td>${brand.id}</td > 是给 id 转 getId，调用 Brand 对象里的 getId() 方法，不是直接 id 成员变量。

**代码演示：**

*   `servlet` 还是使用之前的名为 `ServletDemo1` 。
    
*   定义名为 `jstl-foreach.jsp` 页面，内容如下：
    
    ```
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
     
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
    <input type="button" value="新增"><br>
    <hr>
    <table border="1" cellspacing="0" width="800">
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
                <td><a href="#">修改</a> <a href="#">删除</a></td>
            </tr>
        </c:forEach>
    </table>
    </body>
    </html>
    ```
    

**用法二**

类似于 Java 中的普通 for 循环。涉及到的 `<c:forEach>` 中的属性如下

*   begin：开始数
    
*   end：结束数
    
*   step：步长
    

**实例代码：**

从 0 循环到 10，变量名是 `i` ，每次自增 1，可以配合 a 标签进行分页展示。

```
<c:forEach begin="0" end="10" step="1" var="i">
    ${i}
</c:forEach>
```

### 5.7 MVC 模式和三层架构

提高代码维护性和扩展性。

#### 5.7.1 MVC 模式

MVC 是一种分层开发的模式，其中：

*   M：Model，**模型**。**处理业务**
    
*   V：View，**视图**。界面展示
    
*   C：Controller，**控制器**。获取并处理请求，调用模型来获取数据，发送数据给视图 View 展示
    

视图和控制器是表现层， 模型是业务层和数据访问层（持久层，dao 层）。

**流程：** 

![](<assets/1740015479843.png>)

**控制器**（serlvlet）用来接收浏览器发送过来的请求，控制器调用模型（JavaBean）来获取数据，比如从数据库查询数据；控制器获取到数据后再交由视图（JSP）进行数据展示。

**MVC 好处：**

*   职责单一，互不影响。每个角色做它自己的事，各司其职。
    
*   有利于分工协作。
    
*   有利于组件重用
    

#### 5.7.2 三层架构

三层架构是将我们的项目分成了三个层面，分别是 `表现层`、`业务逻辑层`、`数据访问层`。

![](<assets/1740015480092.png>)

*   **数据访问层**（也叫**持久层**，dao 层）**：**对数据库的 CRUD 基本操作
    
*   **业务逻辑层（业务层）：**对业务逻辑进行封装，组合数据访问层层中基本功能，形成复杂的业务逻辑功能。例如 `注册业务功能` ，我们会先调用 `数据访问层` 的 `selectByName()` 方法判断该用户名是否存在，如果不存在再调用 `数据访问层` 的 `insert()` 方法进行数据的添加操作
    
*   **表现层：**接收请求，封装数据，调用业务逻辑层，响应数据
    

而整个流程是，**浏览器**发送请求，**表现层**的 Servlet 接收请求并调用**业务逻辑层**的方法进行业务逻辑处理，而业务逻辑层方法调用数据访问层方法进行数据的操作，依次返回到 serlvet，然后 servlet 将数据交由 JSP 进行展示。

三层架构的每一层都有特有的**包名称：**

*   表现层： `com.itheima.controller` 或者 `com.itheima.web`
    
*   业务逻辑层：`com.itheima.service`
    
*   数据访问层：`com.itheima.dao` 或者 `com.itheima.mapper`
    

后期我们还会学习一些框架，不同的框架是对不同层进行封装的

![](<assets/1740015480305.png>)

#### 5.7.3 MVC 和三层架构

 MVC 和 三层架构有什么**区别和联系**？

通过控制器连接，控制器获取并处理请求，调用模型（访问数据库，处理业务）来获取数据，发送数据给视图展示

![](<assets/1740015480508.png>)

如上图上半部分是 MVC 模式，上图下半部分是三层架构。

`MVC 模式` 中的 C（控制器）和 V（视图）就是 `三层架构` 中的表现层，而 `MVC 模式` 中的 M（模型）就是 `三层架构` 中的 业务逻辑层 和 数据访问层。

可以将 `MVC 模式` 理解成是一个大的概念，而 `三层架构` 是对 `MVC 模式` 实现架构的思想。 那么我们以后按照要求将不同层的代码写在不同的包下，每一层里功能职责做到单一，将来如果将表现层的技术换掉，而业务逻辑层和数据访问层的代码不需要发生变化。

### 5.8 品牌数据增删改查案例

#### 5.8.1 **需求：完成品牌数据的增删改查操作**

```
    排序：<input name="ordered" value="${brand.ordered}"><br>

```

**需求：完成品牌数据的增删改查操作**

![](<assets/1740015480771.png>)

这个功能我们之前一直在做，而这个案例是将今天学习的所有的内容（包含 MVC 模式 和 三层架构）进行应用，并将整个流程贯穿起来。

#### 5.8.2 主要坑点

1.input 标签的 value 里不能有空格，特别是要 String 转 Integer 的。

```
    排序：<input name="ordered" value="${brand.ordered}"><br>

```

 2. 添加、修改数据别忘了在服务层提交事务。

```
sqlSession.commit();

```

3.Servlet 使用 post 方式别忘了设置编码

```
 request.setCharacterEncoding("utf-8");

```

4. 请求转发路径没有虚拟目录（项目名） ，目前学到的路径只有它没有虚拟目录

```
request.getRequestDispatcher("/brand.jsp").forward(request,response);

```

#### 5.8.3 环境准备

环境准备工作，我们分以下步骤实现：

*   1. 创建新的模块 brand_demo，引入坐标
    
*   2. 创建三层架构的包结构
    
*   3. 数据库表 tb_brand
    
*   4. 实体类 Brand
    
*   5.MyBatis 基础环境
    
    *   Mybatis-config.xml
        
    *   BrandMapper.xml
        
    *   BrandMapper 接口
        

**1. 创建新的模块 brand_demo，引入坐标**

创建新的模块 brand_demo，引入坐标。我们只要分析出要用到哪儿些技术，那么需要哪儿些坐标也就明确了

*   需要操作数据库。**mysql** 的驱动包
    
*   要使用 mybatis 框架。**mybaits** 的依赖包
    
*   web 项目需要用到 servlet 和 jsp。**servlet** 和 **jsp** 的依赖包
    
*   需要使用 jstl 进行数据展示。**jstl** 的依赖包
    

`pom.xml` 内容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.example</groupId>
    <artifactId>brand-demo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>
 
    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>
 
    <dependencies>
        <!-- mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.5</version>
        </dependency>
 
        <!--mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.34</version>
        </dependency>
 
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
</project>
```

**2. 创建三层架构的包结构**

创建不同的包结构，用来存储不同的类。包结构如下

![](<assets/1740015481049.png>)

 mapper 下 BrandMapper.java 接口，pojo 下 Brand.java 实体类，service 下 BrandService.java 业务类，util 下 SqlSessionFactoryUtils.java 工具类，web 下 SelectAllServlet.java、AddServlet.java 等 Servlet 类

**3. 数据库表 tb_brand**

```
-- 删除tb_brand表
drop table if exists tb_brand;
-- 创建tb_brand表
create table tb_brand
(
    -- id 主键
    id           int primary key auto_increment,
    -- 品牌名称
    brand_name   varchar(20),
    -- 企业名称
    company_name varchar(20),
    -- 排序字段
    ordered      int,
    -- 描述信息
    description  varchar(100),
    -- 状态：0：禁用  1：启用
    status       int
);
-- 添加数据
insert into tb_brand (brand_name, company_name, ordered, description, status)
values ('三只松鼠', '三只松鼠股份有限公司', 5, '好吃不上火', 0),
       ('华为', '华为技术有限公司', 100, '华为致力于把数字世界带入每个人、每个家庭、每个组织，构建万物互联的智能世界', 1),
       ('小米', '小米科技有限公司', 50, 'are you ok', 1);
```

**4. 创建实体类**

在 `pojo` 包下创建名为 `Brand` 的类。

```
public class Brand {
    // id 主键
    private Integer id;
    // 品牌名称
    private String brandName;
    // 企业名称
    private String companyName;
    // 排序字段
    private Integer ordered;
    // 描述信息
    private String description;
    // 状态：0：禁用  1：启用
    private Integer status;
 
 
    public Brand() {
    }
 
    public Brand(Integer id, String brandName, String companyName, String description) {
        this.id = id;
        this.brandName = brandName;
        this.companyName = companyName;
        this.description = description;
    }
 
    public Brand(Integer id, String brandName, String companyName, Integer ordered, String description, Integer status) {
        this.id = id;
        this.brandName = brandName;
        this.companyName = companyName;
        this.ordered = ordered;
        this.description = description;
        this.status = status;
    }
 
    public Integer getId() {
        return id;
    }
 
    public void setId(Integer id) {
        this.id = id;
    }
 
    public String getBrandName() {
        return brandName;
    }
 
    public void setBrandName(String brandName) {
        this.brandName = brandName;
    }
 
    public String getCompanyName() {
        return companyName;
    }
 
    public void setCompanyName(String companyName) {
        this.companyName = companyName;
    }
 
    public Integer getOrdered() {
        return ordered;
    }
 
    public void setOrdered(Integer ordered) {
        this.ordered = ordered;
    }
 
    public String getDescription() {
        return description;
    }
 
    public void setDescription(String description) {
        this.description = description;
    }
 
    public Integer getStatus() {
        return status;
    }
 
    public void setStatus(Integer status) {
        this.status = status;
    }
 
    @Override
    public String toString() {
        return "Brand{" +
                "id=" + id +
                ", brandName='" + brandName + '\'' +
                ", companyName='" + companyName + '\'' +
                ", ordered=" + ordered +
                ", description='" + description + '\'' +
                ", status=" + status +
                '}';
    }
}
```

**5. 准备 mybatis 环境**

定义核心配置文件 `Mybatis-config.xml` ，并将该文件放置在 `resources` 下

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--起别名，映射配置文件resultType后就可省略包名-->
    <typeAliases>
        <package name="package1.pojo"/>
    </typeAliases>
 
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///db1?useSSL=false&useServerPrepStmts=true"/>
                <property name="username" value="root"/>
                <property name="password" value="1234"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <!--扫描mapper-->
        <package name="package1.mapper"/>
    </mappers>
</configuration>
```

在 `resources` 下创建放置映射配置文件的目录结构 `com/itheima/mapper`，并在该目录下创建映射配置文件 `BrandMapper.xml`

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.mapper.BrandMapper">
    
</mapper>
```

#### 5.8.4 查询所有

![](<assets/1740015481719.png>)

当我们点击 `index.html` 页面中的 `查询所有` 这个超链接时，就能查询到上图右半部分的数据。

对于上述的功能，点击 `查询所有` 超链接是需要先请后端的 `servlet` ，由 `servlet` 跳转到对应的页面进行数据的动态展示。而整个流程如下图：

![](<assets/1740015481995.png>)

之前代码是直接用 web 层直接调用数据访问层方法，加上业务层是为了提高代码复用性。如果用 Servlet 实现包含多个 Dao 层方法的业务，其他 Servlet 想使用这个业务就得重新写，不如直接调用业务层方法方便。

**编写 BrandMapper**

在 `mapper` 包下创建创建 `BrandMapper` 接口，在接口中定义 `selectAll()` 方法

```
/**
  * 查询所有
  * @return
  */
//sql语句简单，直接注解开发，替代配置文件开发
@Select("select * from tb_brand")
List<Brand> selectAll();
```

**编写工具类**

在 `com.itheima` 包下创建 `utils` 包，并在该包下创建名为 `SqlSessionFactoryUtils` 工具类

```
package package1.util;
 
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
 
import java.io.IOException;
import java.io.InputStream;
 
public class SqlSessionFactoryUtils {
 
    private static SqlSessionFactory sqlSessionFactory;
 
    static {
        //静态代码块会随着类的加载而自动执行，且只执行一次
        //静态代码块不能抛异常，要用try-catch
        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
 
 
    public static SqlSessionFactory getSqlSessionFactory(){
        return sqlSessionFactory;
    }
}
```

**编写 BrandService**

在 `service` 包下创建 `BrandService` 类

```
public class BrandService {
    SqlSessionFactory factory = SqlSessionFactoryUtils.getSqlSessionFactory();
 
    /**
     * 查询所有
     * @return
     */
    public List<Brand> selectAll(){
        //调用BrandMapper.selectAll()
 
        //2. 获取SqlSession
        SqlSession sqlSession = factory.openSession();
        //3. 获取BrandMapper
        BrandMapper mapper = sqlSession.getMapper(BrandMapper.class);
 
        //4. 调用方法
        List<Brand> brands = mapper.selectAll();
        //一定别忘了关闭会话
        sqlSession.close();
 
        return brands;
    }
}
```

**编写 Servlet**

在 `web` 包下创建名为 `SelectAllServlet` 的 `servlet`，该 `servlet` 的逻辑如下：

*   调用 `BrandService` 的 `selectAll()` 方法进行业务逻辑处理，并接收返回的结果
    
*   将上一步返回的结果存储到 `request` 域对象中
    
*   跳转到 `brand.jsp` 页面进行数据的展示
    

具体的代码如下：

```
@WebServlet("/selectAllServlet")
public class SelectAllServlet extends HttpServlet {
    //在这里创建服务对象
    private  BrandService service = new BrandService();
 
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
 
        //1. 调用BrandService完成查询
        List<Brand> brands = service.selectAll();
        //2. 存入request域中
        request.setAttribute("brands",brands);
        //3. 转发到brand.jsp
        request.getRequestDispatcher("/brand.jsp").forward(request,response);
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

**编写 brand.jsp 页面**

将资料 `资料\2. 品牌增删改查案例\静态页面` 下的 `brand.html` 页面拷贝到项目的 `webapp` 目录下，并将该页面改成 `brand.jsp` 页面，而 `brand.jsp` 页面在表格中使用 `JSTL` 和 `EL表达式` 从 request 域对象中获取名为 `brands` 的集合数据并展示出来。页面内容如下：

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
<hr>
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
            <td><a href="/brand-demo/selectByIdServlet?id=${brand.id}">修改</a> <a href="#">删除</a></td>
        </tr>
    </c:forEach>
</table>
</body>
</html>
```

**测试**

启动服务器，并在浏览器输入 `http://localhost:8080/brand-demo/index.html`，看到如下 `查询所有` 的超链接，点击该链接就可以查询出所有的品牌数据

![](<assets/1740015482320.png>)

为什么出现这个问题呢？是因为查询到的字段名和实体类对象的属性名没有一一对应。相比看到这大家一定会解决了，就是在映射配置文件中使用 `resultMap` 标签定义映射关系。映射配置文件内容如下：

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.mapper.BrandMapper">
 
    <resultMap id="brandResultMap" type="brand">
        <result column="brand_name" property="brandName"></result>
        <result column="company_name" property="companyName"></result>
    </resultMap>
</mapper>
```

并且在 `BrandMapper` 接口中的 `selectAll()` 上使用 `@ResuleMap` 注解指定使用该映射

```
/**
  * 查询所有
  * @return
  */
@Select("select * from tb_brand")
@ResultMap("brandResultMap")
List<Brand> selectAll();
```

重启服务器，再次访问就能看到我们想要的数据了

![](<assets/1740015482764.png>)

#### 5.8.5 添加

![](<assets/1740015483052.png>)

上图是做 添加 功能流程。点击 `新增` 按钮后，会先跳转到 `addBrand.jsp` 新增页面，在该页面输入要添加的数据，输入完毕后点击 `提交` 按钮，需要将数据提交到后端，而后端进行数据添加操作，并重新将所有的数据查询出来。整个流程如下：

![](<assets/1740015483341.png>)

接下来我们根据流程来实现功能：

**编写 BrandMapper 方法**

在 `BrandMapper` 接口，在接口中定义 `add(Brand brand)` 方法

```
@Insert("insert into tb_brand values(null,#{brandName},#{companyName},#{ordered},#{description},#{status})")
void add(Brand brand);
```

**编写 BrandService 方法**

在 `BrandService` 类中定义添加品牌数据方法 `add(Brand brand)`

```
 	/**
     * 添加
     * @param brand
     */
    public void add(Brand brand){
 
        //2. 获取SqlSession
        SqlSession sqlSession = factory.openSession();
        //3. 获取BrandMapper
        BrandMapper mapper = sqlSession.getMapper(BrandMapper.class);
 
        //4. 调用方法
        mapper.add(brand);
 
        //提交事务，一定别忘了
        sqlSession.commit();
        //释放资源
        sqlSession.close();
    }
```

**改进 brand.jsp 页面**

我们需要在该页面表格的上面添加 `新增` 按钮

```
<input type="button" value="新增" id="add"><br>

```

并给该按钮绑定单击事件，当点击了该按钮需要跳转到 `brand.jsp` 添加品牌数据的页面

```
<script>
    document.getElementById("add").onclick = function (){
        location.href = "/brand-demo/addBrand.jsp";
    }
</script>
```

**注意：**该 `script` 标签建议放在 `body` 结束标签前面。

**编写 addBrand.jsp 页面**

 `addBrand.jsp` 动态页面

```
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<!DOCTYPE html>
<html lang="en">
 
<head>
    <meta charset="UTF-8">
    <title>添加品牌</title>
</head>
<body>
<h3>添加品牌</h3>
<form action="/brand-demo/addServlet" method="post">
    品牌名称：<input name="brandName"><br>
    企业名称：<input name="companyName"><br>
    排序：<input name="ordered"><br>
    描述信息：<textarea rows="5" cols="20" name="description"></textarea><br>
    状态：
    <input type="radio" name="status" value="0">禁用
    <input type="radio" name="status" value="1">启用<br>
 
    <input type="submit" value="提交">
</form>
</body>
</html>
```

**编写 servlet**

在 `web` 包下创建 `AddServlet` 的 `servlet`，该 `servlet` 的逻辑如下:

*   设置处理 post 请求乱码的字符集
    
*   接收客户端提交的数据
    
*   将接收到的数据封装到 `Brand` 对象中
    
*   调用 `BrandService` 的`add()` 方法进行添加的业务逻辑处理
    
*   跳转到 `selectAllServlet` 资源重新查询数据
    

具体的代码如下：

```
@WebServlet("/addServlet")
public class AddServlet extends HttpServlet {
    private BrandService service = new BrandService();
 
 
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
 
        //处理POST请求的乱码问题，Tomcat8及以后版本默认utf-8
        request.setCharacterEncoding("utf-8");
 
        //1. 接收表单提交的数据，封装为一个Brand对象
        String brandName = request.getParameter("brandName");
        String companyName = request.getParameter("companyName");
        String ordered = request.getParameter("ordered");
        String description = request.getParameter("description");
        String status = request.getParameter("status");
 
        //封装为一个Brand对象
        Brand brand = new Brand();
        brand.setBrandName(brandName);
        brand.setCompanyName(companyName);
        brand.setOrdered(Integer.parseInt(ordered));
        brand.setDescription(description);
        brand.setStatus(Integer.parseInt(status));
 
        //2. 调用service 完成添加
        service.add(brand);
 
        //3. 转发到查询所有Servlet
        request.getRequestDispatcher("/selectAllServlet").forward(request,response);
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

**测试**

点击 `brand.jsp` 页面的 `新增` 按钮，会跳转到 `addBrand.jsp`页面

![](<assets/1740015483552.png>)

点击 `提交` 按钮，就能看到如下页面，里面就包含我们刚添加的数据

![](<assets/1740015483739.png>)

#### 5.8.6 修改（回显数据）

![](<assets/1740015483968.png>)

点击每条数据后面的 `编辑` 按钮会跳转到修改页面，如下图：

![](<assets/1740015484182.png>)

在该修改页面我们可以看到将 `编辑` 按钮所在行的数据 **回显** 到表单，然后需要修改那个数据在表单中进行修改，然后点击 `提交` 的按钮将数据提交到后端，后端再将数据存储到数据库中。

从上面的例子我们知道 `修改` 功能需要从两方面进行实现，数据回显和修改操作。

**回显数据**

![](<assets/1740015484508.png>)

上图就是回显数据的效果。要实现这个效果，那当点击 `修改` 按钮时不能直接跳转到 `update.jsp` 页面，而是需要先带着当前行数据的 `id` 请求后端程序，后端程序根据 `id` 查询数据，将数据存储到域对象中跳转到 `update.jsp` 页面进行数据展示。整体流程如下

![](<assets/1740015484846.png>)

**编写 BrandMapper 方法**

在 `BrandMapper` 接口，在接口中定义 `selectById(int id)` 方法

```
	/**
     * 根据id查询
     * @param id
     * @return
     */
    @Select("select * from tb_brand where id = #{id}")
    @ResultMap("brandResultMap")
    Brand selectById(int id);
```

**编写 BrandService 方法**

在 `BrandService` 类中定义根据 id 查询数据方法 `selectById(int id)`

```
    /**
     * 根据id查询
     * @return
     */
    public Brand selectById(int id){
        //调用BrandMapper.selectAll()
        //2. 获取SqlSession
        SqlSession sqlSession = factory.openSession();
        //3. 获取BrandMapper
        BrandMapper mapper = sqlSession.getMapper(BrandMapper.class);
        //4. 调用方法
        Brand brand = mapper.selectById(id);
        sqlSession.close();
        return brand;
    }
```

**编写 servlet**

在 `web` 包下创建 `SelectByIdServlet` 的 `servlet`，该 `servlet` 的逻辑如下:

*   获取请求数据 `id`
    
*   调用 `BrandService` 的 `selectById()` 方法进行数据查询的业务逻辑
    
*   将查询到的数据存储到 request 域对象中
    
*   跳转到 `update.jsp` 页面进行数据真实
    

具体代码如下：

```
@WebServlet("/selectByIdServlet")
public class SelectByIdServlet extends HttpServlet {
    private  BrandService service = new BrandService();
 
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 接收id
        String id = request.getParameter("id");
        //2. 调用service查询
        Brand brand = service.selectById(Integer.parseInt(id));
        //3. 存储到request中
        request.setAttribute("brand",brand);
        //4. 转发到update.jsp
        request.getRequestDispatcher("/update.jsp").forward(request,response);
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

**编写 update.jsp 页面**

拷贝 `addBrand.jsp` 页面，改名为 `update.jsp` 并做出以下修改：

```
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>修改品牌</title>
</head>
<body>
<h3>修改品牌</h3>
<form action="/brand-demo/updateServlet" method="post">
 
    品牌名称：<input name="brandName" value="${brand.brandName}"><br>
    企业名称：<input name="companyName" value="${brand.companyName}"><br>
    排序：<input name="ordered" value="${brand.ordered}"><br>
    描述信息：<textarea rows="5" cols="20" name="description">${brand.description} </textarea><br>
    状态：
    <c:if test="${brand.status == 0}">
        <input type="radio" name="status" value="0" checked>禁用
        <input type="radio" name="status" value="1">启用<br>
    </c:if>
 
    <c:if test="${brand.status == 1}">
        <input type="radio" name="status" value="0" >禁用
        <input type="radio" name="status" value="1" checked>启用<br>
    </c:if>
 
    <input type="submit" value="提交">
</form>
</body>
</html>
```

**修改数据**

做完回显数据后，接下来我们要做修改数据了，而下图是修改数据的效果：

![](<assets/1740015485206.png>)

在修改页面进行数据修改，点击 `提交` 按钮，会将数据提交到后端程序，后端程序会对表中的数据进行修改操作，然后重新进行数据的查询操作。整体流程如下：

![](<assets/1740015485444.png>)

**编写 BrandMapper 方法**

在 `BrandMapper` 接口，在接口中定义 `update(Brand brand)` 方法

```
/**
  * 修改
  * @param brand
  */
@Update("update tb_brand set brand_name = #{brandName},company_name = #{companyName},ordered = #{ordered},description = #{description},status = #{status} where id = #{id}")
void update(Brand brand);
```

**编写 BrandService 方法**

在 `BrandService` 类中定义根据 id 查询数据方法 `update(Brand brand)`

```
	/**
     * 修改
     * @param brand
     */
    public void update(Brand brand){
        //2. 获取SqlSession
        SqlSession sqlSession = factory.openSession();
        //3. 获取BrandMapper
        BrandMapper mapper = sqlSession.getMapper(BrandMapper.class);
        //4. 调用方法
        mapper.update(brand);
        //提交事务
        sqlSession.commit();
        //释放资源
        sqlSession.close();
    }
```

**编写 servlet**

在 `web` 包下创建 `AddServlet` 的 `servlet`，该 `servlet` 的逻辑如下:

*   设置处理 post 请求乱码的字符集
    
*   接收客户端提交的数据
    
*   将接收到的数据封装到 `Brand` 对象中
    
*   调用 `BrandService` 的`update()` 方法进行添加的业务逻辑处理
    
*   跳转到 `selectAllServlet` 资源重新查询数据
    

具体的代码如下：

```
@WebServlet("/updateServlet")
public class UpdateServlet extends HttpServlet {
    private BrandService service = new BrandService();
 
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
 
        //处理POST请求的乱码问题
        request.setCharacterEncoding("utf-8");
        //1. 接收表单提交的数据，封装为一个Brand对象
        String id = request.getParameter("id");
        String brandName = request.getParameter("brandName");
        String companyName = request.getParameter("companyName");
        String ordered = request.getParameter("ordered");
        String description = request.getParameter("description");
        String status = request.getParameter("status");
 
        //封装为一个Brand对象
        Brand brand = new Brand();
        brand.setId(Integer.parseInt(id));
        brand.setBrandName(brandName);
        brand.setCompanyName(companyName);
        brand.setOrdered(Integer.parseInt(ordered));
        brand.setDescription(description);
        brand.setStatus(Integer.parseInt(status));
 
        //2. 调用service 完成修改
        service.update(brand);
 
        //3. 转发到查询所有Servlet
        request.getRequestDispatcher("/selectAllServlet").forward(request,response);
    }
 
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

**存在问题：update.jsp 页面提交数据时是没有携带主键数据的，而后台修改数据需要根据主键进行修改。**

针对这个问题，我们不希望页面将主键 id 展示给用户看，但是又希望在提交数据时能将主键 id 提交到后端。此时我们就想到了在学习 HTML 时学习的隐藏域，在 `update.jsp` 页面的表单中添加如下代码：

```
<%--隐藏域，提交id--%>
<input type="hidden" name="id" value="${brand.id}">
```

`update.jsp` 页面的最终代码如下：

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>修改品牌</title>
</head>
<body>
<h3>修改品牌</h3>
<form action="/brand-demo/updateServlet" method="post">
 
    <%--隐藏域，提交id--%>
    <input type="hidden" name="id" value="${brand.id}">
 
    品牌名称：<input name="brandName" value="${brand.brandName}"><br>
    企业名称：<input name="companyName" value="${brand.companyName}"><br>
    排序：<input name="ordered" value="${brand.ordered}"><br>
    描述信息：<textarea rows="5" cols="20" name="description">${brand.description} </textarea><br>
    状态：
    <c:if test="${brand.status == 0}">
        <input type="radio" name="status" value="0" checked>禁用
        <input type="radio" name="status" value="1">启用<br>
    </c:if>
 
    <c:if test="${brand.status == 1}">
        <input type="radio" name="status" value="0" >禁用
        <input type="radio" name="status" value="1" checked>启用<br>
    </c:if>
    <input type="submit" value="提交">
</form>
</body>
</html>
```