## Callable
```Java
public interface Callable<V> {
    V call() throws Exception;
}
```
一般配合ExecutorService来使用。
```Java
<T> Future<T> submit(Callable<T> task);
Future<?> submit(Runnable task);
```
## Future
Future就是对于具体的Runnable或者Callable任务的执行结果进行取消、查询是否完成、获取结果。
```Java
public interface Future<V> {
    boolean cancel(boolean mayInterruptIfRunning);
    boolean isCancelled();
    boolean isDone();
    V get() throws InterruptedException, ExecutionException;
    V get(long timeout, TimeUnit unit) throws InterruptedException, ExecutionException, TimeoutException;
}
```
- cancel 取消任务。
- isCancelled 是否取消成功
- isDone 是否已经完成
- get() 获取执行结果
- get(long, TimeUnit) 指定时间内获取执行结果
## FutureTask
```Java
public class FutureTask<V> implements RunnableFuture<V>
...
public interface RunnableFuture<V> extends Runnable, Future<V> {
    void run();
}

```
## Runnable和Callable区别
1. Callable是可以有返回值的，具体的返回值就是Callable的接口方法call返回的，由实现Future接口对象的get方法获取。Runnable中的run方法是没有返回值的。
2. Callable中的call方法是可以抛出异常的。但Runnable中的run方法是不可以抛出异常的，异常必须在run方法内部得到处理。