#Unity #永久 

***作用:用于进行Unity内置编辑器, 调试工具, 编辑工具...    不用游戏UI***
缺点: 
- 无分辨率自适应
- 运行时查看结果

1. 他们都是GUI公共类中提供的静态函数 直接调用即可
2. 他们的参数都大同小异
  - 位置参数：Rect参数 x y位置 w h尺寸
  - 显示文本：string参数
  - 图片信息：Texture参数
  - 综合信息：GUIContent参数
  - 自定义样式：GUIStyle参数
3. 每一种控件都有多种重载，都是各个参数的排列组合

ps: 必备的参数内容 是 **位置信息**和**显示信息**

### 基本控件
```csharp
// 文本控件  
GUI.Label(rect, tex, style);

// 按钮控件  (在按钮范围内 按下鼠标再抬起鼠标 才算一次点击 才会返回true
GUI.Button(btnRect, btnContent, btnStyle);

// 长按按钮控件   (只要在长按按钮范围内 按下鼠标 就会一直返回true
GUI.RepeatButton(btnRect, btnContent);

// 多选框    GUI.Toggle的默认点击范围是用的Rect中的宽高, 想光改变样式大小去改fixed widith
GUI.Toggle(togRect, flag, "选项1", style);

// 单选框    (一般通过一个int来控制所有的flag, 可通过点击来改变是否选中)
if (GUI.Toggle(rect, selIndex == 1, "选项1"))
{
	selIndex = 1;
}
if (GUI.Toggle(rect, selIndex == 2, "选项2"))
{
	selIndex = 2;
}

// 拖动条    [0, 1] 之间拖动musicSize
musicSize = GUI.HorizontalSlider(musicSliderPos, musicSize, 0, 1);
songSize = GUI.HorizontalSlider(songSliderPos, songSize, 0, 1);


// 图片
- scaleMode (ScaleMode)：指定纹理的缩放模式，可以是以下几种值之一：
    - ScaleMode.StretchToFill：拉伸纹理以填充整个矩形区域。
    - ScaleMode.ScaleToFit：按照纹理的原始宽高比例缩放纹理，使其完整地显示在矩形区域内。
    - ScaleMode.ScaleAndCrop：按照纹理的原始宽高比例缩放纹理，然后裁剪超出矩形区域的部分。
- alphaBlend (bool)：指定是否使用alpha混合（即透明度混合），默认为true。
- imageAspect (float)：指定纹理的宽高比例，用于在ScaleMode.ScaleToFit模式下调整纹理的显示。

GUI.DrawTexture(texPos, tex, scaleMode, alphaBlend, imageAspect);
```


### 复合控件
#### 工具栏和选择网格
```csharp
// 
toolbarIndex = GUI.Toolbar(toolbarPos, toolbarIndex, toolbarStrings);
// 相对toolbar多一个参数xCount,  代表水平方向最多显示的数量
toolbarIndex = GUI.SelectionGrid(selGridPos, toolbarIndex, toolbarStrings, 2);
switch (toolbarIndex) ...      不同的工具栏选项处理不同逻辑 
```

#### 分组
用于批量控制控件位置 
可以理解为 包裹着的控件加了一个父对象 
可以通过控制分组来控制包裹控件的位置
```csharp
GUI.BeginGroup(groupPos);    // 必须搭配EndGroup来使用
GUI.Button(new Rect(0, 0, 100, 50), "测试按钮");
GUI.Label(new Rect(0, 60, 100, 20), "Label信息");
GUI.EndGroup();
```

#### 滚动视图
在Unity中，GUI.BeginScrollView函数用于创建一个可以滚动的视图区域，允许在较小的矩形区域内显示较大的内容。该函数的参数如下所示：

- **position (Rect)**：指定  **滚动视图**  的矩形区域，使用Rect类型来表示，包含了左上角的位置坐标和宽度、高度。
- **scrollPosition (Vector2)**：指定当前滚动位置的二维向量，表示在滚动视图中内容的偏移量。
- **viewRect (Rect)**：指定  **总内容**  的矩形区域大小，使用Rect类型来表示，包含了左上角的位置坐标和宽度、高度。
```csharp
// 想让滚动视图的真实内容大小 随内容(这里是strings)大小变化而变化
// 在绘制滚动视图之前把变化的数据计算好
showView.height = strs.Length * 30;
nowPos = GUI.BeginScrollView(scrollPos, nowPos, showView);
for (int i = 0; i < strs.Length; i++)
{ 
    GUI.Label(new Rect(0, i * 30, 100, 30), strs[i]);
}
GUI.EndScrollView();
```

#### 窗口
```csharp
// 第一个参数是窗口唯一id
// 第三个参数是一个 要一个int作为参数的无返回值的委托, 用来处理窗口内容的
GUI.Window(1, new Rect(0, 0, 500, 500), (a) =>
{
    // 一个函数处理不同的窗口
    switch (a)
    {
        case 1:
            GUI.Button(new Rect(0, 0, 100, 30), "第一个窗口逻辑");
            break;
        case 2:
            break;
    }
}, "测试窗口");

// 只有把模态窗口处理后, 才可以点击其它窗口
GUI.ModalWindow(2, new Rect(500, 500, 100, 30), (a) =>
{
}, "模态窗口测试");

// 拖动窗口
winRect = GUI.Window(3, winRect, (a) =>
{
    // 无参默认窗口所有位置都可以拖动
    GUI.DragWindow();
}, "拖动窗口测试");
```


#### 全局颜色 整体皮肤样式(GUI.skin) 自动布局GUILayout
可以改全局颜色(会和其它做特殊运算) 
GUILayout:主要用于进行编辑器开发 如果用它来做游戏UI不太合适

```csharp
// 全局着色
GUI.color = Color.red;

// 文本着色
GUI.contentColor = Color.yellow;

// 背景着色
GUI.backgroundColor = Color.red;

// 可以在Unity右键创建GUISkin对象
public GUISkin skin;
GUI.skin = skin;

// 自动布局    默认竖着来对齐
GUILayout.BeginVertical();

GUILayout.Button("123", GUILayout.Width(200));
GUILayout.Button("245666656565");
GUILayout.Button("235", GUILayout.ExpandWidth(false));

GUILayout.EndVertical();

GUILayoutOption 布局选项
        //控件的固定宽高
        GUILayout.Width(300);
        GUILayout.Height(200);
        //允许控件的最小宽高
        GUILayout.MinWidth(50);
        GUILayout.MinHeight(50);
        //允许控件的最大宽高
        GUILayout.MaxWidth(100);
        GUILayout.MaxHeight(100);
        //允许或禁止水平拓展
        GUILayout.ExpandWidth(true);//允许
        GUILayout.ExpandHeight(false);//禁止
        GUILayout.ExpandHeight(true);//允许
        GUILayout.ExpandHeight(false);//禁止
```









