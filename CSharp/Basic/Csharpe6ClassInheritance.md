## :notes:<a id="top">CSharp继承</a>  . :confetti_ball: 
`继承是一门面向对象语言的基础 ,大多数面向对象语言都采用的是单继承接口模式,接口也可以继承,摒弃了C++的杂交混种且及其复杂惹事儿设计得极其
垃圾并无终极父类的多继承方式.`

- [x] <a href="#ClassInhrite" >类的继承</a>

- [x] <a href="#Polymorphism" >多态性</a>

   - [x] <a href="#HiddenMethod">`隐藏方法`</a>
   - [x] <a href="#ChongzaiFngafa">`重载方法`</a>
   - [x] <a href="#abstract">`抽象类和抽象方法`</a>
   - [x] <a href="#sealed">密封类,密封方法</a>

- [x] <a href="#Interface" >接口</a>

- [x] <a href="#asis" >is和as运算符</a>

#### C#终极父类

```c#


namespace System
{

    public class Object
    {

        public Object();
        
        ~Object();
        
        public static bool Equals(Object objA, Object objB);
        
        public virtual bool Equals(Object obj);
        
        public static bool ReferenceEquals(Object objA, Object objB);
        
        public virtual int GetHashCode();
        
        public Type GetType();
        
        public virtual string ToString();
        
        protected Object MemberwiseClone();
    }
}
```



#### <a id="ClassInhrite">类的继承</a> <a href="#top" >`置顶` :arrow_up:</a>  

`类的继承采用的字符为` **`:`** 
```C#
 // MyDirivedClass 继承自 MyBaseClass
 
 class MyDirivedClass:MyBaseClass{
    //tec 
 }
```
`类派生接口也使用` `:` `中间使用` `,` `隔开`


```C#
 // MyDirivedClass 继承自 MyBaseClass 并且派生自接口 IInterFace,IInterface2
 
 class MyDirivedClass:MyBaseClass,IInterFace,IInterface2
 {
    //tec 
    
    //实现接口 IInterFace,IInterface2 的方法 .....
    
    // etc
 }
```
##### 虚方法,虚属性,虚类 
`使用 `virtual` 关键字修饰。把一个基类的方式声明为虚方法,就可以在派生类中重写这个方法而不会影响方法的调用 调用时去区分父类的方法还是子类的方法`
`Override 重写父类的方法或者属性`

---
```C#
using System;
namespace DotnetConsole
{
    using static System.Console;
    //父类
    public  class Father
    {
        protected String _name;
        
        //虚属性
        public virtual String Name{
            get{return $"Father:{_name}";}
            set{_name=value;}
        }
        //虚方法
        public virtual void Said(){
            WriteLine("Father Said:You are My Son");
        }

    }
    
    //子类
    public  class Son:Father
    {
        //重写父类的方法
        public override void Said(){
            WriteLine("Son Said:I am Your Son");
        }
        
        //重写属性
        public override String Name{
            get{return $"Son:{_name}";}
            set{_name=value;}
        }
    }
}


  static void Main(string[] args)
  {
      Father f=new Father("JiangXing");
      Son s=new Son("JiangZhiMing");

      WriteLine(f.Name);
      WriteLine(s.Name);
      f.Said();
      s.Said();
  }
  
  //输出结果:
    Father:JiangXing
    Son:JiangZhiMing
    Father Said:You are My Son
    Son Said:I am Your Son
  
```
#### <a id="Polymorphism">多态性</a> <a href="#top" >`置顶` :arrow_up:</a>  
`动态的定义调用的方法,而不是编译期间定义,编译器会创建一个虚拟方法表,其中列出可以在运行期间调用的方法,它根据运行期间的类型调用方法`

