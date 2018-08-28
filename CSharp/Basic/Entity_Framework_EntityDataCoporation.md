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
