<a id="top">:checkered_flag:</a> C# 集合 :blue_heart:
------
`一门语言缺什么都不应该缺集合,没有集合就缺失了很多灵活性,集合允许元素的个数是动态的,List<T> 是最常用的与数组相等的集合类,其他的类型的集合还有,队列,栈,链表,字典和集还有一些特殊的集合,集合必然是泛型的,这个不用说`

  - [x] <a href="#CollectionInterface">`集合接口和类型`</a>
  
----
#### 命令空间
```C#
  using System.Collections.Generic; //泛型集合 都尽量有这个
  using System.Collections; //普通集合
```

#### <a id="CollectionInterface">1.1&nbsp;&nbsp;  `集合接口和类型` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>

* `把集合进行抽象就是接口,各个集合类型就是借口的实现类而已 集合基于` [`ICollection 接口`](https://msdn.microsoft.com/zh-cn/library/92t2ye13(v=vs.110).aspx)、 [`IList 接口`](https://msdn.microsoft.com/zh-cn/library/5y536ey6(v=vs.110).aspx)、[`IDictionary 接口`](https://msdn.microsoft.com/zh-cn/library/s4ys34ea(v=vs.110).aspx) `或它们对应的泛型集合。`

* ` IList 接口和 IDictionary 接口都派生自 ICollection 接口：因此，所有集合都直接或间接基于 ICollection 接口`

* ` 在基于 IList 接口（比如 Array、ArrayList 或 List<T>）或直接基于 ICollection 接口（比如 Queue、ConcurrentQueue<T>、Stack、ConcurrentStack<T> 或 LinkedList<T>）的集合里，每个元素都只有一个值。`

* `在基于 IDictionary 接口（比如 Hashtable 和 SortedList 类，Dictionary<TKey,TValue> 和SortedList<TKey,TValue> 泛型类）或 ConcurrentDictionary<TKey,TValue> 类的集合中，每个元素都有一个键和一个值。 KeyedCollection<TKey,TItem> 类是唯一的，因为它是值中嵌键的值的列表，因此，它的行为类似列表和字典。`
-----
|接口|说明|
|:----|:------|
|[`IEnumerable<T>`](https://msdn.microsoft.com/zh-cn/library/9eekhta0(v=vs.110).aspx) |`如果foreach语句用于集合,就需要IEnumrable接口,这个接口定义了方法GetEnumrator(),它返回一个实现IEnumrator接口的枚举`|
|[`ICollection<T>`](https://msdn.microsoft.com/zh-cn/library/92t2ye13(v=vs.110).aspx) |`ICollection<T>接口由泛型集合类实现,使用这个接口可以获得集合中的元素个数,CopyTo() 方法,Add,Remove,Clear`|
|`IList<T>`       |`IList<T>接口用于可通过为止访问其中的元素列表,这个接口定义了一索引器,可以在集合的置顶为止插入和删除某些项,Insert方法和RemoveAt,派生自 ICollection<T>接口`|
|`ISet<T>` `接口` |`ISet接口由集实现集允许合并不同的集,获得两个集的交集,检查两个集是否需重叠,派生自ICollection<T>`| 
|[`Dictionary<TKey,TValue>`](https://msdn.microsoft.com/zh-cn/library/xfhwa508(v=vs.110).aspx) `接口`|`键值对泛型集合类实现,可以通过这个接口访问所有的集合,或者索引查询,增删改查`.|
|[`ILookup<Tkey,TValue>`](https://msdn.microsoft.com/zh-cn/library/bb534291(v=vs.110).aspx) |`类似于Dictionary<TKey,TValue> 实现该接口的集合有键和值,且可以通过一个键包含多个值`|
|[`Comparer<T>`](https://msdn.microsoft.com/zh-cn/library/8ehhxeaf(v=vs.110).aspx) |`由比较器实现比较合适,通过Compare方法给集合中的元素排序`|
|[`IEqualityComparer<T>`]()|`接口IEqualityComparer<T> 由一个比较器实现,该比较器可以用于字典中的键,使用这个接口,可以对对象进行相等性排序`|

----- 
