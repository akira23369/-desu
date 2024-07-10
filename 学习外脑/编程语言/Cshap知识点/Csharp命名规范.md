#编程语言/Csharp #永久 

以Google的代码规范为主，稍加改动 [https://google.github.io/styleguide/csharp-style.html](https://google.github.io/styleguide/csharp-style.html)
 [Unity项目代码书写规范_unity项目规范-CSDN博客](https://blog.csdn.net/yinfourever/article/details/108246594)

# CSharp代码规范

- https://docs.microsoft.com/zh-cn/dotnet/csharp/programming-guide/inside-a-program/coding-conventions
# 一、命名规约
## （一）命名风格
1.  【强制】代码中的命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式。
说明：正确的英文拼写和语法可以让阅读者易于理解，避免歧义。注意，即使纯拼音命名方式
也要避免采用。
> 正例：test / consume  
> 反例：ceshi / tongbu

2. 【强制】类名使用 UpperCamelCase 风格，必须遵从驼峰形式，但以下情形例外：DO / BO /
DTO / VO / AO  

> 正例：MarcoPolo / UserDO / XmlService / TcpUdpDeal / TaPromotion  
> 反例：macroPolo / UserDo / XMLService / TCPUDPDeal / TAPromotion

3. 【强制】方法名统一使用 UpperCamelCase 风格，必须遵从
驼峰形式。  
> 正例： Fetch / GetUserInfo / ReadWbs  
> 反例： getUserInfo / createReserveNo / buildParameter

4. 【强制】参数名、成员变量、局部变量都统一使用 lowerCamelCase 风格，必须遵从
驼峰形式。  
> 正例：GetApplicatonInfo(int applicationId) / var localInfo / private int sapSystemId  

5. 【强制】常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长。
> 正例：MAX_STOCK_COUNT  
> 反例：MAX_COUNT

6. 【强制】抽象类命名使用 Abstract 或 Base 开头；异常类命名使用 Exception 结尾；测试类
命名以它要测试的类的名称开始，以 Test 结尾

7. 【强制】中括号是数组类型的一部分，数组定义如下：String[] args;
反例：使用 String args[]的方式来定义。

8. 【强制】杜绝完全不规范的缩写，避免望文不知义。
反例：AbstractClass“缩写”命名成 AbsClass；condition“缩写”命名成 condi，此类随
意缩写严重降低了代码的可阅读性。

9. 【推荐】为了达到代码自解释的目标，任何自定义编程元素在命名时，使用尽量完整的单词
组合来表达其意。  
> 正例：从远程仓库拉取代码的类命名为 PullCodeFromRemoteRepository。  
> 反例：变量 int a; 的随意命名方式。

10. 接口命名以I开头，接口以名词形式出现
> 正例：ISapOperator  
> 反例：ISapOperate

11. 【推荐】如果模块、接口、类、方法使用了设计模式，在命名时体现出具体模式。
说明：将设计模式体现在名字中，有利于阅读者快速理解架构设计理念。
> 正例：  
public class OrderFactory;  
public class LoginProxy;  
public class ResourceObserver;  

12. 【参考】各层命名规约：  
- A) Service/DAO 层方法命名规约
    - 1） 获取单个对象的方法用 Get 做前缀。
    - 2） 获取多个对象的方法用 List 做前缀。
    - 3） 获取统计值的方法用 Count 做前缀。
    - 4） 插入的方法用 Save/Insert 做前缀。
    - 5） 删除的方法用 Remove/Delete 做前缀。
    - 6） 修改的方法用 Update 做前缀。
    - 
- B) 领域模型命名规约
    - 1） 数据对象：xxxDO，xxx 即为数据表名。
    - 2） 数据传输对象：xxxDTO，xxx 为业务领域相关的名称。
    - 3） 展示对象：xxxVO，xxx 一般为网页名称。
    - 4） POJO 是 DO/DTO/BO/VO 的统称，禁止命名成 xxxPOJO。
    

12. 【推荐】一个类一个文件，且文件名与类名保持一至

