### [ServletContent](#top) <b id="top"></b> :maple_leaf:

----
`用于获取web.xml 每个Servlet对象的 单独配置信息 这就是Servlet的功能 返回的是当前Servlet的 config 信息` [`官方文档`](http://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/ServletConfig.html)

- [x] :maple_leaf: [`获取 ServletConfig 对象`](#get) 
- [x] :maple_leaf: [`基本方法`](#func) 


##### [获取ServletContent对象](#top)  <b id="get"></b> :maple_leaf:
```xml
  <servlet>
      <description>处理用户登录</description>
      <display-name>登录</display-name>
      <servlet-name>login</servlet-name>
      <servlet-class>com.user.dealwith.UserLoginServlet</servlet-class>
      <init-param>
          <param-name>config</param-name>
          <param-value>name</param-value>
      </init-param>
  </servlet>
  <servlet-mapping>
      <servlet-name>login</servlet-name>
      <url-pattern>/login</url-pattern>
  </servlet-mapping>
```
```c#
 ServletConfig config =  this.getServletConfig();
 String val = config.getInitParameter("config");
```

##### [基本方法](#top)  <b id="func"></b> :maple_leaf:
* `java.lang.String	getInitParameter(java.lang.String name) `:`返回String包含指定的初始化参数的值的a ，或者null如果该参数不存在。`
* `java.util.Enumeration	getInitParameterNames() `:`返回servlet的初始化参数的名称作为Enumeration的String对象或空Enumeration如果servlet没有初始化参数。`
* `ServletContext	getServletContext() `:`返回ServletContext对调用者正在执行的引用。`
* `java.lang.String	getServletName() `:`返回此servlet实例的名称。`
