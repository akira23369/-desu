#Unity #永久 
## 位置与位移

### Vector常用
```csharp
Vector3.forward     001
Vector3.up....      010

// 两点之间的距离
Vector3.Distance(v1, v2);
```

### 位置
```csharp
// 相对世界坐标系的值
print($"距离世界源点 {this.transform.position}");
// 相对父对象    没父对象就相对于源点
print($"距离父对象坐标点 {this.transform.localPosition}");

// 位置的赋值不能单独改x, y, z     只能整体来赋值
this.transform.position = new Vector(5, this.transform.position.y, ...);
// 或者
Vector3 tmp = this.transform.position;    tmp.y = 666;
this.transfomr.position = tmp;
```

### 朝向
本地坐标系
```csharp
print(this.transform.forward);          // 游戏对象当前的面朝向
print(this.transform.up);               // 对象当前的头顶朝向
Vector3 right = this.transform.right;   // 右手边
```

### 位移
```csharp
// 1 位移 = 方向 * 速度 * 时间    (手动写, 世界坐标系的话用Vector3.XX)
//this.transform.position = this.transform.forward * 1 * Time.deltaTime;


// 2 位移API
// 参数1 表示位移多少
// 参数2 移动所采用的坐标系, 即(1, 0, 0)是哪个坐标系的(1, 0, 0) (不填默认自己坐标系)

this.transform.Translate(Vector3.forward * 1 * Time.deltaTime, Space.World);
this.transform.Translate(transform.forward * 1 * Time.deltaTime, Space.World);
// 不会这样移动, 自己相对于世界坐标系的歪曲的数据 再应用于自己的坐标系
this.transform.Translate(transform.forward * 1 * Time.deltaTime, Space.Self);
```




## 角度与旋转

### 角度相关
![[Pasted image 20240402180211.png]]
```csharp
// 相对于世界坐标角度
print(this.transform.eulerAngles);
// 相对于父对象角度
// Inspector中显示的是相对父对象的角度
print(this.transform.localEulerAngles);

// 不能单个x, y, z赋值
this.transform.localEulerAngles = new Vector3(10, 10, 10);
```

### 旋转相关
```csharp
// 自转API    第二个参数默认不写绕着自己坐标转
this.transform.Rotate(new Vector3(0, 10, 0) * Time.deltaTime * 100, Space.World);

// 绕着某个点旋转              绕着源点,   y轴,     转的多少度
this.transform.RotateAround(Vector3.zero, Vector3.up, 10 * Time.deltaTime);
```



## 缩放和看向
### 缩放
```csharp
// 相对于世界坐标系     // 不能改
print(this.transform.lossyScale);

// 相对于父对象        // 可以改
print(this.transform.localScale);

// unity 没有提供缩放相关的API, 只能自己手动来搞    eg: 
this.transform.localScale += Vector3.one * Time.deltaTime;
```

### 看向
```csharp
// 将自己的面朝向源点
this.transform.LookAt(Vector3.zero);
```

## 父子关系
### 获取和设置父对象
```csharp
// 获取父对象
print(this.transform.parent);

// 断绝父子关系
this.transform.parent = null;

 // 认父亲      // 也会顺带把孙子的爷爷也换了
this.transform.parent = GameObject.Find("obj").transform;


// 下面这种认父亲的方法    特殊一些

// 第二个参数为true 则不会改掉物体本来的世界坐标位置,大小什么的
//          为false, 则会把原来Inspector面板上的数字原封不动的赋值到新的Inspector窗口上
this.transform.SetParent(GameObject.Find("obj").transform, false);
```

### 抛妻弃子
```csharp
// 和自己所有儿子断绝关系,  不会断掉儿子和孙子的
this.transform.DetachChildren();
```

### 获取子对象及其操作
```csharp
// 根据名字查找儿子对象       (不可以查找孙子, 可以找到失活的儿子)
print(this.transform.Find("Capsule"));
// 查找Father的儿子Son
print(this.transform.Find("Father/Son"));

// 儿子数量
print(this.transform.childCount);
// 遍历
for (int i = 0; i < this.transform.childCount; i++)
    print(this.transform.GetChild(i));
	
// 判断是不是某人的儿子
bool v = this.transform.IsChildOf(transform.parent);

// 设置自己作为儿子编号
this.transform.SetSiblingIndex(0);

// 得到自己作为儿子的编号
print(this.transform.GetSiblingIndex());
```




## 坐标转换
### 世界 -> 本地
```csharp
// 世界坐标 转换为 本地坐标    (会受到缩放影响
// 即 在世界坐标系下的   (0, 0, 1) 在本地坐标系下表示为   transform.InverseTransformPoint(Vector3.forward)
print(this.transform.InverseTransformPoint(Vector3.forward));

// 世界坐标系的向量 平移到 本地坐标系后的向量值是     (不受缩放影响
print(this.transform.InverseTransformDirection(Vector3.forward));
// 世界坐标系的向量 平移到 本地坐标系后的向量值是     (受缩放影响
print(this.transform.InverseTransformVector(Vector3.forward));
```

---
### 本地 -> 世界
```csharp
// 世界坐标 转换为 本地坐标    (会受到缩放影响
// 即 在世界坐标系下的   (0, 0, 1) 在本地坐标系下表示为   transform.InverseTransformPoint(Vector3.forward)
print(this.transform.TransformPoint(Vector3.forward));

// 世界坐标系的向量 平移到 本地坐标系后的向量值是     (不受缩放影响
print(this.transform.TransformDirection(Vector3.forward));
// 世界坐标系的向量 平移到 本地坐标系后的向量值是     (受缩放影响
print(this.transform.TransformVector(Vector3.forward));
```