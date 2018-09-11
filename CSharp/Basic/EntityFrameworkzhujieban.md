<a id="top" href="#top">	:maple_leaf: EntityFramework ORM框架 [注解版] :blue_heart:</a> 
-----
- [x] :maple_leaf: <a href="#ModelAgreement">表约定,映射</a>
  - <a href="#Mainkey">`主键 [key] `</a>
  - <a href="#DefaultValue">`默认值 亲测无效 [DefaultValue(value)] `</a>
  - <a href="#notNullProperty">`不可空属性 [Required] `</a>
  - <a href="#NullProperty">`可空属性 ?`</a>
  - <a href="#ziduanLength">`字段长度 [MaxLength] `</a>
- [x] :maple_leaf: <a href="#TableStructure">表结构</a>
  - <a href="#TableName">`表名另取 [Table("tableName")]`</a>
  - <a href="#Index">`索引 [Index] `</a>
  - <a href="#columnSet" >`列设置 [Column("ColumnName",TypeName ="sqltypeName`]</a>
- [x] :maple_leaf: <a href="#EFYuedingPeizhi">EF框架映射配置</a>
  - <a href="#NotMapped">`属性排除不映射为表字段 [NotMapped] `</a> 
##### 说明
`虽然EF框架很复杂而且庞大,但是在创建实体表的使用上面,使用Map类 映射的方式,总是不如注解方法来得快捷,而且有效也不需要注册Map类,而注解方法简单
 方便,清晰易懂,而且需要记忆的少，且稳定.`
##### <a id="Mainkey" href="#Mainkey">主键 `[key]` </a> :star2: <a href="#top">  :arrow_up:</a>
`1.主键约束`
```C#
  [Key]
  public int OId { get; set; } //字段上面加上key注解
```
##### <a id="DefaultValue" href="#DefaultValue">默认值 `[DefaultValue(value)]` </a> :star2: <a href="#top"> :arrow_up:</a>
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
##### <a id="notNullProperty" href="#notNullProperty">默认值 亲测无效 `[DefaultValue(value)]` </a> :star2: <a href="#top"> :arrow_up:</a>
```C#
  public class Blog
  {
      public int BlogId { get; set; }
      [Required]
      public string Url { get; set; }
  }
```
`你可以尝试构建一个构造函数,在构造函数里里面初始化属性,间接的实现默认值!`

##### <a id="TableName" href="#TableName">不可空属性 `[Required]`</a> :star2: <a href="#top">:arrow_up:</a>
`默认是以类名称为表名`
```C#
  [Table("OrderInfo")]
  class Order
  {  
      //etc   
  }
```
##### <a id="NullProperty" href="#NullProperty">可空属性 `?`</a> :star2: <a href="#top">:arrow_up:</a>
`下面定义了一张表 里面的 isExam ...属性都是可空的 在后面加一个? 就行`
```C#
  public class Student
  {
      [Key]
      public String StudentID { get; set; } //学号

      [MaxLength(1000)][Required]
      public String Passwrod { get; set; } //密码

      [MaxLength(200)]
      public String Email { get; set; } //邮件

      [MaxLength(200)]
      public String IDNumber { get; set; } //身份证号码

      public bool Sex { get; set; } //性别

      [MaxLength(100)]
      public String Name { get; set; } //姓名

      [Required]
      public int Grade { get; set; } //年级

      public StudentIdentity StudentIdentity { get; set; } //枚举 本科生/研究生
      public Boolean? IsExam { get; set; } //是否已经参加考试
      public int? JoinExamYear { get; set; } //什么年度参加考试的
      public float? ExamScore { get; set; } //考试分数

      public Boolean IsMalicious { get; set; } //是否恶意不学习,而多次刷分
      [ForeignKey("Profession")]
      public int ProfessionID { get; set; }  //外键：学生专业
      public virtual Profession Profession { get; set; }
  }
```
##### <a id="ziduanLength" href="#ziduanLength">字段长度 ` [MaxLength]` </a> :star2: <a href="#top"> :arrow_up:</a>
`如果不规定字段长度那么string 会被映射为 nvarchar(max) 这是极其不合理的,所以可以使用这个注解完成我们的需求`
```C#
  [MaxLength(3000)]
  public String Title { get; set; }  // 题干

  [MaxLength(2000)]
  public string Answer { get; set; } // 主观题题目 答案
```
##### <a id="columnSet" href="#columnSet">列 `[Column("ColumnName",TypeName ="sqltypeName"]` </a> :star2: <a href="#top"> :arrow_up:</a> 
* `第一个参数`:`类名称`
* `第二个 TypeName`：`指定类类型`
```C#
  [Column("AddTime",TypeName ="datetime2")]
  public DateTime AddTime { get; set; } //添加时间
  
  //注意： [Column("AddTime",TypeName ="datetime2(7)")]  datetime2(7) 不支持后面的 (7)
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
`错误的索引创建方式`
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
##### <a id="NotMapped" href="#NotMapped">属性排除不映射为表字段`[NotMapped]` </a> :star2: <a href="#top">  :arrow_up:</a>
```C#
  public class Blog
  {
      [key]
      public int BlogId { get; set; }
      public string Url { get; set; }

      [NotMapped]
      public DateTime LoadedFromDatabase { get; set; } //该属性不会被映射打数据库中
  }

```
