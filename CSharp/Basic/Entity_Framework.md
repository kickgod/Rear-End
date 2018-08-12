
<a id="top" href="#top">	:maple_leaf: EntityFramework ORM框架 :blue_heart:</a> 
-----
> `ORM框架`:`ORM`是通过使用描述对象和数据库之间映射的元数据,在我们想到描述的时候自然就想到了xml和特性(Attribute).目前的ORM框架中,Hibernate就是典型的使用xml文件作为描述实体对象的映射框架,而大名鼎鼎的Linq则是使用特性(Attribute)来描述的。

- [x] :maple_leaf: <a href="#ConnectDataBase">`链接数据库`</a>
- [x] :maple_leaf: <a href="#StartDataBase">`数据库初始化策略配置 `</a>
- [x] :maple_leaf: <a href="#DBContent">`数据上下文类 `</a>
- [x] :maple_leaf: <a href="#ModelAgreement">`约定,映射`</a>
    - <a href="#primarykey">`主键约束`</a>
    - <a href="#ForeginKeyOntToMany">`外键约束一对多`</a>
- [x] :maple_leaf: <a href="#DataMoveNotDisapper">`数据迁移 `</a>    


#### <a id="ConnectDataBase" href="#ConnectDataBase">链接数据库</a> :star2: <a href="#top"> `置顶` :arrow_up:</a>
`通过配置文件`
##### 在App.Config里面添加

```XML
    <connectionStrings>
      <add name="ConnectionString" connectionString="Data Source=Ip地址;Initial Catalog=BlogsDb;
           Persist Security Info=True;User ID=UserName;Password=Password"  providerName="System.Data.SqlClient"/>
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

#### <a id="DataMoveNotDisapper" href="#DataMoveNotDisapper">数据迁移</a> :star2: <a href="#top"> `置顶` :arrow_up:</a>
`当模型发生变化的时候,总是重新创建数据库会导致很严重的问题,就是数据丢失`
* `我们使用数据迁移解决这个问题`
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

