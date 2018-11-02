[第一个Servlet ](#top) <span id="top"></span>  	:maple_leaf:
-----
`JSP的底层其实就是 Servlet 现在我们来跑一个Servlet 吧` [`servlet 文档`](https://docs.oracle.com/cd/E17802_01/products/products/servlet/2.1/api/packages.html)

- [x] [`servlet 结构类图`](#servletapi)
- [x] [`Servlet 接口`](#interface)
- [x] [`ServletRequest 接口`](#servletrequest)
- [x] [`ServletResponse 接口`](#servletresponse)
- [x] [`ServletConfig 接口`](#servletconfig)
- [x] [`GenericServlet 类`](#genericservlet)
- [x] [`HttpServlet 类`](#httpservlet)

* `新建一个 java web 项目 然后在src 中添加这个类`
* `更改xml 配置文件`
```xml
  <servlet>
  	<servlet-name>my</servlet-name>
  	<servlet-class>com.study.server.MyServlet</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>my</servlet-name>
  	<url-pattern>/my</url-pattern>
  </servlet-mapping>
```
```JAVA
package com.study.jsp;
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
public class MyServlet extends HttpServlet {
   @Override
   protected void service(HttpServletRequest req,HttpServletResponse resp)
   throws ServletException,IOException
   {
      resp.setContentType("text/html;charset=UTF-8");
      String re_str = "<h2>我是你的第一个 Servlet类 </h2>";
     resp.getWriter().write(re_str);
     System.out.println(re_str);
   }
}
```
`启动它 然后输入路由 http://localhost:8080/study_Servlet/my  然后就看到效果啦`
#### [Servlet API](#top) <span id="servletapi"></span> 
`Serlvet 的结构`

![图片](/Image/Servlet.png)

##### [Servlet 接口](#top) <span id="interface"></span> 
`这个接口定义了五个方法`
* **`public abstract void init(ServletConfig config) throws ServletException`** 
	* `初始化servlet。该方法在加载servlet时由servlet引擎[Tomcate]自动调用一次。保证在接受任何服务请求之前完成。Servlet容器通过 ServletConfig 参数向Servlet 传递配置信息 ServletConfig从应用程序配置信息中获取名-值对的初始化参数`
* **`public abstract void service(ServletRequest req,ServletResponse res) throws ServletException, IOException`**
	* `处理客户端请求的方法 在调用这个方法之前要 包装 init 方法被正确调用 里面包含 request 和Response两个参数 一个获取请求信息 一个设置响应信息`
* **`public abstract String getServletInfo()`**
	* `返回servlet的信息,版权 作者等等`
* **` public abstract ServletConfig getServletConfig()`***
	* `返回传递给init方法的ServletConfig`
* **`public abstract void destroy()`**
	* `宣布这个对象已经完成使命,接下来静静等待gc回收它就行了`
##### [ServletRequest 接口](#top) <span id="servletrequest"></span> [官方文档](http://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/ServletRequest.html)
###### [`与http请求相关`](#httpRequestforservlet)
* `getReader（）`:`返回一个缓冲读取器，用于读取请求正文中的文本。`
* `getContentType()` :`返回请求正文的MIME类型，或者 null类型是否已知。`	
* `getContentLength()` :` 返回请求正文的长度（以字节为单位），并由输入流提供，如果长度未知，则返回-1。`
* `getCharacterEncoding()` :`返回此请求正文中使用的字符编码的名称。`
* `setCharacterEncoding(java.lang.String env)`：`设置此请求正文中使用的字符编码的名称。`
* `setAttribute（String，Object）`:`此方法将属性存储在请求上下文中; 这些属性将在请求之间重置。`
* `getProtocol()`:`以protocol / majorVersion.minorVersion格式返回请求使用的协议的名称和版本，例如HTTP / 1.1。`
* `getScheme()` :`返回请求方式，例如 http，https或ftp。`
###### [请求者客户端](#httpclientforservlet)
* `getServerPort()`: `返回发送请求的端口号。`
* `getRemoteAddr()`: `返回发送请求的客户端或最后一个代理的Internet协议（IP）地址。`
* `getRemoteHost()` :`返回客户端的完全限定名称或发送请求的最后一个代理。`
* `getServerName()`: `返回发送请求的服务器的主机名。`
###### [与文件上传有关](#httpFileforservlet)
* `getInputStream() `:`使用ServletInputStream以二进制数据的形式检索请求的主体。`
###### [与request 属性有关](#)
* `setAttribute(java.lang.String name, java.lang.Object o) `：`在此请求中存储属性。 如果第二参数为null 那么相当于removeAttribute`
* `getAttributeNames()`:`Returns an Enumeration containing the names of the attributes available to this request.`
* `removeAttribute（java.lang.String name)`:`从此请求中删除属性。`
* `getParameter(java.lang.String name)`:`以String形式返回请求参数的值，如果参数不存在，则返回null。`
* `getParameterMap() `:`返回此请求的参数的java.util.Map。`
* `getParameterValues(java.lang.String name) `:`返回包含给定请求参数所具有的所有值的String对象数组，如果参数不存在，则返回null。`
##### [`ServletResponse 接口` ](#top)   <span id="servletresponse"></span> [官方文档](http://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/ServletResponse.html)
`响应对象`
###### 数据写入有关
* `setBufferSize(size)`: `设置响应正文的首选缓冲区大小。`
* `flushBuffer()`: `强制将缓冲区中的任何内容写入客户端。`
* `getBufferSize()`: `返回用于响应的实际缓冲区大小。`
* `getOutputStream()`: `返回ServletOutputStream适合在响应中写入二进制数据。`
* `getWriter()`: `返回PrintWriter可以将字符文本发送到客户端的对象。`
* `resetBuffer()`: `清除响应中底层缓冲区的内容，而不清除标头或状态代码。`
###### http 响应设置有关
* `setCharacterEncoding(charset)`: `设置发送到客户端的响应的字符编码（MIME字符集），例如，设置为UTF-8。`
* `getCharacterEncoding()`: `返回用于此响应中发送的正文的字符编码（MIME字符集）的名称。`
* `getContentType()`: `返回用于此响应中发送的MIME正文的内容类型。`
* `getLocale()`: `使用该setLocale(java.util.Locale)方法返回为此响应指定的语言环境。`
* `isCommitted()`: `返回一个布尔值，指示响应是否已提交。`
* `reset()`: `清除缓冲区中存在的所有数据以及状态代码和标头。`
* `setContentLength(len)`: `设置响应中内容主体的长度在HTTP-servlet中，此方法设置HTTP-Content-Length标头。`
* `setContentType(java.lang.String)`: `如果尚未提交响应，则设置发送到客户端的响应的内容类型。`
* `setLocale(java.util.Locale)`: `如果尚未提交响应，则设置响应的区域设置。`

##### [ServletConfig 接口](#top) <span id="servletconfig"></span> [官方文档](http://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/ServletConfig.html)
* `getInitParameter(name)`: `name:String返回String包含指定的初始化参数的值的a,或者null如果该参数不存在。`
* `getInitParameterNames()`: `返回servlet的初始化参数的名称作为Enumeration的String对象或空Enumeration如果servlet没有初始化参数。`
* `getServletContext()`: `返回ServletContext对调用者正在执行的引用。`
* `getServletName()`: `返回此servlet实例的名称。`
##### [GenericServlet 类](#top) <span id="genericservlet"></span> 
```
java.lang.Object
   |
   ---javax.servlet.GenericServlet:java.io.Serializable，Servlet，ServletConfig
     |
     --- HttpServlet
```
- `它给出了 Servlet 除了Service 之外其他四个方法的简单实现,让我们不必关注与另外四个方法的实现 你只需要关注service 的实现就好啦,但是它还是不如他的子类那么方便,所以还是使用HttpServlet吧 `
##### 具有的方法
* `destroy()`: `由servlet容器调用，以向servlet指示servlet正在停止服务。`
* `getInitParameter(java.lang.String-name)`: `返回String包含指定的初始化参数的值的a，或者null如果该参数不存在。`
* `getInitParameterNames()`: `返回servlet的初始化参数的名称作为Enumeration的String对象或空Enumeration如果servlet没有初始化参数。`
* `getServletConfig()`: `返回此servlet的ServletConfig对象。`
* `getServletContext()`: `返回ServletContext对此servlet运行的引用。`
* `getServletInfo()`: `返回有关servlet的信息，例如作者，版本和版权。`
* `getServletName()`: `返回此servlet实例的名称。`
* `init()`: `一种方便的方法，可以被覆盖，这样就不需要打电话了super.init(config)。`
* `init(ServletConfig-config)`: `由servlet容器调用，以向servlet指示servlet正在投入使用。`
* `log(java.lang.String-msg)`: `将指定的消息写入servlet日志文件，该文件以servlet的名称为前缀。`
* `log(java.lang.String-message,`: `java.lang.Throwable-t)`
* `service(ServletRequest-req,`: `ServletResponse-res)`

##### [HttpServlet 类](#top) <span id="httpservlet"></span> 
`用于应对浏览器的请求和发送响应提供了许多的方法`
* `void doDelete(HttpServletRequest req, HttpServletResponse resp) 由服务器调用（通过service方法）以允许servlet处理DELETE请求。`
* `void doGet(HttpServletRequest req, HttpServletResponse resp) 由服务器调用（通过service方法）以允许servlet处理GET请求。`
* `void doHead(HttpServletRequest req, HttpServletResponse resp) 从受保护service方法接收HTTP HEAD请求 并处理请求。`
* `void doOptions(HttpServletRequest req, HttpServletResponse resp) 由服务器调用（通过service方法）以允许servlet处理OPTIONS请求。`
* `void doPost(HttpServletRequest req, HttpServletResponse resp) 由服务器调用（通过service方法）以允许servlet处理POST请求。`
* `void doPut(HttpServletRequest req, HttpServletResponse resp) 由服务器调用（通过service方法）以允许servlet处理PUT请求。`
* `void doTrace(HttpServletRequest req, HttpServletResponse resp) 由服务器调用（通过该service方法）以允许servlet处理TRACE请求。`
* `long getLastModified(HttpServletRequest req) 返回HttpServletRequest 自上次修改对象的时间，以格林威治标准时间1970年1月1日午夜为单位，以毫秒为单位。`
* `void service(HttpServletRequest req, HttpServletResponse resp) 从public service方法接收标准HTTP请求， 并将它们分派给此类中定义的doXXX方法。`
* `void service(ServletRequest req, ServletResponse res) 将客户端请求分派给受保护的 service方法。`
