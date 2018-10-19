<a id="top" href="#top">JSP 架构:maple_leaf:</a> 
----
`介绍一下JSP是如何设计运行web服务的`

- [x] :maple_leaf: <a href="#DefaultMvcData">`基本介绍`</a>
- [x] :maple_leaf: <a href="#JSPDealWith">`JSP 处理Web请求`</a>
- [x] :maple_leaf: <a href="#JSPContent">`JSP 页面组成`</a>
- [x] :maple_leaf: <a href="#JSPRunTime">`JSP 生命周期`</a>
- [x] :maple_leaf: <a href="#RazorLayout">`JSP 评价`</a>

##### <a href="#" id="DefaultMvcData">基本介绍</a>  <a href="#top">首页 :arrow_up: </a>
[`安装使用教程`](http://www.51zxw.net/list.aspx?cid=617) `JSP的本质是对servlet的封装` <br/> 
`JavaServer Pages(JSP)是一种支持动态内容开发的网页技术它可以帮助开发人员通过利用特殊的JSP标签，其中大部分以<%开始并以%>作为结
束标志插入Java代码到HTML页面。JavaServer Pages组件是一个Java servlet的类型，旨在满足Java Web应用程序用户界面的一个角色。Web
开发人员编写JSP为文本文件，结合HTML或XHTML代码，XML元素，并嵌入JSP动作和命令。`<br/>
`Web服务器需要一个JSP引擎，即一个处理JSP页面的容器(类似于：Tomcat和Jetty)。 JSP容器负责拦截JSP页面的请求。本教程使用内置JSP容器
的Apache Tomcat来支持JSP页面的开发。`
##### <a href="#" id="JSPDealWith">JSP处理Web请求</a> <a href="#top">首页 :arrow_up: </a>
* `1.与一般的页面一样，浏览器向Web服务器发送HTTP请求。`
* `2.Web服务器识别HTTP请求是针对JSP页面，并将其转发给JSP引擎。`
* `3.这可以通过使用以.jsp(而不是.html结尾)的URL或JSP页面完成。JSP引擎从磁盘加载JSP页面并将其转换为servlet内容。这个转换非常简单，所有模板文本都转换为println()语句，并将所有JSP元素转换为Java代码。`
* `4.此代码实现页面的相应动态行为。JSP引擎将servlet编译为可执行类，并将原始请求转发到servlet引擎。`
* `5.Servlet引擎的Web服务器加载Servlet类并执行它。在执行期间，servlet生成HTML格式的输出。HTTP响应中的servlet引擎将输出传递给Web服务器。`
* `6.Web服务器根据HTTP响应将静态HTML内容转发到浏览器。最后，Web浏览器处理HTTP响应中动态生成的HTML页面，就像它是静态页面一样。`

##### 总结
* :one: `HTTP 向web服务器请求Jsp` -> :two:`找jsp页面发送给jsp引擎` ->:three:`引擎将其转换为 Servlet Jsp页面转换为Java类` ->:four: `Servlet 引擎加载Servlet 并执行它` -> :five:`输出Html代码 发生给Web服务器` -> :six:`Web服务器将HTML 转发回浏览器`
* `所以在某种程度上，JSP页面实际上只是另一种编写servlet的方式。除了编译阶段，JSP页面的处理方式与一般的servlet完全相同。`

##### <a href="#top" id="JSPContent">JSP页面组成</a> <a href="#top">首页 :arrow_up: </a>
* `一个JSP页面可以被分为以下几部份：`
  * `静态数据，如HTML`
  * `JSP指令，如include指令`:`JSP指令控制JSP编译器如何去生成servlet，以下是可用的指令`
  * `JSP脚本元素和变量`
  * `JSP动作`
  * `用户自定义标签`
  
 ###### 指令标签
 `页面指令page –页面指令有以下几个选项：` `language指令 很少用，默认为java 虽然一开始都在说jsp可能能支持其他语言例如C# php 但是一直到jsp死掉了 都没有实现过`
 * `import`:`使一个JAVA导入声明被插入到最终页面文件。导入Java包`
 ```jsp
  <%@ page import="java.util.*" %>
 ```
* `contentType`:`规定了生成内容的类型。当生成非HTML内容或者当前字符集character set并非默认字符集时使用。指定客户端读码方式`
```jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
```
* `errorPage`:`处理HTTP请求时，如果出现异常则显示该错误提示信息页面。`
```jsp
  <%@ page  errorPage="Error.jsp" %>
```
* `isErrorPage`:`如果设置为TRUE，则表示当前文件是一个错误提示页面。`
```jsp
  <%@ page isErrorPage="true" %>
```
* `isThreadSafe`：`表示最终生成的servlet是否安全线程（threadsafe）。`
```jsp
  <%@ page isErrorPage="true" isThreadSafe="true" %>
```
* `pageEncoding`:`指定文件编码,设定的是服务端以那种编码格式读取jsp文件`
```Jsp
 <%@ page contentType="text/html;charset=UTF-8" language="java" pageEncoding="utf-8" %>
```
* `extend`:`指定这个jsp文件继承自什么类，默认不使用 乱用有麻烦`

###### `include 指令`
`将另一个jsp文件包含进来,但是这个包含是静态包含`
```Jsp
  <%@include file="index.jsp" %>
```

###### <a href="#top" id="val_in_zero">JSP动作</a>
`JSP动作`:`JSP动作是一系列可以调用内建于网络服务器中的功能的XML标签。JSP提供了以下动作：`
* `jsp:include` `和子过程类似，JAVA SERVLET暂时接管对其它指定的JSP页的请求和响应。当处理完该JSP页后就马上把控制权交还当前JSP页。这样JSP代码就可以在多个JSP页中共享而不用复制。`
* `jsp:param` `可以在jsp:include, jsp:forward或jsp:params块之间使用。指定一个将加入请求的当前参数组中的参数。`
* `jsp:forward` `用于处理对另一个JSP或SERVLET的请求和响应。控制权永远不会交还给当前JSP页。`
* `jsp:plugin` `Netscape Navigator的老版本和Internet Explorer使用不同的标签以嵌入一个applet。这个动作产生为嵌入一个APPLET所需要的指定浏览器标签。`
* `jsp:fallback` `如果浏览器不支持APPLETS则会显示的内容。`
* `jsp:getProperty` `从指定的JavaBean中获取一个属性值。`
* `jsp:setProperty` `在指定的JavaBean中设置一个属性值。`
* `jsp:useBean` `创建或者复用一个JavaBean变量到JSP页。`

##### <a href="#top" id="JSPRunTime">JSP生命周期</a> <a href="#top">首页 :arrow_up: </a>
`JSP执行过程`
* `JSP生命周期被定义为从创建到破坏的过程。这类似于一个Servlet生命周期，需要一个额外的步骤来将JSP编译成Servlet。JSP执行过程以下是JSP遵循的过程`
* `编译`:`解析JSP。将JSP转换为servlet。编译servlet。`
* `初始化`:`当容器加载JSP时，它会在处理任何请求之前调用jspInit()方法 ，一般会在jspInit()方法中初始化数据库连接，打开文件和创建查找表。`
* `执行`:`，JSP引擎将调用JSP中的_jspService()方法 响应请求`
* `清理`:`jspDestroy()方法，如：释放数据库连接或关闭打开的文件。`
 
##### <a href="#top" id="RazorLayout">JSP 评价</a> <a href="#top">首页 :arrow_up: </a>
`是一个在当下已经落后的技术,学习的必要性在于许多框架和在运行项目任然在运行它,但是后续慢慢消失逐渐被新的框架取代是必然趋势,就像ASP.NET 事件驱动动态
web一样都会逐渐消失`
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
