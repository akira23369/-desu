#Unity #永久 #数学

## 欧拉角
![[Pasted image 20240402180529.png]]
### 欧拉角优缺点
**优点**
- 直观、易理解
- 存储空间小(三个数表示)
- 可以进行从一个方向到另一个方向旋转大于180度的角度
**缺点**:
- 同一旋转的表示不唯    450 % 360 == 90 % 360
- 万向节死锁


## 四元数
注意: transform.rotation = **四元数**
![[Pasted image 20240402201129.png]]
```csharp
Quaternion a = new Quaternion(Mathf.Sin(30 * Mathf.Deg2Rad), 0, 0, Mathf.Cos(30 * Mathf.Deg2Rad));
transform.rotation = a;

Quaternion b = Quaternion.AngleAxis(60, transform.right);
transform.rotation = b;
```

### 四元数和欧拉角相互转换
```csharp
// 欧拉角转四元数
Quaternion c = Quaternion.Euler(60, 0, 0);
// 四元数转欧拉角
Vector3 eulerAngles = c.eulerAngles;
```

### 四元数相乘代表旋转
```csharp
// 每帧绕着y轴旋转1°
void Update()
{
	transform.rotation *= Quaternion.AngleAxis(1, transform.up);
}


void Start()
{
    // 定义一个向量
    Vector3 vector = new Vector3(1, 0, 0);

    // 定义一个四元数表示45度绕Y轴的旋转
    Quaternion rotation = Quaternion.Euler(0, 45, 0);

    // 使用四元数旋转向量
    Vector3 rotatedVector = rotation * vector;

    // 打印结果
    Debug.Log("Original Vector: " + vector);
    Debug.Log("Rotated Vector: " + rotatedVector);
}

```

### 四元数常用
```csharp
 // 单位四元数 [1, (0, 0, 0)]
 Quaternion i = Quaternion.identity;

// 插值运算     Lerp 和 Slerp差别不大, Slerp效果要好点
transform.rotation = Quaternion.Slerp(transform.rotation, target.rotation, Time.deltaTime);


// Quaternion.LookRotation(Vector3 forward, Vector3 upwards = Vector3.up);
// forward 参数表示要面向的方向向量。
// upwards 参数表示指定旋转中的上方向向量，默认为世界空间中的 Y 轴单位向量 Vector3.up。
// 返回值：返回一个四元数，表示将物体旋转到面向 forward 方向的旋转。
transform.rotation = Quaternion.LookRotation(target.transform.position - transform.position);
```

