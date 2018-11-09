### [Servlet 处理HTTP请求的一些类](#top) <b id="top"></b> :maple_leaf:
- [x] [`HttpServletRequest 类`](#httpservletrequest) :maple_leaf:
- [x] [`HttpServletResponse 类`](#httpservletresponse) :maple_leaf:
- [x] [`HttpSession 类`](#httpsession) :maple_leaf:
- [x] [`Cookie 类`](#cookie) :maple_leaf:
- [x] [`HttpSessionListener 类`](#cookie) :maple_leaf:

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
* `addHeader(java.lang.String name, java.lang.String value)` :`添加具有给定名称和值的响应标头。`
* `addDateHeader(java.lang.String name, long date)`:`添加具有给定名称和日期值的响应标头。`
* `setHeader(java.lang.String name, java.lang.String value)`:` 设置具有给定名称和值的响应标头。`
* `setDateHeader(java.lang.String name, long date)`:` 设置具有给定名称和日期值的响应标头。`
* `setIntHeader(java.lang.String name, int value) `:`设置具有给定名称和整数值的响应标头。`
##### [编码有关](#top)
* `encodeURL(java.lang.String url)`:` 通过在其中包含会话ID来对指定的URL进行编码，或者，如果不需要编码，则返回URL不变。`
* `encodeRedirectURL(java.lang.String url)`:` 对指定的URL进行编码以在sendRedirect方法中使用， 或者，如果不需要编码，则返回URL不变。`
##### [Http响应有关](#top)
* `sendError(int sc)`:` 使用指定的状态代码向客户端发送错误响应并清除缓冲区。`
* `sendError(int sc, java.lang.String msg)`:` 使用指定的状态向客户端发送错误响应。`
* `sendRedirect(java.lang.String location) `:`使用指定的重定向位置URL向客户端发送临时重定向响应。`
* `setStatus(int sc)`:`设置此响应的状态代码。`
#### [`HttpSession 类`](#top) <b id="httpsession"></b>
* `提供一种在多个页面请求中识别用户或访问网站并存储有关该用户的信息的方法。` 
   * `查看和操作有关会话的信息，例如会话标识符，创建时间和上次访问时间`
   * `将对象绑定到会话，允许用户信息在多个用户连接中保持不变`
##### [方法](#top)
* `Object getAttribute(java.lang.String name)`:`返回在此会话中使用指定名称绑定的对象，或者 null如果名称下没有绑定任何对象。`
* `Enumeration getAttributeNames()`:` 返回Enumeration的String含有与本次会议的所有对象的名称的对象。`
* `long getCreationTime()`:` 返回创建此会话的时间，以格林威治标准时间1970年1月1日午夜以来的毫秒数为单位。`
* `String getId() `:`返回包含分配给此会话的唯一标识符的字符串。`
* `long getLastAccessedTime()`:`返回客户端最后一次发送与此会话关联的请求 并以容器收到请求的时间为标记。`
* `getServletContext() `:`返回此会话所属的ServletContext。`
* `invalidate() `:`使此会话无效，然后取消绑定绑定到它的任何对象。`
* `isNew()` :`返回true如果客户端还不知道该会话或者如果客户选择不参加会议。`
* `setMaxInactiveInterval(int interval)`：`指定servlet容器使此会话失效之前的客户端请求之间的时间（以秒为单位）。`
*	`removeAttribute(java.lang.String name) `:`从此会话中删除与指定名称绑定的对象。`
* `setAttribute(java.lang.String name, java.lang.Object value)` ：`使用指定的名称将对象绑定到此会话。`
#### [`Cookie 类`](#top) <b id="cookie"></b>
* `创建一个cookie，由servlet发送到Web浏览器的少量信息，由浏览器保存，然后发送回服务器。cookie的值可以唯一标识客户端，因此cookie通常用于会话管理。`
* `Cookie具有名称，单个值和可选属性，例如注释，路径和域限定符，最大年龄和版本号`
##### [设置cookie](#top)
`有set就有对应的 get`
* `setMaxAge(int expiry)`:`以秒为单位设置cookie的最大年龄。` **设置Cookie 过期时间**
* `setValue(java.lang.String newValue)`:`创建cookie后，为cookie分配新值。`
* `setDomain(java.lang.String pattern)`:`指定应在其中显示此cookie的域。`
* `setComment(java.lang.String purpose) `:`指定描述cookie用途的注释。`
* `setPath(java.lang.String uri)`:`指定客户端应返回cookie的cookie的路径。`
* `setSecure(boolean flag) `:`向浏览器指示是否应仅使用安全协议（例如HTTPS或SSL）发送cookie。` **设置Cookie Http-Only**
* `setVersion(int v)`:`设置此cookie符合的cookie协议的版本。`
#### [HttpSessionListener 类](#top)
`继承自` `java.util.EventListener` `将通知此接口的实现，以更改Web应用程序中的活动会话列表。要接收通知事件，必须在Web应用程序的部署描述符中配置实现类。`
* `void	sessionCreated(HttpSessionEvent se)`: `创建会话的通知。`
* `void	sessionDestroyed(HttpSessionEvent se)`:`会话即将失效的通知。`