13. 【推荐】其它补充
    - 尽量保证类，方法，变量的命名的可读性
    - 尽量保证不要用缩写的名称
    - 如果要在变量长度与可读性之间做取舍，优先选择可读性
    - 从别的项目拷贝代码后保证方法名称与当前项目匹配
    - 很多程序员喜欢争论代码风格。别否认哦，类似的话题总能吵起来。Bill Sourour 认为：代码风格没有绝对的对错，只要团队代码风格统一就行了。Bill 觉得比较安全的做法：① 通过工具自动规范代码风格；② 参照名声好的大公司使用的代码风格。
    - 
## （二）常量定义

1.  【推荐】不要使用一个常量类维护所有常量，按常量功能进行归类，分开维护。
> 说明：大而全的常量类，非得使用查找功能才能定位到修改的常量，不利于理解和维护。  
> 正例：缓存相关常量放在类 CacheConsts 下；系统配置相关常量放在类 ConfigConsts 下。

2.  【推荐】常量的复用层次有五层：跨应用共享常量、应用内共享常量、子工程内共享常量、包
内共享常量、类内共享常量。
    - 1） 跨应用共享常量：放置在二方库中，通常是 client.dll 中的 constant 目录下。
    - 2） 应用内共享常量：放置在一方库中，通常是 modules 中的 constant 名称空间下。
    - 3） 子工程内部共享常量：即在当前子工程的 constant 名称空下。
    - 4） 包内共享常量：即在当前项目下单独的 constant 名称称空间下。
    - 5） 类内共享常量：直接在类内部 private static readonly 定义。
    -

## （三）代码格式
1. 【强制】注释的双斜线与注释内容之间有且仅有一个空格。
> 正例：// 注释内容，注意在//和注释内容之间有一个空格。

2. 【推荐】方法体内的执行语句组、变量的定义语句组、不同的业务逻辑之间或者不同的语义
之间插入一个空行。相同业务逻辑和语义之间不需要插入空行。


## 书写规范

### 基础写法

1. Pascal和驼峰混用，参数用驼峰写法，除参数外，都以Pascal写法为主。
2. 括号建议用换行方式书写

