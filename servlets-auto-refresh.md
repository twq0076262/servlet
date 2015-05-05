# Servlets——自动刷新页面

假设有一个网页，它是显示现场比赛成绩或股票市场状况或货币兑换率。对于所有这些类型的页面，你需要定期刷新网页。

Java Servlet 提供了一个机制，使得网页会在给定的时间间隔自动刷新。

刷新网页的最简单的方式是使用响应对象的方法 **setIntHeader()**。以下是这种方法的定义：

``` 
public void setIntHeader(String header, int headerValue)
```

此方法把头信息 “Refresh” 连同一个表示时间间隔的整数值（以秒为单位）发送回浏览器。

## 自动刷新页面实例:

本实例演示了 Servlet 如何使用 **setIntHeader()** 方法来设置 **Refresh** 头信息，从而实现自动刷新页面。

``` 
// Import required java libraries
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import java.util.*;
// Extend HttpServlet class
public class Refresh extends HttpServlet {
  // Method to handle GET method request.
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // Set refresh, autoload time as 5 seconds
      response.setIntHeader("Refresh", 5);
      // Set response content type
      response.setContentType("text/html");
      // Get current time
      Calendar calendar = new GregorianCalendar();
      String am_pm;
      int hour = calendar.get(Calendar.HOUR);
      int minute = calendar.get(Calendar.MINUTE);
      int second = calendar.get(Calendar.SECOND);
      if(calendar.get(Calendar.AM_PM) == 0)
        am_pm = "AM";
      else
        am_pm = "PM"; 
      String CT = hour+":"+ minute +":"+ second +" "+ am_pm;   
      PrintWriter out = response.getWriter();
      String title = "Auto Page Refresh using Servlet";
      String docType =
      "<!doctype html public \"-//w3c//dtd html 4.0 " +
      "transitional//en\">\n";
      out.println(docType +
        "<html>\n" +
        "<head><title>" + title + "</title></head>\n"+
        "<body bgcolor=\"#f0f0f0\">\n" +
        "<h1 align=\"center\">" + title + "</h1>\n" +
        "<p>Current Time is: " + CT + "</p>\n");
  }
  // Method to handle POST method request.
  public void doPost(HttpServletRequest request,
                     HttpServletResponse response)
      throws ServletException, IOException {
     doGet(request, response);
  }
}
```

现在让我们来编译上面的 servlet，并在 web.xml 文件中创建以下条目：

``` 
....
 <servlet>
     <servlet-name>Refresh</servlet-name>
     <servlet-class>Refresh</servlet-class>
 </servlet>
 <servlet-mapping>
     <servlet-name>Refresh</servlet-name>
     <url-pattern>/Refresh</url-pattern>
 </servlet-mapping>
....
```

现在通过访问 URL http://localhost:8080/Refresh 来调用这个 servlet。这将会每隔 5 秒钟显示一次当前系统时间。运行该 servlet，并等待查看结果：

<pre class="result notranslate">
<h1 align="center">Auto Page Refresh using Servlet</h1>
<p>Current Time is: 9:44:50 PM</p>
</pre>
