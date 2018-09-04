<a id="top" href="#top">表单和HTML辅助方法:maple_leaf:</a> 
----
`表单很重要对于一个网站来说,HTML辅助方法提供了很多有用的特性和方法来帮助生成HTML`

- [x] :maple_leaf: <a href="#From">`From标签`</a>
- [x] :maple_leaf: <a href="#HTMLFunctioN">`HTML 方法`</a>
   - <a href="#CodingBySelf">`自动编码`</a>
   - <a href="#AboutForm">`关于表单`</a>
   - <a href="#AntherFunction">`其他方法`</a>
   - <a href="#AntherForFunction">`其他带F方法`</a>
   - <a href="#DefineByYourSelf">`自定义属性data-`</a>
- [x] :maple_leaf: <a href="#ActionLink">`URL链接标签辅助方法`</a>
- [x] :maple_leaf: <a href="#loadPartView">`加载部分视图`</a>

####  <a id="From" href="#From">From标签</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
> `From标签是重要的,且是强大的,它的优势在Web From中并未完全发挥出来`
##### <a href="#top">action和method 特性 :arrow_up:</a>
* **`action`**:`action 指定将表单发送给服务器处理的URL路径 在MVC中是指定接受表单数据和处理表单请求的控制器的方法`<Br/>
* **`method`**:`指定HTTP请求方式,有两种方式GET POST 他们的区别在与向浏览器传输数据的方式不一样`
   * `GET`:`使用URL传值,将值加入到URL后面 例如 /Home/Edit?Index=5$Name=Se1 传递少量数据`
   * `POST`:`与服务器建立连接传递数据,较为安全,类似于交易信息就要用POST来提交,且POST可以传递大量数据,例如文件上传`
##### <a href="#top">新建一个表单 :arrow_up:</a>   
```html
<form class="form-horizontal" role="form" action="/Home/Create" method="post">
   ...
</form>
```
##### <a href="#top">使用HTML辅助方法 :arrow_up:</a>   
`使用HTML辅助方法,来创建表单`
```C#
  @using (Html.BeginForm("Show", "Home", FormMethod.Post))
  {
      <p><input name="UserName" type="text" /></p>
      <p><input name="phoneNumber" type="text" /></p>
      <p><input name="UserPwd" type="password" /></p>
      <p><input type="submit" value="Create" class="btn btn-danger" /></p>
  }
```
`为什么要使用using 因为这个方法调用完 会返回一个实现了接口IDisposable的对象,使用using就可以自动调用Dispose方法释放`<br/>
`你也可以采用这种标签写法`
```C#
  @{Html.BeginForm("Show", "Home", FormMethod.Post); }
      <p><input name="UserName" type="text" /></p>
      <p><input name="phoneNumber" type="text" /></p>
      <p><input name="UserPwd" type="password" /></p>
      <p><input type="submit" value="Create" class="btn btn-danger" /></p>
  @{Html.EndForm();}
```
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
- <a href="CodingBySelf">自动编码</a>
####  <a id="HTMLFunctioN" href="#HTMLFunctioN">HTML方法</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`HTML辅助方法可以帮我们解决很多的事情,不可获取的,但是不利于前后端分离,应该使用Ajax 将试图和控制器数据交换 通过Ajax隔开,HTML辅助方法
就是这样的为了不隔开方面试图和控制器 传递数据准备的,不推荐使用,前后端分离,吧数据交给JS或者前端框架,虚拟DOM, 彻底的分离才是正道,但是个人开发的时候这个还是得会啊,神tm跟你前后端分离啊`
####  <a id="CodingBySelf" href="#CodingBySelf">自动编码</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
- <a href="#"></a>
`Html 构建的HTML标签是自编码的,也就是说通过XSS 是不可能的,例如下面的方法 参数`
```C#
    @Html.TextArea("Text","<br/> hello World",new { @class="col-md-8",data_val_type="34" });
    // 第一个参数指定 Dom节点的 id和name的值
    // 第二参数指定value的值 后面一个参数指定HtmlAttribute 
    // data-val-type 自定义属性 要写出这个样子 data_val_type - 转换为 _
    // @class 指定类 加上@的原因是因为 class是C#关键字
```
`构造结果`
```HTML
  <textarea class="col-md-8" cols="20" data-val-type="34" id="Text" name="Text" rows="2">
     &lt;br/&gt; hello World
  </textarea>;
```
#####  <a id="AboutForm" href="#AboutForm">关于表单</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
```c#
  @using (Html.BeginForm("Show", "Home", FormMethod.Post)){
      @Html.AntiForgeryToken()
      @Html.ValidationSummary(excludePropertyErrors:false)
      <p><input name="UserName" type="text" /></p>
      <p><input name="phoneNumber" type="text" /></p>
      <p><input name="UserPwd" type="password" /></p>
      <p><input type="submit" value="Create" class="btn btn-danger" /></p>
  }
  
  //控制器方法
  [HttpPost]
  [ValidateAntiForgeryToken]
  public ActionResult Show(String UserName,String phoneNumber,string UserPwd){
      //...
  }
