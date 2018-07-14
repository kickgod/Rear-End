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
* <a href="#new">new</a>
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
