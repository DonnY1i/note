# DOM(Document Object Model, DOM)

DOM是一种树形解析器，用它生成树结构将会消耗大量内存。

## 解析过程
1. 要读入一个XML文档，首先需要一个DocumentBuilder对象，可以从DocumentBuilderFactory中得到这个对象
    ```
    DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
    DocumentBuilder builder = factory.newDocumentBuilder();
    ```
2. 然后从文件中读入某个文档
    ```
    File f = ...
    Document doc = builder.parse(f);
    ```
    或者，用一个url
    ```
    URL u = ...
    Document doc = builder.parse(u);
    ```
    甚至可以指定一个任意的输入流
    ```
    InputStream in = ...
    Document doc = builder.parse(in);
    ```
    Document对象是XML文档的树型结构在内存中的表现，它由实现了Node接口及其他各种子接口的类的对象构成。
    
    ```
    graph LR
    B(Attr)-->A(Node)
    C(CharacterData)-->A(Node)
    L(Text)-->C
    M(Commet)-->C
    N(CDATASection)-->L
    D(Document)-->A
    E(DocumentType)-->A
    F(DocumentFragment)-->A
    G(Entity)-->A
    H(Element)-->A
    I(EntityReference)-->A
    J(Notation)-->A
    K(ProcessingInstruction)-->A
    ```
3. 可以通过调用getDocumentElement来启动对文档内容的分析，它将返回根元素。
    ```
    Element root = doc.getDocumentElement();
    ```
    getTagName()可以返回标签名。
4. 如果要得到该元素的子元素，使用getChildNodes()，返回一个NodeList集合，遍历方法如下
    ```
    NodeList children = root.getChildNodes();
    for(int i = 0; i < children.getLenth(); i++){
        Node child = children.item(i);
        if(child instanceof Element){
            Element childElement = (Element)child;
            Text textNode = (Text) childElement.getFirstChild();
            String text = textNode.getData().trim();
            if(childElement.getTagName().equals("name"))
                name = text;
            else if(childElement.getTagName().equals("size"))
                size = Integer.parseInt(text);
        }
    }
    ```
    换行会被当作空白符占据一个子元素。
    也可以使用getLastChild()方法得到最后一项子元素，用getNextSibling()得到下一个兄弟节点。
    
    要枚举节点属性，可以调用getAttributes()方法。它返回一个NamedNodeMap对象，其中包含了描述属性的Node对象。
    ```Java
    NamedNodeMap attributes = element.getAttributes();
    for(int i = 0; i < attributes.getLength(); i++){
        Node attribute = attributes.item(i);
        String name = attribute.getNodeName();
        String value = attribute.getNodeValue();
        ...
    }
    ```