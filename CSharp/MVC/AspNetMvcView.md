<a id="top" href="#top">视图[View] :maple_leaf:</a> 
----
`视图就是返回给用户浏览的HTML文档,视图的职责就是想用户提供用户界面,视图可以使用布局页,也可以不使用布局页,布局页类似于WEB From的母版页`

- [x] :maple_leaf: <a href="#DefaultMvcData">`向视图传递数据[非强类型视图]`</a>
- [x] :maple_leaf: <a href="#ClasstMvcData">`向视图传递数据[强类型视图]`</a>
- [x] :maple_leaf: <a href="#ViewJiJia">`视图基架`</a>
- [x] :maple_leaf: <a href="#Razor">`Razor 视图引擎`</a>
- [x] :maple_leaf: <a href="#RazorLayout">`Razor 视图布局`</a>
- [x] :maple_leaf: <a href="#RazorPart">`部分视图`</a>

####  <a id="DefaultMvcData" href="#DefaultMvcData">`向视图传递数据[非强类型视图]`</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`一般我们可以通过两种方式添加数据,这里我们介绍ViewBag/ViewData 接着说Razor 在将强类型视图,强类型视图最好结合表单使用`
* 1.采用ViewBag/ViewData 加载数据
* 2.采用强类型视图

##### 使用ViewBag 传递数据
`ViewBag适用于向视图传递少量数据,数据可以通过属性动态创建,也可以痛ViewData字典形式传递数据。`

##### 注意：ViewBag和ViewData的差异
* `只有当要访问的关键字是一个有效的C#标识符时，ViewBag才起作用。例如，如果在ViewData["Key With Spaces"]中
存放一个值，那么就不用使用ViewBag访问，因为无法通过编译。`
* `动态值不能作为一个参数传递给扩展方法，因为C#编译器为了选择正确的扩展方法，在编译时必须知道每一个参数的真正类型。`
* `例如 @Html.TextBox("name",ViewBag.Name).要使得这行代码通过编译有两个方法:第一是使用ViewData[“Name”],第
   二个吧ViewBag.Name转换为一个据图 的类型(string)ViewBag.Name.`  

```C#
  public ActionResult Index()
  {
      ViewBag.Message = "ASP.NET MVC";
      ViewData["Name is"] = "CMS系统开发实战经典";
      return View();
  }
```

```html
  @{
      Layout = null;//不使用布局页
  }
  <!DOCTYPE html>
  <html>
  <head>
      <meta name="viewport" content="width=device-width" />
      <title>Index</title>
  </head>
  <body>
      <div>
            @ViewBag.Message <span>书籍推荐:</span> @ViewData["Name is"]      
      </div>
  </body>
  </html>
  //启动结果：ASP.NET MVC 书籍推荐: CMS系统开发实战经典
```
`大多时候我们都使用的是ViewBag,相对对字典ViewData 语法更加优美,不需要类型转换.`

####  <a id="ClasstMvcData" href="#ClasstMvcData">`向视图传递数据[强类型视图]`</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`视图通常需要显示各种没有直接映射到域模型的数据,例如需要视图来显示商品的信息`


##### 模型类
```C#
    namespace NetMvc.Models
    {
        public class CsharpGeneration
        {
            public string Desc { get; set; }
            public float Generation { get; set; }

            public CsharpGeneration(float id,string Intr)
            {
                Desc = Intr;
                this.Generation = id;
            }
        }
    }
```
##### 控制器
```C#
    public ViewResult MvcHistory()
    {
        List<CsharpGeneration> Alist = new List<CsharpGeneration>()
        {
            new CsharpGeneration(1,"2002年c#第一个版本发行了"),
            new CsharpGeneration(2,"c#第二个版本发行加入web 和数据库编程 事件组件驱动成为C#的标志"),
            new CsharpGeneration((float)3.5,"Linq成为C#的标准支持"),
            new CsharpGeneration(4,"C#在事件编程上走向成熟 推出了MVC模式 C#的新时代来领了"),
            new CsharpGeneration((float)4.5,"异步多线变得更加方便！MVC成为流行编程方式 
            C#支持游戏 移动app开发 wpf"),
            new CsharpGeneration(6,"C#迎来跨平台时代")
        };
        return View(Alist);
    }
```
##### 使用基架创建视图 
* `方法名称上右键创建视图`
* `Template 选择List`
* `Model class 选择:CsharpGeneration`
* `使用layout page布局页 系统就自动生成了如下代码`

