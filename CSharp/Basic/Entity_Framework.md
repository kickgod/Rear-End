
<a id="top" href="#top">	:maple_leaf: EntityFramework ORM框架 :blue_heart:</a> 
-----
> `ORM框架`:`ORM`是通过使用描述对象和数据库之间映射的元数据,在我们想到描述的时候自然就想到了xml和特性(Attribute).目前的ORM框架中,Hibernate就是典型的使用xml文件作为描述实体对象的映射框架,而大名鼎鼎的Linq则是使用特性(Attribute)来描述的。

- [x] :maple_leaf: <a href="#ConnectDataBase">`链接数据库`</a>
- [x] :maple_leaf: <a href="#StartDataBase">`数据库初始化策略配置 `</a>

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

