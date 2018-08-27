<a id="top" href="#top">	:maple_leaf: EntityFramework ORM框架 [注解版] :blue_heart:</a> 
-----
- [x] :maple_leaf: <a href="#ModelAgreement">表属性,约定,映射</a>
  - [x] <a href="TableName">`表名另取 [Table("tableName")]`</a>
  - [x] <a href="Mainkey">`主键 [key] `</a>
- [x] :maple_leaf: <a href="#TableStructure">表结构</a>
  - [x] <a href="Index">`索引 [Index] `</a>

##### <a id="TableName" href="#TableName">表名另取 `[Table("OrderInfo")]`</a> :star2: <a href="#top"> `置顶` :arrow_up:</a>
`默认是以类名称为表名`
```C#
    [Table("OrderInfo")]
    class Order
    {  
        //etc   
    }
```
##### <a id="Mainkey" href="#Mainkey">主键 `[key]` </a> :star2: <a href="#top"> `置顶` :arrow_up:</a>
```C#
    [Key]
    public int OId { get; set; } //字段上面加上key注解
```

#### <a id="Index" href="#Index">索引 `[Index]` </a> :star2: <a href="#top"> `置顶` :arrow_up:</a>
* `索引Index 默认为非聚集非唯一索引`
* `对于外键按照约定最好创建索引`
* `对于经常需要查询的字段创建索引`
* `索引不在多,在巧`
```C#
    [Table("Users")]
    public class User
    {
        [Key]
        public string ID { get; set; }
        public string Pwd { get; set; }
        public string Name { get; set; }
        public int Age { get; set; }
        public Address adrress { get; set; }
        [Index]
        [ForeignKey("classTeam")]
        public int UserClassTeam { get; set; } 
        public virtual ClassTeam classTeam { get; set; }
    }
```
* `创建唯一索引 ` ：**`[Index(IsUnique = true)]`**
```C#
    [Table("Users")]
    public class User
    {
        [Index(IsUnique = true)]
        [Key]
        public string ID { get; set; }
        public string Pwd { get; set; }
        public string Name { get; set; }
        public int Age { get; set; }
        public Address adrress { get; set; }
        [Index]
        [ForeignKey("classTeam")]
        public int UserClassTeam { get; set; } 
        public virtual ClassTeam classTeam { get; set; }
    }
```
