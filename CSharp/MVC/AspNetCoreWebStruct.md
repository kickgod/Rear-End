ASP.NET Core web结构解析
-----

#### 文件结构 `[2.1 Core版本]`
![文件结构](/Image/CoreWeb.png)
```C#
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
      .UseStartup<Startup>();
}
```
