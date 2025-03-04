---
url: https://blog.csdn.net/qq_31832209/article/details/116526830
title: Java 中实现跨域的五种方式_如何在 java 设置跨域 - CSDN 博客
date: 2025-02-21 14:22:41
tag: 
summary: 文章浏览阅读 1.7w 次，点赞 20 次，收藏 108 次。
---
#### 一、什么是跨域？为什么会出现跨域

定义：当一个请求 url 的协议、域名、端口三者之间任意一个与当前页面 url 不同即为跨域。

原因：在前后端分离的模式下，前后端的域名是不一致的，此时就会发生跨域访问问题。在请求的过程中我们要想回去数据一般都是 post/get 请求，所以.. 跨域问题出现。

跨域问题来源于 JavaScript 的同源策略，即只有 协议 + 主机名 + 端口号 (如存在) 相同，则允许相互访问。也就是说 JavaScript 只能访问和操作自己域下的资源，不能访问和操作其他域下的资源。跨域问题是针对 JS 和 ajax 的，html 本身没有跨域问题，比如 a 标签、script 标签、甚至 form 标签（可以直接跨域发送数据并接收数据）等  
作者：Tz1314  
链接：https://www.jianshu.com/p/9b6b6a135432  
来源：简书  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

#### 二、Java 实现跨域方式

**1. 返回新的 CorsFilter(全局跨域)**

```
package org.chuancey.config;

import org.springframework.context.annotation.Bean;

import org.springframework.context.annotation.Configuration;

import org.springframework.web.cors.CorsConfiguration;

import org.springframework.web.cors.UrlBasedCorsConfigurationSource;

import org.springframework.web.filter.CorsFilter;

public class GlobalCorsConfig {

public CorsFilter corsFilter() {

CorsConfiguration config = new CorsConfiguration();

        config.addAllowedOrigin("*");

        config.setAllowCredentials(true);

        config.addAllowedMethod("*");

        config.addAllowedHeader("*");

        config.addExposedHeader("*");

UrlBasedCorsConfigurationSource corsConfigurationSource = new UrlBasedCorsConfigurationSource();

        corsConfigurationSource.registerCorsConfiguration("/**",config);

return new CorsFilter(corsConfigurationSource);
```

**2. 重写 WebMvcConfigurer(全局跨域)**

```
package org.chuancey.config;

import org.springframework.context.annotation.Configuration;

import org.springframework.web.servlet.config.annotation.CorsRegistry;

import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;

import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

public class CorsConfig implements WebMvcConfigurer {

public void addCorsMappings(CorsRegistry registry) {

        registry.addMapping("/**")

                .allowedMethods("GET", "POST", "OPTIONS", "DELETE", "PUT", "PATCH")

public void addResourceHandlers(ResourceHandlerRegistry registry) {

        registry.addResourceHandler("/**")

                .addResourceLocations("classpath:/static/");

        registry.addResourceHandler("swagger-ui.html")

                .addResourceLocations("classpath:/META-INF/resources/");

        registry.addResourceHandler("doc.html")

                .addResourceLocations("classpath:/META-INF/resources/");

        registry.addResourceHandler("/webjars/**")

                .addResourceLocations("classpath:/META-INF/resources/webjars/");
```

**3. 使用注解 (局部跨域)**

在控制器 (类上) 上使用注解 @CrossOrigin，表示该类的所有方法允许跨域。

```
@CrossOrigin(origins = "*")

public class VerificationController {
```

在方法上使用注解 @CrossOrigin

```
@PostMapping("/check/phone")

@CrossOrigin(origins = "*")

public boolean checkPhoneNumber(@RequestBody @ApiParam VerificationPojo verification) throws BusinessException {
```

**4. 手动设置响应头 (局部跨域)**

使用 HttpServletResponse 对象添加响应头 (Access-Control-Allow-Origin) 来授权原始域，这里 Origin 的值也可以设置为 “*”, 表示全部放行。

```
public String home(HttpServletResponse response) {

    response.addHeader("Access-Allow-Control-Origin","*");
```

**5. 使用自定义 filter 实现跨域**

```
import java.io.IOException;

import javax.servlet.Filter;

import javax.servlet.FilterChain;

import javax.servlet.FilterConfig;

import javax.servlet.ServletException;

import javax.servlet.ServletRequest;

import javax.servlet.ServletResponse;

import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Component;

@WebFilter(filterName = "accessFilter", urlPatterns = "/*")

public class MyCorsFilter implements Filter {

public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {

HttpServletResponse response = (HttpServletResponse) res;

    response.setHeader("Access-Control-Allow-Origin", "*");

    response.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS, DELETE");

    response.setHeader("Access-Control-Max-Age", "3600");

    response.setHeader("Access-Control-Allow-Headers", "x-requested-with,content-type");

    chain.doFilter(req, res);

public void init(FilterConfig filterConfig) {log.info("AccessFilter过滤器初始化！");}
```

xml 使自定义 Filter 生效方式

```
<filter-name>CorsFilter</filter-name>

<filter-class>org.chuancey.filter.MyCorsFilter</filter-class>

<filter-name>CorsFilter</filter-name>

<url-pattern>/*</url-pattern>
```