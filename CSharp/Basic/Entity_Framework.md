<a id="top" href="#top">	:maple_leaf: EntityFramework ORM框架 :blue_heart:</a> 
-----
> `ORM框架`:`ORM`是通过使用描述对象和数据库之间映射的元数据,在我们想到描述的时候自然就想到了xml和特性(Attribute).目前的ORM框架中,Hibernate就是典型的使用xml文件作为描述实体对象的映射框架,而大名鼎鼎的Linq则是使用特性(Attribute)来描述的。

- [x] :maple_leaf: <a href="#ConnectDataBase">`链接数据库`</a>
- [x] :maple_leaf: <a href="#StartDataBase">`数据库初始化策略配置 `</a>
- [x] :maple_leaf: <a href="#DBContent">`数据上下文类 `</a>
- [x] :maple_leaf: <a href="#ModelAgreement">`约定,映射`</a>
    - <a href="#primarykey">`主键约束`</a>
	- <a href="#oneSerachMany">`一查找多`</a> 	
    - <a href="#ForeginKeyOntToMany">`外键约束一对多`</a>
    - <a href="#ForeginKeyManyToMany">`外键约束多对多`</a>	
- [x] :maple_leaf: <a href="#TableField">`表的字段约束,表属性`</a>	
	- <a href="#TableName">`表名称`</a>
	- <a href="#IdentityColumn">`标识列`</a>
	- <a href="#HasComputedColumnSql">`计算列`</a>
	- <a href="#HasPrecision">`设置精度`</a>
	- <a href="#StringLEngth">`字符串长度`</a>
	- <a href="#StringLEngth">`可不可空属性`</a>
	- <a href="#StringLEngth">`指定字段类型`</a>
	- <a href="#ComplexAttributes">`复合属性`</a>
- [x] :maple_leaf: <a href="#DataMoveNotDisapper">`数据迁移 `</a>
- [x] :maple_leaf: <a href="#ParalleToken">`乐观并发属性 `</a>
- [x] :maple_leaf: <a href="#IndexEntity">`索引 `</a>

#### <a id="ConnectDataBase" href="#ConnectDataBase">链接数据库</a> :star2: <a href="#top"> `置顶` :arrow_up:</a>
`通过配置文件`
##### 在App.Config里面添加

```XML
<connectionStrings>
	<add name="ConnectionString" connectionString="Data Source=Ip地址;Initial Catalog=BlogsDb;
			 Persist Security Info=True;User ID=UserName;Password=Password" 
			 providerName="System.Data.SqlClient"/>
</connectionStrings>
```
* 在数据库上下文类中调用构造函数
```C#
    class BLogDBContent:DbContext
    {
        public BLogDBContent() : base("name=ConnectionString")
        {
        }
    }
```

#### <a id="StartDataBase" href="#StartDataBase">数据库初始化策略</a> :star2: <a href="#top"> `置顶` :arrow_up:</a>
* **`Database.SetInitializer<MyContentClassName>(new DropCreateDatabaseIfModelChanges<MyContentClassName>());`**:`EF矿建检查到数据库发生了变化,将会更新模型` `MyContentClassName 用户自己的数据上下文类`
* **`Database.SetInitializer(new CreateDatabaseIfNotExists<MyContentClassName>());`**:`如果数据库不存在,那么就创建它.`
* **`Database.SetInitializer(new DropCreateDatabaseAlways<BLogDBContent>());`**:`无论数据库是否存在都创建数据库`
* **`Database.SetInitializer<BLogDBContent>(null);`**:`禁用数据库初始化策略`

##### 通过数据上下文类来配置策略
`通过数据上下文类的构造函数`


