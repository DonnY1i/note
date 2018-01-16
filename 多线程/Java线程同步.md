# 线程同步
## 锁对象
用ReentrantLock保护代码块的基本结构如下：
```Java
myLock.lock();//a ReentrantLock object
try{
    //critical section
}
finally{
    myLock.unlock();
}
```
这一结构确保任何时刻只有一个线程进入临界区。一个线程封锁了锁对象，其他线程都无法通过lock语句，它们被阻塞。

锁是可重入的，因为线程可以重复获得已经持有的锁。锁保持一个持有计数来跟踪对lock方法的嵌套调用。线程在每一次调用lock都要调用unlock来释放锁。由于这一特性，被一个锁保护的代码可以调用另一个使用相同锁的方法。
>公平锁 ReentrantLock(boolean fair)
## 条件对象
通常，线程进入了临界区，却发现在某一条件满足之后它才能运行。要使用一个条件对象来管理那些已经获得了一个锁但是却不能做有用工作的线程。

一个锁对象可以有一个或多个相关的条件对象。可以用newCondition()方法获得一个条件对象。
```Java
class Bank{
    private Lock bankLock = new ReentrantLock();
    private Condition sufficientFunds;
    ...
    public Bank(){
        ...
        sufficientFunds = bankLock.newCondition();
        ...
    }
}
```
当前线程发现条件不满足时，调用await()方法，当前线程被阻塞了，并==放弃了锁==。
> 等待锁的线程和调用await方法的线程存在本质的不同。一旦一个线程调用了await()方法，它进入该条件的等待集。当锁可用时，该线程不能马上解除阻塞。相反，它处于阻塞状态，直到另一个线程调用同一条件上的signalAll()方法时为止。

另一线程调用signalAll()重新激活因为这一条件等待的所有线程。当这些线程从等待集当中移出时，它们再次成为可运行的，调度器将再次激活它们。同时，它们将试图重新进入该对象。一旦锁成为可用的，它们中的某个将从await调用返回，获得该锁并从被阻塞的地方继续执行。

此时，线程应该再次测试条件。signalAll只是通知正在等待的线程可能已经满足条件。
```Java
while(!(ok to process))
    condition.await();
```
调用signalAll并不会立即激活一个等待线程。它仅仅接触等待线程的阻塞，以便这些线程可以在当前线程退出同步方法之后，通过竞争实现对对象的访问。
> 另一个方法signal()，随机解除等待集中的某个线程的阻塞状态。

#### 锁测试与锁超时
tryLock()方法视图申请一个锁，在成功获得锁后返回true，否则，立即返回false，而且线程可以立即离开去做其他事情，不会阻塞。
```Java
if(myLock.tryLock()){
    //now the thread own the lock
    try{...}
    finally{
        myLock.unlock();
    }
}
else
    //do something else
```
lock()方法不能被中断。如果一个线程等待获得一个锁时被中断，中断线程在获得锁之前一直处于阻塞状态。如果出现死锁，那么lock方法无法终止。

然而，如果调用带有用超时参数的tryLock，那么如果线程在等待期间被中断，将抛出InterruptedException异常。允许程序打破死锁。
```Java
if(myLock.tryLock(100, TimeUtil>MILLISECONDS))...
```
#### 读/写锁
java.util.concurrent.locks包定义了两个锁类，ReentrantLock类和ReentrantReadWriteLock类。很多读者线程很少写者线程时很有用。这种情况下，允许读者线程共享访问，写者线程互斥访问。
```Java
//1.构造一个ReentrantReadWriteLock对象
private ReetrantReadWriteLock rwl = new ReentrantReadWriteLock();
//2.抽取读锁和写锁
private Lock readLock = rwl.readLock();
private Lock writeLock = rwl.writeLock();
//3.对所有获取方法加读锁
public double getTotalBalance(){
    readLock.lock()
    try{...}
    finally{
        readLock.unlock();
    }
}
//4.对所有修改方法加写锁
public void transfer(...){
    writeLock.lock();
    try{...}
    finally{
        writeLock.unlock();
    }
}
```
## synchronized关键字
从1.0版开始，Java中的每个对象都有一个内部锁。如果一个方法用synchronized关键字声明，那么对象的锁将保护整个方法。要调用该方法，线程必须获得内部的对象锁。

