## 运行机制

1. 所有操作是链式调用，一个元素只迭代一次
2. 每一个中间操作返回一个新的流。流里面有一个属性`sourceStage`，指向同一个地方，即`Head`
3. Head -> nextStage -> nextStage -> ... -> null
4. 有状态操作会把无状态操作截断，单独处理
5. 并行环境下，有状态的中间操作不一定能并行操作
6. parallel / sequential 这2个操作也是中间操作，但不创建流，只是修改 Head 的并行标志

```java
@Test
public void test() {
    Random random = new Random();
    // 随机产生数据
    Stream<Integer> stream = Stream.generate(random::nextInt)
            // 产生500个(无限流需要短路操作)
            .limit(500)
            // 第一个无状态操作
            .peek(s -> print("peek: " + s))
            // 第二个无状态操作
            .filter(s -> {
                print("filter: " + s);
                return s > 1000000;
            })
            // 有状态操作
            .sorted((i1,i2) -> {
                print("排序： " + i1 + "," +i2);
                return i1.compareTo(i2);
            })
            .peek(s -> {
                print("peek2: " + s);
            })
            // 并行操作
            .parallel();
    // 终止操作
    stream.count();

}

/**
 * 打印日志
 * @param s
 */
public void print(String s) {
    System.out.println(Thread.currentThread().getName() + " > " + s);
}
```