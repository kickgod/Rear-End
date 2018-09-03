<a id="top" href="#top"> ASP.NET MVC过滤器 :maple_leaf:</a> 
----
`过滤器(Filter) 吧附加逻辑注入到MVC框架请求处理,他们提供了一种简单而雅致的方式,实现了交叉关注,所谓交叉关注,是指可以用于整个应用程序,而又不适合
放置在某个局部位置的功能,否则会打破关注分离模式,典型的例子就是页面需要登录才能访问`

- [x] :maple_leaf: <a href="#WithFunction">`简单的配置一个使用Authorize 的例子`</a>
- [x] :maple_leaf: <a href="#FilterInterface">`过滤器简洁`</a>
- [x] :maple_leaf: <a href="#">``</a>
- [x] :maple_leaf: <a href="#">``</a>

####  <a id="WithFunction" href="#WithFunction">简单的配置一个使用Authorize 的例子</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
##### 1. 在根目录下的Web.config文件中设置网站验证方式
```xml
  <authentication mode="Forms"> 
    <forms loginUrl="~/Account/Login" timeout="2880"/>
  </authentication>
```
`这支持表单验证,注意mode 要用Forms 不要写Clear 已经被微软弃用了,下面这种方式微软已经弃用`<Br/>
`如果用户尚未完成认证,将会被传送到 Account 控制器的Login方法中,重定向,且会向方法传递一个ReturnUrl参数,指明返回路径`
```xml
  //这是错误的实例 不要使用这个
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
##### 2.添加一个Account控制器和 Login方法
* `控制器方法中我们使用Cookie存储了用户信息 保存时间为一天,然后为用户发放了一个身份验证票证 使用SetAuthCookie方法`
```C#
  public class AccountController : Controller
  {
      // GET: Account
      public ActionResult Index()
      {
          return View();
      }
      public ActionResult Login()
      {
          return View();
      }
      [HttpPost]
      public ActionResult Login(string username,string userpassword,string returnurl)
      {
          JsonData data = new JsonData();
          data["UserID"] = username;
          data["UserPwd"] = userpassword;
          String UserData = JsonMapper.ToJson(data).ToString();

          HttpCookie ck_User = new HttpCookie("User");
          ck_User.Values.Add("UserData", UserData);
          ck_User.Values.Add("LoginTime", DateTime.Now.ToLocalTime().ToString());
          Random ran = new Random();
          int valtype = ran.Next(1, 2000)*3;
          ck_User.Values.Add("Type", valtype.ToString());

          ck_User.Expires = DateTime.Now.AddDays(1); //不设置的话,那么关闭浏览器回话结束就没有了 设置有效期
          Response.Cookies.Add(ck_User);

          FormsAuthentication.SetAuthCookie(username, false);
          return Redirect(returnurl ?? Url.Action("Index", "Home"));
      }
  }
```
##### 3.添加一个Admin控制器
```C#
public class AdminController : Controller
{
    public ActionResult Index()
    {
        if (!Request.IsAuthenticated) //这段代码会检测用户是否登录如果没有登录会跳转到Account控制器的Login方法中
        {
            FormsAuthentication.RedirectToLoginPage();
        }
        ViewBag.User = Request.Cookies["User"].Values.Get("UserData");
        ViewBag.LoginTime = Request.Cookies["User"].Values.Get("LoginTime");

        return View();
    }
    public ActionResult List()
    {
        if (!Request.IsAuthenticated)
        {
            FormsAuthentication.RedirectToLoginPage();
        }
        return View();
    }
}
```
`每一个控制器都这样写一个判断实在是太累了所以我们使用方法注解` `这样代码就非常的简洁了`
```C#
    public class AdminController : Controller
    {
        [Authorize]
        public ActionResult Index()
        {
            ViewBag.User = Request.Cookies["User"].Values.Get("UserData");
            ViewBag.LoginTime = Request.Cookies["User"].Values.Get("LoginTime");
            return View();
        }
        [Authorize]
        public ActionResult List()
        {
            return View();
        }
    }
```
##### 4.为了测试的在主页每次启动包 验证票证取消通过SingOut方法
```C#
  public ViewResult Index()
  {
      FormsAuthentication.SignOut();
      return View();
  }
