## SAX解析器
SAX解析器在解析XML输入的组成部分时会报告事件，但不会以任何方式存储文档，而是由事件处理器建立相应的数据结构。

在使用SAX解析器时，需要一个处理器来为不同解析器事件定义事件动作。ContentHandler接口定义了若干个在解析文档时解析器会调用的回调方法。

- startElement和endElement
- characters
- startDocument和endDocument
处理器必须覆盖这些方法，让它们执行在解析文件时想要执行的动作。
1. 得到SAX解析器
```
SAXParserFactory factory = SAXParserFactory.newInstance();
SAXParser parser = factory.newSAXParser();
```
2. 处理文档
```
parser.parse(source,handler);
```
handler属于DefaultHandler的一个子类，它为一下四个接口定义了空的方法：
- ContentHandler
- DTDHandler
- EntityResolver
- ErrorHandler

## StAX解析器
StAX是一种拉解析器，与安装事件处理器不同，只需使用以下这样的基本循环来迭代所有事件
```
InputStream in = url.openStream();
XMLInputFactory factory = XMLInputFactory.newInstance();
XMLStreamReader parser = factory.createXMLStreamReader(in);
while(parser.hasNext()){
    int event = parser.next();
    //call parser methods to obtain event details
}
```