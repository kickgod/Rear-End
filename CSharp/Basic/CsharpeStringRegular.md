<a id="top">:checkered_flag: </a> 字符串和正则表达式 :whale2:
-----
:fast_forward:`对于一个程序,字符串无所不在,在C#中string关键字的映射实际上指向NET基类System.String,它是一个功能非常强大且用户广泛的类,当然处理字符串
,格式化字符串,字符串查找匹配才是字符串的重点呀` :rewind:

### [字符串](https://docs.microsoft.com/zh-cn/search/index?search=%E5%AD%97%E7%AC%A6%E4%B8%B2&scope=.NET)
 
- [x] [`System.String 类`](https://msdn.microsoft.com/zh-cn/library/system.string(v=vs.110).aspx) <a href="#StringClass">`自我讲解`</a>

- [x] <a href="#QueDianStringClass">`String缺点`</a>

- [x] <a href="#QueDianStringBuilder">`StringBuilder`</a>
------

#### <a id="StringClass">1.1&nbsp;&nbsp;  `System.String` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`String有一些非常可用的方法,非常的重要,提供了对字符串的许多操作,下面介绍一下他们的静态方法,String拥有无限多的静态方法,扩展方法,多得让人绝望
,了解一下,需要的时候翻一翻官方文档就好了`

|`方法`|`作用`|
|----|----|
|<a href="#CompareFunction">`Compare`</a>| `比较两个指定的 String 对象，并返回一个指示二者在排序顺序中的相对位置的整数`|
|<a href="#ConcatFunction">`Concat`</a>| `链接字符串 Concat(String[]) 支持无限个字符串参数`|
|<a href="#CopyToFunction">`CopyTo`</a>| `把从选定下标开始的特定数量字符复制到数组的一个全新实例中` [`CopyTo(Int32, Char[], Int32, Int32)`](https://msdn.microsoft.com/zh-cn/library/system.string.copyto(v=vs.110).aspx)|
|<a href="#FormatFunction">`Format`</a>| `格式化字符串`|
|<a href="#IndexOfFunction">`IndexOf`</a>| `格式化字符串`|
|<a href="#">`Contains(String)`</a>|`返回一个值，该值指示指定的子串是否出现在此字符串中。`|
|<a href="#">`StartsWith(String)`</a>|`字符串以什么开始`|
|<a href="#">`	EndsWith(String)`</a>|`字符串以什么结束`|
|<a href="#">`IsNullOrEmpty(String)`</a>|`指示指定的字符串是 null 还是 Empty 字符串。`|
|<a href="#">`IsNullOrWhiteSpace(String)`</a>|`指示指定的字符串是 null、空还是仅由空白字符组成。`|
|[`Replace`](https://msdn.microsoft.com/zh-cn/library/czx8s9ts(v=vs.110).aspx)|`字符串替代 Replace(Char, Char) Replace(String, String)` |

#### [`1.1.1 StringComparison 枚举`](https://msdn.microsoft.com/zh-cn/library/system.stringcomparison(v=vs.110).aspx)

|成员名称|	说明|
|----|----|
|CurrentCulture|	使用区分区域性的排序规则和当前区域性比较字符串。|
|CurrentCultureIgnoreCase|通过使用区分区域性的排序规则、当前区域性，并忽略所比较的字符串的大小写，来比较字符串。|
|InvariantCulture |使用区分区域性的排序规则和固定区域性比较字符串|
|InvariantCultureIgnoreCase|通过使用区分区域性的排序规则、固定区域性，并忽略所比较的字符串的大小写，来比较字符串。|	
|Ordinal	|使用序号（二进制）排序规则比较字符串。|
|OrdinalIgnoreCase|通过使用序号（二进制）区分区域性的排序规则并忽略所比较的字符串的大小写，来比较字符串。|

##### &nbsp;&nbsp;&nbsp;&nbsp;**Compare** 方法 比较字符串  <a id="CompareFunction"></a>
```C#
  Console.WriteLine(String.Compare("Jxing","Jxing"));  // 0   相等返回 1
  Console.WriteLine(String.Compare("Axing","Jxing"));  // -1  前面的字符串优先级高 为-1
  Console.WriteLine(String.Compare("Jxing","Axing"));  // 1   前面的字符串优先级低 1
```
&nbsp;&nbsp;&nbsp;&nbsp;`String 实例比较字符串方法`  **`CompareTo`**
 
```C#
  Console.WriteLine("Jxing".CompareTo("Axing"));
  Console.WriteLine("Axing".CompareTo("Jxing"));
  Console.WriteLine("Jxing".CompareTo("Jxing"));  
```
##### &nbsp;&nbsp;&nbsp;&nbsp;**Concat** 方法 链接字符串  <a id="ConcatFunction"></a>

```C#
  Console.WriteLine(String.Concat("Jxing-","Xing","sadasda","asdasd"));
  Console.WriteLine(String.Concat("Jxing-","Xing"));
```
##### &nbsp;&nbsp;&nbsp;&nbsp;**Format** 格式化字符串  <a id="FormatFunction"></a>
```C#
WriteLine(String.Format("{0},{1},{2}","Hello","The","World"));
```
##### &nbsp;&nbsp;&nbsp;&nbsp;**IndexOf** <a id="FormatFunction"></a>
`报告指定 Unicode 字符在此字符串中的第一个匹配项的从零开始的索引。`
* [`IndexOf(Char)`](https://msdn.microsoft.com/zh-cn/library/5xkyx09y(v=vs.110).aspx) :`报告指定 Unicode 字符在此字符串中的第
一个匹配项的从零开始的索引。 该搜索从指定字符位置开始。`

* [`IndexOf(Char, Int32, Int32)`](https://msdn.microsoft.com/zh-cn/library/ms131434(v=vs.110).aspx)：`报告指定字符串在
此实例中的第一个匹配项的从零开始的索引。 搜索从指定字符位置开始，并检查指定数量的字符位置。`

* [`IndexOf(String, Int32, Int32, StringComparison)`](https://msdn.microsoft.com/zh-cn/library/ms224423(v=vs.110).aspx):`报告指定的
字符串在当前 String 对象中的第一个匹配项的从零开始的索引
。 参数指定当前字符串中的起始搜索位置、要搜索的当前字符串中的字符数量，以及要用于指定字符串的搜索类型。`

#### <a id="QueDianStringClass">1.2&nbsp;&nbsp;  `String缺点` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>

* `String 存储的字符串是一个不可改变的,一旦改变就需要再次新建一个,非常耗费性能,不适合大规模或者长字符串处理`
* `每次再次新建新的字符串后又要调整地址,消除原来的字符串,不利已对字符串的删除,修改`

-----
##### 我们更加建议使用[StringBuilder 类](https://msdn.microsoft.com/zh-cn/library/system.text.stringbuilder(v=vs.110).aspx)

#### <a id="QueDianStringBuilder">1.2&nbsp;&nbsp;  `StringBuilder` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
```C#
    //初始化 利用字符串
    StringBuilder builder=new StringBuilder("My Name is Jx ");
    //初始化 说明初始长度
    StringBuilder buildLength=new StringBuilder(20);

    //追加字符串
    builder.Append("Hello the World do you know");
    builder.Insert(1,"ASDS");  //指定地方添加字符串

    builder.Replace("Jx","JiangXing");

    builder.Remove(1,5); //从哪里开始删除 删除多少位

    var val=builder.ToString(); //将StringBuilder转换为字符串
```

