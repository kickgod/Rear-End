<a id="top">:checkered_flag: </a>数组和元祖 :bus:
----
:fast_forward: `数组就不说了,和C语言差不多啊,但是现在一般都用集合和数组了,但是呢该学的还是要学啊,这里注意关于数组的排序和过滤方法`:rewind:  
:fast_forward: `元组还是很重要的，多看看主要用于数组传递结合了泛型`:rewind:
### :arrow_lower_right: 1. 数.组 :information_source:
   - [x] <a href="#SampleArray"> 简单数组</a> :minibus:
   
   - [x] <a href="#DuoWeiArray"> 多维数组</a> :bird:
   
   - [x] <a href="#JuChiArray"> 锯齿数组</a> :bird:
   
   - [x] <a href="#ArrayArray">Array 类</a> :moneybag:
   
   - [x] <a href="#ArraySort">数组排序</a> :moneybag:
   
   - [x] <a href="#EnumQi">枚举器</a> :whale2:

   - [x] <a href="#yieldEnumQi">yield 语句</a> :whale2:
   
### :arrow_lower_right: 1. 元.组 :information_source:
   - [x] <a href="#SampleTuple"> 元祖实例</a> :minibus:

#### <a id="SampleArray">1.1&nbsp;&nbsp; :sailboat: 简单数组</a> <a href="#top"> `置顶` :arrow_up:</a>
`如果事先不知道数组有多少个数组最好用集合`
* 二维数组申请中间用逗号 `int[,] list=new int[5,5]`; 申请一个五行五列的数组
* 数组使用for语句迭代数组中的元素之外,还可以使用foreach语句迭代
* **`foreach`** `之所以能够迭代的原因是因为实现了` **`IEnumerator`**
```C#
  int[] MyArray=new int[10];  //MyArray 是一个引用存放在栈中 指向-> 存放在堆中的数组数据
  int[0]=10;  //初始化值
  int a=int[0]; //取得其值
  
  //数组初始化方式
  int[] ary=new int[]{1,2,3,4,5,6,7,8,9};   //或者  int[] ary=new int[9]{1,2,3,4,5,6,7,8,9}; 都可以
  
  //申请一个2行9列的数组
  int[,] ary=new int[2,9]
  {
      {1,2,3,4,5,6,7,8,9},
      {1,2,3,4,5,6,7,8,9}
  };
 
``` 
`数组遍历`
```C#   
   foreach(var val in ary){
       Console.Write($"{val.ToString()} ");
   }
```
#### <a id="DuoWeiArray">1.2&nbsp;&nbsp; :sailboat: 多维数组</a> <a href="#top"> `置顶` :arrow_up:</a>

`多维数组的申请方式需要逗号隔开[,,....]`
* 三维数组: `int[,,] list=new int[10,10,10]`;
* 二维数组: `int[,] list=new int[10,10]`

`得到多维数组维度的方法` `GetUpperBound(i)+1`  `GetLength(i)`

 * `第一种方法`:`得到` `int[,] list=new int[7,8]` `的行数:list.GetLength(0),列数:GetLength(1)` 
 
 * `第二种方法`:`第一维度[行数]:list.GetUpperBound(0)+1, 第二维度[列数]list.GetUpperBound(1)+1`
 
 * `得到一个数组的第i个维度数` `list.GetLength(i-1) 或者 list.GetUpperBound(i-1)+1`

#### <a id="JuChiArray">1.3&nbsp;&nbsp; :sailboat: 锯齿数组</a> <a href="#top"> `置顶` :arrow_up:</a>
`用得不多,看看就行`
* 注意申请方式,使用中括号间隔申请,必须声明行数
```C#
  int[][] juchi = new int[3][]；
  int[0]=new int[2]{1,2}
  int[1]=new int[6]{1,2,3,5,6,7}
  int[2]=new int[3]{1,2,9}
```
###### 这个锯齿数组实际上就是申请一个3个元素的数组,然后每一个数组里面放一个指针,指向一个一维数组