```C#
    class BLogDBContent:DbContext
    {
        public BLogDBContent() : base("name=ConnectionString")
        {
            Database.SetInitializer<BLogDBContent>(new DropCreateDatabaseIfModelChanges<BLogDBContent>());
        }
        public DbSet<Blog> Blogs { get; set;}
    }
```
#### <a id="DBContent" href="#https://msdn.microsoft.com/zh-cn/library/system.data.entity.dbcontext(v=vs.113).aspx">数据上下文类</a> :star2: <a href="#top"> `置顶` :arrow_up:</a>
`数据上下文类,DbContext 实例表示工作单元和存储库模式的组合，可用来查询数据库并将更改组合在一起，这些更改稍后将作为一个单元写回存储区中。 DbContext 在概念上与 ObjectContext 类似。`
* 构造函数
    * `DbContext()  `: `使用约定构造一个新的上下文实例以创建将连接到的数据库的名称。 按照约定，该名称是派生上下文类的全名（命名空间与类名称的组合）。 请参见有关这如何用于创建连接的类备注。`
    * `DbContext(String)    `: `可以将给定字符串用作将连接到的数据库的名称或连接字符串来构造一个新的上下文实例。 请参见有关这如何用于创建连接的类备注。`
    * `DbContext(DbCompiledModel)   `: `使用约定构造一个新的上下文实例以创建将连接到的数据库的名称，并从给定模型初始化该名称。 按照约定，该名称是派生上下文类的全名（命名空间与类名称的组合）。 请参见有关这
    如何用于创建连接的类备注。`
    * `DbContext(DbConnection, Boolean)     `: `通过现有连接来连接到数据库以构造一个新的上下文实例。 如果 contextOwnsConnection 是 false，则释放上下文时将不会释放该连接。`
    * `DbContext(String, DbCompiledModel)   `: `可以将给定字符串用作将连接到的数据库的名称或连接字符串来构造一个新的上下文实例，并从给定模型初始化该实例。 请参见有关这如何用于创建连接的类备注。`
    * `DbContext(ObjectContext, Boolean)    `: `围绕现有 ObjectContext 构造一个新的上下文实例。`
    * `DbContext(DbConnection, DbCompiledModel, Boolean)    `: `通过使用现有连接来连接到数据库以构造一个新的上下文实例，并从给定模型初始化该实例。 如果 contextOwnsConnection 是 false，则释放上下文时将不会
    释放该连接。`
* 常用方法
    * `SaveChanges  `: `将在此上下文中所做的所有更改保存到基础数据库。`
    * `SaveChangesAsync()   `: `将在此上下文中所做的所有更改异步保存到基础数据库。`
    * `SaveChangesAsync(CancellationToken)`: `将在此上下文中所做的所有更改异步保存到基础数据库。`
* `一个项目中可以有多个数据上下文类`

#### <a id="ModelAgreement" href="#ModelAgreement">约定,映射</a> :star2: <a href="#top"> `置顶` :arrow_up:</a>
`将模型类映射到数据库中,如果不想映射模型,可以通过Fluent API来忽略将其映射到数据库中`

-----
#####  <a id="primarykey" href="#primarykey">主键约定--`主键约束`</a>
* `Code First根据模型定义的ID(不区分大小写),或者是以类名加上ID的属性推断这样的属性为主键,如果主键为int或者guid类型,那么主键将被映射为标识列(自动增长)`
```C#
    using System.ComponentModel.DataAnnotations;
    using System.ComponentModel.DataAnnotations.Schema;
    
    public class Blog
    {
        public int BlogId { get; set; } //这一列会被映射为主键
        [MaxLength(50)]
        public string Name { get; set; }
        public string Url { get; set; }
        public DateTime? CreateTime { get; set; }
        public double BDouble { get; set; }
        public float BFloat { get; set; }

        public override string ToString()
        {
            return $"{Id} Name{Name} url:{Url}";
        }

    }
```
* `通过特性指定主键 [key]`
```C#
    [Table("Blog")] //指定表的名称,默认使用类名为表名称
    public class Blog
    {
        [Key]  //指定这个字段是主键
        public int Id { get; set; } 
        [MaxLength(50)]
        public string Name { get; set; }
        public string Url { get; set; }
        public DateTime? CreateTime { get; set; }
        public double BDouble { get; set; }
        public float BFloat { get; set; }

        public override string ToString()
        {
            return $"{Id} Name{Name} url:{Url}";
        }
    }
```
#### <a id="ForeginKeyOntToMany" href="#ForeginKeyOntToMany">外键约束一对多</a> :star2: <a href="#top"> `置顶` :arrow_up:</a>
* `通过导航属性的方式建立外键1对多关系`
* `这里通过产品和种类 建立1对多关系`

