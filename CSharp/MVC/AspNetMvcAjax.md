<a id="top" href="#top">ASP.NET MVC Ajax 和 客户端验证  :maple_leaf:</a> 
----
`MVC框架提供了许多的脚本,并且做了需要的处理,Jquery 和Ajax 本身我们就不讲了`
- [x] :maple_leaf: <a href="#MVCAttribute">`Script 文件夹中的文件解释`</a>
- [x] :maple_leaf: <a href="#AjaxUnobtrusiveFunction">`Ajax辅助方法`</a>
  - <a href="#Required">`引入Ajax脚本`</a>
  - <a href="#StringLength">`使用Ajax`</a>
- [x] :maple_leaf: <a href="#ClienValidation">`客户端验证`</a>
####  <a id="MVCAttribute" href="#MVCAttribute">Scripts 文件夹中的文件解释</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`Scripts 文件夹的内容,如下所示,一下脚本都是非侵入式脚本,意思就是加载如html 后不惜改变html 或者css 等等的结构引起其他脚本变化无法运行等等`
```C#
bootstrap.js
bootstrap.min.js
jquery.validate.js
jquery.validate.min.js
jquery.validate.unobtrusive.js
jquery.validate.unobtrusive.min.js
jquery.validate-vsdoc.js
jquery-3.3.1.intellisense.js
jquery-3.3.1.js
jquery-3.3.1.min.js
jquery-3.3.1.min.map
jquery-3.3.1.slim.js
jquery-3.3.1.slim.min.js
jquery-3.3.1.slim.min.map
modernizr-2.8.3.js
```
* `在调试阶段 BundleConfig将会加载 js源文件,而在发布模式下,系统会自动找到并提供。.min.js 微小化版本,对于.map.js 文件,这是一个源代码映射文件,源代码映射
是一种新兴的标准,它允许浏览器将微小化,编译后的代码映射为原来编写的代码`
----
**`如果我们剔除min 文件和map 文件 那么还剩下文件如下`**
```C#
bootstrap.js
jquery.validate.js
jquery.validate.unobtrusive.js
jquery-3.3.1.js
modernizr-2.8.3.js
```
* `modernizr 是一个js库,它通过改造老版本浏览器来帮助我们构建现代气息的应用程序,在老版本浏览器中支持新的HTML5元素`
* `unobtrusive 是微软写的,支持MVC框架的 Ajax 特性 是非侵入式的脚本 MVC框架中还要包含一组Ajax辅助方法,它们可以用来创建表单和连接,不同的
是他们是异步进行的,但是这些Ajax辅助方法依赖于MVC的JQuery 扩展,如果需要使用Ajax 辅助方法 需要引入脚本jquery.nuobtrusive-ajax.js 并在试图中引用`
####  <a id="AjaxUnobtrusiveFunction" href="#AjaxUnobtrusiveFunction">Ajax辅助方法</a>  :star2: <a href="#top"> :arrow_up:</a>
`使用Ajax 辅助方法需要引入Ajax 脚本`