#### <a id="ArrayArray">1.4&nbsp;&nbsp; :sailboat: Array 类</a> <a href="#top"> `置顶` :arrow_up:</a>
`使用中括号声明数组其实是C#中使用Array类的表示法,在后台其实是一个派生自抽象类基类Array的新类,这样就可以使用Array类为每个C#数组定义的方法和属性了`

---
Array是一个抽象类,所以不能使用构造函数创建数组需要使用方法 `	CreateInstance(Type, Int32[])`

```C#
   //创建一个二维数组
   Array ary=Array.CreateInstance(typeof(int),10,20);
   
   //创建一个一维数组
   Array ary=Array.CreateInstance(typeof(int),10);
   //初始化
   for(int i=0;i<arys.Length;i++){
       arys.SetValue(i+10,i);
   }
   //访问
   for(int i=0;i<arys.Length;i++){
       WriteLine(arys.GetValue(i));
   }
   
   //得到行数和列数
   Console.WriteLine($"Row：{art.GetLength(0)} Column: {art.GetLength(1)}");
   
```
##### 数组复制
* `Array.Copy(Object source,Object Destination,Length)` `arys.CopyTo(copyResult,index_Start);`
* `args.Object Clone()`
```C#
      Array arys=Array.CreateInstance(typeof(int),10);    
      for(int i=0;i<arys.Length;i++){
          arys.SetValue(i+10,i);
      }
      Array copyResult=Array.CreateInstance(typeof(int),10);    
      Array.Copy(arys,copyResult,arys.Length);

      arys.CopyTo(copyResult,0);
      
      var copyResult_=arys.Clone();
```
#### <a id="ArraySort">1.5&nbsp;&nbsp; :sailboat: 数组排序</a> <a href="#top"> `置顶` :arrow_up:</a>
`数组排序使用Array.Sort(myArray) 方法 `

* `1.要求`:`myArray 数组里面的成员必须实现IComparable<T>接口  里面定义了一个 ComparaTo()方法 相等返回0 大于返回1 小于返回-1`

* `2.也可利用一个类实现 IComparar<T> 接口 里面有一个CompareTo() 方法 然后传递两个参数给Sort方法 第二参数是实现了IComparar<T>的类 也可以直接使用委托`

```C#
   public void Start() { 
      Array arys = Array.CreateInstance(typeof(int), 10);

      for(int i=0;i<arys.Length;i++){
          arys.SetValue(i+10,i);
      }

      Array.Sort(arys,(IComparer)new box());  
      //VScode  要报错 请用vs2017 因为 有时候需要引入一些命名空间
      // 如果不想报错请引入
      
      /* using System.Collections.Generic;
         using System.Collections;
       */
       
       // Array.Sort(arys,1,9,(IComparer)new box());   只对1~9号元素进行排序
   }
   
   public class box : IComparer<int>
    {
        public int Compare(int i, int j)
        {
            return (i % 5) - (j % 5);
        }
    }
```
##### ArraySegment<T>
`这个类可以获得数组的一部分`
 ```C#
    int ar1={1,2,3,4,5,6,7,8,9};
    var seg= ArraySegment<int>(ar1,0,5);
 ```
#### <a id="EnumQi">1.6&nbsp;&nbsp; :sailboat: 枚举器</a> <a href="#top"> `置顶` :arrow_up:</a>
:fast_forward: `在foreach语句中使用枚举,可以迭代集合中的元素,而无须知道集合中元素的个数,foreach语句使用了一个枚举器,集合和数组实现了带GetEumerator()方法的IEumerator接口,GetEumerator()返回一个实现IEumerator接口的枚举，这样foreach就可以使用IEumerator迭代集合了。`:rewind:
* `foreach 语句使用IEnumerator接口的方法和属性，迭代所有的集合`
 
