<a id="top">:checkered_flag:</a> C# 集合 :blue_heart:
------
`一门语言缺什么都不应该缺集合,没有集合就缺失了很多灵活性,集合允许元素的个数是动态的,List<T> 是最常用的与数组相等的集合类,其他的类型的集合还有,队列,栈,链表,字典和集还有一些特殊的集合,集合必然是泛型的,这个不用说`

 - [x] :whale2: <a href="#CollectionInterface">`集合接口和类型`</a>
  
 - [x] :whale2: <a href="#CollectionList">`列表`</a>
   * <a href="#list_T_">  `1. List<T>` </a>
   * <a href="#Quene_T_"> `2. Quene<T>`</a>
   * <a href="#Stack_T_"> `3. Stack<T>`</a>
   * <a href="https://msdn.microsoft.com/zh-cn/library/ahf4c754(v=vs.110).aspx"> `4. LinkedListNode<T>`</a>
   * <a href="#SortedList_TKey_TValue_"> `5. SortedList<TKey, TValue>`</a>
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
#### <a id="CollectionList">1.1&nbsp;&nbsp;  `列表` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`.NET Framwork 为控台列表提供了 泛型类List<T> 这个类声明如下`
##### 列表 :[`List<T>`](https://msdn.microsoft.com/zh-cn/library/6sh2ey19(v=vs.110).aspx) <a id="list_T_" ></a>
```C#
  [SerializableAttribute]
  public class List<T> : IList<T>, ICollection<T>, IEnumerable<T>, 
    IEnumerable, IList, ICollection, IReadOnlyList<T>, IReadOnlyCollection<T>
    
  //Predicate<T> 是一个委托
    public delegate bool Predicate<in T>(
        T obj
    )
```
* 常用属性
  * `Capacity`:	`获取或设置该内部数据结构在不调整大小的情况下能够容纳的元素总数`
  * `Count`:`获取 List<T> 中包含的元素数。`
  * `Item[Int32]`:`获取或设置指定索引处的元素`
```C#
    static void Main(string[] args)
    {
        List<int> intList=new List<int>(5);
        intList.AddRange(new int[]{1,2,3,4,5,6});

        Console.WriteLine("索引1位置元素:"+intList[1]);
        Console.WriteLine("元素数量:"+ intList.Count);
        Console.WriteLine("当前最大容量:"+intList.Capacity);            
    }
    /*
      索引1位置元素:2
      元素数量:6
      当前最大容量:10
     */
```
* 常用方法
   * 	`Add(T)`:	`将对象添加到 List<T> 的结尾处。`
   *  `AddRange(IEnumerable<T>)`:`将指定集合的元素添加到 List<T> 的末尾。`
   *  `BinarySearch(T)`:`使用默认的比较器在整个已排序的 List<T> 中搜索元素，并返回该元素从零开始的索引。`
   *  `BinarySearch(T, IComparer<T>)`:`使用指定的比较器在整个已排序的 List<T> 中搜索元素，并返回该元素从零开始的索引。`
   *  `BinarySearch(Int32, Int32, T, IComparer<T>)`:`使用指定的比较器在已排序 List<T> 的某个元素范围中搜索元素，并返回该元素从零开始的索引。`
   *  `Contains(T)`: `确定某元素是否在 List<T> 中。`
   *  `Exists(Predicate<T>)`:`确定 List<T> 是否包含与指定谓词定义的条件匹配的元素。`
   *  `Remove(T)`:`从 List<T> 中移除特定对象的第一个匹配项。`
   *  `RemoveAll(Predicate<T>)`:`移除与指定的谓词所定义的条件相匹配的所有元素。`    
   *  `RemoveAt(Int32)`:`移除 List<T> 的指定索引处的元素。`
   *  `RemoveRange(Int32, Int32)`:`从 List<T> 中移除一定范围的元素。`
   *  `Insert(Int32, T)`:`将元素插入 List<T> 的指定索引处。`
   *  `IndexOf(T)`:`搜索指定的对象，并返回整个 List<T> 中第一个匹配项的从零开始的索引。`
   *  `GetEnumerator()`:`返回循环访问 List<T> 的枚举数。`
   *  `ForEach(Action<T>)`:`对 List<T> 的每个元素执行指定操作。`
   *  `Equals(Object)`:`确定指定的对象是否等于当前对象。（继承自 Object。）`
   *  `Clear()`:`从 List<T> 中移除所有元素。`
   *  `Sort()`:`	使用默认比较器对整个 List<T> 中的元素进行排序。`
   *  `Sort(IComparer<T>)`:`使用指定的比较器对整个 List<T> 中的元素进行排序。`
   *  `Reverse()`:`将整个 List<T> 中元素的顺序反转。`
   *  `TrimExcess()`:`将容量设置为 List<T> 中元素的实际数目（如果该数目小于某个阈值）`
