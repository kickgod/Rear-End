### [Request Cookie](#top) <b id="top"></b> :maple_leaf:

----
`用于存储信息,在浏览器本地,在网站响应过程中附加在http请求中发送给服务器`

- [x] :maple_leaf: [`创建一个Cookie`](#create) 
- [x] :maple_leaf: [`Cookie 存储在哪里`](#place) 
- [x] :maple_leaf: [`获取Cookie`](#get) 

##### [创建一个Cookie](#top)  <b id="create"></b>
`如果不设置Cookie的 MaxAge 最大存储时间 那么默认会存储在浏览器运行内存之中 浏览器关闭之后 Cookie 信息消失`
`构造函数 Cookie(java.lang.String name, java.lang.String value) 构造具有指定名称和值的cookie。`
```c#
  Cookie userName = new Cookie("UserName",UserID);
  Cookie userPassword = new Cookie("UserPassword",UserPwd);
  
  response.addCookie(userName);
  response.addCookie(userPassword);
```
* `setMaxAge(int expiry)`:`以秒为单位设置cookie的最大年龄。 返回值: void`
* `setPath(java.lang.String uri)`:`指定客户端应返回cookie的cookie的路径 返回值: void`
* `setComment(java.lang.String purpose)`:`指定描述cookie用途的注释。返回值: void`
* `setValue(java.lang.String newValue)`:`创建cookie后，为cookie分配新值。 返回值: void`
* `以上四个方法都有对应的get方法 返回类型为设置的类型`
<br/>

> `如果设置了存储时间那么cookie信息就存储在浏览器文件中也就是说硬盘上面 如果不设置有效路径那么每一次对服务器的请求都会带着cookie信息 而且由于会明文保存在
http协议中所以推荐对存储的cookie信息进行加密存储`


```c#
 userName.setMaxAge(60*60*24); //保存一天
 userPassword.setMaxAge(60*60*24); //保存一天

 userName.setPath("/Login.jsp"); //只有在请求/login.jsp 的时候才把cookie信息 加到http请求中
```


##### [Cookie 存储在哪里](#top)  <b id="place"></b>
`Cookie 存储在HTTP 请求中 的Cookie 字段中 且你存什么它保存什么 本身并不加密存储`
```http
Request URL: http://localhost:8089/index.jsp
Request Method: GET
Status Code: 200 
Remote Address: [::1]:8089
Referrer Policy: no-referrer-when-downgrade
Content-Length: 145
Content-Type: text/html;charset=UTF-8
Date: Mon, 26 Nov 2018 00:29:15 GMT
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cache-Control: max-age=0
Connection: keep-alive
Cookie: UserPassword=123123;B6348CE35C47ED815CC1738C9E315A59;UserName=ad123123
Host: localhost:8089
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) 
Chrome/69.0.3497.100 Safari/537.36
```
##### [Cookie 获取Cookie](#top)  <b id="get"></b>
`小心一种特殊情况 就是用户注销了自己的信息 从数据库中删除了自己的信息 但是自己的cookie并没有被删除的问题`
```c#
 int islogin = 0;
 for (Cookie ck : request.getCookies()) {
     if(ck.getName().equals("UserName")){ //不要用 == 去判断
         response.getWriter().println("已经登录成功");
         islogin ++;
     }
 }

 if(islogin == 0){
     response.getWriter().println("<a href='/Login.jsp'>点击登录</a>");
 }
```
