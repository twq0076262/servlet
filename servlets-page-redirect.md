# Servlets——网页重定向

当文档移动到新的位置，我们需要向客户端发送这个新位置时，就要用到网页重定向。当然，也可能是为了负载均衡，或者只是为了简单的随机，这些情况都有可能用到网页重定向。

重定向请求到另一个网页的最简单的方式是使用 response 对象的 **sendRedirect()** 方法。下面是该方法的定义： 

``` 
public void HttpServletResponse.sendRedirect(String location)
throws IOException 
```

该方法把响应连同状态码和新的网页位置发送回浏览器。你也可以通过把 setStatus() 和 setHeader() 方法一起使用来达到同样的效果：

``` 
....
String site = "http://www.newpage.com" ;
response.setStatus(response.SC_MOVED_TEMPORARILY);
response.setHeader("Location", site); 
....
```

## 实例：

本实例显示了 servlet 如何进行页面重定向到另一个位置：

``` 
import java.io.*;
import java.sql.Date;
import java.util.*;
import javax.servlet.*;
import javax.servlet.http.*;
public class PageRedirect extends HttpServlet{    
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // Set response content type
      response.setContentType("text/html");
      // New location to be redirected
      String site = new String("http://www.photofuntoos.com");
      response.setStatus(response.SC_MOVED_TEMPORARILY);
      response.setHeader("Location", site);    
    }
} 
```

现在让我们来编译上面的 servlet，并在 web.xml 文件中创建以下条目：

``` 
....
 <servlet>
     <servlet-name>PageRedirect</servlet-name>
     <servlet-class>PageRedirect</servlet-class>
 </servlet>
 <servlet-mapping>
     <servlet-name>PageRedirect</servlet-name>
     <url-pattern>/PageRedirect</url-pattern>
 </servlet-mapping>
....
```

现在通过访问 URL http://localhost:8080/PageRedirect 来调用这个 servlet。这将使你转到给定的 URL http://www.photofuntoos.com。
