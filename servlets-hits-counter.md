# Servlets——点击计数器

## 网页点击计数器：

很多时候，你可能有兴趣知道网站的某个特定页面上的总点击量。使用 Servlet 来计算这些点击量是非常简单的，因为一个 Servlet 的生命周期是由它运行所在的容器控制的。

以下是实现一个简单的基于 Servlet 生命周期的网页点击计数器需要采取的步骤：

- 在 init() 方法中初始化一个全局变量。

- 每次调用 doGet() 或 doPost() 方法时，都增加全局变量。

- 如果需要，你可以使用一个数据库表来存储全局变量的值在 destroy() 中。在下次初始化 Servlet 时，该值可在 init() 方法内被读取。这一步是可选的。

- 如果你只想对一个 session 会话计数一次页面点击，那么请使用 isNew() 方法来检查该 session 会话是否已点击过相同页面。这一步是可选的。

- 你可以通过显示全局计数器的值，来在网站上展示页面的总点击量。这一步是可选的。

在这里，我们假设 Web 容器将无法重新启动。如果是重新启动或 servlet 被销毁，计数器将被重置。

## 实例：

本实例演示了如何实现一个简单的网页点击计数器：

``` 
import java.io.*;
import java.sql.Date;
import java.util.*;
import javax.servlet.*;
import javax.servlet.http.*;
public class PageHitCounter extends HttpServlet{   
  private int hitCount;                
  public void init() 
  { 
     // Reset hit counter.
     hitCount = 0;
  } 
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // Set response content type
      response.setContentType("text/html");
      // This method executes whenever the servlet is hit 
      // increment hitCount 
      hitCount++; 
      PrintWriter out = response.getWriter();
      String title = "Total Number of Hits";
      String docType =
      "<!doctype html public \"-//w3c//dtd html 4.0 " +
      "transitional//en\">\n";
      out.println(docType +
        "<html>\n" +
        "<head><title>" + title + "</title></head>\n" +
        "<body bgcolor=\"#f0f0f0\">\n" +
        "<h1 align=\"center\">" + title + "</h1>\n" +
        "<h2 align=\"center\">" + hitCount + "</h2>\n" +
        "</body></html>");
  }
  public void destroy() 
  { 
      // This is optional step but if you like you
      // can write hitCount value in your database.
  } 
} 
```

现在让我们来编译上面的 servlet，并在 web.xml 文件中创建以下条目：

``` 
....
 <servlet>
     <servlet-name>PageHitCounter</servlet-name>
     <servlet-class>PageHitCounter</servlet-class>
 </servlet>
 <servlet-mapping>
     <servlet-name>PageHitCounter</servlet-name>
     <url-pattern>/PageHitCounter</url-pattern>
 </servlet-mapping>
....
```

现在通过访问 URL http://localhost:8080/PageHitCounter 来调用这个 servlet。这将会在每次页面刷新时，把计数器的值增加 1，结果如下所示：

<pre class="result notranslate">
<h1 align="center">Total Number of Hits</h1>
<h2 align="center">6</h2>
</pre>


## 网站点击计数器：

很多时候，你可能有兴趣知道整个网站的总点击量。在 Servlet 中，这也是非常简单的，我们可以使用过滤器做到这一点。

以下是实现一个简单的基于过滤器生命周期的网站点击计数器需要采取的步骤：

- 在过滤器的 init() 方法中初始化一个全局变量。

- 每次调用 doFilter 方法时，都增加全局变量。

- 如果需要，你可以使用一个数据库表来存储全局变量的值在过滤器的 destroy() 中。在下次初始化过滤器时，该值可在 init() 方法内被读取。这一步是可选的。

在这里，我们假设 Web 容器将无法重新启动。如果是重新启动或 servlet 被销毁，点击计数器将被重置。

## 实例：

本实例演示了如何实现一个简单的网站点击计数器：

``` 
// Import required java libraries
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import java.util.*;
public class SiteHitCounter implements Filter{   
  private int hitCount;                
  public void  init(FilterConfig config) 
                    throws ServletException{
     // Reset hit counter.
     hitCount = 0;
  }
  public void  doFilter(ServletRequest request, 
              ServletResponse response,
              FilterChain chain) 
              throws java.io.IOException, ServletException {
      // increase counter by one
      hitCount++;
      // Print the counter.
      System.out.println("Site visits count :"+ hitCount );
      // Pass request back down the filter chain
      chain.doFilter(request,response);
  }
  public void destroy() 
  { 
      // This is optional step but if you like you
      // can write hitCount value in your database.
  } 
} 
```

现在让我们来编译上面的 servlet，并在 web.xml 文件中创建以下条目：

``` 
....
<filter>
   <filter-name>SiteHitCounter</filter-name>
   <filter-class>SiteHitCounter</filter-class>
</filter>
<filter-mapping>
   <filter-name>SiteHitCounter</filter-name>
   <url-pattern>/*</url-pattern>
</filter-mapping>
....
```

现在访问网站的任意页面，比如 http://localhost:8080/。这将会在每次任意页面被点击时，把计数器的值增加 1，它会在日志中显示以下消息：

<pre class="result notranslate">
Site visits count : 1
Site visits count : 2
Site visits count : 3
Site visits count : 4
Site visits count : 5
..................
</pre>



