#编程语言/Csharp #永久 

# 相关函数
```c#
string str = "0123456789";
for (int i = 0; i < 26; i++)
{
    char ch1 = 'a', ch2 = 'A';
    str += (char)(ch1 + i);
    str += (char)(ch2 + i);
}
Console.WriteLine(str);
char[] chs = str.ToCharArray();       
int num = str.LastIndexOf("456");     // 找子串返回下标
str = str.Replace("567", "666");      // 替换指定字符串
str = str.ToUpper();                  // 转换大小写
str = str.Substring(2);               // 截取[index, index + len) 的字符串index + len默认INF
str = str.Remove(3, 5);               // 移出[index, index + len) 的字符串 默认INF

str = "12, 3   4, 5   55, 666, 789";
str = str.Replace(" ", "");           // 字符串去空格 Trim()只能去首尾
string[] strs = str.Split(',');       // 字符串切割
foreach (string s in strs) Console.WriteLine(s);

Console.WriteLine(str);

```

# StringBuilder



```c#
StringBuilder s = new StringBuilder("1");
// 增
s.Append("23");
s.AppendFormat("{0}先生", "黄");    
s.Insert(s.Length, "aaaa");                // 0) 插入
// 删
s.Remove(3, 1);     // [index, index + len)
// 改 + 查
s[0] = 'a';         // s.Replace
// 重新赋值
s.Clear();
s.Append("");

```

![[如何优化内存]]