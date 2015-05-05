# Servlets——编写过滤器

Servlet 过滤器是可用于 Servlet 编程的 Java 类，有以下目的：

- 在客户端的请求访问后端资源之前，拦截这些请求。

- 在服务器的响应发送回客户端之前，处理这些响应。

根据规范建议的各种类型的过滤器：

- 身份验证过滤器（Authentication Filters）。

- 数据压缩过滤器（Data compression Filters）。

- 加密过滤器（Encryption Filters）。

- 触发资源访问事件过滤器。

- 图像转换过滤器（Image Conversion Filters）。

- 日志记录和审核过滤器（Logging and Auditing Filters）。

- MIME-TYPE 链过滤器（MIME-TYPE Chain Filters）。

- 标记化过滤器（Tokenizing Filters）。

- XSL/T 过滤器（XSL/T Filters），转换 XML 内容。

过滤器被部署在部署描述符文件 **web.xml** 中，然后映射到您的应用程序的部署描述符中的 Servlet 名称或 URL 模式。

当 Web 容器启动 Web 应用程序时，它会为你在部署描述符中声明的每一个过滤器创建一个实例。该过滤器执行的顺序是按它们在部署描述符中声明的顺序。

## Servlet 过滤器方法：

过滤器是一个实现了 javax.servlet.Filter 接口的 Java 类。javax.servlet.Filter 接口定义了三个方法：

<table class="table table-bordered">
<tr><th style="width:5%">序号</th><th>方法 &amp; 描述</th></tr>
<tr><td>1</td><td><p><b>public void doFilter (ServletRequest, ServletResponse, FilterChain)</b></p>
<p>该方法在每次一个请求/响应对因客户端在链的末端请求资源而通过链传递时由容器调用。</p></td></tr>
<tr><td>2</td><td><p><b>public void init(FilterConfig filterConfig)</b></p>
<p>该方法由 Web 容器调用，指示一个过滤器被放入服务。</p></td></tr>
<tr><td>3</td><td><p><b>public void destroy()</b></p>
<p>该方法由 Web 容器调用，指示一个过滤器从服务被去除。</p></td></tr>
</table> 

## Servlet 过滤器实例：

以下是 Servlet 过滤器的实例，将输出客户端的 IP 地址和当前的日期时间。本实例让你对 Servlet 过滤器有基本的了解，你可以使用相同的概念编写更复杂的过滤器应用程序：

``` 
// Import required java libraries
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import java.util.*;
// Implements Filter class
public class LogFilter implements Filter  {
   public void  init(FilterConfig config) 
                         throws ServletException{
      // Get init parameter 
      String testParam = config.getInitParameter("test-param"); 
      //Print the init parameter 
      System.out.println("Test Param: " + testParam); 
   }
   public void  doFilter(ServletRequest request, 
                 ServletResponse response,
                 FilterChain chain) 
                 throws java.io.IOException, ServletException {
      // Get the IP address of client machine.   
      String ipAddress = request.getRemoteAddr();
      // Log the IP address and current timestamp.
      System.out.println("IP "+ ipAddress + ", Time "
                                       + new Date().toString());
      // Pass request back down the filter chain
      chain.doFilter(request,response);
   }
   public void destroy( ){
      /* Called before the Filter instance is removed 
      from service by the web container*/
   }
}
```

以通常的方式编译 **LogFilter.java**，把你的类文件放入 <Tomcat-installation-directory>/webapps/ROOT/WEB-INF/classes 中。

## Web.xml 中的 Servlet 过滤器映射：

定义过滤器，然后映射到一个 URL 或 Servlet，这与定义 Servlet，然后映射到一个 URL 模式方式大致相同。在部署描述符文件 **web.xml** 中为过滤器标签创建下面的条目：

``` 
<filter>
   <filter-name>LogFilter</filter-name>
   <filter-class>LogFilter</filter-class>
   <init-param>
	  <param-name>test-param</param-name>
	  <param-value>Initialization Paramter</param-value>
   </init-param>
</filter>
<filter-mapping>
   <filter-name>LogFilter</filter-name>
   <url-pattern>/*</url-pattern>
</filter-mapping>
```

上述过滤器适用于所有的 servlet，因为我们在配置中指定 /* 。如果你只想在少数的 servlet 上应用过滤器，你可以指定一个特定的 servlet 路径。

现在试着以常用的方式调用任何 servlet，你将会看到在 Web 服务器中生成的日志。你也可以使用 Log4J 记录器来把上面的日志记录到一个单独的文件中。

## 使用多个过滤器：

Web 应用程序可以根据特定的目的定义若干个不同的过滤器。假设你定义了两个过滤器 *AuthenFilter* 和 *LogFilter*。你需要创建一个如下所述的不同的映射，其余的处理与上述所讲解的大致相同：

``` 
<filter>
   <filter-name>LogFilter</filter-name>
   <filter-class>LogFilter</filter-class>
   <init-param>
	  <param-name>test-param</param-name>
	  <param-value>Initialization Paramter</param-value>
   </init-param>
</filter>
<filter>
   <filter-name>AuthenFilter</filter-name>
   <filter-class>AuthenFilter</filter-class>
   <init-param>
	  <param-name>test-param</param-name>
	  <param-value>Initialization Paramter</param-value>
   </init-param>
</filter>
<filter-mapping>
   <filter-name>LogFilter</filter-name>
   <url-pattern>/*</url-pattern>
</filter-mapping>
<filter-mapping>
   <filter-name>AuthenFilter</filter-name>
   <url-pattern>/*</url-pattern>
</filter-mapping>
```

## 过滤器的应用顺序：

web.xml 中的 filter-mapping 元素的顺序决定了 Web 容器应用过滤器到 Servlet 的顺序。若要反转过滤器的顺序，你只需要在 web.xml 文件中反转 filter-mapping 元素即可。

例如，上面的实例将先应用 LogFilter，然后再应用 AuthenFilter，但是下面的实例将颠倒这个顺序：

``` 
<filter-mapping>
   <filter-name>AuthenFilter</filter-name>
   <url-pattern>/*</url-pattern>
</filter-mapping>
<filter-mapping>
   <filter-name>LogFilter</filter-name>
   <url-pattern>/*</url-pattern>
</filter-mapping>
```


