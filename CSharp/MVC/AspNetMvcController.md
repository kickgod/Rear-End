<a id="top" href="#top"> 控制器[Controller] :maple_leaf:</a> 
------
`Controller负责响应用户Http请求[用户输入]并将处理的信息返回给浏览器,并在响应过程中修改模型,通过这种方式,MVC模型,中的控制器主要关注的是应用程序流
,输入数据的处理,以及对相关视图输出数据的模块.`
* :maple_leaf: 控制器的方法觉得返回那个视图,给视图传递怎样是数据
* :maple_leaf: 控制器的方法处理不同的请求
-----
* [x] :maple_leaf: <a href="#ControllerAgreement">控制器约定</a>
* [x] :maple_leaf: <a href="#CreateStringController">创建一个返回String类型的控制器/映射规则</a>
* [x] :maple_leaf: <a href="#CreateViewController">返回视图</a>
* [x] :maple_leaf: <a href="#CreateParameterController">方法参数</a>

####  <a id="MVC2" href="#MVC2">控制器约定</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`控制器`:`响应用户的HTTP请求并将处理后的信息通过HTML页面的形式返回给浏览器`
* **`命名方式`**:`名称后面跟一个Controller`   `例如`:`HomeController`,`StudentController`,`UserController`
* **`存放位置`**:`放在根目录下面的Controller`
* **`联系`**:`每一个控制器在根目录的Views目录下面以其前缀名称相同的文件例如 HomeController 在Views下面有一个Home文件夹用于存放视图的文件`
* **`用户创建的控制器`**:`每一个用户控制器都继承自Controller基类`

####  <a id="CreateStringController" href="#CreateStringController">创建一个返回String类型的控制器</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
* `添加一个控制器 名称为UserController,然后启动项目`
```C#
    public class UserController : Controller
    {
        // GET: User
        public String Index()
        {
            return "Hello The Wolrd";
        }
    }
```
* `http://localhost:62738/User 访问,会返回Hello the World 控制器将字符串返回到浏览器了.`
* `这是根据路由配置规则来觉得响应URL映射到控制器的方法,配置类如下 App_Start/RouteConfig.cs`
```C#
    public class RouteConfig
    {
        public static void RegisterRoutes(RouteCollection routes)
        {
            routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

            routes.MapRoute(
                name: "Default",
                url: "{controller}/{action}/{id}", // 名称/方法名/数据ID
                defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional } 
                //默认控制器Home 默认方法Index
            );
        }
    }
```
####  <a id="CreateViewController" href="#CreateViewController">返回视图</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
-----
`控制器名称为UserController `
* 控制器方法返回值必须是 `ActionResult 类型`
* `return View(); 这样方法返回的视图和方法同名 如果方法名为 Abort 那么返回就是 Views/User/Abort.cshtml`
* `将鼠标点在方法名称上面右键就可以添加视图`
##### 返回默认的视图
```C#
    public ActionResult Index()
    {
        return View();
    } //返回Index.cshtml 的内容 
```
##### 返回指定名称的方法
```C#
    
    public ActionResult Abort() //添加一个方法 然后添加一个 Myview.cshtml的视图
    {
        return View("Myview");
    }
    //启动项目 http://localhost:62738/User/Abort 最后返回的是Myview.cshtml 的内容
```
####  <a id="CreateParameterController" href="#CreateParameterController">方法参数</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
```C#
    public String Abort(String Name)
    {
        return $"Name:{Name}";
    }
    //如此请求 http://localhost:62738/User/Abort?Name=Wangzhe
    //返回内容为: Name:Wangzhe
```
##### 多个参数
```C#
    public String Abort(String Name,String Age)
    {
        return $"Name:{Name} Age:{Age}";
    }
    //如此请求:http://localhost:62738/User/Abort?Name=Wangzhe&Age=15
    //返回内容为:Name:Wangzhe Age:15

```





















