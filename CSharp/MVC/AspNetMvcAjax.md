<a id="top" href="#top">ASP.NET MVC Ajax  :maple_leaf:</a> 
----
`MVC框架提供了许多的脚本,并且做了需要的处理,Jquery 和Ajax 本身我们就不讲了`
- [x] :maple_leaf: <a href="#MVCAttribute">`Script 文件夹中的文件解释`</a>
- [x] :maple_leaf: <a href="#DefineByUserself">`自定义验证逻辑注解`</a>
- [x] :maple_leaf: <a href="#AjaxUnobtrusiveFunction">`Ajax辅助方法`</a>
  - <a href="#Required">`引入Ajax脚本`</a>
  - <a href="#StringLength">`StringLength`</a>
  - <a href="#RegularExpression">`RegularExpression`</a>
  - <a href="#Range">`Range`</a>
  - <a href="#Compare">`Compare`</a>
  - <a href="#Remote">`Remote`</a>

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














































