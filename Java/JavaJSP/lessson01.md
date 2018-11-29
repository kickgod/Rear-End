### [JSP 基础课程 001](#top) <b id="top"></b> :maple_leaf:

----
`JSP的本质是 Servlet 谨记 没有JSP Servlet早就凉凉了`

- [x] :maple_leaf: [`如何在Idea里面导入Jar 包呢？`](#notice)
- [x] :maple_leaf: [`JSP注释 <%-- --%>`](#note)


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
`JSP 注释:` `<%-- --%>`
```c#
 <%--
      response.getWriter().println("在这里我不会被编译哟！");
 --%>
```
