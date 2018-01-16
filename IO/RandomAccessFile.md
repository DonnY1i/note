## 随机访问文件
```
RandomAccessFile in = new RandomAccessFile("employee.dat","r");
RandomAccessFile inOut = new RandomAccessFile("employee.dat","rw");
```
RandomAccessFile有一个文件指针，seek可以设置文件指针。
- getFilePointer
- seek
