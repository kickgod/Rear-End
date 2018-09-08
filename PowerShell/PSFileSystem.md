<a id="top" href="#top">PowerShell 文件系统  :maple_leaf:</a> 
----
`在PowerShell控制台中，文件系统有很特别的重要性。一个明显的原因是管理员需要执行许多涉及文件系统的任务。另一个原因是文件系统是一个层次结构信息模型。
在接下来的章节中，你还会看到PowerShell在此基础上控制其它层次信息系统。你可以非常容易的将PowerShell中学到的驱动器，目录和文件的知识点应用到其它
地方，其中就包括注册表或者微软的Exchange。`

- [x] :maple_leaf: <a href="#GetListContent">访问文件和目录内容</a>
  - <a href="#ListContent" >`列出文件内容`</a> 
  - <a href="#ListContentfilter" >`列出文件内容-过滤和排除标准`</a>
  - <a href="#ListContentContext" >`列出文件内容-文件的FileInfo信息`</a>
  - <a href="#ListContentNeed" >`列出文件内容-特殊需求`</a>
- [x] :maple_leaf: <a href="#NavigatorSystem">导航文件系统</a>
- [x] :maple_leaf: <a href="#DIrctionAndFileSystem">使用目录和文件工作</a>
  - <a href="#NewDir" >`创建新目录`</a>
  - <a href="#NewFile" >`创建新文件`</a>
  - <a href="#ReadAndWriteFile" >`读写文件内容`</a>
  - <a href="#MoveAndCopyFile" >`移动和复制文件和目录`</a>
  - <a href="#DeleteFileOrDirectionay" >`删除文件和目录`</a>

####  <a id="GetListContent" href="#GetListContent">访问文件和目录内容</a>  :star2: <a href="#top"> :arrow_up:</a>
##### <a id="ListContent" href="#ListContent">列出文件内容</a>  :star2: <a href="#top"> :arrow_up: </a>
* `使用Get-ChildItem列出目录的内容。预定义的别名为Dir和ls，Get-ChildItem执行了一些很重要的任务：`
  * `显示目录内容`
  * `递归地搜索文件系统查找确定的文件`
  * `获取文件和目录的对象`
  * `把文件传递给其它命令，函数或者脚本` <br/>
    
**`Dir ls Get-ChildItem`**:`不带任何参数会列出所有的文件,包含文件夹和文件`    <br/>
```powershell
PS D:\> ls $home
//列出用户目录的所有文件
```

