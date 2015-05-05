# Servlet——Cookies 处理

Cookies 是存储在客户端计算机上的文本文件，并保留了各种跟踪信息。Java Servlet 显然支持 HTTP Cookies。

识别返回用户包括三个步骤：

- 服务器脚本向浏览器发送一组 Cookies。例如：姓名、年龄或识别号码等。

- 浏览器将这些信息存储在本地计算机上，以备将来使用。

- 当下一次浏览器向 Web 服务器发送任何请求时，浏览器会把这些 Cookies 信息发送到服务器，服务器将使用这些信息来识别用户。

本章将向你讲解如何设置或重置 Cookies，如何访问它们，以及如何将它们删除。

## Cookie 剖析：

Cookies 通常设置在 HTTP 头信息中（虽然 JavaScript 也可以直接在浏览器上设置一个 Cookie）。设置 Cookie 的 servlet 会发送如下的头信息：

``` 
HTTP/1.1 200 OK
Date: Fri, 04 Feb 2000 21:03:38 GMT
Server: Apache/1.3.9 (UNIX) PHP/4.0b3
Set-Cookie: name=xyz; expires=Friday, 04-Feb-07 22:03:38 GMT; 
                 path=/; domain=tutorialspoint.com
Connection: close
Content-Type: text/html
```

正如你所看到的，Set-Cookie 头包含了一个名称值对、一个 GMT 日期、一个路径和一个域。名称和值会被 URL 编码。expires 字段是一个指令，告诉浏览器在给定的时间和日期之后“忘记”该 Cookie。

如果浏览器被配置为存储 Cookies，它将会保留此信息直到到期日期。如果用户的浏览器指向任何匹配该 Cookie 的路径和域的页面，它会重新发送 Cookie 到服务器。浏览器的头信息可能如下所示：

``` 
GET / HTTP/1.0
Connection: Keep-Alive
User-Agent: Mozilla/4.6 (X11; I; Linux 2.2.6-15apmac ppc)
Host: zink.demon.co.uk:1126
Accept: image/gif, */*
Accept-Encoding: gzip
Accept-Language: en
Accept-Charset: iso-8859-1,*,utf-8
Cookie: name=xyz
```

Servlet 就能够通过请求方法 request.getCookies() 访问 Cookie，该方法将返回一个 Cookie 对象的数组。

## Servlet Cookies 方法：

以下是在 Servlet 中操作 Cookies 时可使用的有用的方法列表。

<table class="table table-bordered">
<tr><th style="width:5%">序号</th><th>方法 &amp; 描述</th></tr>
<tr><td>1</td><td><p><b>public void setDomain(String pattern)</b></p>
<p>该方法设置 cookie 适用的域，例如 tutorialspoint.com。</p></td></tr>
<tr><td>2</td><td><p><b>public String getDomain()</b></p>
<p>该方法获取 cookie 适用的域，例如 tutorialspoint.com。</p></td></tr>
<tr><td>3</td><td><p><b>public void setMaxAge(int expiry)</b></p>
<p>该方法设置 cookie 过期的时间（以秒为单位）。如果不这样设置，cookie 只会在当前 session 会话中持续有效。</p></td></tr>
<tr><td>4</td><td><p><b>public int getMaxAge()</b></p>
<p>该方法返回 cookie 的最大生存周期（以秒为单位），默认情况下，-1 表示 cookie 将持续下去，直到浏览器关闭。</p></td></tr>
<tr><td>5</td><td><p><b>public String getName()</b></p>
<p>该方法返回 cookie 的名称。名称在创建后不能改变。</p></td></tr>
<tr><td>6</td><td><p><b>public void setValue(String newValue)</b></p>
<p>该方法设置与 cookie 关联的值。</p></td></tr>
<tr><td>7</td><td><p><b>public String getValue()</b></p>
<p>该方法获取与 cookie 关联的值。</p></td></tr>
<tr><td>8</td><td><p><b>public void setPath(String uri)</b></p>
<p>该方法设置 cookie 适用的路径。如果您不指定路径，与当前页面相同目录下的（包括子目录下的）所有 URL 都会返回 cookie。</p></td></tr>
<tr><td>9</td><td><p><b>public String getPath()</b></p>
<p>该方法获取 cookie 适用的路径。</p></td></tr>
<tr><td>10</td><td><p><b>public void setSecure(boolean flag)</b></p>
<p>该方法设置布尔值，表示 cookie 是否应该只在加密的（即 SSL）连接上发送。</p></td></tr>
<tr><td>11</td><td><p><b>public void setComment(String purpose)</b></p>
<p>该方法规定了描述 cookie 目的的注释。该注释在浏览器向用户呈现 cookie 时非常有用。</p></td></tr>
<tr><td>12</td><td><p><b>public String getComment()</b></p>
<p>该方法返回了描述 cookie 目的的注释，如果 cookie 没有注释则返回 null。</p></td></tr>
</table> 

