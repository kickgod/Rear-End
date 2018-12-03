### [JSP 内置对象,资源路径](#top) <b id="top"></b> :maple_leaf:

----
`别看 这些内置对象都是Servlet的类 然后实例化一个对象`

- [x] :maple_leaf: [`JSP 内置九大对象`](#obj)
- [x] :maple_leaf: [`JSP 资源路径`](#url)

------

##### [JSP 内置九大对象](#top)  :maple_leaf: <b id="obj"></b> 
* `Request对象`:`HttpServletRequest`
* `Session对象`:`HttpSession`
* `Application对象`：`ServletContent`
* `Response`:`HttpServletResponse`
* `Out`：`响应对象 JSP 内部使用 带有缓存的响应对象[Response] 相率高于 Response对象`
* `PageContext`:`页面上下文对象 封存了其他内置对象 作用域范围 当前页面 在 _JspService 方法创建`
* `Page`:`就是this 当前Servlet对象`
* `Exception`：`异常对象 存储当前JSP 运行时的异常信息 如果使用需要 <% isErrorPage=true %>`
* `Config`:`ServletConfig`

##### [JSP 资源路径](#top)  :maple_leaf: <b id="url"></b> 
`在JSP中可以使用相对路径实现资源跳转 但是有一个问题`
  * `使用相对路径的资源 两个资源都不能随意更改位置[资源的位置不能够任意更改]`
  * `使用../跳出 文件夹 太麻烦了`
  
`JSP 可以使用 / 表示网站根目录 用户资源跳转访问例如 JSP 之类的没有问题`
 * `在请求Servlet的时候 有些变化需要注意一下`

  
