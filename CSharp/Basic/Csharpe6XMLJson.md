<a id="top" href="#top">	:maple_leaf: C# XML和JSON :blue_heart:</a> 
----
- [ ] 	:maple_leaf: <a href="#json">`JSON `</a>
   - [ ] <a href="#Newtonsoft">`Newtonsoft JSON解析包`</a>
       - <a href="#CreateJson">`创建JSON对象 JObject JArray`</a>
       - <a href="#ConvertJson">`转换对象 ConvertJson`</a>   
       - <a href="#JsonSerializer">`转换对象 JsonSerializer`</a>
   - [ ] <a href="#LitJson;">`LitJson JSON解析包`</a>    
       - <a href="#JSONNOGeneric">`非泛型 JsonMapper JsonData`</a> 
       - <a href="#JSONGeneric">`泛型 JsonMapper`</a> 

#####  <a href="#Newtonsoft"> Newtonsoft包</a>  `创建JSON对象` <a id="CreateJson"></a>  :star2: <a id="Newtonsoft" href="#top"> `置顶` :arrow_up:</a>
`为了适应JSON.NET手动创建JSON对象,Newtonsoft.Json.Linq 名称空间提供几个类型`
* **`JObject`**:`代表JSON对象,是一个字典,键是字符串值是JToken,提供索引访问,继承自JContainer`
* **`JArray`**:`代表JSON对象数组,继承自JContainer`
```C#
 //命名空间需要添加引用
  using Newtonsoft.Json;
  using Newtonsoft.Json.Linq;
  
  static void Main(string[] args)
  {
      var book1_ = new JObject();
      book1_["Name"] = "React Natice 6.0实践之路";
      book1_["Publisher"] = "China No.1";
      var book2_ = new JObject();
      book2_["Name"] = "JavaScript5 高级教程";
      book2_["Publisher"] = "China No.1";

      var list = new JArray();
      list.Add(book1_);
      list.Add(book2_);
      var bookRestore = new JObject();
      bookRestore["Name"] = "王者图书馆";
      bookRestore["books"] = list;
      Console.WriteLine(bookRestore);
      Console.ReadKey();
      /* 输出:
        {
          "Name": "王者图书馆",
          "books": [
            {
              "Name": "React Natice 6.0实践之路",
              "Publisher": "China No.1"
            },
            {
              "Name": "JavaScript5 高级教程",
              "Publisher": "China No.1"
            }
          ]
        }      
      
      */
  }
```
##### `转换对象 JsonConvert` <a id="ConvertJson"></a>  :star2: <a href="#top"> `置顶` :arrow_up:</a>

`JsonConvert 类允许对象树创建JSON,吧JSON字符串转换会对象树`
* `Formatting`：`枚举类型--None 将空白降到最低 Indented 提供更好的可读性`

```C#
    public class Book
    {
        public string ID{get;set;}
        public string Name{get;set;}
        public int PublishYear{get;set;}

        public override string ToString(){
            return $"ID:{ID} Name:{Name} Publish Year:{PublishYear}";
        }

        public Book(String ID,String Name,int year){
            this.ID=ID;
            this.Name=Name;
            this.PublishYear= year;
        }

    }
```

##### 将对象转换为JSON
**`SerializeObject`** `序列化方法`

```C#
    Book k=new Book("2018001XSST12","天下第一2",2018);
    String res= JsonConvert.SerializeObject(k,Formatting.Indented);
    Console.WriteLine(res);
    /* 输出:
      {
        "ID": "2018001XSST12",
        "Name": "天下第一2",
        "PublishYear": 2018
      }    
    */
```
##### 将JSON转换为字符串 <span id="LitJson"></span>
`通过泛型方法，确定返回类型,Json键对应为 属性,这个LitJSON不一样 LitJson解析包 对应的是字段`

```C#
    Book k=new Book("2018001XSST12","天下第一2",2018);
    String res= JsonConvert.SerializeObject(k,Formatting.Indented);
    Book t=JsonConvert.DeserializeObject<Book>(res);
    Console.WriteLine(t);  
    // 输出:ID:2018001XSST12 Name:天下第一2 Publish Year:2018
```
##### `转换对象 JsonSerializer` <a id="JsonSerializer"></a>  :star2: <a href="#top"> `置顶` :arrow_up:</a>
* `将JSON写入到文件中`
* `JsonSerializer 初始化要使用抽象方法 create`
```C#
    // 将对象写入到文件中
    using(StreamWriter writer=File.CreateText("resource/Inputbooks.json")){
        JsonSerializer serializer=JsonSerializer.Create(new JsonSerializerSettings{Formatting =Formatting.Indented });
        Book k=new Book("2018001XSST12","天下第一1",2018);
        Book k1=new Book("2018001XSST19","天下第一2",2019);
        Book k2=new Book("2018001XSST15","天下第一3",2020);
        Book k3=new Book("2018001XSST13","天下第一",2017);
        List<Book> bs=new List<Book>();
        bs.Add(k);
        bs.Add(k1);
        bs.Add(k2);
        bs.Add(k3);
        serializer.Serialize(writer,bs);
    }
```
* `将JSON 解析为对象 从文件中读出`

```C#
    // 将JSON读出文件 解析为对象
    using(StreamReader reader=File.OpenText("resource/Inputbooks.json")){
        JsonSerializer serializer=JsonSerializer.Create();
        var books=serializer.Deserialize(reader,typeof(List<Book>)) as List<Book> ;

        foreach(var book in books){
            Console.WriteLine(book);
        }                
    }
```

#####  <a href="#LitJson"> LitJson解析包</a> `非泛型 JsonMapper JsonData` <a id="JSONNOGeneric"></a>  :star2: <a href="#top"> `置顶` :arrow_up:</a>
`依靠`
```C#
    JsonData val=JsonMapper.ToObject(File.ReadAllText("resource/Inputbooks.json"));
    foreach(JsonData temp in val){
         JsonData  valueID=temp["ID"];  
         JsonData  valueName=temp["Name"];  
         JsonData  valueYear=temp["PublishYear"];    
         Console.WriteLine((new Book((string)valueID,(string)valueName,(int)valueYear)));
    }
```
* 转换回去 toJson
```C#
      Book k=new Book("2018001XSST12","天下第一1",2018);
      Book k1=new Book("2018001XSST19","天下第一2",2019);
      Book k2=new Book("2018001XSST15","天下第一3",2020);
      Book k3=new Book("2018001XSST13","天下第一",2017);
      List<Book> bs=new List<Book>();
      bs.Add(k);
      bs.Add(k1);
      bs.Add(k2);
      bs.Add(k3);
      String val=JsonMapper.ToJson(bs);
      Console.WriteLine(val);
```


##### `泛型 JsonMapper` <a id="JSONGeneric"></a>  :star2: <a href="#top"> `置顶` :arrow_up:</a>
`LitJson解析包 模型类`
* Book 模型类,`必须有无参数构造函数`

```C#
    public class Book
    {
        public string ID{get;set;}
        public string Name{get;set;}
        public int PublishYear{get;set;}

        public override string ToString(){
            return $"ID:{ID} Name:{Name} Publish Year:{PublishYear}";
        }

        public Book(String ID,String Name,int year){
            this.ID=ID;
            this.Name=Name;
            this.PublishYear= year;
        }

        public Book(){
            
        }

    }
```
```C#
      string strJson=File.ReadAllText("resource/Books.json");
      Book[] vals=JsonMapper.ToObject<Book[]>(strJson);
      foreach(var val in vals){
          Console.WriteLine(val);
      }
```
