#Unity #永久 

### Mathf
```csharp
// Math是C#封装的静态类, System命名空间
// Mathf是Unity封装的结构体, UnityEngine命名空间


// pi
float pI = Mathf.PI;
// 绝对值
int v = Mathf.Abs(-10);
// 向上取整
int v1 = Mathf.CeilToInt(1.2f);
// 向下取整
Mathf.FloorToInt(1.8f);
// 四舍五入
Mathf.RoundToInt(1.4f);
// 钳制函数     如果比最小值还小, 或比最大值还大, 就直接去最小或最大
Mathf.Clamp(10, 20, 100);   // 20
Mathf.Clamp(110, 20, 100);  // 110
Mathf.Clamp(1.5f, 1f, 2f);  // 1.5f
// 最大和最小
int max = Mathf.Max(1, 3, 5, 7, 9);
float min = Mathf.Min(2.0f, 4.0f, 6.0f, 8.0f);
// 指数
float v2 = Mathf.Pow(2, 3);
// 平方根
double v3 = Mathf.Sqrt(16f);
// 判断一个数是否是2^n      return (x != 0) && x & (x - 1) == 0;
int x = 16; bool v4 = Mathf.IsPowerOfTwo(x);
// 判断正负数    正返回1  负-1;
float v5 = Mathf.Sign(-0.1f);


// 插值运算
// Lerp :   result = Mathf.Lerp(start, end, t);
// t为插值系数   取值范围[0, 1]
// result = start + (end - start) * t;
// 用法1  先快后慢, 无限趋近10
float start = 1; float time = 0, result = 0;
start = Mathf.Lerp(start, 10, Time.deltaTime);
// 用法2  匀速变化
time += Time.deltaTime;
result = Mathf.Lerp(start, 10, time);
```


### 三角
```csharp
// 弧度转角度    * 180 / pi
float rad = 1;
float degress = rad * 180 / Mathf.PI;
degress = rad * Mathf.Rad2Deg;
Debug.Log(degress);
// 角度转弧度
rad = degress * Mathf.PI / 180;
rad = degress * Mathf.Deg2Rad;
Debug.Log(rad);

Mathf.Sin(弧度值);
```


### 向量
向量在数学和编程中有许多常用的成员和方法，常见的包括：
1. **成员**：
    - `x`、`y`、`z`：向量的分量，表示向量在 x、y、z 方向上的值。
    - `magnitude` 或 `length`：向量的**长度或模长**，表示从原点到向量的终点的距离。
    - `normalized`：返回与当前向量方向相同但长度为1的**单位向量**。
    - `sqrMagnitude`：向量**长度的平方**，用于比较向量长度时避免开方计算。
    - `zero`：零向量，所有分量均为0的向量。
2. **方法**：
    - `Vector3.Dot(Vector3 a, Vector3 b)`：计算两个向量的**点乘**结果。返回弧度
    - `Vector3.Cross(Vector3 a, Vector3 b)`：计算两个向量的**叉乘**结果。
    - `Vector3.Distance(Vector3 a, Vector3 b)`：计算两个向量之间的**距离**。
    - `Vector3.Lerp(Vector3 a, Vector3 b, float t)`：在两个向量之间进行**线性插值**。
    - `Vector3.Slerp(Vector3 a, Vector3 b, float t)`：在两个向量之间进行**球形插值**
    - `Vector3.Normalize(Vector3 value)`：将向量转化为单位向量。
    - `Vector3.Project(Vector3 vector, Vector3 onNormal)`：将一个向量**投影**到另一个向量上。
    - `Vector3.RotateTowards(Vector3 current, Vector3 target, float maxRadiansDelta, float maxMagnitudeDelta)`：将一个向量从当前方向**旋转**到目标方向。
    - `Vector3.Angle(Vector3 from, Vector3 to)`：返回值float, 计算两个向量之间的**角度值**

例子eg: 
- 利用 $A \cdot B = |A| * |B| cos\theta$ 或者 Vector.Angle()(角度) 来计算$\theta$ 
- $A\cdot B >= 0$ 前方 $A \times B < 0$ 则A右B左
- 线性插值 (匀速)
```csharp
public Transform target;

private Vector3 startPos;
private Vector3 tmpTarget;
private float time = 0;
void Update()
{
	// 每次终点移动变化, 重置时间, 起点
	if (tmpTarget != target.position)
	{
		tmpTarget = target.position;
		time = 0;
		startPos = transform.position;
	}

	time += Time.deltaTime;
	transform.position = Vector3.Lerp(startPos, target.position, time);
}
```