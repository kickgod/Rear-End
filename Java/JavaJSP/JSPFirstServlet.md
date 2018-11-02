[第一个Servlet ](#top) <span id="top"></span>  	:maple_leaf:
-----
`JSP的底层其实就是 Servlet 现在我们来跑一个Servlet 吧` [`servlet 文档`](https://docs.oracle.com/cd/E17802_01/products/products/servlet/2.1/api/packages.html)

- [x] [`servlet 结构类图`](#servletapi)
- [x] [`Servlet 接口`](#interface)
- [x] [`ServletRequest`](#servletrequest)


* `新建一个 java web 项目 然后在src 中添加这个类`
* `更改xml 配置文件`
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
#### [Servlet API](#servletapi) <span id="servletapi"></span> 
`Serlvet 的结构`
![图片](/Image/Servlet.png)

##### [Servlet 接口](#interface) <span id="interface"></span> 
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
##### [ServletRequest](#servletrequest) <span id="servletrequest"></span>[官方文档](http://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/ServletRequest.html)
###### [`与http请求相关`](#httpRequestforservlet)
* `getReader（）`:`返回一个缓冲读取器，用于读取请求正文中的文本。`
* `getContentType()` :`返回请求正文的MIME类型，或者 null类型是否已知。`	
* `getContentLength()` :` 返回请求正文的长度（以字节为单位），并由输入流提供，如果长度未知，则返回-1。`
* `getCharacterEncoding()` :`返回此请求正文中使用的字符编码的名称。`
* `setCharacterEncoding(java.lang.String env)`：`设置此请求正文中使用的字符编码的名称。`
* `setAttribute（String，Object）`:`此方法将属性存储在请求上下文中; 这些属性将在请求之间重置。`
* `getProtocol()`:`以protocol / majorVersion.minorVersion格式返回请求使用的协议的名称和版本，例如HTTP / 1.1。`
* `getScheme()` :`返回请求方式，例如 http，https或ftp。`
###### [`请求者客户端`](#httpclientforservlet)
* `getServerPort()`: `返回发送请求的端口号。`
* `getRemoteAddr()`: `返回发送请求的客户端或最后一个代理的Internet协议（IP）地址。`
* `getRemoteHost()` :`返回客户端的完全限定名称或发送请求的最后一个代理。`
* `getServerName()`: `返回发送请求的服务器的主机名。`
###### [`与文件上传有关`](#httpFileforservlet)
* `getAttributeNames()`:`Returns an Enumeration containing the names of the attributes available to this request.`
* `getInputStream() `:`使用ServletInputStream以二进制数据的形式检索请求的主体。`
###### [`与request 属性有关`](#)
* `removeAttribute（java.lang.String name)`:`从此请求中删除属性。`
* `getParameter(java.lang.String name)`:`以String形式返回请求参数的值，如果参数不存在，则返回null。`
* `getParameterMap() `:`返回此请求的参数的java.util.Map。`
* `getParameterValues(java.lang.String name) `:`返回包含给定请求参数所具有的所有值的String对象数组，如果参数不存在，则返回null。`
