 :fountain: <a id="top">泛.型</a> :white_check_mark:
----
`这是C# 流式编程的开端,重要组成部分,没有泛型,集合将会失去强大的功能,流式编程将会无比艰难,泛型加上Lambda,集合,linq,正则表达式,扩展方法,匿名类型组成了C#的流式
编程。总而言之泛型很重要的`:closed_umbrella:

- [x] <a href="#FanXingLizi" > 泛型实例</a> :blue_book:

- [x] <a href="#GenericClassFucntion" > 泛型类的功能</a> :blue_book:

- [x] <a href="#GenericClassYueShu" > 泛型约束</a> :blue_book:

- [x] <a href="#GenericInnerface" > 泛型接口</a> :blue_book:

- [x] <a href="#GenericStruct" > 泛型结构</a> :sob:

- [x] <a href="#GenericFunction" > 泛型方法</a> :sweat_drops:

----
- [x] `有了泛型可以创建独立于被包含类型的类和方法,我们不必给不同的类型变成功能相同的许多方法和类,只创建一个方法或类;`

- [x] `另一个减少代码的选项是使用Object类,但是使用派生自Object类的类型进行传递不是类型安全的.泛型类使用泛型类型,`

- [x] `使用泛型类型可以最大限度地重用代码、保护类型安全性以及提高性能`

- [x] `在 System.Collections.Generic 命名空间中包含几个新的泛型集合类。 应尽可能使用这些类来代替某些类，如 System.Collections 命名空间中的 ArrayList。`

- [x] `泛型不仅限类,还可以使用于接口和方法`
-----

#### <a id="FanXingLizi">泛型实例</a> <a href="#top">置顶  :fountain:</a>
##### 简单的泛型例子
```C#
public class GenericList<T>
{
    public void Add(T input) 
    {
        System.Console.WriteLine(input.ToString());
    }
}
```
##### 泛型数组
```C#
using System;
namespace DotnetConsole
{
    public class GenericList<T>:IIncreseStep
    {
     
        public int Step{get;}=10;
        public int Opacity;
        public int length;
        public GenericList(int len=10){
            itemList=new T[len];
            Opacity=len;
            length=0;
        }

        T[] itemList;
        public void Add(T input) 
        {
            if(length+1==Opacity){
                IncreseArray();
            }
            System.Console.WriteLine($"Add Item: {input.ToString()}");
            itemList[length]=input;
        }

        public T this[int index]{
            get{
                if(index>=0&&index<Opacity){
                    return itemList[index];
                }else{
                    throw new Exception("索引越界");
                }
            }
            set{
                if(index>=0&&index<Opacity){
                    itemList[index]=value;
                }else{
                    throw new Exception("索引越界");
                }
            }
        }

        private void IncreseArray(){
            T[] newArray=new T[Opacity+Step];
            int len=0;
            foreach(var val in itemList){
                newArray[len++]=val;
            }
            Opacity=Opacity+Step;
            itemList=null;
            itemList=newArray;
        }

    }
}
```
##### 泛型命名规则T开头
```C#
 public class List<T>();
 
 public class LinkedList<T>{};
 
 public class SortedList<TKey,TValue> { };
```
------

#### <a id="GenericClassFucntion">泛型类的功能</a> <a href="#top">置顶  :fountain:</a>

##### 默认值

`泛型类default 关键字`
* `泛型参数可以是值类型也可以是引用类型,默认初始化可能是0也可能是null,使用default可以,将null赋值给引用类型,将0赋值给值类型。`
```C#
  public T GetDocument(){ 
   T doc=default(T);   //值类型: doc=0 引用类型 doc=null;
   lock(this){
     doc=documentQuene.Dequene();
   }
   return doc;
  }
```
##### 泛型继承
`1.泛型类可以派生自泛型基类`
```C#
   public class Father<T>
   {
       //etc
   }
  
   public class Son<T>:Father<T>
   {
      //etc
   }
```
`2.或者派生自父类的一个类型`
```C#
   public class Father<T>
   {
       //etc
   }
  
   public class Son<T>:Father<string>
   {
      //etc
   }
```
`3.泛型类也可以是抽象的`
##### 静态成员
`泛型类的静态成员需要特别关注.泛型类的静态成员只能在类的一个实例中共享`

