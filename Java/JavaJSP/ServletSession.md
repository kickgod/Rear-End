### [Request Session](#top) <b id="top"></b> :maple_leaf:

----
`将客户信息存储在服务器中...` [`官方文档`](http://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/http/HttpSession.html)

- [x] :maple_leaf: [`创建一个 Session`](#create) 
- [x] :maple_leaf: [` Session 存储在哪里`](#place) 
- [x] :maple_leaf: [` Session 存储数据`](#store) 
- [x] :maple_leaf: [`获取 Session`](#get) 

##### [创建一个 Session](#top)  <b id="create"></b> :maple_leaf:
* `HttpSession	getSession()`:`返回与此请求关联的当前会话，或者如果请求没有会话，则创建一个会话。`
* `HttpSession	getSession(boolean create)`:` 返回HttpSession 与此请求关联的当前值，如果没有当前会话且create为true，则返回新会话。`
* `int	getMaxInactiveInterval()`:` 返回servlet容器在客户端访问之间保持此会话打开的最长时间间隔（以秒为单位）`
* `int	setMaxInactiveInterval(int interval)`:` 指定servlet容器使此会话失效之前的客户端请求之间的时间（以秒为单位）`

`Session 本质是有客户第一次访问服务器由服务器自己创建的 `
```c#
HttpSession session = request.getSession(); //如果服务器已经创建那么返回session 否则新建一个session 并返回

response.getWriter().println(session.getId());
```


##### [Session 存储在哪里](#top)  <b id="place"></b> :maple_leaf:
* `Seeion ID的唯一性如何保证的` `当用户第一次访问服务器的时候,服务器会创建一个Session对象给用户,并且产生一个唯一标识ID 存在Session 对象中 并且将此唯一Session
ID 发送到浏览器保存在浏览器中[存储在浏览器的Cookie中 名称为:JSESSIONID],作为用户的唯一标识 也就是说 唯一标识Session的ID 会存储一份在客户浏览器中,Session 对象存储在
服务器中`

* `本质上唯一标识只存储在浏览器运行内存中 也就是说关闭浏览器就会失效`

* `Session 默认生效时间 30分钟`

* `如果session 失效了 那么他又会生成一个session 并发送一个sessionID 给浏览器存储`

* **`规则`**:`如果Session 在规定的时间内没有使用就销毁 否则就重新计时`

`设置Session 强制失效`
```c#
 session.invalidate();
```


```http
Request URL: http://localhost:8089/login
Request Method: POST
Status Code: 200 
Remote Address: [::1]:8089
Referrer Policy: no-referrer-when-downgrade
Content-Length: 57
Content-Type: text/html;charset=UTF-8
Date: Mon, 26 Nov 2018 01:11:19 GMT
Set-Cookie: UserName=3444444444444444
Set-Cookie: UserPassword=23423424
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cache-Control: max-age=0
Connection: keep-alive
Content-Length: 40
Content-Type: application/x-www-form-urlencoded
Cookie: Webstorm-66e715af=5d7e1fa0-a9e9-4c01-9c16-aaff75cf10f7; JSESSIONID=2AF4A84C2F8FC452F54C77D3092B2E1B; 
UserName=3444444444444444; UserPassword=23423424
Host: localhost:8089
Origin: http://localhost:8089
Referer: http://localhost:8089/Login.jsp
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko)
Chrome/69.0.3497.100 Safari/537.36
```

##### [Session 存储数据](#top)  <b id="store"></b> :maple_leaf:
`通过如下访问 获取或者存储Session数据 当然你要保证你这次的Session 是你上次存储的Session`
* `void	setAttribute(java.lang.String name, java.lang.Object value)` :`使用指定的名称将对象绑定到此会话。`
* `java.lang.Object	getAttribute(java.lang.String name)` :`返回在此会话中使用指定名称绑定的对象，或者 null如果名称下没有绑定任何对象。`

`最好用一个属性存储一个值表示 此时是否具有 有效Session 例如`
```c#

  session.setAttribute("isLoginStore",true);
  session.setAttribute("id",userName);
  session.setAttribute("pwd",userPassword);

  //获取
  if(getAttribute(isLoginStore) != null){
     session.getAttribute("id");
     session.getAttribute("pwd");
  }else{
     response.getWriter().println("登录超时！请重新登录");
  }

```
##### [获取 Session](#top)  <b id="get"></b> :maple_leaf:
`如下方法 就可以了`
```c#
HttpSession session = request.getSession();
```























