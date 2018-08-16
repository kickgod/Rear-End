<a id="top" href="#top">ASP.NET MVC 历史版本 :maple_leaf:</a> 
----
`ASP.NET MVC 正式版本推出于2009/3/13 产生于2007年,到今日已经是有六个版本了，第一个版本不解释了`
- [x] :maple_leaf: <a href="#MVC2">`ASP.NET MVC 2`</a>
- [x] :maple_leaf: <a href="#MVC3">`ASP.NET MVC 3`</a>
- [x] :maple_leaf: <a href="#MVC4">`ASP.NET MVC 4`</a>
- [x] :maple_leaf: <a href="#MVC5">`ASP.NET MVC 5`</a>
- [x] :maple_leaf: <a href="#Agreement">`ASP.NET MVC 约定`</a>

####  <a id="MVC2" href="#MVC2">ASP.NET MVC 2</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
:fast_forward:`发布与2010年3月份主要特点如下:`:rewind:
 * `强类型的HTML辅助程序`
 * `支持异步控制器`
 * `支持使用Html.RenderAction支持渲染网页或网站的某一部分`
 * `大型应用程序划分为域`
####  <a id="MVC3" href="#MVC3">ASP.NET MVC 3</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
  * `支持Razor试图引擎` **`[非常重要]`**
  * `支持.NET数据注解 极大的方便了开发过程`  **`[非常重要]`**
  * `更改了验证模式`
  * `支持依赖注解和全局操作过滤器`
  * `丰富的JS支持,Jq验证和JSON绑定`
  * `NuGet支持` **`[重要]`**
####  <a id="MVC4" href="#MVC4">ASP.NET MVC 4</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>  
  * `ASP.NET WEB API`  **`[非常重要]`**
     * **`介绍:`** `ASP.NET MVC使得控制到字节的相应变得非常容易,而且MVC模式在创建服务层时变得容易,可以是MVC创建Web服务这些服务可以返回XML,Json或者其他非HTML格式的数据,并且比使用其他服务框架(WCF) 或编写原始的HTTP处理程序更加容易,尽管如此,MVC任然有一些不足之处,比如我们需要假设Web来传送服务,但是总体而言MVC更加优,MVC4提供了一个良好的解决方案:ASP.NET Web API，它是一个提供了ASP.NET MVC开发风格的框架,专门用来编写Htttp服务,改框架包括在HTTO服务域修改一些MVC概念,并提供了一些新的面相服务的功能`
         * **`路由`** ：`MVC API使用相同的路由系统,将URL映射到控制器操作,按照约定将HTTP动词映射到操作,从而实现将路由融入HTTP上下文,这样方便阅读和
         RESTful服务设计`
         * **`模型绑定和验证`** :`可以将HTTP请求值映射到模型`
         * **`过滤器`** :`MVC使用过滤器以便通过特性想操作添加行为,例如向某个操作添加[Authorize]特性将会组织用户的匿名访问,当用户匿名访问会重定向要登陆页面`
         * **`基架`**:`可以添加MVC控制器一样的对话框来添加API的控制器`
         * **`简易的单元测试`**:`API建立在依赖注入和避免全局状态模型类的概念之上`
         * **`HTTP编程模型`**:`为了更好地处理HTTP请求和响应,Web API开发体验得到优化,提供了一个强类型的HTTP对象模型,状态码和容易访问的header`
         * **`基于HTTP动词的动作调度`**:`MVC根据操作方法的名称来调度,web api则根据HTTP动词自动调度操作方法 例如一个GET请求会被自动调度到GetItem的控制器操作`
         * **`内容调度`**:`HTTP已经长期支持内容协商系统,在这些系统中,浏览器给出他们的响应格式优先级,服务器选取它优先支持的格式做出响应,这样我们的控制器就可以提供XML,JSON,和其他格式,来响应客户端想要的格式`
         * **`基于代码的配置`** :`WCF采用冗长复杂的配置文件来完成配置,与其不同的是,Web API完全通过代码配置.`
  * `增强了默认的项目模板`
  * `添加使用Jquery Mobile 手机项目模板`
  * `支持显示模式Dispaly Mode`
     * **`介绍:`** :`显示默认更具浏览器发出的请求,使用基于约定的方法来选择不同的视图，当浏览器的用户代理指示一台已知的移动设备时,默认的视图影响首先查找名以.Mobile.cshtml 结尾的视图,例如网站项目中有一个通用摄图和一个移动视图,他们的名称分别是Index.cshtml和Index.Mobile.cshtml 那么当在移动浏览器网站访问到该页面时,MVC5将自动使用移动视图,虽然移动浏览器的默认页面确定方式基于用户代理检测,但是也可以通过注册自定义设备模式来自定义此逻辑`
  * `支持异步控制器的任务`
  * `捆绑和微小框架`
     * **`介绍:`** :`该框架通过合并脚本引用把若干个请求合并为一个请求,从而减少发送到站点的请求数量,于此同时它采用各种技术来压缩请求的大小,比如缩短变量名,删除空格,和注释,它也很好的适应CSS可以把若干吗CSS请求打包成一个请求,并且压缩CSS请求大小,捆绑式高度配置的 App_Start/BundleConfig.cs 这样我们就可以在视图代码里面删除文件引用.这样我们就可以在不更新视图或布局的情况下,添加或升级脚本库,和那些勇于不同文件名称的CSS文件，因为引用的是脚本或CSS绑定不是单独文件,例如MVC Internet应用程序模板就包含一个不依赖于版本号的jQuery绑定`
     * `这样在站点布局(Views/Sharee/_Layout.cshtml) 就可以绑定URL来引用它`
       ```C#
           public class BundleConfig
           {
               // 有关捆绑的详细信息，请访问 https://go.microsoft.com/fwlink/?LinkId=301862
               public static void RegisterBundles(BundleCollection bundles)
               {
                   bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                               "~/Scripts/jquery-{version}.js")); //不依赖于版本号的jQuery绑定

                   bundles.Add(new ScriptBundle("~/bundles/jqueryval").Include(
                               "~/Scripts/jquery.validate*"));

                   // 使用要用于开发和学习的 Modernizr 的开发版本。然后，当你做好
                   // 生产准备就绪，请使用 https://modernizr.com 上的生成工具仅选择所需的测试。
                   bundles.Add(new ScriptBundle("~/bundles/modernizr").Include(
                               "~/Scripts/modernizr-*"));

                   bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
                             "~/Scripts/bootstrap.js"));

                   bundles.Add(new StyleBundle("~/Content/css").Include(
                             "~/Content/bootstrap.css",
                             "~/Content/site.css"));
               }
           }       
       ```
    *  在_Layout.cshtml引用
       ```csthml
           //这样引用不需要绑定到jquery绑定号 框架会自动获取最新版本,而不需修改任何代码
           @Scripts.Render("~/bundles/jquery")
       ```
