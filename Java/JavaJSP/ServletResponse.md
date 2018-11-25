### [Response 对象](#top) <b id="top"></b> :maple_leaf:

----
`Request 对象是一个专门用来存储HTTP 请求信息的对象` [`官方文档`](http://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/http/HttpServletResponse.html)

- [x] :maple_leaf: [`Response 对象的由来`](#response) 
- [x] :maple_leaf: [`Response 功能`](#func) 
- [x] :maple_leaf: [`乱码原因`](#code) 
 



##### [Response 对象的由来](#top)  <b id="response"></b>
`封装HTTP响应信息的对象 处理渲染细节 负责将响应信息发送给浏览器` `它还具有 ServletResponse 接口的很多方法`

##### [Response 功能](#top)  <b id="func"></b>
* `设置响应编码格式 设置响应头信息`
  * `addHeader(java.lang.String name, java.lang.String value)` ：`添加具有给定名称和值的响应标头。`
  * `setHeader(java.lang.String name, java.lang.String value)` :`设置具有给定名称和值的响应标头。`
  * `resp.setContentType("text/html;charset=UTF-8");`:`设置响应格式为 UTF8`
* `设置响应HTTP 信息 或者获取`
  * `setStatus(int sc)`:`设置此响应的状态代码。`
  * `isCommitted() `:`返回一个布尔值，指示响应是否已提交。`
* `重定向 可以改变浏览器的URL 请求转发不改变浏览器URL`
  * `sendRedirect(java.lang.String location)` :`使用指定的重定向位置URL向客户端发送临时重定向响应。`

##### [乱码原因](#top)  <b id="code"></b>
`因为浏览器在处理编码的时候使用的编码格式是` `iso-8859-1` `所以会出现中文乱码` `虽然服务器和浏览器使用 其他格式 但是 浏览器在发送数据的时候使用的是iso-8859-1格式`<br/>

`解决方法  解码 再编码 使用S`
```java
String UserID  = new String(request.getParameter("UserID").getBytes("iso-8859-1"), "utf-8");
```

##### Get 方式乱码解决
`在service 方法中调用`
```java
req.setCharacterEncoding("utf-8");
```
`在Tomecat  服务器中的 conf 文件夹里面的 service.xml 修改配置为`
```xml
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" useBodyEncodingFromIRI="true"  />
```
##### Post 方式乱码解决
`在service 方法中调用`
```java
req.setCharacterEncoding("utf-8");
```
