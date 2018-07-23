<a id="top">:checkered_flag: </a>数组和元祖 :bus:
----
:fast_forward: `数组就不说了,和C语言差不多啊,但是现在一般都用集合和数组了,但是呢该学的还是要学啊,这里注意关于数组的排序和过滤方法`:rewind:  
:fast_forward: `元组还是很重要的，多看看主要用于数组传递结合了泛型`:rewind:
### :arrow_lower_right: 1. 数.组 :information_source:
   - [x] <a href="#SampleArray"> 简单数组</a> :minibus:

#### <a id="SampleArray">1.1&nbsp;&nbsp; :sailboat: 简单数组</a> <a href="#top"> `置顶` :arrow_up:</a>
`如果事先不知道数组有多少个数组最好用集合`
* 二维数组申请中间用逗号 `int[,] list=new int[5,5]`; 申请一个五行五列的数组
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
