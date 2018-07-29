<a id="top">:checkered_flag:</a> Linq 语言集成查询 :blue_heart:
---
`LINQ（Language Integrated Query）语言集成查询` 是一组用于c#和Visual Basic语言的扩展。它允许编写C#或者Visual Basic代码以操作内存数据的方式，查
询数据库。`对象查询语言`

- [x] :whale2: <a href="#">`初期准备`</a>
   * <a href="#LearingNeed">  `1. 知识需要` </a>
   * <a href="#DataPrepared"> `2. 程序数据准备`</a>
- [x] :whale2: <a href="#">`初期准备`</a>
#### 学习Linq的需要: <a id="LearingNeed"></a> 
* `C# 集合精通 非常的熟悉`
* `C# 类对象接口等等镜头 集合学习前面的需要全部`
* `SQL语句精通`
* `没有也可以学 只是学起来要痛苦一点`
#### 数据准备 <a id="DataPrepared"></a> 
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