##### 视图HTML辅助方法代码
```html
    
    @model IEnumerable<NetMvc.Models.CsharpGeneration>

    @{
        ViewBag.Title = "MvcHistory";
    }

    <h2>MvcHistory</h2>

    <p>
        @Html.ActionLink("Create New", "Create")
    </p>
    <table class="table">
        <tr>
            <th>
                @Html.DisplayNameFor(model => model.Desc)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.Generation)
            </th>
            <th></th>
        </tr>

    @foreach (var item in Model) {
        <tr>
            <td>
                @Html.DisplayFor(modelItem => item.Desc)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Generation)
            </td>
            <td>
                @Html.ActionLink("Edit", "Edit", new { /* id=item.PrimaryKey */ }) |
                @Html.ActionLink("Details", "Details", new { /* id=item.PrimaryKey */ }) |
                @Html.ActionLink("Delete", "Delete", new { /* id=item.PrimaryKey */ })
            </td>
        </tr>
    }
    </table>
```
##### 名称空间的问题
`很多的类的使用需要引入名称空间,如果我们使用模板常见,那么VS会帮我们自动引入寻找类的命名空间,然后插入
到web.config配置文件中,否则我们就需要自动引入
例如:` `@using NetMvc.Models;    `
* `但是按照约定,应该把使用到的命名空间写入web.config中,如果使用模板Template创建 VS会自动帮我们做这件事情,
如果模板为Empty就需要自己动手了`

```xml
  <system.web.webPages.razor>
    <host factoryType="System.Web.Mvc.MvcWebRazorHostFactory, System.Web.Mvc, Version=5.2.4.0, Cul
    ture=neutral, PublicKeyToken=31BF3856AD364E35" />
    <pages pageBaseType="System.Web.Mvc.WebViewPage">
      <namespaces>
        <add namespace="System.Web.Mvc" />
        <add namespace="System.Web.Mvc.Ajax" />
        <add namespace="System.Web.Mvc.Html" />
        <add namespace="System.Web.Optimization"/>
        <add namespace="System.Web.Routing" />
        <add namespace="NetMvc" />
      </namespaces>
    </pages>
  </system.web.webPages.razor>
```

####  <a id="ViewJiJia" href="#ViewJiJia">`视图基架`</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>

|基架|描述|
|:---|:----|
|`Create`| 创建模型的表单 |
|`Delete`|删除表单|
|`Detail`|视图展示模型每一个属性的标签及其相应值|
|`Edit`|创建一个视图,显示修改表单|
|`Empty`|不使用视图空的 只有一个@model 指定模型类型|
|`Empty(Without model)`|空视图|
|`List`|创建一个带有IEnumerable泛型 模型的展示列表 |

* **`Create as a partial view`**:`选择此表示创建的视图不是一个完整的视图,此时Layout是不可选生成视图除了在其顶部没有html 和 head标签之外没有
什么不同了`
* **`Reference Script Libraries`**:`这个选项用来指示要创建的视图是否应该包含指向JavaScript库的引用,模型情况下Layout.cshtml不引入jQuery Validation库
也不引入Unobtrysive jQuery Validation库 之引入主Jquery库,但视图创建为Create /Edit时最好引入`
* **`Use a Layaout page`**:`使用布局页面 默认布局在 _ViewStart.cshtml已经指定了`
```html
  @{
      Layout = "~/Views/Shared/_Layout.cshtml";
  }
```
####  <a id="Razor" href="#Razor">`Razor 视图引擎`</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`Razor视图引擎可以在视图中添加C#代码,解析数据,Razor 是一种标记语法，用于将基于服务器的代码嵌入网页中。 Razor 语法由 Razor
标记、C# 和 HTML 组成。 包含 Razor 的文件通常具有 .cshtml 文件扩展名。`
* 注意: `Razor 支持 C#，并使用 @ 符号从 HTML 转换为 C#。 Razor 计算 C# 表达式，并将它们呈现在 HTML 输出中。
当 @ 符号后跟 Razor 保留关键字时，它会转换为 Razor 特定标记。 否则会转换为纯 C#。
若要对 Razor 标记中的 @ 符号进行转义，请使用另一个 @ 符号：`

```html
 <p>@@Username</p>
 //呈现结果@Username
```
* `包含电子邮件地址的 HTML 属性和内容不将 @ 符号视为转换字符。 Razor 分析不会处理以下示例中的电子邮件地址：`

