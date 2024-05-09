#编程语言/Csharp 


![[Pasted image 20240418230649.png]]

![[Pasted image 20240419163739.png]]

```csharp
public void WriteXml(XmlWriter writer)
{
    writer.WriteStartElement("Person");

    writer.WriteElementString("Name", Name);
    writer.WriteElementString("Age", Age.ToString());

    // Writing child nodes
    writer.WriteStartElement("Address");
    writer.WriteElementString("Street", "123 Main St");
    writer.WriteElementString("City", "Anytown");
    writer.WriteElementString("ZipCode", "12345");
    writer.WriteEndElement(); // End of Address

    writer.WriteEndElement(); // End of Person
}
```

^edf399

```xml
<Person>
    <Name>John Doe</Name>
    <Age>30</Age>
    <Address>
        <Street>123 Main St</Street>
        <City>Anytown</City>
        <ZipCode>12345</ZipCode>
    </Address>
</Person>

```

^2e65e7

`reader.ReadStartElement("Person")` 后，`XmlReader` 将会移动到 `<Person>` 元素的开始位置。
```csharp
public void ReadXml(XmlReader reader)
{
    reader.ReadStartElement(); // Move to the <Person> node

    Name = reader.ReadElementString("Name");
    Age = Convert.ToInt32(reader.ReadElementString("Age"));

    reader.ReadStartElement("Address"); // Move to the <Address> node
    Street = reader.ReadElementString("Street");
    City = reader.ReadElementString("City");
    ZipCode = reader.ReadElementString("ZipCode");
    reader.ReadEndElement(); // Move out of the <Address> node

    reader.ReadEndElement(); // Move out of the <Person> node
}
```

^561b92

**移动到下一个节点**：使用 `Read` 方法可以移动到 XML 结构中的下一个节点。
**reader一开始指向根元素**
**检查节点类型**：使用 `NodeType` 属性可以检查当前节点的类型（元素、文本、属性等）
使用 `.Read` 方法来移动到 `<Person>` 元素的开始标记：
```csharp
public void ReadXml(XmlReader reader)
{
    while (reader.Read())
    {
        if (reader.NodeType == XmlNodeType.Element && reader.Name == "Person")
        {
            // Move to the <Person> element
            break;
        }
    }
    
    if (reader.NodeType == XmlNodeType.Element && reader.Name == "Person")
    {
        // Read data from the <Person> element
        Name = reader.ReadElementString("Name");
        Age = Convert.ToInt32(reader.ReadElementString("Age"));
        
        // Read data from the <Address> element
        reader.ReadStartElement("Address"); // Move to the <Address> node
        Street = reader.ReadElementString("Street");
        City = reader.ReadElementString("City");
        ZipCode = reader.ReadElementString("ZipCode");
        reader.ReadEndElement(); // Move out of the <Address> node
    }
}
```




1. **XmlNodeType.Element**：表示元素节点，即 XML 文件中的开始标签和结束标签之间的内容或子元素。例如：`<ElementName>Content</ElementName>` 中的 `<ElementName>` 和 `</ElementName>` 就是元素节点。
2. **XmlNodeType.Attribute**：表示属性节点，即元素节点中的属性。例如：`<ElementName AttributeName="AttributeValue">Content</ElementName>` 中的 `AttributeName="AttributeValue"` 就是属性节点。
3. **XmlNodeType.Text**：表示文本节点，即元素节点中的文本内容。例如：`<ElementName>Content</ElementName>` 中的 `Content` 就是文本节点。
4. **XmlNodeType.CDATA**：表示 CDATA 节点，即 XML 文件中的 CDATA 区域。CDATA 区域可以包含文本数据，不会被 XML 解析器解析，常用于存储特殊字符或大量文本数据。
5. **XmlNodeType.Comment**：表示注释节点，即 XML 文件中的注释内容。例如：`<!-- This is a comment -->` 中的 `This is a comment` 就是注释节点。
6. **XmlNodeType.Document**：表示整个 XML 文档的节点，包括文档声明、根元素等。
7. **XmlNodeType.XmlDeclaration**：表示 XML 文档声明节点，即 XML 文件中的 `<?xml version="1.0" encoding="utf-8"?>` 部分。
8. **XmlNodeType.EndElement**：表示结束元素节点，即 XML 文件中的结束标签。