```C#
    class Category
    {
        [Key]
        public int Cid { get; set; }
        public string CName { get; set; }
    }
    //数据库
```
```sql
    CREATE TABLE [dbo].[Categories](
	[Cid] [int] IDENTITY(1,1) NOT NULL,
	[CName] [nvarchar](max) NULL)

```
* 主键不加特性注解的方式
```C#
    //产品类
    using System.ComponentModel.DataAnnotations;
    using System.ComponentModel.DataAnnotations.Schema;
    class Product
    {
        [Key]
        public int Pid { get; set; }
        public string PName { get; set; }
        public int Cid {get;set;}
        public virtual Category Cat { get; set; } //导航属性
    }
    // 数据库
```
* 主键加注解的方式 [ForeignKey("Cat")]

```C#
    class Product
    {
        [Key]
        public int Pid { get; set; }
        public string PName { get; set; }
        [ForeignKey("Cat")] //里面要跟着导航属性的名称
        public int CategoryId {get;set;}
        //延迟加载
        public virtual Category Cat { get; set; }//导航属性

    }

```
```sql
    CREATE TABLE [dbo].[Products](
        [Pid] [int] IDENTITY(1,1) NOT NULL,
        [PName] [nvarchar](max) NULL,
        [Cid] [int] NOT NULL,
        Foregin key
```
* 数据上下文类
```C#
    class ProductEntity: DbContext
    {
        public ProductEntity() : base("name=ConnectionString")
        {
            Database.SetInitializer<BLogDBContent>(new DropCreateDatabaseIfModelChanges<BLogDBContent>());

        }

        public DbSet<Product> products { get; set; }

        public DbSet<Category> categories { get; set; }
    }
```
* 读取数据
```C#
    static void ReadData()
    {
        ProductEntity entity = new ProductEntity();
        Product p = entity.products.Include("Cat").FirstOrDefault(); //注意这里非常重要 不然 p.cat为空

        if (p != null)
        {
            Console.WriteLine($"{p.Pid} {p.PName} {p.Cid} {p.Cat.CName}");
        }

    }
```
**`注意:因为外键有virtual修饰,使用延迟加载,所以在需要导航属性的时候,需要使用Include("导航属性");`**

#### <a id="ForeginKeyManyToMany" href="#ForeginKeyManyToMany">外键约束多对多</a> :star2: <a href="#top"> `置顶` :arrow_up:</a>

*  <a id="oneSerachMany">`通过一查找多`</a> `一查找多 通过集合[List<>/ICollection]和 virtual 关键字 实现`
```C#
    class Category
    {
        [Key]
        public int Cid { get; set; }
        public string CName { get; set; }

        public virtual ICollection<Product> Products { get; set; } //必须有virtual 注意是延迟加载
    }
```
##### 产品订单 多对多关系
`一个产品可以有多个订单,一个订单可以多多个产品`
* 产品表
```
    class Product
    {
        [Key]
        public int Pid { get; set; }
        public string PName { get; set; }
        public virtual List<Order> Orders { get; set; }
    }
```
* 订单表

```C#
    class Order
    {
        [Key]
        public int OId { get; set; }
        [MaxLength(80)]
        public string OName { get; set; }

        public virtual List<Product> Products { get; set; }
    }
```

