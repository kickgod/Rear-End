### [Request 对象](#top) <b id="top"></b> :maple_leaf:

----
`Request 对象是一个专门用来存储HTTP 请求信息的对象`

- [x] :maple_leaf: [`Request对象的由来`](#request) 
- [x] :maple_leaf: [`Request功能`](#func) 

##### [Request对象的由来](#top)  <b id="request"></b>
(1).`浏览器发送给服务器的HTTP请求既要存储又要保证其完整性，Servlet的解决方法是创建一个对象进行存储,服务器每接收一个HTTP请求，就专门创建一个对象对该请求信息
进行存储,这个对象就是request对象`<br/>

(2).`服务器接收到HTTP 请求创建Request 对象，对象中存储与此次请求相关的数据 服务器调用Servlet的时候 将Request对象作为参数传递给 Servlet 方法`

##### [Request功能](#top)  <b id="func"></b>
* `获取请求头数据`
   * `getMethod()`:`获取请求方式 [GET,POST或PUT]`
   * `getRequestURL()`:`获取请求路径`
   
   <br/>
   
   ```Java
     System.out.println(req.getMethod()); //GET 
     System.out.println(req.getRequestURL()); //http://localhost:8089/servlet/update
   ```
* `获取请求行数据`
  * `java.lang.String	getHeader(java.lang.String name) `: `以a形式返回指定请求标头的值String。`
  * `java.util.Enumeration	getHeaderNames() `:`返回此请求包含的所有标头名称的枚举。`
  * `java.util.Enumeration	getHeaders(java.lang.String name) `:`返回指定请求头作为的所有值Enumeration的String对象。`
  <br/>
  
  ```java
    Enumeration enums = request.getHeaderNames();
    while (enums.hasMoreElements()){
        System.out.println(enums.nextElement());
    }

   /*
    host:localhost:8089
    connection:keep-alive
    upgrade-insecure-requests:1
    user-agent:Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36
    accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
    accept-encoding:gzip, deflate, br
    accept-language:zh-CN,zh;q=0.9,en;q=0.8
    cookie:JSESSIONID=F054B1A83D51D3320C177469DC567048; Webstorm-66e715af=5d7e1fa0-a9e9-4c01-9c16-aaff75cf10f7
   */
  ```
  
* `获取用户数据`
  * `java.lang.String`	`getParameter(java.lang.String name) `:`以a为参数String，或者null如果参数不存在，则返回请求参数的值。`
  * `java.util.Map`	`getParameterMap()`:`返回此请求的参数的java.util.Map。`
  * `java.util.Enumeration`	`getParameterNames()  `:` 返回 包含此请求中包含的参数名称Enumeration的String对象。`
  * `java.lang.String[]`	`getParameterValues(java.lang.String name) `:`返回String包含给定请求参数具有的所有值的对象数组，或者 null参数是否不存在。`
  * ` java.lang.String`	`getQueryString()` `返回路径后请求URL中包含的查询字符串。`
  * `Bool` `isRequestedSessionIdValid() ` `   检查请求的会话ID是否仍然有效。`
* `请求转发`:`request.getRequestDispatcher("Login.jsp").forward(request,response);`   
  
  ```java
   //请求转发之后要加 return   在之后 一般
   request.getRequestDispatcher("Login.jsp").forward(request,response);
   return;
  ```

