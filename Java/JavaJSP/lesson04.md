### [JSP EL表达式 004](#top) <b id="top"></b> :maple_leaf:

----
`别看 这些内置对象都是Servlet的类 然后实例化一个对象`

- [x] :maple_leaf: [`EL 表达式简介`](#intro)
- [x] :maple_leaf: [`EL 获取对象数据`](#url)

------

##### [EL表达式简介](#top)  :maple_leaf: <b id="intro"></b> 
* `EL 全名为Expression Language。EL主要作用`
  * `1、获取数据`:`EL表达式主要用于替换JSP页面中的脚本表达式，以从各种类型的web域 中检索java对象、获取数据。(某个web域 中的对象，访问javabean的属性、访问list集合、访问map集合、访问数组)`
  * `2、执行运算`:`利用EL表达式可以在JSP页面中执行一些基本的关系运算、逻辑运算和算术运算，以在JSP页面中完成一些简单的逻辑运算。${user==null}`
  * `3、获取web开发常用对象`:`EL 表达式定义了一些隐式对象，利用这些隐式对象，web开发人员可以很轻松获得对web常用对象的引用，从而获得这些对象中的数据。`
  * `4、调用Java方法`:`EL表达式允许用户开发自定义EL函数，以在JSP页面中通过EL表达式调用Java类的方法。`
  * `查找顺序： pageContext-->request->session->application`
##### [JSP 获取对象数据](#top)  :maple_leaf: <b id="url"></b> 
`EL 框架获取的数据只能是来自于 pageContext request session application 的值 其他数据一概不支持 找到了获取返回  找不到 就什么也不发生`
* `普通对象`：`${键名称.属性.属性...}   ${user.id}`  
* `list集合`:`${键名[下标].属性}`
* `map集合`：`${键名.map集合的键名}`

`el 还可以进行表达式运行`：`${5-6}`