```Java
public synchronized void method(){
    method body
}
```
等价于
```Java
public void mehthod(){
    this.intrisicLock.lock();
    try{
        method body
    }finally{
        this.intrinsicLock.unlock();
    }
}
```
内部对象锁只有一个相关条件。调用wait()或notifyAll()等价于
```Java
intrinsicCondition.await();
intrinsicCondition.signalAll();
```
```
public final void wait(long timeout)
                throws InterruptedException
Causes the current thread to wait until either another thread invokes the notify() method or the notifyAll() method for this object, or a specified amount of time has elapsed.
The current thread must own this object's monitor.

This method causes the current thread (call it T) to place itself in the wait set for this object and then to relinquish any and all synchronization claims on this object. Thread T becomes disabled for thread scheduling purposes and lies dormant until one of four things happens:

Some other thread invokes the notify method for this object and thread T happens to be arbitrarily chosen as the thread to be awakened.
Some other thread invokes the notifyAll method for this object.
Some other thread interrupts thread T.
The specified amount of real time has elapsed, more or less. If timeout is zero, however, then real time is not taken into consideration and the thread simply waits until notified.
The thread T is then removed from the wait set for this object and re-enabled for thread scheduling. It then competes in the usual manner with other threads for the right to synchronize on the object; once it has gained control of the object, all its synchronization claims on the object are restored to the status quo ante - that is, to the situation as of the time that the wait method was invoked. Thread T then returns from the invocation of the wait method. Thus, on return from the wait method, the synchronization state of the object and of thread T is exactly as it was when the wait method was invoked.
A thread can also wake up without being notified, interrupted, or timing out, a so-called spurious wakeup. While this will rarely occur in practice, applications must guard against it by testing for the condition that should have caused the thread to be awakened, and continuing to wait if the condition is not satisfied. In other words, waits should always occur in loops, like this one:

     synchronized (obj) {
         while (<condition does not hold>)
             obj.wait(timeout);
         ... // Perform action appropriate to condition
     }
```
> wait、notifyAll以及notify方法是Object类的final方法。

## 同步阻塞
每一个Java对象都有一个锁，还有另一种机制可以获得锁，通过进入一个同步阻塞。
```Java
synchronized(obj){//this is the syntax for a synchronized block
    critical section
}
```
于是它获得了obj的锁。
## Volatile域
volatile关键字为实例域的同步访问提供了一种免锁机制。如果声明一个域为volatile，那么编译器和虚拟机就知道该域是可能被另一个线程并发更新的。

比如一个boolean型变量done，它的值被一个线程设置却被另一个线程查询，可以使用锁：
```Java
private boolean done;
public synchronized boolean isDone(){return done;}
public synchronized boolean setDone(){done = true;}
```
内部锁不是个好主意，如果另一线程已经对该对象加锁，isDone和setDone方法可能阻塞。可以为这一变量使用独立的Lock。但也会带来很多麻烦。
这种情况下，将域声明为volatile是合理的：
```Java
private volatile boolean done;
public boolean isDone(){return done;}
public void setDone(){done = true;}
```
> volatile变量不提供原子性。

## 原子性
java.util.concurrent.atomic包中有很多类使用了很高效的机器级指令来保证其他操作的原子性。
```
public static AtomicLong nextNumber = new AtomicLong();

long id = nextNumber.incrementAndGet();
```
## 线程局部变量
ThreadLocal辅助类可以为各个线程提供各自实例。常用方法如下：
```Java
ThreadLocal():创建一个本地变量
get():返回此线程局部变量的当前线程副本中的值
initialValue():返回此线程局部变量的当前线程的初始值
set(T value):将此线程局部变量的当前线程副本中的值设置为value
```
```Java
public class Bank{
        private static ThreadLocal<Integer> account = new ThreadLocal<Integer>(){
            @Override
            protected Integer initialValue(){
                return 100;
            }
            
            protected void save(int money){
                account.set(account.get() + money);
            }
            
            public int getAccount(){
                return account.get();
            }
        };
}
```
## 阻塞队列
阻塞队列方法分为以下3类，这取决于当队列满或空时它们的处理方式。如果将队列当作线程管理工具来使用，将要用到put和take方法。当试图向满的队列添加或从空的队列移出元素时，add、remove和element操作抛出异常。offer、poll和peek如果不能完成任务，只是给出一个错误提示而不会抛出异常。

java.util.concurrent包提供了阻塞队列的几个变种。
- LinkedBlockingQueue
- LinkedBlockingDeque
- ArrayBlockingQueue
- PriorityBlockingQueue
- DelayQueue
- LinkedTransferQueue