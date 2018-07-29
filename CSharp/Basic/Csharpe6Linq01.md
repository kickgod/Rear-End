<a id="top">:checkered_flag:</a> Linq 语言集成查询 :blue_heart:
---
`LINQ（Language Integrated Query）语言集成查询` 是一组用于c#和Visual Basic语言的扩展。它允许编写C#或者Visual Basic代码以操作内存数据的方式，查
询数据库。`对象查询语言`

- [x] :whale2: <a href="#">`初期准备`</a>
   * <a href="#LearingNeed">  `1. 知识需要` </a>
   * <a href="#DataPrepared"> `2. 程序数据准备`</a>

- [x] :whale2: <a href="#SampleExcetion">`Linq小例子-初体验`</a>

- [x] :whale2: <a href="#LinqIntroduce">`Linq 查询概述`</a>

- [x] :whale2: <a href="#LinqInstandered">`Linq 操作符`</a>
   * <a href="#tiaojianshaixuan" >`条件筛选`</a>
      
##### 学习Linq的需要: <a id="LearingNeed"></a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
* `C# 集合精通 非常的熟悉`
* `C# 类对象接口等等镜头 集合学习前面的需要全部`
* `SQL语句精通`
* `没有也可以学 只是学起来要痛苦一点`
##### 数据准备 <a id="DataPrepared"></a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
* `测试数据`:`赛车类`
```C#
    using System;
    namespace DotnetConsole.classlib
    {
        public class Car
        {
            public string Name{get;set;}

            public DateTime ProductTime{get;set;}

            public string CarType{get;set;}

            public Car(string carName,string type,DateTime productTime){
                this.Name=carName;
                this.CarType=type;
                this.ProductTime=ProductTime;
            }

            public override String ToString() {
                return $"Car Name:{Name} Type:{CarType} Product Date:{ProductTime.ToLongDateString()} ";
            }
        }
    }
```
* `测试数据`:`赛车手类`

```C#
    using System;
    using System.Text;

    namespace DotnetConsole.classlib
    {
        public class GCRacer
        {
            public int  Id{get;}
            public string FirstNmae{get;set;}
            public string LastName{get;set;}
            public string Country{get;set;}
            public int Wins{get;set;}

            public Car UseCar;
            public string GetName=> $"{this.FirstNmae} {this.LastName}";

            public override string ToString(){
                return $"ID: {this.Id} Name:{this.FirstNmae} {this.LastName} Country:{Country} 
                Wins Time:{Wins}";
            }
            public  int CompareTo(Racer obj)
            {
                return this.Id.CompareTo(obj.Id);
            }
            public GCRacer(int id,string fname,string lname,string cou,int win,Car car){
                this.Id=id;
                this.FirstNmae=fname;
                this.LastName=lname;
                this.Country=cou;
                this.Wins=win;
                this.UseCar=car;
            }
        }
    }
```
* `主函数 数据`
```C#
          static void Main(string[] args)
        {
           //初始化集合
           List<Car> Carlist=new List<Car>(){
               new Car("DongFen-ZXC","G",Convert.ToDateTime("2011/5/21")),
               new Car("KuangFen","T",Convert.ToDateTime("2009/4/6")),
               new Car("TInay","Q",Convert.ToDateTime("2013/10/8")),
               new Car("Wudi","G",Convert.ToDateTime("2011/5/19")),
               new Car("Ati","Z",Convert.ToDateTime("2006/9/6")),
               new Car("TianYi","T",Convert.ToDateTime("2007/12/6")),
               new Car("DIlong","Z",Convert.ToDateTime("2013/1/1"))       
           };

           List<GCRacer> rs=new List<GCRacer>{
                new GCRacer(1,"J","X","China  ",20,Carlist[1]),
                new GCRacer(2,"C","L","America",3,Carlist[2]),
                new GCRacer(3,"G","X","Indian ",2,Carlist[3]),
                new GCRacer(4,"Z","B","China  ",5,Carlist[4]),
                new GCRacer(5,"J","T","China  ",10,Carlist[5]),
                new GCRacer(6,"X","X","China  ",20,Carlist[6]),
                new GCRacer(7,"D","L","America",3,Carlist[0]),
                new GCRacer(8,"G","F","Indian ",2,Carlist[1]),
                new GCRacer(9,"Q","B","China  ",5,Carlist[2]),
                new GCRacer(10,"N","T","China  ",10,Carlist[3])                
           };

        }   
```
#### <a id="SampleExcetion">2.1&nbsp;&nbsp;  `Linq小例子-初体验` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>

*  查询所有及格了的分数排序 求得及格学生的平均分
```C#
    float[] ClassGrades=new float[]{69.5F,99.5F,75.8F,95.6F,85.5F,77F,86F,59F,52F,85.2F,76.5F};
    // 查询所有及格了的分数排序 求得及格学生的平均分
    var query =from val in ClassGrades
               where val>=60 
               orderby val descending   //降序排列
               select val;
    foreach(var grade in query){
        WriteLine($"及格成绩:{grade}");
    }
    WriteLine($"及格学生平均成绩:{query.Average()}");
    /*输出:
        及格成绩:99.5
        及格成绩:95.6
        及格成绩:86
        及格成绩:85.5
        及格成绩:85.2
        及格成绩:77
        及格成绩:76.5
        及格成绩:75.8
        及格成绩:69.5
        及格学生平均成绩:83.4    
    */
```
* 查询中国的赛车手 名称,编号,赢的场次  
```C#
     //查询中国的赛车手 名称,编号,赢的场次  生成新类型 注意空格
     var query=from user in rs
                    where user.Country=="China  "
                    select new {
                        user.GetName,
                        user.Id,
                        user.Wins,
                        user.Country
                    };
      foreach(var user in query){
          WriteLine($"Country{user.Country} Id: {user.Id}  Name:{user.GetName} Win Times:{user.Wins}");
      }
      /* 输出:
          Id: 1  Name:J X Win Times:20
          Id: 4  Name:Z B Win Times:5
          Id: 5  Name:J T Win Times:10
          Id: 6  Name:X X Win Times:20
          Id: 9  Name:Q B Win Times:5
          Id: 10  Name:N T Win Times:10
       */
```
#### <a id="LinqIntroduce">3.1&nbsp;&nbsp;  `Linq 查询概述` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`Linq是一门对象查询语言,子句from，where,select,orderby,descending... 都是Linq查询预定义的关键字`

