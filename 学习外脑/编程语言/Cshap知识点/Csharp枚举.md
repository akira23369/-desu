#编程语言/Csharp #永久 


```c#
namespace 枚举
{
    // 枚举一般声明在namespace中

    // 声明了一个E_MonsterType 的枚举类型
    enum E_MonsterType
    {
        Normal,
        Boss,
    }

    enum E_PlayerType
    {
        Main,
        Other,
    }

    internal class Program
    {
        static void Main(string[] args)
        {
            //使用枚举类型
            E_PlayerType a = E_PlayerType.Main;

            // 枚举的强转
            int i = (int)a;
            Console.WriteLine(i);
            Console.WriteLine(a);
            Console.WriteLine(a.ToString());        // 和上面一样的

            // 把string强转为枚举
            a = (E_PlayerType)Enum.Parse(typeof(E_PlayerType), "Other");
            Console.WriteLine(a);
            Console.ReadKey(true); 
        }
    }
}

```