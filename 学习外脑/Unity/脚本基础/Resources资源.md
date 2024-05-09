#Unity #永久 

Resources 动态资源加载是 Unity 中一种常用的方式，用于在运行时动态加载项目中的资源文件，比如预制体、纹理、音频、动画等。这种加载方式通常用于需要在代码中动态加载资源的情况, 避免拖拖拽拽，比如根据游戏进程加载不同的关卡、角色模型等。


常用资源类型
1. 预设体对象    GameObject
2. 音效文件      AudioClip
3. 文本文件      TextAsset
4. 图片文件      Texture

## Resources同步加载
### 预设体对象（GameObject
假设有一个名为 "MyPrefab" 的预制体，它存储在 "Assets/Resources/Prefabs" 文件夹下。你可以使用以下代码动态加载这个预制体：
**注意Resources文件夹如果有同名的话, 会将其它Resources文件夹合并**
```csharp
// 这里只是把预制体这个配置文件加载到内存, 还没实例化
GameObject obj = Resources.Load<GameObject>("Prefabs/Cube");
Instantiate<GameObject>(obj);
```

### 音效文件（AudioClip）：
假设有一个名为 "MySound" 的音效文件，它存储在 "Assets/Resources/Sounds" 文件夹下。可以使用以下代码动态加载这个音效文件：
```csharp
// 加载音效数据
AudioClip audioClip = Resources.Load<AudioClip>("Sounds/Loves Me Not - t.A.T.u.");
// 通过Unity封装好的组件来使用数据, 不用实例
audioSc.clip = audioClip;
audioSc.Play();
```

### 文本文件 (TextAsset)
假设有一个名为 "MyTextFile" 的文本文件，它存储在 "Assets/Resources/Txt" 文件夹下。可以使用以下代码动态加载这个文本文件：
```csharp
// Unity支持的的文本格式
// txt, xml, bytes, json, html
TextAsset txt = Resources.Load<TextAsset>("Txt/Test");
print(txt.text);
print(txt.bytes);
```

### 图片文件（Texture）：
假设你有一个名为 "MyImage" 的图片文件，它存储在 "Assets/Resources/Textures" 文件夹下。你可以使用以下代码动态加载这个图片文件：
```csharp
private Texture tex;

tex = Resources.Load<Texture>("Textures/《原神》神里绫华4k电脑桌面壁纸_花猫壁纸(huamaobizhi.com)_1a75ce61");

void OnGUI()
{
    GUI.DrawTexture(new Rect(0, 0, 198 * 4, 108 * 4), tex);
}
```

ps:
可以加载所有同名的,但消耗性能
Load一次会触发缓存机制, 不会每次都给内存






## Resources异步加载

如果加载一个大一点的资源, 会造成主线程卡顿(1s跑不满60fps)
可以开一个线程进行资源加载

### Resources异步加载方法
```csharp
 ResourceRequest rrq = Resources.LoadAsync<Texture>("Textures/神里绫华4k电脑桌面壁纸_花猫壁纸(huamaobizhi.com)_1a75ce61");
 rrq.completed += LoadOver; 

private void LoadOver(AsyncOperation rq)
{
	tex = (rq as ResourceRequest).asset as Texture;
}

// 使用注意要判空, 多线程至少要一帧才加载完
if (tex != null) GUI.DrawTexture(.....)
```


### 在协程函数里面异步加载
```csharp
public class ResourceLoader : MonoBehaviour
{
    private GameObject loadedPrefab;
    private void Start()
    {
        StartCoroutine(LoadPrefabAsync());
    }
    private IEnumerator LoadPrefabAsync()
    {
        // 异步加载预制体
        ResourceRequest request = Resources.LoadAsync<GameObject>("Prefabs/MyPrefab");
        // 等待加载完成
        while (!request.isDone)
        {
            yield return null;
        }
        
		// 也可以直接返回request, Unity会自动判断资源加载完毕后才继续执行后面的
		yield return request;
		
        // 加载完成后获取预制体对象
        loadedPrefab = request.asset as GameObject;
        // 使用加载的预制体对象
        if (loadedPrefab != null)
        {
            Instantiate(loadedPrefab, transform.position, Quaternion.identity);
        }
        else
        {
            Debug.LogError("Failed to load prefab!");
        }
    }
}
```


### 异步加载方法 VS 协程异步加载
1 
- 好处: 写法简单
- 坏处: 只能资源加载后处理
线性加载

2
- 好处: 在协程中处理复杂逻辑, 比如: 进度条更新
- 坏处: 写法稍微麻烦
并行加载


## 资源卸载
资源第一次加载到内存中，同时会将加载的资源对象缓存起来
如果发生重复加载会先看缓存, 不会再次加载到内存

### 卸载指定资源 Resources.UnloadAsset
注意:
[[为什么不能通过卸载资源的方式卸载GameObject及其脚本对象]]
只能用于**纹理, 预制体, 文本文件, shader**等等