* **`重要`**: `Linq查询 必须以from子句开头,以select或group结束,在这两个子句中间可以使用where,orderby,join,let和from子句`

* **`重要`**:`query只指定了Linq查询,该查询不是通过这个赋值语句执行的,只是在使用foreach迭代访问查询时,该查询才执行`

* `编译器会转换Linq查询,以调用方法而不是Linq查询,Linq查询为IEnumerable<T>接口提供了各种扩展方法,以便用户在实现了该接口的任意集合上使用Linq查询,扩展方法再静态类中声明。其中第一个参数定义了它扩展的类型,定义LINQ扩展方法的一个类是System.Linq命名空间中的Enumerable.只需要导入这个名称空间`

```C#
  var query_=ClassGrades.Where(val=>val>=60).OrderByDescending(ValueTuple=>ValueTuple).Select(val=>val);

  foreach(var grade in query_){
      WriteLine($"及格成绩:{grade}");
  }
  WriteLine($"及格学生平均成绩:{ClassGrades.Where(val=>val>=60).OrderByDescending(ValueTuple=>ValueTuple).Select(val=>val).Average()}");
   
  /*输出:
        及格成绩:99.5
        及格成绩:95.6
        及格成绩:86
        及格成绩:85.5
        及格成绩:85.2
        及格成绩:77
        及格成绩:76.5
        及格成绩:75.8
        及格成绩:69.5
        及格学生平均成绩:83.4    
    */
```
```C#
           var chinaRacer=rs.Where(user=>user.Country=="China  ").Select(user=>new {
                user.GetName,
                user.Id,
                user.Wins,
                user.Country
           });
            foreach(var user in chinaRacer){
                WriteLine($"Country{user.Country} Id: {user.Id}  Name:{user.GetName} Win Times:{user.Wins}");
            }
      /*
        CountryChina   Id: 1  Name:J X Win Times:20
        CountryChina   Id: 4  Name:Z B Win Times:5
        CountryChina   Id: 5  Name:J T Win Times:10
        CountryChina   Id: 6  Name:X X Win Times:20
        CountryChina   Id: 9  Name:Q B Win Times:5
        CountryChina   Id: 10  Name:N T Win Times:10      
       */      
            
```      
##### 得到查询结果
如果查询结果多次用到,那么没使用一次就进行一次查询,那么当数据量大的时候就太吃亏了,``


#### <a id="LinqInstandered">3.1&nbsp;&nbsp;  `Linq 操作符` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>

##### <a id="tiaojianshaixuan" >条件筛选</a>

|`标准查询操作符`|`说明`|
|:----|:------|
|Where|基于元素的谓词查找,例如lambda表达式定义的谓词,来返回布尔值|
|OfType<TResult> |根据类型筛选元素,只返回TResult类型的元素|
  
  

* 操作符说明
```C#
    Where<TSource>(Func<TSource, Boolean>)  // 基于谓词筛选值序列
    Where<TSource>(Func<TSource, Int32, Boolean>) //将在谓词函数的逻辑中使用每个元素的索引。可以基于索引查找
    OfType<TResult>()  //筛选的元素 IEnumerable 根据指定的类型。
```
* 基于谓词对象的属性筛选

```C#
    //筛选赢过五次及其以上的中国选手
    var racers=from val in rs
               where val.Wins>=5 && val.Country.Trim()=="China"
               select val;
    foreach(var r in racers){
        WriteLine(r.ToString());    
    }       
    
    foreach(var r in rs.Where(val=>val.Wins>=5&&val.Country.Trim()=="China").Select(val=>val)){
        WriteLine(r.ToString());    
    }      
```

* 基于索引的查找
```C#
    // 查找索引为奇数的选手 Where 重载的Func方法中 Func<TSource, Int32, Boolean> 第二个参数可以传递索引
    foreach(var r in rs.Where((val,index)=>index%2==0).Select(val=>val)){
        WriteLine(r.ToString());    
    }   
```
* 基于类型筛选

```C#
    //筛选整形
    object[] data={"one",1,2,50,69.32F,new Car("TInay","Q",Convert.ToDateTime("2013/10/8"))};
    foreach(var val in data.OfType<int>()){
        Console.WriteLine(val);
    }
```
##### <a id="tiaojianshaixuan" >条件筛选</a>

|`标准查询操作符`|`说明`|
|:----|:------|
|Select     |投射操作符用于把对象转换为新的对象 |
|SelectMany |投射操作符用于把对象转换为新的对象,定义了根据选择函数选择结果值的映射                 |

```C#
  	SelectMany<TSource, TResult>(Func<TSource, IEnumerable<TResult>>)
```
