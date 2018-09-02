<a id="top" href="#top"> ASP.NET MVC过滤器 :maple_leaf:</a> 
----
`过滤器(Filter) 吧附加逻辑注入到MVC框架请求处理,他们提供了一种简单而雅致的方式,实现了交叉关注,所谓交叉关注,是指可以用于整个应用程序,而又不适合
放置在某个局部位置的功能,否则会打破关注分离模式,典型的例子就是页面需要登录才能访问`

- [x] :maple_leaf: <a href="#WithFunction">`配置一个项目来测试过滤器`</a>
- [x] :maple_leaf: <a href="#">``</a>
- [x] :maple_leaf: <a href="#">``</a>
- [x] :maple_leaf: <a href="#">``</a>

####  <a id="WithFunction" href="#WithFunction">配置一个项目来测试过滤器</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
##### 定义静态用户凭据 在Web.config文件中
```xml
  <system.web>
    <authentication>
      <forms loginUrl="~/Account/Login" timeout="2880">
        <credentials passwordFormat="Clear">
          <user name="user" password="secret"/>
          <user name="admin" password="secret"/>
        </credentials>
      </forms>
    </authentication>
   </system.web>
```
`定义了两个用户user和admin，并给他们分配了相同的密码secret 再次使用forms认知并且LoginUrl属性来指定未认证的将会被重定向到/Account/Login.需要添加这个
控制器 等下添加`
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
