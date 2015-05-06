# Servlets——概述

## 什么是 Servlets？

Java Servlets 是运行在 Web 服务器或应用服务器上的程序，它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和数据库或 HTTP 服务器上的应用程序之间的中间层。

使用 Servlet，你可以通过网页表单收集来自用户的输入，呈现来自数据库或者其他源的记录，还可以动态创建网页。

通常情况下，Java Servlets 与使用公共网关接口(CGI)实现的程序可以达到异曲同工的效果。但是相比于 CGI，Servlets 有以下几点优势：

- 性能明显更好。

- Servlets 在 Web 服务器的地址空间内执行。这样它就没有必要再创建一个单独的进程来处理每个客户端请求。

- Servlets 是独立于平台的，因为它们是用 Java 编写的。

- 服务器上的 Java 安全管理器执行了一系列限制来保护服务器计算机上的资源。因此，servlets 是可信的。

- Java 类库的全部功能对 servlet 来说都是可用的。它可以通过你已经了解的 sockets 和 RMI 机制与 applets、数据库或其他软件进行交互。

## Servlets 架构：

下图显示了在 Web 应用程序中 Servlets 的位置。

![](images/arch1.jpg)

## Servlets 任务：

Servlet 执行以下主要任务：

- 读取由客户端（浏览器）发送的显式数据。这包括网页上的 HTML 表单，或者也可以是来自 applet 或自定义的 HTTP 客户端程序的表单。

- 读取由客户端（浏览器）发送的隐式 HTTP 请求数据。这包括 cookies、媒体类型和浏览器能理解的压缩格式等等。

- 处理数据并生成结果。这个过程可能需要访问数据库，执行 RMI 或 CORBA 调用，调用 Web 服务，或者直接计算响应。

- 发送显式数据（即文档）到客户端（浏览器）。该文档可以以多种多样的格式被发送，包括文本文件（HTML 或 XML）、二进制文件（GIF 图像）、Excel 等。

- 发送隐式的 HTTP 响应到客户端（浏览器）。这包括告诉浏览器或其他客户端被返回的文档类型（例如 HTML），设置 cookies 和缓存参数，以及其他类似的任务。

## Servlets 包：

Java Servlets 是 Java 类，是通过带有支持 Java Servlet 规范的解释器的 web 服务器运行的。

Servlets 可以使用 **javax.servlet** 和 **javax.servlet.http** 包创建，它是 Java 企业版的标准组成部分，Java 企业版是支持大型开发项目的 Java 类库的扩展版本。

这些类实现了 Java Servlet 和 JSP 规范。在写本教程的时候，二者相应的版本分别是 Java Servlet 2.5 和 JSP 2.1。

Java Servlet 就像任何其他的 Java 类一样已经被创建和编译。在你安装 servlet 包并把它们添加到你的计算机上的 Classpath 类路径中之后，你就可以使用 JDK 的 Java 编译器或任何其他当前编译器来编译 servlets。

## 接下来的内容？

接下来，本教程会带你一步一步地设置你的环境，以便开始后续的 Servlet 使用。因此，请系紧安全带，随我们一起开始 Servlet 的学习之旅吧！相信你会很喜欢这个教程的。
