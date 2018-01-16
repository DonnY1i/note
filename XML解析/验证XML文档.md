
# 文档类型定义(DTD)与XML Scheme定义。
### DTD
一条DTD规则如下
```
<!ELEMENT font (name,size)>
```
提供DTD的方式有多种。可以直接纳入XML文档中
```
<?xml version="1.0"?>
<!DOCTYPE configuration [
    <!ELEMENT configuration ...>
    more rules
    ...
]>
<configuration>
    ...
</configuration>
```
SYSTEM声明可以把DTD存储在XML文档外。
```
<!DOCTYPE configuration SYSTEM "config.dtd">
```
### 配置解析器利用DTD
1. 通知DocumentBuilderFactory打开验证特性
    ```
    factory.setValidating(true);
    ```
    这样，该对象的所有DocumentBuilder都会根据DTD来验证它们的输入。
    ```
    factory.setIgnoringElementContentWhitespace(true);
    ```
    这样DocumentBuilder就不会报告文本节点中的空白字符。
2. 当解析器报告错误时，应用程序希望对该错误执行操作。因此，在验证时，应该安装一个错误处理器。
    ```
    builder.setErrorHandler(handler);
    ```
### XML Schema
要在XML文档中引用Schema文件，需要在根元素中添加属性。
```
<?xml version="1.0"?>
<configuration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:noNamespaceSchemaLocation="config.xsd">
    ...
</configuration>
```
Shema使用名字空间定义了每个元素的类型。可以是简单类型或是复杂类型。
### 解析带有Schema的XML文件
与解析带有DTD的文件相似，但有3点差别：
1. 必须打开对命名空间的支持，即使在XML文件里你不使用它。
```
factory.setNamespaceAware(true);
```
2. 必须通过一下“魔咒”来准备好处理Schema的工厂
```
final String JAXP_SCHEMA_LANGUAGE = "http://java.sun.com/xml/jaxp/properties/schemaLanguage";
final String W3C_XML_SCHEMA = "http://www.w3.org/2001/XMLSchema";
factory.setAttribute(JAXP_SCHEMA_LANGUAGE,W3C_XML_SCHEMA);
```
3. 解析器不会丢弃元素中的空白字符。