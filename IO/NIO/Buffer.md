最常用的是ByteBuffer和CharBuffer。每个缓冲区都有：
- 一个容量，它永远不变。
- 一个读写位置，下一个值在此读写。
- 一个界限，超过它的读写是没有意义的。
- 一个可选标记，可用于重复一个读入或写出操作。
缓冲区本质上是一块可以写入的数据，然后可以从中读取数据的内存。
### Buffer的基本用法
1. 写入数据到Buffer
2. 调用flip()方法
3. 从Buffer中读取数据
4. 调用clear()方法或者compact()方法

clear()方法会清空整个缓冲区，compact()方法只会清空已经读过的数据。任何未读的数据都被移到缓冲区的起始处，新写入的数据将放到缓冲区未读数据的后面。

#### Buffer的capacity,position,limit
- capacity

    作为一个内存块，Buffer有一个固定的大小值，你只能往里写capacity个byte,long,char等类型。
- position

    写数据到Buffer时，position表示当前位置。初始为0，最大为capacity - 1。当讲Buffer从写模式切换到读模式，position会被重置。
- limit

    写模式下limit等于capacity；当切换到读模式时，limit表示你最多能读到多少数据。limit会设置为写模式下最终position的值。
    
#### Buffer的分配

    allocate();
#### 向Buffer中写数据
    ```
    int bytesRead = inChannel.read(buf);
    ...
    buf.put(arg);
    ```
#### flip()

    从写模式切换到读模式。position置为0，limit设置为之前position的值。
#### 从Buffer中读
    ```
    int bytesWritten = inChannel.write(buf);
    ...
    buf.get();
    ```
#### rewind()
    将position设回0.
#### clear(),compact()
    clear()被调用，position被设回0，limit设为capacity的值。
    
    compact()被调用，将未读的数据拷贝到Buffer起始处。position设到最后一个未读元素的正后面，limit属性设置成capacity。Buufer准备好写数据，但不会覆盖未读的数据
#### mark()和reset()
    调用Buffer.mark()标记Buffer中一个特定position。之后Buffer.reset()恢复到整个position。
#### equals()和compareTo()
    满足：
    1. 类型相同
    2. Buffer中剩余个数相同
    3. Buffer中剩余值相同
    时，两个Buffer相等。
    
    第一个不相等的元素小于另一个Buffer中对应的元素时，一个Buffer小于另一个。