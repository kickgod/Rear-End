<a id="top" href="#top">表单和HTML辅助方法:maple_leaf:</a> 
----
`表单很重要对于一个网站来说,HTML辅助方法提供了很多有用的特性和方法来帮助生成HTML`

- [x] :maple_leaf: <a href="#From">`From标签`</a>
- [x] :maple_leaf: <a href="#HTMLFunctioN">`HTML 方法`</a>
   - <a href="#CodingBySelf">`自动编码`</a>
   - <a href="#AboutForm">`关于表单`</a>
- [x] :maple_leaf: <a href="#">``</a>
- [x] :maple_leaf: <a href="#">``</a>

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
####  <a id="AboutForm" href="#AboutForm">关于表单</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
```c#
  @using (Html.BeginForm("Show", "Home", FormMethod.Post)){
      @Html.AntiForgeryToken()
      @Html.ValidationSummary(excludePropertyErrors:false)
      <p><input name="UserName" type="text" /></p>
      <p><input name="phoneNumber" type="text" /></p>
      <p><input name="UserPwd" type="password" /></p>
      <p><input type="submit" value="Create" class="btn btn-danger" /></p>
  }
```
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
