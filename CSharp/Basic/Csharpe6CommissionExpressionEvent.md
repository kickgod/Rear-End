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





























