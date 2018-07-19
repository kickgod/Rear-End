NET Core 命令行接口 (CLI) 工具
----
### `Dotnet Build` 
> 生成项目及其所有依赖项。
* dotnet build 命令将项目及其依赖项生成为一组二进制文件。 二进制文件包括中间语言 (IL) 文件（带 `.dll 扩展名`）和用于调试的符号文件（带 `.pdb 扩展名`）中的项目的代码。 生成`依赖项 JSON 文件 (\*.deps.json)`，该文件列出了`应用程序的依赖项`。 生成 `\*.runtimeconfig.json 文件`，该文件指定应用程序的共享运行时及其版本。
<br/>

* ![项目文件](/Image//Console.png)


* dotnet build 使用 MSBuild 生成项目，因此它支持并行生成和[增量生成](https://docs.microsoft.com/zh-cn/visualstudio/msbuild/incremental-builds)。
* 项目是否可执行由项目文件中的 <OutputType> [在项目文件中:*.csproj]属性决定。 以下示例显示生成可执行代码的项目：

```
<PropertyGroup>
  <OutputType>Exe</OutputType>
</PropertyGroup>
```
##### `从.NET Core 2.0 开始，无需运行 dotnet restore，因为它由需有还原的所有命令隐式运行，如 dotnet build 和 dotnet run。`

### `Dotnet clean` 
> 清除项目输出。
* dotnet clean 命令可清除上一个生成的输出。 它以 MSBuild 目标 的形式实现，以便在运行命令时对项目进行评估。 只会清除在生成过程中创建的输出。 中间 (obj) 和最终输出 (bin) 文件夹都会被清除。

### `Dotnet Run`
> 无需任何显式编译或启动命令即可运行源代码。
* 编译源码,生产输出程序,然后运行该程序,此命令既可以用于快速迭代开发也可以用于运行源分布式程序
* 此命令依赖于`dotnet build` 一边在启动程序前生产.NET程序集的源输入
* 输出文件被写入`bin`文件夹中,如果不存在则创建它,根据需要文件将会被覆盖,临时文件被写入到obj文件夹中。
* 也可以直接运行dll文件 `Dotnet  Console.dll` 

### `Dotnet test`
* dotnet test 命令用于执行给定项目中的单元测试。 dotnet test 命令启动为项目指定的测试运行程序控制台应用程序。 测试运行程序执行为单元测试框架（例如 MSTest、NUnit 或 xUnit）定义的测试，并报告每个测试是否成功。 测试运行程序和单元测试库打包为 NuGet 包并还原为该项目的普通依赖项。
* 测试项目使用普通 <PackageReference> 元素指定测试运行程序，如下方示例项目文件所示：
```xml
  <Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

   <ItemGroup>
    <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.0.0" />
    <PackageReference Include="xunit" Version="2.2.0" />
    <PackageReference Include="xunit.runner.visualstudio" Version="2.2.0" />
   </ItemGroup>
   </Project>

```

#### `添加离线引用包的方法:`
  
```xml
    <ItemGroup>
    <PackageReference Include="MyLibPack" Version="1.0.0" />
    </ItemGroup>
  
```

### `dotnet restore`
> 恢复项目的依赖项和工具。

#### 从 .Net Core 2.0 开始，当发出下列命令时，如有必要，将隐式运行 dotnet restore。
----
* dotnet new
* dotnet build
* dotnet build-server
* dotnet run
* dotnet test
* dotnet publish
* dotnet pack
----
#### 用户 添加离线本地nuget包 例如：自己写的类库
> 指定要在还原操作期间使用的 NuGet 包源。 这会替代 NuGet.config 文件中指定的所有源。 多次指定此选项可以提供多个源。
* -s|--source <SOURCE>
```xml
  将类库:ClassLib pack打包后的nuget包 引入到MyApp 项目中:
  先将 ClassLib 打包

  首先：MyApp.csproj 文件夹中添加
    <ItemGroup>
      <PackageReference Include="ClassLib" Version="1.0.0" />
    </ItemGroup>
  然后执行下面命令:
  G:\Acigela\DotnetConsole\MyApp> dotnet restore -s G:\Acigela\DotnetConsole\ClassLib\bin\Debug\
  
```
  
  
### `Dotnet pack`
> 将代码打包到 NuGet 包.
* dotnet pack 命令生成项目并创建 NuGet 包。 该命令的结果是一个 NuGet 包。 如果 --include-symbols 选项存在，则创建包含调试符号的另一个包。
* `常用于类库的打包`
