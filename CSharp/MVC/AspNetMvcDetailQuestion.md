<a id="top" href="#top"> ASP.NET MVC 细节的一些问题:maple_leaf:</a> 
-----
`这里记录一些细节问具体`
- [x] :maple_leaf: <a href="#FileError">文件问题</a>
  - <a href="#FilePath">`MVC 文件路径`</a>

####  <a id="FilePath" href="#FilePath">MVC 文件路径错误问题</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
* `使用visual studio 调试mvc程序。再后台获取项目文件夹的路径，本地调试的话，直接使用相对路径根目录是IIS express,就不能正确获取项目的文件夹路径。`
* `需要使用 Server.MapPath(）方法来获取项目文件夹下的相对路径。`
```C#
   Server.MapPath("~/Scripts/App/Validation.js");
```
