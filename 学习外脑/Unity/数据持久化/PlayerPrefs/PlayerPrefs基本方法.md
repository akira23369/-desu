#Unity #永久 


`PlayerPrefs` 是 Unity 中用于在游戏中存储和检索玩家偏好设置（Player Preferences）的类。通过 `PlayerPrefs`，你可以轻松地存储和获取一些简单的数据，例如玩家的分数、游戏设置、解锁的关卡等。这些数据将在应用程序关闭后仍然保留，可以在下次启动应用程序时继续使用。

```csharp
// 存储相关     (类似键值对存储, 键:string, 值:int, float, string    键同名覆盖
// set相关方法只能存在内存, 游戏结束时, Unity会自动存到硬盘   (如果游戏崩溃, 数据会丢失
PlayerPrefs.SetInt("第几帅", 1);
PlayerPrefs.SetFloat("存个浮点数", 1.1f);
PlayerPrefs.SetString("黄", "先生");
// 调用该方法马上存到硬盘(各个平台默认位置)
PlayerPrefs.Save();

// 因为是GetInt, 所以没返回string类型的, 返回Int的默认值:0
// 如果填了第二个参数代表没找到就返回后的默认值, 可用来进行基础数据的初始化
string test = PlayerPrefs.GetString("黄");     

// 判断键是否已经存在
if (PlayerPrefs.HasKey("测试"))
{
	// 删除
	// 根据键来删除
	PlayerPrefs.DeleteKey("测试");
	// 删除所有键值对
	PlayerPrefs.DeleteAll();
}
```

### 注意事项：

- `PlayerPrefs` 适用于存储少量简单数据。如果需要存储大量数据或更复杂的结构，考虑使用其他持久性存储解决方案，如文件、数据库等。
- 存储在 `PlayerPrefs` 中的数据是明文存储的，对于一些重要的数据（例如玩家的分数），应该使用其他更安全的手段，如服务器存储。
- 在WebGL平台上，由于浏览器的安全策略，对 `PlayerPrefs` 的使用有一些限制。在这种情况下，可以考虑使用UnityWebRequest等其他方式来进行数据存储和检索。


## PlayerPrefs的存储位置
### Windows：
`PlayerPrefs` 存储在注册表中。具体位置为：
- 32位系统：`HKEY_CURRENT_USER\Software\[公司名]\[产品名]`
- 64位系统：`HKEY_CURRENT_USER\Software\WOW6432Node\[公司名]\[产品名]`

### Android：
在Android上，`PlayerPrefs` 存储在应用的持久数据目录中。具体路径为：`/data/data/[包名]/shared_prefs/unity.[公司名].[产品名]_prefs.xml`。

### Linux：
在Linux上，`PlayerPrefs` 存储在用户主目录的 `.config/unity3d/[公司名]/[产品名]/prefs` 目录中。

### iOS：
在iOS上，`PlayerPrefs` 存储在应用的 `Library/Preferences` 目录中，文件名是 `unity.[公司名].[产品名].plist`。

### macOS：
在macOS上，`PlayerPrefs` 存储在用户主目录的 `Library/Preferences` 目录中，文件名是 `unity.[公司名].[产品名].plist`。