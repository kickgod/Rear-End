<a id="top">C# 类</a> . :blue_heart:	
-----
- [x] <a href="#ClassStructDifference" >类和结构的区别</a>  

- [x] <a href="#AnonymousType">匿名类型</a>  

- [x] <a href="#Structure">结构体</a>  

- [x] <a href="#CallByValueAndByReference">按值调用和按引用调用</a>

- [x] <a href="#AccessModifier">修饰符</a>

- [x] <a href="#ExtensionMethod">扩展方法</a>

---
#### <a id="ClassStructDifference">类和结构的区别</a>  <a href="#top">置顶 :arrow_up:</a>  
* `类和结构实际上都是创建对象的模板,每个对象都包含数据`,`并提供了处理和访问数据的方法`
* `说明`:`结构不同于类,因为它们不需要再堆上分配空间,类在堆上 结构在栈上面`
* `说明`:`大多数情况下类要比结构常用得多`
* `说明`:`类和结构使用new来声明实例,默认初始化字段为null或者0`
----
#### 类的成员
|成员|说明|
|-|-|
|<a href="#ziduan">`字段`</a>|`字段是类的数据成员,它是类型的一个变量,这类型是类的一个成员,字段命名方式为：_book 然后使用属性封装命名为Book`|
|<a href="#changliang">`常量`</a>|`常量和类相关,编译器使用真实值代替常量`|
|<a href="#shuxing">`属性`</a>|`与读取和写入类的已命名属性相关联的操作`|
|<a href="#fangfa">`方法`</a>|`类可以执行的计算和操作`|
|<a href="#suoyinqi">`索引器`</a>|`与将类实例编入索引（像处理数组一样）相关联的操作`|
|<a href="#gouzaohanshu">`构造函数`</a>|`初始化类实例或类本身所需的操作`|
|`终结器`|`永久放弃类实例前要执行的操作`|
|`类型`|`类声明的嵌套类型`|
|`运算符`|`运算符重载之类的`|
|`事件`|`类可以生成的通知`|
-----
<a href="#zonghe">综合实例</a>
##### <a id="ziduan">字段</a>
```C#
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);
    private byte r, g, b;
    public Color(byte r, byte g, byte b) 
    {
        this.r = r;
        this.g = g;
        this.b = b;
    }
}
```
#### <a id="shuxing">属性</a>
* `新特性`:`表达式属性---public String _userName=>"JxKicker";`
* `只读属性`: `可以使用readonly修饰符字段`
* `也可以使用只读属性`:`public String Id{get;};`
```C#
    public int _id;

    public int ID{
        get {return _id;} 
        set{_id=value;}
    }
    /*这样写太麻烦了,我们可以通过get;set;编译器自动实现字段*/
    
    public int ID{get;set;} 
    
    /*编译器会自动实现一个_id字段*/
```
#### <a id="fangfa">方法</a>
* 表达式体方法:public int getAryCount()=>ArrayInt.Length; 常常用于一些简单的方法,复杂方法还是需要自己写
* 方法的重载:参数类型不同,参数个数不同,方法名称相同
* 默认参数,参数也可以是可选的,必须为可选参数提供默认值,可选参数还必须是方法定义的最后的参数
```C#
    public int getAryCount()=>ArrayInt.Length;

    public void AddList(int[] list){
         int[] newList=new int[list.Length+ArrayInt.Length];
         int i=0;
         int j=0;
        foreach (var item in list)
        {
            newList[i]=ArrayInt[j];
        }
        j=0;
        foreach (var item in ArrayInt)
        {
            newList[i]=list[j];
        }
        ArrayInt=newList;
    }
    
    /*默认方法*/
    public int TestMethoed(int notOptionNunber,int OptionNumber=42){
        return notOptionNunber+OptionNumber;
    }

```
#### 参数个数可变
* 可以使用可选参数,可以定义数量可变的参数
* 声明数组类型的参数(示例代码使用一个int数组),添加params关键字,就可以使用任意数量的int参数调用该方法
```C#
    /*参数可变*/
    public int Sum(params int[] data){
        int sums=0;
        foreach(var val in data){
            sums+=val;
        }
        return sums;
    }
    
    public int ShowName(params String[] data){
        foreach(var val in data){
           WriteLine(val);
        }
        return data.Length;
    }
```

