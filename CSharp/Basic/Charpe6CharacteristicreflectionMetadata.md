<a href="#top" id="top" >:maple_leaf: 特性,反射,元数据,动态编程</a>	:blue_heart:
----
- [x] <a href="#Characteristic">`C# 特性`</a>
  - [x] <a href="#Obsolute">`Obsolute 特性`</a>:`弃用方法`
  - [x] <a href="#Conditional">`Conditional 特性`</a>:`取消调用`
  - [x] <a href="#DebuggerStepThrough">`DebuggerStepThrough 特性`</a>:`跳过单步调试`
  - [x] <a href="#ConditionalByYourself">`编写一个自定义特性`</a>:`取消调用`
- [x] <a href="#typeReflection">`反射`</a>
  - [x] <a href="#SystemType">`System.Type 类`</a>:`T`
  
#### C# 特性 <a id="Characteristic"></a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>

```C# 
  //命名空间
  using System.Diagnostics;
  using System.Runtime.CompilerServices;
```

##### 概念
`在.NET使用特性，可以有效地将元数据或声明性信息与代码（程序集、类型、方法、属性等）相关联。 将特性与程序实体相关联后，可以在运行时使用反射这项技术
查询特性。`

* **`使用方法`**:`括号即可`
```C#
  [Serializable]
  public class SampleClass
  {
      // Objects of this type can be serialized.
  }
```

#####  <a id="Obsolute"></a> Obsolute 特性

* `Obsolute其实是一个类 有两个构造函数 声明为`: **`public sealed class ObsoleteAttribute : Attribute `** `可以将当前一个方法标记为一个旧的方法`

```C#
    // 摘要:新实例初始化 System.ObsoleteAttribute 使用指定的变通方法消息。
    // 参数: message:描述了可选的变通方法文本字符串。
    public ObsoleteAttribute(string message);
    // error: true 如果使用过时的元素将生成编译器错误; false 如果它将生成编译器警告。
    public ObsoleteAttribute(string message, bool error);
```

```C#
    //表示这个方法以及被弃用了,但是还是可以调用 但是编译器会在调用这个方法的地方画下划线 
    [Obsolete("You should use new Function that Name is NewChangeState")]
    public int ChangeStats(int i)
    {
        return i + 10;
    }
```

* `如果使用构造函数声明`

```C#
    //编译器会在调用这个方法的地方画 蓝色 下划线 并且会提示 
    // `You should use new Function that Name is NewChangeState`
    [Obsolete("You should use new Function that Name is NewChangeState")]
    public int ChangeStats(int i)
    {
        return i + 10;
    }
```

* **`第二个参数`**:`是否告诉编译器不能够编译通过此方法`

```C#
    //编译器会在调用这个方法的地方画 红色 下划线 并且会提示 
    // `You should use new Function that Name is NewChangeState`
    // 并且不能够通过编译
    [Obsolete("You should use new Function that Name is NewChangeState")]
    public int ChangeStats(int i)
    {
        return i + 10;
    }
```
#####  <a id="Conditional"></a> Conditional 特性
`如果没有使用#define IsTest 定义  IsTest 那么调用这个方法的地方 方法不会生效`
* `只能用于无返回值方法`
```C#
    [Conditional("IsTest")]
    public static void wahr()
    {
        Console.WriteLine("没有调用我的");
    }
    //主函数调用这个方法后 毫无输出   
    consoleTest();
    consoleTest();
    consoleTest();
    consoleTest();

```
#####  <a id="Caller"></a> 调用者信息
* `CallerFilePath`:`在那个文件调用此方法 参数必须有默认值 值由编译器自动填入`
* `CallerLineNumber`:`调用行号  参数必须有默认值 值由编译器自动填入`
* `CallerMemberName`:`调用此方法的方法名称 参数必须有默认值 值由编译器自动填入`

