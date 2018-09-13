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