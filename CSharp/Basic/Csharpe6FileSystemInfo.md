<a id="top" href="#top">:checkered_flag: C# 文件和流 :blue_heart:</a> 
----
- [x] :whale2: <a href="#SystemFileManagement">`管理文件系统`</a>
    * <a href="#DriveInfo">  `DriveInfo 检查驱动信息` </a>
    * <a href="#Path">  `Path 路径类` </a>
    * <a href="#File">  `File 文件操作` </a>
    * <a href="#FileInfo">  `FileInfo 文件对象` </a>
    * <a href="#Directory">  `Directory 目录操作` </a>
	* <a href="#DirectoryInfo">  `DirectoryInfo 目录对象` </a>

#### <a href="#SystemFileManagement" id="SystemFileManagement">管理文件系统</a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
* `FileSystemInfo`：`这是表示任何文件系统对象的基类`
* `FileInfo和 File`：`这些类表示文件系统上面的文件`
* `DirectoryInfo和Directory`：`这些类表示文件系统上面的目录`
* `Path`:`这个类包含的静态成员可以用于处理路径名`
* `DriveInfo`:`它的属性和方法提供了指定驱动器的信息`

```c# 
                      基类
               FileSystemInfo
              /              \
             / 继承            \ 继承 
            /                   \   
    DirectoryInfo             FileInfo   

    File[静态类]        Directory[静态类] 
```
* `Directory和File类只包含静态犯法,不能实例化.只要调用一个成员方法,提供合适的文件系统对象的路径,就可以使用这些类.如果只对文件夹或文件执行一个操作这些类
十分有效`
* `DirectoryInfo,FileInfo实现与Directory和File类有大致相同的公共方法,并拥有一些公共属性和构造函数,但他们都是有状态的,并且这些类的成员都不是静态的
需要实例化这些类,之后吧每个实例与特定的文件或者文件夹关联起来。`