## 通过 Servlet 设置 Cookies：

通过 Servlet 设置 cookies 包括三个步骤：

**(1) 创建一个 Cookie 对象：**你可以调用带有 cookie 名称和 cookie 值的 Cookie 构造函数，cookie 名称和 cookie 值都是字符串。

``` 
Cookie cookie = new Cookie("key","value");
```

请记住，无论是名字还是值，都不应该包含空格或以下任何字符：

``` 
[ ] ( ) = , " / ? @ : ;
```

**(2) 设置最大生存周期：**你可以使用 setMaxAge 方法来指定 cookie 能够保持有效的时间（以秒为单位）。下面将设置一个最长有效期为 24 小时的 cookie。

``` 
cookie.setMaxAge(60*60*24); 
```

**(3) 发送 Cookie 到 HTTP 响应头：**你可以使用 **response.addCookie** 来添加 HTTP 响应头中的 Cookies，如下所示：

``` 
response.addCookie(cookie);
```

## 实例：

让我们修改我们的[表单数据实例]( http://www.tutorialspoint.com/servlets/servlets-form-data.htm)，为名字和姓氏设置 cookies。

``` 
// Import required java libraries
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
// Extend HttpServlet class
public class HelloForm extends HttpServlet { 
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // Create cookies for first and last names.      
      Cookie firstName = new Cookie("first_name",
                      request.getParameter("first_name"));
      Cookie lastName = new Cookie("last_name",
                      request.getParameter("last_name"));
      // Set expiry date after 24 Hrs for both the cookies.
      firstName.setMaxAge(60*60*24); 
      lastName.setMaxAge(60*60*24); 
      // Add both the cookies in the response header.
      response.addCookie( firstName );
      response.addCookie( lastName );
      // Set response content type
      response.setContentType("text/html");
      PrintWriter out = response.getWriter();
      String title = "Setting Cookies Example";
      String docType =
      "<!doctype html public \"-//w3c//dtd html 4.0 " +
      "transitional//en\">\n";
      out.println(docType +
                "<html>\n" +
                "<head><title>" + title + "</title></head>\n" +
                "<body bgcolor=\"#f0f0f0\">\n" +
                "<h1 align=\"center\">" + title + "</h1>\n" +
                "<ul>\n" +
                "  <li><b>First Name</b>: "
                + request.getParameter("first_name") + "\n" +
                "  <li><b>Last Name</b>: "
                + request.getParameter("last_name") + "\n" +
                "</ul>\n" +
                "</body></html>");
  }
}
```

编译上面的 servlet **HelloForm**，并在 web.xml 文件中创建适当的条目，最后尝试下面的 HTML 页面来调用 servlet。

<pre class="prettyprint notranslate"> 
&lt;html&gt;
&lt;body&gt;
&lt;form action="HelloForm" method="GET"&gt;
First Name: &lt;input type="text" name="first_name"&gt;
&lt;br /&gt;
Last Name: &lt;input type="text" name="last_name" /&gt;
&lt;input type="submit" value="Submit" /&gt;
&lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;
</pre>


保存上面的 HTML 内容到文件 hello.htm 中，并把它放在 <Tomcat-installation-directory>/webapps/ROOT 目录中。当你访问 *http://localhost:8080/Hello.htm* 时，上面表单的实际输出如下所示：

<form action="javascript:void();" method="get" target="_blank"> 
First Name: <input type="text" name="first_name" />  <br /> 
 
Last Name: <input type="text" name="last_name" /> 
<input type="button" value="Submit" /> 
</form> 

尝试输入名字和姓氏，然后点击“提交”按钮，名字和姓氏将显示在屏幕上，同时会设置 firstName 和 lastName 这两个 cookies，当下次你按下提交按钮时，会将这两个 cookies 传回到服务器。

下一节会讲解如何在 Web 应用程序中访问这些 cookies。

## 通过 Servlet 读取 Cookies：

要读取 cookies，你需要通过调用 *HttpServletRequest* 的 **getCookies( )** 方法创建一个 *javax.servlet.http.Cookie* 对象的数组。然后循环遍历数组，并使用 getName() 和 getValue() 方法来访问每个 cookie 和关联的值。

## 实例：

让我们读取上面的实例中设置的 cookies：

``` 
// Import required java libraries
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
// Extend HttpServlet class
public class ReadCookies extends HttpServlet {
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      Cookie cookie = null;
	  Cookie[] cookies = null;
      // Get an array of Cookies associated with this domain
      cookies = request.getCookies();     
	  // Set response content type
      response.setContentType("text/html");
      PrintWriter out = response.getWriter();
      String title = "Reading Cookies Example";
      String docType =
      "<!doctype html public \"-//w3c//dtd html 4.0 " +
      "transitional//en\">\n";
      out.println(docType +
                "<html>\n" +
                "<head><title>" + title + "</title></head>\n" +
                "<body bgcolor=\"#f0f0f0\">\n" );
      if( cookies != null ){
         out.println("<h2> Found Cookies Name and Value</h2>");
         for (int i = 0; i < cookies.length; i++){
            cookie = cookies[i];
            out.print("Name : " + cookie.getName( ) + ",  ");
            out.print("Value: " + cookie.getValue( )+" <br/>");
         }
      }else{
          out.println(
            "<h2>No cookies founds</h2>");
      }
      out.println("</body>");
      out.println("</html>");
   }
}
```

编译上面的 servlet **ReadCookies**，并在 web.xml 文件中创建适当的条目。如果你已经设置了 first_name cookie 为 “John”，last_name cookie 为 “Player” ，尝试运行 *http://localhost:8080/ReadCookies*，将显示如下结果：

<pre class="result notranslate">
<h2> Found Cookies Name and Value</h2>
Name : first_name, Value: John <br />
Name : last_name,  Value: Player
</pre>


## 通过 Servlet 删除 Cookies：

删除 cookies 是非常简单的。如果你想删除一个 cookie，那么只需要按照以下三个步骤进行：

- 读取一个现有的 cookie，并把它存储在 Cookie 对象中。

- 使用 **setMaxAge()** 方法设置 cookie 的年龄为零，来删除现有的 cookie。

- 把这个 cookie 添加到响应头。

## 实例：

下面的例子将删除现有的名为 “first_name” 的 cookie，当你下次运行 ReadCookies 的 servlet 时，它会返回 first_name 为空值。

``` 
// Import required java libraries
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
// Extend HttpServlet class
public class DeleteCookies extends HttpServlet {
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      Cookie cookie = null;
	  Cookie[] cookies = null;
      // Get an array of Cookies associated with this domain
      cookies = request.getCookies();     
	  // Set response content type
      response.setContentType("text/html");
      PrintWriter out = response.getWriter();
      String title = "Delete Cookies Example";
      String docType =
      "<!doctype html public \"-//w3c//dtd html 4.0 " +
      "transitional//en\">\n";
      out.println(docType +
                "<html>\n" +
                "<head><title>" + title + "</title></head>\n" +
                "<body bgcolor=\"#f0f0f0\">\n" );
       if( cookies != null ){
         out.println("<h2> Cookies Name and Value</h2>");
         for (int i = 0; i < cookies.length; i++){
            cookie = cookies[i];
            if((cookie.getName( )).compareTo("first_name") == 0 ){
                 cookie.setMaxAge(0);
                 response.addCookie(cookie);
                 out.print("Deleted cookie : " + 
                              cookie.getName( ) + "<br/>");
            }
            out.print("Name : " + cookie.getName( ) + ",  ");
            out.print("Value: " + cookie.getValue( )+" <br/>");
         }
      }else{
          out.println(
            "<h2>No cookies founds</h2>");
      }
      out.println("</body>");
      out.println("</html>");
   }
}
```

编译上面的 servlet **DeleteCookies**，并在 web.xml 文件中创建适当的条目。现在运行 *http://localhost:8080/DeleteCookies*，将显示如下结果：

<pre class="result notranslate">
<h2>Cookies Name and Value</h2>
<p>Deleted cookie : first_name</p>
<p>Name : first_name, Value: John</p>
<p>Name : last_name,  Value: Player</p>
</pre>


现在尝试运行 *http://localhost:8080/ReadCookies*，它将只显示一个 cookie，如下所示：

<pre class="result notranslate">
<h2> Found Cookies Name and Value</h2>
Name : last_name,  Value: Player
</pre>


你可以手动在 Internet Explorer 中删除 Cookies。在“工具”菜单，选择“Internet 选项”。如果要删除所有的 cookies，请按“删除 Cookies”。