![](https://img-blog.csdnimg.cn/20200826200249421.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lpbmZvdXJldmVy,size_16,color_FFFFFF,t_70)

Code

- 类, 方法, 枚举, public 字段, public 属性, 命名空间的命名规则用: `PascalCase`. ^ef0934
- 局部变量，函数参数命名规则用: `camelCase`.
- private, protected, internal and protected internal 字段和属性的命名规则用: `_camelCase`.
- 命名规则不受const, static, readonly等修饰符影响.
- 对于缩写，也按`PascalCase` 命名，比如 `MyRpc`而不是`MyRPC`.
- 接口以`I`,开头..

Files

- 文件和文件夹 命名规则为`PascalCase`, 例如 `MyFile.cs`.
- 文件名尽量和文件中主要的类名一直, 例如 `MyClass.cs`.
- 通常，一个文件中一个类.

#### Organization

- 如果出现，修饰符按下列顺序书写: `public protected internal private new abstract virtual override sealed static readonly extern unsafe volatile async`.
- 命名空间在最顶部，using顺序按Sytem，Unity，自定义的命名空间以及字母顺序排序，
- 类成员的顺序:
    - Group按下列顺序:
        - Nested classes, enums, delegates and events.
        - Static, const and readonly fields.
        - Fields and properties.
        - Constructors and finalizers.
        - Methods.
    - 每个Group内，按下列顺序:
        - Public.
        - Internal.
        - Protected internal.
        - Protected.
        - Private.
    - 接口的实现尽可能安排写在一起

#### 注释规范

代码头部注释  
文件名称：文件的名称。  
功能描述：文件的功能描述与大概流程说明。  
作者：创建并编写的人员。  
日期：创建并编写的日期。  
修改记录：若类有所修改，则需要有修改人员的名字、修改日期及修改理由。

// 文件名称：UserInput.cs

// 功能描述：玩家输入按键的定义

// 编写作者：张三

// 编写日期：2017.7.16

// 修改记录：

//      R1:

//          修改作者：李四

//          修改日期：2017.7.17

//          修改理由：使玩家可以自定义输入按键

Using System;

**方法注释**

采用 /// 形式自动产生XML标签格式的注释。包括方法功能，参数含义，返回内容

    /// <summary>

    /// 设置场景的名字.

    /// </summary>

    /// <returns><c>true</c>, 场景名字设置成功, <c>false</c> 场景名字设置失败.</returns>

    /// <param name="sceneName">场景名字.</param>

    public bool SetSceneName(string sceneName)

    {

}

类变量注释

采用 /// 形式自动产生XML标签格式的注释变量含义。

    /// <summary>

    /// 场景的名字

    /// </summary>

    [private](https://so.csdn.net/so/search?q=private&spm=1001.2101.3001.7020) string mSceneName;

局部变量注释

在变量声明语句的后面注释，与前后行变量声明的注释左对齐，注释与代码间以Tab隔开。

string firstName;   //姓

string lastName;    //名

代码行注释

注释位于代码上行，与代码开始处左对齐，双斜线与注释之间以空格分开

//设置场景的名字。

this.mSceneName = sceneName;

#### 书写示例：

```csharp
using System;                                        //using写在整个文件最前，多个using按下面层级以及字母排序
using System.Collections;                            //1.system提供的
using System.Collections.Generic;
using UnityEngine;                                   //2.unity提供的
using UnityEngine.UI;
using GameDataModule;                                //3.自定义的namespace
 
namespace MyNamespace 								 // Namespaces 命名规则为 PascalCase.  
{      
	public interface IMyInterface                    // Interfaces 以 'I' 开头
	{          
		public int Calculate(float value, float exp);// 方法函数 命名规则为 PascalCase 
	}
 
	public enum MyEnum                               // Enumerations 命名规则为 PascalCase.
	{                               
		Yes = 0,                                     // Enumerations 命名规则为 PascalCase，并显示标注对应值
		No = 1,
	}
 
	public class MyClass 				        	// classes 命名规则为 PascalCase.
	{                          
		public int Foo = 0;                          // Public 公有成员变量命名规则为 PascalCase.
		public bool NoCounting = false;              // 最好对变量初始化.
		private class Results 
		{
			public int NumNegativeResults = 0;
			public int NumPositiveResults = 0;
		}
		private Results _results;                    // Private 私有成员变量命名规则为 _camelCase.
		public static int NumTimesCalled = 0;
		private const int _bar = 100;                // const 不影响命名规则.
		private int[] _someTable =                  
		{       
			2, 3, 4,                 
		}
		public MyClass()                             // 构造函数命名规则为 PascalCase.
		{
			_results = new Results                   // 对象初始化器最好用换行的方式赋值.
			{
				NumNegativeResults = 1,              // 操作符前后用个空格分割.  
				NumPositiveResults = 1,           
			};
		}
 
    public int CalculateValue(int mulNumber) 
	{      
        var resultValue = Foo * mulNumber;           // Local variables 局部变量命名规则为camelCase.
        NumTimesCalled++;
        Foo += _bar;
        if (!NoCounting)                             // if后边和括号用个空格分割.
	    {                                                                          
			if (resultValue < 0)
			{                                                                  
				_results.NumNegativeResults++        
			} 
			else if (resultValue > 0)
			{         
				_results.NumPositiveResults++;
			}
		}
		return resultValue;
    }
 
    public void ExpressionBodies() 
	{
      //对于简单的lambda，如果一行能写下，不需要括号
      Func<int, int> increment = x => x + 1;
 
      // 对于复杂一些的lambda，多行书写.
      Func<int, int, long> difference1 = (x, y) => 
	  {
          long diff = (long)x - y;
          return diff >= 0 ? diff : -diff;
      };
    }
 
    void DoNothing() {}                          // Empty blocks may be concise.
 
    
    void CallingLongFunctionName() 
	{
      int veryLongArgumentName = 1234;
      int shortArg = 1;
      // 函数调用参数之间用空格分隔
      AnotherLongFunctionNameThatCausesLineWrappingProblems(shortArg, shortArg, veryLongArgumentName);
	  // 如果一行写不下可以另起一行，与第一个参数对齐
      AnotherLongFunctionNameThatCausesLineWrappingProblems(veryLongArgumentName, 
															veryLongArgumentName, veryLongArgumentName);
    }
  }
}
```