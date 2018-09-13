## Fork/Join 框架

Fork/Join 框架就是在必要的情况下，将一个大任务，进行拆分(fork)成若干个小任务(拆到不可再拆时)，再将一个个的小任务运算的结果进行 join 汇总。


* Fork/Join 框架与传统线程池的区别

采用“工作窃取”模式(work-stealing)：当执行新的任务时，它可以将其拆分成更小的任务执行，并将小任务加到线程队列中，然后再从一个随机线程的队列中偷一个并把它放在自己的队列中。

相对于一般的线程池实现，Fork/Join 框架的优势体现在对其中包含的任务的处理方式上。在一般的线程池中，如果一个线程正在执行的任务由于某些原因无法继续运行，那么该线程会处于等待状态。而在 Fork/Join 框架实现中，如果某个子问题由于等待另外一个子问题的完成而无法继续运行，那么处理该子任务的线程会主动寻找其他尚未运行的子问题来执行。这种方式减少了线程的等待时间，提高了性能。

```java
/**
 * Fork/Join 框架
 */
public class ForkJoinCalculate extends RecursiveTask<Long> {
    private long start;
    private long end;

    public ForkJoinCalculate(long start, long end) {
        this.start = start;
        this.end = end;
    }

    private static final long THRESHOLD = 10000;

    @Override
    protected Long compute() {
        long length = end - start;

        if (length <= THRESHOLD) {
            long sum = 0;
            for (long i = start; i <= end; i++) {
                sum += i;
            }
            return sum;
        } else {
            long middle = (start+end)/2;
            ForkJoinCalculate left = new ForkJoinCalculate(start, middle);
            left.fork(); // 拆分子任务，同时压入线程队列

            ForkJoinCalculate right = new ForkJoinCalculate(middle+1, end);
            right.fork();

            return left.join() + right.join();
        }
    }
}

/**
 * Fork-Join框架测试
 */
@Test
public void test1() {
    Instant start = Instant.now();
    ForkJoinPool pool = new ForkJoinPool();
    ForkJoinTask<Long> task = new ForkJoinCalculate(0,10000000000L);
    Long sum = pool.invoke(task);
    System.out.println(sum);

    Instant end = Instant.now();

    System.out.println(Duration.between(start,end).toMillis());
}

/**
 * 普通For循环
 */
@Test
public void test2() {
    Instant start = Instant.now();

    long sum = 0L;

    for (long i=0; i<=10000000000L; i++) {
        sum += i;
    }

    System.out.println(sum);

    Instant end = Instant.now();

    System.out.println(Duration.between(start,end).toMillis());
}
```