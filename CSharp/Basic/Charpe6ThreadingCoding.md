<a id="top" href="#top">:maple_leaf: C# 线程变成 :bird:</a> 
-----
`C# 多线程编程,线程有同步和异步,多线程,特别是C# 这个三个东西每一个都是一大块,慢慢学把;线程的概念 就某该了`

- [x] :mountain_bicyclist:	<a href="#DelegateThreading">**`通过委托使用多线程`**</a>

##### &nbsp;&nbsp; 通过委托使用多线程 <a id="DelegateThreading"></a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>

* `C#委托实现多线程 需要借助两个方法 BeginInvoke 和 EndInvoke`

   * `BeginInvoke(callbackFunction(IAsyncResult ar,Object obj))`
   * `BeginInvoke(param object[],callbackFunction(),Object obj)`
* `Object EndInvoke(IAsyncResult ar) 方法返回泛型方法执行的结果`

```C#
    static void Main(string[] args)
    {
        Console.WriteLine("Start World!");
        //给委托赋予一个方法
        Func<int,int> a = (val)=>
        {
            Console.WriteLine( $"input value:{val}");
            return val % 5;
        };
        //开始执行方法
        // 倒数第二个参数 为回调函数 有一个参数 IAsyncResult 为线程状态信号作为参数填充到 IAsyncResult的
        AsyncState 属性中
        IAsyncResult ar =a.BeginInvoke(93, null, null); 

        //主线程不断检测 委托线程的执行状态是否完成
        //
        while (ar.IsCompleted==false)
        {
            Thread.Sleep(10);
            Console.WriteLine("等待中...");
        }
        //执行完毕后 执行EndInvoke 传递信号量,得到返回值
        var reval = a.EndInvoke(ar);
        Console.WriteLine($"Value： {reval}");
        Console.WriteLine("End World!");
        Console.ReadKey();
    }
```
* `限制子线程的执行时间，超过时间则停止返回false`

```C#
      // 倒数第二个参数 为回调函数 有一个参数 IAsyncResult 为线程状态信号作为参数填充到 IAsyncResult的
      AsyncState 属性中
      IAsyncResult ar =a.BeginInvoke(93, null, null);
      Boolean isFinsih = ar.AsyncWaitHandle.WaitOne(1000);//超时时间,1000毫秒线程结束返回true否则返回false

      //如果已经完成了
      if (isFinsih)
      {   
          var vals = a.EndInvoke(ar);
          Console.WriteLine(vals);
      }
      Console.WriteLine("End World!");
      Console.ReadKey();
```

* `通过回调函数得到返回结果,和线程结束`

```C#
      static void Main(string[] args)
      {

          //给委托赋予一个方法
          Func<int,int> a = (val)=>
          {
              Console.WriteLine( $"input value:{val}");
              return val % 5;
          };
          //开始执行方法
          // 倒数第二个参数 为回调函数 有一个参数 IAsyncResult 为线程状态信号作为参数填充到 IAsyncResult的AsyncState 属性中
          a.BeginInvoke(93,(IAsyncResult er)=> {
              string val = er.AsyncState as String;
              var reval = a.EndInvoke(er);
              Console.WriteLine($" {val} Said Return Value:{reval}");

          }, "JiangXing");
          Console.WriteLine("Start World!");
          Console.WriteLine("End World!");
          Console.ReadKey();
      }
```
