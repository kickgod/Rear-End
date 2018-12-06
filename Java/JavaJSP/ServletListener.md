### [Servlet Listener 监听器](#top) <b id="top"></b> 

----
`Servlet监听器是给Web应用增加事件处理机制，以便更好地监视和控制Web应用的状态变化，从而在后台调用相应处理程序。  可以监听session request application内部
的创建 销毁 和属性,内容的改变等等  从而实现很多的功能例如 通过session 统计网站在线人数的数量。`

- [x] [`监听器介绍`](#intro) 
- [x] [`创建一个 request监听器`](#reexm) 

------

##### [监听器介绍](#top)  <b id="intro"></b> 
[`博客资料`](https://blog.csdn.net/wxwzy738/article/details/8615244)
* `Session 监听器`
  * `HttpSessionActivationListener	绑定到会话的对象可以侦听容器事件，通知它们会话将被钝化并且会话将被激活。`
  * `HttpSessionAttributeListener	可以实现此侦听器接口，以便获得对此Web应用程序中的会话的属性列表的更改的通知。`
  * `HttpSessionListener 将通知此接口的实现，以更改Web应用程序中的活动会话列表。`
* `Request 监听器`
  * `ServletRequestListener	ServletRequestListener可以由开发人员实现，该开发人员有兴趣收到进入和离开Web组件范围的请求的通知。`
* `Application 监听器`  
  * `ServletContextListener`:`此接口的实现接收有关它们所属的Web应用程序的servlet上下文的更改的通知。`
  * `ServletContextAttributeListener	此接口的实现接收Web应用程序的servlet上下文中属性列表更改的通知。`
##### [创建一个 request监听器](#top)  <b id="reexm"></b> 
`和过滤器一样 也要配置`
```java
package com.zzw.lintener;

import javax.servlet.ServletRequestEvent;
import javax.servlet.ServletRequestListener;

public class MyFirstListener implements ServletRequestListener {

    @Override
    public void requestDestroyed(ServletRequestEvent sre) {
        System.out.println("Request 销毁了");
    }

    @Override
    public void requestInitialized(ServletRequestEvent sre) {
        System.out.println("Request 被创建了");
    }
}
```
###### ServletRequestEvent 参数具有的方法
* `ServletContext	getServletContext() `:`返回此Web应用程序的ServletContext。`
* `ServletRequest	getServletRequest() `:`返回正在更改的ServletRequest。`

`xml 配置`
```xml
<listener>
    <listener-class>com.zzw.lintener.MyFirstListener</listener-class>
</listener>
```

#####  Session 监听
```java
package com.zzw.lintener;

import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

public class SessionLinstener implements HttpSessionListener {

    //Session 创建
    @Override
    public void sessionCreated(HttpSessionEvent se) {

    }

    //Session 销毁
    @Override
    public void sessionDestroyed(HttpSessionEvent se) {

    }
}

```
`HttpSessionEvent 内容:`
* `HttpSessionEvent(HttpSession source) 从给定的源构造会话事件。`
