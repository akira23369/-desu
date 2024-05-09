#Unity #永久 



![[需求分析图1.png]]

![[需求分析图2.png]]


## Base(最核心)
### CustomGUIPos.cs
使用通过Inspector来控制位置
计算公式:  **屏幕九宫格 + 控件中心九宫格 + 偏移**
```csharp
public enum E_Alignment_Type
{
    Up,
    Down,
    Left,
    Right,
    Center,
    Left_Up,
    Left_Down,
    Right_Up,
    Right_Down,
}
// 自定义类需要再Inspector面板显示
[System.Serializable]
public class CustomGUIPos
{
    private Rect _pos = new Rect(0, 0, 101, 100);
    // 屏幕九宫格对齐方式
    public E_Alignment_Type Screen_Alignment_Type = E_Alignment_Type.Center;
    // 控件中心点对齐方式
    public E_Alignment_Type Constrol_Center_Alignment_Type = E_Alignment_Type.Center;
    // 偏移位置
    public Vector2 dPos;
    // 宽高 
    public float Width = 100;
    public float Height = 50;
    public Rect Pos
    {
        get
        {
            CalcCenterPos();
            CalcPos();
            _pos.width = Width;
            _pos.height = Height;
            return _pos;
        }
    }
    // 控件中心点位置
    private Vector2 _centerPos;
    private void CalcCenterPos()
    {
        switch (Constrol_Center_Alignment_Type)
        {
            case E_Alignment_Type.Up:
                _centerPos.x = -Width / 2;
                _centerPos.y = 0;
                break;
            case E_Alignment_Type.Down:
                _centerPos.x = -Width / 2;
                _centerPos.y = -Height;
                break;
            case E_Alignment_Type.Left:
                _centerPos.x = 0;
                _centerPos.y = -Height / 2;
                break;
            case E_Alignment_Type.Right:
                _centerPos.x = -Width;
                _centerPos.y = -Height / 2;
                break;
            case E_Alignment_Type.Center:
                _centerPos.x = -Width / 2;
                _centerPos.y = -Height / 2;
                break;
            case E_Alignment_Type.Left_Up:
                _centerPos.x = 0;
                _centerPos.y = 0;
                break;
            case E_Alignment_Type.Left_Down:
                _centerPos.x = 0;
                _centerPos.y = -Height;
                break;
            case E_Alignment_Type.Right_Up:
                _centerPos.x = -Width;
                _centerPos.y = 0;
                break;
            case E_Alignment_Type.Right_Down:
                _centerPos.x = -Width;
                _centerPos.y = -Height;
                break;
        }
    }
    // 计算最后的位置
    private void CalcPos()
    {
        switch (Screen_Alignment_Type)
        {
            case E_Alignment_Type.Up:
                _pos.x = Screen.width / 2 + _centerPos.x + dPos.x;
                _pos.y = Screen.height * 0 + _centerPos.y + dPos.y;
                break;
            case E_Alignment_Type.Down:
                _pos.x = Screen.width / 2 + _centerPos.x + dPos.x;
                _pos.y = Screen.height + _centerPos.y + dPos.y;
                break;
            case E_Alignment_Type.Left:
                _pos.x = Screen.width * 0 + _centerPos.x + dPos.x;
                _pos.y = Screen.height / 2 + _centerPos.y + dPos.y;
                break;
            case E_Alignment_Type.Right:
                _pos.x = Screen.width + _centerPos.x + dPos.x;
                _pos.y = Screen.height / 2 + _centerPos.y + dPos.y;
                break;
            case E_Alignment_Type.Center:
                _pos.x = Screen.width / 2 + _centerPos.x + dPos.x;
                _pos.y = Screen.height / 2 + _centerPos.y + dPos.y;
                break;
            case E_Alignment_Type.Left_Up:
                _pos.x = Screen.width * 0 + _centerPos.x + dPos.x;
                _pos.y = Screen.height * 0 + _centerPos.y + dPos.y;
                break;
            case E_Alignment_Type.Left_Down:
                _pos.x = Screen.width * 0 + _centerPos.x + dPos.x;
                _pos.y = Screen.height + _centerPos.y + dPos.y;
                break;
            case E_Alignment_Type.Right_Up:
                _pos.x = Screen.width + _centerPos.x + dPos.x;
                _pos.y = Screen.height * 0 + _centerPos.y + dPos.y;
                break;
            case E_Alignment_Type.Right_Down:
                _pos.x = Screen.width + _centerPos.x + dPos.x;
                _pos.y = Screen.height + _centerPos.y + dPos.y;
                break;
        }

    }
}
```

