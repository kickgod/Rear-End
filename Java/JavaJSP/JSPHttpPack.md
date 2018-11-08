### [Servlet 处理HTTP请求的一些类](#top) <b id="top"></b> :maple_leaf:
- [x] [`HttpServletRequest 类`](#httpservletrequest) :maple_leaf:
- [x] [`HttpServletResponse 类`](#httpservletresponse) :maple_leaf:

#### [HttpServletRequest 类](#top) <b id="httpservletrequest"></b> 
`实现了接口 ServletRequest` [`官方文档`](http://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/http/HttpServletRequest.html)

###### 四个属性的值
```
static java.lang.String	BASIC_AUTH  String identifier for Basic authentication.
static java.lang.String	CLIENT_CERT_AUTH String identifier for Client Certificate authentication.
static java.lang.String	DIGEST_AUTH String identifier for Digest authentication.
static java.lang.String	FORM_AUTH String identifier for Form authentication.
```

* `getContextPath() `:`返回请求URI的一部分，指示请求的上下文。`
```Java
 //请求的url:  http://localhost:8080/study_Servlet/my
 response.getWriter().append("Served at: ").append(request.getContextPath());
 //输出:Served at: /study_Servlet 
 //并没有打印:/my
```

* `getCookies() `:`返回 Cookie[] 所有的Cookie 对象 `
* `getHeader(java.lang.String name)`:`以String 形式返回指定请求标头的值String。返回报文头中指定文臣的值 如果报文中没有这个名称  返回null。`
* `getHeaders(java.lang.String name)`:`返回java.util.Enumeration  返回指定请求头作为的所有值的枚举集合`
* `getMethod()` :`返回用于发出此请求的HTTP方法的名称，例如，GET，POST或PUT。`
* `getQueryString()`:`返回路径后请求URL中包含的查询字符串。`
```java
  请求路径：http://localhost:8080/study_Servlet/my?name=jx
  response.getWriter().println(request.getQueryString());
  name=jx 
```
* `getRequestURI()`:`返回主机名/域名 到查询字符之间的部分`
```Java
  请求路径：http://localhost:8080/study_Servlet/my?name=jx  返回 study_Servlet/my
  请求路径：http://localhost:8080/study_Servlet/index.js?name=jx  返回 study_Servlet/index.jsp
```
* `getSession()`:`返回与当前相关联的Session,如果没有那么会创建一个,分配给客户端 ` `返回:HttpSession` 
* `getSession(boolean create)`:`返回与当前相关联的Session,若没有Session 如果create为true 那么会创建一个,分配给客户端 否则返回null
此次没有Session` `返回:HttpSession` 
* `getServerPort()`:`返回请求端口 例如 80 或者8080`
* `getServletPath()`:`返回Servlet的路径 `
```xml
http://localhost:8080/study_Servlet/my 返回 /my
  //MyServlet 配置文件路径如下
  <servlet>
  	<servlet-name>my</servlet-name>
  	<servlet-class>com.study.server.MyServlet</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>my</servlet-name>
  	<url-pattern>/my</url-pattern>
  </servlet-mapping>
```
* `isRequestedSessionIdValid() `:`返回当前请求ID 是否任然有效`
* `getAuthType()`:`BASIC_AUTH，FORM_AUTH，CLIENT_CERT_AUTH，DIGEST_AUTH`
#### [`HttpServletResponse 类`](#top) <b id="httpservletresponse"></b>
`处理Http 响应的类`
##### `拥有的方法`
* `addCookie(Cookie cookie)`: `将指定的cookie添加到响应中`