```C#
    class ProductEntity: DbContext
    {
        public ProductEntity() : base("name=ConnectionString")
        {
            Database.SetInitializer<ProductEntity>(new DropCreateDatabaseIfModelChanges<ProductEntity>());
           
        }
        public DbSet<Order> orders { get; set; }

        public DbSet<Product> products { get; set; }
    }
```

`这样就默认会创建一张表 表名称:OrderProducts 两个字段:Order_OId / Product_Pid 主外键`


* 如果关系表要自己命名和键自己命名 可以使用如下方法
```C#
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Entity<Product>().HasMany(t => t.Orders).WithMany(a => a.Products).Map(m =>
    {
        m.ToTable("ProductOrder");
        m.MapLeftKey("Pid");
        m.MapRightKey("Oid");
    });
}
```


#### <a id="DataMoveNotDisapper" href="#DataMoveNotDisapper">数据迁移</a> :star2: <a href="#top"> `置顶` :arrow_up:</a>
`当模型发生变化的时候,总是重新创建数据库会导致很严重的问题,就是数据丢失`
##### `我们使用数据迁移解决这个问题，数据迁移有以下几个命令`
* `[Enable-Migrations]`:`在项目中启用代码迁移`
   * 参数:`-ContextTypeName`:`指定要使用的上下文,如何省略迁移,将会在目标项目中查找单个上下文类`
   * 参数:`-EnableAutomaticMigrations`:`指定是否在基架中是否使用自动迁移,如果省略,将禁止自动迁移`
   * 参数:`-MigrationsDiretory`:`指定包含迁移类的文件的目录名称,如果省略,那么迁移目录名称为Migrations`
   * 参数:`-ProjectName`:`制定基架配置类文件将会添加的项目,如果省略,将使用包管理器中默认使用的项目`
* `[Add-Migration]`:`对已经挂起模型改变搭建几家,也就是说在上次迁移后对模型进行了更改,以此为下一次迁移搭建基价,此时生成的模型状态为挂起状态
(获得)`
   * 参数:`-Name`:`自定义脚本名称`
   * 参数:`-ProjectName`:`和上面一样`
* `[Update-DataBase]`:`通过Add-Migrations 命令将挂起的模型迁移应用到数据库并保持数据同步`
   * 参数:`-Script`:`不是直接执行挂起的更改而是生成SQL脚本`
   * 参数:`无参数`:`更新数据库到最近的迁移`
* `[Get-Migrations]`:`显示已经应用到数据库的迁移`
-----

* 可能操作
   * **`打开Nuget管理控制台输入命令:Enable-Migrations `**
   * **`注意`**:`将迁移的项目设置为启动项目`
   * `然后回车后就会产生一个文件Migrations 里面存放 迁移记录类`
   * `然后就可以修改实体类了`
   * `然后搭建基架 Add-Migration AddPasswrod 后面的名称可以随意乱写标识一下做了什么更改就可以了`
   * `输入: Update-DataBase 更新数据库 使之生效`
