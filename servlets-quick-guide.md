# Servlets——快速指南

## Servlets 是什么？ 

Java servlet 是运行在 Web 或应用服务器上的程序，作为在来自 Web 浏览器或其他 HTTP 客户机的请求和在 HTTP 服务器上的数据库或应用程序的中间层。 

使用 Servlet，你可以通过 Web 页面表单来收集用户的输入，显示从数据库或其他来源的记录，动态地创建 Web 页面。 

Java servlet 通常服务于使用 Common Gateway Interface (CGI) 实现的同样的目的程序。但与 CGI 相比，Servlet 具有几个优点。

- 性能更好。

- Servlet 在 Web 服务器的地址空间内执行。没有必要创建一个单独的进程来处理每个客户端请求。

- 由于 Servlet 是用 Java 编写的，所以它是跨平台的。

- 在服务器上的 Java 安全性管理器执行的一些限制来保护服务器上的资源。所以 servlet 是可信的。

- Java 类库的完整的功能是对 Servlet 来说是可用的。它可以与小应用程序、数据库或其他软件通过通信接口和你已经了解的RMI机制进行通信。

## Servlet 体系结构： 

以下图表显示了Servlet在Web应用程序中的位置。

![](../images/quick1.jpg)

## Servlet 包： 

Java Servlet 是运行在 Web 服务器上的 Java 类，在 Web 服务器上有一个支持 Java Servlet 规范的解释器。 

Servlet 可以使用 **javax.servlet** 和 **javax.servlet.http** 包来创建。它们是 Java 企业版的一个标准部分，也是支持大型开发项目的 Java 类库的扩展版。

这些类实现了 Java Servlet 和 JSP 规范。在写这篇教程的时候，使用的版本分别是 Java Servlet 2.5 和 JSP 2.5。 

就像任何其他 Java 类一样，Java Servlet 可以创建和编译。在安装 Servlet 包，并将它们添加到你的电脑的 Classpath 中之后，你可以使用 JDK 的 Java 编译器或其他任何当前编译器来编译 Servlet。 

## 设置 Java 开发工具包 

这一步涉及到下载 Java 软件开发工具包(SDK)的实现，并适当地设置 PATH 环境变量。 

