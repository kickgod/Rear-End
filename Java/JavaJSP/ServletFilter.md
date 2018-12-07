### [Servlet Filter过滤器](#top) <b id="top"></b> 

----
`过滤器 在请求到达特定Servlet之前 对请求的统一处理 过滤器是对资源请求（servlet或静态内容）或来自资源的响应（或两者）执行过滤任务的对象.` [`官方文档`](http://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/Filter.html)

- [x] [`过滤器的三个方法`](#create) 
- [x] [`配置一个Servlet过滤器吧`](#example) 
- [x] [`过滤器流程`](#life) 
- [x] [`过滤器用处 统一编码`](#code) 
- [x] [`过滤器用户 用户登录session 管理`](#session) 

------

##### [过滤器的三个方法](#top)  <b id="create"></b> 
* `void	destroy() `:`由Web容器调用以向过滤器指示它正在停止服务。`
* `void	doFilter(ServletRequest request, ServletResponse response, FilterChain chain) `:`doFilter每次由于客户端请
求链末端的资源而请求/响应对通过链时，容器都会调用Filter的方法。 它是主要方法 需要手动放行 不然就啥也没有了 到过滤器就结束了`
* `void	init(FilterConfig filterConfig) `:`由Web容器调用以向过滤器指示它正在投入使用。服务器启动就执行`

##### 注意
* `一个Servlet 可能进过多个过滤器`
##### [配置一个Servlet过滤器吧](#example)  <b id="create"></b> 
`过滤器是服务器tomcat 自动调用,程序员做的就是配置了以servlet 说明服务器如何调用过滤器 配置在web.xml 中进行`
```c#
package com.zzw.filter;

import javax.servlet.*;
import java.io.IOException;

public class MyFirstServletFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("MyFirstServletFilter 被初始化了");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse,
    FilterChain filterChain) throws IOException, ServletException {
        System.out.println("MyFirstServletFilter 我被执行了");

        //如果不放心可以使用重定向 通过 servletResponse
        System.out.println( "我被放行了");
        //放行 这个过滤器完事了 你继续接下啦的事情
        filterChain.doFilter(servletRequest,servletResponse);
    }

    @Override
    public void destroy() {
        System.out.println("MyFirstServletFilter 被销毁了");
    }
}
```
##### [配置Filter](#top)
```xml
<filter>
    <filter-name>MyFilter</filter-name>
    <filter-class>com.zzw.filter.MyFirstServletFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>MyFilter</filter-name>
    <url-pattern>/*</url-pattern>   
</filter-mapping>

<!--
<url-pattern>/*</url-pattern> 说明作用的url路径 此处是所有路径都起作用
这说明你请求的每一个路径都会被过滤器拦截处理
-->
```
* `<url-pattern>/*</url-pattern>`:`拦截所有的请求`
* `<url-pattern>*do</url-pattern>`:`只要url以do结尾的都要走这个过滤器`

##### [过滤器流程](#life)  <b id="create"></b> 
`浏览器发送请求到服务器,服务器接收请求后 根据url在web.xml 中查询是否具有过滤器对此url进行拦截 有则执行对于的过滤器类,如果
满足此拦截条件 那么久在过滤器中的doFilter方法中调用filterChain.doFilter(servletRequest,servletResponse); 放行,让此请求继续
被处理 继续查找是否具有对此url的其他拦截器...不满足则使用重定向 此请求到提示或者错误页面`

##### [过滤器用处 统一编码](#code)  <b id="create"></b>
`写一个过滤器然后 /* 执行 这样就不用再每一个 Servlet中 再设置编码格式了`
```java
public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, 
FilterChain filterChain) throws IOException, ServletException {

    servletRequest.setCharacterEncoding("utf-8");
    servletResponse.setCharacterEncoding("utf-8");
    servletResponse.setContentType("text/html;charset=UTF-8");
    filterChain.doFilter(servletRequest,servletResponse);
}
```

##### [过滤器用户 用户登录session 管理](#session)  <b id="create"></b>
`用户登录那就可以进入网站 没有登录那么跳转到登录页面 这里有很多的难点 稍微不留意就发生循环重定向 和资源加载被拦截问题`
```java
public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse,
FilterChain filterChain) throws IOException, ServletException {
    //如果不判断请求路由 那么造成请求错误 因为 会造成重复重定向
    if(((HttpServletRequest)servletRequest).getRequestURI()!= "/exam02/login.html" ) {
    
        HttpSession session = ((HttpServletRequest)servletRequest).getSession();
        if (session.getAttribute("UserName")!= null){
             filterChain.doFilter(servletRequest,servletResponse);
        }else {
            ((HttpServletResponse)servletResponse).sendRedirect("login.html"); //再次请求
        }  
    }else {
        filterChain.doFilter(servletRequest,servletResponse);
    } 
}
```
* `为啥可以强转:因为虽然传入的类型规定是接口 servletRequest 但是实际上传入的是类 HttpServletRequest...同理如此`
* ``

















