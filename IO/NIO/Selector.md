Selector是Java NIO中能够检测到一到多个NIO通道，并能够知晓通道是否为诸如读写事件做好准备的组件。
```
Selector selector = Selector.open();
channel.configureBlocking(false);
//和Selector一起使用时，channel必须为非阻塞模式
channel.register(selector.SelectionKey.OP_READ);
```
- FileChannel不能和Selector一起使用，因为FileChannel不能切换为非阻塞模式。
- register()的第二个参数是一个interest集合，可以监听四种不同类型
    1. Connect
    2. Accept
    3. Read
    4. Write
- 可以用|（位或）操作符连接起来，
- register()方法会返回SelectionKey对象
### SelectionKey
- interest集合
    ```
    int interestSet = selectionKey.interestOps();
    ```
- ready
    ```
    int readySet = selectionKey.readyOps();
    ```
- Channel和Selector
    ```
    Channel channel = selectionKey.channel();
    Selector selector = selectionKey.selector();
    ```
- 附加对象
    ```
    selectionKey.attach(theObject);
    Object attachedObj = selectionKey.attachment();
    ```
- selectedKeySet()会返回注册到该Selector的所有通道对应的SelectionKey集合。
### Selector选择通道
```
int select();
//阻塞到至少一个通道就绪
int select(long timeout);
//最长阻塞timeout毫秒
int selectNow();
//不会阻塞，立即返回
```
select()方法返回的int值表示由多少个通道已经就绪。亦即，自己上次调用select()方法后有多少通道变成就绪状态，因为有一个通道就绪了，就返回1，若再次调用select()，另一个通道就绪，它会再次返回1。如果对第一个就虚的channel没有做任何操作，现在有两个就绪的通道，但在每次调用select()方法之间，只有一个通道就绪了。
### SelectedKeys()
调用select()后，返回值表面由一个或多个通道就绪，可以调用selector的selectedKeys()返回已选择键集中的就绪通道。
```
Set selectedKeys = selector.selectedKeys();
```
可以遍历这个键集来访问就绪的通道。每次迭代的末尾要keyIterator.remove()将已处理的键集移除。
### wakeup()
某个线程调用select()方法后阻塞，只要在其他线程在同一个Selector对象上调用wakeup()方法，阻塞的线程就会立马返回。
```
try {
			Selector selector = Selector.open();
			channel.configureBlocking(false);
			SelectionKey key = channel.register(selector,SelectionKey.OP_READ);
			while(true){
				int readyChannels = selector.select();
				if(readyChannels == 0)
					continue;
				Set selectedKeys = selector.selectedKeys();
				Iterator keyIterator = selectedKeys.iterator();
				while(keyIterator.hasNext()){
					SelectionKey key = keyIterator.next();
					if(key.isAcceptable()){
						
					}else if(key.isConnectable()){
						
					}else if(key.isReadable()){
						
					}else if(key.isWritable()){
						
					}
					keyIterator.remove();
				}
			}
		} catch (IOException e) {
			e.printStackTrace();
		}
```