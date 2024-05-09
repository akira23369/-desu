#Unity #永久 

[[多线程]]
Unity支持多线程, 但新开线程无法访问Unity相关对象的内容(如transform, 等场景上某一信息)

Unity多线程作用: 
一般开线程来处理复杂逻辑, 如 A*, 网络 然后把处理好的结果放在像公共成员容器让主线程来用就行


协程概念:**分时分步执行的函数** 迭代器函数MoveNext实现分步,  yield return 实现逻辑分时
主要作用:**把可能对主线程卡顿的逻辑分时分步执行**
使用场景:
1. 异步加载文件
2. 异步下载文件
3. 场景异步加载
4. 批量创建防止卡顿

 **注意:**
 只有当组件失活时协程不会停止, 其它情况都不会停止协程
 

### 协程函数的申明
![[迭代器#yield return 语法糖]]

```csharp
// 协程函数返回值必须是IEnumerator或者其子类   
IEnumerator MyCoroutine(int a, string b)
{
    Debug.Log(a);
    yield return new WaitForSeconds(2f);
    Debug.Log(b);
}
```

### 协程调用
Unity提供的协程协调器来启动协程
`StartCoroutine(MyCoroutine());`

手动调度
```csharp
// 手动执行自己的协程函数
IEnumerator cor = MyCoroutine();
while (cor.MoveNext())
{
    print("协程函数的各个yield return返回值" + cor.Current);
}
```




### 协程关闭
```csharp
Coroutine crt = StartCoroutine(MyCoroutine(1, "hello"));
StopAllCoroutines();        // 关闭所有协程
StopCoroutine(crt);         // 关闭指定协程
```

 Unity中yield return 可以返回的内容
**`yield return null 或者 数字;`**：暂停一帧后继续执行。这通常用于在协程中实现延迟操作或者等待下一帧执行某些逻辑。在Update 和 LateUpdate之间执行
**`yield return new WaitForSeconds(seconds);`**：暂停指定时间后继续执行。在Update 和 LateUpdate之间执行
**`yield return new WaitForFixedUpdate();`**：暂停到下一次固定帧更新后继续执行。这通常用于等待物理更新后再执行一些物理相关的逻辑。FixedUpdate和碰撞检测函数后
`yield return new WaitForEndOfFrame()`等待摄像机和GUI渲染后执行  LateUpdate之后
特殊类型对象, 比如异步加载相关函数返回的对象. Update 和 LateUpdate之间

### 停止协程`yield break`
### eg:
#### 分时分步创建大量物体
```csharp
IEnumerator MyCoroutine(int num)
{
	for (int i = 0; i < num; i++)
	{
		GameObject obj = GameObject.CreatePrimitive(PrimitiveType.Sphere);
		obj.transform.position = new Vector3(Random.Range(-100, 100), Random.Range(-100, 100), Random.Range(-100, 100));
		// 每一帧创建1000个, 而不是一口气在一帧创建100000个
		if (i % 1000 == 0) yield return null;
	}
}
```

#### 手动实现协程协调器
```csharp
public class YieldReturnTime
{
    public IEnumerator ie;
    public float time;
}

public class MyStartCorountine : MonoBehaviour
{
    private static MyStartCorountine _instance;
    public static MyStartCorountine Instance => _instance;
    public List<YieldReturnTime> list;
    private void Awake()
    {
        _instance = this;
    }

    void Update()
    {
        for (int i = list.Count - 1; i >= 0; i--)
        {
            // 如果有满足条件的
            if (list[i].time <= Time.time)
            {
                if (list[i].ie.MoveNext())
                {
                    if (list[i].ie.Current is int)
                    {
                        list[i].time = Time.time + (int)list[i].ie.Current;
                    }
                    else
                    {
                        // 如果不是这个类型就应该放在别的容器中 
                        list.RemoveAt(i);
                    }
                }
                else
                {
                    list.RemoveAt(i);
                }
            }
        }
    }

    public void MyStart(IEnumerator ie)
    {
        if (ie.MoveNext())
        {
            if (ie.Current is int)
            {
                YieldReturnTime y = new YieldReturnTime();
                y.ie = ie;
                y.time = Time.time + (int)ie.Current;
                list.Add(y);
            }
        }
    }
}
```

使用SortedSet<>改良版本
```csharp
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class CoroutineManager : MonoBehaviour
{
    private static CoroutineManager _instance;
    public static CoroutineManager Instance => _instance;

    private SortedSet<YieldReturnTime> queue;

    private void Awake()
    {
        _instance = this;
        queue = new SortedSet<YieldReturnTime>(new YieldReturnTimeComparer());
    }

    void Update()
    {
        // 执行待执行的协程
        ExecuteCoroutines();
    }

    public void StartCoroutineAtTime(IEnumerator coroutine, float time)
    {
        if (coroutine == null || time < Time.time)
        {
            Debug.LogWarning("Coroutine or time is invalid.");
            return;
        }

        YieldReturnTime y = new YieldReturnTime();
        y.ie = coroutine;
        y.time = time;
        queue.Add(y);
    }

    private void ExecuteCoroutines()
    {
        while (queue.Count > 0 && queue.Min.time <= Time.time)
        {
            YieldReturnTime y = queue.Min;
            queue.Remove(y);
            if (y.ie.MoveNext())
            {
                if (y.ie.Current is int)
                {
                    y.time = Time.time + (int)y.ie.Current;
                    queue.Add(y);
                }
                else
                {
                    // 处理不是 int 类型的情况
                }
            }
            else
            {
                // 协程执行完毕
            }
        }
    }

    // 比较器用于 SortedSet 排序
    private class YieldReturnTimeComparer : IComparer<YieldReturnTime>
    {
        public int Compare(YieldReturnTime x, YieldReturnTime y)
        {
            return x.time.CompareTo(y.time);
        }
    }
}

public class YieldReturnTime
{
    public IEnumerator ie;
    public float time;
}
```


 