```C#
   public class StaticDemo<T>
   {
       public static int x;   
   }
   
   StaticDemo<int>.x=4;
   StaticDemo<string>.x=8;
   WriteLine(StaticDemo<int>.x); //输出 4
```


#### <a id="GenericClassYueShu">泛型约束</a> <a href="#top">置顶  :fountain:</a>
`有时候我们需要泛型的类型实现某个接口或者是某一特定类型,具有什么方法`  

|约束|说明|
|:----|:-----|
|where T:struct|对于结构约束,类型T必须是值类型  `struct`必须写在前面 |
|where T:class|类约束要求类型T必须是引用类型 `class`必须写在前面|
|where T:IFoo|要求类型T必须实现接口IFoo|
|where T:Foo|要求类型T必须派生自基类Foo|
|where T:new|这是构造函数约束,制定类型T必须有一个默认构造函数|
|where T:T2|这个约束也可以制定要求T1派生自T2|

```C#
using System;
using System.Collections;
using System.Collections.Generic;
namespace DotnetConsole
{
    public class MyClass<T1,T2>
    where T1:IComparable,IEnumerable 
    where T2:class,IEnumerable  //class ，struct 必须写在前面
    {
        //etc
    }       
}
```

#### <a id="GenericInnerface">泛型接口</a> <a href="#top">置顶  :fountain:</a>
`使用泛型可以定义接口，接口中的泛型方法可以带泛型参数`
```C#
  public interface IComparable<in T>{
     int CompareTo(T other);
  }
```
`C#4.0之前泛型接口是不变的,.NET4.0通过协变抗变,和泛型委托添加了一个重要的扩展,协变和抗变指对参数和返回值的类型进行转换,例如,可以给Father类参数的方法中传递Son吗？下面用实例说明这些扩展的优点。`

* `泛型接口的协变`:`如果泛型类型用out关键字标注,那么泛型接口就是协变的,泛型类型T必须是返回值`
* `泛型接口的抗变`:`如果泛型类型用in关键字标注,那么泛型接口就是抗变的,泛型类型T只是接受参数,不是返回值`


