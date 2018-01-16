# 创建线程
1. 实现Runable接口
    1. 实现Runable接口的run()方法
    ```Java
    public interface Runnable {
        void run();
    }
    ```
    ```Java
    Runable r = () -> { task code};
    ```
    2. 由Runable创建一个Thread对象
    ```Java
    Thread t = new Thread(r);
    ```
    3. 启动
    ```Java
    t.start();
    ```

---
2. 也可以通过构建一个Thread类的子类来定义一个线程，如下
```Java
class MyThread extends Thread {
    public void run(){
        task code
    }
}
```
然后构造一个子类对象，并调用start()方法。这种方法不再推荐，应该将要并行的任务与运行机制解耦。
# 相关方法
1. 暂停线程

    Thread.sleep()是一个静态方法，用于暂停当前线程，sleep方法**可以**抛出一个InterruptedException异常，不会让出同步锁。
    ```Java
    static void sleep(long millis)
    ```
2. 中断线程
    
    没有可以强制线程终止的方法，interrupt可以用来请求线程中断。interrupt调用时，线程中断状态位被置位。
    ```Java
    Runnable r = () -> {
        try{
            while(!Thread.currentThread().isInterrupted() && more work){
                //do more work
            }
        }catch(InterruptedException e){
            //Thread was imterrupted during sleep or wait
        }finally{
            //cleanup,if required
        }
    };
    ```
    如果线程被阻塞，就无法检测中断状态，这是产生InterruptedException的地方，当在一个被阻塞的线程(sleep()或者wait())上调用interrupt方法时，==阻塞调用==(interrupt不会抛出异常)将会被InterruptedException异常中断。
    
    如果在每次工作迭代之后都调用sleep方法，isInterrupted检测既没有必要也没有用处。如果在中断状态被置位时调用sleep方法，它不会休眠。相反，它将清除这一状态，并抛出InterruptedException。因此，如果你的循环调用sleep，不会检测中断状态。要用如下方式捕获InterruptedException异常：
    ```Java
    Runnable r = () -> {
        try{
            ...
            while(more work to do){
                //do more work
                Thread.sleep(DELAY);
            }
        }catch(InterruptedException e){
            //thread is interrupted during sleep
        }finally{
            //cleanup,if required
        }
        //exiting the run method terminates the thread
    };
    ```
    ### Interrupted和isInterrupted
    Interrupted()是一个静态方法，它检测当前的线程是否被中断。而且，调用Interrupted()方法会清除该线程的中断状态。isInterrupted()是一个实例方法，可用来检测是否有线程被中断。调用这个方法不会改变中断状态。
# Java线程状态
1. New
    ```Java
    new Thread(r);
    ```
    new操作符创建一个新的线程，还未运行。
2. Runnable

    一旦调用start()方法，线程就处于Runnable状态。一个可运行的线程可能运行也可能没运行，取决于操作系统给线程的运行时间。
3. Blocked

    当一个线程试图获取一个内部的对象锁(不是java.util.concurrent锁)，而该锁被其他线程所持有，则该线程进入阻塞状态。当其他线程释放该锁，而且线程调度器允许本线程持有它时，线程变为非阻塞状态。
4. Waiting

    线程等待另一个线程通知调度器一个条件时，它自己进入等待状态。在调用Object.wait()、Thread.join()或是等待java.util.concurrent中的Lock或Condition时，就会出现这种情况。
5. Timed waiting

    有几个方法有一个超时参数，调用它们导致线程进入计时等待状态。一直保持到超时期满或者接收到适当通知。Thread.sleep()、Object.wait()、Thread.join()、Lock.tryLock()以及Condition.await()的计时版。
6. Terminated

    线程终止有如下几个原因：
    - 因为run方法正常退出而自然死亡
    - 因为一个没有捕获的异常终止了run方法而意外死亡

```
graph LR
A((New))-->|start|B((Runnable))
B-->|请求锁|D((Blocked))
D-->|得到锁|B
B-->|等待通知|E((Waiting))
E-->|出现通知|B
B-->|等待超时或通知|F((Timed waiting))
F-->|出现超时或通知|B
B-->|exits|C((Terminated))
```
# 线程属性
### 优先级
一个线程继承它父线程的优先级，可以用setPriority方法提高或降低任何一个线程的优先级。MIN_PRIORITY(1)与MAX_PRIORITY(10)之间，NORM_PRIORITY定义为5。进程调度器会优先选择较高优先级的线程。线程的优先级高度依赖于系统。
### 守护线程
通过调用
```Java
t.setDaemon(true);
```
将线程转化为守护线程。为其他线程提供服务。在线程启动前调用。
### 未捕获异常受理器
非受查异常被传递到一个用于未捕获异常的处理器。该处理器必须实现Thread.UncaughtExceptionHandler接口的类。这个接口只有一个方法
```Java
void uncaughtException(Thread t, Throwable e)
```
可以用setUncaughtExceptionHandler为任何一个线程安装处理器。可以用Thread的静态方法setDefaultUncaughtExceptionHandler为所有线程安装一个默认处理器。不为独立线程安装处理器，此时处理器就是该线程的ThreadGroup对象。
> 线程组是一个可以统一管理的线程集合。默认情况下，创建的所有线程属于相同线程组，但是，也可能会建立其他的组。

ThreadGroup的uncaughtException方法做如下操作：

1. 如果该线程组有父线程组，那么父线程组的uncaughtException方法被调用。
2. 否则，如果Thread.getDefaultExceptionHandler方法返回非空处理器，则调用该处理器。
3. 否则，如果Throwable是ThreadDeath的一个实例，什么都不做。
4. 否则，线程的名字以及Throwable的栈轨迹被输出到System.err上。