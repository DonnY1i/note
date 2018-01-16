## Channel
- Channel有点像流，数组从Channel读到Buffer中，也可以从Buffer写到Channel中。
- 即可以从Channel中读取数据，又可以写数据到通道。
- Channel可以异步地读写。
- Channel中的数据总是要读到一个Buffer，或者从一个Buffer中写入。
### FileChannel
```
RandomAcccessFile aFile = new RandomAccessFile("nio-data.dat","rw");
FileChannel inChannel = aFile.getChannel();

ByteBuffer buf = ByteBuffer.allocate(48);

int bytesRead = inChannel.read(buf);
while(bytesRead != -1){
    System.out.println("Read " + bytesRead);
    buf.flip();
    
    while(buf.hasRemaining())
        System.out.println((char) buf.get());
    
    buf.clear();
    bytesRead = inChannel.read(buf);
}
aFile.close();
```
### scatter/gather
scattering Reads将从Channel中读取出的数据写入多个Buffer。

gathering Writes将多个Buffer中的数据聚集后发送到Channel。
```
ByteBuffer header = ByteBuffer
```
### 通道之间的数据传输
```
fromChannel.transferTo(position,count,toChannel);
toChannel.transferFrom(fromChannel,position,count);
```