#### <a id="GenericStruct">泛型结构</a> <a href="#top">置顶  :fountain:</a>
`结构体也可以是泛型的,和泛型类相似,这里介绍一个泛型结构体` **`Nullable<T>`**  `int? 和数据库中的int是可以为null的，为了更好的映射数据库的字段提供这个类型，C#中的int不可以为null`
```C#
#region 程序集 System.Runtime, Version=4.2.1.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a
// C:\Program Files\dotnet\sdk\NuGetFallbackFolder\microsoft.netcore.app\2.1.0\ref\netcoreapp2.1\System.Runtime.dll
#endregion

namespace System
{
    //
    // 摘要:
    //     Represents a value type that can be assigned null.
    //
    // 类型参数:
    //   T:
    //     The underlying value type of the System.Nullable`1 generic type.
    public struct Nullable<T> where T : struct
    {
        //
        // 摘要:
        //     Initializes a new instance of the System.Nullable`1 structure to the specified
        //     value.
        //
        // 参数:
        //   value:
        //     A value type.
        public Nullable(T value);

        //
        // 摘要:
        //     Gets a value indicating whether the current System.Nullable`1 object has a valid
        //     value of its underlying type.
        //
        // 返回结果:
        //     true if the current System.Nullable`1 object has a value; false if the current
        //     System.Nullable`1 object has no value.
        public bool HasValue { get; }
        //
        // 摘要:
        //     Gets the value of the current System.Nullable`1 object if it has been assigned
        //     a valid underlying value.
        //
        // 返回结果:
        //     The value of the current System.Nullable`1 object if the System.Nullable`1.HasValue
        //     property is true. An exception is thrown if the System.Nullable`1.HasValue property
        //     is false.
        //
        // 异常:
        //   T:System.InvalidOperationException:
        //     The System.Nullable`1.HasValue property is false.
        public T Value { get; }

        //
        // 摘要:
        //     Indicates whether the current System.Nullable`1 object is equal to a specified
        //     object.
        //
        // 参数:
        //   other:
        //     An object.
        //
        // 返回结果:
        //     true if the other parameter is equal to the current System.Nullable`1 object;
        //     otherwise, false. This table describes how equality is defined for the compared
        //     values: Return Value Description true The System.Nullable`1.HasValue property
        //     is false, and the other parameter is null. That is, two null values are equal
        //     by definition. -or- The System.Nullable`1.HasValue property is true, and the
        //     value returned by the System.Nullable`1.Value property is equal to the other
        //     parameter. false The System.Nullable`1.HasValue property for the current System.Nullable`1
        //     structure is true, and the other parameter is null. -or- The System.Nullable`1.HasValue
        //     property for the current System.Nullable`1 structure is false, and the other
        //     parameter is not null. -or- The System.Nullable`1.HasValue property for the current
        //     System.Nullable`1 structure is true, and the value returned by the System.Nullable`1.Value
        //     property is not equal to the other parameter.
        public override bool Equals(object other);
        //
        // 摘要:
        //     Retrieves the hash code of the object returned by the System.Nullable`1.Value
        //     property.
        //
        // 返回结果:
        //     The hash code of the object returned by the System.Nullable`1.Value property
        //     if the System.Nullable`1.HasValue property is true, or zero if the System.Nullable`1.HasValue
        //     property is false.
        public override int GetHashCode();
        //
        // 摘要:
        //     Retrieves the value of the current System.Nullable`1 object, or the object&#39;s
        //     default value.
        //
        // 返回结果:
        //     The value of the System.Nullable`1.Value property if the System.Nullable`1.HasValue
        //     property is true; otherwise, the default value of the current System.Nullable`1
        //     object. The type of the default value is the type argument of the current System.Nullable`1
        //     object, and the value of the default value consists solely of binary zeroes.
        public T GetValueOrDefault();
        //
        // 摘要:
        //     Retrieves the value of the current System.Nullable`1 object, or the specified
        //     default value.
        //
        // 参数:
        //   defaultValue:
        //     A value to return if the System.Nullable`1.HasValue property is false.
        //
        // 返回结果:
        //     The value of the System.Nullable`1.Value property if the System.Nullable`1.HasValue
        //     property is true; otherwise, the defaultValue parameter.
        public T GetValueOrDefault(T defaultValue);
        //
        // 摘要:
        //     Returns the text representation of the value of the current System.Nullable`1
        //     object.
        //
        // 返回结果:
        //     The text representation of the value of the current System.Nullable`1 object
        //     if the System.Nullable`1.HasValue property is true, or an empty string (&quot;&quot;)
        //     if the System.Nullable`1.HasValue property is false.
        public override string ToString();

        public static implicit operator T? (T value);
        public static explicit operator T(T? value);
    }
}
```

#### <a id="GenericFunction">泛型方法</a> <a href="#top">置顶  :fountain:</a>
* `泛型不能直接使用乘除加减方法需要转换到动态类型,跳过静态检查`
```C#
     public T Sum<T>(T one, T two)
     where T : struct, IComparable
     {
         dynamic v1 = one;
         dynamic v2 = two;
         return (T)(v1+v2);
     }
     
      //求和
      public static T Sum<T>(params T[] one)
      where T:struct,IComparable
      {
          dynamic sum=default(T);
          foreach(var val in one){
              dynamic v1 = val;
              sum=sum+v1;
          }
          return (T)(sum);
      }
```
