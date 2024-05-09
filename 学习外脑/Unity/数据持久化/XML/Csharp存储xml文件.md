#编程语言/Csharp #未整理 

![[特殊文件夹]]

![[Pasted image 20240418190317.png]]


```csharp
string path = Application.persistentDataPath + "/PlayerInfo2.xml";

XmlDocument xmlDoc = new XmlDocument();
// 添加头一行
XmlDeclaration xmlDeclaration = xmlDoc.CreateXmlDeclaration("1.0", "UTF-8", "");
xmlDoc.AppendChild(xmlDeclaration);
// 添加root根节点
XmlElement root = xmlDoc.CreateElement("Root");
xmlDoc.AppendChild(root);
// 给根节点添加子节点
//在XML中，节点（Node）是XML文档的基本构建块，可以包含各种类型的数据和信息。
//CreateNode 方法通常用于创建各种类型的节点，包括元素节点、属性节点等。    
//CreateElement 方法通常用于在文档中创建新的元素节点，并设置其属性或内容。
XmlNode name = xmlDoc.CreateElement("name");
name.InnerText = "黄先生";
root.AppendChild(name);
// 添加List
XmlElement intList = xmlDoc.CreateElement("intList");
root.AppendChild(intList);
for (int i = 0; i < 3; i++)
{
    XmlElement tmp = xmlDoc.CreateElement("int");
    // 添加属性
    tmp.SetAttribute("id", $"{i}");
    tmp.InnerText = (i + 1).ToString();
    intList.AppendChild(tmp);
}
print(path);
xmlDoc.Save(path);


// 如果这个路径的文件存在
if (File.Exists(path))
{
    XmlDocument xmlDoct = new XmlDocument();
    xmlDoct.Load(path);
    XmlNode nme = xmlDoct.SelectSingleNode("Root/name");
    XmlNode rootNode = xmlDoct.SelectSingleNode("Root");
    // 只能两代移除
    rootNode.RemoveChild(nme);
    xmlDoct.Save(path);
}
```


![[IXmlSerializable接口#^edf399]]
![[IXmlSerializable接口#^2e65e7]]





![[Pasted image 20240418195704.png]]

