### [fastJson 开始使用吧 第一课](#top) :maple_leaf: <b id="top"></b> 

----
`要想要使用 就要引入fastJson的 Jar 包 怎么引用不会的就自己百度吧 Jar包都不会引用那么你还没有到学习JSON的地步,去学前面的语法吧`

- [x] :maple_leaf: [`fastjson 介绍 `](#introduce)
- [x] :maple_leaf: [`序列化对象为JSON字符串`](#objectto)
- [x] :maple_leaf: [`JSON解析为JAVA对象`](#hook)
- [x] :maple_leaf: [`数组类型与JSONArray之间的转换`](#array)
- [x] :maple_leaf: [`将JSON字符串数组转换为List`](#toarray)
- [ ] :maple_leaf: [`使用JSONObject 对象`](jsonobject)


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

```java
public class Book {
    
    public String BookID;

    public String BookName;

    public int BookPublishYear;
}
```
##### [序列化对象为JSON字符串](#top)  :maple_leaf: <b id="objectto"></b> 
`使用方法: ` [`String JSON.toJSONString(Object obj)`](#top)

```java
public static void  ObjectToJsonExample(){
    User one = new User();
    one.hasLogin = true;

    String result = JSON.toJSONString(one);

    System.out.println(result);
}
/*
{
  "UserID": "",
  "hasLogin": true,
  "hasRegister": false,
  "isLoginSuccess": false,
  "isRegisterSuccess": false,
  "loginErrorInfo": "登陆错误！长或或密码错误",
  "rgisterErrorInfo": "注册错误！ 信息重复已注册,或信息不合格",
  "states": 404
}
*/
```

##### [JSON解析为JAVA对象](#top)  :maple_leaf: <b id="hook"></b> 
`将Json 字符串转换为JAVA对象 使用方法: ` [`Object JSON.parseObject(String json,Object.class)`](#top)
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
`json字符串与javaBean之间的转换推荐使用 TypeReference<T> 这个类 例子:`
```java
public class Teacher {

    private String teacherName;
    private Integer teacherAge;
    private Course course;
    private List<Student> students;

    public String getTeacherName() {
        return teacherName;
    }

    public void setTeacherName(String teacherName) {
        this.teacherName = teacherName;
    }

    public Integer getTeacherAge() {
        return teacherAge;
    }

    public void setTeacherAge(Integer teacherAge) {
        this.teacherAge = teacherAge;
    }

    public Course getCourse() {
        return course;
    }

    public void setCourse(Course course) {
        this.course = course;
    }

    public List<Student> getStudents() {
        return students;
    }

    public void setStudents(List<Student> students) {
        this.students = students;
    }
}
```
`转换`
```java
public static void testJSONStrToJavaBeanObj(){

    Student student = JSON.parseObject(JSON_OBJ_STR, new TypeReference<Student>() {});
    //Student student1 = JSONObject.parseObject(JSON_OBJ_STR, new TypeReference<Student>() {});
    //因为JSONObject继承了JSON，所以这样也是可以的

    System.out.println(student.getStudentName()+":"+student.getStudentAge());

}
```
##### [数组类型与JSONArray之间的转换](#top)  :maple_leaf: <b id="array"></b> 
`JSON.toJSONString(books);`
```Java
    public  static  void ArrayToJson(){
        List<Book> books = new ArrayList<Book>();
        Book bk0 = new Book("20161104A","深入理解JavaScript",2009);
        Book bk1 = new Book("20161104B","C# 高级教程",2017);
        Book bk2 = new Book("20161104C","Java并发编程艺术",2015);
        books.add(bk0);
        books.add(bk1);
        books.add(bk2);

        String result = JSON.toJSONString(books);

        System.out.println(result);

    }

```
```json
[
  {
    "BookID": "20161104A",
    "BookName": "深入理解JavaScript",
    "BookPublishYear": 2009
  },
  {
    "BookID": "20161104B",
    "BookName": "C# 高级教程",
    "BookPublishYear": 2017
  },
  {
    "BookID": "20161104C",
    "BookName": "Java并发编程艺术",
    "BookPublishYear": 2015
  }
]
```
##### [将JSON字符串数组转换为List](#top)  :maple_leaf: <b id="toarray"></b> 
`使用:` [JSON.parseArray(jsonArray,Book.class);](#top)
```java
public  static  void JSONtoArray(){
    String jsonArray = "[{\"BookID\":\"20161104A\",\"BookName\":\"深入理解JavaScript\"," +
            "\"BookPublishYear\":2009},{\"BookID\":\"20161104B\",\"BookName\":\"C# 高级教程\",\"" +
            "BookPublishYear\":2017},{\"BookID\":\"20161104C\",\"BookName\":\"Java并发编程艺术\"," +
            "\"BookPublishYear\":2015}]\n";

    List<Book> books = JSON.parseArray(jsonArray,Book.class);

    System.out.println(books.size());
}
```
`使用parseObject`:`ArrayList<Book> books = JSON.parseObject(jsonArray,new TypeReference<ArrayList<Book>>() {});`
```java
public  static  void JSONtoArray(){
    String jsonArray = "[{\"BookID\":\"20161104A\",\"BookName\":\"深入理解JavaScript\"," +
            "\"BookPublishYear\":2009},{\"BookID\":\"20161104B\",\"BookName\":\"C# 高级教程\",\"" +
            "BookPublishYear\":2017},{\"BookID\":\"20161104C\",\"BookName\":\"Java并发编程艺术\"," +
            "\"BookPublishYear\":2015}]\n";

    ArrayList<Book> books = JSON.parseObject(jsonArray,new TypeReference<ArrayList<Book>>() {});

    System.out.println(books.size());
}
```

##### [使用JSONObject 对象](#top)  :maple_leaf: <b id="jsonobject"></b> 
```java
  JSONObject jsonObject = JSON.parseObject(COMPLEX_JSON_STR);
  //JSONObject jsonObject1 = JSONObject.parseObject(COMPLEX_JSON_STR);
  //因为JSONObject继承了JSON，所以这样也是可以的

  String teacherName = jsonObject.getString("teacherName");
  Integer teacherAge = jsonObject.getInteger("teacherAge");
  JSONObject course = jsonObject.getJSONObject("course");
  JSONArray students = jsonObject.getJSONArray("students");
```
