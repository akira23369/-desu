#编程语言/Csharp #永久 
1.表达式Lambda，表达式为其主体：
`(input-parameters) => expression`
2.语句Lambda，语句块作为其主体：
`(input-parameters) => {<sequence-of-statements>}`


可以将lambad表达式 理解为匿名函数的简写
使用上和[[匿名函数]]一模一样
都是和[[委托]]或者[[事件]] 配合使用的




**[[匿名函数]]**
```csharp
delegate (参数列表)
{

};
```

**Lambda表达式**
```csharp
(参数列表) =>
{
    //函数体
};
```

**eg:**
```csharp
Func<int, int> square = x => x * x;
Console.Write(square(3)); // square(3) == 9

Func<int, string> f = (int val) =>
{
    return "这是数字" + val;
};
```


## Lambda表达式的简写
- 可以省略类型参数说明
- 只有一个参数可以省略()
- **方法体**只有一个返回值可以省略{}
- **方法体**只有一个省略`return`

## 闭包 !!!

^78a3d9

内层的函数可以引用包含在它外层的函数的变量
即使外层函数的执行已经终止
注意： 
- **该变量提供的值并非变量创建时的值，而是在父函数范围内的最终值。**
- ***Lambda表达式的函数内部可以享用外部的资源!!!!!!!!!!!!!!!!!!!!!!!!!!***[[封装GUI#^cf6658 |eg]]


```csharp
class Test
{
	public event Action action;
	public Test()
	{
		int val = 10;
		// 这里改变了val的生命周期
		action = () =>
		{
			Console.WriteLine(val);
		};
		for (int i = 0; i < 10; i++)
		{
			// 该变量提供的值并非变量创建时的值(1,2..)，而是在父函数范围内的最终值(10)!!!
			// 在 for (int i = 0; i < 10; i++) 中，变量 i 定义了一次。
			// 如果直接使用捕获的外部变量, 使用的外部变量的值是 其最终值
			// 解决方法 int tmp = i;
			action += () =>
			{
				Console.WriteLine(i);
			};
		}
	}
	public void DoSomething()
	{
		action();
	}
}
class Program
{
	static void Main(string[] args)
	{
		Test t = new Test();
		t.DoSomething();
	}
}

```