```C#
    public void Print([CallerFilePath] string filename="Person.cs", [CallerLineNumber] int linenumer=0
        ,[CallerMemberName] string methodname="Print")
    {
        Console.WriteLine($"文件:{filename}");
        Console.WriteLine($"行号:{linenumer}");
        Console.WriteLine($"调用者名称:{methodname}");
    } 
```

#####  <a id="DebuggerStepThrough"></a> DebuggerStepThrough 特性
* `可以跳过单步调试,不让进入该方法,但这个方法确定无误的情况下`
```C#
    [DebuggerStepThrough]
    public int ChangeStats(int i)
    {
        return i + 10;
    }
```

##### <a href="#" id="ConditionalByYourself">编写一个自定义特性类 </a>
* `可通过定义特性类创建自己的自定义特性，特性类是直接或间接派生自 Attribute 的类，可快速轻松地识别元数据中的特性定义。`
* `类名称后缀为Attribute`
* `类一般声明为sealed`
* `类里面定义一些属性和字段,一般没有方法索引器等等`
* `AttributeTargets  指定特性可以适用的代码结构`

   ```C#
        // 摘要:特性可以应用于程序集。
        Assembly = 1,
        // 摘要: 特性可以应用于模块中。
        Module = 2,
        // 摘要:特性可以应用于类。
        Class = 4,
        // 摘要:特性可以应用于结构;即，类型值。
        Struct = 8,
        // 摘要:特性可以应用于枚举。
        Enum = 16,
        // 摘要:特性可以应用于构造函数。
        Constructor = 32,
        // 摘要:特性可以应用于方法。
        Method = 64,
        // 摘要:特性可以应用于属性。
        Property = 128,
        // 摘要:特性可以应用于字段。
        Field = 256,
        // 摘要:特性可以应用于事件。
        Event = 512,
        // 摘要:特性可以应用于接口。
        Interface = 1024,
        // 摘要:特性可以应用于参数。
        Parameter = 2048,
        // 摘要:特性可以应用于委托。
        Delegate = 4096,
        // 摘要:特性可以应用于返回的值。
        ReturnValue = 8192,
        // 摘要:特性可以应用于泛型参数。
        GenericParameter = 16384,
        // 摘要: 特性可以应用于任何应用程序元素。
        All = 32767
   ```
* 创建特性类   
```C#
    [AttributeUsage(AttributeTargets.Class)]
    public sealed class MyTestAttribute: Attribute
    {
        public string Description { get; set; }
        public string Version { get; set; }
        public int ID { get; set; }
        public MyTestAttribute(string Dec) {
            this.Description = Dec;
        }
        public MyTestAttribute()
        {

        }
    }
```
* 适用特性类
  * `可以通过构造函数初始化 也可以通过属性名称=value 在声明中初始化`
```C#
    Person p = new Person();
    Type val = p.GetType();
    object[] array = val.GetCustomAttributes(false); //指定是否搜索继承的父类的特性 特性可能不止一个
    MyTestAttribute attr = array[0] as MyTestAttribute;
    Console.WriteLine($"描述：{attr.Description}");
 ```
##### <a href="#" id="typeReflection">反射 </a>
`反射提供描述程序集、模块和类型的对象（Type 类型）。 可以使用反射动态地创建类型的实例，将类型绑定到现有对象，或从现有对象中获取类型，然后调用其方法或访问其字段和属性。 如果代码中使用了特性，可以利用反射来访问它们`
##### <a href="#" id="SystemType">System.Type 类 </a>
`使用Type类的目的是存储数据类型的引用,本质上是一个抽象的基类 实例化它的方法如下`
```c#
  int i = 42;
  Type f_type = typeof(float);
  System.Type type = i.GetType();
  System.Console.WriteLine(type.Name);
  System.Console.ReadKey();
```
* `有用的属性`
   * `Name`:`数据类型名称`
   * `FullName`:`数据类型完全名称 包含命名空间`
   * `NameSpace`:`数据类型所属的命名空间`
