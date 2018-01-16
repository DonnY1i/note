XPath描述了XML文档中的节点集
```
/gridbag/row
```
可以用[]来选择特定原色
```
/gridbag/row[1]
```
使用@操作符可以得到属性值
```
/gridbag/row[1]/@anchor
```
XPath有很多有用的函数
```
count(/gridbag/row)
//返回gridbag根元素的row子元素的数量
```
### Java计算XPath的值
1. 从XPathFactory创建一个XPath对象
```
XPathFactory xpfactory = XPathFactory.newInstance();
path = xpfactory.newXPath();
```
2. 然后调用evaluate来计算XPath表达式
```
String username = path.evaluate("/configuration/database/username",doc);
```
3. 如果XPath表达式产生了一组节点
```
NodeList nodes = (NodeList) path.evaluate("/gridbag/row",doc,XPathConstants.NODESET);
```
4. 如果结果只有一个节点
```
Node node = (Node) path.evaluate("/gridbag/row[1]",doc,XPathConstants.NODE);
```
5. 如果结果是一个数字
```
int count = ((Number) path.evaluate("count(/gridbag/row)",doc,XPathConstants.NUMBER)).intValue();
```
6. 可以从任意一个节点或节点列表开始
```
result = path.evaluate(expression,node);
```