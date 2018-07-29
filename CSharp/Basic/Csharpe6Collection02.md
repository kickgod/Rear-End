<a id="top">:checkered_flag:</a> C# 特殊集合 :blue_heart:
-----
`C#有一些特殊的集合可以用于一些特殊的操作，例如位数组,位矢量,可观察集合,不可变集合,管道,不可变集合`

- [x] :whale2: <a href="#BitArray">`BitArray`</a>
- [x] :whale2: <a href="#ObservableCollection">`ObservableCollection<T>`</a>

----
#### :whale2: <a id="BitArray" href="https://msdn.microsoft.com/zh-cn/library/system.collections.bitarray(v=vs.110).aspx">`BitArray`</a>
`BitArray 可以重新设置大小,如果事先不知道需要多少位数，就可以使用BitArray类,是一个引用类型`

* 构造函数
  * 1.0
```C#
       BitArray ibm=new BitArray(5); //默认每一位为 false 0
       ibm.Set(1,true);
       Console.WriteLine("位数:"+ibm.Count); 
       Console.WriteLine("长度:"+ibm.Length); 
       foreach(bool bit in ibm){
           Console.Write((bit? 1:0)+" ");
       }
       Console.WriteLine(); 
       /*
        位数:5
        长度:5
        0 1 0 0 0 
        */ 
```
  * 1.1
```C#
       BitArray ge=new BitArray(10,true); //初始化每一位 为 true 1
       foreach(bool bit in ge){
           Console.Write((bit? 1:0)+" ");
       }
       Console.WriteLine();    // 1 1 1 1 1 1 1 1 1 1
```
  * 1.2
```C#
       BitArray bo=new BitArray(new bool[]{false,true,false,true,true});
       foreach(bool bit in bo){
           Console.Write((bit? 1:0)+" ");
       }
       Console.WriteLine();   //0 1 0 1 1
```
* 常用属性
	* `Count`:`获取 BitArray 中包含的元素数。`
  *	`IsReadOnly`:`获取一个值，该值指示 BitArray 是否为只读`
  * `IsSynchronized	`:`获取一个值，该值指示是否同步对 BitArray 的访问（线程安全）。`
  * `Item[Int32]	`:`获取或设置 BitArray 中特定位置的位值。`
  * `Length`:	`获取或设置 BitArray 中的元素数。`
  * `SyncRoot	`:`获取可用于同步对 BitArray 的访问的对象。`
* 常用方法
  * `Set(Int32, Boolean)`:`将 BitArray 中特定位置处的位设置为指定值。`
  * `SetAll(Boolean)`:`将 BitArray 中的所有位设置为指定值。`
  * `And(BitArray)`:`在当前 BitArray 对象中的元素和指定数组中的相应元素之间执行按位“与”运算。 将修改当前 BitArray 对象，以存储按位“与”运算的结果。`
  * `Get(Int32)`:`获取 BitArray 中特定位置处的位值。`
  * `Not()`:`反转当前 BitArray 中的所有位值，以便将设置为 true 的元素更改为 false；将设置为 false 的元素更改为 true。`
  * `Or(BitArray)`:`在当前 BitArray 对象中的元素和指定数组中的相应元素之间执行按位“或”运算。 将修改当前 BitArray 对象，以存储按位“或”运算的结果。`

#### :whale2: <a id="ObservableCollection" href="https://msdn.microsoft.com/zh-cn/library/ms668604(v=vs.110).aspx">`ObservableCollection<T>`</a>

* `ObservableCollection派生自Collection<T> 该基类可用于创建自定义集合,并在内部使用List<T> 类,重写基类的虚方法SetItem和RemoveItem，以触发Collection
Changed事件`
* `这个类的用户可以使用INotifyCollectionChanged接口注册事件。`
* `命名空间`:`System.Collections.ObjectModel`
```C#
/* 赛车手类*/
using System;
using System.Collections;
using System.Collections.Generic;

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
}

using System;
using System.Text;
using System.Text.RegularExpressions;
using System.Collections.Generic;
using System.Collections;
using DotnetConsole.classlib;
using System.Linq;
using System.Collections.ObjectModel;
using System.Collections.Specialized;

namespace DotnetConsole
{
    using static System.Console;
    class Program
    {
        static void Main(string[] args)
        {
           //初始化集合
            List<Racer> rs=new List<Racer>{
                new Racer(1,"J","X","China  ",20),
                new Racer(2,"C","L","America",3),
                new Racer(3,"G","X","Indian ",2),
                new Racer(4,"Z","B","China  ",5),
                new Racer(5,"J","T","China  ",10)
           };
           var data=new ObservableCollection<Racer>();
            data.CollectionChanged+=Data_CollectionChange;
            data.Add(rs[1]);
            data.Add(rs[2]);
            data.Add(rs[3]);
            data.Remove(rs[1]);
            data.Remove(rs[3]);

        }

        public static void Data_CollectionChange(object sender, NotifyCollectionChangedEventArgs e){
             Console.WriteLine("集合对象发生变化:");
             Console.WriteLine("执行动作:"+ e.Action.ToString());
             if(e.OldItems!=null){
                 Console.WriteLine("删除了元素:" +"删除索引开始位置:"+e.OldStartingIndex);
                 foreach(var item in e.OldItems){
                     Console.WriteLine(item.ToString());
                 }
             }
             if(e.NewItems!=null){
                 Console.WriteLine("新增索引开始位置:"+e.NewStartingIndex);
                 foreach(var item in e.NewItems){
                     Console.WriteLine(item.ToString());
                 }
             }
             Console.WriteLine();
        }   
    }
}
          /*
          集合对象发生变化:
          执行动作:Add
          新增索引开始位置:0
          ID: 2 Name:C L Country:America Wins Time:3

          集合对象发生变化:
          执行动作:Add
          新增索引开始位置:1
          ID: 3 Name:G X Country:Indian  Wins Time:2

          集合对象发生变化:
          执行动作:Add
          新增索引开始位置:2
          ID: 4 Name:Z B Country:China   Wins Time:5

          集合对象发生变化:
          执行动作:Remove
          删除了元素:删除索引开始位置:0
          ID: 2 Name:C L Country:America Wins Time:3

          集合对象发生变化:
          执行动作:Remove
          删除了元素:删除索引开始位置:1
          ID: 4 Name:Z B Country:China   Wins Time:5

          */
```