```C#
 //三条命令
 Enable-Migrations
 Add-Migration AddPasswrod
 Update-DataBase
```
#### <a id="TableField" href="#TableField">表的字段约束,表属性</a> :star2: <a href="#top"> `置顶` :arrow_up:</a>
`表还有自增长,计算属性,默认值,约束鞥,下面我们来讲述这些映射,首先得知道在那些配置映射,也是在派生类上下文重写OnModelCreating`
```C#
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {

    }
```
* `对属性进行映射对常见的可以划分为数值映射,字符串映射,日期映射,当然还有xml映射,GUID映射,text映射等等`
#####  <a id="TableName">`表名称`</a>
```C#
  modelBuilder.Entity<Blog>().ToTable("Bloginfo"); //将Blog映射为表 表名称为Bloginfo
```
#####  <a id="PrimaryKeyByCode">`主键`</a>
* 单值主键
```C#
  modelBuilder.Entity<Blog>().HasKey(k => k.Id);
```
* 组合主键
```C#
modelBuilder.Entity<Blog>().HasKey(k =>
new {
   ID = k.Id,
   BlogName = k.Name
});
```
#####  <a id="IdentityColumn">`标识列`</a>
`通过属性 设置DatabaseGeneratedOption 枚举值`
```C#
  modelBuilder.Entity<Blog>().Property(P=>P.Id).HasDatabaseGeneratedOption(DatabaseGeneratedOption.Identity);
```
#####  <a id="HasComputedColumnSql">`计算列`</a>
```C#
   protected override void OnModelCreating(ModelBuilder modelBuilder)
   {
       modelBuilder.Entity<Person>()
           .Property(p => p.DisplayName)
           .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
   }
```
##### 数值映射说明
* `C#中的Int类型默认映射后对应于数据库中的Int类型`
* `C#中的Double类型默认映射后对应于数据库中的float类型`
* `C#中的float类型默认映射后对应于数据库中的real类型`
* `C#中的Decimal类型默认映射后对应于数据库中的Decimal(18,2)类型`
* `C#中的Int64类型默认映射后对应于数据库中的BigInt类型`
##### <a id="HasPrecision">`设置精度`</a>
```C#
  modelBuilder.Entity<Product>().Property(p => p.RealPrice).HasPrecision(18, 4);// 4位小数 默认两位小数
```
##### <a id="StringLEngth">`字符串长度`</a>
`字符串自动映射到nvarchar,默认为 nvarchar(max)`
```C#
  modelBuilder.Entity<Product>().Property(p => p.PName).HasMaxLength(50);
```
##### <a id="StringLEngth">`可不可空属性`</a>
```C#
  modelBuilder.Entity<Product>().Property(p => p.PName).IsOptional(); //可空
  modelBuilder.Entity<Product>().Property(p => p.PName).IsRequired(); //不可空
```
##### <a id="StringLEngth">`指定类型`</a>
```C#
  modelBuilder.Entity<Product>().Property(p => p.PName).HasColumnType("char");
```
##### <a id="StringLEngth">`日期`</a>
`日期类型默认为Date类型,但是数据库中海油DateTime,datetime2,smalldatetime，time(7) 可以通过HasColumnType指定`
```C# 
  modelBuilder.Entity<Blog>().Property(p => p.CreateTime).HasColumnType("datetime2");
```
##### <a id="ComplexAttributes">`复合属性`</a>
* 将一些属性合并为一个类
```C#
    public class User
    {
        [Key]
        public string ID { get; set; }
        public string Pwd { get; set; }
        public string Name { get; set; }
        public int Age { get; set; }
        public Address address { get; set; } //地址类 复合属性
    }
```
* 地址类
```C#
    public class Address
    {
        public string Street { get; set; }
        public string City { get; set; }
        public string ZipCode { get; set; }
    }
```
* 映射结果
```SQL
CREATE TABLE [dbo].[Users](
	[ID] [nvarchar](128) NOT NULL,
	[Pwd] [nvarchar](max) NULL,
	[Name] [nvarchar](max) NULL,
	[Age] [int] NOT NULL,
	[address_Street] [nvarchar](max) NULL,
	[address_City] [nvarchar](max) NULL,
	[address_ZipCode] [nvarchar](max) NULL
    )
```
```C#
public async Task<ActionResult> Index()
{
    using(StoreContent store = new StoreContent())
    {
        store.Database.Connection.Open();
        User u = new User();
        u.Age = 19;
        u.Name = "无双";
        u.ID = "201611000001";
        u.Pwd = "ASCBASKJDPOWQMDASDKJIOQWDNQWKDNQWUYUE123";
				
        u.adrress = new Address(); //不管address属性是否需要填值 都要做这一步 不然一定要报错
				
				
        u.address.City = "BeiJing";
        u.address.ZipCode = "56300";
        u.address.Street = "天下无敌街道";
        store.Users.Add(u);
        int i =await store.SaveChangesAsync();
        ViewBag.status = i;
    }
            
    return View();
}
```
**`规则:`** `复合属性总是要初始化,为了防止抛弃异常可以在复合类中使用构造函数初始化`
```C#
    public class User
    {
        [Key]
        public string ID { get; set; }
        public string Pwd { get; set; }
        public string Name { get; set; }
        public int Age { get; set; }
        public Address address { get; set; } //地址类 复合属性
				//使得address属性能够总是被初始化 防止实体异常
				public User(){
					  address =new Address();
				}
    }
```
#### <a id="ParalleToken" href="#ParalleToken">乐观并发属性</a> :star2: <a href="#top"> `置顶` :arrow_up:</a>
* `当一个用户对实体的数据进行编辑，然后另一个用户在前一个用户将更改写入到数据库之前更新同一实体的数据时将发生并发冲突。如果你没有启用冲突检测，那么最后一次对数据库的更新将会覆盖其他用户对数据库所做的更改。在大部分应用程序中，这种风险是可以接受的：如果只有少量的用户，或者很少的更新，或者被覆盖的数据是不太重要的，实现并发冲突可能是得不偿失的。在这种情况下，你不需要配置应用程序以处理并发冲突`
* `EF框架解决并发有两种方式,一种是利用并发Token，另一种则是利用行版本的方式,配置如下`
* `并发分悲观并发和乐观并发。`
	* `悲观并发：比如有两个用户A,B，同时登录系统修改一个文档，如果A先进入修改，则系统会把该文档锁住，B就没办法打开了，只有等A修改完，完全退出的时候B才能进入修改。`
	* `乐观并发：同上面的例子，A,B两个用户同时登录，如果A先进入修改紧跟着B也进入了。A修改文档的同时B也在修改。如果在A保存之后B再保存他的修改，此时系统检测到数据库中文档记录与B刚进入时不一致，B保存时会抛出异常，修改失败。`
  * `Entity Framework不支持悲观并发，只支持乐观并发 如果要对某一个表做并发处理，就在该表中加一条Timestamp类型的字段。注意，一张表中只能有一个Timestamp的字段。Data Annotations中用Timestamp来标识设置并发控制字段，标识为Timestamp的字段必需为byte[]类型。`
