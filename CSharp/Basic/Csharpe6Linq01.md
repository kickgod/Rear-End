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
   * <a href="#tiaojianshaixuan" >`条件筛选 where`</a>
   * <a href="#fromSeletMany" >`复合 from`</a>
   * <a href="#OrderbyWhat">`排序 orderby`</a> 
   * <a href="#GroupFenZu">`分组 Group`</a>
      
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

```C#
        var query=from val in stus
                  group val by val.Sex into SexGroup
                  orderby SexGroup.Count() descending,SexGroup.Key
                  where SexGroup.Count() >0
                  select new {
                       SexName=SexGroup.Key?"男":"女",
                       Count=SexGroup.Count() 
                  };
        /*
        var query=stus.GroupBy(val=>val.Sex).Where(g=>g.Count()>0).Select(g=>new {
           SexName=g.Key?"男":"女",
           Count=g.Count()
        })        
        */          

        foreach(var val in query.Distinct()){
            Console.WriteLine( $"类别: {val.SexName} 数量:{val.Count}" );
        }
        /*
          类别: 男 数量:4
          类别: 女 数量:3 */
```