```C#
   //源代码:
   #region Assembly System.Runtime, Version=4.2.1.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a
   // System.Runtime.dll
   #endregion

   namespace System.Collections
   {
       public interface IEnumerator
       {

           object Current { get; } //当前元素

           bool MoveNext(); //下一个元素 返回是否有下一个元素

           void Reset(); //和COM交互操作
       }
   }
```
`操作IEnumerator接口方法  利用一些类型数组已经实现的GetEnumerator 方法`
```C#
   int[] list= new int[10]{1,2,3,4,5,6,7,8,9,10};

   IEnumerator Enum=list.GetEnumerator();

    while(Enum.MoveNext()){
        WriteLine($"current:{Enum.Current}");   
    }   

    //foreach 的本质就是在编译器中实现了上面的功能
```
#### <a id="yieldEnumQi">1.7&nbsp;&nbsp; :sailboat: yield 语句</a> <a href="#top"> `置顶` :arrow_up:</a>
`可以利用 yield语句 实现一个集合创建一个枚举器.下面是一个例子`
```C#
   using System;
   using System.Collections.Generic;
   using System.Collections;
   namespace DotnetConsole
   {
       public class HelloCollection
       {
           public IEnumerator<string> GetEnumerator(){
               yield return "Hello world";
               yield return "你好 世界";
               yield return "nihao shijie";
               yield return "wata lisp";
           }
       }
   }
   
   
     static void Main(string[] args)
     {
            HelloCollection ho=new HelloCollection();

            IEnumerator Enum=ho.GetEnumerator();

             while(Enum.MoveNext()){
                 WriteLine($"Current Value:{Enum.Current}");   
             }   
       }
       //输出:
       /*
         Current Value:Hello world
         Current Value:你好 世界
         Current Value:nihao shijie
         Current Value:wata lisp       
        */
```
#### <a id="SampleTuple">2.1&nbsp;&nbsp; :sailboat: 元祖实例</a> <a href="#top"> `置顶` :arrow_up:</a>
`数组合并了相同类型的元素,而元祖合并了不同类型的元素.元祖取源于函数编程语言F#,它频繁使用元组,.NET FK 定义了八个泛型Tuple类和一个静态的类,它们用作
元组的工厂.不同的泛型Tuple类支持不同数量的元素`
* `1`:`Tuple<T1>`
* `2`:`Tuple<T1,T2>`
* `3`:`Tuple<T1,T2,T3>`
* `....`
* `总共八个元素`,`如果想要超过八个元素可以元组里面嵌套元素,就可以扩展无限多的元素个数`

* `使用Tuple类的Create方法创造元组,使用Itemi 访问第i个元素`

```C#
     public static Tuple<int,string> GetInfoByUserID(String userId){
         // 查询数据库 省略
         int result=userId.Length;
         String name="LiZhiMing";
         return Tuple.Create(result,name);
     }
     static void Main(string[] args)
     {
          var UserInfo=GetInfoByUserID("2016110418");
          WriteLine($"User Name:{UserInfo.Item1},User Age:{UserInfo.Item2}"); //使用Item1 Item2访问元素
     }
     //输出 User Name:10,User Age:LiZhiMing
```
##### 比较元组 -----2015以后 被废弃了
`实现IEqualityComparer 接口的类作为第二个参数传递给元组的Equals方法`
```C#
	public class TupleComparer<T1,T2>:IEqualityComparer
	{
		new public bool Equals(object x, object y)
		{
			if (x is T1)
			 //if (typeof(x) is string) 
				return true;
			return x.Equals(y);
		}
		public int GetHashCode(object obj)
		{
			if (obj is T1)
				return ((T1) obj).GetHashCode();
			else
				return ((T2) obj).GetHashCode();
			//return obj.GetHashCode();
		}
	}
    
    var UserInfo=GetInfoByUserID("2016110418");
    var UserInfo2=GetInfoByUserID("201611041");
    UserInfo.Equals(UserInfo2,(IEqualityComparer)new TupleComparer()); //这里报错--错误不明中
```