#### 隐式Razor表达式 
`以@开头,后更C# 代码`
```C#
  @model.message
  <p>@DateTime.Now</p>
  <p>@DateTime.IsLeapYear(2016)</p>
```
* 注意：`隐式表达式不能包含空格，但 C# await 关键字除外。 如果该 C# 语句具有明确的结束标记，则可以混用空格：`
```C#
  <p>@await DoSomething("hello", "world")</p>
```
* 注意:`隐式表达式不能包含 C# 泛型，因为括号 (<>) 内的字符会被解释为 HTML 标记。 以下代码无效：`
```C#
  <p>@GenericMethod<int>()</p>
```
#### 显式 Razor 表达式
`显式 Razor 表达式由 @ 符号和平衡圆括号组成。@()  若要呈现上一周的时间，可使用以下 Razor 标记：`
```C#
  <p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
  //将计算 @() 括号中的所有内容，并将其呈现到输出中。
```
* 解决一个二义性的问题
```C#
  @{
  string Title = "迎接一个美好的未来";
  }
  <h2>@Title.新未来</h2>
```
`提示没有新未来的属性，在这种情况下Razor不能理解我们的意图， 而会认为@Title.新未来是一个代码表达式，这是可
以通过将表达式圆括号括起来以支持显示代码表达式`
```C#
  @{
  string Title = "迎接一个美好的未来";
  }
  <h2>@(Title).新未来</h2>
```
* 可以使用显式表达式将文本与表达式结果串联起来：
```C#
  @{
      var joe = new Person("Joe", 33);
  }
  <p>Age@(joe.Age)</p>
```
`如果不使用显式表达式，<p>Age@joe.Age</p> 会被视为电子邮件地址，因此会呈现 <p>Age@joe.Age</p>。 如果编写为显式表达式，则呈现 <p>Age33</p>。`
* 显式表达式可用于从 .cshtml 文件中的泛型方法呈现输出。 以下标记显示了如何更正之前出现的由 C# 泛型的括号引起的错误。 此代码以显式表达式的形式编写：
```html
  <p>@(GenericMethod<int>())</p> //错误语法
```
#### 表达式编码
`计算结果为字符串的 C# 表达式采用 HTML 编码。 计算结果为 IHtmlContent 的 C# 表达式直接通过 IHtmlContent.Write
To 呈现。 计算结果不为 IHtmlContent 的 C# 表达式通过 ToString 转换为字符串，并在呈现前进行编码。`
```C#
  @("<span>Hello World</span>")
  //浏览器显示结果:<span>Hello World</span>
  @Html.Raw("<span>Hello World</span>")
  //浏览器显示结果 Hello World
```
#### Razor 代码块
`Razor 代码块以 @ 开头，并括在 {} 中。 代码块内的 C# 代码不会呈现，这点与表达式不同。 一个视图中的代码块和表达式共享相同的作
用域并按顺序进行定义：`
```html
  @{ var quote = "The future depends on what you do today. - Mahatma Gandhi"; }
  <p>@quote</p>
  @{ quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";}
  <p>@quote</p>
  //编译后的html代码
  <p>The future depends on what you do today. - Mahatma Gandhi</p>
  <p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```
##### 隐式转换
`代码块中的默认语言为 C#，不过，Razor 页面可以转换回 HTML：`
```C#
  @{
      var inCSharp = true;
      <p>Now in HTML, was in C# @inCSharp</p>
  }
```
##### 带分隔符的显式转换
```C#
    @{
        String[] Names = new String[] {"厦王","田健","后胜","赵丹","嬴政","刘季","萧何" };
    }
    @for (var i = 0; i < Names.Length; i++)
    {
        var person = Names[i];
        @:Name: @person
    }
  //输出:Name: 厦王 Name: 田健 Name: 后胜 Name: 赵丹 Name: 嬴政 Name: 刘季 Name: 萧何
```
#### 控制结构
`控制结构是对代码块的扩展。 代码块的各个方面（转换为标记、内联 C#）同样适用于以下结构：`
* `条件语句`: `@if`、else if、else 和 `@switch`
* **`@if`**
```C#
  @if (value % 2 == 0)
  {
      <p>The value was even.</p>
  }
  else if (value >= 1337)
  {
      <p>The value is large.</p>
  }
  else
  {
      <p>The value is odd and small.</p>
  }
```
* **`@switch `**
```C#
  @switch (value)
  {
      case 1:
          <p>The value is 1!</p>
          break;
      case 1337:
          <p>Your number is 1337!</p>
          break;
      default:
          <p>Your number wasn't 1 or 1337.</p>
          break;
  }
```

#### 循环语句 
* @for、@foreach、@while 和 @do while

```C#
  @{
      var people = new Person[]
      {
            new Person("Weston", 33),
            new Person("Johnathon", 41),
            ...
      };
  }
```
* **`@for`**
```C#
  @for (var i = 0; i < people.Length; i++)
  {
      var person = people[i];
      <p>Name: @person.Name</p>
      <p>Age: @person.Age</p>
  }
```

* **`@foreach`**
```C#
  @foreach (var person in people)
  {
      <p>Name: @person.Name</p>
      <p>Age: @person.Age</p>
  }
```


* **`@while`**
```C#
  @{ var i = 0; }
  @while (i < people.Length)
  {
      var person = people[i];
      <p>Name: @person.Name</p>
      <p>Age: @person.Age</p>

      i++;
  }
```


