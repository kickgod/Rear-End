## :notes:<a id="top">CSharp继承</a>  . :confetti_ball: 
`继承是一门面向对象语言的基础 ,大多数面向对象语言都采用的是单继承接口模式,接口也可以继承,摒弃了C++的杂交混种且及其复杂惹事儿设计得极其
垃圾并无终极父类的多继承方式.`

- [x] <a href="#ClassInhrite" >类的继承</a>

- [ ] <a href="#Polymorphism" >多态性</a>

   - [x] <a href="#HiddenMethod">`隐藏方法`</a>

- [ ] <a href="#ClassInhrite" >类的继承</a>

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

##### <a href="#HiddenMethod">:one: 隐藏方法</a>  <a href="#top" >`置顶` :arrow_up:</a>  
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
在子类中可以使用base调用父类的方法
```C#
    public int getSon(){
        return base.getSon();
    }    
```
    
