# Servlets——Session 跟踪

HTTP 是一种“无状态”协议，这意味着每次客户端检索网页时，客户端打开一个单独的连接到 Web 服务器，服务器会自动不保留之前客户端请求的任何记录。

但是仍然有以下三种方式来维持 web 客户端和 web 服务器之间的会话：

## Cookies：

一个 web 服务器可以分配一个唯一的会话 ID 作为每个 web 客户端的 cookie，对于客户端的后续请求可以使用接收到的 cookie 来识别。

这可能不是一个有效的方法，因为很多时候浏览器不支持 cookie，所以我们建议不要使用这种方式来维持会话。

## 隐藏的表单字段：

一个 web 服务器可以发送一个隐藏的 HTML 表单字段，以及一个唯一的会话 ID，如下所示：

``` 
<input type="hidden" name="sessionid" value="12345">
```

该条目意味着，当表单被提交时，指定的名称和值会被自动包含在 GET 或 POST 数据中。每次当 web 浏览器发送回请求时，session_id 值可以用于保持不同的 web 浏览器的跟踪。

这可能是一种保持会话跟踪的有效方式，但是点击常规的超文本链接（<A HREF...>）不会导致表单提交，因此隐藏的表单字段也不支持常规的会话跟踪。

## URL 重写：

你可以在每个 URL 末尾追加一些额外的数据来标识会话，服务器会把该会话标识符与已存储的有关会话的数据相关联。

例如，http://tutorialspoint.com/file.htm;sessionid=12345，会话标识符被附加为 sessionid=12345，标识符可被 web 服务器访问以识别客户端。

URL 重写是一种更好的维持会话的方式，它在浏览器不支持 cookie 时能够很好地工作，但是它的缺点是会动态生成每个 URL 来为页面分配一个会话 ID，即使是在很简单的静态 HTML 页面中也会如此。

## HttpSession 对象：

除了上述的三种方式，servlet 还提供了 HttpSession 接口，该接口提供了一种跨多个页面请求或访问网站时识别用户以及存储有关用户信息的方式。

Servlet 容器使用这个接口来创建一个 HTTP 客户端和 HTTP 服务器之间的会话。会话在一个指定的时间段内持续，跨多个连接或用户请求。

你可以通过调用 HttpServletRequest 的公共方法 **getSession()** 来获取 HttpSession 对象，如下所示：

``` 
HttpSession session = request.getSession();
```

你需要在向客户端发送任何文档内容之前调用 *request.getSession()*。下面总结了 HttpSession 对象中可用的几个重要的方法：