#####  <a id="DriveInfo" href="#DriveInfo">  `DriveInfo 检查驱动信息` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
[`DirveInfo`](https://msdn.microsoft.com/zh-cn/library/system.io.driveinfo(v=vs.110).aspx) `类可以用于检查驱动器信息,可以描述驱动器列表还可以进一步提供任何关于驱动器的大量细节`
* 构造函数
  * `DriveInfo(String)`:	 `提供对有关指定驱动器的信息的访问。`
* 属性
	* `AvailableFreeSpace`	`指示驱动器上的可用空闲空间总量（以字节为单位）。`
	* `DriveFormat`	`获取文件系统的名称，例如 NTFS 或 FAT32。`
	* `DriveType`	`获取驱动器类型，如 CD-ROM、可移动、网络或固定`
	* `IsReady`	`获取一个指示驱动器是否已准备好的值。`
	* `Name`	`获取驱动器的名称，如 C:\。`
	* `RootDirectory`	`获取驱动器的根目录。`
	* `TotalFreeSpace`	`获取驱动器上的可用空闲空间总量（以字节为单位）。`
	* `TotalSize`	`获取驱动器上存储空间的总大小（以字节为单位）。`
	* `VolumeLabel`	`获取或设置驱动器的卷标。`
* 方法 只有一个方法有用
  * `GetDrives()`:`检索计算机上的所有逻辑驱动器的驱动器名称。`

```C#
      static void Main(string[] args)
      {
          DriveInfo[] drives=DriveInfo.GetDrives();
          foreach(DriveInfo drive in drives){
              if(drive.IsReady){
                  Console.WriteLine("---------------------------------------------------");
                  Console.WriteLine($"磁盘名称:{drive.Name}");
                  Console.WriteLine($"磁盘格式:{drive.DriveFormat}");
                  Console.WriteLine($"磁盘类型:{drive.DriveType}");
                  Console.WriteLine($"磁盘根目录:{drive.RootDirectory}");
                  Console.WriteLine($"磁盘卷标:{drive.VolumeLabel}");
                  Console.WriteLine($"磁盘空余空间:{drive.TotalFreeSpace}");
                  Console.WriteLine($"磁盘可利用大学:“{drive.AvailableFreeSpace}"); 
                  Console.WriteLine($"磁盘总大小:{drive.TotalSize}");
              }
          }   
      }
      //输出
      ---------------------------------------------------
      磁盘名称:C:\
      磁盘格式:NTFS
      磁盘类型:Fixed
      磁盘根目录:C:\
      磁盘卷标:
      磁盘空余空间:79885795328
      磁盘可利用大学:“79885795328
      磁盘总大小:127440777216
      ....
```
##### [Path](https://msdn.microsoft.com/zh-cn/library/system.io.path(v=vs.110).aspx) 路径类 <a id="Path"></a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`对包含文件或目录路径信息的 String 实例执行操作。 这些操作是以跨平台的方式执行的。`  **`静态列`**

* Path 字段
   * `AltDirectorySeparatorChar`:	`提供平台特定的替换字符，该替换字符用于在反映分层文件系统组织的路径字符串中分隔目录级别。`
   * `DirectorySeparatorChar`	:`提供平台特定的字符，该字符用于在反映分层文件系统组织的路径字符串中分隔目录级别。`
   * `InvalidPathChars[过时]`	:`已过时。 提供平台特定的字符数组，这些字符不能在传递到 Path 类的成员的路径字符串参数中指定。`
   * `PathSeparator`:	`用于在环境变量中分隔路径字符串的平台特定的分隔符。`
   * `VolumeSeparatorChar`:	`提供平台特定的卷分隔符。`
* 方法
   * `Combine(String[])`:`将字符串数组组合成一个路径。`
   * `GetDirectoryName(String)`:`返回指定路径字符串的目录信息。`
   * `GetExtension(String)[重要]`:`返回指定的路径字符串的扩展名`
   * `GetFileName(String)[重要]`:`返回指定路径字符串的文件名和扩展名`
   * `GetFileNameWithoutExtension(String)[重要]`:`返回不具有扩展名的指定路径字符串的文件名`
   * **`GetFullPath(String)[重要]`**:`返回指定路径字符串的绝对路径`
   * **`HasExtension(String)[重要]`**:`确定路径是否包括文件扩展名`
   * `IsPathRooted(String)`:`获取一个值，该值指示指定的路径字符串是否包含根`
   * `GetInvalidFileNameChars()`:`获取包含不允许在文件名中使用的字符的数组`
   * `GetInvalidPathChars()`:`获取包含不允许在路径名中使用的字符的数组`
   * `GetPathRoot(String)`:`获取指定路径的根目录信息`
   * `GetTempFileName()`:`在磁盘上创建磁唯一命名的零字节的临时文件并返回该文件的完整路径`
   * `GetTempPath()`:`返回当前用户的临时文件夹的路径`
```C#
      string path2 = @"G:\Acigela\DotnetConsole2\resource";
      string path3 = @"filtering-labels.html";
      string path4 = @"G:\Acigela\DotnetConsole2\resource\multi-axis.html";

      Console.WriteLine($"{Path.AltDirectorySeparatorChar}");
      Console.WriteLine($"{Path.DirectorySeparatorChar}");
      Console.WriteLine($"{Path.PathSeparator}");
      Console.WriteLine($"{Path.VolumeSeparatorChar}");

      //	Combine(String, String)
      Console.WriteLine($"路径: {Path.Combine(path2,path3)}");
      //	GetDirectoryName(String)
      Console.WriteLine($"目录信息:{Path.GetDirectoryName(path4)}");  
      //	GetInvalidFileNameChars()
      foreach(var i in Path.GetInvalidFileNameChars()){
          Console.WriteLine($"文件禁止字符:{i}");
      }

      //	GetInvalidPathChars()
      foreach(var i in Path.GetInvalidPathChars()){
          Console.WriteLine($"路径禁止字符:{i}");
      }
      //  GetRandomFileName() 	返回随机文件夹名或文件名
      Console.WriteLine($"{Path.GetRandomFileName()}");
      // 	GetTempFileName() 在磁盘上创建磁唯一命名的零字节的临时文件并返回该文件的完整路径
      Console.WriteLine($"{Path.GetTempFileName()}");
      //  GetTempPath() 返回当前用户的临时文件夹的路径
      Console.WriteLine($"{Path.GetTempPath()}");
```
##### [File](https://msdn.microsoft.com/zh-cn/library/system.io.file(v=vs.110).aspx) 文件操作 <a id="File"></a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`提供用于创建、复制、删除、移动和打开单一文件的静态方法，并协助创建 FileStream 对象。静态类`
* 文件编码:[Encoding 类](https://msdn.microsoft.com/zh-cn/library/system.text.encoding(v=vs.110).aspx)  `System.Text.Encoding`

* 方法
  * **`AppendAllLines(String, IEnumerable<String>)`**:`向一个文件中追加行，然后关闭该文件。 如果指定文件不存在，此方法会创建一个文件，向其中写入指定的行，然后关闭该文件`
  * **`AppendAllLines(String, IEnumerable<String>, Encoding)`**:`使用指定的编码向一个文件中追加行，然后关闭该文件。 如果指定文件不存在，此方法会创建一个文件，向其中写入指定的行，然后关闭该文件`
  * **`AppendAllText(String, String)`**:`打开一个文件，向其中追加指定的字符串，然后关闭该文件。 如果文件不存在，此方法将创建一个文件，将指定的字符串写入文件，然后关闭该文件。`
  * **`AppendAllText(String, String, Encoding)`**:`将指定的字符串追加到文件中，如果文件还不存在则创建该文件。`
  * **`AppendText(String)`**:`创建一个 StreamWriter，它将 UTF-8 编码文本追加到现有文件或新文件（如果指定文件不存在）`
  * **`Copy(String, String)[重要]`**:`将现有文件复制到新文件。 不允许覆盖同名的文件。`
  * **`Copy(String, String, Boolean)`**:`将现有文件复制到新文件。 允许覆盖同名的文件。`
  * **`Create(String)`**:`在指定路径中创建或覆盖文件。`
  * **`Create(String, Int32)`**:`创建或覆盖指定的文件。`
  * **`Create(String, Int32, FileOptions)`**:`创建或覆盖指定的文件，指定缓冲区大小和一个描述如何创建或覆盖该文件的 FileOptions 值。`
  * **`Create(String, Int32, FileOptions, FileSecurity)`**:`创建或覆盖具有指定的缓冲区大小、文件选项和文件安全性的指定文件`
  * **`CreateText(String)[重要]`**:`创建或打开用于写入 UTF-8 编码文本的文件`
  * **`Decrypt(String)`**:`使用 Encrypt 方法解密由当前帐户加密的文件`
  * **`Exists(String)`**:`确定指定的文件是否存在`
  * **`Encrypt(String)`**:`将某个文件加密，使得只有加密该文件的帐户才能将其解密`
  * **`GetAccessControl(String)`**:`获取一个 FileSecurity 对象，它封装指定文件的访问控制列表 (ACL) 条目`
  * **`Move(String, String)[重要]`**:`将指定文件移到新位置，提供要指定新文件名的选项。`
  * **`Open(String, FileMode)`**:`以读/写访问权限打开指定路径上的 FileStream`
  * **`Open(String, FileMode, FileAccess)`**:`以指定的模式和访问权限打开指定路径上的 FileStream。`
  * **`Open(String, FileMode, FileAccess, FileShare)`**:`打开指定路径上的 FileStream，具有带读、写或读/写访问的指定模式和指定的共享选项`
  * **`OpenRead(String)`**:`打开现有文件以进行读取`
  * **`OpenText(String)`**:`打开现有 UTF-8 编码文本文件以进行读取`
  * **`OpenWrite(String)`**:`	打开一个现有文件或创建一个新文件以进行写入`
  * **`ReadAllBytes(String)`**:`打开一个二进制文件，将文件的内容读入一个字节数组，然后关闭该文件。`
  * **`ReadAllLines(String)`**:`打开一个文本文件，读取文件的所有行，然后关闭该文件`
  * **`ReadAllLines(String, Encoding)`**:`打开一个文件，使用指定的编码读取文件的所有行，然后关闭该文件`
  * **`Replace(String, String, String)`**:`使用其他文件的内容替换指定文件的内容，这一过程将删除原始文件，并创建被替换文件的备份。`
  * **`Replace(String, String, String, Boolean)`**:`用其他文件的内容替换指定文件的内容，这一过程将删除原始文件，并创建被替换文件的备份，还可以忽略合并错误。`
  * **`WriteAllText(String, String, Encoding)`**:`创建一个新文件，使用指定编码向其中写入指定的字符串，然后关闭文件。 如果目标文件已存在，则覆盖该文件`
  * **`WriteAllText(String, String)`**:`创建一个新文件，向其中写入指定的字符串，然后关闭文件。 如果目标文件已存在，则覆盖该文件`
  * **`WriteAllLines(String, String[], Encoding)`**:`创建一个新文件，使用指定编码在其中写入指定的字符串数组，然后关闭该文件`
  * **`WriteAllLines(String, String[])`**:`建一个新文件，在其中写入指定的字节数组，然后关闭该文件`
  * **`WriteAllLines(String, IEnumerable<String>)`**:`创建一个新文件，向其中写入一个字符串集合，然后关闭该文件`
  * **`WriteAllLines(String, IEnumerable<String>, Encoding)`**:`使用指定的编码创建一个新文件，向其中写入一个字符串集合，然后关闭该文件`
  * **`WriteAllBytes(String, Byte[])`**:`创建一个新文件，在其中写入指定的字节数组，然后关闭该文件。 如果目标文件已存在，则覆盖该文件`
  * **`SetAttributes(String, FileAttributes)`**:`获取指定路径上的文件的指定 FileAttributes。`

##### `按行读出文件`
```C#
     var path=@"resource\filtering-labels.html";
     Console.WriteLine($"绝对路径 Path :{Path.GetFullPath(path)}");
     if(File.Exists(path)){
         //按照行读出文件
         var listallline=File.ReadAllLines(path,Encoding.UTF8);
          foreach(var line in listallline){
              Console.WriteLine(line);   
          }
     }
```
##### `覆盖或者新建文件 修改内容为`

* 文件覆盖内容
```C#
     /*覆盖文件 */     
     var pathw=@"resource\user.cs"; 

     File.WriteAllText(pathw," using System;\n using System.Text;\n using System.IO;\n using System.
     Collections.Generic;\n using System.Collections;using System.Threading; \n using System.Threading.Tasks;"); 
```
* 按照行覆盖文件
```C#
      var path=@"resource\filtering-labels.html";
      List<String> str=new List<string>();

      if(File.Exists(path)){
          //按照行读出文件
          var listallline=File.ReadAllLines(path,Encoding.UTF8);
              foreach(var line in listallline){
                  str.Add(line);
              }
      }
     /*覆盖或者新建文件*/     
     var pathw=@"resource\index.html";

     File.WriteAllLines(pathw,str,Encoding.UTF8);
```
##### 追加文件

* 追加文件字符串
```C#
     /*追加文件 */     
    var pathw=@"resource\user.cs"; 

    File.AppendAllText(pathw," using System;\n using System.Text;\n using System.IO;\n using
    System.Collections.Generic;\n using System.Collections;using System.Threading; 
    \n using System.Threading.Tasks;"); 

```
* 按照行追加文件
```C#
    var path=@"resource\multi-axis.html";
    List<String> str=new List<string>();

    if(File.Exists(path)){
        //按照行读出文件
        var listallline=File.ReadAllLines(path,Encoding.UTF8);
            foreach(var line in listallline){
                str.Add(line);
            }
    }
    /*覆盖或者新建文件*/     
    var pathw=@"resource\index.html";

    File.AppendAllLines(pathw,str,Encoding.UTF8); 
```

##### `创建文件`
```C#
   //创建UTF-8 的文本文件
   File.CreateText("resource/ac.cs");
   File.CreateText("resource/index.html");
   // 创建文件 注意是文件 不能创建目录
   File.Create("resource/sd.md");
```
##### `删除文件`
```C#
   /*删除文件*/     
   var pathw=@"resource\sd.md";
   if(File.Exists(pathw)){
        File.Delete(pathw);
   }
```
##### `复制文件`
```C#
   /*复制文件*/     
   var pathw=@"resource\indexs.html";
   Console.WriteLine($"{Path.GetDirectoryName(pathw)}\\2016110418xing.html");
   if(File.Exists(pathw)){
       File.Copy(pathw, $"{Path.GetDirectoryName(pathw)}\\2016110418xing.html",true);
       File.Delete(pathw);
   }
```
##### `移动文件`
`根据随机文件名称,重命名文件`
```C#
   /*移动文件*/     
   string code="qwertyuiopasdfghjklzxcvbnm".ToUpper();
   Random r=new Random();
   int val= r.Next(0,code.Length);	//会产生0 但是不会产生26
   int valt= r.Next(0,code.Length);	//会产生0 但是不会产生26
   var now=DateTime.Now;
   var pathw=@"resource\GP20188918343995.html";
   var name=$"{Path.GetDirectoryName(pathw)}\\{code[val]}{code[valt]}{now.Year}{now.Month}{now.Day}
   {now.Hour}{now.Minute}{now.Second}{now.Millisecond}{Path.GetExtension(pathw)}";
   var pathnew=$"{Path.GetFullPath(name)}";
    Console.WriteLine($"新文件路径: {pathnew}");
    if(File.Exists(pathw)){
        File.Move(pathw, pathnew);
    }
```
#### <a href="https://msdn.microsoft.com/zh-cn/library/system.io.fileinfo(v=vs.110).aspx" id="FileInfo">FileInfo 文件对象</a> :closed_umbrella: <a href="#top"> `置顶` </a> 
`提供用于创建、复制、删除、移动和打开文件的属性和实例方法，并且帮助创建 FileStream 对象。 此类不能被继承`
* 构造函数  
   * `FileInfo(String)`	:`初始化作为文件路径的包装的 FileInfo 类的新实例 `
* 属性
    * `Attributes`	`获取或设置当前文件或目录的特性。（继承自 FileSystemInfo。）`
    * `CreationTime`	`获取或设置当前文件或目录的创建时间。（继承自 FileSystemInfo。）`
    * `CreationTimeUtc`	`获取或设置当前文件或目录的创建时间，其格式为协调世界时 (UTC)。（继承自 FileSystemInfo。）`
    * `Directory`	`获取父目录的实例。`
	* `DirectoryName`	`获取表示目录的完整路径的字符串。`
	* `Exists`	`获取指示文件是否存在的值。（覆盖 FileSystemInfo.Exists。）`
	* `Extension`	`获取表示文件扩展名部分的字符串。（继承自 FileSystemInfo。）`
	* `FullName`	`获取目录或文件的完整目录。（继承自 FileSystemInfo。）`
	* `IsReadOnly`	`获取或设置确定当前文件是否为只读的值。`
	* `LastAccessTime`	`获取或设置上次访问当前文件或目录的时间。（继承自 FileSystemInfo。`
	* `LastAccessTimeUtc`	`获取或设置上次访问当前文件或目录的时间，其格式为协调世界时 (UTC)。（继承自 FileSystemInfo。）`
	* `LastWriteTime`	`获取或设置上次写入当前文件或目录的时间。（继承自 FileSystemInfo。）`
	* `LastWriteTimeUtc`	`获取或设置上次写入当前文件或目录的时间，其格式为协调世界时 (UTC)。（继承自 FileSystemInfo。）`
	* `Length`	`获取当前文件的大小（以字节为单位）。`
    * `Name` `获取文件名。（覆盖 FileSystemInfo.Name。）`
* 属性操作
```C#
	FileInfo info=new FileInfo("resource/index.html");
	if(info.Exists){
		Console.WriteLine($"父目录:{info.DirectoryName}");
		Console.WriteLine($"文件名称:{info.Name}");
		DirectoryInfo dr=info.Directory;
		Console.WriteLine($"文件扩展:{info.Extension}");
		Console.WriteLine($"完整目录:{info.FullName}");
		Console.WriteLine($"父目录名称:{dr.Name}");
		Console.WriteLine($"文件初次创建时间:{info.CreationTime}");
		Console.WriteLine($"最后一次写入时间:{info.LastWriteTime}");
		Console.WriteLine($"最后一次访问时间:{info.LastAccessTime}");

	}else{
		Console.WriteLine($"文件不存在 或者路径错误");    
	}
	/*
		父目录名称:resource
		文件初次创建时间:2018/8/9 18:40:24
		最后一次写入时间:2018/8/9 18:40:24
		最后一次访问时间:2018/8/9 18:40:24
		PS G:\Acigela\DotnetConsole2> dotnet run
		父目录:G:\Acigela\DotnetConsole2\resource
		文件名称:index.html
		文件扩展:.html
		完整目录:G:\Acigela\DotnetConsole2\resource\index.html
		父目录名称:resource
		文件初次创建时间:2018/8/9 18:40:24
		最后一次写入时间:2018/8/9 21:07:44
		最后一次访问时间:2018/8/9 18:40:24
	*/
```
* 方法操作
	* `AppendText()` :`创建一个 StreamWriter，它向 FileInfo 的此实例表示的文件追加文本。`
	* `CopyTo(String)`：`将现有文件复制到新文件，不允许覆盖现有文件。`
	* `CopyTo(String, Boolean)`: `将现有文件复制到新文件，允许覆盖现有文件。`
	* `Create()` :`创建文件。`
	* `CreateObjRef(Type)`：`创建包含所有生成代理用于与远程对象进行通信所需的相关信息的对象。（继承自 MarshalByRefObject。）`
	* `CreateText()`: `创建写入新文本文件的 StreamWriter。`
	* `Decrypt()`:`使用 Encrypt 方法解密由当前帐户加密的文件。`
	* `Delete()`:`永久删除文件。（覆盖 FileSystemInfo.Delete()。）`
	* `Encrypt()`:`将某个文件加密，使得只有加密该文件的帐户才能将其解密。`
	* `Equals(Object)`:`确定指定的对象是否等于当前对象。（继承自 Object。）`
	* `GetAccessControl() `:`获取 FileSecurity 对象，该对象封装当前 FileInfo 对象所描述的文件的访问控制列表 (ACL) 项。`
	* `GetAccessControl(AccessControlSections)`:`获取一个 FileSecurity 对象，该对象封装当前 FileInfo 对象所描述的文件的指定类型的访问控制列表 (ACL) 项。`
	* `GetHashCode()`:`作为默认哈希函数。（继承自 Object。）`
	* `GetLifetimeService() `:`检索当前生存期服务对象，用于控制此实例的生存期策略。（继承自 MarshalByRefObject。）`
	* `GetObjectData(SerializationInfo, StreamingContext) `:`设置带有文件名和附加异常信息的 SerializationInfo 对象。（继承自 FileSystemInfo。）`
	* `GetType() `:`获取当前实例的 Type。（继承自 Object。）`
	* `InitializeLifetimeService() `:`获取生存期服务对象来控制此实例的生存期策略。（继承自 MarshalByRefObject。）`
	* `MoveTo(String)`:`将指定文件移到新位置，提供要指定新文件名的选项。`
	* `Open(FileMode)`:`在指定的模式中打开文件。`
	* `Open(FileMode, FileAccess)`:`用读、写或读/写访问权限在指定模式下打开文件。`
	* `Open(FileMode, FileAccess, FileShare)`:`用读、写或读/写访问权限和指定的共享选项在指定的模式中打开文件。`
	* `OpenRead()`:`创建一个只读的 FileStream。`
	* `OpenText()`:`创建使用从现有文本文件中读取的 UTF8 编码的 StreamReader。`
	* `OpenWrite() `:`创建一个只写的 FileStream。`
	* `Refresh()`:`刷新对象的状态。（继承自 FileSystemInfo。）`
	* `Replace(String, String)`:`使用当前 FileInfo 对象所描述的文件替换指定文件的内容，这一过程将删除原始文件，并创建被替换文件的备份。`
	* `Replace(String, String, Boolean)`:`使用当前 FileInfo 对象所描述的文件替换指定文件的内容，这一过程将删除原始文件，并创建被替换文件的备份。 还指定是否忽略合并错误。`
	* `SetAccessControl(FileSecurity)`:`将 FileSecurity 对象所描述的访问控制列表 (ACL) 项应用于当前 FileInfo 对象所描述的文件。`
	* `ToString()`:`以字符串形式返回路径。（覆盖 Object.ToString()。）`
	
```C#
	FileInfo info=new FileInfo("resource/User.txt");
	if(info.Exists){                                    
	}else{
	   info.CreateText();
	}

	//追加文本
	StreamWriter wr= info.AppendText();
	wr.WriteLine("let me start to keick hello wrold");
	//将缓存刷到文件中去
	wr.Flush();

	//复制文件
	info.CopyTo("resource/list.txt");
	//移动文件
	info.MoveTo("/resource/HHH.txt");
	//删除文件
	info.Delete();
```

#### <a href="#Directory" id="Directory">Directory 目录操作</a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`公开用于通过目录和子目录进行创建、移动和枚举的静态方法。 无法继承此类。`  
* 公共方法
	* `CreateDirectory(String)`: `在指定路径中创建所有目录和子目录，除非它们已经存在。`
	* `CreateDirectory(String,?DirectorySecurity)`: `在指定路径中创建所有目录（除非已存在），并应用指定的 Windows 安全性。`
	* `Delete(String)`: `从指定路径删除空目录。`
	* `Delete(String,?Boolean)`: `删除指定的目录，并删除该目录中的所有子目录和文件（如果表示）。`
	* `EnumerateDirectories(String)`: `返回指定路径中的目录名的可枚举集合。`
	* `EnumerateDirectories(String,?String)`: `返回指定路径中与搜索模式匹配的目录名的可枚举集合。`
	* `EnumerateDirectories(String,?String,?SearchOption)`: `返回指定路径中与搜索模式匹配的目录名称的可枚举集合，还可以搜索子目录。`
	* `EnumerateFiles(String)`: `返回指定路径中的文件名的可枚举集合。`
	* `EnumerateFiles(String,?String)`: `返回指定路径中与搜索模式匹配的文件名称的可枚举集合。`
	* `EnumerateFiles(String,?String,?SearchOption)`: `返回指定路径中与搜索模式匹配的文件名称的可枚举集合，还可以搜索子目录。`
	* `EnumerateFileSystemEntries(String)`: `返回指定路径中的文件名和目录名的可枚举集合。`
	* `EnumerateFileSystemEntries(String,?String)`: `返回指定路径中与搜索模式匹配的文件名和目录名的可枚举集合。`
	* `EnumerateFileSystemEntries(String,?String,?SearchOption)`: `返回指定路径中与搜索模式匹配的文件名称和目录名的可枚举集合，还可以搜索子目录。`
	* `Exists(String)`: `确定给定路径是否引用磁盘上的现有目录。`
	* `GetAccessControl(String)`: `获取 DirectorySecurity 对象，该对象封装指定目录的访问控制列表 (ACL) 项。`
	* `GetAccessControl(String,?AccessControlSections)`: `获取一个 DirectorySecurity 对象，它封装指定目录的指定类型的访问控制列表 (ACL) 条目。`
	* `GetCreationTime(String)`: `获取目录的创建日期和时间。`
	* `GetCreationTimeUtc(String)`: `获取目录创建的日期和时间，其格式为协调通用时 (UTC)。`
	* `GetCurrentDirectory()`: `获取应用程序的当前工作目录。`
	* `GetDirectories(String)`: `返回指定目录中的子目录的名称（包括其路径）。`
	* `GetDirectories(String,?String)`: `返回指定目录中与指定的搜索模式匹配的子目录的名称（包括其路径）。`
	* `GetDirectories(String,?String,?SearchOption)`: `返回与在指定目录中的指定搜索模式匹配的子目录的名称（包括其路径），还可以选择地搜索子目录。`
	* `GetDirectoryRoot(String)`: `返回指定路径的卷信息、根信息或两者同时返回。`
	* `GetFiles(String)`: `返回指定目录中文件的名称（包括其路径）。`
	* `GetFiles(String,?String)`: `返回指定目录中与指定的搜索模式匹配的文件的名称（包含其路径）。`
	* `GetFiles(String,?String,?SearchOption)`: `返回指定目录中与指定的搜索模式匹配的文件的名称（包含其路径），使用某个值确定是否要搜索子目录。`
	* `GetFileSystemEntries(String)`: `返回指定路径中的所有文件和子目录的名称。`
	* `GetFileSystemEntries(String,?String)`: `返回与指定路径中搜索模式匹配的文件名和目录名的数组。`
	* `GetFileSystemEntries(String,?String,?SearchOption)`: `返回指定路径中与搜索模式匹配的所有文件名和目录名的数组，还可以搜索子目录。`
	* `GetLastAccessTime(String)`: `返回上次访问指定文件或目录的日期和时间。`
	* `GetLastAccessTimeUtc(String)`: `返回上次访问指定文件或目录的日期和时间，其格式为协调通用时 (UTC)。`
	* `GetLastWriteTime(String)`: `返回上次写入指定文件或目录的日期和时间。`
	* `GetLastWriteTimeUtc(String)`: `返回上次写入指定文件或目录的日期和时间，其格式为协调通用时 (UTC)。`
	* `GetLogicalDrives()`: `检索此计算机上格式为 "<drive letter>:\" 的逻辑驱动器的名称。`
	* `GetParent(String)`: `检索指定路径的父目录，包括绝对路径和相对路径。`
	* `Move(String,?String)`: `将文件或目录及其内容移到新位置。`
	* `SetAccessControl(String,?DirectorySecurity)`: `将 DirectorySecurity 对象描述的访问控制列表 (ACL) 项应用于指定的目录。`
	* `SetCreationTime(String,?DateTime)`: `为指定的文件或目录设置创建日期和时间。`
	* `SetCreationTimeUtc(String,?DateTime)`: `设置指定文件或目录的创建日期和时间，其格式为协调通用时 (UTC)。`
	* `SetCurrentDirectory(String)`: `将应用程序的当前工作目录设置为指定的目录。`
	* `SetLastAccessTime(String,?DateTime)`: `设置上次访问指定文件或目录的日期和时间。`
	* `SetLastAccessTimeUtc(String,?DateTime)`: `设置上次访问指定文件或目录的日期和时间，其格式为协调通用时 (UTC)。`
	* `SetLastWriteTime(String,?DateTime)`: `设置上次写入目录的日期和时间。`
	* `SetLastWriteTimeUtc(String,?DateTime)`: `设置上次写入某个目录的日期和时间，其格式为协调通用时 (UTC)。`
##### 创建目录
* `得到父目录里`
* `得到创建时间`
* `判断目录是否存在`
* `删除目录`
```C#
	var path=@"resource";
	var patstr=Path.GetFullPath(path);

	Console.WriteLine(patstr);
	//创建一个目录
	Directory.CreateDirectory(patstr);
	//删除一个空目录,第二个参数为true 可以删除非空目录 Directory.Delete(patstr,true);

	if(Directory.Exists(path))
		Directory.Delete(patstr);
	else{
		Console.WriteLine("文件已删除");
	}

	// 得到父目录
	Console.WriteLine($"父目录: {Directory.GetParent(patstr) }");

	p($"创建时间:{ Directory.GetCreationTime(patstr)}");
```
##### 遍历子文件子目录
```C#
	var path=@"resource";
	var patstr=Path.GetFullPath(path);
	// 返回目录下的所有文件
	Console.WriteLine();
	Console.WriteLine("-- 文件 --------------------------------------------------");
	Console.WriteLine(); 
	foreach(var file in Directory.GetFiles(patstr)){
		 Console.WriteLine(file);   
	}
	Console.WriteLine();
	Console.WriteLine("-- 子目录 -------------------------------------------------");
	Console.WriteLine();
	// 返回目录下的子目录
	foreach(var dir in Directory.GetDirectories(patstr)){
		Console.WriteLine(dir);
	}
	Console.WriteLine();
	/*
		G:\Acigela\DotnetConsole2\resource\Chart.min.js
		G:\Acigela\DotnetConsole2\resource\index.html
		G:\Acigela\DotnetConsole2\resource\list.txt
		G:\Acigela\DotnetConsole2\resource\multi-axis.html
		G:\Acigela\DotnetConsole2\resource\User.txt
		G:\Acigela\DotnetConsole2\resource\UZ201889183456565.html

		-- 子目录 -------------------------------------------------

		G:\Acigela\DotnetConsole2\resource\list
		G:\Acigela\DotnetConsole2\resource\Mydb	
	*/
```
##### 移动目录
```C#
	var path=@"resource";
	var patstr=Path.GetFullPath(path);
	Console.WriteLine($"最后一次访问时间:{Directory.GetLastAccessTime(path)}");
	Console.WriteLine($"最后一次写入时间:{Directory.GetLastWriteTime(path)}");

	// 移动目录
	Directory.Move(@"G:\MyFile",path);
```

#### <a href="#DirectoryInfo" id="DirectoryInfo">DirectoryInfo 目录对象</a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`公开用于通过目录和子目录进行创建、移动和枚举的实例方法。 此类不能被继承。`
* 构造方法
	* `DirectoryInfo(String)` :`初始化指定路径上的 DirectoryInfo 类的新实例。`
* 属性
	* `Attributes`: `获取或设置当前文件或目录的特性。（继承自 FileSystemInfo。）`
	* `CreationTime`: `获取或设置当前文件或目录的创建时间。（继承自 FileSystemInfo。）`
	* `CreationTimeUtc`: `获取或设置当前文件或目录的创建时间，其格式为协调世界时 (UTC)。（继承自 FileSystemInfo。）`
	* `Exists`: `获取指示目录是否存在的值。（覆盖 FileSystemInfo.Exists。）`
	* `Extension`: `获取表示文件扩展名部分的字符串。（继承自 FileSystemInfo。）`
	* `FullName`: `获取目录的完整路径。（覆盖 FileSystemInfo.FullName。）`
	* `LastAccessTime`: `获取或设置上次访问当前文件或目录的时间。（继承自 FileSystemInfo。）`
	* `LastAccessTimeUtc`: `获取或设置上次访问当前文件或目录的时间，其格式为协调世界时 (UTC)。（继承自 FileSystemInfo。）`
	* `LastWriteTime`: `获取或设置上次写入当前文件或目录的时间。（继承自 FileSystemInfo。）`
	* `LastWriteTimeUtc`: `获取或设置上次写入当前文件或目录的时间，其格式为协调世界时 (UTC)。（继承自 FileSystemInfo。）`
	* `Name`: `获取此 DirectoryInfo 实例的名称。（覆盖 FileSystemInfo.Name。）`
	* `Parent`: `获取指定的子目录的父目录。`
	* `Root`: `获取目录的根部分。`
* 一个目录的信息	
```C#
	DirectoryInfo dirinfo=new DirectoryInfo(@"resource");
	if(dirinfo.Exists){                
		Console.WriteLine($"属性: {dirinfo.Attributes}");
		Console.WriteLine($"是否存在: {dirinfo.Exists}");
		Console.WriteLine($"全目录: {dirinfo.FullName}");
		Console.WriteLine($"根目录: {dirinfo.Root}");
		Console.WriteLine($"父目录: {dirinfo.Parent}");
		Console.WriteLine($"访问时间: {dirinfo.LastAccessTime}");                
		Console.WriteLine($"写入时间: {dirinfo.LastWriteTime}");
		Console.WriteLine($"创建时间: {dirinfo.CreationTime}");
	}
```
* 常用方法
	* `Create()`: `创建目录。`
	* `Create(DirectorySecurity)`: `使用 DirectorySecurity 对象创建目录。`
	* `CreateObjRef(Type)`: `创建包含所有生成代理用于与远程对象进行通信所需的相关信息的对象。（继承自 MarshalByRefObject。）`
	* `CreateSubdirectory(String)`: `在指定路径上创建一个或多个子目录。 指定路径可以是相对于 DirectoryInfo 类的此实例的路径。`
	* `CreateSubdirectory(String,?DirectorySecurity)`: `使用指定的安全性在指定的路径上创建一个或多个子目录。 指定路径可以是相对于 DirectoryInfo 类的此实例的路径。`
	* `Delete()`: `如果此 DirectoryInfo 为空则将其删除。（覆盖 FileSystemInfo.Delete()。）`
	* `Delete(Boolean)`: `删除 DirectoryInfo 的此实例，指定是否删除子目录和文件。`
	* `EnumerateDirectories()`: `返回当前目录中目录信息的可枚举集合。`
	* `EnumerateDirectories(String)`: `返回与指定的搜索模式匹配的目录信息的可枚举集合。`
	* `EnumerateDirectories(String,?SearchOption)`: `返回与指定的搜索模式和搜索子目录选项匹配的目录信息的可枚举集合。`
	* `EnumerateFiles()`: `返回当前目录中的文件信息的可枚举集合。`
	* `EnumerateFiles(String)`: `返回与搜索模式匹配的文件信息的可枚举集合。`
	* `EnumerateFiles(String,?SearchOption)`: `返回与指定的搜索模式和搜索子目录选项匹配的文件信息的可枚举集合。`
	* `EnumerateFileSystemInfos()`: `返回当前目录中的文件系统信息的可枚举集合。`
	* `EnumerateFileSystemInfos(String)`: `返回与指定的搜索模式匹配的文件系统信息的可枚举集合。`
	* `EnumerateFileSystemInfos(String,?SearchOption)`: `返回与指定的搜索模式和搜索子目录选项匹配的文件系统信息的可枚举集合。`
	* `Equals(Object)`: `确定指定的对象是否等于当前对象。（继承自 Object。）`
	* `GetAccessControl()`: `获取一个 DirectorySecurity 对象，该对象封装当前 DirectoryInfo 对象所描述的目录的访问控制列表 (ACL) 项。`
	* `GetAccessControl(AccessControlSections)`: `获取一个 DirectorySecurity 对象，该对象封装当前 DirectoryInfo 对象所描述的目录的指定类型的访问控制列表 (ACL) 项。`
	* `GetDirectories()`: `返回当前目录的子目录。`
	* `GetDirectories(String)`: `返回当前 DirectoryInfo 中与给定搜索条件匹配的目录的数组。`
	* `GetDirectories(String,?SearchOption)`: `返回当前 DirectoryInfo 中目录的数组，该数组与给定的搜索条件匹配并使用某个值确定是否搜索子目录。`
	* `GetFiles()`: `返回当前目录的文件列表。`
	* `GetFiles(String)`: `返回当前目录中与给定的搜索模式匹配的文件列表。`
	* `GetFiles(String,?SearchOption)`: `返回当前目录的文件列表，该列表与给定的搜索模式匹配并且使用某个值确定是否搜索子目录。`
	* `GetFileSystemInfos()`: `返回表示某个目录中所有文件和子目录的强类型 FileSystemInfo 项的数组。`
	* `GetFileSystemInfos(String)`: `检索强类型 FileSystemInfo 对象的数组，该数组表示与指定的搜索条件匹配的文件和子目录。`
	* `GetFileSystemInfos(String,?SearchOption)`: `检索 FileSystemInfo 对象的数组，该数组表示与指定的搜索条件匹配的文件和子目录。`
	* `GetHashCode()`: `作为默认哈希函数。（继承自 Object。）`
	* `GetLifetimeService()`: `检索当前生存期服务对象，用于控制此实例的生存期策略。（继承自 MarshalByRefObject。）`
	* `GetObjectData(SerializationInfo,?StreamingContext)`: `设置带有文件名和附加异常信息的 SerializationInfo 对象。（继承自 FileSystemInfo。）`
	* `GetType()`: `获取当前实例的 Type。（继承自 Object。）`
	* `InitializeLifetimeService()`: `获取生存期服务对象来控制此实例的生存期策略。（继承自 MarshalByRefObject。）`
	* `MoveTo(String)`: `将 DirectoryInfo 实例及其内容移动到新路径。`
	* `Refresh()`: `刷新对象的状态。（继承自 FileSystemInfo。）`
	* `SetAccessControl(DirectorySecurity)`: `将 DirectorySecurity 对象所描述的访问控制列表 (ACL) 项应用于当前 DirectoryInfo 对象所描述的目录。`
	* `ToString()`: `返回用户所传递的原始路径。（覆盖 Object.ToString()。）`
* 代码操作
```C#
	DirectoryInfo dirinfo=new DirectoryInfo(@"resource");
	//创建子目录
	dirinfo.CreateSubdirectory("Script");

	// 返回目录下的所有文件
	Console.WriteLine();
	Console.WriteLine("-- 文件 --------------------------------------------------");
	Console.WriteLine(); 
	foreach(var file in dirinfo.GetFiles()){
		Console.WriteLine(file);   
	}
	Console.WriteLine();
	Console.WriteLine("-- 子目录 -------------------------------------------------");
	Console.WriteLine();
	// 返回目录下的子目录
	foreach(var dir in dirinfo.GetDirectories()){
		Console.WriteLine(dir);
	}
	Console.WriteLine();
```

  
  
  
  
  
  
  
  
  
  
  
  
  
