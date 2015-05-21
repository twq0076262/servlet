# HTTP 状态码

HTTP 请求的格式和 HTTP 响应消息的格式是相似的且都有如下所示结构：

- 一个初始状态行 + CRLF(回车 + 换行 即新行)

- 零个或多个标题行 + CRLF

- 一个空白行，即一个 CRLF

- 一个可选的消息主体，如文件、查询数据或查询输出

例如，服务器的响应头信息看起来如下所示：

<pre class="prettyprint notranslate">
HTTP/1.1 200 OK
Content-Type: text/html
Header2: ...
...
HeaderN: ...
  (Blank Line)
&lt;!doctype ...&gt;
&lt;html&gt;
&lt;head&gt;...&lt;/head&gt;
&lt;body&gt;
...
&lt;/body&gt;
&lt;/html&gt;
</pre>


状态行包括 HTTP 版本（例子中的 HTTP/1.1）、一个状态码（例子中的 200）和一个对应于状态码的短消息（例子中的 OK）。

以下 HTTP 状态码以及可能从 Web 服务器返回的相关的消息的列表：

<table class="table table-bordered">
  <tr>
    <th align="left" style="width:10%">代码：</th>
    <th align="left" style="width:30%">消息：</th>
    <th align="left" style="width:60%">描述：</th>
  </tr>
  <tr>
    <td valign="top">100</td><td> Continue</td>
    <td valign="top">只有请求的一部分已经被服务器接收，但只要它没有被拒绝，客户端应继续该请求。</td>
  </tr>
  <tr>
    <td valign="top">101</td><td> Switching Protocols</td>
    <td valign="top">服务器切换协议。</td>
  </tr>
  <tr>
    <td valign="top">200</td><td> OK</td>
    <td valign="top">请求成功。</td>
  </tr>
  <tr>
    <td valign="top">201</td><td> Created</td>
    <td valign="top">该请求是完整的，并创建一个新的资源。</td>
  </tr>
  <tr>
    <td valign="top">202</td><td> Accepted</td>
    <td valign="top">该请求被接受处理，但是该处理是不完整的。</td>
  </tr>
  <tr>
    <td valign="top">203</td><td style="width:36%;"> Non-authoritative Information</td>
    <td valign="top">&nbsp;</td>
  </tr>
  <tr>
    <td valign="top">204 </td><td>No Content</td>
    <td valign="top">&nbsp; </td>
  </tr>
  <tr>
    <td valign="top">205</td><td> Reset Content</td>
    <td valign="top">&nbsp;</td>
  </tr>
  <tr>
    <td valign="top">206</td><td> Partial Content</td>
    <td valign="top">&nbsp; </td>
  </tr>
  <tr>
    <td valign="top">300</td><td> Multiple Choices</td>
    <td valign="top">链接列表。用户可以选择一个链接，进入到该位置。最多五个地址</td>
  </tr>
  <tr>
    <td valign="top">301</td><td> Moved Permanently</td>
    <td valign="top">所请求的页面已经转移到一个新的 URL。</td>
  </tr>
  <tr>
    <td valign="top">302</td><td> Found</td>
    <td valign="top">所请求的页面已经临时转移到一个新的 URL。</td>
  </tr>
  <tr>
    <td valign="top">303</td><td> See Other</td>
    <td valign="top">所请求的页面可以在另一个不同的 URL 下被找到。</td>
  </tr>
  <tr>
    <td valign="top">304</td><td> Not Modified</td>
    <td valign="top">&nbsp;</td>
  </tr>
  <tr>
    <td valign="top">305</td><td> Use Proxy</td>
    <td valign="top">&nbsp;
    </td>
  </tr>
  <tr>
    <td valign="top">306</td><td> <i>Unused</i></td>
 
    <td valign="top">在以前的版本中使用该代码。现在已不再使用它，但代码仍被保留。</td>
  </tr>
  <tr>
    <td valign="top">307</td><td> Temporary Redirect</td>
    <td valign="top">所请求的页面已经临时转移到一个新的 URL。</td>
  </tr>
   <tr>
    <td valign="top">400</td><td>Bad Request</td>
    <td valign="top">服务器不理解请求。</td>
  </tr>
  <tr>
    <td valign="top">401</td><td> Unauthorized</td>
    <td valign="top">所请求的页面需要用户名和密码。</td>
  </tr>
  <tr>
    <td valign="top">402</td><td> Payment Required</td>
    <td valign="top"><i>你还不能使用该代码。</i> </td>
  </tr>
  <tr>
    <td valign="top">403</td><td> Forbidden</td>
    <td valign="top">禁止访问所请求的页面。</td>
  </tr>
  <tr>
    <td valign="top">404</td><td> Not Found</td>
    <td valign="top">服务器无法找到所请求的页面。</td>
  </tr>
  <tr>
    <td valign="top">405</td><td> Method Not Allowed</td> 
    <td valign="top">在请求中指定的方法是不允许的。</td>
  </tr>
  <tr>
    <td valign="top">406</td><td> Not Acceptable</td>
    <td valign="top">服务器只生成一个不被客户端接受的响应。</td>
  </tr>
  <tr>
    <td valign="top">407 </td><td>Proxy Authentication Required</td>
    <td valign="top">在请求送达之前，您必须使用代理服务器的验证。</td>
  </tr>
  <tr>
    <td valign="top">408</td><td> Request Timeout</td>
    <td valign="top">请求需要的时间比服务器能够等待的时间长，超时。</td>
  </tr>
  <tr>
    <td valign="top">409</td><td> Conflict</td>
    <td valign="top">请求因为冲突无法完成。</td>
  </tr>
  <tr>
    <td valign="top">410 </td><td>Gone</td>
    <td valign="top">所请求的页面不再可用。</td>
  </tr>
  <tr>
    <td valign="top">411</td><td> Length Required</td>
    <td valign="top">&quot;Content-Length&quot; 未定义。服务器无法处理客户端发送的不带 Content-Length 的请求信息。</td>
  </tr>
  <tr>
    <td valign="top">412</td><td> Precondition Failed</td>
    <td valign="top">请求中给出的先决条件被服务器评估为 false。</td>
  </tr>
  <tr>
    <td valign="top">413</td><td> Request Entity Too Large</td>
    <td valign="top">服务器不接受该请求，因为请求实体过大。</td>
  </tr>
  <tr>
    <td valign="top">414</td><td> Request-url Too Long</td>
    <td valign="top">服务器不接受该请求，因为 URL 太长。当你转换一个 “post” 请求为一个带有长的查询信息的 “get” 请求时发生。</td>
  </tr>
  <tr>
    <td valign="top">415</td><td> Unsupported Media Type</td>
    <td valign="top">服务器不接受该请求，因为媒体类型不被支持。</td>
  </tr>
  <tr>
    <td valign="top">417</td><td> Expectation Failed</td>
    <td valign="top">&nbsp;</td> 
  </tr>
  <tr>
    <td valign="top">500</td><td>Internal Server Error</td>
    <td valign="top">未完成的请求。服务器遇到了一个意外的情况。</td>
  </tr>
  <tr>
    <td valign="top">501</td><td> Not Implemented</td>
    <td valign="top">未完成的请求。服务器不支持所需的功能。</td>
  </tr>
  <tr>
    <td valign="top">502</td><td> Bad Gateway</td>
    <td valign="top">未完成的请求。服务器从上游服务器收到无效响应。</td>
  </tr>
  <tr>
    <td valign="top">503</td><td> Service Unavailable</td>
    <td valign="top">未完成的请求。服务器暂时超载或死机。</td>
  </tr>
  <tr>
    <td valign="top">504</td><td> Gateway Timeout</td>
    <td valign="top"> 	网关超时。</td>
  </tr>
  <tr>
    <td valign="top">505</td><td> HTTP Version Not Supported</td>
    <td valign="top">服务器不支持“HTTP协议”版本。</td>
  </tr>
