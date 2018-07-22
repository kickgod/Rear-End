 :fountain: <a id="top">泛.型</a> :white_check_mark:
----
`这是C# 流式编程的开端,重要组成部分,没有泛型,集合将会失去强大的功能,流式编程将会无比艰难,泛型加上Lambda,集合,linq,正则表达式,扩展方法,匿名类型组成了C#的流式
编程。总而言之泛型很重要的`:closed_umbrella:

- [x] <a href="#FanXingLizi" > 泛型实例</a> :blue_book:

- [x] <a href="#GenericClassFucntion" > 泛型类的功能</a> :blue_book:

- [x] <a href="#GenericClassYueShu" > 泛型约束</a> :blue_book:

- [x] <a href="#GenericInnerface" > 泛型接口</a> :blue_book:


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
