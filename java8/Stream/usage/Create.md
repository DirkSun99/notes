### 创建Stream
* 通过 Collection 系列集合提供的 stream() 或 parallelStream()
* 通过 Arrays 中的静态方法 stream() 获取数组流
* 通过 Stream 类中的静态方法 of()
* 创建无限流
	* 迭代
	* 生成
* 示例代码
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
}
```