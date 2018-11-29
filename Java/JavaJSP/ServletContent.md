### [ServletContent](#top) <b id="top"></b> :maple_leaf:

----
`实现Application的职责 完成所有用户共享数据 使得不同用户可以共享同一个对象 Servlet为我们实现了ServletContent对象来完成这个功能` [`官方文档`](http://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/ServletContext.html)

- [x] :maple_leaf: [`获取ServletContent对象`](#get) 
- [x] :maple_leaf: [`说明`](#desc) 
- [x] :maple_leaf: [`功能`](#func) 
- [x] :maple_leaf: [`数据存储交互`](#data) 



##### [获取ServletContent对象](#top)  <b id="get"></b> :maple_leaf:
`在Servlet 对象中可以直接使用 this `
```c#
ServletContext sc = this.getServletContext();
```
`在jsp中`
```c#
ServletContext sc = this.getServletConfig().getServletContext();
```
`通过request获取`
```c#
ServletContext context = req.getSession().getServletContext();
```
##### [说明](#top)  <b id="desc"></b> :maple_leaf:
* `由服务器创建`
* `所有用户共享`
* `一个项目只有一个` 
* **`生命周期`**:`从你将项目部署到服务器到 服务器停止此网站或项目被停止 `

##### [功能](#top)  <b id="func"></b> :maple_leaf:
* 1.`在ServletContext存储数据`
* 2.`获取项目中web.xml文件的全局配置信息`
* 3.`获取项目 webroot下的文件资源的绝对路径`
* 4.`获取项目 webroot下的文件资源的流对象 但是不能获取类文件的流对象`
* 5.`可以用来统计网站访问情况`
`web-app 下面`
```xml
<context-param>
    <param-name>Author</param-name>
    <param-value>JxKicker</param-value>
</context-param>
<context-param>
    <param-name>age</param-name>
    <param-value>18</param-value>
</context-param>
```
`获取配置信息`
```c#
ServletContext sc = this.getServletConfig().getServletContext();
String val = sc.getInitParameter("Author");
String key[] = sc.getInitParameters(); //返回所有自定义配置项
response.getWriter().println("作者:" + val );
```
`获取真实路径`：`传入的路径为项目根目录`
```c#
ServletContext sc = this.getServletConfig().getServletContext();
String realPath = sc.getRealPath("/resources/my.txt");
response.getWriter().println(realPath);
```
`获取webroot下的文件资源的流对象`：`传入的路径为项目根目录`
```
InputStream stream_  = sc.getResourceAsStream("/resources/my.txt");
```
##### [数据存储交互](#top)  <b id="data"></b> :maple_leaf:
* `void	removeAttribute(java.lang.String name) `:`从servlet上下文中删除具有给定名称的属性。`
* `void	setAttribute(java.lang.String name, java.lang.Object object)`: `将对象绑定到此servlet上下文中的给定属性名称。`
* `java.lang.Object	getAttribute(java.lang.String name)` :`返回具有给定名称的servlet容器属性，或者null如果该名称没有属性。`
* `java.util.Enumeration	getAttributeNames()`:` 返回Enumeration包含此servlet上下文中可用的属性名称的内容。`
