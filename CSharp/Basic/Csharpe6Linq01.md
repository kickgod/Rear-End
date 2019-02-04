<a id="top">:checkered_flag:</a> Linq 语言集成查询 :blue_heart:
---
`LINQ（Language Integrated Query）语言集成查询` 是一组用于c#和Visual Basic语言的扩展。它允许编写C#或者Visual Basic代码以操作内存数据的方式，查
询数据库。`对象查询语言`
 
- [x] :whale2: <a href="#LearingNeed">`初期准备`</a>
   * <a href="#LearingNeed">  `1. 知识需要` </a>
   * <a href="#DataPrepared"> `2. 程序数据准备`</a>
- [x] :whale2: <a href="#SampleExcetion">`Linq小例子-初体验`</a>
- [x] :whale2: <a href="#LinqIntroduce">`Linq 查询概述`</a>
- [x] :whale2: <a href="#LinqInstandered">`Linq 操作符`</a>
   * <a href="#tiaojianshaixuan" >`条件筛选 where`</a>
   * <a href="#fromSeletMany" >`复合 from`</a>
   * <a href="#OrderbyWhat">`排序 orderby`</a> 
   * <a href="#GroupFenZu">`分组 Group`</a>
   * <a href="#QianTaoGroupFenZu">`嵌套 linq`</a>
   * <a href="#letLinqVariable">`let 定义Linq中的变量`</a>
   * <a href="#InnerConnection" >`内连接 join`</a> 
   * <a href="#LeftOutterConnection" >`左外链接 join DefaultIfEmpty `</a> 
   * <a href="#GroupConnection">`组连接 join new`</a>
   * <a href="#SetOperation">`集合操作 Union,Intersect,Except..`</a>
   * <a href="#elementXulie">`限定符 Any,All,Contains`</a>
   * <a href="#PartitionOperation" >`分区 Take() Skip()`</a> 
   * <a href="#polymerizationFunction" >`聚合Count,Sum,Min,Max,Average,Aggregate`</a> 
   * <a href="#ChangeToCollection" >`转换ToList(),ToLookup...`</a> 
   * <a href="#GetRange" >`生成操作符Range,`</a> 
- [x] :whale2: <a href="#AsynAwaitLinq">`并行 Linq`</a>
- [x] :whale2: <a href="#CancelSearch" >`取消查询..`</a> 



`System.Linq 名称空间中包含的类ParallelEnumerable分解查询的工作,使其分布在多个线程上,尽管Enumerable类给IEnumerable<T> 定义了扩展方法,但ParallelEnumerable类大多数的扩展方法是ParallelQuery<TSource>类的扩展`