```
####  <a id="FilterInterface" href="#FilterInterface">过滤器简介</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`MVC支持五种不同类型的过滤器,每一种类型让你能够在请求处理的不同点上面引入逻辑`

|过滤器类型|接口|注解默认实现|描述|
|:-----:|:------|:----|:----|
|**`认证过滤器`**|`IAuthenticationFilter`|`N/A`|`最先运行,在任何其他过滤器和动作方法之前,但在授权过滤器之后可以再次运行`|
|**`授权过滤器`**|`IAuthorizationFilter`|`AuthorizeAttribute`|`在认证之后,其他过滤器或动作方法之前运行,第二个运行`|
|**`方法过滤器`**|`IActionFilter`|`ActionFilterAttribute`|`在动作方法之前和之后运行里面包含两个方法`|
|**`结果过滤器`**|`IResultFilter`|`ActionFilterAttribute`|`在动作结果被执行之前和之后运行`|
|**`异常处理过滤器`**|`IExceptionFilter`|`HnadleErrorAttribute`|`仅在另一个过滤器,方法或动作结果抛出异常的时候运行`|

`在MVC框架调用一个动作之前,会首先检测该方法的定义,以查看它是否具有实现这些接口的注解属性,如果有那么便会在请求处理程序的相应点上调用这些接口
所定义的方法,框架包含了一些默认注解属性类,他们实现了这些过滤器接口`

##### <a href="#top">Authorize :arrow_up:</a>
`这个授权过滤可以适用于方法或类`
   * `如果把它直接注解到类上面那么整个类的方法都相当于被加上了这个注解`
   * `上面已经讲了它的用法`
`如果在一个类上面加上了这个注解,而又向某一个方法可以匿名访问呢 使用注解` **`AllowAnonymous`**
```C#
  [Authorize]
  public class AdminController : Controller
  {
      public ActionResult Index()
      {
          ViewBag.User = Request.Cookies["User"].Values.Get("UserData");
          ViewBag.LoginTime = Request.Cookies["User"].Values.Get("LoginTime");
          return View();
      }
      [AllowAnonymous]
      public ActionResult List()
      {
          return View();
      }
      ...
  }
```
##### 自定义一个授权过滤器
* `一般哈一些自以为是的程序总以为自己能够写出很好的安全性代码,但是有时候也搞得一团糟,这实际上是一少部分人才有的技能所以,你一般不要在刚开始接触
MVC的时候就自己去编写一个继承IAuthorizationFilter的方法`
* `一个更加安全的方法就是,编写一个` `AuthorizeAttribute` `的子类,让他照管所有棘手的事情,而且编写自定义的授权带是很容易的`
```C#
  public class MyFilterAttribute: AuthorizeAttribute
  {
      private Boolean isCanAllowedAnonymous { get; set; }

      public MyFilterAttribute(Boolean isCanAnonymous)
      {
          this.isCanAllowedAnonymous = isCanAnonymous;
      }
      protected override bool AuthorizeCore(HttpContextBase httpContext)
      {
          if (!isCanAllowedAnonymous)
          {
              if (httpContext.Request.Cookies["User"] == null)
              {
                  return false;
              }
              else
              {
                  return true;
              }
          }
          return true;
      }
  }
```
##### 参数HttpContextBase
* `传递的参数为HttpContextBase 对象它提供的是对请求信息进行访问的方法,而不是访问运用该注解属性的控制器和动作方法的信息`
##### 使用它
```C#
  public class AdminController : Controller
  {
      [MyFilter(false)]
      public ActionResult Index()
      {
          ViewBag.User = Request.Cookies["User"].Values.Get("UserData");
          ViewBag.LoginTime = Request.Cookies["User"].Values.Get("LoginTime");
          return View();
      }
      [MyFilter(false)]
      public ActionResult List()
      {
          return View();
      }
  }
```
##### 使用异常过滤器
`只有抛出未处理异常的时候,异常过滤器才会执行,这种异常来自以下位置,另一种过滤器,动作方法本身,当动作结果被执行`
```C#
    public class MyExceptionFilter : FilterAttribute, IExceptionFilter
    {
        public void OnException(ExceptionContext filterContext)
        {
            throw new NotImplementedException();
        }
    }
```
参数： ExceptionContext filterContext
`派生自ControllerContext,提供了许多有用的属性和方法,可用于获取关于请求的信息`
##### ControllerContext 有用的属性
----
* `Controller`:`类型：ControllerBase`，`返回请求的控制器对象`
* `HttpContext`:`类型:HttpContextBase`,` 获取或设置 HTTP上下文。`
* `IsChildAction`:`类型:Boolean`,`获取一个值，该值指示关联的操作方法是否为子操作。`
* `RequestContext`:`类型:RequestContext`,`获取或设置请求上下文。`
* `RequestContext`:`类型:RequestContext`,` 获取或设置 URL 路由数据。`


####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
