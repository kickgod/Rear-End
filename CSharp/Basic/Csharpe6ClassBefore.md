核心C#--类前基础部分
----
#### C# 变量
> C#是一门纯面向对象的,本质上任何一种数据类型都是一个对象不想C++,Java 有值类型,引用类型 
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
##### 变量的初始化问题
  * 避免空指针异常 例如：
  ```C#
   Person li=new;
   
   String name=li.UserName; //这样的错误经常犯,有时候代码相隔太远,或者是忘了判断 就会导致空指针异常
  ```
###### 建议:多使用空值传播运算符使得代码更加简单: name=li?.UserName; 对于int数值类型 也可以使用可null类型 int age?=li>.Age; 
