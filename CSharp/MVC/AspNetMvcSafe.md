<a id="top" href="#top">安全性/加密/攻击方式  :maple_leaf:</a> 
----
`保护Web 应用程序的安全性看起来是一件苦差事,这件必须要做的工作并不能带来太多的乐趣，但是这缺非常的重要`

- [x] :maple_leaf: <a href="#ASPNETMVCSAFE">`ASP.NET MVC 安全说明`</a>
- [x] :maple_leaf: <a href="#codeAjaxaData">`MVC的 威胁`</a>
- [x] :maple_leaf: <a href="#EncodeHtml">`HTML JavaScript 编码`</a>
- [x] :maple_leaf: <a href="#CSRFA">`阻止CSRF攻击`</a>
- [x] :maple_leaf: <a href="#CookieLost">`使用HttpOnly阻止Cookie窃取`</a>

#####  <a id="ASPNETMVCSAFE" href="#ASPNETMVCSAFE">ASP.NET MVC 安全说明</a>  :star2: <a href="#top"> :arrow_up: </a>
`ASP.NET MVC不像ASP.NET Web form那样提供了很多自动保护机制来保护页面不受恶意用户的攻击,例如：`
 * `服务器组件对显示的值进行HTML编码,以及帮助XSS攻击`
 * `加密和验证视图状态,从而帮助阻止篡改提交的表单`
 * `请求验证<% @page validationquest="true" %> 接货开起来像是恶意的数据,并给出警告,这是ASP.NET MVC框架默认开启的保护`
 * `事件验证帮助阻止注入攻击和提交无效值` <br/>
`当转向MVC之后意味着这些工作都需要程序员了` 
#####  <a id="codeAjaxaData" href="#codeAjaxaData">MVC的 威胁</a>  :star2: <a href="#top"> :arrow_up: </a>
* 准则:`永远都不要相信用户提供的任何数据`
  * `每当渲染作为用户输入引入的数据的时候,请求其进行编码,最常见的做法就是HTML编码,然后如果数据作为特性值显示就应该对其进行HTML编码如果数据要用到
  JavaScript中就要对其尽心JavaScript编码`
  * `在不需要通过客户端脚本访问cookie的时候最好使用http-only-cookie`
  * `请注意外部输入不仅仅包括客户端form 还包括URL 隐藏表单域,Ajax请求`
  * `考虑哪些网站需要匿名访问,那么需要认真登录`
  * `页面最好使用Authorize 特性登录认真/主要对于过滤器的使用,过滤非法操作`
  * `IIS部署同样需要注意很多事项,身份验证设置等等`
#####  <a id="EncodeHtml" href="#EncodeHtml">HTML JavaScript 编码</a>  :star2: <a href="#top"> :arrow_up: </a>
`Html.Encode ` `Html.AttributeEncode` `方法可实现对于特性值的 编码替换，页面的每一点输出应该是进过HTML 或者HTML 特性编码的`

* `值得一提的是`：`Razor 视图引擎默认对输出内容采用HTML编码`
* `对于HTML属性最好使用 AttributeEncode 编码`
```C#
  <a href="@Url.Action("Index","Home",new { name=Html.AttributeEncode(ViewData["name"]) })">Go Home</a>
```
* `UrlEncode` `UrlPathEncode` `编码路径`
* `cssEncode` `编码Css样式代码`
####  <a id="CSRFA" href="#CSRFA">阻止CSRF攻击</a>  :star2: <a href="#top"> :arrow_up: </a>
`为了阻止这个攻击,MVC提供了三种方法`
  * `1. 令牌验证`:`在表单请求中插入一个包含唯一值的隐藏输入元素,该值将会与作为回话cookie存储在用户浏览器的另一个值相互匹配在提交表单时,ActionFilter
  就会验证这个两个值是否匹配`
  ```c#
     @using (Html.BeginForm()) {
        @Html.AntiForgeryToken()

        <div class="form-actions no-color">
            <input type="submit" value="Delete" class="btn btn-default" /> |
            @Html.ActionLink("Back to List", "Index")
        </div>
     }
     
    [HttpPost]
    [ValidateAntiForgeryToken]
    public async Task<ActionResult> Create([Bind(Include = "ProfessionID,Name,AddTime,
     InstituteID")] Profession profession)
    {   ...
  ```
  * `2. 幂等的GET请求`:`就是如果一个操作是幂等的就是可以重复执行多次而不改变执行结果,一般来说,仅通过使用POST 请求修改数据中或者网站上的内容
  这种修改一定程度下就可以避免攻击`
  * `3. HttpReferrer 验证`：`它通过使用ActionFilter 处理,这种情况下,可查看提交表单的客户端是否确实在目标站点上`
####  <a id="CookieLost" href="#CookieLost">使用HttpOnly阻止Cookie窃取</a>  :star2: <a href="#top"> :arrow_up: </a>  
`在web.conf 中设置`
```xml
  <system.web>
    <httpCookies domain="" httpOnlyCookies="true" requireSSL="false" />
  </system.web>
```

####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>







