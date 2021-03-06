<a id="top" href="#top">	:maple_leaf: EntityFramework 实体状态和数据操作 :blue_heart:</a> 
----
`我们通过EntityFramework来说对数据进行操作并持久化到数据库,那么EF,要通过EF上下文泪来维护实体的状态,明确知道每一个状态对应的操作,也就是说
EF上下文来维护尸体的状态,操作数据库的方式也有很多变化`
- [x] :maple_leaf: <a href="#EntityState">实体状态</a>
- [x] :maple_leaf: <a href="#DataCorpration">数据操作</a>
   - <a href="#DataQuery">`查询数据`</a> 
- [x] :maple_leaf: <a href="#LoadRelationData">`加载关联数据`</a> 
   - <a href="#LayzLoadData">`延迟加载`</a> 
   - <a href="#EagerLoadData">`饥饿加载`</a>
   - <a href="#ExplicilyLoadData">`显式加载`</a>
- [x] :maple_leaf: <a href="#SqlQuery">`原始查询`</a> 
   - <a href="#DbContextDatabase">`上下文类Database属性查询`</a> 
   - <a href="#EntitySearch">`实体执行查询`</a>
   - <a href="#SearchSendParameter">`传递参数执行查询`</a>
   - <a href="#SearchCarryOutStore">`原始查询支持存储过程`</a>
- [x] :maple_leaf: <a href="#CarryOutSQLCommand">`支持SQL命令`</a>
- [x] :maple_leaf: <a href="#DataSave">`数据保存`</a>
##### <a id="EntityState" href="#EntityState">实体状态</a> :star2: <a href="#top">:arrow_up:</a>
`EF实体状态存在于命名空间System.Data.Entity,下的EntityState枚举中,通过实体标记可以实现一些基本操作`
```C#
 public enum EntityState {
   Detached = 1,
   Unchanged = 2,
   Added = 4,
   Deleted = 8,
   Modified = 16
 }
```
##### Added 状态
`Added 状态针对添加操作,当标记为此状态时,表明实体被上下文所追踪但是不存在于数据库.当调用SaveChanges时,则会插入数据库中,表明Added状态有两种方式,
一种为直接通过Add方法调用,另一种为显示标记,通过Entry方法调用,代码如下`
```C#
public  ActionResult Index()
{
    using (StoreContent store=new StoreContent())
    {
        Movie m = new Movie();
        m.Genre = "阿斯顿";
        m.Title = "阿斯顿";
        m.ReleaseDate = DateTime.Now;
        m.Price = 98.9M;
        store.Movies.Add(m); //通过方法增加  也可以通过标记增加
        ViewBag.status = store.Entry(m).State; //Added
        store.SaveChanges();
        ViewBag.status_ = store.Entry(m).State; //Unchanged
    }
    return View();
}
//通过标记增加
  using (StoreContent store=new StoreContent())
  {
      Movie m = new Movie();
      m.Genre = "asd";
      m.Title = "asd";
      m.ReleaseDate = DateTime.Now;
      m.Price = 28.9M;
      store.Entry(m).State = EntityState.Added; //标记实体为添加状态,保存数据库的时候把实体添加到数据库
      store.SaveChanges();

  }
```