##### <a id="suoyinqi">索引器</a> 
* 重点:get,set 后面不能跟分号`;`并且 不能像属性方法一样使用名称 只能使用this名称
* 直接使用类名加索引访问
```C#
using System;
using System.Text;
using System.Collections.Generic;

namespace DotnetConsole
{
    using static System.Console;
    class MyClass
    {
        public int[] ArrayInt;

        public MyClass(int [] list){
            this.ArrayInt=list;
        }

        public int this[int index]{
            get{
                if(index<0||index>this.ArrayInt.Length){
                    throw new Exception("索引越界");
                }else{
                    return ArrayInt[index];
                }
            }
            set{
                if(index>0||index<this.ArrayInt.Length){
                     ArrayInt[index]=value;
                }else{

                }
            }
        }
    }
}


/* 访问*/
using System;
using System.Text;
using System.Collections.Generic;

namespace DotnetConsole
{
    using static System.Console;
    class Program
    {
        static void Main(string[] args)
        {
            MyClass ary=new MyClass(new int[] {1,2,45,26,-99,-100,52,-78,-55});
            ary[5]=150;
            WriteLine(ary[3]);
        }
    }
}



```
#### <a id="gouzaohanshu">C# 构造函数</a>
* 调研父类构造函数的方法
```C#
  class FatherClass{
     FatherClass(){
         //etc
     }
     
     FatherClass(String name){
        this.Name=name;
     }
     //etc
  }
  class SonClass:FatherClass{
      public string _sonFenzhi;
     
      SonClass():base(){
         
         //etc
      }
      
      SonClass(String FaherName,String _sonFenzhi):base(FaherName){
        this._sonFenzhi=_sonFenzhi;
      }
  }
```

-------
#### <a id="zonghe">C# 类</a> 
```C#
public class List<T>
{
    // Constant
    const int defaultCapacity = 4;

    // Fields
    T[] items;
    int count;
    // Constructor
    public List(int capacity = defaultCapacity) 
    {
        items = new T[capacity];
    }
    // Properties
    public int Count => count; 

    public int Capacity 
    {
        get { return items.Length; }
        set 
        {
            if (value < count) value = count;
            if (value != items.Length) 
            {
                T[] newItems = new T[value];
                Array.Copy(items, 0, newItems, 0, count);
                items = newItems;
            }
        }
    }
    // 索引器
    public T this[int index] 
    {
        get 
        {
            return items[index];
        }
        set 
        {
            items[index] = value;
            OnChanged();
        }
    }
    
    // 方法
    public void Add(T item) 
    {
        if (count == Capacity) Capacity = count * 2;
        items[count] = item;
        count++;
        OnChanged();
    }
    protected virtual void OnChanged() =>
        Changed?.Invoke(this, EventArgs.Empty);

    public override bool Equals(object other) =>
        Equals(this, other as List<T>);

    static bool Equals(List<T> a, List<T> b) 
    {
        if (Object.ReferenceEquals(a, null)) return Object.ReferenceEquals(b, null);
        if (Object.ReferenceEquals(b, null) || a.count != b.count)
            return false;
        for (int i = 0; i < a.count; i++) 
        {
            if (!object.Equals(a.items[i], b.items[i])) 
            {
                return false;
            }
        }
    return true;
    }

    // 事件
    public event EventHandler Changed;

    // 运算符重载
    public static bool operator ==(List<T> a, List<T> b) => 
        Equals(a, b);

    public static bool operator !=(List<T> a, List<T> b) => 
        !Equals(a, b);
}
```

#### <a id="AnonymousType">匿名类型</a> <a href="#top">置顶 :arrow_up:</a>  

`匿名类型提供了一种方便的方法，可用来将一组只读属性封装到单个对象中，而无需首先显式定义一个类型。 类型名由编译器生成，并且不能在源代码级使用。 每个属性的类型由编译器推断。`

* 匿名类型通常用在查询表达式的 select 子句中[Linq中]
* 定义一个匿名类型时，需要用到 var 关键字和对象初始化语法。
* 内部成员不需要类型声明 编译器自动推断
* 匿名类型同样继承Object它只是没有名字常用于数据传输和交换
----
```C#
    var car = new {
        Productor="BenTian",
        ProductTime=Convert.ToDateTime("2017/5/6"),
        User="Kicker"
    };

```
#### <a id="Structure" >结构体</a> <a href="#top">置顶 :arrow_up:</a>  
`结构的目的:有时候需要一个小的数据结构,此时类提供的功能多于我们需要的功能,由于性能原因,最后使用结构.`:smirk:  
* 结构体是值类型 存储在栈中
* 结构体不支持继承 每一个结构体都派生自`System.ValueType` 然后`ValueType`派生自`System.Object`
* 大多数时候结构体里面的字段全部声明为public
* new申请空间和类不同,结构体使用new运算符不分配堆中的内存。只是调研构造函数传递参数初始化值,不需要等待垃圾回收
* 当把结构体当参数传递时,最后使用按引用传递 使用关键字ref,不然默认是按值传递,造成性能损失
* 结构可带有 `方法`、`字段`、`索引`、`属性`、`运算符方法`和`事件`。:white_check_mark:
* 结构可定义构造函数，但不能定义析构函数。但是，您不能为结构定义默认的构造函数。默认的构造函数是自动定义的，且不能被改变。
-----
```C#
using System;
using System.Text;
using System.Collections.Generic;

namespace DotnetConsole
{
    using static System.Console;
    class MyStrcut
    {
        //字段
        public static int Count=0;
        public string _strcutName;


        //属性
        public string StrcutName{
            get{return _strcutName;}
            set{_strcutName=value;}
        }
        public Decimal X{get;set;}

        public Decimal Y{get;set;}

        //构造函数
        public  MyStrcut(Decimal x,Decimal y){
           Count++;
           this.X=x;
           this.Y=y;
        }
        //索引器
        int[]  ArrayList_int;

        public int this[int index]{
            get{
                return ArrayList_int[index];
            }
            set{
                this.ArrayList_int[index]=value;
            }
        }
        //方法
        public String getStringOfPoint()=> $"({X},{Y})";



        //运算符方法
        public static bool operator == (MyStrcut one,MyStrcut two) =>one.X.Equals(two.X)&&one.Y.Equals(two.Y);
 
        public static bool operator != (MyStrcut one,MyStrcut two) =>!(one.X.Equals(two.X)&&one.Y.Equals(two.Y));

        //事件
        public event EventHandler PointChange;
    }
}
```
#### <a id="CallByValueAndByReference">按值调用和按引用调用 </a> <a href="#top">置顶 :arrow_up:</a>  
`按值调用如果是值类型那么传递给方法的是复制品,如果按照引用调用传递给方法的是引用即如同指针一样的东东`  

