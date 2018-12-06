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
* `Out`：`响应对象 JSP 内部使用 带有缓存的响应对象[Response] 相率高于 Response对象 等同与response.getWriter()`
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

  
##### [pageContext](#top)
`pageContext也是域对象，它的范围是当前页面。它的范围也是四个域对象中最小的！` <Br/>
`pageContext自身还是一个域对象，可以用来保存数据，同时可以通过pageContext这个域对象操作另外三个域(Request域，Session域，ServletContext域)`<br/>
* `JspWriter getOut()：获取out内置对象；`
* `ServletConfig getServletConfig()：获取config内置对象；`
* `Object getPage()：获取page内置对象；`
* `ServletRequest getRequest()：获取request内置对象；`
* `ServletResponse getResponse()：获取response内置对象；`
* `HttpSession getSession()：获取session内置对象；`
* `ServletContext getServletContext()：获取application内置对象；`
* `Exception getException()：获取exception内置对象；`

###### 其他方法
* `void setAttribute(String name, Object value)；`
* `Object getAttrbiute(String name, Object value)；`
* `void removeAttribute(String name, Object value)；`
* `Object findAttribute(String name)`：`依次在page、request、session、application范围查找名称为name的数据，如果找到就停止查找。这说明在这个范围内有相同名称的数据，那么page范围的优先级最高！`
