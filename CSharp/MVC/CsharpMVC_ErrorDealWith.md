<a id="top">:checkered_flag:</a> [ASP.NET MVC错误处理](#)
----
- [x] <a href="#ControllerXmlAttack">`控制器跨站点脚本攻击`</a> :white_check_mark:

- [x] <a href="#404Pages">`404页面重定向`</a>



#### <a id="ControllerXmlAttack"> 控制器跨站点脚本攻击</a>  <a href="#top"> `置顶` :arrow_up:</a>
`这个有时候因为黑客跨站脚本攻击:错误的URL 导致` [`ASP.NET中请求验证`](https://msdn.microsoft.com/en-us/library/hh882339.aspx) `引发的错误黄页`
* `您可以禁用整个应用程序的请求验证，但不建议这样做。建议仅针对要允许标记的虚拟路径或特定页面选择性地禁用请求验证。`

----
##### 在ASP.NET MVC中禁用请求验证
`要在ASP.NET MVC应用程序中禁用请求验证，必须将请求验证更改为在请求处理序列中更早发生，如前面对ASP.NET Web窗体所述。在Web.config文件中，进行以下设置：`
```xml
<system.web> 
  <httpRuntime requestValidationMode =“2.0”/> 
</system.web>
```
`要禁用操作方法的请求验证，请使用属性ValidateInput（false）标记该方法，如以下示例所示：`
```C#
[HttpPost] 
[ValidateInput（false）]
 public ActionResult Edit（string comment）
{ if（ModelState.IsValid）
    { // Etc. 
    } return View（comment）; 
}
```
`要禁用特定属性的请求验证，请使用AllowHtml属性标记属性定义：`
```C#
[AllowHtml]
 public  string Prop1 { get;  set; }
```
-----
案例说明: `下面的方法如果被跨站脚本攻击:会触发ASP.NET的HTTP请求验证错误,导致黄页错误,这里我们尝试将其错误引入到一个自定义的错误页面`

* `1.我们给这个方法添加注解禁止验证, 低版本在ASP.NET MVC中禁用请求验证web.config   <httpRuntime requestValidationMode =“2.0”/> [高版本MVC5可以不加了]`
```C#
      // GET:/Default/ShowInfo?UserName=JiangXing
      [HttpPost]
      [ValidateInput(false)]
      public String ShowInfo(String UserName)
      {
          try
          {
              String Message = HttpUtility.HtmlEncode($"UserName:{UserName}");
              //利用HttpUtility.HtmlEncode 来预处理用户输入,防止跨站脚本攻击  当然这里会产生黄页错误
              //例如:/Default/ShowInfo?UserName=<script>window.location='http://hacker.example.xom'</script>
              return UserName;
          }
          catch(Exception ex)
          {
              return "URL错误";
          }
          return "错误信息";
      }
```
* `2.在Global.asax 添加一个Application_Error 方法用于处理HttpException 重定位到一个控制器的方法处理错误`
`这里是添加了一个ErrorController 方法 下面页面重定向到 ErrorController的Index方法了 然后给Index 返回一个视图,这就可以成为一个错误或者404页面了`
```C#
    protected void Application_Error(object sender, EventArgs e)
    {
        Exception ex = Server.GetLastError();
        if (ex is HttpException && ((HttpException)ex).GetHttpCode() == 404)
        {
            Server.ClearError();
            Response.Redirect("/Error");
        }
    }
```
#### <a id="404Pages"> 404页面重定向</a>  <a href="#top"> `置顶` :arrow_up:</a>
`在Global.asax 添加一个Application_Error 方法用于处理HttpException` [`处理404错误`](https://www.cnblogs.com/jiyang2008/p/4948130.html)
```C#
    protected void Application_Error(object sender, EventArgs e)
    {
        Exception ex = Server.GetLastError();
        if (ex is HttpException && ((HttpException)ex).GetHttpCode() == 404)
        {
            Server.ClearError();
            Response.Redirect("/ControllerName/FunctionName");
        }
    }
```
