<a id="top" href="#top">	:maple_leaf: EntityFramework 实体状态和数据操作 :blue_heart:</a> 
----
`我们通过EntityFramework来说对数据进行操作并持久化到数据库,那么EF,要通过EF上下文泪来维护实体的状态,明确知道每一个状态对应的操作,也就是说
EF上下文来维护尸体的状态,操作数据库的方式也有很多变化`
- [x] :maple_leaf: <a href="#EntityState">实体状态</a>
- [x] :maple_leaf: <a href="#DataCorpration">数据操作</a>
   - <a href="#DataQuery">`查询数据`</a> 
    
#### <a id="EntityState" href="#EntityState">实体状态</a> :star2: <a href="#top">:arrow_up:</a>
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
#### <a id="DataQuery" href="#DataQuery">数据查询</a> :star2: <a href="#top">:arrow_up:</a>
        
        
        
        
