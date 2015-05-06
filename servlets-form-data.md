# Servlets——表单数据

当你需要从浏览器到 Web 服务器传递一些信息并最终传回到后台程序时，你会遇到许多情况。浏览器使用两种方法将这些信息传递到 Web 服务器。这些方法是 GET 方法和 POST 方法。

## GET 方法：

GET 方法向页面请求发送已编码的用户信息。页面和已编码的信息中间用 ? 字符分隔，如下所示：

``` 
http://www.test.com/hello?key1=value1&key2=value2
```

GET 方法是默认的从浏览器向 web 服务器传递信息的方法，它会产生一个很长的字符串，出现在浏览器的地址栏中。如果你要向服务器传递的是密码或其他的敏感信息，请不要使用 GET 方法。GET 方法有大小限制：请求字符串中最多只能有 1024 个字符。

这些信息使用 QUERY_STRING 头传递，并可以通过 QUERY_STRING 环境变量访问，Servlet 使用 **doGet()** 方法处理这种类型的请求。

## POST 方法：

一种向后台程序传递信息的更可靠的方法是 POST 方法。POST 方法打包信息的方式与 GET 方法基本相同，但是 POST 方法不是把信息作为 URL 中 ? 字符后的文本字符串进行发送，而是把这些信息作为一个单独的消息。消息以标准输出的形式传到后台程序，您可以解析和使用这些标准输出。Servlet 使用 **doPost()** 方法处理这种类型的请求。

## 使用 Servlet 读取表单数据：

Servlet 以自动解析的方式处理表单数据，根据不同的情况使用如下不同的方法：

- **getParameter()：**你可以调用 request.getParameter() 方法来获取表单参数的值。

- **getParameterValues()：**如果参数出现一次以上，则调用该方法，并返回多个值，例如复选框。

- **getParameterNames()：**如果你想要得到当前请求中的所有参数的完整列表，则调用该方法。

## 使用 URL 的 GET 方法实例：

下面是一个简单的 URL，将使用 GET 方法向 HelloForm 程序传递两个值。

**http://localhost:8080/HelloForm?first_name=ZARA&last_name=ALI**
下面是处理 Web 浏览器输入的 **HelloForm.java** Servlet 程序。我们将使用 **getParameter()** 方法，可以很容易地访问传递的信息：

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
      // Set response content type
      response.setContentType("text/html");
      PrintWriter out = response.getWriter();
	  String title = "Using GET Method to Read Form Data";
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

假设你的环境已经正确地设置，编译 HelloForm.java，如下所示：

``` 
$ javac HelloForm.java
```

如果一切顺利，上述编译会产生 **HelloForm.class** 文件。接下来，你就必须把该类文件复制到 <Tomcat-installation-directory>/webapps/ROOT/WEB-INF/classes 中，并在位于 <Tomcat-installation-directory>/webapps/ROOT/WEB-INF/ 的 **web.xml** 文件中创建以下条目：

``` 
	<servlet>
        <servlet-name>HelloForm</servlet-name>
        <servlet-class>HelloForm</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>HelloForm</servlet-name>
        <url-pattern>/HelloForm</url-pattern>
    </servlet-mapping>
```

现在在浏览器的地址栏中输入 *http://localhost:8080/HelloForm?first_name=ZARA&last_name=ALI*，并在触发上述命令之前确保已经启动 Tomcat 服务器。如果一切顺利，你会得到下面的结果：

<pre class="result notranslate">
<h1 align="center">Using GET Method to Read Form Data</h1>
<ul class="list">
  <li><b>First Name</b>: ZARA</li>
  <li><b>Last Name</b>: ALI</li>
</ul>
</pre>


## 使用表单的 GET 方法实例：

下面是一个简单的实例，使用 HTML 表单和提交按钮传递两个值。我们将使用相同的 Servlet HelloForm 来处理输入。

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


保存这个 HTML 到 hello.htm 文件中，并把它放在 <Tomcat-installation-directory>/webapps/ROOT 目录下。当你访问 *http://localhost:8080/Hello.htm* 时，下面是上面表单的实际输出。