**`参数 *.dat`** <br/><br/>
`得到以什么结尾的文件`
```powershell
PS D:\> ls *.dat

   目录: D:\

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2018/7/12      8:21         139076 DocProCfgRes.dat
-a----        2018/6/21     15:51         138189 DocProCloudCfg.dat
```
**`参数 -path`** <br/><br/>
`后面可以用,号隔开列出多个路径下的文件 .表示当前文件,选课变量要用符号引用 键盘上tab键 上面的符号`
```powershell
PS D:\> ls -path '.\Program Files\'

    目录: D:\Program Files
    
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----         2018/7/8     21:31                2345Soft
d-----         2018/9/7     15:30                apache-tomcat-8.5.5
d-----        2018/7/10     19:03                Fresco Logic
.....
```
**`参数 -name`** <br/><br/>
`列表文件的名称即可,其他信息不需要`
```powershell
PS D:\> ls -name
HeidiSQL_9.4_Portable
Program Files
Program Files (x86)
ProgramData
QMDownload
Users
wamp64
Windows Kits
xampp
搜狗高速下载
迅雷下载
DocProCfgRes.dat
DocProCloudCfg.dat
InstallConfig.ini
```
**`参数 -recurse`** <br/><br/>
`递归子目录`
```C#
PS D:\>  Dir -recurse
....列出D盘所有文件....

PS D:\> Dir *.dat -recurse
// 列出D盘的以.dat结尾的所有文件
// ps低版本 有问题 因为低版本是先筛*.dat 再执行 -recurse

```
##### <a id="ListContentfilter" href="#ListContentfilter">过滤和排除标准</a>  :star2: <a href="#top"> :arrow_up: </a>
`现在回到刚开始问题，怎样递归列出同类型的所有文件，比如所有PowerShell scripts。答案是使用Dir完全列出所有目录内容，同
时指定一个过滤条件。Dir现在可以过滤出你想要列出的文件了。` <br/><br/>
**`参数`**:` -filter/-include`
```powershell
PS D:\> Dir $home -filter *.ps1 -recurse

Dir $home -include *.ps1 -recurse

PS D:\Program Files\nodejs> (Measure-Command {Dir  -filter .cmd -recurse}).TotalSeconds
0.2680067
PS D:\Program Files\nodejs> (Measure-Command {Dir  -include .cmd -recurse}).TotalSeconds
0.5885915
```
`你会看到这一戏剧性的变化，-filter的执行效率明显高于-include：其原因在于-include支持正则表达式，从内部实现上就更加复杂，而-filter只支持简单的
模式匹配。这也就是为什么你可以使用-include进行更加复杂的过滤。比如下面的例子，搜索所有第一个字符为A-F的脚本文件，显然已经超出了-filter的能力范围。`
```powershell
# -filter 查询所有以 "[A-F]"打头的脚本文件，屁都没找到
Dir $home -filter [a-f]*.ps1 -recurse
# -include 能够识别正则表达式，所以可以获取a-f打头，以.ps1收尾的文件
Dir $home -include [a-f]*.ps1 -recurse
```
**`参数`**:`-exclude` <br/><br/>
`与-include相反的是-exclude。在你想排除特定文件时，可以使用-exclude。不像-filter，-include和-exclude还支持数组，能让你获取你的家目录
下所有的图片文件。`
```powershell
Dir $home -recurse -include *.bmp,*.png,*.jpg, *.gif
```
**`参数`** :`Where-Object`<br/><br/>
`获取你家目录下比较大的文件，指定文件至少要100MB大小。`
```C#
Dir $home -recurse | Where-Object { $_.length -gt 100MB }
```
#####  <a id="ListContentContext" href="#ListContentContext">文件的FileInfo信息</a>  :star2: <a href="#top"> :arrow_up:</a>
```powershell
PS C:\Users\Kick> $file = ls *.json
PS C:\Users\Kick> $file | Format-List
PS C:\Users\Kick> $file.Attributes
Archive
PS C:\Users\Kick> $file.Mode
-a---
**`命令`**:`Get-Item` <br/><br/>
`获得单个文件 也可以获得目录本身`
```powershell
# Get-Item 获取的是目录对象本身:
$directory = Get-Item c:\windows
$directory

```
#####  <a id="ListContentNeed" href="#ListContentNeed">列出文件内容-特殊需求</a>  :star2: <a href="#top"> :arrow_up:</a>
`可以使用相对时间获取2周以内更改过的文件：`
```powershell
Dir | Where-Object { $_.CreationTime -gt (Get-Date).AddDays(-14) }
```
`比如下面的例子通过管道过滤2007年5月12日后更改过的文件：`
```powershell
Dir | Where-Object { $_.CreationTime -gt [datetime]::Parse("May 12, 2007") }
```
`只列出文件目录`
```powershell
 dir | Where-Object {$_.Mode -like "d*"  }
```
####  <a id="NavigatorSystem" href="#NavigatorSystem">导航文件系统</a>  :star2: <a href="#top"> :arrow_up:</a>
**`命令`**:`使用Get-Location命令获取当前工作的目录。`
```powershell
PS C:\> Get-Location

