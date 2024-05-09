#Unity #永久 

## 输入Input
### 鼠标位置
```csharp
// 鼠标位置
// 返回一个Vector3, 但只有x, y有值
print(Input.mousePosition);
```

### 检测鼠标输入
```csharp
// 检测鼠标输入   (0左键, 1右键, 2滚轮
if (Input.GetMouseButtonDown(0))
    print("你按了一次 鼠标左键");
if (Input.GetMouseButtonUp(1))
    print("你抬起了一次 鼠标右键");
if (Input.GetMouseButton(0))
    print("你是一直在按着 鼠标左键");
// 返回一个Vector2, 而滚动会改变其中的y值
// y值   -1 往下棍, 0 没滚, 1 往上滚
Vector2 mouseScrollDelta = Input.mouseScrollDelta;
print(mouseScrollDelta);
```

### 检测键盘输入
```csharp
// 键盘按下 up抬起, 无长按...
if (Input.GetKeyDown(KeyCode.W))
    print("你按下了W键");
```

### 检测默认轴
```csharp
// 默认轴输入
// 当键盘A,D键按下时,  返回-1 到 1之间的变换   
// 即一直按A的话, 返回值会慢慢从0变到 -1
// Input.GetAxisRaw是只会有 -1, 0, 1三个数字之间的突变
float v = Input.GetAxis("Horizontal");
print(v);

// W, S 返回-1, 到 1
float v1 = Input.GetAxis("Vertical");
print(v1);

// 鼠标横向移动   纵向  Mouse Y
float v2 = Input.GetAxis("Mouse X");
print(v2);
```

### 其它
```csharp
// 是否有任意键或鼠标长按
bool anyKey = Input.anyKey;
if (Input.anyKeyDown)
{
    print($"有一个键按下了按下的是{Input.inputString}");
}

// 得到连接的手柄的所有按钮名字
string[] strings = Input.GetJoystickNames();
for (int i = 0; i < strings.Length; i++)
{
    print("你连接的所有按钮有");
    print(strings[i]);
}
// 某一手柄键按下, 抬起, 长按...
bool v3 = Input.GetButtonDown(strings[0]);

// 移动设备触摸
if (Input.touchCount > 0)
{
    Touch touch = Input.touches[0];
    // 位置
    print(touch.position);
    // 相对上次位置的变化
    print(touch.deltaPosition);
}

// 是否启用多点触控
Input.multiTouchEnabled = true;


// 陀螺仪  (重力感应)
// 是否开启陀螺仪
Input.gyro.enabled = true;

// 重力加速度 
Vector3 gravity = Input.gyro.gravity;

// 旋转速度
Vector3 rotationRate = Input.gyro.rotationRate;

// 陀螺仪 当前旋转的四元数
// 比如用这个角度信息 来控制场景上的一个3D物体收到重力影响
// 手机怎么动, 他就怎么动
Quaternion attitude = Input.gyro.attitude;
```




## 屏幕Screen
### 常用
```csharp
// 当前屏幕分辨率  (设备的分辨率
Resolution currentResolution = Screen.currentResolution;
print($"当前分辨率宽:{currentResolution.width}, 高:{currentResolution.height}");

// 当前游戏窗口分辨率 
print($"当前窗口分辨率为:{Screen.width} * {Screen.height}");


// 屏幕休眠模式   
Screen.sleepTimeout = SleepTimeout.NeverSleep;

```


### 不常用
```csharp

// 运行时是否全屏
Screen.fullScreen = true;

// 窗口模式     (以后在发布的时候在设置, 一般不用代码去设置)
// 独占全屏 FullScreenMode.ExclusiveFullScreen
// 全屏窗口 FullScreenMode.FullScreenWindow;
// 最大化窗口 FullScreenMode.MaximizedWindow
// 窗口模式 FullScreenMode.Windowed;
Screen.fullScreenMode = FullScreenMode.FullScreenWindow;


// 移动设备转向相关...
Screen.autorotateToLandscapeLeft = true;

// 指定屏幕显示方向
Screen.orientation = ScreenOrientation.LandscapeLeft;


// 设置分辨率 第三个参数是否为全屏    (移动设备不用, 
Screen.SetResolution(1920, 1080, false);
```