### CustomGUIControl.cs
**这是所有控件的公共基类**
```csharp
public enum E_Style_onoff
{
    On,
    Off,
}
public abstract class CustomGUIControl : MonoBehaviour
{
    // GUI控件公共部分    !!!!!!
    // 位置信息
    public CustomGUIPos guiPos;
    // 显示内容信息
    public GUIContent guiContent;
    // 自定义样式
    public GUIStyle style;
    // 自定义样式开关
    public E_Style_onoff styleIsOn = E_Style_onoff.Off;

    // 提供公共的控件绘制
    public void GUIDraw()
    {
        switch (styleIsOn)
        {
            case E_Style_onoff.On:
                // 样式开启时使用的绘制函数
                DrawIsOn();
                break;
            case E_Style_onoff.Off:
                DrawIsOff();
                break;
        }
    }
    protected abstract void DrawIsOn();
    protected abstract void DrawIsOff();
}
```

### CustomGUIRoot.cs
该类是用来管理所有GUI的, 而且所见所得
```csharp
[ExecuteAlways]
public class CustomGUIRoot : MonoBehaviour
{
    private CustomGUIControl[] _customGUIControls;
    private void Start()
    {
        _customGUIControls = this.GetComponentsInChildren<CustomGUIControl>();
    }

    // 统一控制所有子对象挂载的控件的 绘制
    private void OnGUI()
    {
        if (!Application.isPlaying)    // 这里的条件判断主要是为了性能, 实际运行可能会出错
        {
            _customGUIControls = this.GetComponentsInChildren<CustomGUIControl>();
        }
        for (int i = 0; i < _customGUIControls.Length; i++)
        {
            _customGUIControls[i].GUIDraw();
        }
    }
}
```




## 基础控件预制体的制作
### CustomGUIButton.cs
```csharp
public class CustomGUIButton : CustomGUIControl
{
    // 外部提供给的按钮点击事件, Unity自带的事件
    public event UnityAction clickEvent;
    protected override void DrawIsOff()
    {
        if (GUI.Button(guiPos.Pos, guiContent))
        {
            clickEvent?.Invoke();
        }
    }

    protected override void DrawIsOn()
    {
        if (GUI.Button(guiPos.Pos, guiContent, style))
        {
            clickEvent?.Invoke();
        }
    }
}

```

### CustomGUILabel.cs
```csharp
public class CustomGUILabel : CustomGUIControl
{
    protected override void DrawIsOff()
    {
        GUI.Label(guiPos.Pos, guiContent);
    }
    protected override void DrawIsOn()
    {
        GUI.Label(guiPos.Pos, guiContent, style);
    }
}
```

### CustomGUIInput.cs
```csharp
public class CustomGUIInput : CustomGUIControl
{
    public event UnityAction<string> changeText;
    private string oldStr = "";
    protected override void DrawIsOff()
    {
        guiContent.text = GUI.TextField(guiPos.Pos, guiContent.text);
        // 当输入框变化时调用的委托函数
        if (oldStr != guiContent.text)
        {
            changeText?.Invoke(guiContent.text);
            oldStr = guiContent.text;
        }
    }
    protected override void DrawIsOn()
    {
        guiContent.text = GUI.TextField(guiPos.Pos, guiContent.text, style);
        if (oldStr != guiContent.text)
        {
            changeText?.Invoke(guiContent.text);
            oldStr = guiContent.text;
        }
    }
}
```

