### [Request Cookie](#top) <b id="top"></b> :maple_leaf:

----
`用于存储信息,在浏览器本地,在网站响应过程中附加在http请求中发送给服务器`

- [x] :maple_leaf: [`创建一个Cookie`](#create) 
- [x] :maple_leaf: [`Request功能`](#func) 
- [x] :maple_leaf: [`Request作用域`](#life) 

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
