#编程语言/Csharp #永久 
```c#
// 函数定义
static void Change(ref int a)

// ref使用
int a;
ref int b = ref a;
b = 666;
Change(ref a);


// ref用法和c++中的&a一样必须初始化, 但out必须在函数内部去初始化

``` 