#未整理 


![[Pasted image 20240419170023.png]]

# **每次Deserialize(reader), reader就会往后移动一步跳过一整个元素!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!**


```csharp
public class SerializDictionary<TKey, TValue> : Dictionary<TKey, TValue>, IXmlSerializable
{
    public XmlSchema GetSchema() => null;

    //自定义字典的 反序列化 规则
    public void ReadXml(XmlReader reader)
    {
        XmlSerializer keySer = new XmlSerializer(typeof(TKey));
        XmlSerializer valueSer = new XmlSerializer(typeof(TValue));
        
        //要跳过根节点
        Debug.Log("根节点" + reader.Name);
        reader.Read();
        Debug.Log(reader.Name);
        //判断 当前不是元素节点 结束 就进行 反序列化
        while (reader.NodeType != XmlNodeType.EndElement)
        {
            Debug.Log("1111 " + reader.Name);
            //反序列化键
            TKey key = (TKey)keySer.Deserialize(reader);
            Debug.Log("2222 " + reader.Name);
            //反序列化值
            TValue value = (TValue)valueSer.Deserialize(reader);
            Debug.Log("3333 " + reader.Name);
            //存储到字典中
            this.Add(key, value);
        }
        // 记得Read不然后面的读取可能会出问题
        reader.Read();
    }
    
    //自定义 字典的 序列化 规则
    public void WriteXml(XmlWriter writer)
    {
        XmlSerializer keySer = new XmlSerializer(typeof(TKey));
        XmlSerializer valueSer = new XmlSerializer(typeof(TValue));
        
        foreach (KeyValuePair<TKey, TValue> kv in this)
        {
            //键值对 的序列化
            keySer.Serialize(writer, kv.Key);
            valueSer.Serialize(writer, kv.Value);
        }
    }
}
```


