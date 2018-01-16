### join
```
public final void join(long millis)
                throws InterruptedException
Waits at most millis milliseconds for this thread to die. A timeout of 0 means to wait forever.
This implementation uses a loop of this.wait calls conditioned on this.isAlive. As a thread terminates the this.notifyAll method is invoked. It is recommended that applications not use wait, notify, or notifyAll on Thread instances.
```
调用线程将会等待this线程的执行结束或者millis毫秒后继续执行。millis为0时，会持续等待。靠被调用线程this的同步锁和wait、notifyAll方法实现。
### yeild
```
public static void yield()
A hint to the scheduler that the current thread is willing to yield its current use of a processor. The scheduler is free to ignore this hint.
Yield is a heuristic attempt to improve relative progression between threads that would otherwise over-utilise a CPU. Its use should be combined with detailed profiling and benchmarking to ensure that it actually has the desired effect.

It is rarely appropriate to use this method. It may be useful for debugging or testing purposes, where it may help to reproduce bugs due to race conditions. It may also be useful when designing concurrency control constructs such as the ones in the java.util.concurrent.locks package.
```
放弃占用CPU，让出cpu给其他线程。
### sleep
```
public static void sleep(long millis)
                  throws InterruptedException
Causes the currently executing thread to sleep (temporarily cease execution) for the specified number of milliseconds, subject to the precision and accuracy of system timers and schedulers. The thread does not lose ownership of any monitors.
```
休眠millis毫秒，但不会让出同步锁。
### 总结
sleep方法不会让出同步锁，wait()或者await()方法会让出同步锁。