##### 学习Linq的需要: <a id="LearingNeed"></a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
* `C# 集合精通 非常的熟悉`
* `C# 类对象接口等等扩展方法,匿名类型,泛型,lambda表达式 集合学习前面的需要全部`
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
* `学生类`
```C#
using System;
using System.Text;

namespace DotnetConsole
{
   public class Person{
      public Person(String Name,int Age,String Id,Boolean Sex){
          this.UserName=Name;
          this.Age=Age;
          this.Sex=Sex;
          this.UserId=Id;
      }

	public override int GetHashCode() {
		int prime = 31;
		int result = 1;
		result = prime * result + Age;
		result = prime * result + Sex.GetHashCode();
		result = prime * result + ((UserId == null) ? 0 : UserId.GetHashCode());
		result = prime * result + ((UserName == null) ? 0 : UserName.GetHashCode());
		return result;
	}

      public String UserName{get;set;}
    
      public int Age{get;set;}

      public String UserId{get;set;}

      public Boolean Sex{get;set;}=true;
      
      public override String ToString(){
         return $"Name: {this.UserName} Age:{this.Age} 用户ID:{this.UserId} ";
      }

      public virtual int Wang(int a){
          return a*a;
      }

      public override Boolean Equals(object obj){
          Person one=obj as Person;
          if(one==null){
              return false;
          }else{
                if(this.UserName==one.UserName&&
                this.Age==one.Age&&
                this.Sex==one.Sex&&
                this.UserId==one.UserId){
                    return true;
                }else{
                    return false;
                }
          }

      }
  }

}
// 继承父类
using System;
using System.Text;
using System.Collections;
using System.Collections.Generic;

namespace DotnetConsole
{
   class Student:Person{
      public Student(String Name,int Age,String Id,Boolean Sex,int[] Grades)
        :base(Name,Age, Id,Sex){
          Grade=new List<int>(Grades);
      }
      public List<int> Grade;

      public override Boolean Equals(object obj){
          Student one=obj as Student;
          if(one==null){
              return false;
          }
          for(int i=0;i<one.Grade.Count;i++){
            if(this.Grade[i]!=one.Grade[i]){
              return false;
            }
          }
          return base.Equals(obj);
      }

      	public override int GetHashCode() {
          return base.GetHashCode();
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
           
           List<Student> stu= new List<Student>(){
                new Student("Wanglimi",15,"2014001",false,new int[]{61,60,90,75,80,72}),
                new Student("shanghai",14,"2014002",true,new int[]{99,87,90,85,97,82}),
                new Student("tianguan",16,"2014003",false,new int[]{61,60,90,75,83,92}),
                new Student("Yangliwe",15,"2014004",false,new int[]{88,93,95,95,90,62}),
                new Student("Caoniman",15,"2014005",true,new int[]{71,80,70,75,82,75}),
                new Student("Jiangfen",15,"2014006",true,new int[]{60,62,50,55,42,75})
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
  WriteLine($"及格学生平均成绩:{ClassGrades.Where(val=>val>=60).OrderByDescending(ValueTuple=>ValueTuple)
                                          .Select(val=>val).Average()}");
   
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
`如果查询结果多次用到,那么没使用一次就进行一次查询,那么当数据量大的时候就太吃亏了,于是我们尝试一次查询,多次使用,将第一次查询的结果固定下来`
* `ToArray()`:`执行查询保存查询结果为一个数组`
* `ToList()`:`执行查询保存查询结果为一个列表`
* ....等等方法

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
##### <a id="fromSeletMany" >`复合from`</a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>

|`标准查询操作符`|`说明`| 
|:----|:------|
|Select     |投射操作符用于把对象转换为新的对象 |
|SelectMany |投射操作符用于把对象转换为新的对象,定义了根据选择函数选择结果值的映射                 |

`适合类型,一个对象的一个对象是一个数组或者一个集合,例如一个教师有几十个学生,在一群教师中寻找教师 这个教师所带学生有人得到过省级以上的奖项的,或者
查询一个学生,这个学生期末成绩六科,查找学生中科目中又不及格的学生`

```C#
  SelectMany<TSource, TResult>(Func<TSource, IEnumerable<TResult>>)
  
  SelectMany<TSource, TResult>(Func<TSource, Int32, IEnumerable<TResult>>)
  
  SelectMany<TSource, TCollection, TResult>(Func<TSource, IEnumerable<TCollection>>,
  Func<TSource, TCollection, TResult>)
  
  SelectMany<TSource, TCollection, TResult>(Func<TSource, Int32, IEnumerable<TCollection>>,
  Func<TSource, TCollection, TResult>)
```
* 查找成绩不及格的学生
```C#
  var query=stus.SelectMany(val=>val.Grade,(val,grade)=>new { Student_=val,Grades_=grade })
            .Where(no=>no.Grades_<60)
            .Select(val=>val.Student_);                    
  foreach(var val in query){
      Console.WriteLine(val.ToString());
  }
  /*
    Name: Jiangfen Age:15 用户ID:2014006
    Name: Jiangfen Age:15 用户ID:2014006
    Name: Jiangfen Age:15 用户ID:2014006
    Name: Qitianji Age:15 用户ID:2014007
    Name: Qitianji Age:15 用户ID:2014007
  */
