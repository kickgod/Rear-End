### [Servlet 基础知识](#top) <span id="top"></span>  	:maple_leaf:

-----
`Servlet具有生命周期,等等各种本质的知识` [`servlet 文档`](https://docs.oracle.com/cd/E17802_01/products/products/servlet/2.1/api/packages.html)

* [`Servlet 生命周期`](#servlet)
* [`web.xml 配置`](#xml)  
* [`Servlet 注意事项`](#notice)  
* [`Servlet 基本流程`](#service)  

##### [一.写代码来试一试](#top) <span id="servlet"></span> 
```java
@WebServlet(name = "ChangeUser")
public class ChangeUser extends HttpServlet {

    public void init() throws ServletException{
        System.out.println("开始初始化了！");
    }

    protected void service(HttpServletRequest req,HttpServletResponse resp)
            throws ServletException,IOException
    {
        System.out.println("开始服务啦！");
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) 
    throws ServletException, IOException {
        response.getWriter().write("你们好啊 post");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) 
    throws ServletException, IOException {
        response.getWriter().write("你们好啊 do");
    }

    public void destroy(){
        System.out.println("我被销毁啦！");
    }
}
```
`启动服务器访问这个类,多次请求这个servlet 打印如下`<br/>
[`基本流程:`](#top) `http 请求`->`tomecat`->`通过web.xml找到 servlet类`->`通过反射构造这个类的对象`->`放进内存中`->`service 方法`->`post/get`->`销毁 destroy 当服务器关闭
或者此类不用 垃圾回收`

##### [调用结果](#top)

* `开始初始化了！` `---第一次请求这个servlet 类`
* `开始服务啦！`
* `开始服务啦！`
* `开始服务啦！`
* `开始服务啦！`
* `我被销毁啦！` `---关闭服务器的时候`
```xml
    <servlet>
        <servlet-name>update</servlet-name>
        <servlet-class>com.study.start.ChangeUser</servlet-class>
        <load-on-startup>1</load-on-startup> <!-- 当服务器启动的时候就加载这个类进入内存 数字是优先级 小的优先 -->
    </servlet>
    <servlet-mapping>
        <servlet-name>update</servlet-name>
        <url-pattern>/update</url-pattern>
    </servlet-mapping>
```
##### [结论](#top)
* `servlet 生命周期`
   * `第一次调用到服务器关闭`
   * `如果Servlet 在web.xml中配置load-on-startup 生命周期为服务器启动到服务器关闭`
* `init 方法是对Servlet 进行初始化的一个方法,会在Servlet第一次加载进行内存的时候执行`
* `destroy 当Servlet容器关闭时，Servlet实例也随时销毁。其间，Servlet容器会调用Servlet 的destroy()方法去判断该Servlet是否应当被释放（或回收资源）`
* `当一个客户请求改Servlet时，实际的处理工作全部有service方法来完成，service方法用来处理客户端的请求，并生成格式化数据返回给客户端。
每一次请求服务器都会开启一个新的线程并执行一次service方法，service根据客户端的请求类型，调用doGet、doPost等方法。`

##### [二.Web.xml 常用配置](#top) <span id="xml"></span> 
[`参考文件`](https://blog.csdn.net/xiuwu0423/article/details/54861184) <br/>
* `<display-name></display-name>`:`定义了WEB应用的名字 `
* `<error-page></error-page>`:` 在返回特定HTTP状态代码时，或者特定类型的异常被抛出时，能够制定将要显示的页面。 `
```xml
 <error-page> 
    <error-code>404</error-code> 
    <location>/NotFound.jsp</location> 
 </error-page> 
 
 //或者

<error-page> 
   <exception-type>java.lang.NullException</exception-type> 
   <location>/error.jsp</location> 
</error-page> 
```
* `<servlet></servlet> `:`在向servlet或JSP页面制定初始化参数或定制URL时，必须首先命名servlet或JSP页面。Servlet元素就是用来完成此项任务的。 `
```xml
<servlet>
    <description>修改用户信息</description>
    <display-name>用户信息修改</display-name>
    <servlet-name>update</servlet-name>
    <servlet-class>com.study.start.ChangeUser</servlet-class>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>update</servlet-name>
    <url-pattern>/update</url-pattern>
</servlet-mapping>
```
* `<description>`：`为Servlet指定一个文本描述。`
* `<display-name>`：`为Servlet提供一个简短的名字被某些工具显示。`
* `<load-on-startup>number</load-on-startup>`:`表示启动容器时，初始化Servlet。number 小的先加载 没什么用`
* `<servlet-name>`：`Servlet的名字，唯一性和一致性，与<servlet>元素中声明的名字一致。`
* `<url-pattern>`：`指定相对于Servlet的URL的路径。该路径相对于web应用程序上下文的根路径`
* `<servlet-mapping>`:`将URL模式映射到某个Servlet，即该Servlet处理的URL`
* `<welcome-file-list></welcome-file-list>`:` 指示服务器在收到引用一个目录名而不是文件名的URL时，使用哪个文件。`
```xml
<welcome-file-list> 
    <welcome-file>index.jsp</welcome-file> 
    <welcome-file>index.html</welcome-file> 
    <welcome-file>index.htm</welcome-file> 
 </welcome-file-list> 
```
* `<filter></filter> 过滤器元素将一个名字与一个实现javax.servlet.Filter接口的类相关联。 `
```xml
<filter> 
  <filter-name>setCharacterEncoding</filter-name> 
  <filter-class>com.myTest.setCharacterEncodingFilter</filter-class> 
  <init-param> 
      <param-name>encoding</param-name> 
      <param-value>GB2312</param-value> 
  </init-param> 
</filter> 
<filter-mapping> 
  <filter-name>setCharacterEncoding</filter-name> 
  <url-pattern>/*</url-pattern> 
</filter-mapping> 
```
##### [三.Servlet 注意事项](#top) <span id="notice"></span> 
##### [`service 方法 405错误`](#top)
`HttpServlet 中还有 doPost doGet 方法 如果你重写了service 方法但是并没有在方法内部调用super.service(req, resp);` `那么客户端在请求post/get 会报错
因为`：`每一次请求服务器都会开启一个新的线程并执行一次service方法，service根据客户端的请求类型，调用doGet、doPost等方法。`
```java
protected void service(HttpServletRequest req,HttpServletResponse resp)
        throws ServletException,IOException
{
    System.out.println("开始服务啦！");
    super.service(req, resp);
}
```
`所以一般我们不重写写service 只是重写doPost doGet方法 更加安全方便 避免405错误`：`请求方式不支持` 

##### [`505 错误`](#top)
* 1.`web.xml 类路径错误`：`<servlet-class>com.study.start.UserLogin</servlet-class>` `没有找到这个类` `检测方法 按住ctrl 鼠标放到类名上面
点击是否可以跳转到类文件中`
* 2.`service 方法体代码错误`

##### [四.Servlet 基本流程](#top) <span id="service"></span> 
* `浏览器发起请求到服务器`
* `服务器[Tomcat]接收HTTP请求 并且创建request 对象存储HTTP请求信息`
* `服务器调用对应的Servlet类对请求进行处理并且 request 对象作为参数传递给Servlet 方法`
* `执行Servlet方法处理请求`
* `设置请求编码格式`
* `设置响应格式`
* `获取请求信息`
* `处理请求信息`
* `创建业务层对象`
* `调用业务层方法`
* `响应处理结果`





