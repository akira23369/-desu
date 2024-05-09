#未整理 

序列化: **将类对象**   ===>  **可以用来存储, 网络传输的文件格式(如xml, json)**
反序列化: **可以用来存储, 网络传输的文件格式(如xml, json)** ===> **类对象**

![[Pasted image 20240418203715.png]]

![[Pasted image 20240418204848.png]]

![[Pasted image 20240418205104.png]]


```csharp
string path = Application.persistentDataPath + "/test.xml";
Test test = new Test();
using (StreamWriter wirte = new StreamWriter(path))
{
    XmlSerializer xmlSerializer = new XmlSerializer(typeof(Test));
    xmlSerializer.Serialize(wirte, test);
}
```

XmlDocument 与 序列化? 
序列化的方式, 适合动态的存储和读取
XmlDocument主要适用于读取纯粹的xml文件, 里面没有类与之映射


注意:
不能直接序列化Dictionary
如果引用成员为null,那么在xml中看不到该字段


特性
```csharp
// 表明下面t3是一个名为wocao666的属性
[XmlAttribute("wocao666")]
public bool t3 = true;
```


![[Pasted image 20240418213523.png]]


### 反序列化
```csharp
Test t = new Test();
string path = Application.persistentDataPath + "/test.xml";
if (File.Exists(path))
{
    using (StreamReader reader = new StreamReader(path))
    {
        XmlSerializer serializer = new XmlSerializer(typeof(Test));
        t = serializer.Deserialize(reader) as Test;
    }
}
```


![[Pasted image 20240418215129.png]]