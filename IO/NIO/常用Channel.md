### FileChannel
#### 打开
通过FileInputStream/FileOutputStream/RandomAccessFile来获取一个FileChannel实例
#### 非阻塞模式
#### 读写
```
ByteBuffer buf = ByteBuffer.allocate(48);
int bytesRead = inChannel.read(buf);
...
String newData = "new string to write to file ..." + System.currentTimeMillis();
ByteBuffer buf = ByteBuffer.allocate(48);
buf.clear();
buf.put(newData.getBytes());
buf.flip();
while(buf.hasRemaining())
    Channel.write(buf);
```
#### position
```
long pos = channel.position();
channel.position(pos + 123);
```
#### size/truncate/force
### SocketChannel
#### 创建
```
SocketChannel socketChannel = SocketChannel().open();
socketChannel.connect(new InetSocketAddress("http:locahost",80));
```
#### 非阻塞模式
```
socketChannel.configureBlocking(false);
...
while(!socketChannel.finishConnect())
    ...
```
或者结合Selector
### ServerSocketChannel
```
ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
serverSocketChannel.socket().bind(new InetSocketAddress(8888));

while(true){
    SocketChannel socketChannel = serverSocketChannel.accept();
    ...
}
```
### DatagramChannel
```
DatagramChannel channel = DatagramChannel.open();
channel.socket().bind(new InetSocketAddress(8888));

```
#### receive()/send()
#### connect()/read()/write()