* **`@do while`**
```C#
  @{ var i = 0; }
  @do
  {
      var person = people[i];
      <p>Name: @person.Name</p>
      <p>Age: @person.Age</p>

      i++;
  } while (i < people.Length);
```
#### 复合语句 @using
`在 C# 中，using 语句用于确保释放对象。 在 Razor 中，可使用相同的机制来创建包含附加内容的 HTML 帮助程序。 在下面
的代码中，HTML 帮助程序使用 @using 语句呈现表单标记：`
* @using
```C#
  @using System.IO
  @using (Html.BeginForm())
  {
      <div>
          email:
          <input type="email" id="Email" value="">
          <button>Register</button>
      </div>
  }
```
* @try、catch、finally
```C#
  @try
  {
      throw new InvalidOperationException("You did something invalid.");
  }
  catch (Exception ex)
  {
      <p>The exception message: @ex.Message</p>
  }
  finally
  {
      <p>The finally statement.</p>
  }
```
* @lock Razor 可以使用 lock 语句来保护关键节： 
···C#
  @lock (SomeLock)
  {
      // Do critical section work
  }
···
#### Razor 支持 C# 和 HTML 注释：

-----
#### 指令
* @using 指令用于向生成的视图添加 C# using 指令：
* @model 指令指定传递到视图的模型类型：
* @inherits 指令对视图继承的类提供完全控制：
* @inject 指令允许 Razor 页面将服务从服务容器注入到视图。 有关详细信息，请参阅视图中的依赖关系注入。
* @functions 指令允许 Razor 页面将 C# 代码块添加到视图中：
```C#
  @functions {
      public string GetHello()
      {
          return "Hello";
      }
  }

  <div>From method: @GetHello()</div> 
  //生成HTML结果  <div>From method: Hello</div>
```
#### Razor注释
```C#
 @*
     这是Razor 注释    
 *@
```
####  <a id="RazorLayout" href="#RazorLayout">`Razor 视图布局`</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`制作布局页面,布局页面类似于母版页,制作布局页需要解决三个问题如何设定填充位置,另一个是如何把页面填充到指定的位置,不填充如何不报错`
* 1. **`@RazorBody()`** :`这是一个占位符,用来标记使用这个布局的视图将渲染他们的主要内容的位置,多个Razor视图可以利用这个布局保持一致性`
* 2. **`@RazorSection(Name)`**:`添加一个布局节,和RazorBody基本一样`
* 3. **`@RenderSection("scripts", required: false)`** :`required 参数置为false 那么可以填充也可以不填充,不会报错`
* 创建布局页面
```html
  <!DOCTYPE html>
  <html>
  <head>
      <meta name="viewport" content="width=device-width" />
      <title>@ViewBag.Title</title>
  </head>
  <body>
      <div class="Main">
          @RenderBody()
      </div>
      <footer>
          @RenderSection("Footer")
      </footer>
  </body>
  </html>

```
* 使用布局页面
```html
  @{
      ViewBag.Title = "About Page";
      Layout = "~/Views/Shared/_MyLayout.cshtml";
  }

  <h2>About 这是主页面</h2>

  @section Footer{
      <h2>这是眉角</h2>    
  }
```
* 渲染结果
```html
<html><head>
    <meta name="viewport" content="width=device-width">
    <title>About Page</title>
</head>
<body>
    <div class="Main">
        <h2>About 这是主页面</h2>
    </div>
    <footer>
      <h2>这是眉角</h2>    
    </footer>
  </body>
</html>
```

####  <a id="RazorPart" href="#RazorPart">`部分视图`</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`部分视图除了没有html标签之外其他和普通视图没有什么区别,控制器在返回部分视图的时候需要视图 return PartialView();`
* 用处:`不能够指定布局页,布局视图的作用,将一个部分视图通过Ajax加载到一个正在调用Ajax的当前视图中,做异步刷新`
##### HomeController
```C#
        public ActionResult Index()
        {
            return View();
        }
        
        public ActionResult Message()
        {
            Random r = new Random();
            int t = r.Next(0, 15);
            ViewBag.Message = t;
            return PartialView();
        }
```
##### Message 视图
```html
  <h4 class="h4">
     更新中: Message:@ViewBag.Message
  </h4>
```

##### Index 视图
```html
    @{
        ViewBag.Title = "Home Page";
    }
    <style>
        #result {
            text-indent:20px;
        }
    </style>
    <div class="jumbotron">
        <h1>ASP.NET</h1>
        <p class="lead">ASP.NET is a free web framework for building great Web sites and Web applications using HTML, CSS and JavaScript.</p>
        <p><a href="https://asp.net" class="btn btn-primary btn-lg">Learn more &raquo;</a></p>
    </div>

    <div class="row">
        <div id="result"></div>
    </div>
    @section  scripts{
        <script type="text/javascript">
            $(function () {
                //每隔两秒钟刷新一下
                window.setInterval(function () {
                    $("#result").load('/Home/Message');
                },2000);

            })
          </script>
    }

```



