* 元素搜索
   *   `元素是否存在使用 Exists()方法`
   
   ```C#
   
      //Exists(Predicate<T>)	确定 List<T> 是否包含与指定谓词定义的条件匹配的元素。
        List<int> intList=new List<int>(5);
        intList.AddRange(new int[]{1,2,3,4,5,6});
        bool ishave=intList.Exists(val=>val>2);
        if(ishave){
             WriteLine("存在满足条件的元素");   
        }
        //输出:存在满足条件的元素  
   ```
   
   * `Find 方法` 
   
   ```C#
   
      //FindIndex(Predicate<T>)	搜索与指定谓词所定义的条件相匹配的元素，并返回整个 List<T> 中第一个匹配
      //FindLast(Predicate<T>)  搜索与指定谓词所定义的条件相匹配的元素，并返回整个 List<T> 中的最后一个匹配元素。
        元素的从零开始的索引。
        List<int> intList=new List<int>(5);
        intList.AddRange(new int[]{1,2,3,4,5,6});
        int first=intList.FindIndex(ValueTuple=> ValueTuple>=4);
        Console.WriteLine($"索引:{first} 元素:{intList[first]}");
        //索引:3 元素:4
   ```
   
   * `FindAll方法 返回满足条件的集合`
   <br/>
   
   ```C#
      /*
      public List<T> FindAll(
         Predicate<T> match
      )
      */
      
       List<int> intList=new List<int>(5);
       intList.AddRange(new int[]{1,2,3,4,5,6});

       List<int> vallist=intList.FindAll(ValueTuple=> ValueTuple>=4);
       foreach(var vals in vallist){
           Console.WriteLine($"元素:{vals}");    
       }
       
       //输出:
       元素:4
       元素:5
       元素:6
   ```
* 排序
 ```C#
      namespace DotnetConsole.classlib
      {
          public class Racer:IComparable<Racer>
          {
              public int  Id{get;}
              public string FirstNmae{get;set;}
              public string LastName{get;set;}
              public string Country{get;set;}
              public int Wins{get;set;}

              public string GetName=> $"{this.FirstNmae} {this.LastName}";

              public override string ToString(){
                  return $"{this.Id} Name:{this.FirstNmae} {this.LastName} Country:{Country} Wins Time:{Wins}";
              }

              public  int CompareTo(Racer obj)
              {
                  return this.Id.CompareTo(obj.Id);
              }

              public Racer(int id,string fname,string lname,string cou,int win){
                  this.Id=id;
                  this.FirstNmae=fname;
                  this.LastName=lname;
                  this.Country=cou;
                  this.Wins=win;
              }
          }
      }
 ```
 
 
 ```C#
       List<Racer> rs=new List<Racer>{
            new Racer(1,"J","X","China  ",20),
            new Racer(2,"C","L","America",3),
            new Racer(3,"G","X","Indian ",2),
            new Racer(4,"Z","B","China  ",5)
       };

       foreach(var owner in rs){
           Console.WriteLine($"选手: {owner.ToString()}");
       }
       // 按照赢的次数排序 
       rs.Sort((val1,val2)=>val2.Wins.CompareTo(val1.Wins));
       Console.WriteLine("排序后:---------------------------------------------");

       foreach(var owner in rs){
           Console.WriteLine($"选手: {owner.ToString()}");
       } 
       /*
         选手: 1 Name:J X Country:China   Wins Time:20
         选手: 2 Name:C L Country:America Wins Time:3
         选手: 3 Name:G X Country:Indian  Wins Time:2
         选手: 4 Name:Z B Country:China   Wins Time:5
         排序后:---------------------------------------------
         选手: 1 Name:J X Country:China   Wins Time:20
         选手: 4 Name:Z B Country:China   Wins Time:5
         选手: 2 Name:C L Country:America Wins Time:3
         选手: 3 Name:G X Country:Indian  Wins Time:2
       */
 ```
 ##### 列表 :[`Queue<T>`](https://msdn.microsoft.com/zh-cn/library/7977ey2c(v=vs.110).aspx) <a id="Quene_T_" ></a>
