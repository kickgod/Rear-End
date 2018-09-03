<a id="top" href="#top">存储用户信息 Cookie Session :maple_leaf:</a> 
----
`通过Cookie或者Session保存用户信息，在用户访问网站期间,作为一种用户认证手段`

- [x] :maple_leaf: <a href="#SessionObject">`Session`</a>
- [x] :maple_leaf: <a href="#">``</a>
- [x] :maple_leaf: <a href="#">``</a>
- [x] :maple_leaf: <a href="#">``</a>

####  <a id="SessionObject" href="#SessionObject">Session对象</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
* `每一个用户访问Web时,都会被分配一个Session对象回话结束或者操作就会手动释放`
  * `Session可以存储任何对象 是object 类型`
  * `存储大小只限于内存,当服务器的内存不够的时候反而会是的Session被丢失`
-----
##### 工作方式
* `在客户机存放一个SessionID,在服务器再存放一个SessionID,SessionID下面再存放key 存放许多的值,客户机通过Session向服务器请求Session键值`
   * `如果用户禁用Cookie 那么Session也会失效 因为SessionID存放在Cookie中`
##### <a href="#top">添加Session :arrow_up: </a>
```C#
    //把用户数据存成JSON格式
    JsonData data = new JsonData();
    data["UserID"] = "123456";
    data["UserPwd"] = "sddsda";
    String UserData =JsonMapper.ToJson(data);
    Session["User"] = UserData;  
  //Session.Add("User", UserData); 
```
* `调用Add方法 如果键已经存在会覆盖`
##### <a href="#top">获取Session的值 :arrow_up: </a>
```C#
  //Session的本质是键值对 所以通过键去获取就可以了
  String data=(String)Session["User"]; 
```
* `注意类型转换就行`
##### 移除Session 
```C#
  Session.Clear(); //从回话状态中移除所有键值
  Session.Abandon(); //取消当前回话状态
  Session.Remove("User");//移除当个键
  Session.RemoveAll();//从回话状态中移除所有键值
```
* `获取当前URL`：`Request.Url.LocalPath.ToString();`
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
