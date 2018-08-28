<a id="top" href="#top">	:maple_leaf:Linq to Entity :blue_heart:</a> 
-----
`Linq to Entity语法和Linq to Object差不多,只有一些语法支持性的差别,LINQ to Object：继承IEnumerable或IEnumerable<T>接口，无须
使用LINQ提供程序或API。所有的操作都是在内存中进行的，继承IQueryable或IQueryable<T>接口，LINQ to Entities是Entity Framework的
一部分并且取代LINQ to SQL作为在数据库上使用LINQ的标准机制。` [`区分分析`](https://www.cnblogs.com/Tony100/p/8980282.html)
> `学习Entity 参考类为Queryable 参考接口IQueryable  `

- [x] :maple_leaf: <a href="#BasicSearch">`基础查询`</a> 
    - <a href="#BasicFunction">基本方法</a>
    
#### <a id="BasicSearch" href="#BasicSearch">基础查询</a> :star2: <a href="#top"> :arrow_up:</a>
`在C#中 Linq to Entity支持` `Find`,`First`,`FirstOrDefault`,`Single`,`SingleOrDefault`,**`但是Linq不支持` `Last`,`LastOrDefault`**
* 会产生错误
```C#
  public ActionResult Show()
  {
      BUser b = db.BUsers.Last<BUser>();
      return View(b);
  }
  System.NotSupportedException:“LINQ to Entities does not recognize the 
  method 'Blogs.Models.BUser Last[BUser]  
  (System.Linq.IQueryable`1[Blogs.Models.BUser])' method, and this method 
  cannot be translated into a store expression.”
```
#####  <a id="BasicFunction" >说明一下这些方法</a> <a href="#top">:arrow_up:</a>
* **`SingleOrDefault`**:`返回序列中满足指定条件的唯一元素；如果这类元素不存在，则返回默认值；如果有多个元素满足该条件，此方法将引发异常。`
* **`Single`**:`返回序列的唯一元素；如果该序列并非恰好包含一个元素，则会引发异常。`
* **`First<TSource>(IQueryable<TSource>)`**:`返回序列的第一个元素`
* **`FirstOrDefault<TSource>(IQueryable<TSource>)`**:`返回序列中的第一个元素；如果序列中不包含任何元素，则返回默认值。`
`常见的聚合函数 包括Count/LongCount Sum Min Max Average都能翻译成SQL聚合函数,例如将Count对应SQL Server的Count函数,LongCount对称Count_Big函数等等
若有重载提供条件,则翻译SQL语句时加上Where进行过滤.`
##### 量词方法支持下 <a href="#top">:arrow_up:</a>
`量词方法包括`,`Contains`,`Any`,`Exists/Not`,`Exists`,`等等查询方法,` **`EF不支持Contain对 实体 类型的查询`** **`注意是实体包含,可以检查属性包含`**
```C#
  public string LC()
  {
      BUser u = new BUser();
      u.BUserID = 1;
      u.Age = 24;
      u.Name = "蒋星";
      u.Password = "123456";
      bool isHava= db.BUsers.Contains<BUser>(u);
      return isHava.ToString();
  }
 System.NotSupportedException:“Unable to create a constant value of type 
 'Blogs.Models.BUser'. Only primitive types or enumeration types are sup
 ported in this context.”
```
* `但是支持属性包含 查找ID为1的用户`
```C#
  public string LC()
  {
      BUser u = new BUser();
      u.BUserID = 1;
      u.Age = 24;
      u.Name = "蒋星";
      u.Password = "123456";
      bool isHava = db.BUsers.Select(bu =>bu.BUserID).Contains<int>(u.BUserID);
      return isHava.ToString(); //返回为ture
  }
```
##### 类型转换 <a href="#top">:arrow_up:</a>
`EF框架不支持Cast 全局转换，因为一下子转换不安全,但是支持OfType函数,但是不支持原始类型的OfType，类似于OfType<String>()`
```C#
  public ActionResult Index()
  {
      return View(db.BUsers.OfType<BUser>().ToList());
  }
```
* `EF 支持cast的原始类型`
```C#
  db.BUsers
 .Select(u=>u.Name)
 .Cast<String>().ToList()
```
##### 分页问题 <a href="#top">:arrow_up:</a>
`SQL利用OFFSET..LIMIT 关键字进行分页更加简洁同时OFFSET..LIMIT是Order by子句的一部分,也就是说如果在使用OFFSET..LIMIT进行分页时候如果不进行ORDER BY 分页会报错,其分页对应的C#查询方法则是SKIP...TAKE 映射到实体 就是先试用Orderby 再使用Skip Take，不按照这个顺序就会出错`












