`队列`：`先进先出的队列`
<br/>
`备注:`:`此类作为循环数组实现泛型队列中。 对象存储在 Queue<T> 另一端插入和删除来自其他。 队列和堆栈是有用时需要临时存储信息;也就是说，您可能想要检索其值后放弃某个元素。 使用 Queue<T> 需要访问存储在集合中的相同顺序的信息。 使用 Stack<T> 如果您需要按相反的顺序访问的信息。 使用 ConcurrentQueue<T> 或 ConcurrentStack<T> 如果您需要同时从多个线程访问集合。`
 
 * 常用属性
   * `Count`:`队列中元素的个数`
 * 常见方法
   * `Clear()`：`清楚所有元素`
   * `Contains(T)`：`确定某元素是否在 Queue<T> 中。`
   * `Peek()`:`返回位于 Queue<T> 开始处的对象但不将其移除。`
   * `Enqueue(T)`:`将对象添加到 Queue<T> 的结尾处。`
   * `Dequeue()`:`移除并返回位于 Queue<T> 开始处的对象。`
   * `TrimExcess()`:`如果元素数小于当前容量的 90%，将容量设置为 Queue<T> 中的实际元素数。`
   * `CopyTo(T[], Int32)`：`从指定数组索引开始将 Queue<T> 元素复制到现有一维 Array 中。`
   
 ##### 列表 :[`Stack<T>`](https://msdn.microsoft.com/zh-cn/library/3278tedw(v=vs.110).aspx) <a id="Stack_T_" ></a>  
 `Stack<T> 不解释,重要的数据结构`
 
  * 常用属性
   * `Count`:`队列中元素的个数`
 * 常见方法
   * `Clear()`：`清楚所有元素`
   * `Contains(T)`：`确定某元素是否在 Queue<T> 中。`
   * `Peek()`:`返回的对象顶部的 Stack<T> 而不删除它。`
   * `Push(T)`:`推入栈中。`
   * `Pop()`:`移除并返回位于顶部的对象 Stack<T>。`
   * `TrimExcess()`:`如果元素数小于当前容量的 90%，将容量设置为 Queue<T> 中的实际元素数。`
   * `CopyTo(T[], Int32)`：`从指定数组索引开始将 Queue<T> 元素复制到现有一维 Array 中。`
 ##### 有序列表 [`SortedList<TKey, TValue>`](https://msdn.microsoft.com/zh-cn/library/ms132319(v=vs.110).aspx) <a href="#SortedList_TKey_TValue_"></a>
`这个类按照键给元素排序,这个集合的值和键都可以使用任何类型, 里面存储的是 KeyValuePair<TKey,KValue>`

```C#
    public class Racer:IComparable<Racer>
    {
        public int  Id{get;}
        public string FirstNmae{get;set;}
        public string LastName{get;set;}
        public string Country{get;set;}
        public int Wins{get;set;}

        public string GetName=> $"{this.FirstNmae} {this.LastName}";

        public override string ToString(){
            return $"ID: {this.Id} Name:{this.FirstNmae} {this.LastName} Country:{Country} Wins Time:{Wins}";
        }

        public  int CompareTo(Racer obj)
        {
            return this.Id.CompareTo(obj.Id);
        }

        public Racer(int id,string fname,string lname,string cou,int win){
            this.Id=id;
            this.FirstNmae=fname;
            this.LastName=lname;
            this.Country=cou;
            this.Wins=win;
        }
    }
    
       static void Main(string[] args)
        {
           //初始化集合
           List<Racer> rs=new List<Racer>{
                new Racer(1,"J","X","China  ",20),
                new Racer(2,"C","L","America",3),
                new Racer(3,"G","X","Indian ",2),
                new Racer(4,"Z","B","China  ",5)
           };

           SortedList<String,Racer> idnameset=new SortedList<String,Racer>();
           idnameset.Add("America",rs[1]);
           idnameset.Add("Indian ",rs[2]);
           idnameset.Add("China  ",rs[0]);

           foreach(KeyValuePair<String,Racer> val in idnameset){
               WriteLine($"{val.Key}  {val.Value.ToString()}");
           } 
        }
       /*
        America  2 Name:C L Country:America Wins Time:3
        China    1 Name:J X Country:China   Wins Time:20
        Indian   3 Name:G X Country:Indian  Wins Time:2
       */
```
