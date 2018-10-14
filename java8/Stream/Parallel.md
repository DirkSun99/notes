## 并行流（Parallel Stream）

并行流就是把一个内容分成多个数据块，并用不同的线程分别处理每个数据块的流。

Java8 中将并行进行了优化，可以很容易的地数据进行并行操作。 Stream API 可以声明式地通过 parallel() 与 sequential() 在并行流与顺序流之间进行切换。

```java
/**
 * java8 并行流
 */
@Test
public void test3() {
    Instant start = Instant.now();
    LongStream.rangeClosed(0, 10000000000L)
            .parallel()
            .reduce(0, Long::sum);
    Instant end = Instant.now();
    System.out.println(Duration.between(start, end).toMillis());
}
```

多次调用 parallel 与 sequential ，以最后一次为准

```java
public static void debug() {
	System.out.println("debug " + i);
	try {
        TimeUnit.SECONDS.sleep(3);
    } catch (InterruptedException e) {
           
}

public static void debug2() {
	System.err.println("debug2 " + i);
        try {
            TimeUnit.SECONDS.sleep(3);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
}

/**
 * 目标：实现先并行，再串行效果（做不到）
 * 多次调用 parallel / sequential，以最后一次为准
 */
@Test
public void test1() {
	IntStream.range(1,100)
            // 调用parallel产生并行流
            .parallel().peek(TestStreamAPI4::debug)
            // 调用sequential产生串行流
            .sequential().peek(TestStreamAPI4::debug2)
            .count();
}
```

并行流默认使用的线程池为：`ForkJoinPool.commonPool`；默认的线程数是当前机器的cup个数；使用`parallelism`属性可以修改默认的线程数

```java
public static void debug3(int i) {
    System.out.println(Thread.currentThread().getName() + " debug " + i);
}

@Test
public void test() {
	// 设置默认线程数
	System.setProperty("java.util.concurrent.ForkJoinPool.common.parallelism", "20");
    IntStream.range(1, 100).parallel().peek(TestStreamAPI4::debug3).count();
}
```

使用自定义线程池，防止任务被阻塞

```java
@Test
public void test() {
	// 自定义线程池，名字为：ForkJoinPool-1
	ForkJoinPool pool = new ForkJoinPool(20);

    pool.submit(() -> IntStream.range(1,100).parallel().peek(TestStreamAPI4::debug3).count());
    pool.shutdown();

    // 防止主线程退出
    // 线程池以守护进程方式进行，如果主线程退出，守护进行也会退出
    synchronized (pool) {
        try {
            pool.wait();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```