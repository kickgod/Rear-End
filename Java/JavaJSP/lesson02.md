### [JSP 动态引入和静态引入 002](#top) <b id="top"></b> :maple_leaf:

----
`JSP的本质是 Servlet 谨记 没有JSP Servlet早就凉凉了`

- [x] :maple_leaf: [`静态引入`](#static)
- [x] :maple_leaf: [`动态引入`](#dynamic)
- [x] :maple_leaf: [`Jsp的转发标签`](#formward)

------

##### [静态引入](#top)  :maple_leaf: <b id="static"></b> 
`<%@ include file="xxxx.jsp" %>`：`file 为路径 且为相对路径`
* `两个JSP 文件被转移成一个Servlet 执行然后输出为html页面 【可以理解为整合一个servlet，一起编译，一次执行】`
* `对于被引入的jsp文件我们建议不再 声明html head body标签 `
* `不要在两个jsp文件使用同一个名称的变量 因为会编译为一个servlet java类`
```jsp
	<%@ include file="footer.jsp" %>
	<%! int price = 19; %>
	<p>衬衫的价格是：<%=price%></p>
```
##### [动态引用](#top)  :maple_leaf: <b id="dynamic"></b> 
`<jsp:include page="page.jsp"/>`：`page 为路径 且为相对路径`
* `分开编译 编译结果然后HTML 合并成一个HTML文件`
* `动态引入允许两个jsp文件具有同名变量`

```jsp
	<%! int price = 19; %>
	<p>衬衫的价格是：<%=price%></p>
	<jsp:include page="footer.jsp" />
```

##### [Jsp的转发标签](#top)  :maple_leaf: <b id="formward"></b> 
`JSP 页面实现转发 在Servlet中我们通过 :request.getRequestDispatcher("Login.jsp").forward(request,response); 实现请求转发 jsp
给我们提供了一个标签来帮助我们实现这个功能`
* `<jsp:forward page="default.jsp"></jsp:forward>`:`相对路径 只要页面有这个表情 那么就会跳转到 default.jsp页面去`
* `记住转发 url 路由不变`
* `它还可以传递参数 或者参数的方式 `
```jsp
<jsp:forward page="login.jsp">
<jsp:param value="admin" name="userName"/>
<jsp:param value="password" name="password"/>


//另一个页面 获取参数的方式
<%=request.getParameter("userName")%>
<%=request.getParameter("password")%>
</jsp:forward>
```
