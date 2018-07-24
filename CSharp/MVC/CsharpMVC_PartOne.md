<a id="top">:checkered_flag:</a> [Model-View-Controller](https://docs.microsoft.com/zh-cn/aspnet/mvc/overview/getting-started/introduction/getting-started):whale2:	
----
* `Model(模型)`:`是应用程序中用于处理应用程序数据逻辑的部分。通常模型对象负责在数据库中存取数据。`　
* `View(视图):`是应用程序中处理数据显示的部分。通常视图是依据模型数据创建的。`
* `Controller(控制器)`:`是应用程序中处理用户交互的部分。通常控制器负责从视图读取数据，控制用户输入，并向模型发送数据。`
* `使用MVC模板后 有一系列的文件生成`
----
##### [`解析ASP.NET MVC初始化文件系统和结构`](https://docs.microsoft.com/zh-cn/aspnet/mvc/overview/getting-started/introduction/getting-started)

- [x] <a href="#MVCSampleIntroduce">`控制器-视图-模型`</a> :white_check_mark:


#### <a id="MVCSampleIntroduce">1.1&nbsp;&nbsp; :sailboat: 控制器-视图-模型</a> <a href="#top"> `置顶` :arrow_up:</a>
##### 	:cocktail:控制器 

`控制器`:`响应用户的HTTP请求并将处理后的信息通过HTML页面的形式返回给浏览器`
* **`命名方式`**:`名称后面跟一个Controller`   `例如`:`HomeController`,`StudentController`,`UserController`
* **`存放位置`**:`放在根目录下面的Controller`
* **`联系`**:`每一个控制器在根目录的Views目录下面以其前缀名称相同的文件例如 HomeController 在Views下面有一个Home文件夹用于存放视图的文件`
* **`用户创建的控制器`**:`每一个用户控制器都继承自Controller基类`

-----
* `用户通过控制器中的方法返回视图或者重定向到其他方法再返回视图`,`如果方法返回为 return View(); 那么就会在Views文件对于这个控制器视图文件中查找这个
方法名对应的视图`
* `鼠标放到方法名称上面,可以创建与方法名称相对应的视图`
```C#
    public class HomeController : Controller
    {
        //默认请求
        public ActionResult Index()
        {
            return View();
        }
        //在 Views/Home 文件夹下面查找Index.cshtml视图  这个方法返回没有传递数据
          
        public ActionResult About()
        {
            ViewBag.Message = "Your application description page.";

            return View();
        }

        public ActionResult Contact()
        {
            ViewBag.Message = "Your contact page.";

            return View();
        }
    }
```
* `浏览器路由: IP地址/控制器前缀名称/方法名称[/数据]`
```XML
  http://localhost:50861/Default   //默认查找:http://localhost:50861/Default/Index 
  http://localhost:50861/Default/Index
  http://localhost:50861/Default/ShowInfo
```
---
