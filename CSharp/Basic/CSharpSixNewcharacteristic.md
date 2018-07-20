C#6.0 语言新特性
----
#### `1.静态 using 声明`
```C#
  using System;
  using System.Text;

  namespace DotnetConsole
  {
      using static System.Console; /*这样不需要Console.WriteLine 可以直接使用它的方法*/
      class Program
      {

          static void Main(string[] args)
          {

              WriteLine("没有实现吧!");
          }
      }
  }
```
----
#### `2.表达式体方法`
> `包含一个可以用lambda语法编写的语句`
```C#
    /*C# 5.0 判断是否是正数*/ 
    public bool IsRightNumber(int val){
        return val>0;
    }
    /*C# 6.0 判断是否是负数*/ 
    public bool IsNegativeNumber(int val)=> val<0; 
```
----
#### `3.表达式属性`
> 只有get 存取器的单行熟悉可以用Lambda语法编写
```C#
    /*C# 5.0*/ 
    public Boolean AgeIsRight{
        get{
            return IsRightNumber(Age);
        }
    }   
    public String userName{
        get{
            return "JxKicker";
        }
    }     
    /*C# 6.0*/
    public int Age;

    public Boolean AgeIsRight => IsRightNumber(Age);

    public String userName=>"JxKicker";
    
```
#### `4.自动实现的属性初始化器`
> 自动实现的熟悉可以用这个实现
```C#

  // C# 6.0
  public  Boolean sex{get;set;} = true; //默认为男
  
  //字段可以直接初始化
  public int age=18;
  
```
#### `5.只读的自动属性`
> 之C#6.0中 实现自动实现的熟悉
```C#
  //C# 5.0
  public readonly int _bookId;
  
  public int BookId{
     get{return _bookId;}
  }
  //C# 6.0
  public int BookId{get;}
```
#### `6.nameof 运算符`
> 得到类名称方法名称字段名称
```C#
  public static Boolean userIsRight(Person one){
      WriteLine("类名:"+nameof(one));
      WriteLine("名称:"+nameof(one.UserName));
      return true;
  }  
  /* 输出
     类名:one
     名称:UserName
  */
```
----
#### `7.空值传播运算符?`
> 简化了空值的检查
```C#
    static void Main(string[] args)
    {
        Person User_li=null;
        String name=User_li?.UserName; 
        
        /*这样可以防止空指针错误 如果User_li对象为null那么name就是null不会引起报错*/
        
        /*  String name=User_li.UserName; 不加? 会产生空指针错误 */ 
        
        WriteLine(name);

    }
```
#### `8.字符串插值`
> 以后再也不用 + 符号 拼接字符串了 使用`$` 加 `{}`
```C#
  class Person{


      public Person(String Name,int Age,String Id,Boolean Sex){
          this.UserName=Name;
          this.Age=Age;
          this.Sex=Sex;
          this.UserId=Id;
      }
      public String UserName{get;set;}
    
      public int Age{get;set;}

      public String UserId{get;set;}

      public Boolean Sex{get;set;}=true;
      
      public override String ToString(){
         // C# 5.0 return "Name："+UserName+" Age："+this.Age+" 用户ID: "+this.UserId;
         
         //C# 6.0
         return $"Name: {this.UserName} Age:{this.Age} 用户ID:{this.UserId} ";
      }
  }
```
#### `9.字典初始化器`
> 使用数组的方式 初始化字典类型
```
    var dictinary =new Dictionary<int,string>();
    dictinary.Add(1,"jxkicker");
    dictinary.Add(2,"wang");    

    /*C# 6.0 字典初始化器 */
    var dict6 =new Dictionary<int,string>(){
        [1]="Jxkicker",
        [7]="wang",
        [10]="songli"
    };
```
----
#### `10.异常过滤器`
> then 关键字 过滤异常
```C#
    /*C# 5.0*/
    try{
        throw new MyException(404);
    }catch(MyException ex){
        if(ex.ErrorCode==404){
            WriteLine("网页跳转错误");
        }
    }
    
    /*C# 6.0*/
    try{
        throw new MyException(404);
    }catch(MyException ex) when (ex.ErrorCode==404){
        WriteLine("网页跳转错误");                
    }
```
----
#### `11.Catch 中的await`
> 
```C#
    try{
        throw new MyException(404);
    }catch(MyException ex) when (ex.ErrorCode==404){
       await new MessageDialog.ShowAsync("网页跳转错误");                
    }
```
