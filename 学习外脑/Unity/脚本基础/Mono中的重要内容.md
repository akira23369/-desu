#Unity #永久 

![[Pasted image 20240321183442.png]]
### 重要成员
```csharp
//1.获取依附的GameObject
print(this.gameObject.name);

//2.获取依附的GameObject的位置信息
//得到对象位置信息
print(this.transform.position);//位置
print(this.transform.eulerAngles);//角度
print(this.transform.lossyScale);//缩放大小
//这种写法和上面是一样的效果 都是得到依附的对象的位置信息
this.gameObject.transform

//3.设置脚本是否激活
this.enabled = true;

print($"这是其它脚本对象 otherTest, 可以使用本脚本的其它脚本对象来获取gameObject  {otherTest.gameObject.name}");

```

### 重要方法
```csharp
// 获取依附对象上挂载的其它脚本    三个重载获取
// 通过挂载脚本名
Component component = this.GetComponent("Test2");
Test2 t = component as Test2;
print(t);

// 通过type
t = this.GetComponent(typeof(Test2)) as Test2;

// 通过           *************用最多*************
t = this.GetComponent<Test2>();



// 获取多个脚本
Test2[] test2s = this.GetComponents<Test2>();

List<Test2> list = new List<Test2>();
this.GetComponents<Test2>(list);



// 找自己及其子对象的所挂载的脚本   这里可以不传参数, 默认传false表示不找失活状态的脚本
// 找自己及其父对象的同理
this.GetComponentsInChildren<Test2>(false);
```

^155e67



### 注意
**要想操作其它GameObject对象, 必须获取其它的脚本对象, 而且得用Inspector来赋值**
