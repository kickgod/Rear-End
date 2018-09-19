<a id="top" href="#top">C# 文件流操作 :maple_leaf:</a> 
----
`System.IO下的Stream类是所有数据流的基类，当我们对数据进行逐字节操作时，首先需要将数据转换为数据流。C#数据流主要分为三类：FileStream、MemoryStream、
NetworkStream，还有常用的StreamReader、StreamWriter和TextWriter类等。`

- [x] :maple_leaf: <a href="#FileModeEnum">`FileMode [Enum]`</a>
- [x] :maple_leaf: <a href="#FileStreamIndex">`FileStream`</a>
- [x] :maple_leaf: <a href="#">``</a>
- [x] :maple_leaf: <a href="#">``</a>

####  <a id="FileModeEnum" href="#FileModeEnum">FileMode [Enum]</a>  :star2: <a href="#top"> :arrow_up: </a>
`指定操作系统打开文件的方式。`

|`打开方式` | `值`|`描述`|
|---|:----|----|
|`CreateNew` |1 |	`指定操作系统应创建新文件。 这需要 Write 权限。 如果文件已存在，则将引发 IOException异常。` |
|`Create`    |2 |`指定操作系统应创建新文件。 如果文件已存在，它将被覆盖。 这需要 Write 权限。如果文件不存在，则使用 CreateNew；否则使用 Truncate。 如果该文件已存在但为隐藏文件，则将引发 UnauthorizedAccessException异常。`| 
|`Open`|3 |`指定操作系统应打开现有文件。 打开文件的能力取决于 FileAccess 枚举所指定的值。 如果文件不存在，引发一个 FileNotFoundException 异常。`|
|`OpenOrCreate`|4 |`指定操作系统应打开文件（如果文件存在）；否则，应创建新文件。 如果用 FileAccess.Read 打开文件，则需要 Read权限。 如果文件访问为 FileAccess.Write，则需要 Write权限。 如果用 FileAccess.ReadWrite 打开文件，则同时需要 Read 和 Write权限。`|
|`Truncate`|5 |`指定操作系统应打开现有文件。 该文件被打开时，将被截断为零字节大小。 这需要 Write 权限。 尝试从使用 FileMode.Truncate 打开的文件中进行读取将导致 ArgumentException 异常。`|
|`Append`|6 |`若存在文件，则打开该文件并查找到文件尾，或者创建一个新文件。 这需要 Append 权限。 FileMode.Append 只能与 FileAccess.Write 一起使用。 试图查找文件尾之前的位置时会引发 IOException 异常，并且任何试图读取的操作都会失败并引发 NotSupportedException 异常。`|

####  <a id="FileStreamIndex" href="#FileStreamIndex">FileStream</a>  :star2: <a href="#top"> :arrow_up: </a>
`1byte = 1字节 1024byte = 1KB 1024kb = 1M 1024M =1G, FileSream 读出来的是字节 0~255，虽然可以读写文本文件，但是无法得到原文,一般用来读取
二进制文件,例如图片，语音,视频之类的`
* `Read(byte[],offset:int,Maxlength) -> Count:int方法`:`读出数据,第一个参数存储数据 第二个参数 array 中的字节偏移量，将在此处放置读取的字节。，从此处开始将字节复制到该流。,第三个参数表示最大字节读取数量 返回读取
字节个数`
* `FileStream(path,FileMode.Open`:`构造函数： 路径，打开方式`<br/>

**`基本使用:文件内容少量的读数据`**
```C#
    var path = Path.GetFullPath(@"File/TextFile1.txt");
    FileStream stream = new FileStream(path,FileMode.Open);
    byte[] data = new byte[1024];
    //1byte = 1字节 1024byte = 1KB 1024kb = 1M 1024M =1G
    int length = stream.Read(data, 0, data.Length); //返回读到的字节数量
    for(var i=0;i< length; i++)
    {
        Console.Write(data[i]+" ");
    }
    Console.ReadKey();
```

**`基本使用:文件内容大量的读数据 数据的量超过1024的时候`**
```C#
  var path = Path.GetFullPath(@"File/TextFile1.txt");
  FileStream stream = new FileStream(path,FileMode.Open);
  byte[] data = new byte[1024];
  //1byte = 1字节 1024byte = 1KB 1024kb = 1M 1024M =1G
  while (true)
  {
        int length = stream.Read(data, 0, data.Length); //返回读到的字节数量
        if(length == 0)
        {
            Console.WriteLine("\n读写完毕");
            break;
        }
        for (var i = 0; i < length; i++)
        {
            Console.Write(data[i] + " ");
        }
  }
  Console.ReadKey();
```
**`将一张图片复制一下`**
```C#
  var path = Path.GetFullPath(@"File/123456.jpg");
  var copypath = Path.GetFullPath(@"File/fuben.jpg");
  FileStream readStream = new FileStream(path,FileMode.Open);
  FileStream writeStream = new FileStream(copypath, FileMode.Create);

  byte[] data = new byte[1024];

  while (true)
  {
      int length = readStream.Read(data, 0, data.Length); //返回读到的字节数量
      writeStream.Write(data, 0, length);
      if (length == 0)
      {
          Console.WriteLine("\n读写完毕");
          break;
      }
  }
  writeStream.Flush();
  writeStream.Close();
  readStream.Close();
  Console.ReadKey();
```
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up: </a>







