#Unity #未整理 

![[Pasted image 20240414162955.png]]
![[Pasted image 20240414162952.png]]



可以使用Button脚本中的OnClick来添加监听事件的函数
也可以在自己脚本中申明使用UIButton


![[Pasted image 20240414164648.png]]

```csharp
public UIButton bt;
void Start()
{
    bt.onClick.Add(new EventDelegate(() =>
    {
        print("监听的函数");
    }));
}
```


eg:
Tank.cs实例化子弹, bullet.cs管理子弹自动前进, Panel.cs添加按钮点击事件