<a id="top" href="#top">	:maple_leaf: EntityFramework ORM框架 [注解版] :blue_heart:</a> 
-----
- [x] :maple_leaf: <a href="#ModelAgreement">表约定,映射</a>
  - [x] <a href="#Mainkey">`主键 [key] `</a>
  - [x] <a href="#DefaultValue">`默认值 [DefaultValue(value)] `</a>
- [x] :maple_leaf: <a href="#TableStructure">表结构</a>
  - [x] <a href="#TableName">`表名另取 [Table("tableName")]`</a>
  - [x] <a href="#Index">`索引 [Index] `</a>
  

##### <a id="Mainkey" href="#Mainkey">主键 `[key]` </a> :star2: <a href="#top">  :arrow_up:</a>
`1.主键约束`
```C#
  [Key]
  public int OId { get; set; } //字段上面加上key注解
```
##### <a id="Mainkey" href="#DefaultValue">默认值 `[DefaultValue(value)]` </a> :star2: <a href="#top"> :arrow_up:</a>
* `输入字符串是最安全的，可以传入sql函数 向下面这样`
```C#
  [Table("Movie")]
  public class Movie
  {
      [Key,DisplayName("编号")]
      public int MovieID { get; set; }
      [DisplayName("标题")]
      public string Title { get; set; }
      [DisplayName("发行时间")]
      [DefaultValue("getdate()")]
      public DateTime ReleaseDate { get; set; }
      [DisplayName("种类")]
      public string Genre { get; set; }
      [DisplayName("价格")]
      public decimal Price { get; set; }
  }
```
##### <a id="TableName" href="#TableName">表名另取 `[Table("tableName")]`</a> :star2: <a href="#top">:arrow_up:</a>
`默认是以类名称为表名`
```C#
  [Table("OrderInfo")]
  class Order
  {  
      //etc   
  }
```
##### <a id="Index" href="#Index">索引 `[Index]` </a> :star2: <a href="#top"> :arrow_up:</a>
* `索引Index 默认为非聚集非唯一索引`
* `对于外键按照约定最好创建索引`
* `对于经常需要查询的字段创建索引`
* `索引不在多,在巧`
* `如果字段类型是string 并且没有规定 MaxLength(number) 那么无法在这个字段上面创建索引`
* `SQL Server ( 直到 2008 R2 ) 中有一个限制，varchar(MAX) 和 nvarchar(MAX) ( 和其他几种类型，如文本，ntext ) 不能用于索引`
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
      public int ClassTeamID { get; set; } 
      public virtual ClassTeam classTeam { get; set; }
  }
```
`创建唯一索引 ` ：**`[Index(IsUnique = true)]`**
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
* `错误的索引创建方式`
```C#
  public class User
  {
      [Key]
      public string ID { get; set; }
      public string Pwd { get; set; }
      [Index]
      [MaxLength(60)]
      public string Name { get; set; }
      [Index]
      public int Age { get; set; }
      public Address adrress { get; set; }
  }
```
`解释`:`这个会 报错表 dbo.Users 中的列 Name 的类型不能用作索引中的键列。 因为默认string 会被映射为 varchar(max) 这个是无法做索引的,而字段的顺序是先索引后字段规定 那就创建索引的时候还是 varchar(max) 所以无法创建索引`
```C#
  public class User
  {
      [Key]
      public string ID { get; set; }
      public string Pwd { get; set; }
      [MaxLength(60)] 
      [Index]
      public string Name { get; set; }
      [Index]
      public int Age { get; set; }
      public Address adrress { get; set; }
  }
```
