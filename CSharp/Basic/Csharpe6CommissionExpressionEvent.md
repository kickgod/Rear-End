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
-----

#### <a id="delegateProduct">1.1&nbsp;&nbsp;  `声明委托` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`要点`:`声明关键字 :delegate` </br>
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
    WriteLine($"委托执行: {worker()}");
    
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
`只有动作没有返回值,返回值为void,只处理不返回 参数抗变 总共可以定义八个参数`
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