----
* :whale2: **`ref`**: 通过引用传递变量

   ``` C#
        public static void Swap(ref int  a,ref int b){
            int c=a;
            a=b;
            b=c;
        }
        //Test
        int a=10;
        int b=20;
        Swap(ref a,ref b);
        WriteLine($"a:{a},b:{b}");
        
        //输出结果: a:20,b:10
   ``` 
    * 要求必须事先初始化 `int c,d;` ` Swap(ref c,ref d);` c和d没有初始化那么会报错 语法错误 无法编译通过
    
* :sailboat: **`out`**:同样按照引用传递,不要求变量一定初始化,只需要定义了就行,可以用于返回一些值
    ```C#
        public static void Sum(int  a,int b,out int sum){
             sum=a+b;
        }
        int a=10;
        int b=20;        
        int sum;
        Sum(a,b,out sum);
        WriteLine($"sum:{sum}");
        //输出 sum:30
    ```
    * 注意：语法ref,out 在方法中要写上修饰,还要在调用方法的时候在参数前面修饰
    * 注意：ref,out修饰符放在类型前面  例如 ref int a;
    * 注意:ref,out 常常用于值类型,引用类型不需要
----
#### <a id="AccessModifier">修饰符 </a> <a href="#top">置顶 :arrow_up:</a>  
`访问修饰符何以修饰类,结构体,还有类成员,甚至在C#6.0之后属性的get,set也可以使用访问修饰发修饰`

----
##### 访问修饰符
- [public](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/public)  `公共访问是允许的最高访问级别。 对访问公共成员没有限制`  public int x;

- [private](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/private) `私有访问是允许的最低访问级别。 私有成员只有在声明它们的类和结构体中才是可访问的`

- [internal](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/internal) `只有在同一程序集的文件中，内部类型或成员才可访问，即不再同一个dll中的`

- [protected](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/protected)	`只有在通过派生类类型进行访问时，基类的受保护成员在派生类中才是可访问的。`

##### :minibus: [修饰符](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/modifiers)

- [abstract](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/abstract) `指示某个类只能是其他类的基类。抽象类`

- [static](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/static) `声明属于类型本身而不是特定对象的成员。 静态方法/静态类`

- [sealed](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/sealed) `指定无法继承类。 密封类`

- [virtual](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/virtual) `在派生类中声明其实现可由重写成员更改的方法或访问器。 虚方法`

- [volatile](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/volatile) `指示字段可在程序中由操作系统、硬件或并发执行线程等项修改。`

- [readonly](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/readonly) `声明一个字段，该字段只能赋值为该声明的一部分或者在同一个类的构造函数中。可以用只读属性实现`

- [async](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/async) `指示修改后的方法、lambda 表达式或匿名方法是异步的。`

- [partial](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/partial-type) `在同一程序集中定义分部类、结构和方法。`

- [unsafe](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/unsafe) `生命不安全的上下文`

- [const](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/const) `常量,用于值类型,不可修改`

-----
#### <a id="ExtensionMethod" > :herb: 扩展方法</a> <a href="#top">置顶 :arrow_up:</a>  
`有许多扩展类的方式例如加上接口,继承 扩展方法是给对象添加功能的另一个选项,在不使用继承时,也可以使用这个选项,当一个类是密封的时候！`

-----
* `注意`,`扩展方法必须是静态的[static],它是类的一部分,但实际上没有放在类的源代码中`
* `注意`,`C#一切的代码都要放到类中 对于放扩展方法的类的命名方式 扩展类名称+Extension`
* `注意`,`容纳扩展方法的类也必须是静态的类`
* `注意`,`扩展方法也可以用于扩展接口,那么实现该接口的所有类就有了公共功能了`

##### 扩展string方法 统计单词个数
```C#
using System;
namespace DotnetConsole
{
    using static System.Console;
    public static class StringExtension
    {
        //this 关键字 和 第一个参数 来扩展字符串,这个关键字定义了要扩展的类型
        public static int GetWordCount(this string s)=>s.Split().Length;

    }

}
```
##### **`this 关键字 和 第一个参数定义了要扩展的类型`**