你可以从 Sun 的 Java Servlet 网址 [http://java.sun.com/products/servlet/](http://java.sun.com/products/servlet/) 上下载 SDK。 

一旦你下载了 Java 实现，按照给定的说明来安装和配置设置。最后设置 PATH 和 JAVA_HOME 的环境变量来引用的包含 java 和 javac 的目录，通常分别为 java_install_dir/bin 和 java_install_dir。 

如果你运行的是 Windows 系统，并且在 C:\jdk1.5.0_20上安装SDK，你将把下列语句添加到 C:\autoexec.bat文件中。

``` 
set PATH=C:\jdk1.5.0_20\bin;%PATH%
set JAVA_HOME=C:\jdk1.5.0_20
```

或者，在 Windows NT / 2000 / XP 中，你也可以在我的电脑上单击右键，选择属性，然后是选择高级，再然后是选择环境变量。接下来，你将更新 PATH 值，并且按下 OK 按钮。

在 Unix(Solaris、Linux等等)上，如果在 /usr/local/jdk1.5.0_20 上安装 SDK，并且你使用 C shell 命令，你将把下列语句添加到 .cshrc 文件中。

``` 
setenv PATH /usr/local/jdk1.5.0_20/bin:$PATH
setenv JAVA_HOME /usr/local/jdk1.5.0_20
```

另外，如果你使用一个集成开发环境(IDE)，类似于 Borland JBuilder，Eclipse，IntelliJ IDEA 或 Sun ONE Studio，编译和运行一个简单的程序来确认 IDE 知道你安装了 Java。

## 设置 Web Server: Tomcat 

许多支持 Servlet 的 Web 服务器都能在市场上都可以找到。一些 Web 服务器是可以免费下载的，而 Tomcat 就是其中之一。

Apache Tomcat 是一个实现 Java Servlet 和 JavaServer Pages 技术的开源软件，并可以作为一个独立的服务器进行测试 Servlet 和可以与 Apache Web 服务器一起被集成。下面是在计算机上安装 Tomcat 的步骤:

- 从 [http://tomcat.apache.org/](http://tomcat.apache.org/) 上下载 Tomcat 最新的版本。

- 一旦你下载安装包，并且解压二进制的发行版本到一个方便的位置。例如在 windows 上的 C:\apache-tomcat-5.5.29 中，或在 Linux/Unix 上的 /usr/local/apache-tomcat-5.5.29 中，并且创建 CATALINA_HOME 的环境变量指向这些位置。

在 Windows 机器上，通过执行下列命令可以启动 Tomcat：

``` 
$CATALINA_HOME\bin\startup.bat
 or
 C:\apache-tomcat-5.5.29\bin\startup.bat
```

在 Unix (Solaris、Linux 等等)机器上，通过执行下列命令可以启动 Tomcat：

``` 
$CATALINA_HOME/bin/startup.sh
or
/usr/local/apache-tomcat-5.5.29/bin/startup.sh
```

启动后，可以通过访问 http://localhost:8080 / 来使用包含 Tomcat 的默认的 web 应用程序。如果一切都正常，那么它应该显示下面的结果：

![](../images/environment1.jpg)

关于配置和运行 Tomcat 的更多的信息可以在包含这个的文档中找到，也可以在 Tomcat 网址：http://tomcat.apache.org 中找到。

在 Windows 机器上，通过执行下列命令可以停止 Tomcat：

``` 
C:\apache-tomcat-5.5.29\bin\shutdown
```

在 Unix(Solaris、Linux 等等)机器上，通过执行下列命令可以停止 Tomcat：

``` 
/usr/local/apache-tomcat-5.5.29/bin/shutdown.sh
```

## 设置 CLASSPATH 

因为 Servlet 不是 Java 平台和标准版的一部分，你必须确保 Servlet 类被编译。 

如果你运行的是 Windows 系统，你需要把下列语句添加到 C:\autoexec.bat 文件中。

``` 
set CATALINA=C:\apache-tomcat-5.5.29
set CLASSPATH=%CATALINA%\common\lib\servlet-api.jar;%CLASSPATH%
```

或者，在 Windows NT / 2000 / XP 中，你也可以在我的电脑上单击右键，选择属性，然后是选择高级，再然后是选择环境变量。接下来，你将更新 PATH 值，并且按下 OK 按钮。

在 Unix(Solaris、Linux 等等)上，如果你使用 C shell 命令，你将把下列语句添加到 .cshrc 文件中。

``` 
setenv CATALINA=/usr/local/apache-tomcat-5.5.29
setenv CLASSPATH $CATALINA/common/lib/servlet-api.jar:$CLASSPATH
```

**注：**假设你的开发目录是 C:\ServletDevel(Windows) 或是 /usr/ServletDevel(Unix)，那么你将需要用上面添加的类似的方式在 CLASSPATH 中添加这些目录。 

## 关于 Hello World 的简单代码： 

下面是一个编写 Hello World 的 Servlet 示例的简单的源代码结构：

``` 
// Import required java libraries
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
// Extend HttpServlet class
public class HelloWorld extends HttpServlet {
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

## 编译 Servlet： 

让我们把上面的代码添加到 HelloWorld.java 文件中，并且把这个文件放在 C:\ServletDevel(Windows) 或 /usr/ServletDevel(Unix) 中，然后你将同样需要在 CLASSPATH 中添加这些目录。

假设你的环境正确地设置，进入 **ServletDevel** 目录并且编译 HelloWorld.java，如下所示:

``` 
$ javac HelloWorld.java
```

如果 Servlet 取决于其他库，你必须在你的 CLASSPATH 中同样引入这些JAR文件。我仅仅引入 servlet-api.jar 的 JAR 文件，因为在 Hello World 程序中我没有使用任何其他库。 

这个命令行中使用 Sun 的 Java 软件开发工具包(JDK)中内置的 javac 编译器。要使这个命令正常工作，你必须在 PATH 环境变量中引入你正在使用的 Java SDK 的位置。 

如果一切都正常，上面的编译将会在同一目录中产生 **HelloWorld.class** 文件。下一节将解释已经编译的 Servlet 将如何在产品中部署。 

## Servlet 开发： 

默认情况下，一个 servlet 应用程序位于路径 <Tomcat-installation-directory>/webapps/ROOT 中，而类文件位于 <Tomcat-installation-directory>/webapps/ROOT/WEB-INF/classes 中。 

如果你有一个类名为 **com.myorg.MyServlet** 的完全限定类，那么这个 Servlet 类必须放在 WEB-INF/classes/com/myorg/MyServlet.class 中。 

现在，让我们复制 HelloWorld.class 到 <Tomcat-installation-directory>/webapps/ROOT/WEB-INF/classes 中，并且在位于 <Tomcat-installation-directory>/webapps/ROOT/WEB-INF/中的 **web.xml** 中创建下列条目：

``` 
 	<servlet>
        <servlet-name>HelloWorld</servlet-name>
        <servlet-class>HelloWorld</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>HelloWorld</servlet-name>
        <url-pattern>/HelloWorld</url-pattern>
    </servlet-mapping>
```

在 web.xml 文件中，以上条目创建在有效的标签 <web-app>...</web-app> 内。可能在这个表中不同的条目已经使用，但没关系。 

你几乎已经完成了，现在让我们开始使用 <Tomcat-installation-directory>\bin\startup.bat (在 windows 上)或者 <Tomcat-installation-directory>/bin/startup.sh (在 Linux/Solaris 等等)来启动 Tomcat 服务器，最后在浏览器的地址栏中输入 **http://localhost:8080/HelloWorld**。如果一切都正常，你将获得以下结果：

![](../images/example1.jpg)

为了获得详细的 Servlet 信息，你可以从头开始学习完整的教程：[**Java Servlets**](http://www.tutorialspoint.com/servlets/index.htm)。
