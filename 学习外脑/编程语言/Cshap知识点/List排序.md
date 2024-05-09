#编程语言/Csharp #永久 
1.   .Sort() 直接排序
2. 自定义类的排序  可以继承接口 `IComparable`
3.  通过委托函数进行排序

## 继承`IComparable<>`接口
```csharp
class Test : IComparable<Test>
{
    public int value;
    public Test(int v)
    {
        this.value = v;
    }
    public int CompareTo(Test? other)
    {
        return this.value - other.value;
    }
}

list.Sort();
```

## 委托函数排序
```csharp
class Test
{
    public int value;
    public Test(int v)
    {
        this.value = v;
    }
}
static int MySort(Test t1, Test t2)
{
    return t1.value - t2.value; 
}

list.Sort(MySort);
```
