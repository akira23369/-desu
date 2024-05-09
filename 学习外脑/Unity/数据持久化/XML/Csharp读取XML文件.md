#编程语言/Csharp #未整理 



![[Pasted image 20240417211139.png]]


使用
不能使用SelectSingleNode来跨层调用
```csharp
// 获取PlayerInfo下的name节点,  并使用name节点的值
XmlNode root = xmlDmt.SelectSingleNode("PlayerInfo");
XmlNode nameNode = root.SelectSingleNode("name");
string innerText = nameNode.InnerText;
```


```csharp
// 获取Item节点的属性
XmlNode ItemNode = root.SelectSingleNode("Item");
string idValue = ItemNode.Attributes["id"].Value;
print(idValue);
// 或者 idValue = ItemNode.Attributes.GetNamedItem("id").Value;
```

### 获取所有同名节点
```csharp
// 获取一个节点一层下所有同名节点
XmlNodeList nodeList = root.SelectNodes("Item");
foreach (XmlNode it in nodeList)
{
    XmlNode str = it.SelectSingleNode("string");
    print(str.InnerText);
    XmlNode intnode = it.SelectSingleNode("int");
    print(intnode.InnerText);
}
// for循环
for (int i = 0; i < nodeList.Count; i++)
{
    XmlNode tmp = nodeList[i];
}
```


![[IXmlSerializable接口#^561b92]]


![[Pasted image 20240417232711.png]]