```
**`重复攻击`** ：`[ValidateAntiForgeryToken]` `和`  `@Html.AntiForgeryToken()` `方法配合使用可以防止表单重复攻击 over-posting`
**` @Html.ValidationSummary(excludePropertyErrors:false)`**:`辅助方法可以用来显示ModelState字典中所有验证错误的无序列表,使用布尔参数来告知
辅助方法排除属性级别的错误` 
##### 来一个例子
```C#
  public ActionResult Create()
  {
      ModelState.AddModelError("","this is a Error Page");
      ModelState.AddModelError("ID", "编号重复");
      return View();
  }
```
##### 视图中 @Html.ValidationSummary(true, "", new { @class = "text-danger" }) 参数为true 的时候
```html

   <div class="validation-summary-errors text-danger">
      <ul>
          <li>this is a Error Page</li>
      </ul>
   </div>
```
```C#
    public class Book
    {
        [DisplayName("编号")]
        [Required(ErrorMessage ="请输入书籍编号")]
        public string ID{get;set;}
        [Required(ErrorMessage ="请输入书籍的名称")]
        [DisplayName("书名称")]
        public string Name{get;set;}
        [DisplayName("出版日期")]
        [Required(ErrorMessage ="请输入出版年")]
        public int PublishYear{get;set;}
        [DisplayName("价格")]
        [Required(ErrorMessage ="请输入价格")]
        public double Price{get;set;}
    }