<form action="javascript:void();" method="get" target="_blank">
First Name: <input type="text" name="first_name" />
Last Name: <input type="text" name="last_name" />
<input type="button" value="Submit" />
</form>


尝试输入名字和姓氏，然后点击“提交”按钮，在你本机上查看输出结果。基于所提供的输入，它会产生与上一个实例类似的结果。

## 使用表单的 POST 方法实例

让我们对上面的 Servlet 做小小的修改，以便它可以处理 GET 和 POST 方法。下面的 **HelloForm.java** Servlet 程序使用 GET 和 POST 方法处理由 Web 浏览器给出的输入。

``` 
// Import required java libraries
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
// Extend HttpServlet class
public class HelloForm extends HttpServlet {
  // Method to handle GET method request.
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // Set response content type
      response.setContentType("text/html");
      PrintWriter out = response.getWriter();
	  String title = "Using GET Method to Read Form Data";
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
  // Method to handle POST method request.
  public void doPost(HttpServletRequest request,
                     HttpServletResponse response)
      throws ServletException, IOException {
     doGet(request, response);
  }
}
```

现在，编译部署上述的 Servlet，并使用带有 POST 方法的 Hello.htm 进行测试，如下所示：

<pre class="prettyprint notranslate">
&lt;html&gt;
&lt;body&gt;
&lt;form action="HelloForm" method="POST"&gt;
First Name: &lt;input type="text" name="first_name"&gt;
&lt;br /&gt;
Last Name: &lt;input type="text" name="last_name" /&gt;
&lt;input type="submit" value="Submit" /&gt;
&lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;
</pre>


下面是上面表单的实际输出，尝试输入名字和姓氏，然后点击“提交”按钮，在你本机上查看输出结果。

<form action="javascript:void();" method="get" target="_blank">
First Name: <input type="text" name="first_name" />
Last Name: <input type="text" name="last_name" />
<input type="button" value="Submit" />
</form>


基于所提供的输入，它会产生与上一个实例类似的结果。

## 将复选框数据传递到 Servlet 程序

当需要选择一个以上的选项时，则使用复选框。

下面是一个 HTML 代码实例 CheckBox.htm，一个带有两个复选框的表单。

<pre class="prettyprint notranslate">
&lt;html&gt;
&lt;body&gt;
&lt;form action="CheckBox" method="POST" target="_blank"&gt;
&lt;input type="checkbox" name="maths" checked="checked" /&gt; Maths
&lt;input type="checkbox" name="physics"  /&gt; Physics
&lt;input type="checkbox" name="chemistry" checked="checked" /&gt; 
                                                Chemistry
&lt;input type="submit" value="Select Subject" /&gt;
&lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;
</pre>


这段代码的结果是下面的表单：

<form action="javascript:void();" method="get" target="_blank">
<input type="checkbox" name="maths" checked="checked" /> Maths
<input type="checkbox" name="physics" /> Physics
<input type="checkbox" name="chemistry" checked="checked" /> Chemistry
<input type="button" value="Select Subject" />
</form>


下面是 CheckBox.java Servlet 程序，处理 web 浏览器给出的复选框输入。

``` 
// Import required java libraries
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
// Extend HttpServlet class
public class CheckBox extends HttpServlet {
  // Method to handle GET method request.
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // Set response content type
      response.setContentType("text/html");
      PrintWriter out = response.getWriter();
	  String title = "Reading Checkbox Data";
      String docType =
      "<!doctype html public \"-//w3c//dtd html 4.0 " +
      "transitional//en\">\n";
      out.println(docType +
                "<html>\n" +
                "<head><title>" + title + "</title></head>\n" +
                "<body bgcolor=\"#f0f0f0\">\n" +
                "<h1 align=\"center\">" + title + "</h1>\n" +
                "<ul>\n" +
                "  <li><b>Maths Flag : </b>: "
                + request.getParameter("maths") + "\n" +
                "  <li><b>Physics Flag: </b>: "
                + request.getParameter("physics") + "\n" +
                "  <li><b>Chemistry Flag: </b>: "
                + request.getParameter("chemistry") + "\n" +
                "</ul>\n" +
                "</body></html>");
  }
  // Method to handle POST method request.
  public void doPost(HttpServletRequest request,
                     HttpServletResponse response)
      throws ServletException, IOException {
     doGet(request, response);
  }
}
```

