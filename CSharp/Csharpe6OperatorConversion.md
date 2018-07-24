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