<table class="table table-bordered">
<tr><th style="width:5%">序号</th><th>方法 &amp; 描述</th></tr>
<tr><td>1</td><td><p><b>public Object getAttribute(String name)</b></p>
<p>该方法返回在该 session 会话中具有指定名称的对象，如果没有指定名称的对象，则返回 null。</p></td></tr>
<tr><td>2</td><td><p><b>public Enumeration getAttributeNames()</b></p>
<p>该方法返回 String 对象的枚举，String 对象包含所有绑定到该 session 会话的对象的名称。</p></td></tr>
<tr><td>3</td><td><p><b>public long getCreationTime()</b></p>
<p>该方法返回该 session 会话被创建的时间，自格林尼治标准时间 1970 年 1 月 1 日凌晨零点算起，以毫秒为单位。</p></td></tr>
<tr><td>4</td><td><p><b>public String getId()</b></p>
<p>该方法返回一个包含分配给该 session 会话的唯一标识符的字符串。</p></td></tr>
<tr><td>5</td><td><p><b>public long getLastAccessedTime()</b></p>
<p>该方法返回客户端最后一次发送与该 session 会话相关的请求的时间自格林尼治标准时间 1970 年 1 月 1 日凌晨零点算起，以毫秒为单位。</p></td></tr>
<tr><td>6</td><td><p><b>public int getMaxInactiveInterval()</b></p>
<p>该方法返回 Servlet 容器在客户端访问时保持 session 会话打开的最大时间间隔，以秒为单位。</p></td></tr>
<tr><td>7</td><td><p><b>public void invalidate()</b></p>
<p>该方法指示该 session 会话无效，并解除绑定到它上面的任何对象。</p></td></tr>
<tr><td>8</td><td><p><b>public boolean isNew(</b></p>
<p>如果客户端还不知道该 session 会话，或者如果客户选择不参入该 session 会话，则该方法返回 true。</p></td></tr>
<tr><td>9</td><td><p><b>public void removeAttribute(String name)</b></p>
<p>该方法将从该 session 会话移除指定名称的对象。</p></td></tr>
<tr><td>10</td><td><p><b>public void setAttribute(String name, Object value) </b></p>
<p>该方法使用指定的名称绑定一个对象到该 session 会话。</p></td></tr>
<tr><td>11</td><td><p><b>public void setMaxInactiveInterval(int interval)</b></p>
<p>该方法在 Servlet 容器指示该 session 会话无效之前，指定客户端请求之间的时间，以秒为单位。</p></td></tr>
</table> 

## Session 跟踪实例：

本实例说明了如何使用 HttpSession 对象获取会话创建时间和最后访问时间。如果不存在会话，我们将通过请求创建一个新的会话。

``` 
// Import required java libraries
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import java.util.*; 
// Extend HttpServlet class
public class SessionTrack extends HttpServlet { 
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // Create a session object if it is already not  created.
      HttpSession session = request.getSession(true);
      // Get session creation time.
      Date createTime = new Date(session.getCreationTime());
      // Get last access time of this web page.
      Date lastAccessTime = 
                        new Date(session.getLastAccessedTime());
      String title = "Welcome Back to my website";
      Integer visitCount = new Integer(0);
      String visitCountKey = new String("visitCount");
      String userIDKey = new String("userID");
      String userID = new String("ABCD");
      // Check if this is new comer on your web page.
      if (session.isNew()){
         title = "Welcome to my website";
         session.setAttribute(userIDKey, userID);
      } else {
         visitCount = (Integer)session.getAttribute(visitCountKey);
         visitCount = visitCount + 1;
         userID = (String)session.getAttribute(userIDKey);
      }
      session.setAttribute(visitCountKey,  visitCount);
      // Set response content type
      response.setContentType("text/html");
      PrintWriter out = response.getWriter();
      String docType =
      "<!doctype html public \"-//w3c//dtd html 4.0 " +
      "transitional//en\">\n";
      out.println(docType +
                "<html>\n" +
                "<head><title>" + title + "</title></head>\n" +
                "<body bgcolor=\"#f0f0f0\">\n" +
                "<h1 align=\"center\">" + title + "</h1>\n" +
                 "<h2 align=\"center\">Session Infomation</h2>\n" +
                "<table border=\"1\" align=\"center\">\n" +
                "<tr bgcolor=\"#949494\">\n" +
                "  <th>Session info</th><th>value</th></tr>\n" +
                "<tr>\n" +
                "  <td>id</td>\n" +
                "  <td>" + session.getId() + "</td></tr>\n" +
                "<tr>\n" +
                "  <td>Creation Time</td>\n" +
                "  <td>" + createTime + 
                "  </td></tr>\n" +
                "<tr>\n" +
                "  <td>Time of Last Access</td>\n" +
                "  <td>" + lastAccessTime + 
                "  </td></tr>\n" +
                "<tr>\n" +
                "  <td>User ID</td>\n" +
                "  <td>" + userID + 
                "  </td></tr>\n" +
                "<tr>\n" +
                "  <td>Number of visits</td>\n" +
                "  <td>" + visitCount + "</td></tr>\n" +
                "</table>\n" +
                "</body></html>");
  }
}
```

编译上面的 servlet **SessionTrack**，并在 web.xml 文件中创建适当的条目。在浏览器地址栏输入 *http://localhost:8080/SessionTrack*，当你第一次运行时将显示如下结果：

<pre class="result notranslate">
<h1 align="center">Welcome to my website</h1>
<h2 align="center">Session Infomation</h2>
<table cellpadding="5" class="table table-bordered">
<tr bgcolor="#949494">
  <th>Session info</th><th>value</th></tr>
<tr>
  <td>id</td>
  <td>0AE3EC93FF44E3C525B4351B77ABB2D5</td></tr>
<tr>
  <td>Creation Time</td>
  <td>Tue Jun 08 17:26:40 GMT+04:00 2010  </td></tr>
<tr>
  <td>Time of Last Access</td>
  <td>Tue Jun 08 17:26:40 GMT+04:00 2010  </td></tr>
<tr>
  <td>User ID</td>
  <td>ABCD  </td></tr>
<tr>
  <td>Number of visits</td>
  <td>0</td></tr>
</table>
</pre>


再次尝试运行相同的 servlet，它将显示如下结果：

<pre class="result notranslate">
<h1 align="center">Welcome Back to my website</h1>
<h2 align="center">Session Infomation</h2>
<table cellpadding="5" class="table table-bordered">
<tr bgcolor="#949494">
  <th>info type</th><th>value</th></tr>
<tr>
  <td>id</td>
  <td>0AE3EC93FF44E3C525B4351B77ABB2D5</td></tr>
<tr>
  <td>Creation Time</td>
  <td>Tue Jun 08 17:26:40 GMT+04:00 2010  </td></tr>
<tr>
  <td>Time of Last Access</td>
  <td>Tue Jun 08 17:26:40 GMT+04:00 2010  </td></tr>
<tr>
  <td>User ID</td>
  <td>ABCD  </td></tr>
<tr>
  <td>Number of visits</td>
  <td>1</td></tr>
</table>
</pre>


## 删除会话数据：

当你完成了一个用户的会话数据，你有以下几种选择：

- **移除一个特定的属性：**你可以调用 *public void removeAttribute(String name)* 方法来删除与特定的键相关联的值。

- **删除整个 session 会话：**你可以调用 *public void invalidate()* 方法来丢弃整个会话。

- **设置 session 会话过期时间：**你可以调用 *public void setMaxInactiveInterval(int interval)* 方法来单独设置会话超时。

- **注销用户：**如果使用的是支持 servlet 2.4 的服务器，你可以调用 **logout** 来注销 Web 服务器的客户端，并把属于所有用户的所有会话设置为无效。

- **web.xml 配置：**如果你使用的是 Tomcat，除了上述方法，你还可以在 web.xml 文件中配置会话超时，如下所示：

``` 
<session-config>
    <session-timeout>15</session-timeout>
  </session-config>
```

上面实例中的超时时间是以分钟为单位，将覆盖 Tomcat 中默认的 30 分钟超时时间。

Servlet 中的 getMaxInactiveInterval() 方法会返回会话的超时时间，以秒为单位。所以，如果在 web.xml 中配置会话超时时间为 15 分钟，那么 getMaxInactiveInterval() 会返回 900。
