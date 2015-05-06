# Servlets——处理日期

使用 Servlet 的最重要的优势之一是，可以使用核心 Java 中的大多数可用的方法。本章将讲解 Java 提供的 **java.util** 包中的 **Date** 类，这个类封装了当前的日期和时间。

Date 类支持两个构造函数。第一个构造函数初始化当前日期和时间的对象。

``` 
Date( )
```


下面的构造函数接受一个参数，该参数等于 1970 年 1 月 1 日凌晨零点以来经过的毫秒数。


``` 
Date(long millisec)
```

一旦你有一个可用的 Date 对象，你可以调用下列任意支持的方法来使用日期：

<table class="table table-bordered">
<tr>
<th style="width:5%">序号</th><th style="width:95%">方法和描述</th></tr>
<tr><td>1</td><td><p><b>boolean after(Date date)</b></p>
<p>如果调用的 Date 对象中包含的日期在 date 指定的日期之后，则返回 true，否则返回 false。</p></td></tr>
<tr><td>2</td><td><p><b>boolean before(Date date)</b></p>
<p>如果调用的 Date 对象中包含的日期在 date 指定的日期之前，则返回 true，否则返回 false。</p></td></tr>
<tr><td>3</td><td><p><b>Object clone( )</b></p>
<p>重复调用 Date 对象。</p></td></tr>
<tr><td>4</td><td><p><b>int compareTo(Date date)</b></p>
<p>把调用对象的值与 date 的值进行比较。如果两个值是相等的，则返回 0。如果调用对象在 date 之前，则返回一个负值。如果调用对象在 date 之后，则返回一个正值。</p></td></tr>
<tr><td>5</td><td><p><b>int compareTo(Object obj)</b></p>
<p>如果 obj 是 Date 类，则操作等同于 compareTo(Date)。否则，它会抛出一个 ClassCastException。</p></td></tr>
<tr><td>6</td><td><p><b>boolean equals(Object date)</b></p>
<p>如果调用的 Date 对象中包含的时间和日期与 date 指定的相同，则返回 true，否则返回 false。</p></td></tr>
<tr><td>7</td><td><p><b>long getTime( )</b></p>
<p>返回 1970 年 1 月 1 日以来经过的毫秒数。</p></td></tr>
<tr><td>8</td><td><p><b>int hashCode( )</b></p>
<p>为调用对象返回哈希代码。</p></td></tr>
<tr><td>9</td><td><p><b>void setTime(long time)</b></p>
<p>设置 time 指定的时间和日期，这表示从 1970 年 1 月 1 日凌晨零点以来经过的时间（以毫秒为单位）。</p></td></tr>
<tr><td>10</td><td><p><b>String toString( )</b></p>
<p>转换调用的 Date 对象为一个字符串，并返回结果。</p></td></tr>
</table> 

## 获取当前的日期和时间

在 Java Servlet 中获取当前的日期和时间是非常容易的。你可以使用一个简单的 Date 对象的 *toString()* 方法来输出当前的日期和时间，如下所示：

``` 
// Import required java libraries
import java.io.*;
import java.util.Date;
import javax.servlet.*;
import javax.servlet.http.*; 
// Extend HttpServlet class
public class CurrentDate extends HttpServlet { 
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // Set response content type
      response.setContentType("text/html"); 
      PrintWriter out = response.getWriter();
      String title = "Display Current Date & Time";
      Date date = new Date();
      String docType =
      "<!doctype html public \"-//w3c//dtd html 4.0 " +
      "transitional//en\">\n";
      out.println(docType +
        "<html>\n" +
        "<head><title>" + title + "</title></head>\n" +
        "<body bgcolor=\"#f0f0f0\">\n" +
        "<h1 align=\"center\">" + title + "</h1>\n" +
        "<h2 align=\"center\">" + date.toString() + "</h2>\n" +
        "</body></html>");
  }
}
```

现在，让我们来编译上面的 servlet，并在 web.xml 文件中创建适当的条目，然后通过访问 http://localhost:8080/CurrentDate 来调用该 servlet。这将会产生如下的结果：

<pre class="result notranslate">
<h1 align="center">Display Current Date &amp; Time</h1>
<h2 align="center">Mon Jun 21 21:46:49 GMT+04:00 2010</h2>
</pre>



尝试刷新 URL http://localhost:8080/CurrentDate，每隔几秒刷新一次你都会发现显示时间的差异。

## 日期比较：