### CustomGUITexture.cs
```csharp
public class CustomGUITexture : CustomGUIControl
{
    public ScaleMode mode = ScaleMode.StretchToFill;
    protected override void DrawIsOff()
    {
        GUI.DrawTexture(guiPos.Pos, guiContent.image, mode);
    }
    protected override void DrawIsOn()
    {
        GUI.DrawTexture(guiPos.Pos, guiContent.image, mode);
    }
}
```


### CustomGUISlide.cs
```csharp
public enum E_Slider_Type
{
    Horizontal,
    Vertical,
}
public class CustomGUISlide : CustomGUIControl
{
    public float minValue = 0;
    public float maxValue = 1;
    public float nowValue = 0;

    public E_Slider_Type sliderType = E_Slider_Type.Horizontal;
    // 默认的style是条的, 这里的style是滑块的
    public GUIStyle styleThumb;

    public event UnityAction<float> changeValue;
    private float oldValu
    
    e;
    protected override void DrawIsOff()
    {
        switch (sliderType)
        {
            case E_Slider_Type.Horizontal:
                nowValue = GUI.HorizontalSlider(guiPos.Pos, nowValue, minValue, maxValue);
                break;
            case E_Slider_Type.Vertical:
                nowValue = GUI.VerticalSlider(guiPos.Pos, nowValue, minValue, maxValue);
                break;
        }
        if (oldValue != nowValue)
        {
            changeValue?.Invoke(nowValue);
            oldValue = nowValue;
        }
    }
    protected override void DrawIsOn()
    {
        switch (sliderType)
        {
            case E_Slider_Type.Horizontal:
                nowValue = GUI.HorizontalSlider(guiPos.Pos, nowValue, minValue, maxValue, style, styleThumb);
                break;
            case E_Slider_Type.Vertical:
                nowValue = GUI.VerticalSlider(guiPos.Pos, nowValue, minValue, maxValue, style, styleThumb);
                break;
        }
        if (oldValue != nowValue)
        {
            changeValue?.Invoke(nowValue);
            oldValue = nowValue;
        }
    }
}
```

### CustomGUIToggle.cs
```csharp
public class CustomGUIToggle : CustomGUIControl
{
    // 单选的那个bool
    public bool isSel;
    private bool oldSel;
    // 当选中时所要执行的事件
    public event UnityAction<bool> changeEvent;
    protected override void DrawIsOff()
    {
        isSel = GUI.Toggle(guiPos.Pos, isSel, guiContent);
        // 防止true true true一直调用事件执行 
        if (isSel != oldSel)
        {
            changeEvent?.Invoke(isSel);
            oldSel = isSel;
        }
    }
    protected override void DrawIsOn()
    {
        isSel = GUI.Toggle(guiPos.Pos, isSel, guiContent, style);
        if (isSel != oldSel)
        {
            changeEvent?.Invoke(isSel);
            oldSel = isSel;
        }
    }
}
```

### CustomGUIToggleGroup.cs
#### [[lambda表达式#^78a3d9 | 闭包]]可以使委托函数内也能享用到外部资源!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
^cf6658
```csharp
public class CustomGUIToggleGroup : MonoBehaviour
{
    [SerializeField]
    private CustomGUIToggle[] _toggles;
    // 注意只有游戏运行才能用
    void Start()
    {
        for (int i = 0; i < _toggles.Length; i++)
        {
            CustomGUIToggle tmp = _toggles[i];
            // 为每个Toggle添加一个一旦其值为true时, 其它Toggle变为false的委托
            // 闭包机制可以使委托函数内也能享用到外部资源!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
            tmp.changeEvent += (value) =>
            {
                if (value)
                {
                    for (int j = 0; j < _toggles.Length; j++)
                    {
                        if (tmp != _toggles[j])
                        {
                            _toggles[j].isSel = false;
                        }
                    }
                }
            };
        }
    }
}
```