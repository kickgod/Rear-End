### [JSP EL表达式 JSTL表达式 004](#top) <b id="top"></b> :maple_leaf:

----
`别看 这些内置对象都是Servlet的类 然后实例化一个对象`

- [x] :maple_leaf: [`EL 表达式简介`](#intro)
- [x] :maple_leaf: [`EL 获取对象数据`](#url)
- [x] :maple_leaf: [`JSTL表达式`](#jstl)

------
##### [EL 表达式是什么?](#top)
```java
<%  request.setAttribute("username","LiYang"); %>
姓名: ${username}
```
##### [EL 表达式内置对象]
|属性范围|EL中的名称 [EL隐式对象介绍 ]|
|:--|:--|
|page |pageScope,例如${pageScope.username} 表示在pageContext对象查找username 变量 找不到返回null  一般使用EL 表达式都要带上Scope|
|request|requestScope|
|session|sessionScope|
|application|applicationScope|
|param|返回客户端的请求参数的字符串值|
|paramValues|返回映射至客户端的请求参数的一组值|
|pageContext|提供对用户请求和页面信息的访问|
##### [EL表达式简介](#top)  :maple_leaf: <b id="intro"></b> 
`EL 全名为Expression Language。EL主要作用`
* `1、获取数据`:`EL表达式主要用于替换JSP页面中的脚本表达式，以从各种类型的web域 中检索java对象、获取数据。(某个web域 中的对象，访问javabean的属性、访问list集合、访问map集合、访问数组)`
* `2、执行运算`:`利用EL表达式可以在JSP页面中执行一些基本的关系运算、逻辑运算和算术运算，以在JSP页面中完成一些简单的逻辑运算。${user==null}`
* `3、获取web开发常用对象`:`EL 表达式定义了一些隐式对象，利用这些隐式对象，web开发人员可以很轻松获得对web常用对象的引用，从而获得这些对象中的数据。`
* `4、调用Java方法`:`EL表达式允许用户开发自定义EL函数，以在JSP页面中通过EL表达式调用Java类的方法。`
* `查找顺序： page-->request->session->application`


##### [JSP 获取对象数据](#top)  :maple_leaf: <b id="url"></b> 
`${ EL exprission }`
* `使用变量名获取值`
* `获取对象的属性值`
* `获取集合`


`EL 框架获取的数据只能是来自于 pageContext request session application 的值 其他数据一概不支持 找到了获取返回  找不到 就什么也不发生`
* `普通对象`：`${键名称.属性.属性...}   ${user.id}`  
* `list集合`:`${键名[下标].属性}`
* `map集合`：`${键名.map集合的键名}`

`el 还可以进行表达式运行`：`${5-6}`

```jsp
/************ List集合 **************/
<%
    List names = new ArrayList();
    names.add(0, "LiYang");
    names.add(1,"WangHua");
    request.setAttribute("names", names);
%>
姓名：${names[0]}<br/>
姓名：${names[1]}<br/>

/************ Map集合 **************/
<%
    Map names = new HashMap();
    names.put("one","LiYang");
    names.put("two","WangHua");
    request.setAttribute("names",names);
%>
姓名：${names.one}<br/>
姓名：${names["two"] }<br/>

```
* `EL 表达式也支持逻辑表达式 > < == >= <= != && || ! 都支持`
```jsp
变量 a不存在，则${empty a}返回的结果为true
${not empty a}或${!empty a}返回的结果为false
${23<5}
```
##### [JSP JSTL表达式](#top)  :maple_leaf: <b id="jstl"></b>
`JSTL 表达式...不学也罢 反正前端都模板化了还学这个干吗 但是当前端页面是 jsp且没有使用 ajax axios或者 前端框架技术之前  这还是有用的、特别是JSTL 出现在当时前端并不发达的时候,他的流程控制 循环 判断我们还是可以学习一下的`
* `在工程中引用JSTL的jar包和标签库描述符文件tld`
* `在JSP页面添加taglib指令`
* `使用JSTL标签`
`<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>`
* `在JavaEE 5.0开始，JSTL已经成为标准功能，不需要额外添加`

##### [JSTL标准标签库](#top)
* `通用标签`:`set out remove`
* `条件标签`:`if choose`
* `迭代标签`:`forEach`

`if标签没有对应的else标签`
```jsp
if：实现Java语言中if语句的功能
<c:if  test="codition" var="name"  scope="applicationArea" >
	…
</c:if>	

name:该变量用于保存 返回的true/false   
codition:判断条件表达式 可以省略
applicationArea:指定var变量的作用域 可以省略   
```
`choose：实现Java语言中if-else if-else语句的功能`
```jsp
<c:choose var="varName" scope="scope">
	<c:when test="condition">
		主体内容
 	</c:when>
	<c:otherwise>
		主体内容
	</c:otherwise>
</c:choose >
```
`forEach是for循环语句的变体，实现集合对象(可以是list、数组等)的处理 `
```jsp
<c:forEach items="collection" var= "name" begin="start" end= "end" step="count"  varStatus="status" >
…循环体代码…
</c:forEach>
```
* `items指定要遍历的集合对象`
* `var指定当前成员的引用`
* `begin指定从集合的第几位开始`
* `end指定迭代到集合的第几位结束`
* `step指定循环的步长`
* `varStatus属性用于存放var引用的成员的相关信息，如索引等`
```jsp
<%
  List products = GoodsDao.getAllProducts();
  request.setAttribute("products", products);
%>
…
<!-- 循环输出商品信息 -->
<c:forEach var="product" items="${requestScope.products}" varStatus="status">
<!-- 如果是偶数行，为该行换背景颜色 -->
    <tr <c:if test="${status.index % 2 == 1 }">style="background-color:rgb(219,241,212);"</c:if>>
        <td>${product.name }</td>
        <td>${product.area }</td>
        <td>${product.price }</td>
    </tr>
</c:forEach>


<%
    Map<String,String> map=new HashMap<String,String>();
    map.put("tom", "美国");
    map.put("lily", "英国");
    map.put("jack","中国");
    request.setAttribute("map", map);
%>
<c:forEach var="entry" items="${map}">
     ${entry.key}
     ${entry.value}<p>
</c:forEach>

```

------
`JSTL 有很多的标准库 不要管那么多 用几个就可以了 千万不要都学 已经不用JSP写网页了`
