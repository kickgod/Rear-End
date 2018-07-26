<a id="top">:checkered_flag: </a> 字符串和正则表达式 :whale2:
-----
:fast_forward:`对于一个程序,字符串无所不在,在C#中string关键字的映射实际上指向NET基类System.String,它是一个功能非常强大且用户广泛的类,当然处理字符串
,格式化字符串,字符串查找匹配才是字符串的重点呀` :rewind:

### [字符串](https://docs.microsoft.com/zh-cn/search/index?search=%E5%AD%97%E7%AC%A6%E4%B8%B2&scope=.NET)
 `说明一个运算符 :` `@` `这个前缀非常重要 输出转义符号,这样在字符串中写入转义字符时不会被转义了`
 ```C#
    WriteLine(@"\n \t"); //输出:\n \t
 ```
- [x] [`System.String 类`](https://msdn.microsoft.com/zh-cn/library/system.string(v=vs.110).aspx) <a href="#StringClass">`自我讲解`</a>

- [x] <a href="#QueDianStringClass">`String缺点`</a>

- [x] <a href="#QueDianStringBuilder">`StringBuilder`</a>

### [字符串格式化](https://docs.microsoft.com/zh-cn/search/index?search=%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%A0%BC%E5%BC%8F%E5%8C%96&scope=.NET)

- [x] <a href="#FormatFunctionGSH">`String.Format方法`</a>

- [x] <a href="#FormattableString">`FormattableString`</a>

- [x] <a href="#StanderedFormattableString">`标准字符串方法格式字符串`</a>



### [正则表达式](https://docs.microsoft.com/zh-cn/search/index?search=%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%A0%BC%E5%BC%8F%E5%8C%96&scope=.NET)
`正则表达式作为小型技术领域的一部分,有这难以置信的作用,被非常多的语言支持,已经成为不可缺失的一部分 System.Text.RegularExpressions提供了C#语言对于正则表达式支持的许多类.完成各项功能`

- [`菜鸟教程正则表达式`](http://www.runoob.com/regexp/regexp-tutorial.html)

- [x] <a href="#StanderedSample">`正则表达式小例子`</a>

- [x] <a href="#PipeiGroupCapture">`匹配 组合捕获`</a>

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

#### <a id="QueDianStringBuilder">1.3&nbsp;&nbsp;  `StringBuilder` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
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

#### <a id="FormatFunctionGSH">2.1&nbsp;&nbsp;  `String.Format方法` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`插值法,{index} index从零开始 参数个数要对应`

```C#
   WriteLine(String.Format("{0},{1},{2}","Hello","The","World")); 
   //输出:Hello,The,World
```
#### <a id="FormattableString">2.1&nbsp;&nbsp;  `FormattableString` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`把字符串赋予给FormattableString 就很容易得到翻译过来的插值字符串。插值字符串可以直接分配,因为FormattableString比正常的字符串,更加适合匹配,这个类型定义了Format属性(返回得到的格式字符串),ArgumentCount属性和方法GetArgument(返回值)` [`FormattableString`](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/tokens/interpolated)
```C#
     static void Main(string[] args)
     {
         int x=100;
         string name="Jiangxing";
         int y=-10;
         FormattableString str=$"The Age Of {name} is {x+y}";
         WriteLine($"Format：{str.Format}");
         //得到格式化后的字符串
         WriteLine(str.ToString()); 
         //得到参数个数
         WriteLine($"参数个数:{str.ArgumentCount}"); 
         //遍历参数
         for(int i=0;i<str.ArgumentCount;i++){
             WriteLine($"argument {i}:{str.GetArgument(i)}");
         }
         //返回所有参数
         Object[] allargu=str.GetArguments();
         for(int i=0;i<allargu.Length;i++){
             WriteLine($"argument {i}:{allargu[i]}");
         }        
     }
```
##### 如何输出花括号 {}呢 转义输出 {{}}
```C#
   FormattableString str=$"The Age Of {{}} is {x+y}";
```
#### <a id="StanderedFormattableString">2.1&nbsp;&nbsp;  `标准字符串方法格式字符串` </a> :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
* [`标准数字格式字符串`](https://docs.microsoft.com/zh-cn/dotnet/standard/base-types/standard-numeric-format-strings)
```
标准数字格式字符串用于格式化通用数值类型。 标准数字格式字符串采用 Axx 的形式，其中：

A 是称为“格式说明符”的单个字母字符。 任何包含一个以上字母字符（包括空白）的数字格式字符串都被解释为自定义数字格式字符串。 
有关更多信息，请参见自定义数字格式字符串。

xx 是称为“精度说明符”的可选整数。 精度说明符的范围从 0 到 99，并且影响结果中的位数。 请注意，精度说明符控制数字的字符串
表示形式中的数字个数。 它不舍入该数字。 若要执行舍入运算，请使用 Math.Ceiling、Math.Floor 或 Math.Round 方法。
```
##### C 货币符号
`“C”（或货币）格式说明符将数字转换为表示货币金额的字符串。 精度说明符指示结果字符串中的所需小数位数。 如果省略精度说明符，则默认精度由 NumberFormatInfo.CurrencyDecimalDigits 属性定义。` `结果字符串受当前` [`NumberFormatInfo`](https://msdn.microsoft.com/zh-cn/library/system.globalization.numberformatinfo(v=vs.110).aspx) `对象的格式信息的影响。 下表列出了 NumberFormatInfo 属性，这些属性控制返回字符串的格式。`

---
|NumberFormatInfo属性	|描述|   
|:----|:-----|
|`CurrencyPositivePattern`	|定义正值的货币符号的位置。|
|`CurrencyNegativePattern`	|定义负值的货币符号的位置，并指定负号由括号表示还是由 NegativeSign 属性表示。|
|`NegativeSign`	|定义在 CurrencyNegativePattern 指明不使用括号时使用的负号。|
|`CurrencySymbol`	|定义货币符号。|
|`CurrencyDecimalDigits`|	定义货币值中的默认小数位数。 可使用精度说明符重写此值。|
|`CurrencyDecimalSeparator`|	定义分隔整数位和小数位的字符串。|
|`CurrencyGroupSeparator`|	定义分隔整数的组的字符串。|
|`CurrencyGroupSizes`|	指定在组中显示的整数位数。|


```C#
      decimal value = 123.456m;
      Console.WriteLine("Your account balance is {0:C2}.", value);     //自动识别当地货币符号        
      Console.WriteLine("Your account balance is $ {0:f}.", value);
      //Your account balance is ￥12,345.68.
      //Your account balance is $ 12345.68.
```
##### 十进制 `D`
```C#
      int value; 

      value = 12345;
      Console.WriteLine(value.ToString("D"));
      // Displays 12345
      Console.WriteLine(value.ToString("D8"));
      // Displays 00012345
      //错误用法: $"位置索引:{val.ToString("D3")} 匹配值: {match.Value}"  $里面不支持这个
      value = -12345;
      Console.WriteLine(value.ToString("D"));
      // Displays -12345
      Console.WriteLine(value.ToString("D8"));
      // Displays -00012345
```
##### 定点小数 F
```C#
      int integerNumber;
      integerNumber = 17843;
      // InvariantCulture 获取不依赖于区域性（固定）的 CultureInfo 对象。
      Console.WriteLine(integerNumber.ToString("F", 
                      CultureInfo.InvariantCulture));
      // Displays 17843.00

      integerNumber = -29541;
      Console.WriteLine(integerNumber.ToString("F3", 
                      CultureInfo.InvariantCulture));
      // Displays -29541.000

      double doubleNumber;
      doubleNumber = 18934.1879;

      Console.WriteLine(doubleNumber.ToString("F", CultureInfo.InvariantCulture));
      // Displays 18934.19

      Console.WriteLine(doubleNumber.ToString("F0", CultureInfo.InvariantCulture));
      // Displays 18934
```
##### 百分比  P
```C#
  double number = .2468013;
  Console.WriteLine(number.ToString("P", CultureInfo.InvariantCulture));
  // Displays 24.68 %
  Console.WriteLine(number.ToString("P", 
                    CultureInfo.CreateSpecificCulture("hr-HR")));
  // Displays 24,68%     
  Console.WriteLine(number.ToString("P1", CultureInfo.InvariantCulture));
  // Displays 24.7 %
```
##### 十六进制 X

```C#
   int value; 
   value = 0x2045e;
   Console.WriteLine(value.ToString("x"));
   // Displays 2045e
   Console.WriteLine(value.ToString("X"));
   // Displays 2045E
   Console.WriteLine(value.ToString("X8"));
   // Displays 0002045E

   value = 123456789;
   Console.WriteLine(value.ToString("X"));
   // Displays 75BCD15
   Console.WriteLine(value.ToString("X2"));
   // Displays 75BCD15
```

* [`标准日期和时间格式字符串`](https://docs.microsoft.com/zh-cn/dotnet/standard/base-types/standard-date-and-time-format-strings)

#### <a id="StanderedSample">2.2&nbsp;&nbsp;  `正则表达式小例子` </a> :closed_umbrella: <a href="#top"> `置顶`  </a>
 
正则表达式命名空间: [`using System.Text.RegularExpressions`](https://msdn.microsoft.com/zh-cn/library/system.text.regularexpressions(v=vs.110).aspx)
##### 匹配以p开头以ed结尾的单词
```C#
        static void Main(string[] args)
        {
            const String input=@"ped On April 18, 2002, he participated in the same song and walked into Pudong, 
            Shanghai,
            and sang the song ""Five 
            hundred years to the sky"" [11]. On October 18th, he participated in the opening ceremony of 
            the 11th China Golden Rooster and Hundred Flowers Film Festival and sang the song ""Five hund
            red years to the sky"" [12]. In the same year, he also partipated in the opening ceremony and 
            closing ceremony of the Hunan Golden Eagle Festival TV Awards Gala [13]. In 2003, he participated in th
            e third anniversary of CCTV's ""Same Song"". On September 25th, he participated in the opening ceremony of
             the first International Dongba Culture and Art Festival in Lijiang, China,
              and sang the song ""Nasi Girl"". On October 26th, he participated in the opening ceremony of the 18th
              World Hakkas, and sang the song
             ""The Solid Man"", and co-song the song ""My Chinese Heart"" with Sun Nan, Tian Zhen, Wu Hao and so on. [14]
              On November
              1st, paicipaed in the opening ceremony of the 12th China Golden Rooster and Hundred Flowers Film Festival.";    
            // 匹配模式
            const string pattern= @"\bp[a-zA-Z]*ed\b";
            // MatchCollection 匹配结果集 
            
            var rel=mathces.Count==0? "失败":"成功";
            WriteLine($"  匹配结果:{rel}");
            
            MatchCollection mathces=Regex.Matches(input,pattern,RegexOptions.IgnoreCase|RegexOptions.ExplicitCapture);
            // 每一个匹配结果 Value 匹配到的结果 Index表示 匹配结果在 文本中的位置索引
            foreach(Match nextMatch in mathces){
                WriteLine($"匹配值: {nextMatch.Value} 位置索引:{nextMatch.Index}");
            }    
        }
        
        /*匹配值: ped 位置索引:0
          匹配值: participated 位置索引:26
          匹配值: participated 位置索引:180
          匹配值: partipated 位置索引:394
          匹配值: participated 位置索引:532
          匹配值: participated 位置索引:629
          匹配值: participated 位置索引:824
          匹配值: paicipaed 位置索引:1056*/
```

##### Regex.Matches(text,pattern,RegexOptions.value| RegexOptions.value2) 方法
* `第一个参数: 待匹配文本`
* `第二个参数: 匹配模式,正则表达式`
* `第三个参数`:`匹配选项: RegexOptions 枚举类,选择是否忽略大小写,是否多行匹配,修改收集匹配的方式,方法是确保把显示置顶的匹配作为有效的搜索结果`

|RegexOptions 枚举值|说明|
|:----|:-----|
|ECMAScript|	为表达式启用符合 ECMAScript 的行为。 可以使用此值仅在结合 IgnoreCase, ，Multiline, ，和 Compiled 值。 该值与其他任何值一起使用均将导致异常。|
|ExplicitCapture| 指定唯一有效的捕获是显式命名或编号的 (?<name>…) 形式的组。 这使未命名的圆括号可以充当非捕获组，并且不会使表达式的语法 (?:...) 显得笨拙。|
|IgnoreCase|	指定不区分大小写的匹配| 
|Multiline|多行模式。 更改 ^ 和 $ 的含义，使它们分别在任意一行的行首和行尾匹配，而不仅仅在整个字符串的开头和结尾匹配。|
|Singleline|指定单行模式。 更改点 （.） 的含义 使其匹配 （而不是除 \n 之外的所有字符） 的每个字符。|
|	RightToLeft|	指定搜索从右向左而不是从左向右进行。 |

##### FindValue 方法 查找是否匹配
```C#
      public static bool FindValue(String text,String Pattern,Boolean isIgnoreCase){
          try{
            if(isIgnoreCase){
                MatchCollection mathces=Regex.Matches(text,Pattern,RegexOptions.IgnoreCase|RegexOptions.Multiline);
                return mathces.Count> 0;
            }else{
                MatchCollection mathces=Regex.Matches(text,Pattern,RegexOptions.Multiline);
                 return mathces.Count> 0;
            }  
          }catch(Exception ex){
              throw ex;
          }
      }   
```
#### <a id="PipeiGroupCapture">2.2&nbsp;&nbsp;  `匹配 组合捕获` </a> :closed_umbrella: <a href="#top"> `置顶` </a>
`匹配URl 链接`
```C#
       const String input="  https://www.sicnu.normal.sd.asd:8080 https://www.sicnu.normal.sd.asd:808880 http://www.baidu.com ttps://www.sicnu.normal.sd.asd:8080";

        String pattern=@"\b(http[s]?)(\:\/\/)(([\w]+\.)+[\w]+)([\s:]([\d]{2,5})?)?\b";
        var regular=new Regex(pattern);
        bool isOK=regular.IsMatch(input);
        WriteLine($"是否匹配到了:{isOK}");
        MatchCollection Matches=regular.Matches(input);
        foreach(Match match in Matches){
             WriteLine("位置索引:"+match.Index.ToString("D3") +$" 匹配值: {match.Value}");
            foreach(Group g in match.Groups){
                if(g.Success){
                    WriteLine($"Group index: {g.Index},value: {g.Value}");
                }
            }
            WriteLine();
        }   
 /* 结果:
        是否匹配到了:True
        位置索引:002 匹配值: https://www.sicnu.normal.sd.asd:8080
        Group index: 2,value: https://www.sicnu.normal.sd.asd:8080
        Group index: 2,value: https
        Group index: 7,value: ://
        Group index: 10,value: www.sicnu.normal.sd.asd
        Group index: 27,value: sd.
        Group index: 33,value: :8080
        Group index: 34,value: 8080

        位置索引:039 匹配值: https://www.sicnu.normal.sd.asd:
        Group index: 39,value: https://www.sicnu.normal.sd.asd:
        Group index: 39,value: https
        Group index: 44,value: ://
        Group index: 47,value: www.sicnu.normal.sd.asd
        Group index: 64,value: sd.
        Group index: 70,value: :

        位置索引:078 匹配值: http://www.baidu.com
        Group index: 78,value: http://www.baidu.com
        Group index: 78,value: http
        Group index: 82,value: ://
        Group index: 85,value: www.baidu.com
        Group index: 89,value: baidu.
        Group index: 98,value:
    */
``` 
##### RegexOptions.ExplicitCapture的影响 可以加快匹配速度 通过减少分组
```C#
        const String input="  https://www.sicnu.normal.sd.asd:8080 https://www.sicnu.normal.sd.asd:808880 http://www.baidu.com ttps://www.sicnu.normal.sd.asd:8080";

        String pattern=@"\b(http[s]?)(\:\/\/)(([\w]+\.)+[\w]+)([\s:]([\d]{2,5})?)?\b";
        var regular=new Regex(pattern,RegexOptions.ExplicitCapture);
        //  Regex构造函数
        //  Regex() 
        //  Regex(String pattern)
        //  Regex(String pattern, RegexOptions)	 新实例初始化 Regex 为指定的正则表达式，用修改模式的选项。
        bool isOK=regular.IsMatch(input);
        WriteLine($"是否匹配到了:{isOK}");
        MatchCollection Matches=regular.Matches(input);
        foreach(Match match in Matches){
             WriteLine("位置索引:"+match.Index.ToString("D3") +$" 匹配值: {match.Value}");
            foreach(Group g in match.Groups){
                if(g.Success){
                    WriteLine($"Group index: {g.Index},value: {g.Value}");
                }
            }
            WriteLine();
        }  
        /*
        是否匹配到了:True
        位置索引:002 匹配值: https://www.sicnu.normal.sd.asd:8080
        Group index: 2,value: https://www.sicnu.normal.sd.asd:8080

        位置索引:039 匹配值: https://www.sicnu.normal.sd.asd:
        Group index: 39,value: https://www.sicnu.normal.sd.asd:

        位置索引:078 匹配值: http://www.baidu.com
        Group index: 78,value: http://www.baidu.com
        
        */
```
##### 替换Replace
```C#
        const String input="  https://www.sicnu.normal.sd.asd:8080 https://www.sicnu.normal.sd.asd:808880 http://www.baidu.com ttps://www.sicnu.normal.sd.asd:8080";

        String pattern=@"\b(http[s]?)(\:\/\/)(([\w]+\.)+[\w]+)([\s:]([0-9]{2,4}))?\b";
        var regular=new Regex(pattern,RegexOptions.ExplicitCapture);
        bool isOK=regular.IsMatch(input);
        WriteLine($"是否匹配到了:{isOK}");
        String replaceResult=regular.Replace(input,"匹配到的合格的URL网址");
        WriteLine(replaceResult);
        
        // 输出:是否匹配到了:True
                匹配到的合格的URL网址 匹配到的合格的URL网址:808880 匹配到的合格的URL网址 ttps://www.sicnu.normal.sd.asd:8080
```

##### [Regex](https://msdn.microsoft.com/zh-cn/library/system.text.regularexpressions.regex(v=vs.110).aspx) 有许多的方法 
*  Match
*  Matchs
*  Replace
* 	IsMatch
*  Split