##### <a id="HiddenMethod">:one: 隐藏方法</a>  <a href="#top" >`置顶` :arrow_up:</a>  
`原因:如果签名相同的方法在基类和派生类中都进行了声明,但该方法没有分别声明virtual和override,派生类方法就会隐藏类方法`
`子类覆盖了父类的方法 隐藏方法应该尽量避免,使用虚方法解决这个问题`
```C#
    Father
    ....
    private int sonCount=10;

    public int getSon(){
        return sonCount;
    }  
    Son
    public int getSon(){
        return 0;
    }   
    Mian函数
    WriteLine($"Father：SonCount:{f.getSon()}");
    WriteLine($"Son:SonCount:{s.getSon()}");
    //输出结果
    Father：SonCount:10
    Son:SonCount:0
```
----
在子类中可以使用 **`base`** 调用父类的方法
```C#
    public int getSon(){
        return base.getSon();
    }    
```
##### <a id="ChongzaiFngafa">:two: 重载方法</a>  <a href="#top" >`置顶` :arrow_up:</a>  
`方法和同名存在,只要方法签名不同`
* 方法重载不能够根据返回值类型不同来重载
```C#
  public int sum(){
      return 0;
  }

  public int sum(int a){
      return a;
  }
  public int sum(int a,int b){
      return b+a;
  }

  public int sum(int a,int b,int c){
      return a+b+c;
  }

  public int sum(float a,float b){
      return (int)(a+b);
  }
  
  //下面的两个方法就会报错
  public int sum(float a,float b){
      return (int)(a+b);
  }

  public float sum(float a,float b){
      return a+b+c;
  }
  //但是如果将下面的方法改成 就不会报错了
   public float sum(float a,float b,float c=0){
      return a+b+c;
  }
```

##### <a id="abstract">:three: 抽象类和抽象方法</a>  <a href="#top" >`置顶` :arrow_up:</a>  
`抽象类不可以被实例化,只能通过派生类去继承在非抽象类的派生类中重写,抽象方法本身也是虚拟的，抽象类中不需要实现任何一个方法只需要声明就可以了`
* 非抽象的派生类要实现父抽象类的全部抽象成员.
* 在派生类中是用Override 重写 抽象类
```C#
//抽象类
using System;
namespace DotnetConsole
{
    using static System.Console;
    public abstract class Father
    {


        public abstract  String Name{get;set;}
        private int sonCount;
        public  Father(String name){
            this.Name=name;
        }
        public abstract void Said();
        public abstract int getSon();  
    }

}
//实现派生类
using System;
namespace DotnetConsole
{
    using static System.Console;
    public  class Son:Father
    {
        public override void Said(){
            WriteLine("Son Said:I am Your Son");
        }
        public string _name;
        public override String Name{
            get{return $"Son:{_name}";}
            set{_name=value;}
        }
        public Son(String name):base(name){
            
        }
        public override int getSon(){
            return 0;
        }    
    }

}
```

##### <a id="sealed">:four: 密封类,密封方法</a>  <a href="#top" >`置顶` :arrow_up:</a>  
`给类添加sealed修饰符号,就不允许创建该类的子类,密封一个方法,表示不能重写该方法`

----
```C#
using System;
using System.Text;

namespace DotnetConsole
{
   class Student:Person{
      public Student(String Name,int Age,String Id,Boolean Sex)
         :base(Name,Age, Id,Sex){
          
      }
      // 密封方法 : sealed override 
      public sealed override int Wang(int a){ 
          return a;
      }
  }
}
```

#### <a id="Interface">:blue_book: 接口</a>  <a href="#top" >`置顶` :arrow_up:</a>  
`接口是一门面向对象语言最重要的组成部分,依赖注入,接口式编程等等....总之很重要`

* 接口只包含`方法` `属性` `索引器` `事件` 
* 接口成员必须实现
* 接口可以继承
* 接口命名:前面必须是`I` 后面是名字
```C#

 public interface IDisposable(){
   void Dispose();
 }
 
 
```
##### 接口继承

```C#
   public interface ItransferBank:IBankCount{
      bool TransferTo(decimal d);
   }
```
#### <a id="asis">:blue_book: is和as运算符</a>  <a href="#top" >`置顶` :arrow_up:</a>  

* `is`:`判断对象是否是指定类型,满足条件返回 true否则返回 false`
* `as`:`将一种类型转换为另外一种类型,如果能转换就返回转换后的对象否则返回null`

```C#
   Son s=new Son("JiangZhiMing");
   if(s is Son){
       WriteLine($"{nameof(s)} 对象是Son类型");
   }

   Object list= s as Object;
    WriteLine($"{list.ToString()}");
```
   














