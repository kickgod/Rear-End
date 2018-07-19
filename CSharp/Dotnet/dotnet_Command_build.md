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
