核心C#--类前基础部分
----
### C# 变量
> C#是一门纯面向对象的,本质上任何一种数据类型都是一个对象不想C++,Java 有值类型,引用类型 
##### 常量 `const`
  * 变量前面加一个const 关键字修饰 就是一个常量了
  * `要求`:`声明的时候必须初始化,指定值后不能再次改变`
  * `要求`:`不能从变量中提取的值来初始化常量`
  * `说明`:`常量本身就是静态的 不需要static 修饰`
  * `说明`:`在编译的时候就需要有确定的值，只能用于数值和字符串 不能用于引用类型`
##### 推断类型 `var`
  * var关键字,用来声明并初始化局部变量。编译器根据=右边的语句推断出变量实际的类型。
  * `要求`:`变量一旦声明必须要初始化,初始化不能呢为null`
  * `要求`:`一旦推到就无法更改类型`
  * `要求`:`初始化器必须放到表达式中`
  ```C#
     var name="Tom Cheng";
     Type nameType=name.GetType();
     WriteLine($"Type: {nameType}");
     
     //输出:Type: System.String
  ```
##### 数据类型
   * 值类型 ：直接存储其值 存储在堆栈中
     
   * 引用类型 ：托管到堆栈中 gc会顶起删除不能访问不再使用的对象,进行垃圾回收释放占用内存
##### 变量的初始化问题
  * 避免空指针异常 例如：
  ```C#
   Person li=new;
   
   String name=li.UserName; //这样的错误经常犯,有时候代码相隔太远,或者是忘了判断 就会导致空指针异常
  ```
###### 建议:多使用空值传播运算符使得代码更加简单: name=li?.UserName; 对于int数值类型 也可以使用可null类型 int age?=li>.Age; 
#### 数据类型
##### 整形
> `C#有八种整数` `关键:u加上就是有符号 不加就是无符号 不管sbyte 一般都用int 偶尔用 long 和short 而byte很少用`
-----

|名称|类型|说明|
|-|-|-|
|`sbyte`|System.SByte|8位有符号整数|
|`short`|System.Int16|32位有符号整数|
|`int`|System.Int32|32位有符号整数|
|`long`|System.Int64|64位有符号整数|
|`byte`|System.Int8|8位无符号整数|
|`ushort`|System.UInt16|16位无符号整数|
|`uint`|System.UInt32|32位无符号整数|
|`ulong`|System.UInt64|64位无符号整数|
##### 浮点类型
> `这数值是四舍五入估计的值并不精确 精确度取决于小数位数`
----
|名称|类型|说明|小树位数|
|-|-|-|-|
|`float`|System.Single|32位单精度浮点数|7|
|`double`|System.Double|64位单精度浮点数|15/16|

##### decimal类型 
> 精度更高的浮点数
----
|名称|类型|说明|小树位数|
|-|-|-|-|
|`Decimal`|System.Decimal|128位高精度十进制浮点数|28|
###### 备注:如果制定数字为decimal那么 要如此: decimal d=12.333M; 数字后面要嫁一个M
-----
##### bool类型
> 包含true 或者 false
------
|名称|类型|说明|值|
|-|-|-|-|
|`bool`|System.Boolean|表示true或者false|true/false|

##### char类型
> 保存单个字符
------
|名称|类型|说明|
|-|-|-|
|`char`|System.Char|16位的Unicode字符|
-----
##### 转义字符
> 在字符串前面加一个@就可以避免使用转义符 string con=@"\n 是这个意思?";    结果原样输出
----
|名称|说明|
|-|-|
|` \'`|单引号|
|` \n`|换行符|
|` \"`|双引号|
|` \0`|空|
|` \r`|回车|
|` \\`|反斜线|
##### 引用类型
> c#有两种引用类型String和Object 一切类都直接或者简介继承自Object
---

|名称|类型|说明|
|-|-|-|
|`object`|System.Object|根类型,一切类的父类|
|`string`|System.String|Unicode字符串|

* Object类中有许多一般用途的基本方法：`Equals()`,`GetHashCode()`,`GetType()` `ToString()`

### C# 流程控制
##### `条件语句` 
> if else else if 