```
##### 视图
```html
@using (Html.BeginForm()) 
{
    @Html.AntiForgeryToken()
    
    <div class="form-horizontal">
        <h4>Book</h4>
        <hr />
        @Html.ValidationSummary(false, "", new { @class = "text-danger" })
        <div class="form-group">
            @Html.LabelFor(model => model.ID, 
                  htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.EditorFor(model => model.ID, 
                  new { htmlAttributes = new { @class = "form-control" } })
                @Html.ValidationMessageFor(model => model.ID, "",
                  new { @class = "text-danger" })
            </div>
        </div>

        <div class="form-group">
            @Html.LabelFor(model => model.Name, htmlAttributes: new 
                { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.EditorFor(model => model.Name, new { htmlAttributes = 
                 new { @class = "form-control" } })
                @Html.ValidationMessageFor(model => model.Name, "", new 
               { @class = "text-danger" })
            </div>
        </div>

        <div class="form-group">
            @Html.LabelFor(model => model.PublishYear, htmlAttributes: new 
              { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.EditorFor(model => model.PublishYear, new { htmlAttributes = 
               new { @class = "form-control" } })
                @Html.ValidationMessageFor(model => model.PublishYear, "", 
                new { @class = "text-danger" })
            </div>
        </div>

        <div class="form-group">
            @Html.LabelFor(model => model.Price, htmlAttributes: new { @class =
                "control-label col-md-2" })
            <div class="col-md-10">
                @Html.EditorFor(model => model.Price, new { htmlAttributes = new
                 { @class = "form-control" } })
                @Html.ValidationMessageFor(model => model.Price, "", new { @class
                 = "text-danger" })
            </div>
        </div>

        <div class="form-group">
            <div class="col-md-offset-2 col-md-10">
                <input type="submit" value="Create" class="btn btn-default" />
            </div>
        </div>
    </div>
}
```

![baidu](/Image/truePictureForm.png)  

##### 视图中 @Html.ValidationSummary(false, "", new { @class = "text-danger" }) 参数为false 的时候
![baidu](/Image/falsePictureForm.png)  

#####  <a id="AntherFunction" href="#AntherFunction">其他方法</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
* `第一个参数` :`表名DOM节点的 ID和Name属性` <br/>
* `第二个参数` :`这是内容就是 TextNode input标签就是value`<br/>
* `最后一个参数` :`htmlAttributes: new { @class = "control-label col-md-2" } 设置Html属性 使用匿名对象`<br/>

**`@Html.TextBox("BookID","这是标签的内容" ,new { @class="text-danger" });`**
```html
 <input class="text-danger" id="BookID" name="BookID" type="text" value="这是标签的内容" />;
```
**`@Html.TextArea("BookContext","我是TextAre的内容",new { @class= "text-warning" });`**
```html
<textarea class="text-warning" cols="20" id="BookContext" name="BookContext" rows="2">
   我是TextAre的内容
</textarea>
```
**`@Html.Label("TitleName","王者")`**
```html
   <input id="TitleName" name="TitleName" type="hidden" value="王者" />
```
**`@Html.Hidden("TitleName","王者")`**
```html
   <input id="UserPassword" name="UserPassword" type="password" />;
```
**`RadioButton`**
```Html
   @Html.RadioButton("Coler", "White") White
   @Html.RadioButton("Coler", "Red", false) Red
   @Html.RadioButton("Coler", "Blue", true) Blue
   @Html.RadioButton("Coler", "Yellow", false) Yellow

   <input id="Coler" name="Coler" type="radio" value="White" /> White
   <input id="Coler" name="Coler" type="radio" value="Red" /> Red
   <input checked="checked" id="Coler" name="Coler" type="radio" value="Blue" /> Blue
   <input id="Coler" name="Coler" type="radio" value="Yellow" /> Yellow
```
**`@Html.CheckBox("YourCode") 至高王者`**
```Html
    <input id="YourCode" name="YourCode" type="checkbox"value="true" />
   <input name="YourCode" type="hidden" value="false" /> 至高王者
```
`为什么有两个input 因为HTML 规范 规定浏览器只提交 开 即选中的 复选框的,但是MVC需要绑定模型需要提交一个值`
#####  <a id="AntherForFunction" href="#AntherForFunction">其他带For的方法</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`他们被称之为强类型辅助方法,使用这个方法的时候需要向他传递一个lambda表达式`
**`@Html.LabelFor(model => model.ID, htmlAttributes: new { @class = "control-label col-md-2" })`**<br/>
`如果模型是IEnumerable 的集合或者数组 那么LabelFor就很瓜了,不能使用 这个方法只适用于单体对象的属性`<br/>
```C#
   <label class="control-label col-md-2" for="ID">编号</label>
```
**` @Html.EditorFor(model => model.Name, new { htmlAttributes = new { @class = "form-control" } })`**
```html
  <input class="form-control text-box single-line" 
         data-val="true"
         data-val-required="请输入书籍的名称" 
         id="Name" name="Name" 
         type="text" value=""
  />
```
**` @Html.ValidationMessageFor(model => model.Name, "", new { @class = "text-danger" })`**
```html
  <span class="field-validation-valid text-danger" 
        data-valmsg-for="Name" d
        ata-valmsg-replace="true">
  </span>
```
#####  <a id="DefineByYourSelf" href="#DefineByYourSelf">自定义属性data-</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`自定义属性 需要使用 data-val 中横线`

* `@Html.TextArea("BookContext","我是TextAre的内容",new { @class= "text-warning", data_val_type = "34" });`

```html
 <textarea class="text-warning" cols="20" data-val-type="34" id="BookContext" name="BookContext" rows="2">
   我是TextAre的内容
 </textarea>
```
####  <a id="ActionLink" href="#ActionLink">URL链接标签辅助方法</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`Html辅助方法提供了 ActionLink和Html.RouteLink`
##### ActionLink 常用的参数
* `@Html.ActionLink("链接到主页", "Index", "Book",new {id=15,name="JiangXing" },htmlAttributes: new { @class="btn btn-danger" });`
   * `1`:`a标签的内容`
   * `2`:`方法名称`
   * `3`:`控制器`
   * `4`:`路由传参数`
   * `5`:`Html属性`
```html
 <a class="btn btn-danger" href="/Book/Index/15?name=JiangXing">链接到主页</a>;
```
##### Url.Action
* `@Url.Action("Index", "Book",new {name="JiangXing" },null)` 
```html
  /Book?name=JiangXing;
```
##### Url.RouteUrl
##### Url.Content
####  <a id="loadPartView" href="#loadPartView">加载部分视图</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
##### @Html.Partial("Message","Home");/@Html.RenderPartial("Message","Home");
`这个方法是真的只是加载视图,不会去请求方法,而下面的方法 回去请求方法 然后得到方法返回的视图而 这两个方法 只有视图,不会去请求方法`
#####  @Html.Action / @Html.RouteAction
* `加载部分视图的方法,但是如何内容变化不能够检查到,不能够像Ajax一样 一直获得最新的视图`
  * `@Html.Action("Message")` `内部是方法名称`
  * `@Html.Action("Message","Home");` `方法名称, 控制器名称`
  * `@Html.Action("Message","Home",new { id=5,name="wangzhe"})` `最后一个参数 传递给方法的参数`
```C#
  public ActionResult Message()
  {
      Random r = new Random();
      int t = r.Next(0, 15);
      ViewBag.Message = t;
      return PartialView();
  }
```
`通过Ajaxa 请求的页面每次加载都会刷新一次`

```html
<div class="row">
    <div class="col-md-12">
        <div id="result"></div>
        @Html.Label("Title")
    </div>
    @ViewBag.status
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