##### <a id="Required" href="#top">`引入Ajax脚本`</a>
```shell
//使用命令的方式
Install-Package Microsoft.jQuery.Unobtrusive.Ajax
```
**`注意`**:`然后把脚本拖到试图上面,Vs 会自动添加脚本`
##### <a href="#top" id="StringLength" >`使用Ajax`</a>
###### Ajax 请求控制器方法
```javascript
//利用Ajax 请求另一个页面
@Ajax.ActionLink("Click me now to die", "GetFuck","Home",
     new { name = "kicker" },
     new AjaxOptions {
         HttpMethod = "Get"
     },
     new { @class ="btn btn-primary" }
  )
```
```C#
public ActionResult GetFuck(String name) {
    ViewBag.Message = name;

    return View();
}
```
`如果出现错误:`：`@Ajax.ActionLink为何老是在新窗口中打开页面呢`<br/>
**`原因`** : `JS文件加载顺序不对 先加载Jquery 然后加载 ~/Scripts/jquery.unobtrusive-ajax.js, ~/Scripts/jquery.validate* `
###### Ajax 请求一个部分视图
```javascript
<div id="loadPart">
  @Ajax.ActionLink("加载部分视图", "GetPartFuck", "Home",null,
  new AjaxOptions {
      UpdateTargetId = "loadPart",
      InsertionMode = InsertionMode.ReplaceWith,
      HttpMethod = "GET"
  },
  new { @class ="btn btn-primary" }
  )
</div>
```
* `第一个参数`:`文本信息`
* `第二个参数`:`方法名称`
* `第三个参数`:`控制器名称`
* `第四个参数`:`Ajax参数`
* `第五个参数`:`Ajax配置参数`
* `第六个参数`:`Html属性配置参数`
```C#
  public ActionResult GetPartFuck()
  {
      return PartialView();
  }
```
###### Ajax 加载数据
```Javascript
<div id="loadPart">
</div>
@using (Ajax.BeginForm("Search", "Home", new AjaxOptions
{
    UpdateTargetId = "loadPart",
    InsertionMode = InsertionMode.ReplaceWith,
    OnBegin = "func_bengin", //当请求开始的时候执行的js函数 以下一样
    OnSuccess = "func_Success",
    OnFailure = "func_fialure",
    OnComplete = "func_Finish",
    LoadingElementId = "load-ajax", //请求过程中客户端框架会自动显示这个元素
    LoadingElementDuration = 3000,  //动画持续时间
    HttpMethod = "GET"
}))
{
    <input type="text" name="Key" />
    <button class=" btn  btn-danger">查询</button>
}
@section scripts{
    <script>
        function func_bengin() {
            alert("Ajax 请求开始了")
        }
        function func_Success() {
            alert("Ajax 请求成功了")
        }
        function func_fialure() {
            alert("Ajax 请求失败了")
        }
        function func_Finish() {
            alert("Ajax 请求完成了")
        }
    </script>
}
```
```C#
  public ActionResult Search(String Key)
  {
      ViewData["title"] = Key;
      List<Auto> lis = new List<Auto>()
      {
          new Auto("kicker",15),
          new Auto("wate",17),
          new Auto("watse",18)
      };
      return PartialView(lis);
  }
```
#### :maple_leaf: <a href="#top" id="ClienValidation">`客户端验证`</a>
`ASP.NET MVC框架的客户端验证总是默认开启的, 它的本质还是Jquery 验证,所以要使用它需要 jquery.validate.min.js 和jquery.validate.unobtrusive.min.js 文件 才能启用客户端验证 例如下面的属性`
```C#
  [Required(ErrorMessage ="请填写名称！违令者斩")]
  public string Name { get; set; }

  [Range(10,26,ErrorMessage ="年龄在10岁到26岁之间")][Required(ErrorMessage ="请填写年龄")]
  public int Age { get; set; }
```
`上面的验证一般在特定的场合下启用,例如表单中! 所以一般在创建视图的时候 Views:选择中有一个Reference script libraries 它的意思就是是否引入
Jquery验证脚本,当选择后就会发现在试图中多了一些代码`
```C#
@section scripts{
  @Scripts.Render("~/bundles/jqueryval")
}
```
`这个脚本并不在布局页引入,因为有些页面并不需要Jqery 验证,比例在呈现数据的时候,只是在需要的时候加载,以免性能损失`
#### :maple_leaf: <a href="#top" id="CloseClienValidation">`关闭客户端验证`</a>
`ASP.NET MVC框架的客户端验证总是默认开启的，然而你可以在web.config中设置这些行为，如果需要禁用一下属性中的一个那么把值设置为false就行,但是当你
需要逐个试图的控制这些设置,那么HTML辅助方法EnableClientValidation和EnableUnobtrusiveJavascript在一个具体试图中重写了这些配置设置`

```xml
 <appSettings>
   <add key="ClientValidationEnabled" value="true" />
   <add key="UnobtrusiveJavaScriptEnabled" value="true" />
 </appSettings>
```


