Path
----
C:\
```
**`命令`**:`Set-Location Cd`
`如果你想导航到文件系统的另外一个位置，可以使用Set-Location或者它的别名Cd：`
```powershell
# 进入父目录 (相对路径):
Cd ..
# 进入当前盘的根目录 (相对路径):
Cd \
# 进入指定目录 (绝对路径):
Cd c:\windows
# 从环境变量中获取系统目录 (绝对路径):
Cd $env:windir
# 从普通变量中获取目录 (绝对路径):
Cd $home
```

|字符|  意义|	示例|	示例描述|
|:---:|:----|:-----|:----|
|.|当前目录|	Ii .|	用资源浏览器打开当前目录|
|..|父目录|	Cd ..|	切换到父目录|
| \ |驱动器根目录|	Cd \  |	切换到驱动器的顶级根目录|
|~|家目录|	Cd ~|	切换到PowerShell初始化的目录|

**`命令`**:`Resolve-Path` <br/>
`当你使用相对路径时，PowerShell必须将这些相对转换成绝对路径。在你使用相对路径执行一个文件或者一条命令时，该转换会自动发生。你也可以自己使用Resolve-Path命令来处理。`
```C#
PS C:\> Resolve-Path .\Windows.old\

Path
----
C:\Windows.old\

PS C:\>  Resolve-Path $pshome\*.ps1xml

Path
----
C:\Windows\System32\WindowsPowerShell\v1.0\Certificate.format.ps1xml
C:\Windows\System32\WindowsPowerShell\v1.0\Diagnostics.Format.ps1xml
C:\Windows\System32\WindowsPowerShell\v1.0\DotNetTypes.format.ps1xml
C:\Windows\System32\WindowsPowerShell\v1.0\Event.Format.ps1xml
C:\Windows\System32\WindowsPowerShell\v1.0\FileSystem.format.ps1xml
C:\Windows\System32\WindowsPowerShell\v1.0\getevent.types.ps1xml
C:\Windows\System32\WindowsPowerShell\v1.0\Help.format.ps1xml
C:\Windows\System32\WindowsPowerShell\v1.0\HelpV3.format.ps1xml
C:\Windows\System32\WindowsPowerShell\v1.0\PowerShellCore.format.ps1xml
C:\Windows\System32\WindowsPowerShell\v1.0\PowerShellTrace.format.ps1xml
C:\Windows\System32\WindowsPowerShell\v1.0\Registry.format.ps1xml
C:\Windows\System32\WindowsPowerShell\v1.0\types.ps1xml
C:\Windows\System32\WindowsPowerShell\v1.0\typesv3.ps1xml
C:\Windows\System32\WindowsPowerShell\v1.0\WSMan.Format.ps1xml
```
**`注意`**:`Resolve-Path命令只有在文件确实存在时，才会有效。如果你的当前文件夹中没有一个名为a.png的是，Resolve-Path名讳报错。`
####  <a id="DIrctionAndFileSystem" href="#DIrctionAndFileSystem">使用目录和文件工作</a>  :star2: <a href="#top"> :arrow_up:</a>
`Get-ChildItem 和 Get-Item 命令可以获取已经存在的文件和目录。你也可以创建自己的文件和目录，重命名它们，给它们填充内容，复制它们，移动它们，当然也可以删除它们。`
```powershell
```
#####  <a id="NewDir" href="#NewDir">创建新目录</a>  :star2: <a href="#top"> :arrow_up:</a>
`创建一个新目录最方便的方式是使用MD函数，它内部调用的是New-Item命令，指定参数–type的值为"Directory"`
```powershell
PS E:\> mkdir user
//New-Item user -type Directory  实质上调用的是这个
//md user 也可以这样写
    目录: E:\

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----         2018/9/8      9:56                user
```
#####  <a id="NewFile" href="#NewFile">创建新文件</a>  :star2: <a href="#top"> :arrow_up:</a>
`创建一个新目录最方便的方式是使用MD函数，它内部调用的是New-Item命令，指定参数–type的值为"Directory"`
```powershell
 New-Item "wangzhe.txt"  -type File

    目录: E:\user

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         2018/9/8     10:10              0 wangzhe.txt
```
#####  <a id="ReadAndWriteFile" href="#ReadAndWriteFile">读写入文件</a>  :star2: <a href="#top"> :arrow_up:</a>
`读取文件内容` ：**`Get-Content`**
```powershell
PS E:\user> GET-CONTENT gg.TXT
2018/9/8 10:22:28
```
`追加内容进入文件-->>好像格式不对` ：**` >> `**

```powershell
PS E:\user> "Today Is What" >> GG.txt
PS E:\user> GET-CONTENT gg.TXT
2018/9/8 10:22:28
T o d a y   I s   W h a t   J i a n g x i n g 
```
`写入文件--会覆盖内容` ：**` Set-Content `**
```C#
PS E:\user> Set-Content gg.TXT "Fuck You"
PS E:\user> GET-CONTENT gg.TXT
Fuck You
PS E:\user>
```
`使用管道写入文件--会覆盖文件内容`：**`  Out-File `**
```powershell
PS E:\user> Dir |  Out-File GG.txt
PS E:\user> GET-CONTENT gg.TXT


    目录: E:\user


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         2018/9/8     10:20           1512 G.txt
               2018/9/8     10:31              0 GG.txt
