# String中equals()和==
``` Java
String greeting = "hello";
if(greeting == "hello")
    //probably true;
if(greeting.substring(0, 3) == "hel")
    //probably false;
```
Java中只有相同的**字符串常量**是共享的，而+或substring等操作产生的结果并不是共享的。

---
# 情况1
```Java
String s1 = "xyz";
String s2 = "xyz";
System.out.println(s1 == s2)
->>true
```
这是因为：

字符串缓冲池：程序在运行的时候会创建一个字符串缓冲池。
当使用 String s1 = "xyz"; 这样的表达是创建字符串的时候（非new这种方式），程序首先会在这个 String 缓冲池中寻找相同值的对象，
在 String str1 = "xyz"; 中，s1 先被放到了池中，所以在 s2 被创建的时候，程序找到了具有相同值的 str1
并将 s2 引用 s1 所引用的对象 "xyz"
# 情况2
```Java
String s1 = new String("xyz");
String s2 = new String("xyz");
System.out.println(s1 == s2);
->>false
```
s1和s2指向两个对象