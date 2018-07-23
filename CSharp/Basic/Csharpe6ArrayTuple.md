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
#### <a id="DuoWeiArray">1.1&nbsp;&nbsp; :sailboat: 多维数组</a> <a href="#top"> `置顶` :arrow_up:</a>

`多维数组的申请方式需要逗号隔开[,,....]`
* 三维数组: `int[,,] list=new int[10,10,10]`;
* 二维数组: `int[,] list=new int[10,10]`

`得到多维数组维度的方法` `GetUpperBound(i)+1`  `GetLength(i)`

 * `第一种方法`:`得到` `int[,] list=new int[7,8]` `的行数:list.GetLength(0),列数:GetLength(1)` 
 
 * `第二种方法`:`第一维度[行数]:list.GetUpperBound(0)+1, 第二维度[列数]list.GetUpperBound(1)+1`
 
 * `得到一个数组的第i个维度数` `list.GetLength(i-1) 或者 list.GetUpperBound(i-1)+1`

#### <a id="JuChiArray">1.1&nbsp;&nbsp; :sailboat: 锯齿数组</a> <a href="#top"> `置顶` :arrow_up:</a>
`用得不多,看看就行`
* 注意申请方式,使用中括号间隔申请,必须声明行数
```C#
  int[][] juchi = new int[3][]；
  int[0]=new int[2]{1,2}
  int[1]=new int[6]{1,2,3,5,6,7}
  int[2]=new int[3]{1,2,9}
```
###### 这个锯齿数组实际上就是申请一个3个元素的数组,然后每一个数组里面放一个指针,指向一个一维数组

#### <a id="ArrayArray">1.1&nbsp;&nbsp; :sailboat: Array 类</a> <a href="#top"> `置顶` :arrow_up:</a>
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
#### <a id="ArraySort">1.1&nbsp;&nbsp; :sailboat: 数组排序</a> <a href="#top"> `置顶` :arrow_up:</a>
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