-a----         2018/9/8     10:09              0 new my.md
-a----         2018/9/8     10:16             36 wangzhe.txt

```
`使用管道写入文件--会覆盖文件内容`：**` Set-CONTENT `**
```powershell
PS E:\user> Dir |  Set-Content GG.txt
PS E:\user> GET-CONTENT gg.TXT
G.txt
GG.txt
new my.md
wangzhe.txt
```
`追加文件内容的正确方式--不会有格式问题`：**` Add-Content `**
```powershell
PS E:\user> Dir |  Add-Content GG.txt
PS E:\user> GET-CONTENT gg.TXT
G.txt
GG.txt
new my.md
wangzhe.txt
G.txt
GG.txt
new my.md
wangzhe.txt
PS E:\user>
```
#####  <a id="RenameFileName" href="#RenameFileName">重命名文件</a>  :star2: <a href="#top"> :arrow_up:</a>
`重命名文件 使用`：**`Rename-Item`**
```powershell
PS E:\user>  Rename-Item testfile.txt testfile.csv
```


#####  <a id="MoveAndCopyFile" href="#MoveAndCopyFile">移动和复制文件和目录</a>  :star2: <a href="#top"> :arrow_up:</a>
`Move-Item 和 Copy-Item用来执行移动和拷贝操作。它们也支持通配符。比如下面的脚本会将你家目录下的的所有PowerShell脚本文件复制到桌面上`
```powershell
PS E:\user> Move-Item .\GG.txt ../wang.txt
将GG.txt文件移动到上层文件夹 并且命名为wang.txt
```
`复制文件`：**`Copy-Item`**
```powershell
PS E:\user> Copy-Item G.txt ../F.txt
//将G.txt文件复制到上层文件夹 并且命名为F.txt
PS E:\user> ls

    目录: E:\user
    
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         2018/9/8     10:20           1512 G.txt
-a----         2018/9/8     10:09              0 new my.md
-a----         2018/9/8     10:16             36 wangzhe.txt
```
#####  <a id="DeleteFileOrDirectionay" href="#DeleteFileOrDirectionay">删除文件和目录</a>  :star2: <a href="#top"> :arrow_up:</a>
* `使用Remove-Item和别名Del可以删除文件和目录`
* `如果一个目录被删除了，它里面所有的内容都会丢失。在你尝试去删除一个文件夹连同它的内容时，PowerShell都会请求你的批准。这样是为了防止你无意间销毁大量数据。只有空目录才不需要请求确认信息。`
```powershell
PS E:\user> ls


    目录: E:\user


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         2018/9/8     10:20           1512 G.txt
-a----         2018/9/8     10:09              0 new my.md
-a----         2018/9/8     10:16             36 wangzhe.txt


PS E:\user> del '.\new my.md'
PS E:\user> ls


    目录: E:\user


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         2018/9/8     10:20           1512 G.txt
-a----         2018/9/8     10:16             36 wangzhe.txt


PS E:\user> Remove-Item G.txt
PS E:\user> ls


    目录: E:\user


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         2018/9/8     10:16             36 wangzhe.txt

```






