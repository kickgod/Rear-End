.NET Core 命令行接口 (CLI) 工具 
----
`NET Core 命令行接口 (CLI) 工具是用于开发 .NET 应用程序的新型跨平台工具链。 CLI 是更高级别的工具（如集成开发环境 (IDE)、编辑器和生成协调程序）
可以驻留的基础。`[`安装指南`](https://www.microsoft.com/net/learn/get-started/windows)

* `在默认情况下，SLI 以并行 (SxS) 方式安装，因此，因此，CLI 工具的多个版本可以在一个计算机上共存`
----
命令结构：CLI 命令结构包含驱动程序（**`dotnet`**）、`命令`（或 **`谓词`**）<br/>
驱动程序名为 dotnet，并具有两项职责，即 `1.运行依赖于框架的应用`或`2.执行命令` 唯一一次在不使用命令的情况下使用 dotnet 是在将其用于启动应用程序时。<br/>

#### 默认安装以下命令：
----
#### 基本命令
* <a href="#new">new</a> [`官方资料`](https://docs.microsoft.com/zh-cn/dotnet/core/tools/dotnet-new?tabs=netcore20)
* restore
* build
* publish
* run
* test
* vstest
* pack
* migrate
* clean
* sln
* help
* store
#### 项目修改命令
* add package
* add reference
* remove package
* remove reference
* list reference
#### 高级命令
* nuget delete
* nuget locals
* nuget push
* msbuild
* dotnet install script
----
####  命令 <a name="new">dotnet new</a>
* **Dotnet new -all** 会列出所有的信息说明 与模板引擎提供的所有项目模板已经模板创建样例.
```
PS G:\Acigela\donet> dotnet new -all
使用情况: new [选项]

选项:
  -h, --help          显示有关此命令的帮助。
  -l, --list          列出包含指定名称的模板。如果未指定名称，请列出所有模板。
  -n, --name          正在创建输出的名称。如果未指定任何名称，将使用当前目录的名称。
  -o, --output        要放置生成的输出的位置。
  -i, --install       安装源或模板包。
  -u, --uninstall     卸载一个源或模板包。
  --type              基于可用的类型筛选模板。预定义的值为 "project"、"item" 或 "other"。
  --force             强制生成内容，即使该内容会更改现有文件。
  -lang, --language   指定要创建的模板的语言。


模板                                                短名称              语言                标记
--------------------------------------------------------------------------------------------------------
Console Application                               console          [C#], F#, VB      Common/Console
Class library                                     classlib         [C#], F#, VB      Common/Library
Unit Test Project                                 mstest           [C#], F#, VB      Test/MSTest
xUnit Test Project                                xunit            [C#], F#, VB      Test/xUnit
ASP.NET Core Empty                                web              [C#], F#          Web/Empty
ASP.NET Core Web App (Model-View-Controller)      mvc              [C#], F#          Web/MVC
ASP.NET Core Web App                              razor            [C#]              Web/MVC/Razor Pages
ASP.NET Core with Angular                         angular          [C#]              Web/MVC/SPA
ASP.NET Core with React.js                        react            [C#]              Web/MVC/SPA
ASP.NET Core with React.js and Redux              reactredux       [C#]              Web/MVC/SPA
ASP.NET Core Web API                              webapi           [C#], F#          Web/WebAPI
global.json file                                  globaljson                         Config
NuGet Config                                      nugetconfig                        Config
Web Config                                        webconfig                          Config
Solution File                                     sln                                Solution
Razor Page                                        page                               Web/ASP.NET
MVC ViewImports                                   viewimports                        Web/ASP.NET
MVC ViewStart                                     viewstart                          Web/ASP.NET

Examples:
    dotnet new mvc --auth Individual
    dotnet new classlib --framework netcoreapp2.0
    dotnet new --help
```
* 通常我们的应用有几种对于本人来说无非:`Console Application`,`Class library`,`ASP.NET Core Empty`,`ASP.NET Core Web App`   
----
* 创建C# 控制台应用程序:  
`dotnet new console -lang `

* 备注:某些 shell 将 # 解释为特殊字符。 在这些情况下，需要括住语言参数值，如:  
`dotnet new console -lang "F#".`

* 创建一个mvc项目, 设置没有身份验证的ASP.NET Core C#程序 使用.net core 2.0框架  
`dotnet new mvc -au None -f netcoreapp2.0`

----
#### 参数选项: 
* -h&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--help 列出帮助命令
* -l&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--list 列出命令或者模板
* -lang&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;---lang|--language {C#|F#|VB} 指定语言 
* -n&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--name <OUTPUT_NAME>  所创建的输出的名称。 如果未指定名称，使用的是当前目录的名称。
* -o&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; --用于放置生成的输出的位置。 默认为当前目录。
* -na &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;--namespace <NAMESPACE_NAME> - 生成的代码的命名空间。 默认值为 MyApp.Namespace。

#### 参数
* `-au|--auth <AUTHENTICATION_TYPE>` - 要使用的身份验证类型。 可能的值为：
* `None` - 不进行身份验证（默认）。
* `Individual` - 个人身份验证。
* `IndividualB2C` - 使用 Azure AD B2C 进行个人身份验证。
* `SingleOrg` - 对一个租户进行组织身份验证。
* `MultiOrg` - 对多个租户进行组织身份验证。
* `Windows` - Windows 身份验证。

