### 创建Stream
* 通过 Collection 系列集合提供的 stream() 或 parallelStream()，适用于集合
* 通过 Arrays 中的静态方法 stream() 获取数组流，适用于数组
* 通过 Stream 类中的静态方法 of()
* 创建无限流
	* 迭代
	* 生成
* 通过 IntStream 或 LongStream 的 range() 或 rangeClosed() 方法，创建数字流
* 通过 Random 的 ints() 或 longs() 或 doubles() 方法，创建随机数字流

示例代码
```java
// 创建Stream
@Test
public void test1() {
    // 通过 Collection 系列集合提供的 stream() 或 parallelStream()
    List<String> list = new ArrayList<>();
    Stream<String> stream1 = list.stream();

    // 通过 Arrays 中的静态方法 stream() 获取数组流
    Employee[] emps = new Employee[10];
    Stream<Employee> stream2 = Arrays.stream(emps);

    // 通过 Stream 类中的静态方法 of()
    Stream<String> stream3 = Stream.of("aa","bb","cc");
    
    // 创建无限流
    // 迭代
    Stream<Integer> stream4 = Stream.iterate(0,x -> x+2);
    stream4.limit(10).forEach(System.out::println);
    // 生成
    Stream.generate(() -> Math.random()).limit(5).forEach(System.out::println);

    // 创建数字流
    IntStream stream5 = IntStream.of(1,2,3);
    LongStream stream6 = LongStream.rangeClosed(1,10);
    DoubleStream stream7 = new Random().doubles().limit(10);
}
```