</table> 

## 设置 HTTP 状态码的方法：

下面是在 servlet 程序中可以用于设置 HTTP 状态码的方法。通过 *HttpServletResponse* 对象这些方法是可用的。

<table class="table table-bordered">
<tr><th style="width:5%">序号	</th><th>方法&amp;描述</th></tr>
<tr><td>1</td><td><p><b>public void setStatus ( int statusCode )</b></p>
<p>该方法设置一个任意的状态码。setStatus 方法接受一个 int（状态码）作为参数。如果您的反应包含了一个特殊的状态码和文档，请确保在使用 <i>PrintWriter</i> 实际返回任何内容之前调用 setStatus。</p></td></tr>
<tr><td>2</td><td><p><b>public void sendRedirect(String url)</b></p>
<p>该方法生成一个 302 响应，连同一个带有新文档 URL 的 <i>Location</i>  头。</p></td></tr>
<tr><td>3</td><td><p><b>public void sendError(int code, String message)</b></p>
<p>该方法发送一个状态码（通常为 404），连同一个在 HTML 文档内部自动格式化并发送到客户端的短消息。</p></td></tr>
</table> 

## HTTP 状态码实例：

下述例子将发送 407 错误代码到客户端浏览器，且浏览器会向你显示 “需要身份验证！！！”的消息。

```
// Import required java libraries
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import java.util.*;
// Extend HttpServlet class
public class showError extends HttpServlet { 
  // Method to handle GET method request.
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // Set error code and reason.
      response.sendError(407, "Need authentication!!!" );
  }
  // Method to handle POST method request.
  public void doPost(HttpServletRequest request,
                     HttpServletResponse response)
      throws ServletException, IOException {
     doGet(request, response);
  }
}
```

现在调用上述 servlet 会显示如下所示结果：

<pre class="result notranslate">
<h1 style="font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;">HTTP Status 407 - Need authentication!!!</h1>
<p><b style="font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;">type</b> Status report</p>
<p><b style="font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;">message</b><u>Need authentication!!!</u></p>
<p><b style="font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;">description</b><u>The client must first authenticate itself with the proxy (Need authentication!!!).</u></p>
<h3 style="font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;">Apache Tomcat/5.5.29</h3>
</pre>
<hr />



