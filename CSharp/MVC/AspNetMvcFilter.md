<a id="top" href="#top"> ASP.NET MVC过滤器 :maple_leaf:</a> 
----
`过滤器(Filter) 吧附加逻辑注入到MVC框架请求处理,他们提供了一种简单而雅致的方式,实现了交叉关注,所谓交叉关注,是指可以用于整个应用程序,而又不适合
放置在某个局部位置的功能,否则会打破关注分离模式,典型的例子就是页面需要登录才能访问`

- [x] :maple_leaf: <a href="#WithFunction">`简单的配置一个使用Authorize 的例子`</a>
- [x] :maple_leaf: <a href="#">``</a>
- [x] :maple_leaf: <a href="#">``</a>
- [x] :maple_leaf: <a href="#">``</a>

####  <a id="WithFunction" href="#WithFunction">配置一个项目来测试过滤器</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
##### 1. 在Web.config文件中设置网站验证方式
```xml
  <authentication mode="Forms"> 
    <forms loginUrl="~/Account/Login" timeout="2880"/>
  </authentication>
```
`这支持表单验证,注意mode 要用Forms 不要写Clear 已经被微软弃用了,下面这种方式微软已经弃用`<Br/>
`如果用户尚未完成认证,将会被传送到 Account 控制器的Login方法中,重定向,且会向方法传递一个ReturnUrl参数,指明返回路径`
```xml
  //这是错误的实例
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

####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
