Pipe是2个线程之间的单向数据连接。有一个sourceChannel通道和一个sinkChaneel。数据会被写到sinkChaneel，从sourceChannel读取。
1. 打开
    ```
    Pipe pipe = Pipe.open();
    ```
2. 读写
    ```
    Pipe.SinkChannel sinkChannel = pipe.sink();
    ...
    while(buf.hasRemaining()){
        sinkChannel.write(buf);
    }
    
    Pipe.SourceChannel sourceChannel = pipe.source();
    ...
    ByteBuffer buf = ByteBuffer.allocate(48);
    int bytesRead = sourceChannel.read(buf);
    ```