### [JSP 基础课程 001](#top) <b id="top"></b> :maple_leaf:

----
`JSP的本质是 Servlet 谨记 没有JSP Servlet早就凉凉了`

- [x] :maple_leaf: [`如何在Idea里面导入Jar 包呢？`](#notice)
- [x] :maple_leaf: [`JSP注释 <%-- --%>`](#note)
- [x] :maple_leaf: [`JSP Page指令`](#page)
- [x] :maple_leaf: [`JSP 局部代码块`](#code)
- [x] :maple_leaf: [`JSP 全局代码块`](#allcode)
- [x] :maple_leaf: [`JSP 输出HTML`](#html)

------

##### [如何在Idea 里面导入Jar 包呢？](#top)  :maple_leaf: <b id="notice"></b> 
`要写JSP 那么就要添加一个web 项目 然后引用 jsp-api 和 servlet-api 这个两个Java包 ,不然你的JSP 文件就无法编译为 Servlet！成功运行`
* `点击Idea 菜单 File`
* `选择点击下拉栏的【Project Structure】`
* `选择点击【Project Structure】弹窗里左侧的【Modules】`
* `选择点击正中央上方的【Dependencies】`
* `选择右侧的 “ + ”符号，找到jar包的路径，添加进去`
* `添加成功后将会在【Export】区域找到添加的jar包  `
* `那两个Jar 包在 Tomcat 文件夹里面的lib文件夹中`

##### [JSP 注释 <%-- --%>](#top)  :maple_leaf: <b id="note"></b> 
* `jsp 有三种注释`
   * `前端注释`:`会被转义然后 也会被发送到浏览器 但是不会被浏览器执行`
   * `Java 注释`:`会被转义 但是不会被Servlet执行`
   * `JSP 注释`:`不会被转义为Servlet里面的语句`  `<%-- --%>`
```c#
<%--
     response.getWriter().println("在这里我不会被编译哟！");
--%>
```
##### [JSP Page指令](#top)  :maple_leaf: <b id="page"></b> 
`page 有很多的参数 可以指定许多的信息 例如导入包 指定错误页面 编码格式`
* `import:导入Java包`
* `pageEncoding:指定JSP页面的编码格式 和数据接收存储格式`
* `language:指定JSP的转义格式`
* `session: JSP 默认是支持Session 的可以改变这个属性 不默认支持session`
* `contentType="text/html;charset=UTF-8`:`指定浏览器解析格式`
* `errorpage="error.jsp"`:`错误页面`
```jsp
<%@ page  contentType="text/html;charset=UTF-8" language="java" pageEncoding="UTF-8"  %>
<%@ page  import="java.util.*" %>
<%@ page  session="true" %>
<%@ page  errorpage="error.jsp" %>
```
##### [JSP 局部代码块](#top)  :maple_leaf: <b id="code"></b> 
`生命一下：局部代码块都会被原样转移到 Servlet的 ——JspService方法中去`
* `代码里面声明的变量都是局部变量`
* `<%  %> 生命局部代码块`
```jsp
<% 
   Boolean isOk = true; 		
   if(isOk){
%>
<span>一切正常! 请党和人民放心</span>
<%}
   else
{ %>
<span>我愧对党和政府 愧对人民赋予我的使命</span>
<%} %>
```
##### [JSP 全局代码块](#top)  :maple_leaf: <b id="allcode"></b> 
`全局代码也就是说它里面生命的变量和 方法 是全局的 整个[JSP文件的任何地方]类里面都可以使用`
* `一般是 全局代码声明  局部代码调用`
* `<%! %>`
```jsp
<% test(); %>

<%!
   int b = 5;
   public void test(){
     System.out.println("Java 全局函数执行了");
   }
%>
```
##### [JSP 输出HTML](#top)  :maple_leaf: <b id="html"></b> 
`怎么让JSP 输出 变量的值 通过 <%=变量名/方法 %> 或`
```jsp
<%! int price = 19; %>
<p>衬衫的价格是：<%= price%></p>
//衬衫的价格是：19
```
