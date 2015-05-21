# 客户端 HTTP 请求

当浏览器请求网页时，它会向 web 服务器发送大量信息，这些信息不能被直接读取，因为这些信息是作为 HTTP 请求头的一部分行进的。关于这点你可以查看 [HTTP 协议](http://www.tutorialspoint.com/http/index.htm) 来了解更多的信息。

以下是来自浏览器端的重要的头信息，你会在 web 编程中频繁的使用：

<table class="table table-bordered">
<tr><th style="width:30%">头信息</th><th>描述</th></tr>
<tr><td>Accept</td><td>这个头信息指定浏览器或其他客户端可以处理的 MIME 类型。值 <b>image/png</b> 或 <b>image/jpeg</b> 是最常见的两种可能值。</td></tr>
<tr><td>Accept-Charset</td><td>这个头信息指定浏览器可以用来显示信息的字符集。例如 ISO-8859-1。</td></tr>
<tr><td>Accept-Encoding</td><td>这个头信息指定浏览器知道如何处理的编码类型。值 <b>gzip</b> 或 <b>compress</b> 是最常见的两种可能值。</td></tr>
<tr><td>Accept-Language</td><td>这个头信息指定客户端的首选语言，在这种情况下，Servlet 会产生多种语言的结果。例如，en、en-us、ru 等。</td></tr>
<tr><td>Authorization</td><td>这个头信息用于客户端在访问受密码保护的网页时识别自己的身份。</td></tr>
<tr><td>Connection</td><td>这个头信息指示客户端是否可以处理持久 HTTP 连接。持久连接允许客户端或其他浏览器通过单个请求来检索多个文件。值 <b>Keep-Alive</b> 意味着使用了持续连接。</td></tr>
<tr><td>Content-Length</td><td>这个头信息只适用于 POST 请求，并给出 POST 数据的大小（以字节为单位）。</td></tr>
<tr><td>Cookie</td><td>这个头信息把之前发送到浏览器的 cookies 返回到服务器。</td></tr>
<tr><td>Host</td><td>这个头信息指定原始的 URL 中的主机和端口。</td></tr>
<tr><td>If-Modified-Since</td><td>这个头信息表示只有当页面在指定的日期后已更改时，客户端想要的页面。如果没有新的结果可以使用，服务器会发送一个 304 代码，表示 <b>Not Modified</b> 头信息。</td></tr>
<tr><td>If-Unmodified-Since</td><td>这个头信息是 If-Modified-Since 的对立面，它指定只有当文档早于指定日期时，操作才会成功。</td></tr>
<tr><td>Referer</td><td>这个头信息指示所指向的 Web 页的 URL。例如，如果您在网页 1，点击一个链接到网页 2，当浏览器请求网页 2 时，网页 1 的 URL 就会包含在 Referer 头信息中。</td></tr>
<tr><td>User-Agent</td><td>这个头信息识别发出请求的浏览器或其他客户端，并可以向不同类型的浏览器返回不同的内容。</td></tr>
</table> 

## 读取 HTTP 头信息的方法：

下述方法可以用于读取 servlet 程序中的 HTTP 头信息。通过 *HttpServletRequest* 对象这些方法是可用的。

<table class="table table-bordered">
<tr><th style="width:5%">序号</th><th>方法 &amp; 描述</th></tr>
<tr><td>1</td><td><p><b>Cookie[] getCookies()</b></p>
<p>返回一个数组，包含客户端发送该请求的所有的 Cookie 对象。</p></td></tr>
<tr><td>2</td><td><p><b>Enumeration getAttributeNames()</b></p>
<p>返回一个枚举，包含提供给该请求可用的属性名称。</p></td></tr>
<tr><td>3</td><td><p><b>Enumeration getHeaderNames()</b></p>
<p>返回一个枚举，包含在该请求中包含的所有的头名。</p></td></tr>
<tr><td>4</td><td><p><b>Enumeration getParameterNames()</b></p>
<p>返回一个 String 对象的枚举，包含在该请求中包含的参数的名称。</p></td></tr>
<tr><td>5</td><td><p><b>HttpSession getSession()</b></p>
<p>返回与该请求关联的当前 session 会话，或者如果请求没有 session 会话，则创建一个。</p></td></tr>
<tr><td>6</td><td><p><b>HttpSession getSession(boolean create)</b></p>
<p>返回与该请求关联的当前 HttpSession，或者如果没有当前会话，且创建是真的，则返回一个新的 session 会话。</p></td></tr>
<tr><td>7</td><td><p><b>Locale getLocale()</b></p>
<p>基于 Accept-Language 头，返回客户端接受内容的首选的区域设置。</p></td></tr>
<tr><td>8</td><td><p><b>Object getAttribute(String name)</b></p>
<p>以对象形式返回已命名属性的值，如果没有给定名称的属性存在，则返回 null。</p></td></tr>
<tr><td>9</td><td><p><b>ServletInputStream getInputStream()</b></p>
<p>使用 ServletInputStream，以二进制数据形式检索请求的主体。</p></td></tr>
<tr><td>10</td><td><p><b>String getAuthType()</b></p>
<p>返回用于保护 Servlet 的身份验证方案的名称，例如，“BASIC” 或 “SSL”，如果JSP没有受到保护则返回 null。</p></td></tr>
<tr><td>11</td><td><p><b>String getCharacterEncoding()</b></p>
<p>返回请求主体中使用的字符编码的名称。</p></td></tr>
<tr><td>12</td><td><p><b>String getContentType()</b></p>
<p>返回请求主体的 MIME 类型，如果不知道类型则返回 null。</p></td></tr>
<tr><td>13</td><td><p><b>String getContextPath()</b></p>
<p>返回指示请求上下文的请求 URI 部分。</p></td></tr>
<tr><td>14</td><td><p><b>String getHeader(String name)</b></p>
<p>以字符串形式返回指定的请求头的值。</p></td></tr>
<tr><td>15</td><td><p><b>String getMethod()</b></p>
<p>返回请求的 HTTP 方法的名称，例如，GET、POST 或 PUT。</p></td></tr>
<tr><td>16</td><td><p><b>String getParameter(String name)</b></p>
<p>以字符串形式返回请求参数的值，或者如果参数不存在则返回 null。</p></td></tr>
<tr><td>17</td><td><p><b>String getPathInfo()</b></p>
<p>当请求发出时，返回与客户端发送的 URL 相关的任何额外的路径信息。</p></td></tr>
<tr><td>18</td><td><p><b>String getProtocol()</b></p>
<p>返回请求协议的名称和版本。</p></td></tr>
<tr><td>19</td><td><p><b>String getQueryString()</b></p>
<p>返回包含在路径后的请求 URL 中的查询字符串。</p></td></tr>
<tr><td>20</td><td><p><b>String getRemoteAddr()</b></p>
<p>返回发送请求的客户端的互联网协议（IP）地址。</p></td></tr>
<tr><td>21</td><td><p><b>String getRemoteHost()</b></p>
<p>返回发送请求的客户端的完全限定名称。</p></td></tr>
<tr><td>22</td><td><p><b>String getRemoteUser()</b></p>
<p>如果用户已通过身份验证，则返回发出请求的登录用户，或者如果用户未通过身份验证，则返回 null。</p></td></tr>
<tr><td>23</td><td><p><b>String getRequestURI()</b></p>
<p>从协议名称直到 HTTP 请求的第一行的查询字符串中，返回该请求的 URL 的一部分。</p></td></tr>
<tr><td>24</td><td><p><b>String getRequestedSessionId()</b></p>
<p>返回由客户端指定的 session 会话 ID。</p></td></tr>
<tr><td>25</td><td><p><b>String getServletPath()</b></p>
<p>返回调用 JSP 的请求的 URL 的一部分。</p></td></tr>
<tr><td>26</td><td><p><b>String[] getParameterValues(String name)</b></p>
<p>返回一个字符串对象的数组，包含所有给定的请求参数的值，如果参数不存在则返回 null。</p></td></tr>
<tr><td>27</td><td><p><b>boolean isSecure()</b></p>
<p>返回一个布尔值，指示请求是否使用安全通道，如 HTTPS。</p></td></tr>
<tr><td>28</td><td><p><b>int getContentLength()</b></p>
<p>以字节为单位返回请求主体的长度，并提供输入流，或者如果长度未知则返回 -1。</p></td></tr>
<tr><td>29</td><td><p><b>int getIntHeader(String name)</b></p>
<p>返回指定的请求头的值为一个 int 值。</p></td></tr>
<tr><td>30</td><td><p><b>int getServerPort()</b></p>
<p>返回接收到这个请求的端口号。</p></td></tr>
</table> 

## HTTP 头信息请求实例：

下述例子使用了 HttpServletRequest 的 **getHeaderNames()** 方法来读取 HTTP 头信息。该方法返回了一个枚举，包含与当前的 HTTP 请求相关的头信息。

一旦我们得到一个枚举，我们可以以标准方式循环这个枚举，使用 hasMoreElements() 方法来确定何时停止循环，使用 *nextElement()* 方法来获取每个参数的名称。

``` 
// Import required java libraries
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import java.util.*;
// Extend HttpServlet class
public class DisplayHeader extends HttpServlet {
  // Method to handle GET method request.
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // Set response content type
      response.setContentType("text/html"); 
      PrintWriter out = response.getWriter();
	  String title = "HTTP Header Request Example";
      String docType =
      "<!doctype html public \"-//w3c//dtd html 4.0 " +
      "transitional//en\">\n";
      out.println(docType +
        "<html>\n" +
        "<head><title>" + title + "</title></head>\n"+
        "<body bgcolor=\"#f0f0f0\">\n" +
        "<h1 align=\"center\">" + title + "</h1>\n" +
        "<table width=\"100%\" border=\"1\" align=\"center\">\n" +
        "<tr bgcolor=\"#949494\">\n" +
        "<th>Header Name</th><th>Header Value(s)</th>\n"+
        "</tr>\n"); 
      Enumeration headerNames = request.getHeaderNames();      
      while(headerNames.hasMoreElements()) {
         String paramName = (String)headerNames.nextElement();
         out.print("<tr><td>" + paramName + "</td>\n");
         String paramValue = request.getHeader(paramName);
         out.println("<td> " + paramValue + "</td></tr>\n");
      }
      out.println("</table>\n</body></html>");
  }
  // Method to handle POST method request.
  public void doPost(HttpServletRequest request,
                     HttpServletResponse response)
      throws ServletException, IOException {
     doGet(request, response);
  }
}
```

现在，调用上述 servlet 会产生如下所示的结果：

<pre class="result notranslate">
<h1 align="center">HTTP Header Request Example</h1>
<table width="100%" border="1" align="center" class="table table-bordered">
<tr bgcolor="#949494">
<th width="30%">Header Name</th><th>Header Value(s)</th>
</tr>
<tr><td>accept</td>
<td>*/*</td></tr>
<tr><td>accept-language</td>
<td>en-us</td></tr>
<tr><td>user-agent</td>
<td>Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0; InfoPath.2; MS-RTC LM 8)</td></tr>
<tr><td>accept-encoding</td>
<td>gzip, deflate</td></tr>
<tr><td>host</td>
<td>localhost:8080</td></tr>
<tr><td>connection</td>
<td>Keep-Alive</td></tr>
<tr><td>cache-control</td>
<td>no-cache</td></tr>
</table>
</pre>