上面的实例将显示下面的结果：

<pre class="result notranslate">
<h1 align="center">Reading Checkbox Data</h1>
<ul class="list">
  <li><b>Maths Flag : </b>: on</li>
  <li><b>Physics Flag: </b>: null</li>
  <li><b>Chemistry Flag: </b>: on</li>
</ul>
</pre>


## 读取所有的表单参数：

以下是通用的实例，使用 HttpServletRequest 的 **getParameterNames()** 方法读取所有可用的表单参数。该方法返回一个枚举，其中包含未指定顺序的参数名。

一旦我们有一个枚举，我们可以以标准方式循环枚举，使用 *hasMoreElements()* 方法来确定何时停止，使用 *nextElement()* 方法来获取每个参数的名称。

``` 
// Import required java libraries
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import java.util.*;
// Extend HttpServlet class
public class ReadParams extends HttpServlet {
  // Method to handle GET method request.
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // Set response content type
      response.setContentType("text/html");
      PrintWriter out = response.getWriter();
	  String title = "Reading All Form Parameters";
      String docType =
      "<!doctype html public \"-//w3c//dtd html 4.0 " +
      "transitional//en\">\n";
      out.println(docType +
        "<html>\n" +
        "<head><title>" + title + "</title></head>\n" +
        "<body bgcolor=\"#f0f0f0\">\n" +
        "<h1 align=\"center\">" + title + "</h1>\n" +
        "<table width=\"100%\" border=\"1\" align=\"center\">\n" +
        "<tr bgcolor=\"#949494\">\n" +
        "<th>Param Name</th><th>Param Value(s)</th>\n"+
        "</tr>\n");
      Enumeration paramNames = request.getParameterNames();      
      while(paramNames.hasMoreElements()) {
         String paramName = (String)paramNames.nextElement();
         out.print("<tr><td>" + paramName + "</td>\n<td>");
         String[] paramValues =
                request.getParameterValues(paramName);
         // Read single valued data
         if (paramValues.length == 1) {
           String paramValue = paramValues[0];
           if (paramValue.length() == 0)
             out.println("<i>No Value</i>");
           else
             out.println(paramValue);
         } else {
             // Read multiple valued data
             out.println("<ul>");
             for(int i=0; i < paramValues.length; i++) {
                out.println("<li>" + paramValues[i]);
             }
             out.println("</ul>");
         }
      }
      out.println("</tr>\n</table>\n</body></html>");
  }
  // Method to handle POST method request.
  public void doPost(HttpServletRequest request,
                     HttpServletResponse response)
      throws ServletException, IOException {
     doGet(request, response);
  }
}
```

现在，通过下面的表单尝试上面的 servlet：

<pre class="prettyprint notranslate">
&lt;html&gt;
&lt;body&gt;
&lt;form action="ReadParams" method="POST" target="_blank"&gt;
&lt;input type="checkbox" name="maths" checked="checked" /&gt; Maths
&lt;input type="checkbox" name="physics"  /&gt; Physics
&lt;input type="checkbox" name="chemistry" checked="checked" /&gt; Chem
&lt;input type="submit" value="Select Subject" /&gt;
&lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;
</pre>


现在使用上面的表单调用 servlet，将产生以下结果：

<pre class="result notranslate">
<h1 align="center">Reading All Form Parameters</h1>
<table width="100%" border="1" align="center" class="table table-bordered">
<tr bgcolor="#949494">
<th>Param Name</th><th>Param Value(s)</th>
</tr>
<tr><td>maths</td>
<td>on</td>
</tr>
<tr><td>chemistry</td>
<td>on</td>
</tr>
</table>
</pre>


你可以尝试使用上面的 servlet 来读取有其他对象的其他表单数据，比如文本框、单选按钮或下拉框等。
