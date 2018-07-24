<a id="top">:checkered_flag: </a>运算符和类型强制转换 :large_blue_diamond:	
----
:fast_forward:[`C#运算符`](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/),`和C，C++差不多这里只是说明一下一些C#特有的运算符,和
一些必须要掌握的运算符`:rewind:

- [x] <a href="#NullOprator">`空合并运算符`</a> :white_check_mark:

- [x] <a href="#UnCheckOprator">`Checked unChecked运算符`</a> :white_check_mark:

- [x] <a href="#SizeofTypeOfNameOfOperator">`sizeof typeof nameof运算符`</a> :white_check_mark:

- [x] <a href="#keKongtypeKongCHUngabo"> `可空类型和空值传播运算符`</a> :white_check_mark:

- [x] <a href="#ReferenceEquals"> `ReferenceEquals()方法`</a> :white_check_mark:

- [x] <a href="#ChongzaiOperator"> `运算符重载`</a> :white_check_mark:

:fast_forward:[`C#类型转换`](https://docs.microsoft.com/zh-cn/dotnet/standard/base-types/conversion-tables),`基本上分为显示和隐式转换
一般小类型变成大类型要好些`:rewind:

- [x] <a href="#SampleChange"> `普通转换`</a> :white_check_mark:

- [x] <a href="#ChangeDefineByUser"> `用户自定义类型强制转换`</a> :white_check_mark:

- [x] <a href="#QiangzhuanmanyTimes"> `多重类型强制转换`</a> :white_check_mark:

#### <a id="NullOprator">1.1&nbsp;&nbsp; :sailboat: 空合并运算符</a> <a href="#top"> `置顶` :arrow_up:</a>
**`??`**:`运算符称作 空合并运算符。 如果此运算符的左操作数不为 null，则此运算符将返回左操作数；否则返回右操作数。`

```C#
    int? x = null;
    int y = x ?? 110;
    
    // 以前是三元运算符  var y= x==null? 110:x;
    String Name=null ?? "this is not Null value";

    WriteLine($"y:{y} Name:{Name}");  //输出: y:110 Name:this is not Null value
```
#### <a id="UnCheckOprator">1.2&nbsp;&nbsp; :sailboat: Checked unChecked运算符</a> <a href="#top"> `置顶` :arrow_up:</a>
`C#中有时候会发生数据溢出,例如乘法越界溢出后,已经无法表示,但是不会报错,这里为了检查出溢出错误,C#提供了这两个运算符`
* 如果溢出会引发`System.OverflowException:` 异常,对于一些高要求的计算一定尽力使用checked运算符检查运算正确性,及时发现捕获错误禁止检查
使用`unChecked{ //etc }` 即可;

```C#
    // 溢出
    byte i=255;
    i++;
    WriteLine($"sum:{i}"); //输出: sum:0
    这里我们加入check运算符检查错误
    byte j=255;
    checked{
      j++;
    }
    // 引发:System.OverflowException:
    WriteLine($"sum:{j}"); //输出: ...   
```
#### <a id="SizeofTypeOfNameOfOperator">1.3&nbsp;&nbsp; :sailboat: sizeof typeof nameof运算符</a> <a href="#top"> `置顶` :arrow_up:</a>
##### sizeof 
`可以确定栈中值类型需要的长度,自定义的对象就免了`
```C#
  WriteLine(sizeof(int)); // 4
```
##### typeof  
`返回一个表示特定类型的System.Type对象`
```C#
  WriteLine($"Type:{typeof(String)}"); //Type:System.String
```
##### nameof  运算符
`C#6.0以后的运算符,接受一个符号,方法类返回它的名称`
```C#
  WriteLine($"name:{nameof(String.Concat)}"); //name:Concat
```
#### <a id="keKongtypeKongCHUngabo">1.4&nbsp;&nbsp; :sailboat: 可空类型和空值传播运算符</a> <a href="#top"> `置顶` :arrow_up:</a>
`可空类型:int?  空值传播运算符:String name=son?.Name 很重要`

#### <a id="ReferenceEquals">1.5&nbsp;&nbsp; :sailboat: ReferenceEquals()方法</a> <a href="#top"> `置顶` :arrow_up:</a>
`ReferenceEquals是Object提供的一个静态方法,测试两个引用是否指向类的同一个实例`
```C#
  Person p1=new Person("Jx",19,"2016110418",true);
  Person p2=new Person("Jx",19,"2016110418",true);
  
  WriteLine(p1.Equals(p2));  //返回 true
  WriteLine(Object.ReferenceEquals(p1,p2)); // 返回 

```
#### <a id="ChongzaiOperator">1.6&nbsp;&nbsp; :sailboat: 运算符重载</a> <a href="#top"> `置顶` :arrow_up:</a>
`运算符重载的关键是在对象上不能总调用方法和属性,有时还需要做一些其他工作,例如对数值进行相加,相乘,逻辑操作 +-*/ 本质上是方法名称`

```C#
  Matrix a,b,c;
  // 初始化
  Matrix d=a*(a+b); //进行矩阵乘除法 
  
  //如果不适用运算符重载的话
  Matrix d=a.Muitiply(a.Add(b));
```
* 运算符重载都是静态的 方法 必须适应 operator 声明 `public static return_type operator +(obj1,obj2)`

`运算符重载`
```C#
using System;
using System.Text;

namespace DotnetConsole
{
   class Person{
      public Person(String Name,int Age,String Id,Boolean Sex){
          this.UserName=Name;
          this.Age=Age;
          this.Sex=Sex;
          this.UserId=Id;
      }
      public static Boolean operator ==(Person one,Person two){
          return one.Equals(two);
      }
      public static Boolean operator !=(Person one,Person two){
          return !one.Equals(two);
      }
      public String UserName{get;set;}
    
      public int Age{get;set;}

      public String UserId{get;set;}

      public Boolean Sex{get;set;}=true;
      
      public override String ToString(){
         return $"Name: {this.UserName} Age:{this.Age} 用户ID:{this.UserId} ";
      }
      public virtual int Wang(int a){
          return a*a;
      }
      public override Boolean Equals(object obj){
          Person one=obj as Person;
          if(one==null){
              return false;
          }else{
                if(this.UserName==one.UserName&&
                this.Age==one.Age&&
                this.Sex==one.Sex&&
                this.UserId==one.UserId){
                    return true;
                }else{
                    return false;
                }
          }
      }
  }
}
```
`结构体重载`
```C#
using System;
using System.Text;
using System.Collections.Generic;
using System.Collections;
using System.Threading;
using System.Runtime;
namespace DotnetConsole
{
    public struct Vector
    {   
         public double x;
         public double y;
         public double z;
         public Vector(double x,double y,double z){
             this.x=x;
             this.y=y;
             this.z=z;
         }    
        public static Vector operator + (Vector left,Vector right){
            Vector nv=new Vector(left.x+right.x,left.y+right.y,left.z+right.z);
            return nv;
        }
        public static Vector operator - (Vector left,Vector right){
            Vector nv=new Vector(left.x-right.x,left.y-right.y,left.z-right.z);
            return nv;
        }
        public override String ToString(){
            return $"(x,y,z):({x},{y},{z})";
        }
    }
}
      Vector v1=new Vector(1,2,3.0);
      Vector v2=new Vector(4,1,2);
      Vector v3=v1+v2;
      Vector v4=v1-v2;
      WriteLine(v3); //(x,y,z):(5,3,5)
      WriteLine(v4); //(x,y,z):(-3,1,1)
          
```
##### [可以重载的运算符](https://docs.microsoft.com/zh-cn/dotnet/csharp/programming-guide/statements-expressions-operators/overloadable-operators)

|运算符|可重载性|
|:----|:------|
|+、-、!、~、++、--、true、false|	`这些一元运算符可以进行重载。`|
|+, -, \*, /, %, &, |, ^, <<, >> |	`这些二元运算符可以进行重载。`|
|==, !=, <, >, <=, >=	|比较运算符可以进行重载（但是请参阅此表后面的备注）。|
|&&, \|\|	|条件逻辑运算符无法进行重载，但是它们使用 & 和 |（可以进行重载）来计算。|
|[]	|数组索引运算符无法进行重载，但是可以定义索引器。|
|(T)x	|强制转换运算符无法进行重载，但是可以定义新转换运算符（请参阅 explicit 和 implicit）。|
|+=, -=, \*=, /=, %=, &=, |=, ^=, <<=, >>= |	赋值运算符无法进行重载，但是 +=（举例）使用 +（可以进行重载）来计算。|
|=、.、?:、??、->、=>、f(x)、as、checked、unchecked、default、delegate、is、new、sizeof、typeof|	这些运算符无法进行重载。|

----

#### <a id="SampleChange">2.1&nbsp;&nbsp; 	:closed_umbrella: 普通转换</a> <a href="#top"> `置顶` :arrow_up:</a>

* `1.辅助类`:`Convert 提供多种转换方式 用于值类型` 
* `2.运算符`:`as 运算符转换不成功不报错,直接返回null 但是只能用于引用类型`
```C#
  //1. 普通转换
  int i =3;
  long j=i;  //隐式转换
  short s=(short)i;  //显示强制类型转换
  
  //辅助类帮助转换 值类型
  DateTime no=Convert.ToDateTime("2018/5/6"); 
  
  //辅助运算符帮助转换 引用类型
  Student st=new Student("Wang",12,"2016110418",true);
  Person on=st as Person;
  Console.WriteLine(on.ToString());
```
#### <a id="ChangeDefineByUser">2.1&nbsp;&nbsp; :closed_umbrella: 用户自定义类型强制转换</a> <a href="#top"> `置顶` :arrow_up:</a>
`用户自定义类型转换比如讲二维坐标转换为三维坐标，各个类之间的转换接口,这里面有两个关键字`
 
 * `实现用户自定义隐式转换--implicit` :`关键字用于声明隐式的用户定义类型转换运算符`

 ```C#
 class Digit
{
    public Digit(double d) { val = d; }
    public double val;
    // 实现
    public static implicit operator double(Digit d)
    {
        return d.val;
    }
    public static implicit operator Digit(double d)
    {
        return new Digit(d);
    }
}
class Program
{
    static void Main(string[] args)
    {
        Digit dig = new Digit(7);
        double num = dig;
        Digit dig2 = 12;
        Console.WriteLine("num = {0} dig2 = {1}", num, dig2.val);
        Console.ReadLine();
    }
}
```
* `实现用户自定义显式转换--explicit` :`关键字用于声明隐式的用户定义类型转换运算符`
```C#
class Celsius
{
    public Celsius(float temp)
    {
        degrees = temp;
    }
    public static explicit operator Fahrenheit(Celsius c)
    {
        return new Fahrenheit((9.0f / 5.0f) * c.degrees + 32);
    }
    public float Degrees
    {
        get { return degrees; }
    }
    private float degrees;
}

class Fahrenheit
{
    public Fahrenheit(float temp)
    {
        degrees = temp;
    }
    public static explicit operator Celsius(Fahrenheit fahr)
    {
        return new Celsius((5.0f / 9.0f) * (fahr.degrees - 32));
    }
    public float Degrees
    {
        get { return degrees; }
    }
    private float degrees;
}

class MainClass
{
    static void Main()
    {
        Fahrenheit fahr = new Fahrenheit(100.0f);
        
        Console.Write("{0} Fahrenheit", fahr.Degrees);
        Celsius c = (Celsius)fahr;

        Console.Write(" = {0} Celsius", c.Degrees);
        
        Fahrenheit fahr2 = (Fahrenheit)c;
        
        Console.WriteLine(" = {0} Fahrenheit", fahr2.Degrees);
    }
}
```
##### 基类不能转换成派生类,及时要转换需要使用 explicit 或者  implicit
##### 派生类可以转换为基类,因为这个基类有的，派生类都有

#### <a id="QiangzhuanmanyTimes">2.1&nbsp;&nbsp; :closed_umbrella: 多重类型强制转换</a> <a href="#top"> `置顶` :arrow_up:</a>
`如果要进行要求的数据类型转换时没有游泳的直接强制转换方式,C#编译器就会寻找一种转换方式,把几种强制转换合并起来例如:`

```C#
 var balance=new Currency(10,50);
 long amount=(long)(float)balance;
 double amountD=(double)(float)balance; 
 //有时候如果不写明转换路径 就会发生编译器自动寻找转换路径,导致信息缺陷
 //balance 只定义了转换为float
```