正如上面所提到的，你可以在 Servlet 中使用所有可用的 Java 方法。如果你需要比较两个日期，以下是方法：

- 你可以使用 getTime() 来获取两个对象自 1970 年 1 月 1 日凌晨零点以来经过的时间（以毫秒为单位），然后对这两个值进行比较。

- 你可以使用方法 before( )、after( ) 和 equals( )。由于一个月里 12 号在 18 号之前，例如，new Date(99, 2, 12).before(new Date (99, 2, 18)) 返回 true。

- 你可以使用 compareTo( ) 方法，该方法由 Comparable 接口定义，由 Date 实现。

## 使用 SimpleDateFormat 格式化日期

SimpleDateFormat 是一个以语言环境敏感的方式来格式化和解析日期的具体类。 SimpleDateFormat 允许你选择任何用户定义的日期时间格式化的模式。

让我们修改上面的实例，如下所示：


``` 
// Import required java libraries
import java.io.*;
import java.text.*;
import java.util.Date;
import javax.servlet.*;
import javax.servlet.http.*;
// Extend HttpServlet class
public class CurrentDate extends HttpServlet {
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // Set response content type
      response.setContentType("text/html"); 
      PrintWriter out = response.getWriter();
      String title = "Display Current Date & Time";
      Date dNow = new Date( );
      SimpleDateFormat ft = 
      new SimpleDateFormat ("E yyyy.MM.dd 'at' hh:mm:ss a zzz");
      String docType =
      "<!doctype html public \"-//w3c//dtd html 4.0 " +
      "transitional//en\">\n";
      out.println(docType +
        "<html>\n" +
        "<head><title>" + title + "</title></head>\n" +
        "<body bgcolor=\"#f0f0f0\">\n" +
        "<h1 align=\"center\">" + title + "</h1>\n" +
        "<h2 align=\"center\">" + ft.format(dNow) + "</h2>\n" +
        "</body></html>");
  }
}
```

再次编译上面的 servlet，然后通过访问 http://localhost:8080/CurrentDate 来调用该 servlet。这将会产生如下的结果：

<pre class="result notranslate">
<h1 align="center">Display Current Date &amp; Time</h1>
<h2 align="center">Mon 2010.06.21 at 10:06:44 PM GMT+04:00</h2>
</pre>


## 简单的日期格式的格式代码：

使用时间模式字符串来指定时间格式。在这种模式下，所有的 ASCII 字母被保留为模式字母，这些字母定义如下：

<table class="table table-bordered" border="1" cellpadding="5" cellspacing="0" width="100%">
<tr><th width="10%">字符</th><th width="45%">描述</th><th>实例</th>
</tr>
<tr><td>G</td><td> Era 指示器</td><td>AD</td></tr>
<tr><td>y</td><td>四位数表示的年</td><td>2001</td></tr>
<tr><td>M</td><td>一年中的月</td><td>July 或 07</td></tr>
<tr><td>d</td><td>一月中的第几天</td><td>10</td></tr>
<tr><td>h</td><td>带有 A.M./P.M. 的小时（1~12）</td><td>12</td></tr>
<tr><td>H</td><td>一天中的第几小时（0~23）</td><td>22</td></tr>
<tr><td>m</td><td>一小时中的第几分</td><td>30</td></tr>
<tr><td>s</td><td>一分中的第几秒</td><td>55</td></tr>
<tr><td>S</td><td>毫秒</td><td>234</td></tr>
<tr><td>E</td><td>一周中的星期几</td><td>Tuesday</td></tr>
<tr><td>D</td><td>一年中的第几天</td><td>360</td></tr>
<tr><td>F</td><td>所在的周是这个月的第几周</td><td>2 (second Wed. in July)</td></tr>
<tr><td>w</td><td>一年中的第几周</td><td>40</td></tr>
<tr><td>W</td><td>一月中的第几周</td><td>1</td></tr>
<tr><td>a</td><td> A.M./P.M. 标记</td><td>PM</td></tr>
<tr><td>k</td><td>一天中的第几小时（1~24）</td><td>24</td></tr>
<tr><td>K</td><td>带有 A.M./P.M. 的小时（0~11）</td><td>10</td></tr>
<tr><td>z</td><td>时区</td><td>Eastern Standard Time</td></tr>
<tr><td>'</td><td>Escape for text</td><td>Delimiter</td></tr>
<tr><td>"</td><td>单引号</td><td>`</td></tr>
</table> 

如需查看可用的处理日期方法的完整列表，你可以参考标准的 Java 文档。
