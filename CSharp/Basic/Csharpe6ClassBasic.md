C# 类
-----
*  <a href="#ClassStructDifference">类和结构的区别</a>



---
#### <a id="ClassStructDifference">类和结构的区别</a>
* `类和结构实际上都是创建对象的模板,每个对象都包含数据`,`并提供了处理和访问数据的方法`
* `说明`:`结构不同于类,因为它们不需要再堆上分配空间,类在堆上 结构在栈上面`
* `说明`:`大多数情况下类要比结构常用得多`
* `说明`:`类和结构使用new来声明实例,默认初始化字段为null或者0`
----
#### 类的成员
|成员|说明|
|-|-|
|<a href="#ziduan">`字段`</a>|`字段是类的数据成员,它是类型的一个变量,这类型是类的一个成员,字段命名方式为：_book 然后使用属性封装命名为Book`|
|`常量`|`常量和类相关,编译器使用真实值代替常量`|
|`属性`|`与读取和写入类的已命名属性相关联的操作`|
|`方法`|`类可以执行的计算和操作`|
|`索引器`|`与将类实例编入索引（像处理数组一样）相关联的操作`|
|`构造函数`|`初始化类实例或类本身所需的操作`|
|`终结器`|`永久放弃类实例前要执行的操作`|
|`类型`|`类声明的嵌套类型`|
|`运算符`|`运算符重载之类的`|
|`事件`|`类可以生成的通知`|
##### <a id="ziduan">字段,构造函数</a>
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
#### 属性
* `新特性`:`表达式属性---public String userName=>"JxKicker";`
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
#### 方法
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

##### 索引器
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


-------
#### C# 类
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
