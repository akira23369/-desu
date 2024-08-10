#编程语言/Csharp #算法/数据结构 

### 1. 数组（Array）

**定义**：数组是一种在内存中连续存储相同类型数据的数据结构。数组的每个元素都可以通过索引来访问。

**基本使用**：

- 声明并初始化：`int[] numbers = new int[5] { 1, 2, 3, 4, 5 };` 或 `int[] numbers = { 1, 2, 3, 4, 5 };`
- 访问元素：`int firstNumber = numbers[0];`
- 遍历数组：可以使用for循环或foreach循环。

### 2. 列表（List<T\>）

**定义**：List<T\>是ArrayList的泛型版本，提供了类型安全且无需装箱和拆箱操作的动态数组。

**基本使用**：

- 声明并初始化：`List<int\> numbers = new List<int> { 1, 2, 3, 4, 5 };`
- 添加元素：`numbers.Add(6);`
- 访问元素：`int secondNumber = numbers[1];`
- 遍历列表：可以使用for循环、foreach循环或LINQ查询。

### 3. 字典（Dictionary<TKey, TValue\>）

**定义**：Dictionary<TKey, TValue\>是一种集合，它存储键值对，并且每个键在集合中必须是唯一的。

**基本使用**：

- 声明并初始化：`Dictionary<string, int> ages = new Dictionary<string, int> { { "Alice", 30 }, { "Bob", 25 } };`
- 添加键值对：`ages.Add("Charlie", 35);`
- 访问值：`int aliceAge = ages["Alice"];`
- 遍历字典：可以使用foreach循环遍历字典的键、值或键值对。

### 4. 队列（Queue<T\>）

**定义**：Queue<T\>是一种先进先出（FIFO）的集合。

**基本使用**：

- 声明并初始化：`Queue<int> queue = new Queue<int>();`
- 入队：`queue.Enqueue(1);`
- 出队：`int firstItem = queue.Dequeue();`
- 查看队首元素：`int peekItem = queue.Peek();`

### 5. 栈（Stack<T\>）

**定义**：Stack<T\>是一种后进先出（LIFO）的集合。

**基本使用**：

- 声明并初始化：`Stack<int> stack = new Stack<int>();`
- 入栈：`stack.Push(1);`
- 出栈：`int topItem = stack.Pop();`
- 查看栈顶元素：`int peekItem = stack.Peek();`

### 6. 链表（LinkedList<T\>）

**定义**：LinkedList<T\>是一种双向链表，其中的每个节点都包含数据以及到列表中前一个和后一个节点的链接。

**基本使用**：

- 声明并初始化：`LinkedList<int> linkedList = new LinkedList<int>();`
- 添加元素：`linkedList.AddLast(1);`
- 访问元素：由于链表不是通过索引访问的，所以访问特定元素通常需要遍历链表。
- 遍历链表：可以使用foreach循环遍历链表中的节点。

### 7. 哈希表（Hashtable）

**定义**：Hashtable是一种基于哈希表的集合，它存储键值对，类似于Dictionary<TKey, TValue>，但它是非泛型的，因此在存储和检索时会进行装箱和拆箱操作。

**基本使用**（虽然Hashtable在泛型集合出现后使用较少，但了解其用法仍然有价值）：

- 声明并初始化：`Hashtable hashtable = new Hashtable();`
- 添加键值对：`hashtable.Add("key", "value");`
- 访问值：`string value = (string)hashtable["key"];`
- 遍历哈希表：可以使用foreach循环遍历Hashtable的键、值或键值对。


---


### 1. HashSet<T\>

**定义**：`HashSet<T>`是C#中的一个集合类型，用于存储不重复的元素。它实现了`ISet<T>`接口，并且基于哈希表实现，因此提供了快速的元素查找、添加和删除操作。

**特性**：

