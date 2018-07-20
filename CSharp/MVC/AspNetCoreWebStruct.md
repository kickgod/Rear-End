ASP.NET Core web结构解析
-----
#### [Kestrel服务器](https://www.cnblogs.com/Wddpct/p/6123653.html)
> `Kestrel`是一个基于libuv的跨平台ASP.NET Core web服务器，libuv是一个跨平台的异步I/O库。ASP.NET Core模板项目使用Kestrel作为默认的web服务器。


#### 文件结构 `[2.1 Core版本]`
![文件结构](/Image/CoreWeb.png)
* `Startup` 类 可用来定义请求处理管道和配置应用需要的服务。Startup类必须是公开的(public)  而且需要包含下列方法 `ConfigureServices`和 `Configure`
     * **`ConfigureServices`** ：定义你的应用所使用的服务(ASP.NET MVC / Entity Framework Core / Identity) 
        ```
           服务:应用中用于通用调用的组件,服务通过依赖注入获取并使用.ASP.NET Core内置了一个简单的控制反转IOC容器
           默认支持构造器注入
        ```
     * **`Configure`** ：定义你的请求管道中的[中间件](https://baike.baidu.com/item/%E4%B8%AD%E9%97%B4%E4%BB%B6/452240?fr=aladdin)
        ```
          在ASP.NET Core中你可以使用中间件构建你的请求处理通道.ASP.NET Core中间件为一个HttpContent执行异步逻辑.
          然后按照顺序调研下一个中间件或者直接终止请求
        ```
* `Program` 类 Main方法 关于Web服务器设置

```C#
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
    /*
      创建一个Web程序宿主,
     */
}
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
      .UseStartup<Startup>();
      /*
        CreateDefaultBuilder 按照生成模式来创建WEB应用程序主机,生产器提供定义WEB服务器例如UseKestrel和启动类
        UseStartup的方法Kestrel 是一个庆幸Web服务器可以在IIS或者其他Web服务器上面运行
       */
}
```
-----
