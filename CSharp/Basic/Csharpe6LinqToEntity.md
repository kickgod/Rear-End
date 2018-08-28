<a id="top" href="#top">	:maple_leaf:Linq to Entity :blue_heart:</a> 
-----
`Linq to Entity语法和Linq to Object差不多,只有一些语法支持性的差别,LINQ to Object：继承IEnumerable或IEnumerable<T>接口，无须
使用LINQ提供程序或API。所有的操作都是在内存中进行的，继承IQueryable或IQueryable<T>接口，LINQ to Entities是Entity Framework的
一部分并且取代LINQ to SQL作为在数据库上使用LINQ的标准机制。` [`区分分析`](https://www.cnblogs.com/Tony100/p/8980282.html)
> `学习Entity 参考类为Queryable 参考接口IQueryable  `

- [x] :maple_leaf: <a href="#BasicSearch">`基础查询`</a> 

#### <a id="BasicSearch" href="#BasicSearch">基础查询</a> :star2: <a href="#top"> :arrow_up:</a>
`在C#中 Linq to Entity支持` `Find`,`First`,`FirstOrDefault`,`Single`,`SingleOrDefault`,**`但是Linq不支持` `Last`,`LastOrDefault`**
##### 说明一下这些方法
* **`SingleOrDefault`**:`返回序列中满足指定条件的唯一元素；如果这类元素不存在，则返回默认值；如果有多个元素满足该条件，此方法将引发异常。`
* **`Single`**:`返回序列的唯一元素；如果该序列并非恰好包含一个元素，则会引发异常。`
* **`First<TSource>(IQueryable<TSource>)`**:`返回序列的第一个元素`
* **`FirstOrDefault<TSource>(IQueryable<TSource>)`**:`返回序列中的第一个元素；如果序列中不包含任何元素，则返回默认值。`
`常见的聚合函数 包括Count/LongCount Sum Min Max Average都能翻译成SQL聚合函数,例如将Count对应SQL Server的Count函数,LongCount对称Count_Big函数等等
若有重载提供条件,则翻译SQL语句时加上Where进行过滤.`
##### 量词方法支持下
`量词方法包括`,`Contains`,`Any`,`Exists/Not`,`Exists`,`等等查询方法,` **`EF不支持Contain对实体类型的查询`**
