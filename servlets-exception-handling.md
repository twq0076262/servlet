# Servlets——异常处理

当一个 servlet 抛出一个异常时，web 容器在使用了 exception-type 元素的 **web.xml** 中搜索与抛出异常类型相匹配的配置。

你必须在 web.xml 中使用 **error-page** 元素来指定对特定**异常**或 HTTP **状态码**作出相应的 servlet 调用。

## web.xml 配置：

假设，有一个 *ErrorHandler* 的 servelt 在任何已定义的异常或错误出现时被调用。以下是要在 web.xml 中创建的项。

<pre class="prettyprint notranslate">
&lt;!-- servlet definition --&gt;
&lt;servlet&gt;
        &lt;servlet-name&gt;ErrorHandler&lt;/servlet-name&gt;
        &lt;servlet-class&gt;ErrorHandler&lt;/servlet-class&gt;
&lt;/servlet&gt;
&lt;!-- servlet mappings --&gt;
&lt;servlet-mapping&gt;
        &lt;servlet-name&gt;ErrorHandler&lt;/servlet-name&gt;
        &lt;url-pattern&gt;/ErrorHandler&lt;/url-pattern&gt;
&lt;/servlet-mapping&gt;

&lt;!-- error-code related error pages --&gt;
&lt;error-page&gt;
    &lt;error-code&gt;404&lt;/error-code&gt;
    &lt;location&gt;/ErrorHandler&lt;/location&gt;
&lt;/error-page&gt;
&lt;error-page&gt;
    &lt;error-code&gt;403&lt;/error-code&gt;
    &lt;location&gt;/ErrorHandler&lt;/location&gt;
&lt;/error-page&gt;

&lt;!-- exception-type related error pages --&gt;
&lt;error-page&gt;
    &lt;exception-type&gt;
          javax.servlet.ServletException
    &lt;/exception-type &gt;
    &lt;location&gt;/ErrorHandler&lt;/location&gt;
&lt;/error-page&gt;

&lt;error-page&gt;
    &lt;exception-type&gt;java.io.IOException&lt;/exception-type &gt;
    &lt;location&gt;/ErrorHandler&lt;/location&gt;
&lt;/error-page&gt;
</pre>


如果你想对所有的异常有一个通用的错误处理程序，那么应该定义下面的 error-page，而不是为每个异常定义单独的 error-page 元素：

``` 
<error-page>
    <exception-type>java.lang.Throwable</exception-type >
    <location>/ErrorHandler</location>
</error-page>
```

以下是关于上面的 web.xml 异常处理要注意的点：

- Servelt ErrorHandler 与其他的 servelt 的定义方式一样，且在 web.xml 中进行配置。

- 如果有错误状态代码出现，不管为 404（未找到）或 403（禁止），则会调用 ErrorHandler 的 servlet。

- 如果 web 应用程序抛出 *ServletException* 或 *IOException*，那么 web 容器会调用 /ErrorHandler 的 Servlet。

- 你可以定义不同的错误处理程序来处理不同类型的错误或异常。上面的实例是非常通用的，希望你能通过实例理解基本的概念。

## 请求属性——错误/异常：

以下是错误处理的 servlet 可以访问的请求属性列表，用来分析错误/异常的性质。

<table class="table table-bordered">
<tr><th style="width:5%">序号</th><th>属性 &amp; 描述</th></tr>
<tr><td>1</td><td><p><b>javax.servlet.error.status_code</b></p>
<p>该属性给出状态码，状态码可被存储，并在存储为 java.lang.Integer 数据类型后可被分析。</p></td></tr>
<tr><td>2</td><td><p><b>javax.servlet.error.exception_type</b></p>
<p>该属性给出异常类型的信息，异常类型可被存储，并在存储为 java.lang.Class 数据类型后可被分析。</p></td></tr>
<tr><td>3</td><td><p><b>javax.servlet.error.message</b></p>
<p>该属性给出确切错误消息的信息，信息可被存储，并在存储为 java.lang.String 数据类型后可被分析。</p></td></tr>
<tr><td>4</td><td><p><b>javax.servlet.error.request_uri</b></p>
<p>该属性给出有关 URL 调用 Servlet 的信息，信息可被存储，并在存储为 java.lang.String 数据类型后可被分析。</p></td></tr>
<tr><td>5</td><td><p><b>javax.servlet.error.exception</b></p>
<p>该属性给出异常产生的信息，信息可被存储，并在存储为 java.lang.Throwable 数据类型后可被分析。</p></td></tr>
<tr><td>6</td><td><p><b>javax.servlet.error.servlet_name</b></p>
<p>该属性给出 servlet 的名称，名称可被存储，并在存储为 java.lang.String 数据类型后可被分析。</p></td></tr>
</table> 

