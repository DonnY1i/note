名字空间由URI标识
```
http://www.w3c.org/2001/XMLSchema
```
```
<element xmlns="namespaceURI1">
    <child xmlns="namespaceURI2">
        grandchildren
    </child>
</element>
```
可以用一个前缀来表示命名空间，即为特定文档选取一个短的标识符
```
<xsd:schema xmlns:xsd="http://www.w3c.org/2001/XMLSchema">
    <xsd:element name="gridbag" type="GridBagType" />
    ...
</xsd>
```
### 控制解析器对命名空间的处理
1. 打开命名空间处理特性：
```
factory.setNamespaceAware(true);
```
2. 这样，DocumentBuilderFactory产生的所有DocumentBuilder都支持命名空间了。每个节点有三个属性：
    - 带前缀的限定名，由getNodeName和getTagName方法返回
    - 命名空间URI，由getNamespaceURI返回
    - 不带前缀和命名空间的本地名，由getLocalName返回