##### Unchanged 状态
`实体被Entity F上下文所追踪,但是存在与数据库中的值尚未发生改变`
##### Modified 状态
`实体被EF 上下文所追踪并存在于数据库,同时部分或者所有属性值已被更改,当调研SaveChanges时 则更新属性到数据库中,EF不存在Update方法`
```C#
 using (StoreContent store=new StoreContent())
 {
     //更新的方法
     Movie m = new Movie();
     m.MovieID = 2;
     m.Genre = "修改类型";
     m.Title = "修改标题";
     m.ReleaseDate = DateTime.Now;
     m.Price = 28.9M;
     //编号为2的实体 已经被更新
     store.Entry(m).State = EntityState.Modified;
     ViewBag.Message = store.Entry(m).State;
     store.SaveChanges();          
 }
```
##### Deleted 状态
`通过这个标记,实体被EF上下文所追踪并存在与数据库中,当标记为Delete状态时,SavaChange时数据将在数据库中被删除,同理,和Added状态一样,可以通过调用
Remove或者RemoveChange方法标记尸体,或者标记实体集合为Delete状态`
```C#
public  ActionResult Index()
{
    using (StoreContent store=new StoreContent())
    {
        //标记删除
        Movie m = new Movie();
        m.MovieID = 2;
        m.Genre = "天下无贼";
        //编号为2的实体 保存数据库的时候删除
        // store.Movies.Remove(m); //通过方法删除
        store.Entry(m).State = EntityState.Deleted;
        ViewBag.Message = store.Entry(m).State;
        store.SaveChanges();
    }
    return View();
}
```
##### <a id="DataQuery" href="#DataQuery">数据查询</a> :star2: <a href="#top">:arrow_up:</a>
`在EF框架中 使用Linq To Entity查询` [`Linq To Entity`](https://github.com/kickgod/ProgramingLanguage/blob/master/CSharp/Basic/Csharpe6LinqToEntity.md) `学完了它 就会基本的查询方法了`
##### <a id="LoadRelationData" href="#LoadRelationData">加载关联数据</a> :star2: <a href="#top">:arrow_up:</a>
`EF6.X中 数据加载方式三种:`,`延迟加载`,`饥饿加载`,`显示加载`,`三种加载方式都有其场景,利用不当将会导致性能问题,进阶中会进一步讲到这三者究竟
如何使用而不是滥用一通.饥饿加载的时候要使用Include来实现,默认情况下需要在Include方法中填写加载导航属性字符串.`
#####  <a id="LayzLoadData" href="#LayzLoadData">延迟加载</a> :star2: <a href="#top">:arrow_up:</a>
`顾名思义,延迟加载就是当我们需要的时候才会加载,当第一次访问实体/实体的属性的时候,实体或实体的集合将自动从数据库加载.访问导航属性时,相关对象/子对象
不会自动加载当使用POCO(Plain Old CLR Objects) 实体类型时,通过创建派生代理类型的实例,然后覆盖virtual 的导航属性从而实现延迟加载,好像听起来
有点晦涩难懂,进阶会详细说明`
```C#
  public BLOGEntity() : base("name=ConnectionString")
  {
      Database.SetInitializer<BLOGEntity>(new DropCreateDatabaseIfModelChanges<BLOGEntity>());
      Configuration.LazyLoadingEnabled = false; //在EF上下文派生类中收到关闭延迟加载
  }
```
* `延迟加载会有一个序列化Json的循环引用问题当使用Newtonsoft.Json序列化的时候,但是测试后发现LitJson包 序列化就没有问题,如果有问题使用投影解决就行,将属性的值通过select方法投影到一个新类型上面`
#####  <a id="EagerLoadData" href="#EagerLoadData">饥饿加载</a> :star2: <a href="#top">:arrow_up:</a>
`查询实体时,加载相关实体/子实体 作为查询的一部分,加载父对象时同时加载子对象,在EF中可以使用DBSet<T>.Include() 方法来实现饥饿
加载,和延迟加载,不同的是,饥饿加载时,无论我们用或者不用,利用Include方法斗殴会对子对象进行加载,下面利用Include来加载Customer中
的Order对象 饥饿加载的本质提前加载数据`
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
#####  <a id="ExplicilyLoadData" href="#ExplicilyLoadData">显式加载</a> :star2: <a href="#top">:arrow_up:</a>
`延迟执行有延迟加载和显示加载两种方式,即使我们禁用了延迟加载任然可以通过显示加载来延迟加载相关实体,通过DbEntity<T>.Reference("").Load()加载
实体,通过DbEntityEntry<T>().Collection("").Load()`
```C#
  static void ReadData()
  {
      ProductEntity entity = new ProductEntity();
      Product p = entity.products.FirstOrDefault();

      entity.Entry(p).Collection("Cat").Load();

      var cs = p.Cat;

      if (cs != null)
      {
          Console.WriteLine($"{cs.Cid} {cs.CName}");
      }
  }

```
#### <a id="SqlQuery" href="#SqlQuery">原始查询</a> :star2: <a href="#top">:arrow_up:</a>
`在实体上面执行原始查询,EF框架底层还是基于ADO.NET,对于一些复杂的查询,我们还是需要理由原始查询或者存储过程来完成所有Entity Framework 团队
还是做了底层查询,在EF框架中,我们可以通过SqlQuery方法来进行原始查询,SqlQuery方法有如下两种形式`
```C#
    Context.Database.SqlQuery<TElement>(String sql,params object[]);
    Context.Products.SqlQuery<TElement>(String sql,object[] parameters);
```
`原始查询只有当结果全部枚举完成后（也就是只有ToList后） 才会和数据库进行交互否则不会执行查询`
```C#
   BLOGEntity db = new BLOGEntity();
   var BUserPage = db.Database.SqlQuery<BUser>("Select * from dbo.BUser").ToList();
   return View(BUserPage);
```
##### <a href="#DbContextDatabase" id="DbContextDatabase">`上下文类SqlQuery查询`</a>
`通过上下文类可以对整个数据库进行查询`
**`支持对表数据集合泛型返回`**
```C#
   public ActionResult Index()
   {  
      var BUserPage = db.Database.SqlQuery<BUser>("Select *  from dbo.BUser").ToList();
      return View(BUserPage);
   }
```
**`支持函数统计泛型返回`**
```C#
  public string LC()
  {
      var TBUserCount = db.Database.SqlQuery<int>("Select count(*) from dbo.BUser").ToList();
      String JS = "";
      if (TBUserCount != null)
      {
          JS = LitJson.JsonMapper.ToJson(TBUserCount);
      }
      return JS;
  }
```
**`支持单列泛型方法返回`**
```C#
  public string LC()
  {
      var TBUserCount = db.Database.SqlQuery<String>("Select Name from dbo.BUser").ToList();
      String JS = "";
      if (TBUserCount != null)
      {
          JS = LitJson.JsonMapper.ToJson(TBUserCount);
      }
      return JS;
  }
```
**`不支持单列非泛型方法返回`**
```C#
   //SqlQuery没有使用泛型 
   var TBUserCount = db.Database.SqlQuery("Select Name from dbo.BUser").ToList();
```
##### <a href="#EntitySearch" id="EntitySearch">`实体SqlQuery查询`</a>
`通过实体可以对这个实体对应的表进行查询`
**`支持对表数据集合泛型返回`**
```C#
   public ActionResult Index()
   {  
      var BUserPage = db.BUsers.SqlQuery("Select *  from dbo.BUser").ToList();
      return View(BUserPage);
   }
```
**`不支持函数统计返回`**
```C#
  public string LC()
  {
      var TBUserCount = db.BUsers.SqlQuery("Select count(*) from dbo.BUser").ToList();
      String JS = "";
      if (TBUserCount != null)
      {
          JS = LitJson.JsonMapper.ToJson(TBUserCount);
      }
      return JS;
     /*System.Data.Entity.Core.EntityCommandExecutionException:“The data reader is 
     incompatible with the specified 'Blogs.Models.BUser'. A member of the type, 
     'BUserID', does not have a corresponding column in the data reader with the same name.”*/
  }
```
**`不支持单列方法返回`**
```C#
  public string LC()
  {
      var TBUserName = db.BUsers.SqlQuery("Select Name from dbo.BUser").ToList();
      String JS = "";
      if (TBUserName != null)
      {
          JS = LitJson.JsonMapper.ToJson(TBUserName);
      }
      return JS;
  }
  /*System.Data.Entity.Core.EntityCommandExecutionException:“The data reader is 
  incompatible with the specified 'Blogs.Models.BUser'. A member of the type, 
  'BUserID', does not have a corresponding column in the data reader with the same name.”*/
```
##### <a href="#SearchSendParameter" id="SearchSendParameter">`传递参数执行查询`</a>
`通过方法传递的参数来查询对应的数据从而进行其他操作,传递操作又分为两种,一种是通过string.format进行拼接,另一种则是通过参数化SQL来实现
,string.format会到导致SQL 注入问题,推荐参数化SQL`

--------------------------------------
`string.format 形式`
```C#
  public ActionResult Index()
  {
      int id = 5;
      //会被SQL注入 极其危险
      var BUserPage = db.BUsers.SqlQuery(String.Format("Select *  from dbo.BUser
      where BUserID >= {0}",id)).ToList();
      var BUserPage = db.BUsers.SqlQuery($"Select *  from dbo.BUser where 
      BUserID >= {id}").ToList();
      return View(BUserPage);
  }
```
`参数化SQL实现`
```C#
  public ActionResult Index()
  {
      int id = 5;
      var parameterID = new SqlParameter();

      parameterID.ParameterName = "@Id";
      parameterID.SqlDbType = SqlDbType.Int;
      parameterID.Value = id;
      parameterID.IsNullable = false;

      var BUserPage = db.BUsers.SqlQuery("Select *  from dbo.BUser where
      BUserID >= @Id", parameterID).ToList();
      return View(BUserPage);
  }
```
`参数化SQL数组`
```C#
  public ActionResult Index()
  {
      int id = 5;

      var parameters = new SqlParameter[]
      {
          new SqlParameter(){ParameterName="@Id",SqlDbType = SqlDbType.Int, Value = id },
          new SqlParameter(){ParameterName="@UserSex",SqlDbType = SqlDbType.Bit, Value = true }
      };
      var BUserPage = db.BUsers.SqlQuery("Select *  from dbo.BUser where BUserID >= 
      @Id and Sex =@UserSex", parameters).ToList();
      return View(BUserPage);
  }
```
##### <a href="#SearchCarryOutStore" id="SearchCarryOutStore">`原始查询支持存储过程`</a>
`新建一个存储过程`
```sql
   Create PROCEDURE [dbo].[getAllUser]
   (
     @Sex as bit
   )
   AS
   BEGIN
     Select * from dbo.BUser where BUser.Sex= @Sex ;  
   END

```
```C#
     public ActionResult Index()
     {
         var parameters = new SqlParameter() { ParameterName = "@UserSex", 
         SqlDbType = SqlDbType.Bit, Value = true };
         var BUserPage = db.BUsers.SqlQuery("dbo.getAllUser @UserSex;",
         parameters).ToList();
         return View(BUserPage);
     }
```
#### <a id="CarryOutSQLCommand" href="#CarryOutSQLCommand">支持SQL命令</a> :star2: <a href="#top">:arrow_up:</a>
`支持操作查询比如Insert UPDATE Delete 我们需要使用DataBase 对象上的ExecuteSQLCommand或者ExecuteSQLCommandAsync 方法,和SqlQuery查询一样
ExecuteSQLCommand和ExecuteSQLCommandAsync 也有两个参数,执行操作查询统一由两种方式`
```C#
     public  int UpdateBUsers(BUser user)
     {

         int Exisit = BUsers.Where(u => u.BUserID == user.BUserID).Count();
         if (Exisit == 1)
         {
             string SQLCommand = @"Update dbo.BUser Set Name=@name,Age=@age,Sex=@sex,Password=@pwd where BUserID=@id";
             var parameters = new SqlParameter[]
             {
                 new SqlParameter(){ParameterName="@name",SqlDbType = SqlDbType.VarChar, Value = user.Name },
                 new SqlParameter(){ParameterName="@age",SqlDbType = SqlDbType.Int, Value = user.Age },
                 new SqlParameter(){ParameterName="@sex",SqlDbType = SqlDbType.Bit, Value = user.Sex },
                 new SqlParameter(){ParameterName="@pwd",SqlDbType = SqlDbType.VarChar, Value = user.Password },
                 new SqlParameter(){ParameterName="@id",SqlDbType = SqlDbType.Bit, Value = user.BUserID }
             };
             int result= this.Database.ExecuteSqlCommand(SQLCommand,parameters);
             return result;
         }
         //执行失败 没有更新对象
         return -404;
     }
```
#### <a id="DataSave" href="#DataSave">数据保存</a> :star2: <a href="#top">:arrow_up:</a>
`EF通过变更追踪来维护实体的更改状态,最终通过SaveChages,方法将其写入数据库中,通过EF框架 来完成数据的存储`
##### <a href="#AddDataToDataBase" id="AddDataToDataBase">`添加`</a>
`将数据添加到数据库中EF框架中有 Add和AddRange方法`
```C#
  [HttpPost]
  [ValidateAntiForgeryToken]
  public ActionResult Create([Bind(Include = "BUserID,Name,Age,Sex,Password")] BUser bUser)
  {
      if (ModelState.IsValid)
      {
          db.BUsers.Add(bUser);
          db.SaveChanges();
          return RedirectToAction("Index");
      }

      return View(bUser);
  }
```
##### <a href="#UpdateDataToDataBase" id="UpdateDataToDataBase">`更新`</a>
`不知道你平时对于更新是如何操作,更新是增删改查中最复杂的也是最容易出错的一种数据保存方式,因为在EF中没有对应的Update方法,或许你会立马想到不就是将实体状态修改为Modified,但是上下文追踪却有所问题,例如被标记追踪的实体已经被追踪那么会参数相同的键已存在异常`
* 使用EF查找修改
```C#
   using (var context = new BloggingContext())
   {
       var blog = context.Blogs.First();
       blog.Url = "http://sample.com/blog";
       context.SaveChanges();
   }
```
```C#
  public ActionResult Index()
  {
      var user=db.BUsers.FirstOrDefault(u => u.BUserID == 2);
      user.Age = 100;
      user.Sex = false;
      db.SaveChanges();
      return View(db.BUsers.ToList());
  }
```
