# HTML与XML的区别
- XML大小写敏感。
- XML结束标签不能省略。
- XML单个标签没有对应结束标签的元素必须以/结尾。
- XML属性值必须用引号括起来。
- HTML属性可以没有属性值，XML中所有属性必须都有属性值。
# XML文档结构

### XML文档头
```
<?xml version="1.0"?>
<?xml version="1.0" encoding="UTF-8"?>
```
### 文档类型定义(Document Type Definition, DTD)
```
<!DOCTYPE web-app PUBLIC
    "-//Sun Microsystems, Inc.//DTD Web Application 2.2//EN"
    "http://java.sun.com.j2ee/dtds/web-app_2.2.dtd">
```
### XML正文包括根元素和其他元素

元素可以有子元素、文本或者两者都有。

XML元素可以包含属性

### 其他标记

- 字符引用
    
    &#十进制值或者&#x十六进制值
- 尸实体引用

    \&lt;\&gt;\&amp;\&quot;\&apos;
    
    &lt;&gt;&amp;&quot;&apos;
- CDATA部分
```
<![CDATA[& >] are my favorite delimiters]>
```
- 处理指令
- 注释
```
<!-- this is a comment -->
```