```C#
   decimal d=13.33M;
   if(d>(decimal)23.33) {
       WriteLine("d大于23.33");
   }else if(d==(decimal)13.33){
       WriteLine("d等于13.33");
   }else{
       WriteLine("d小于13.33");
   }
```
##### `switch case`
> C# 每一个case只要有语句结束都要加上break
```C#
      char a='A';
      switch(a){
          case 'A':{
               WriteLine("A");
          }break;
          case 'B':{
               WriteLine("B");
          }break;
          case 'C':
          case 'D':
          case 'E':{
               WriteLine("C~E");     
          }break;
      }
```
##### 循环 `while`,`for` `do while` `foreach`
> foreach 是最厉害循环利用迭代的思想
-----
```C#
   int[] ary=new int[]{1,2,3,4,5,6,7,8,9,10};

   foreach(var val in ary){
       System.Console.WriteLine(val);
   } 

```
##### 跳转语句 `break`,`Lable goto`,`continue`,`return`;
* `continue,break`：`常常使用在循环中`

### 枚举
> `enums`枚举是值类型，数据直接存储在栈中，而不是使用引用和真实数据的隔离方式来存储。本质上是证书类型
* 枚举可以确保变量制定合法的期望的值
* 使得代码更加清晰,允许用描述性的名称表示整数
* 枚举使得代码更加易于输入
----
```C#

    /* enum 是和类平级的不要写在类里面 */
    enum MachineState
    {
        PowerOff = 0,
        Running = 5,
        Sleeping = 10,
        Hibernating = Sleeping + 5
    }
    
    MachineState stat=MachineState.PowerOff;
```
### 注释
* C#使用 `//` 来实现单行添加注释
* `/* */` 多行注释
* `///` xml文档注释 
##### C#引入了新的XML注释，即我们在某个函数前新起一行，输入///，VS.Net会自动增加XML格式的注释，这里整理一下可用的XML注释。
```xml

XML注释分为一级注释（Primary Tags）和二级注释（Secondary Tags），前者可以单独存在，后者必须包含在一级注释内部。

I 一级注释

1. <remarks>对类型进行描述，功能类似<summary>，据说建议使用<remarks>;
 
2. <summary>对共有类型的类、方法、属性或字段进行注释；
 
3. <value>主要用于属性的注释，表示属性的制的含义，可以配合<summary>使用；
 
4. <param>用于对方法的参数进行说明，格式：<param name="param_name">value</param>；
 
5. <returns>用于定义方法的返回值，对于一个方法，输入///后，会自动添加<summary>、<param>列表和<returns>；
 
6. <exception>定义可能抛出的异常，格式：<exception cref="IDNotFoundException">；
 
7. <example>用于给出如何使用某个方法、属性或者字段的使用方法；
 
8. <permission>涉及方法的访问许可；
 
9. <seealso>用于参考某个其它的东东:)，也可以通过cref设置属性；
 
10. <include>用于指示外部的XML注释；
 
II 二级注释
 
1. <c> or <code>主要用于加入代码段；
 
2. <para>的作用类似HTML中的<p>标记符，就是分段；
 
3. <pararef>用于引用某个参数；
 
4. <see>的作用类似<seealso>，可以指示其它的方法；
 
5. <list>用于生成一个列表；
 
另外，还可以自定义XML标签 
 
```
----
### [预处理指令](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/preprocessor-directives/index)
#### #define #undef
* `#define`:使用 #define 来定义符号。 将符号用作传递给 #if 指令的表达式时，该表达式的计算结果为 true，如以下示例所示：
* `#undef`: 允许你定义一个符号，这样一来，通过将该符号用作 #if 指令中的表达式，表达式将计算为 false。
```C#
#undef DEBUG  
using System;  
class MyClass   
{  
    static void Main()   
    {  
        #if DEBUG   
            Console.WriteLine("DEBUG is defined");  
        #else  
            Console.WriteLine("DEBUG is not defined");  
        #endif  
    }  
}  
```
#### #if #else #elif #endif
* 类似于if else else if endif 表示结束
#### #region // #endregine