- **唯一性**：`HashSet<T>`中的元素必须是唯一的，即集合中不会存储重复的元素。
- **无序性**：与`SortedSet<T>`不同，`HashSet<T>`不保证元素的顺序。
- **高性能**：由于基于哈希表实现，`HashSet<T>`在添加、删除和查找元素时通常具有很高的性能。

**基本使用**：

- 创建`HashSet<T>`对象：`HashSet<int> hashSet = new HashSet<int>();`
- 添加元素：`hashSet.Add(1);`
- 删除元素：`hashSet.Remove(1);`
- 判断元素是否存在：`bool contains = hashSet.Contains(1);`
- 遍历元素：可以使用`foreach`循环遍历`HashSet<T>`中的元素。

### 2. BitArray

**定义**：`BitArray`是C#中的一个类，用于管理位值的紧凑数组。它提供了一个布尔值的数组，其中`true`表示位值为1，`false`表示位值为0。

**特性**：

- **压缩存储**：`BitArray`通过位操作来存储布尔值，相比于直接使用布尔数组，它可以更节省内存空间。
- **位操作**：提供了位操作的方法，如`Set`、`Get`和`SetAll`，用于修改和查询位值。

**基本使用**：

- 创建`BitArray`对象：`BitArray bitArray = new BitArray(8);` // 创建一个长度为8的BitArray
- 设置位值：`bitArray.Set(0, true);` // 将索引为0的位设置为1
- 获取位值：`bool value = bitArray.Get(0);` // 获取索引为0的位的值
- 遍历`BitArray`：虽然`BitArray`没有直接支持索引器，但可以通过遍历其长度并使用`Get`方法来检查每个位的值。

### 3. SortedDictionary<TKey, TValue>

**定义**：`SortedDictionary<TKey, TValue>`是C#中的一个集合类型，它根据键的排序顺序来存储键值对。与`Dictionary<TKey, TValue>`相比，它提供了按键排序的功能。

**特性**：

- **排序**：`SortedDictionary<TKey, TValue>`中的键值对会根据键的排序顺序（自然顺序或指定的`IComparer<TKey>`）进行排序。
- **唯一键**：每个键在`SortedDictionary<TKey, TValue>`中必须是唯一的。
- **高效查找**：由于内部实现了排序，因此`SortedDictionary<TKey, TValue>`在查找、插入和删除键值对时通常具有较好的性能。

**基本使用**：

- 创建`SortedDictionary<TKey, TValue>`对象：`SortedDictionary<int, string> sortedDict = new SortedDictionary<int, string>();`
- 添加键值对：`sortedDict.Add(1, "One");`
- 遍历键值对：可以使用`foreach`循环遍历`SortedDictionary<TKey, TValue>`中的键值对。
- 检索指定键的值：`string value; if (sortedDict.TryGetValue(1, out value)) { Console.WriteLine(value); }`
- 删除键值对：`sortedDict.Remove(1);`

### 4. SortedSet<T\>

**定义**：`SortedSet<T>` 是 C# 中一个泛型集合类型，用于存储不重复的元素，并且这些元素会自动按顺序排序。它实现了 `ISet<T>` 接口，内部基于平衡二叉树（红黑树）实现。

**特性**：

- **唯一性**：`SortedSet<T>` 中的元素必须是唯一的，即集合中不会存储重复的元素。
- **有序性**：`SortedSet<T>` 保证元素按顺序存储。默认情况下按自然顺序排序（例如，对于数字是从小到大排序），也可以通过提供自定义比较器来定义排序顺序。
- **高性能**：由于基于平衡二叉树实现，`SortedSet<T>` 在插入、删除和查找操作上具有良好的性能。

**基本使用**：
- 创建 `SortedSet<T>` 对象: `SortedSet<int> sortedSet = new SortedSet<int>();`
- 添加元素: `sortedSet.Add(1);`
- 删除元素: `sortedSet.Remove(1);`
- 判断元素是否存在: `bool contains = sortedSet.Contains(2); // true`