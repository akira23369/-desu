#Unity #永久 


![[LineRenderer组件参数.xmind]]
![[Pasted image 20240413130624.png]]


```csharp
GameObject line = new GameObject();
line.name = "myLine";
LineRenderer myLine = line.AddComponent<LineRenderer>();

// 是否首尾相连
myLine.loop = true;
// 开始结束宽
myLine.startWidth = 0.02f;
myLine.endWidth = 0.02f;
// 开始结束颜色
myLine.startColor = Color.black;
myLine.endColor = Color.white;
// 设置材质
myLine.material = Resources.Load<Material>("Material地址");

// 设置点
// 先设置数量, 在设置每个点的位置
myLine.positionCount = 4;
myLine.SetPositions(new Vector3[]
{
    new Vector3(0, 0, 0),
    new Vector3(5, 0, 0),
    new Vector3(5, 0, 5),
    //new Vector3(0, 0, 5)          // 没有设置最后一个点
});
myLine.SetPosition(3, new Vector3(0, 0, 5));

// 是否使用世界坐标
myLine.useWorldSpace = false;

//是否接受光照进行着色计算
myLine.generateLightingData = true;
```