### [fastJson 开始使用吧 第一课](#top) :maple_leaf: <b id="top"></b> 

----
`要想要使用 就要引入fastJson的 Jar 包 怎么引用不会的就自己百度吧 Jar包都不会引用那么你还没有到学习JSON的地步,去学前面的语法吧`

- [x] :maple_leaf: [`fastjson 介绍 `](#introduce)
- [x] :maple_leaf: [`将对象解析为Json字符串`](#objectto)
- [x] :maple_leaf: [`将JSON字符串 转换为Java对象`](#hook)
- [x] :maple_leaf: [`数组类型与JSONArray之间的转换`](#array)


##### [fastjson 介绍](#introduce) :maple_leaf:
`fastJson对于json格式字符串的解析主要用到了一下三个类：`
* `JSON：fastJson的解析器，用于JSON格式字符串与JSON对象及javaBean之间的转换。`
* `JSONObject：fastJson提供的json对象。`
   * `我们可以把JSONObject当成一个Map<String,Object>来看，只是JSONObject提供了更为丰富便捷的方法，方便我们对于对象属性的操作`
   
   ```java
     public class JSONObject extends JSON implements Map<String, Object>, Cloneable, Serializable,
     InvocationHandler {
      private static final long serialVersionUID = 1L;
      private static final int DEFAULT_INITIAL_CAPACITY = 16;
      private final Map<String, Object> map;
      ...
   ```
  
* `JSONArray：fastJson提供json数组对象。`
   * `同样我们可以把JSONArray当做一个List<Object>，可以把JSONArray看成JSONObject对象的一个集合。`
   
   ```java
     public class JSONArray extends JSON implements List<Object>, Cloneable, RandomAccess, Serializable {
      private static final long serialVersionUID = 1L;
      private final List<Object> list;
      protected transient Object relatedArray;
      protected transient Type componentType;
      ...
   ```
   
##### [将要使用到的实体类](#top) :maple_leaf:
```java
public class User {
    public Boolean isLoginSuccess ; //是否成功登陆

    public Boolean hasLogin;   //是否登陆

    public Boolean hasRegister; //是否注册

    public Boolean isRegisterSuccess; //是否成功注册

    public String  loginErrorInfo;

    public String  rgisterErrorInfo;

    public Integer states;

    public String UserID;

    public  User(){
        this.isRegisterSuccess = false;
        this.hasLogin = false;
        this.hasRegister = false;
        this.isLoginSuccess = false;
        this.loginErrorInfo = "登陆错误！ 长或或密码错误";
        this.rgisterErrorInfo = "注册错误！ 信息重复已注册,或信息不合格";
        this.states = 404;
        this.UserID = "";
    }
}  
```
##### [将对象解析为Json字符串](#top)  :maple_leaf: <b id="objectto"></b> 
`使用方法: ` [`String JSON.toJSONString(Object obj)`](#top)

```java
public static void  ObjectToJsonExample(){
    User one = new User();
    one.hasLogin = true;

    String result = JSON.toJSONString(one);

    System.out.println(result);
}
/*
{"UserID":"","hasLogin":true,"hasRegister":false,"isLoginSuccess":false,"isRegisterSuccess":false,
"loginErrorInfo":"登陆错误！长或或密码错误","rgisterErrorInfo":"注册错误！ 信息重复已注册,或信息不合格","states":404}
*/
```

##### [将JSON字符串 转换为Java对象](#top)  :maple_leaf: <b id="hook"></b> 
`使用方法: ` [`Object JSON.parseObject(String json,Object.class)`](#top)
```java
    public static void JSONToObject(){
        String js = "{\"UserID\":\"\",\"hasLogin\":true,\"hasRegister\":false,\"isLoginSuccess" +
                "\":false,\"isRegisterSuccess\":false,\"loginErrorInfo\":\"登陆错误！ 长或或密码错" +
                "误\",\"rgisterErrorInfo\":\"注册错误！ 信息重复已注册,或信息不合格\",\"states\":404}\n";
        User one = JSON.parseObject(js,User.class);
        System.out.println(one.hasLogin);
    }
    //true
```

##### [数组类型与JSONArray之间的转换](#top)  :maple_leaf: <b id="array"></b> 
``