```
`可见使用selectMany可以达到这个效果,可是此不想sql一样是子查询所以会连带多余数据,比如一个学生有多门不及格的时候,就会把这个学生重复几次,谓词我们需要
消除重复数据,使用distinct 方法`**`但是:集合元素必须实现Equals 和GetHashCode 方法`**

##### <a id="OrderbyWhat" href="https://docs.microsoft.com/zh-cn/dotnet/framework/data/adonet/ef/language-reference/query-expression-syntax-examples-ordering" >`排序`</a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>

|`标准查询操作符`|`说明`| 
|:----|:------|
|OrderBy     |按根据某个键按升序对序列的元素进行排序 |
|OrderByDescending |降序排列              |
|ThenBy |二次排序              |
|ThenByDescending|二次排序降序|
|Reverse|逆序排序|

* 方法声明
```C#
    OrderBy<TSource, TKey>(Func<TSource, TKey>);
    //按根据某个键按升序对序列的元素进行排序。
    OrderBy<TSource, TKey>(Func<TSource, TKey>, IComparer<TKey>);
    //按使用指定的比较器按升序对序列的元素进行排序
    OrderBy<TSource, TKey>(Func<TSource, TKey>, IComparer<TKey>)
    //按使用指定的比较器按升序对序列的元素进行排序
    OrderByDescending<TSource, TKey>(Func<TSource, TKey>)
    //按根据某个键按降序对序列的元素进行排序
    OrderByDescending<TSource, TKey>(Func<TSource, TKey>, IComparer<TKey>)
    //使用指定的比较器按降序对序列的元素排序。
    Reverse<TSource>()
    //反转序列中元素的顺序。
    ThenBy(Func<TSource, TKey>)
    //二次排序

```
```C#

    var query=stus.OrderByDescending(val=>val.Age).ThenByDescending(val=>val.UserId).Select(val=>val); 
    foreach(var val in query.Distinct()){
        Console.WriteLine(val.ToString());
    }
    //输出
    /*
      Name: Yangliwe Age:18 用户ID:2014004
      Name: Caoniman Age:17 用户ID:2014005
      Name: tianguan Age:16 用户ID:2014003
      Name: Qitianji Age:15 用户ID:2014007
      Name: Wanglimi Age:15 用户ID:2014001
      Name: shanghai Age:14 用户ID:2014002
      Name: Jiangfen Age:13 用户ID:2014006
    */
```
##### <a id="GroupFenZu" href="https://docs.microsoft.com/zh-cn/dotnet/framework/data/adonet/ef/language-reference/query-expression-syntax-examples-grouping" >`分组 Group`</a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`如果要根据一个关键字分组,可以使用Group子句:` `Group 对象 by 对象.属性 into GroupName` `然后就可以对GroupName使用聚合函数`

|`标准查询操作符`|`说明`| 
|:----|:------|
|GroupBy |组合操作符, GroupBy组合有公共键的元素|
|ToLookup|ToLookup通过创建一个一对多的字典来组合元素|

```C#
 var query=from val in stus
	  group val by val.Sex into SexGroup
	  orderby SexGroup.Count() descending,SexGroup.Key
	  where SexGroup.Count() >0
	  select new {
	       SexName=SexGroup.Key?"男":"女",
	       Count=SexGroup.Count() 
	  };
 /* 通过扩展方法
  var query=stus.GroupBy(val=>val.Sex).Where(g=>g.Count()>0).Select(g=>new {
      SexName=g.Key?"男":"女",
      Count=g.Count()
  })        
 */   
 
 
 /* 通过 ToLookup
   var query=stus.ToLookup(val=>val.Sex).Where(g=>g.Count()>0).Select(g=>new {
      SexName=g.Key?"男":"女",
      Count=g.Count()
   });
   foreach(var val in query.Distinct()){
   	Console.WriteLine( $"类别: {val.SexName} 数量:{val.Count}" );
   }
 */

 foreach(var val in query.Distinct()){
    Console.WriteLine( $"类别: {val.SexName} 数量:{val.Count}" );
 }
 /*
  类别: 男 数量:4
  类别: 女 数量:3 */
