## 添加尾缀说明
我们知道Java在变量赋值的时候，其中float、double、long数据类型变量，需要在赋值直接量后面分别添加f或F、d或D、l或L尾缀来说明。  
**其中，long类型最好以大写L来添加尾缀，因为小写l容易和数字1混淆。**
```
long lNum = 1234L;
float fNum = 1.23f;
double dNum = 1.23d;
```
这是Java语法规定，不添加尾缀很容易引起编译器报错，并且程序可读性也会变差。
## 不添加尾缀也不会报错的情况
Java语言中，整数直接量（例如：1、2、10等），JVM虚拟机是默认为int类型数据的。所以，当整数直接量赋给long、float或者double，而不添加尾缀，虚拟机也会直接将int类型数据自动转换为对应类型然后赋值。因为数据长度短的转换为长的并不会造成数据丢失，所以默认可以自动转换。
```
long  lNum  = 5;   //不报错，因为int自动转换为long类型，不会报错
float fNum  = 7;   //不报错，因为int自动转换为float类型，不会报错
double dNum = 10;  //同上
```
但是，当浮点直接量（例如：1.2等），JVM虚拟机默认为double类型，如果直接赋值给float就会引起编译器报错。
```
float fNum  = 1.2; //报错，因为1.2虚拟机是默认为double类型，不能直接赋值给float类型变量
float fNew  = 1.3f;//正确，因为尾缀添加了f，即告诉了虚拟机1.3属于float类型变量
```
## 总结
所以，当Java中遇到这三种类型变量需要赋直接量时候，最好都添加上相应的尾缀。这样不仅会防止编译器报错，也会增加程序的可读性。  
　　但是下面这种情况就算添加尾缀也是错的，因为尾缀仅是为了告诉虚拟机该直接数属于什么数据类型，而不能实现数据类型强制转换。
```
long lNum = 1.2L;      //错误，double类型数据不能直接赋值给long类型
long lNew = (long)1.2; //正确，double类型数据强制转换为long类型
```

---
> CSDN 无鞋童鞋