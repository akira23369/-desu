#编程语言/Csharp  #永久 


# c的强转

```csharp
float f = 1.111f ;
int i = (int)1.111f;
Console.WriteLine(i);
```


# 类型.Parse("能够转换的字符串")

```c#
//i = int.Parse("123.1");         // 会报错
i = int.Parse("123");
Console.WriteLine(i);
Console.WriteLine(float.Parse("1.111"));
```

Convert.ToXXX(要转换的类型)

```c#
bool flag = true;
int a = Convert.ToInt32(true);      // a == 1
Console.WriteLine(a);
Console.WriteLine(Convert.ToInt32(flag));
```


