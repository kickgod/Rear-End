 :fountain: <a id="top">泛.型</a> :white_check_mark:
----
`这是C# 流式编程的开端,重要组成部分,没有泛型,集合将会失去强大的功能,流式编程将会无比艰难,泛型加上Lambda,集合,linq,正则表达式,扩展方法,匿名类型组成了C#的流式
编程。总而言之泛型很重要的`:closed_umbrella:

- [x] <a href="#FanXingLizi" > 泛型实例</a> :blue_book:

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