```
##### <a id="QianTaoGroupFenZu" >`嵌套 linq`</a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`嵌套分组`
```C#
    var query=from val in stus 
	      where val.Age>13
	      select new {
		  Name=val.UserName,
		  Jige=from g in  val.Grade
		       group g by g>60 into Ggrade
		       select new {
			   jige_mark=Ggrade.Key,
			   jige_count=Ggrade.Count()
		       }     
	      };
    foreach(var val in query.Distinct()){
	Console.WriteLine( $"姓名: {val.Name}" );
	foreach(var jige in val.Jige){
	     var status=jige.jige_mark?"及格":"不及格";
	     Console.WriteLine($"{status}：{jige.jige_count} ");   
	}
    }
```



##### <a id="letLinqVariable" >`let 定义Linq中的变量`</a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`在Linq表达式中可以使用let定义变量,这样可以多次引用,在扩展方法中可以使用select(new { //etc }) 创建匿名类型保存变量.`

```C#
    var query=from val in stus 
	      group val by val.Age into g
	      let Count=g.Count()
	      orderby g.Count() descending
	      select new {
		  Stu_Age=g.Key,
		  Stu_Count=Count
	      };

    foreach(var val in query.Distinct()){
	Console.WriteLine( $"类别: {val.Stu_Age} 数量:{val.Stu_Count}" );
    }
```
##### <a id="InnerConnection" >`内连接`</a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`使用join子句,可以根据特定的条件合并两个数据源,但之前要获得两个要连接的列表`
* **`注意`**:`join ...on  .. equals ...  中间是用equals 用作链接 而不是 等号 =`

|`标准查询操作符`|`说明`| 
|:----|:------|
|Join |链接操作符,按照属性进行链接,根据键选择器函数链接两个集合|
|GroupJoin|可以链接两个集合,组合其结果|

```C#
    List<Country> countrys= new List<Country>(){
	new Country("China","A-C12QX12456",1989),
	new Country("America","V-Q99QX52457",1959),
	new Country("Indian","I-B72QQ78239",1971),
	new Country("Japan","V-E19CF82357",1961),
	new Country("Intali","W-C16DF77786",1972),
	new Country("Canadna","O-G62QX36445",1973),
	new Country("English","A-N42XX15798",1982),
    };

    var reuslt=(from racer in 
		  from r in rs
		  where r.Wins>=3
		  select new{
		      racer_Name=r.GetName,
		      racer_Country=r.Country.Trim(),
		      Racer_Id=r.Id
		  }
		 join coun in                             
		    from c in countrys
		    select new {
			Country_name=c.CountryName.Trim(),
			Country_start_year=c.CountryStartTime,
			Country_id=c.CountryId
		    }
		on racer.racer_Country equals coun.Country_name   //注意是 equals
		orderby racer.Racer_Id descending
		select new {
		      Id=racer.Racer_Id,
		      Name=racer.racer_Name,
		      Country=racer.racer_Country,
		      CountryId=coun.Country_id,
		      CountryStartYear=coun.Country_start_year

		}).ToList();    
    foreach(var val in reuslt){
	 WriteLine($"选手编号:{val.Id} 名称:{val.Name} 国家编号:{val.CountryId} 所属国家 {val.Country} 
	 加入运动时间:{val.CountryStartYear}");
    }  
    /*
      选手编号:10 名称:N T 国家编号:A-C12QX12456 所属国家 China 加入运动时间:1989
      选手编号:9 名称:Q B 国家编号:A-C12QX12456 所属国家 China 加入运动时间:1989
      选手编号:7 名称:D L 国家编号:V-Q99QX52457 所属国家 America 加入运动时间:1959
      选手编号:6 名称:X X 国家编号:A-C12QX12456 所属国家 China 加入运动时间:1989
      选手编号:5 名称:J T 国家编号:A-C12QX12456 所属国家 China 加入运动时间:1989
      选手编号:4 名称:Z B 国家编号:A-C12QX12456 所属国家 China 加入运动时间:1989
      选手编号:2 名称:C L 国家编号:V-Q99QX52457 所属国家 America 加入运动时间:1959
      选手编号:1 名称:J X 国家编号:A-C12QX12456 所属国家 China 加入运动时间:1989
    */    
    
```
##### <a id="LeftOutterConnection" >`左外链接 join DefaultIfEmpty `</a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
* 关键是搞懂一点:`on r.Country equals t.CountryName into` **`rt`** `这个rt是什么 它是一个判断值,判断右边是否有与左边相匹配的,而tt实质上
不是t 而是 t?null 如果t不为null那么 tt=t否则 `
```C#
    // 如果有一个选手的国家没有收录呢? 这才是问题关键 我们假设没有收录小印度
    List<Country> countrys= new List<Country>(){
	new Country("China","A-C12QX12456",1989),
	new Country("America","V-Q99QX52457",1959),
	new Country("Japan","V-E19CF82357",1961),
	new Country("Intali","W-C16DF77786",1972),
	new Country("Canadna","O-G62QX36445",1973),
	new Country("English","A-N42XX15798",1982),
    };
    //实现左链接

    var reuslt=(from r in rs
	       join t in                           
	       countrys   
	       on r.Country equals t.CountryName into rt
	       from tt in rt.DefaultIfEmpty()     //tt 指代的是新的对象 如果Country 存在 tt就是 t否则 tt==null 
	       orderby r.Id                       //如果tt=null不处理 不去判断数据是否为null  或报错
	       select new{			  // 此时 tt实质上为 tt==t??null;
		    Id=r.Id,
		    Name=r.GetName,
		    CName=r.Country,
		    CId= tt ==null ?"暂未收录此国家信息":tt.CountryId,
		    CJoinYear=tt ==null ?"0000":tt.CountryStartTime.ToString()    
	       }).ToList();

    foreach(var val in reuslt){
	WriteLine($"选手编号:{val.Id} 名称:{val.Name} 国家编号:{val.CId} 所属国家 
	{val.CName} 加入运动时间:{val.CJoinYear}");
    }   
```

##### <a id="GroupConnection" >`组连接 join new`</a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`例如上面的左链接和内连接  一般 都是一个属性对一个属性,如果当需要两个属性都相等呢,比如外国人的 firstname和 lastname，这当然可以如下面所示`
`当然也有其他方法例如两个字符串链接起来`

* `组联接等效于左外部联接，它返回第一个（左侧）数据源的每个元素（即使其他数据源中没有关联元素）。`

```C#
  from v in foregines1  
     join r in users on
     new {
         FirstName=v.FirstName,
	 LastName=v.LastName
     }
     equals
     new
     {
         FirstName=r.FirstName,
	 LastName=r.LastName
     }
     into resulttemps  //结果会放到 resulttemps变量中
     
```
##### <a id="SetOperation" >`集合操作`</a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>


|`标准查询操作符`|`说明`| 
|:----|:------|
|Distinct() |返回序列中通过使用指定的非重复元素,去掉重复元素 只需要一个集合就可以操作|
|Union()|可以链接两个集合,组合其结果|
|Intersect()|返回两个集合都有的元素|
|Except()|返回只出现在其中一个集合中的元素|
|Zip()|把两个集合合并为一个|

```C#
  Distinct<TSource>()
  //通过使用的默认相等比较器对值进行比较从序列返回非重复元素
  Distinct<TSource>(IEqualityComparer<TSource>)
  //返回序列中通过使用指定的非重复元素
  Union<TSource>(IEnumerable<TSource>)
  // 通过使用默认的相等比较器生成的两个序列的并集。
  Union<TSource>(IEnumerable<TSource>, IEqualityComparer<TSource>)
  // 使用指定的生成两个序列的并集
```
* Union :`将两个集合组合在一起,去掉重复元素`
```C#
    List<Student> stu_Union= new List<Student>(){
	new Student("tianguan",16,"2014003",false,new int[]{61,60,90,75,83,92}),
	new Student("Tianwang",18,"2014008",false,new int[]{88,93,95,95,90,62})
    };

    var reuslt=stu_Union.Union(stus).OrderBy(vals=>vals.UserName);

    foreach(var val in reuslt){
	Console.WriteLine(val.ToString());
    }   
/* 输出: 
   Name: Caoniman Age:17 用户ID:2014005
   Name: Jiangfen Age:13 用户ID:2014006
   Name: Qitianji Age:15 用户ID:2014007
   Name: shanghai Age:14 用户ID:2014002
   Name: tianguan Age:16 用户ID:2014003
   Name: Tianwang Age:18 用户ID:2014008
   Name: Wanglimi Age:15 用户ID:2014001
   Name: Yangliwe Age:18 用户ID:2014004
*/    
```
* Intersect():`返回两个集合公有的元素组合成一个集合`
```C#
    List<Student> stu_Interset= new List<Student>(){
	new Student("tianguan",16,"2014003",false,new int[]{61,60,90,75,83,92}),
	new Student("Yangliwe",18,"2014004",false,new int[]{88,93,95,95,90,62}),
	new Student("DiYuWlin",17,"2014014",false,new int[]{88,93,95,95,90,72})
    };
    var reuslt=stus.Intersect(stu_Interset).OrderBy(vals=>vals.UserName);

    foreach(var val in reuslt){
	Console.WriteLine(val.ToString());
    }   
```
##### <a id="elementXulie" >`Any,All,Contains`</a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`如果元素序列满足指定的条件,限定符操作符就返回布尔值`

|`标准查询操作符`|`说明`| 
|:----|:------|
|Any(Func)|确定集合中是否有满足谓词函数的元素 |
|All(Func)|确定所有集合是否满足条件|
|Contains|检查某个集合是否在集合中|

```C#
   Any<TSource>(Func<TSource, Boolean>)
   //确定是否序列中的任何元素都满足条件。
   Any<TSource>()
   //确定序列是否包含任何元素。
   All<TSource>(Func<TSource, Boolean>)
   //确定是否对序列中的所有元素都满足条件。
   Contains<TSource>(TSource)   
   //确定序列是否包含指定的元素使用的默认相等比较器。
```
```C#
    int[] vals={58,60,90,55,96,73,56,84,95,100};
    Boolean has=vals.Any(val=>val>90);
    Boolean allhas=vals.All(val=>val>=60);
    Boolean has100=vals.Contains(100);
    WriteLine($"是否有大于90分的成绩: {has}");
    WriteLine($"是否所有科目都及格了: {allhas}");
    WriteLine($"是否有一百分的科目啊: {has100}");
    /* 输出:
      是否有大于90分的成绩: True
      是否所有科目都及格了: False
      是否有一百分的科目啊: True        
    */
```
#####  <a id="PartitionOperation" >`分区`</a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`扩展方法Skip和Take等的分区操作可用于分页,例如在第一个页面只显示及格选项`

|`标准查询操作符`|`说明`| 
|:----|:------|
|Take|分区操作,必须指定要从集合中提取的元素的个数 |
|Skip|指定跳过元素的的个数|
|TakeWhile|提取条件为真的元素|
|SkipWhile|跳过条件为真的元素|

* 给国家列表进行分页 
* `Skip(page_index*page_size).Take(page_size) 就可以调到第index页去  index从0开始`

```C#
    List<Country> countrys= new List<Country>(){
	new Country("China","A-C12QX12456",1989),
	new Country("America","V-Q99QX52457",1959),
	new Country("Japan","V-E19CF82357",1961),
	new Country("Indian","V-E19CF82357",1961),
	new Country("Intali","W-C16DF77786",1972),

	new Country("Canadna","O-G62QX36445",1973),
	new Country("English","A-N42XX15798",1982),
	new Country("Germany","Q-Q42XX58198",1981),
	new Country("Norway","C-Q42XX58198",1980),
	new Country("Vietnam","K-K41XX58198",1980),

	new Country("Myanmar","L-K46XX58198",1980),
	new Country("Thailand","F-G42XX58198",1980),
	new Country("Laos","W-C42XX58198",1980)
    };

    int PageSize=5;
    for(int page=0;page<=countrys.Count/PageSize;page++){
		
	var con=countrys.Skip(page*PageSize).Take(PageSize);    
	WriteLine($"页号:{page+1}----------------------------------------------------------");
	int id=0;
	foreach (var item in con)
	{
	    WriteLine($"序号 {id++} 加入时间:{item.CountryStartTime} 编号{item.CountryId} 
	    国名:{item.CountryName}");   
	}
	 WriteLine($"----------------------------------------------------------");
	 WriteLine();
    }
    /*
	页号:1----------------------------------------------------------
	序号 0 加入时间:1989 编号A-C12QX12456 国名:China
	序号 1 加入时间:1959 编号V-Q99QX52457 国名:America
	序号 2 加入时间:1961 编号V-E19CF82357 国名:Japan
	序号 3 加入时间:1961 编号V-E19CF82357 国名:Indian
	序号 4 加入时间:1972 编号W-C16DF77786 国名:Intali
	----------------------------------------------------------

	页号:2----------------------------------------------------------
	序号 0 加入时间:1973 编号O-G62QX36445 国名:Canadna
	序号 1 加入时间:1982 编号A-N42XX15798 国名:English
	序号 2 加入时间:1981 编号Q-Q42XX58198 国名:Germany
	序号 3 加入时间:1980 编号C-Q42XX58198 国名:Norway
	序号 4 加入时间:1980 编号K-K41XX58198 国名:Vietnam
	----------------------------------------------------------

	页号:3----------------------------------------------------------
	序号 0 加入时间:1980 编号L-K46XX58198 国名:Myanmar
	序号 1 加入时间:1980 编号F-G42XX58198 国名:Thailand
	序号 2 加入时间:1980 编号W-C42XX58198 国名:Laos
	----------------------------------------------------------    
    */
    
```
#####  <a id="polymerizationFunction" >`聚合Count,Sum,Min,Max,Average,`</a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`类似于SQL的聚合函数,应该没有什么讲的`

|`标准查询操作符`|`说明`| 
|:----|:------|
|Aggregate|一个累加器函数|

```C#
  Aggregate<TSource>(Func<TSource, TSource, TSource>)
  //对一个序列应用累加器函数
  Aggregate<TSource, TAccumulate>(TAccumulate, Func<TAccumulate, TSource, TAccumulate>)
  //对一个序列应用累加器函数。 将指定的种子值用作累加器初始值。
  Aggregate<TSource, TAccumulate, TResult>(TAccumulate, Func<TAccumulate, TSource, TAccumulate>, 
  Func<TAccumulate, TResult>)	
  //已重载。对一个序列应用累加器函数。 将指定的种子值用作累加器的初始值，并使用指定的函数选择结果值。
```
```C#
    string sentence = "the quick brown fox jumps over the lazy dog";
    string[] words = sentence.Split(' ');
            string reversed = words.Aggregate((workingSentence, next) =>
                                                next + " -- " + workingSentence);
    Console.WriteLine(reversed);
    
    //输出:dog -- lazy -- the -- over -- jumps -- fox -- brown -- quick -- the
```
#####  <a id="ChangeToCollection" >`3.14 转换ToList(),ToLookup....`</a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`使用转换操作符会立即执行查询，把查询结果放到数组,列表和字典中`

|`标准查询操作符`|`说明`| 
|:----|:------|
|ToArray()|立即执行查询把查询结果保存为数组返回|
|AsEnumerable()|立即执行查询把查询结果保存为枚举列表返回|
|ToList()|立即执行查询把查询结果保存为集合列表返回|
|ToDictionary()|立即执行,将结果变成字典返回|
|Cast<TResult>|转换集合|
|ToLookup()|转换为字典|	

* ToList
```C#
	var query=(from user in rs.Cast<GCRacer>()
					where user.Country.Trim()=="China"
					select new {
						user.GetName,
						user.Id,
						user.Wins,
						user.Country
					}).ToList();
	foreach(var user in query){
		WriteLine($"Country{user.Country} Id: {user.Id}  Name:{user.GetName} Win Times:{user.Wins}");
	}
```
* ToLookup
```C#
	var query=(from user in rs.Cast<GCRacer>()   //类型转换
					where user.Country.Trim()=="China"
					select new {
						user.GetName,
						user.Id,
						user.Wins,
						user.Country
					}).ToLookup(key=>key.Id,value=>new {
						Name=value.GetName,
						WinTimes= value.Wins,
						Country=value.Country
					});
	foreach(var user in query){
		var key=user.Key; 
		Write("编号: {0:D3} ",key);
		foreach(var  c in user){
			WriteLine($"国家 {c.Country} 名称:{c.Name} 获胜次数: {c.WinTimes} ");    
		}
	}
/*
	编号: 001 国家 China   名称:J X 获胜次数: 20
	编号: 004 国家 China   名称:Z B 获胜次数: 5
	编号: 005 国家 China   名称:J T 获胜次数: 10
	编号: 006 国家 China   名称:X X 获胜次数: 20
	编号: 009 国家 China   名称:Q B 获胜次数: 5
	编号: 010 国家 China   名称:N T 获胜次数: 10
*/	
```
#####  <a id="GetRange" >`3.15 生成操作符Range..`</a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>

|`标准查询操作符`|`说明`| 
|:----|:------|
|Empty|生成一个空的集合|
|Range|返回一系列的数字组成的数字|
|Repeat|返回一个始终重复一个值的集合|

```C#
	var values=Enumerable.Range(1,20);
	foreach(var item in values){
		Console.Write(item+" ");
	} //1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20
	
	var valuerep=Enumerable.Repeat(10,5);
	foreach(var item in valuerep){
		Console.Write(item+" ");
	} //10 10 10 10 10
```

####  <a id="AsynAwaitLinq" >`并行 Linq..`</a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`对集合调用	AsParallel<TSource>(),启用查询的并行化.`

```C#
List<Country> countrys= new List<Country>(){
    new Country("China","A-C12QX12456",1989),
    new Country("America","V-Q99QX52457",1959),
    new Country("Japan","V-E19CF82357",1961),
    new Country("Indian","V-E19CF82357",1961),
    new Country("Intali","W-C16DF77786",1972),

    new Country("Canadna","O-G62QX36445",1973),
    new Country("English","A-N42XX15798",1982),
    new Country("Germany","Q-Q42XX58198",1981),
    new Country("Norway","C-Q42XX58198",1980),
    new Country("Vietnam","K-K41XX58198",1980),

    new Country("Myanmar","L-K46XX58198",1980),
    new Country("Thailand","F-G42XX58198",1980),
    new Country("Laos","W-C42XX58198",1980)
};
var valuerep=countrys.AsParallel().Where(val=>val.CountryStartTime>=1980);
foreach(var item in valuerep){
	WriteLine($"加入时间:{item.CountryStartTime} 编号{item.CountryId} 国名:{item.CountryName}");   
}
```
##### <a id="CancelSearch" >`取消查询..`</a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
* `CancellationTokenSource  被用来做消息传递机制 的令牌`
* `命名空间`:`System.Threading`

```C#
var cts= new CancellationTokenSource();
var valuerep=countrys.AsParallel().WithCancellation(cts.Token)
.Where(val=>val.CountryStartTime>=1980).Select(val=>val);
//将查询包装在一个线程中  运行 然后在另一个线程中调用  cts.Cancel方法 就可停止查询
```