```C#
   public class Person
   {
        public int PersonId { get; set; }
        public int SocialSecurityNumber { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        [Timestamp]
        public byte[] RowVersion { get; set; }
    }
```
* `Fluent API用IsRowVersion方法`
```c#
  modelBuilder.Entity<Person>().Property(p => p.RowVersion).IsRowVersion();
```
* [`并发冲突博客人`](https://blog.csdn.net/johnsonblog/article/details/39298201)
#### <a id="IndexEntity" href="#IndexEntity">索引</a> :star2: <a href="#top"> `置顶` :arrow_up:</a>
`按照约定，为外键使用每个属性 （或组的属性） 中创建索引。`
```C#
 protected override void OnModelCreating(ModelBuilder modelBuilder)
 {
     modelBuilder.Entity<Blog>().HasIndex(b => b.Url);
 }
 
 public class Blog
 {
    public int BlogId { get; set; }
    public string Url { get; set; }
 }
```
* `也可指定性地要求索引的值唯一，也就是说，对于给定的属性，任何两个实体的值都不能相同。`
```C#
 modelBuilder.Entity<Blog>()
  .HasIndex(b => b.Url)
  .IsUnique();
  //复合索引
   modelBuilder.Entity<Blog>()
  .HasIndex(b =>new {url= b.Url,name=b.Name})
  .IsUnique();
```
* `索引Index 默认为非聚集非唯一索引`
* `对于外键按照约定最好创建索引`
* `对于经常需要查询的字段创建索引`
* `索引不在多,在巧`
* `如果字段类型是string 并且没有规定 MaxLength(number) 那么无法在这个字段上面创建索引`
