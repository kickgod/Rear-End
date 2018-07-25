<a id="top">:checkered_flag: </a>委托,lambda,委托,事件 :blue_heart:
----
#### [`委托`](https://docs.microsoft.com/zh-cn/dotnet/csharp/delegates-events)
:fast_forward:`C# 委托是寻址方法的.NET版本.类似于C++的函数指针[一个指向内存的指针,没有类型安全可以报一个函数指针传递给任何需要函数指针的地方],但
是C#委托是类型安全的类,委托不仅包含对方法的引用,还包含对多个方法的引用.lambda表达式与委托直接相关,当参数是委托类型时,就可以通过lambda表达式实现委
托引用的方法.`:rewind:

**`作用`**:`向方法传递方法,给线程传递方法,配合事件处理`</br>
**`委托本质`**:`上是一个特殊类型的对象,特殊之处在与其他对象都包含数据,而委托包含的只是一个或者多个方法的地址,同时也是方法的一个签名`</br>

- [x] <a href="#delegateProduct">`声明委托 `</a>:sailboat:
- [x] <a href="#UserdelegateProduct">`使用委托 `</a>:sailboat:
- [x] <a href="#SystemDefinedelegateProduct">`系统预定义委托 `</a>:sailboat:
- [x] <a href="#MulticastCommission">`多播委托 `</a> :sailboat:
- [x] <a href="#AnonymousFunction">`匿名方法`</a> :sailboat:
-----
#### [`Lambda 表达式`](https://docs.microsoft.com/zh-cn/dotnet/csharp/lambda-expressions)
:fast_forward:`Lambda 表达式是作为对象处理的代码块（表达式或语句块）。 它可作为参数传递给方法，也可通过方法调用返回。 Lambda 表达式广泛用于：`:rewind:
* 将要执行的代码传递给异步方法，例如 Run(Action)。
* 编写 LINQ 查询表达式。
* 创建[表达式树](https://docs.microsoft.com/zh-cn/dotnet/csharp/expression-trees-building)。
* `Lambda 表达式使用 lambda 声明运算符 => `
* `Lambda 表达式使用类型推理`


- [x] <a href="#lambdaparameter">`lambda 参数 `</a> :sailboat:
- [x] <a href="#asyncLmabda"> `异步 lambda`</a> :sailboat:
- [x] <a href="#patattiontoLmabda"> `注意事项 `</a> :sailboat:

#### [`事件`](https://docs.microsoft.com/zh-cn/dotnet/csharp/events-overview)
:fast_forward:`和委托类似，事件是后期绑定机制。 实际上，事件是建立在对委托的语言支持之上的。事件是对象用于（向系统中的所有相关组件）广播已发生事情的一种方式。 任何其他组件都可以订阅事件，并在事件引发时得到通知。例如:鼠标移动、按钮点击和类似的交互通过订阅事件，还可在两个对象（事件源和事件接收器）之间创建耦合。 需要确保当不再对事件感兴趣时，事件接收器将从事件源取消订阅。`:rewind:

#### <a id="delegateProduct">1.1&nbsp;&nbsp;  `声明委托` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`要点`:`声明关键字 :delegate 编译器在你使用 delegate 关键字时生成的代码会映射到调用 Delegate 和 MulticastDelegate 类的成员的方法调用。` </br>
`要点`:`写出返回类型,参数类型与个数` </br>
```C#
  
  //声明一个委托  它包含的方法只能是  参数为int 返回值为 void
  
  public delegate void IntMethodInvoker(int x);
  
  //声明一个泛型委托,带两个参数,返回一个元组,参数类型为值类型并且按照引用传递
  public delegate Tuple<T1,T2> ProcessDate<T1,T2>(ref T1 val,ref T2 val2)
  // 考虑情况决定是否加 ref 否则使用lambda表达式的时候会有不方便 
  where T1:struct
  where T2:struct;
```
**`定义好委托后就可以创建它的一个实例,从而使用该实例存储特定的方法的细节`**

#### <a id="UserdelegateProduct">1.2&nbsp;&nbsp;  `使用委托` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`要点`:`委托的构造函数总是接受一个方法` </br>
`要点`:`委托调用方法的方式`:`委托对象名(参数)`,`委托对象名.invoke(参数)` </br>
`要点`:`委托可以引用任何的静态或者实例方法,只要与委托的方法签名匹配` </br>
```C#
    int x=40;
    DelegateGetString worker=new DelegateGetString(x.ToString); 
    //DelegateGetString worker=x.ToString; //也可以
    WriteLine($"委托执行: {worker()}"); //理解这里为什么 输出的 结果还是40
    
    IntMethodInvoker inv=new IntMethodInvoker(val=>WriteLine($"input:{val}"));
    inv(10);
    inv.Invoke(20);
```
`综合使用`
```C#
    //定义委托
    public delegate Tuple<T1,T2> ProcessDate<T1,T2>(T1 val,T2 val2) 
    where T1:class
    where T2:struct;
    
    //定义方法 把委托当做参数
    public static Tuple<String,int> ProcessDate(String name,int Age,ProcessDate<String,int> Show){
        return Show(ref name,ref Age);
    }
    //调用方法
    var data=ProcessDate("JiangXing",35,(name,age)=>{
      return Tuple.Create(name,age); 
    });

    Console.WriteLine($"name:{data.Item1} age:{data.Item2}");
    
```

#### <a id="SystemDefinedelegateProduct">1.3&nbsp;&nbsp;  `系统预定义委托` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`系统定义了两个委托类型` **`Action` `Func`**

#####  **`Action`**
`只有动作没有返回值,返回值为void,只处理不返回 参数抗变  委托的变体可包含多达 16 个参数`
* `Action<in T1>(T1 val)`
* `Action<in T1,in T2>(T1 val1,T2,,val2)`
* `Action<in T1,in T2,in T3>(T1 val1,T2,,val2,T3 val3)`
* `...`
* `Action<in T1,in T2,in T3,.....,in T8>(T1 val1,T2,,val2,T3 val3....T8 val8)`
```C#
    public static void Process(Action<double,double> action,double val1,double val2){
        action(val1,val2);
    }

    Process((val1,val2)=> WriteLine($"val1:{val1},val2:{val2}"),50,60.22); 
    
```
#####  **`Func`**
`有动作又有返回值,返回值类型为最后一个泛型类型的类型,只处理不返回 参数协变 总共可以定义16个参数`

**`例如`** :`Func<out TResult> 返回值类型为 TResult` </br>
**`例如`** :`Func<in T1,out TResult> 返回值类型为 TResult` </br>

* `Func<out TResult>()`
* `Func<in T1,out TResult>(T1 val1)`
* `Func<in T1,in T2,out TResult>(T1 val1,T2,,val2)`
* `...`
* `Func<in T1,in T2,in T3,.....,out TResult>(T1 val1,T2,,val2,T3 val3....T16 val16)`

```C#
       public static String FunctionUsing(Func<Double,String> funcs){
            return funcs(56);
        }
        
        FunctionUsing( (Double val)=> {return val.ToString();});
```
#### <a id="MulticastCommission">1.4&nbsp;&nbsp;  `多播委托` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`前面每个委托都只包含一个方法,所以调用委托多少次与调用方法的次数相同.如果要调用多个方法,就需要多次显示调用这个委托,但是委托可以包含多个方法.这种委托被称为多播委托,如果调用多播委托,就可以按照顺序调用多个方法`,**`为此委托的签名就必须返回void,否则就只能的到最后一个方法的结果`**

##### 多播委托可以识别运算符 `+` `+=` 还识别运算符 `-`和`-=`
`为了显示存储多个方法所以实例化一个委托数组`
```C#
    static void Main(string[] args)
    {
        Action<Double> Multicast=Arth;
        Multicast+=GetResult;
        Multicast+=SetValue;
        Multicast.Invoke(20);

        Action<Double> Multi1=Arth;
        Action<Double> Multi2=GetResult;

        Action<Double> Multi3=Multi1+Multi2;
        Multi3.Invoke(30);

        Multi3-=Arth;
        Multi3.Invoke(30);
        /*
        Arth Function： 20
        GetResult Function： 40
        SetValue Function： 400
        Arth Function： 30
        GetResult Function： 50
        GetResult Function： 50
        */
    }

    public static void Arth(Double val){
        Console.WriteLine($"Arth Function： {val}");
    }   

    public static void GetResult(Double val){
        Console.WriteLine($"GetResult Function： {val+20}");
    }    


    public static void SetValue(Double val){
        Console.WriteLine($"SetValue Function： {val*val}");
    }    
}
```
##### 多播委托的问题
* `多播委托不适合按照顺序执行的多个方法一起执行`
* `通过一个委托调用多个方法还可以导致一个更加严重的问题.多播委托包含一个逐个调用的委托集合,但是当一个委托抛出异常,整个迭代执行就会停止`

#### <a id="AnonymousFunction">1.5&nbsp;&nbsp;  `匿名方法` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`C#一切代码都包含在类里面,这里未免有些麻烦,如何定义脱离类的方法,这里就要用到匿名方法 和 lambda表达式,这里介绍匿名类型`

* 使用 `delegate(参数)` 声明 可以用返回值
* `C# 3.0` 之后开始 使用`lambda表达式`
```C#
    Func<Double,Double> Multicast=delegate(double val){
            Console.WriteLine($"匿名方法有返回值: {val}");
            return val*10;
    };
    Action<Double> action=delegate(double val){
            Console.WriteLine($"匿名方法无返回参数: {val*10}");
    };
```
#### <a id="lambdaparameter">2.1&nbsp;&nbsp;  `lambda 参数` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>

##### 无参数lambda表达式
`无参数的lambda表达式必须要加括号做分割作用`
```C#
    public delegate void ShowInfomation();
    static void Main(string[] args)
    {
         ShowInfomation info= ()=> WriteLine("没有参数的表达式"); 
    }
    info();
```
##### () {} 大括号
* 无参数的lambda表达式必须加上() 
* 如果参数有参数类型说明必须加()
* 参数个数大于1时必须加上()
* 如果返回使用了return 必须加上{}包裹代码
* 如果代码超过一行使用 {}包裹代码
```C#
 ShowInfomation info= ()=> WriteLine("没有参数的表达式");  //没有返回值
 
 Func<Double,Double,Double> fsum=(valt,vals)=> vals+valt;
 
 Func<Double,Double,Double> fsums=(Double valt,Double vals)=> vals+valt; //自动推断出返回值 和返回类型
 
 Func<Double,Double,Double> fsum=(valt,vals)=>{
    vals+=10;
    valt=vals+valt;
    return vals+valt;
 }; 
```
##### 返回值
`lambda 如果处理数据极其简单的话可以不加return 语句实现返回`
```C#
  Func<Person,String> function = val=> val.ToString();
 
 // 编译器自动把 val.ToString() 的结果作为返回值
 // 如果加上 return 就必须加上大括号,因为这样语句不止一条
```
##### 一个或者多个参数的时候
* `一个参数的时候 可以加括号,也可以不加括号`
* `如果有参数类型说明的话必须加上括号,lambda表达式可以不需要参数说明,编译器会更具委托类型自动推断出参数的类型`

**`Func<Person,String> function =(Person val)=>val.ToString();`**
```C#
    Func<Person,String> function = val=>val.ToString();  
    useFun(new Person("WangliJun",18,"2016110418",false), val=> val.ToString());
    
    public static string useFun(Person son,Func<Person,String> function)
    {
           return function.Invoke(son);
    }
```

`将lambda表达式作为参数`
```C#
    static void Main(string[] args)
    {
        ShowValue(x=>x*x);
    }
    private static void ShowValue(Func<int,int> op)
    {
        for (int ctr = 1; ctr <= 5; ctr++)
            Console.WriteLine("{0} x {0} = {1}",ctr,op(ctr));
    }
```
#### <a id="asyncLmabda">2.2&nbsp;&nbsp;  `异步 lambda` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
通过使用 [async](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/async) 和 [await](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/await) 关键字，可以轻松创建包含异步处理的 Lambda 表达式和语句。 例如，本示例调用异步执行的 ShowSquares 方法
```C#
using System;
using System.Threading.Tasks;

public class Example
{
   public static void Main()
   {
      Begin().Wait();
   }

   private static async Task Begin()
   {
      for (int ctr = 2; ctr <= 5; ctr++) {
         var result = await ShowSquares(ctr);
         Console.WriteLine("{0} * {0} = {1}", ctr, result);
      }
   }

   private static async Task<int>  ShowSquares(int number)
   {
         return await Task.Factory.StartNew( x => (int)x * (int)x, number);
   } 
}
```
#### <a id="patattiontoLmabda">2.3&nbsp;&nbsp;  `注意事项` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
* `捕获的变量将不会被作为垃圾回收，直至引用变量的委托符合垃圾回收的条件。`

* `在外部方法中看不到 lambda 表达式内引入的变量。`

* `Lambda 表达式无法从封闭方法中直接捕获 in、ref 或 out 参数。`

* `Lambda 表达式中的返回语句不会导致封闭方法返回。`

* `如果跳转语句的目标在块外部，则 lambda 表达式不能包含位于 lambda 函数内部的 goto 语句、 break 语句或 continue 语句。 同样，如果目标在块内部，则在 lambda 函数块外部使用跳转语句也是错误的。`
