## Servlet 错误处理程序实例：

以下是 Servlet 实例，将应对任何你所定义的错误或异常发生时的错误处理程序。

本实例让你对 Servlet 中的异常处理有基本的了解，你可以使用相同的概念编写更复杂的异常处理应用程序：

``` 
// Import required java libraries
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import java.util.*;
// Extend HttpServlet class
public class ErrorHandler extends HttpServlet {
  // Method to handle GET method request.
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // Analyze the servlet exception       
      Throwable throwable = (Throwable)
      request.getAttribute("javax.servlet.error.exception");
      Integer statusCode = (Integer)
      request.getAttribute("javax.servlet.error.status_code");
      String servletName = (String)
      request.getAttribute("javax.servlet.error.servlet_name");
      if (servletName == null){
         servletName = "Unknown";
      }
      String requestUri = (String)
      request.getAttribute("javax.servlet.error.request_uri");
      if (requestUri == null){
         requestUri = "Unknown";
      }
      // Set response content type
      response.setContentType("text/html");
      PrintWriter out = response.getWriter();
	  String title = "Error/Exception Information";
      String docType =
      "<!doctype html public \"-//w3c//dtd html 4.0 " +
      "transitional//en\">\n";
      out.println(docType +
        "<html>\n" +
        "<head><title>" + title + "</title></head>\n" +
        "<body bgcolor=\"#f0f0f0\">\n");
      if (throwable == null && statusCode == null){
         out.println("<h2>Error information is missing</h2>");
         out.println("Please return to the <a href=\"" + 
           response.encodeURL("http://localhost:8080/") + 
           "\">Home Page</a>.");
      }else if (statusCode != null){
         out.println("The status code : " + statusCode);
      }else{
         out.println("<h2>Error information</h2>");
         out.println("Servlet Name : " + servletName + 
                             "</br></br>");
         out.println("Exception Type : " + 
                             throwable.getClass( ).getName( ) + 
                             "</br></br>");
         out.println("The request URI: " + requestUri + 
                             "<br><br>");
         out.println("The exception message: " + 
                                 throwable.getMessage( ));
      }
      out.println("</body>");
      out.println("</html>");
  }
  // Method to handle POST method request.
  public void doPost(HttpServletRequest request,
                     HttpServletResponse response)
      throws ServletException, IOException {
     doGet(request, response);
  }
}
```

以通常的方式编译 **ErrorHandler.java**，把你的类文件放入<Tomcat-installation-directory>/webapps/ROOT/WEB-INF/classes 中。

让我们在 web.xml 文件中添加如下配置来处理异常：

<pre class="prettyprint notranslate">
&lt;servlet&gt;
        &lt;servlet-name&gt;ErrorHandler&lt;/servlet-name&gt;
        &lt;servlet-class&gt;ErrorHandler&lt;/servlet-class&gt;
&lt;/servlet&gt;
&lt;!-- servlet mappings --&gt;
&lt;servlet-mapping&gt;
        &lt;servlet-name&gt;ErrorHandler&lt;/servlet-name&gt;
        &lt;url-pattern&gt;/ErrorHandler&lt;/url-pattern&gt;
&lt;/servlet-mapping&gt;
&lt;error-page&gt;
    &lt;error-code&gt;404&lt;/error-code&gt;
    &lt;location&gt;/ErrorHandler&lt;/location&gt;
&lt;/error-page&gt;
&lt;error-page&gt;
    &lt;exception-type&gt;java.lang.Throwable&lt;/exception-type &gt;
    &lt;location&gt;/ErrorHandler&lt;/location&gt;
&lt;/error-page&gt;
</pre>

现在，尝试使用一个会产生异常的 servlet，或者输入一个错误的 URL，这将触发 Web 容器调用 **ErrorHandler** 的 servlet，并显示适当的消息。例如，如果你输入了一个错误的 URL，那么它将显示下面的结果：

<pre class="result notranslate">
The status code : 404
</pre>



上面的代码在某些 web 浏览器中可能无法正常工作。因此，请尽量尝试使用 Mozilla 和 Safari 浏览器。