####  <a id="MVC5" href="#MVC5">ASP.NET MVC 5</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
:fast_forward:`发布与2013年10月份主要特点如下:`:rewind:
  * `One ASP.NET`
     * `过去的版本MVC是 每次创建项目时候，创建一个MVC程序,Web From或其他类型,之后,实际上我们被限制住了,在某种程度上可以把Web From添加到一个MVC程序中,但是吧MVC添加到WebFrom 中是很宽单的添加时需要做一些神秘的修改,在MVC5情况发生了变化,因为现在只有一种ASP.NET 项目类型,在Visual Studio2013 中创建新的Web应用程序时,没有复杂的选项,不只是在一开始创建ASP.NET项目时才支持那么做，在不断开发的过程中,可以添加对其他框架的支持,因为工具和特性都是NuGet包提供了,开发过程中,可以使用ASP.NET基架想任何现有的ASP.NET应用程序添加MVC`
  * `新的WEB项目体验`
     * `创建项目变得简单`
  * `ASP.NET Identity`
     * `成员和身份验证系统`
  * `Bootstrap模板`
  * `特性路由`
     * `可以将注解添加到控制器类或操作方法上,留下AttributeRouting开源项目使得这种方法成为可能`
  * `ASP.NET 基架`
     * `提供了多种的创建Model-Controller-View的方式,还支持强大的自定义基架`
  * `身份验证过滤器`:`允许开发人员参与和结果执行管道,过滤器重写意味着某个控制器不执行全局过滤器`
     * 
  * `过滤器重写`
####  <a id="Agreement" href="#Agreement">ASP.NET MVC 约定</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
:fast_forward:`MVC对约定的依赖性很强,这样就避免了开发人员配置和指定一些项,因为可以通过约定推断`:rewind:
* `控制器默认存放在 Controllers文件夹中,控制器以Controller结尾例如 HomeController`
* `模型类默认放在Models文件夹中`
* `视图存放在 Views文件夹中,每个控制器类都有一个对应其名称的视图文件夹放在views文件夹下用于存放有关此控制器的视图 例如 HomeController 在views文件夹下有一个 Home 文件存放视图`
* `共享的母版视图存放在Views/Shared 文件夹中`
