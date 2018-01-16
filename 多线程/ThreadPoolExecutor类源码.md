```Java
private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
    private static final int COUNT_BITS = Integer.SIZE - 3;
    private static final int CAPACITY   = (1 << COUNT_BITS) - 1;

    // runState is stored in the high-order bits
    private static final int RUNNING    = -1 << COUNT_BITS;
    private static final int SHUTDOWN   =  0 << COUNT_BITS;
    private static final int STOP       =  1 << COUNT_BITS;
    private static final int TIDYING    =  2 << COUNT_BITS;
    private static final int TERMINATED =  3 << COUNT_BITS;

    // Packing and unpacking ctl
    private static int runStateOf(int c)     { return c & ~CAPACITY; }
    private static int workerCountOf(int c)  { return c & CAPACITY; }
    private static int ctlOf(int rs, int wc) { return rs | wc; }
```
AtomicInteger变量ctl低29位表示线程池中线程数，高3位表示线程池状态。
线程池状态：
1. RUNNING：*-1 << COUNT_BITS*，即高3位为111该状态的线程池会接收新任务，并处理阻塞队列中的任务。
2. SHUTDOWN：*0 << COUNT_BITS*，即高3位为000，该状态的线程池不会接收新任务，但会处理阻塞队列中的任务。
3. STOP：*1 << COUNT_BITS*，即高3位为001，该状态的线程不会接收新任务，也不会处理阻塞队列中的任务，而且会中断正在运行的任务。
4. TIDYING：*2 << COUNT_BITS*，即高3位为010。
5. TERMINATED：*3 << COUNT_BITS*，即高3位为011。
