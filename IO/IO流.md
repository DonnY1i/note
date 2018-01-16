## 流的分类
字节流和字符流

抽象基类 | 字节流 | 字符流
-|-|-
输入流| InputStream | Reader
输出流| OutputStream | Writer
字节流：以byte为单位传输
字符流：以char为单位传输

IO流体系

分类 | 字节输入流 | 字节输出流 | 字符输入流 | 字符输出流
-|-|-|-|-
抽象基类 | InputStream | OutputStream | Reader | Writer
访问文件 | FileInputStream | FileOutputStream | FileReader | FileWriter
访问数组 | ByteArrayInputStream | ByteArrayOutputStream | CharArrayReader | CharArrayWriter
访问管道 | PipedInputStream | PipedOutputStream | PipedReader | PipedWriter
访问字符串 | | | StringReader | StringWriter
缓冲流 | BufferedInputStream | BufferedOutputStream | BufferedReader | BufferedWriter
转换流 | | | InputStreamReader | OutputStreamWriter
对象流 | ObjectInputStream | ObjectOutputStream | | | 
| | FilterInputStream | FilterOutputStream | FilterReader | FilterWriter
打印流 | | PrintStream | | PrintWriter
推回输入流 | PushbackInputStream | | PushbackReader | |
特殊流 | DataInputStream | DataOutputStream | | |
## 转换流

InputStreamReader

将字节流中读取到的字节按指定字符集编码成字符。需要与InputStream套接。

```
Reader isr = new InputStreamReader(System.in,"IOS8859-1");
```

OutputStreamWriter

将要写入得到字节流的字符按指定字符集编码成字节。需要和OutputStream套接。

## 标准输入输出流
System.in的类型是InputStream

System.out的类型是PrintStream，是OutputStream的子类FilterOutputStream的子类

