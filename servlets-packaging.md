# Servlets——包

涉及到 WEB-INF 子目录的 Web 应用程序结构是所有的 Java web 应用程序的标准，并由 Servlet API 规范指定。给定一个顶级目录名 myapp，目录结构如下所示：

<pre class="prettyprint notranslate">
/myapp
    /images
    /WEB-INF
        /classes
        /lib
</pre>


WEB-INF 子目录中包含应用程序的部署描述符，名为 web.xml。所有的 HTML 文件都位于顶级目录 *myapp* 下。对于 admin 用户，你会发现 ROOT 目录是 myApp 的父目录。

## 创建包中的 Servlets：

WEB-INF/classes 目录包含了所有的 Servlet 类和其他类文件，类文件所在的目录结构与他们的包名称匹配。例如，如果你有一个完全合格的类名称 **com.myorg.MyServlet**，那么这个 servlet 类必须位于以下目录中：

``` 
/myapp/WEB-INF/classes/com/myorg/MyServlet.class
```

下面的例子创建包名为 *com.myorg* 的 MyServlet 类。

``` 
// Name your package
package com.myorg;  
// Import required java libraries
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
public class MyServlet extends HttpServlet {
  private String message;
  public void init() throws ServletException
  {
      // Do required initialization
      message = "Hello World";
  } 
  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // Set response content type
      response.setContentType("text/html");
      // Actual logic goes here.
      PrintWriter out = response.getWriter();
      out.println("<h1>" + message + "</h1>");
  }  
  public void destroy()
  {
      // do nothing.
  }
}
```

## 编译包中的 Servlets：

编译包中的类与编译其他的类没有什么大的不同。最简单的方法是让你的 java 文件保留完全限定路径，如上面提到的类，将被保留在 com.myorg 中。你还需要在 CLASSPATH 中添加该目录。

假设你的环境已正确设置，进入 **<Tomcat-installation-directory>/webapps/ROOT/WEB-INF/classes** 目录，并编译 MyServlet.java，如下所示：

``` 
$ javac MyServlet.java
```

如果 servlet 依赖于其他库，那么你必须在 CLASSPATH 中也要引用那些 JAR 文件。这里我只引用了 servlet-api.jar JAR 文件，因为我在 Hello World 程序中并没有使用任何其他库。

该命令行使用内置的 javac 编译器，它是 Sun Microsystems Java 软件开发工具包（JDK）附带的。为了让该命令正常工作，必须包括您在 PATH 环境变量中所使用的 Java SDK 的位置。

如果一切顺利，上述编译会在同一目录下生成 **MyServlet.class** 文件。下一节将解释如何把一个已编译的 Servlet 部署到生产中。

## Servlet 打包部署：

默认情况下，servlet 应用程序位于路径 <Tomcat-installation-directory>/webapps/ROOT 下，且类文件放在 <Tomcat-installation-directory>/webapps/ROOT/WEB-INF/classes 中。

如果你有一个完全合格的类名称 **com.myorg.MyServlet**，那么这个 servlet 类必须位于 WEB-INF/classes/com/myorg/MyServlet.class 中，你需要在位于 <Tomcat-installation-directory>/webapps/ROOT/WEB-INF/ 的 **web.xml** 文件中创建以下条目：

``` 
<servlet>
   <servlet-name>MyServlet</servlet-name>
   <servlet-class>com.myorg.MyServlet</servlet-class>
</servlet>
<servlet-mapping>
   <servlet-name>MyServlet</servlet-name>
   <url-pattern>/MyServlet</url-pattern>
</servlet-mapping>
```

上面的条目要被创建在 web.xml 文件中的 <web-app>...</web-app> 标签内。在该文件中可能已经有各种可用的条目，但不要在意。

到这里，你基本上已经完成了，现在让我们使用 <Tomcat-installation-directory>\bin\startup.bat（在 Windows 上）或 <Tomcat-installation-directory>/bin/startup.sh（在 Linux/Solaris 等上）启动 tomcat 服务器，最后在浏览器的地址栏中输入 **http://localhost:8080/MyServlet**。如果一切顺利，你会看到下面的结果：

<pre class="result notranslate">
<h1 align="center">Hello World</h1>
</pre>
