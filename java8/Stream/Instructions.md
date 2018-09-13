## Stream操作步骤
1. 创建 Stream
	
	一个数据源（如：集合、数组），获取一个流

2. 中间操作

	一个中间操作链，对数据源的数据进行处理

3. 终止操作（终端操作）
	
	一个终止操作，执行中间操作链，并产生结果


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

### 中间操作
多个**中间操作**可以连接起来形成一个**流水线**，除非流水线上触发终止操作，否则**中间操作不会执行任何处理！**而在**终止操作时一次性全部处理，称为“惰性求值”。**

* **筛选与切片**

| 方法 | 描述 |
| :------:  | :-----: |
| filter(Predicate p) | 接收Lambda，从流中排除某些元素 |
| distinct() | 筛选，通过流所生成元素的 hashCode() 和 equals() 去除重复元素 |
| limit(long maxSize) | 截断流，使其元素不超过给定数量 | 
| skip(long n) | 跳过元素，返回一个扔掉了前 n 个元素的流。<br />若流中元素不足 n 个，则返回一个空流。 与 limit(n) 互补 |


```java
// 模拟数据
List<Employee> employees = Arrays.asList(
        new Employee("Dirk",31,5555.55),
        new Employee("Kobe",25,9999.99),
        new Employee("Duncan",18,3333.33),
        new Employee("Nash",38,6666.66),
        new Employee("James",21,8888.88),
        new Employee("Kid",48,7777.77),
        new Employee("Terry",34,2222.22),
        new Employee("Terry",34,2222.22),
        new Employee("Terry",34,2222.22)
);

// 内部迭代：迭代操作由 Stream API 完成
/**
 * 使用Filter过滤元素
 */
@Test
public void test1() {
    // 中间操作：不会执行任何操作
    Stream<Employee> stream = employees.stream().filter(e -> {
        System.out.println("Stream API的中间操作");
        return e.getAge() > 35;
    });

    // 终止操作：一次性执行全部内容，即"惰性求值"
    stream.forEach(System.out::println);
}

// 外部迭代
@Test
public void test2() {
    Iterator<Employee> it = employees.iterator();

    while (it.hasNext()) {
        System.out.println(it.next());
    }
}

/**
 * 使用Limit 截断流
 */
@Test
public void test3() {
    employees.stream().filter(e -> {
        System.out.println("短路!"); // && ||
        return e.getSalary() > 5000;
    }).limit(2).forEach(System.out::println);
}

/**
 * 使用Skip，跳过元素
 */
@Test
public void test4() {
    employees.stream().filter(e -> {
        System.out.println("Stream API 中间操作");
        return e.getSalary() > 5000;
    }).skip(2).forEach(System.out::println);
}

/**
 * 使用Distinct去重
 */
@Test
public void test5() {
    employees.stream().filter(e -> e.getSalary() > 2000).distinct().forEach(System.out::println);
}
```

* **映射**

| 方法 | 描述 |
| :---: | :---: |
| map(Function f) | 接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素 |
| mapToDouble(ToDoubleFunction f) | 接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 DoubleStream |
| mapToInt(ToIntFunction f) | 接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 IntStream |
| flatMap(Funtion f) | 接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流 |

```java
/**
 * 使用map
 */
@Test
public void test6() {
    List<String> list = Arrays.asList("aaa","bbb","ccc","ddd","eee");
    list.stream().map(str -> str.toUpperCase()).forEach(System.out::println);

    System.out.println("------------------");

    employees.stream().map(Employee::getName).forEach(System.out::println);
}

/**
 * 使用FlatMap
 */
@Test
public void test7() {
    List<String> list = Arrays.asList("aaa","bbb","ccc","ddd","eee");
    Stream<Stream<Character>> stream = list.stream().map(TestStreamAPI2::filterCharacter); // {{a,a,a},{b,b,b}}
    stream.forEach(sm -> {
        sm.forEach(System.out::println);
    });

    System.out.println("-----------------------");

    Stream<Character> stream2 = list.stream().flatMap(TestStreamAPI2::filterCharacter); // {a,a,a,b,b,b}
    stream2.forEach(System.out::println);
}

public static Stream<Character> filterCharacter(String str) {
    List<Character> list = new ArrayList<>();

    for (Character ch : str.toCharArray()) {
        list.add(ch);
    }

    return list.stream();
}
```

* **排序**

| 方法 | 描述 |
| :---: | :---: |
| sorted() | 产生一个新流，其中按自然顺序排序(Comparable) |
| sorted(Comparator comp) | 产生一个新流，其中按比较器顺序排序(Comparator) |

```java
/**
 * 排序sorted 自然排序
 */
@Test
public void test8() {
    List<String> list = Arrays.asList("ccc","aaa","ddd","bbb");
    list.stream().sorted().forEach(System.out::println);
}

/**
 * 排序sorted 定制排序
 */
@Test
public void test9() {
    employees.stream().sorted((e1,e2) -> {
       if (e1.getAge() == e2.getAge()) {
           return e1.getName().compareTo(e2.getName());
       } else {
           // 按年龄降序排列
           return -Integer.compare(e1.getAge(),e2.getAge());
       }
    }).forEach(System.out::println);
}
```

### 终止操作
终端操作会从流的流水线生成结果。其结果可以是任何不是流的值。例如：List、Integer，甚至是void。

* **查找与匹配**

| 方法 | 描述 |
| :---: | :---: |
| allMatch(Predicate p) | 检查是否匹配所有元素 |
| anyMatch(Predicate p) | 检查是否至少匹配一个元素 |
| noneMatch(Predicate p) | 检查是否没有匹配所有元素 |
| findFirst() | 返回第一个元素 |
| findAny() | 返回当前流中的任意元素 |
| count | 返回流中元素总数 |
| max(Comparator c) | 返回流中最大值 |
| min(Comparator c) | 返回流中最小值 |
| forEach(Consumer c) | 内部迭代（使用 Collection 接口需要用户去做迭代，称为外部迭代）|

```java
// 模拟数据
List<Employee> employees = Arrays.asList(
        new Employee("Dirk",31,5555.55, Employee.Status.FREE),
        new Employee("Kobe",25,9999.99, Employee.Status.BUSY),
        new Employee("Duncan",18,3333.33, Employee.Status.FREE),
        new Employee("Nash",38,6666.66, Employee.Status.VOCATION),
        new Employee("James",21,8888.88, Employee.Status.BUSY),
        new Employee("Kid",48,7777.77, Employee.Status.FREE),
        new Employee("Terry",34,2222.22, Employee.Status.VOCATION)
);

/**
 * allMatch：是否匹配所有元素
 */
@Test
public void test1() {
    boolean b = employees.stream().allMatch(e -> e.getStatus().equals(Employee.Status.BUSY));
    System.out.println(b);
}

/**
 * anyMatch：是否至少匹配一个元素
 */
@Test
public void test2() {
    boolean b = employees.stream().anyMatch(e -> e.getStatus().equals(Employee.Status.BUSY));
    System.out.println(b);
}

/**
 * noneMatch：是否没有匹配所有元素
 */
@Test
public void test3() {
    boolean b = employees.stream().noneMatch(e -> e.getStatus().equals(Employee.Status.BUSY));
    System.out.println(b);
}

/**
 * findFirst：返回第一个元素
 */
@Test
public void test4() {
    Optional<Employee> op = employees.stream()
            .sorted((e1,e2) -> Double.compare(e1.getSalary(),e2.getSalary()))
            .findFirst();
    System.out.println(op.get());
}

/**
 * findAny：返回当前流中任意元素
 */
@Test
public void test5() {
    Optional<Employee> op = employees.parallelStream().filter(e -> e.getStatus().equals(Employee.Status.FREE)).findAny();
    System.out.println(op.get());
}

/**
 * count：返回流中元素总数
 */
@Test
public void test6() {
    Long count = employees.stream().count();
    System.out.println(count);
}

/**
 * max：返回流中最大值
 */
@Test
public void test7() {
    Optional<Employee> op = employees.stream().max((e1,e2) -> Double.compare(e1.getSalary(),e2.getSalary()));
    System.out.println(op.get());
}

/**
 * min：返回流中最小值
 */
@Test
public void test8() {
    Optional<Double> op = employees.stream().map(Employee::getSalary)
            .min(Double::compare);
    System.out.println(op.get());
}
```

* **归约**

| 方法 | 描述 |
| :---: | :---: |
| reduce(T iden, BinaryOperator b) | 可以将流中元素反复结合起来，得到一个值。返回T|
| reduce(BinaryOperator b) | 可以将流中元素反复结合起来，得到一个值。返回Optional<T> |

```java
/**
 * reduce
 */
@Test
public void test9() {
    List<Integer> list = Arrays.asList(1,2,3,4,5,6,7,8,9,10);

    Integer sum = list.stream().reduce(0,(x,y) -> x+y);
    System.out.println(sum);

    System.out.println("---------");

    Optional<Double> op = employees.stream().map(Employee::getSalary)
            .reduce(Double::sum);
    System.out.println(op.get());
}
```

* **收集**

| 方法 | 描述 |
| :---: | :---: |
| collect(Collector c) | 将流转换为其他形式。接收一个 Collector 接口的实现，用于给 Stream 中元素做汇总的方法 |

Collector 接口中方法的实现决定了如何对流执行收集操作（如收集到List、Set、Map）。但是 Collectors 实用类提供了很多静态方法，可以方便地创建常见收集器实例。具体方法与实例如下：

<table>
	<tr>
		<td>方法</td>
		<td>返回类型</td>
		<td>作用</td>
	</tr>
	<tr>
		<td>toList</td>
		<td>List&lt;T&gt;</td>
		<td>把流中元素收集到List</td>
	</tr>
	<tr>
		<td colspan="3">示例：List&lt;Employee&gt; emps = list.stream().collect(Collectors.toList());</td>
	</tr>
	<tr>
		<td>toSet</td>
		<td>Set&lt;T&gt;</td>
		<td>把流中元素收集到Set</td>
	</tr>
	<tr>
		<td colspan="3">示例：Set&lt;Employee&gt; emps = list.stream().collect(Collectors.toSet());</td>
	</tr>
	<tr>
		<td>toCollection</td>
		<td>Collection&lt;T&gt;</td>
		<td>把流中元素收集到创建的集合</td>
	</tr>
	<tr>
		<td colspan="3">示例：Collection&lt;Employee&gt; emps = list.stream().collect(ArrayList::new);</td>
	</tr>
	<tr>
		<td>counting</td>
		<td>Long</td>
		<td>计算流中元素的个数</td>
	</tr>
	<tr>
		<td colspan="3">示例：long count = list.stream().collect(Collectors.counting());</td>
	</tr>
	<tr>
		<td>summingInt</td>
		<td>Integer</td>
		<td>对流中元素的整数属性求和</td>
	</tr>
	<tr>
		<td colspan="3">示例：int total = list.stream().collect(Collectors.summingInt(Employee::getSalary));</td>
	</tr>
	<tr>
		<td>averagingDouble</td>
		<td>Double</td>
		<td>计算流中元素属性的平均值</td>
	</tr>
	<tr>
		<td colspan="3">示例：double avg = list.stream().collect(Collectors.averagingDouble(Employee::getSalary));</td>
	</tr>
	<tr>
		<td>summarizingInt</td>
		<td>IntSummaryStatistics</td>
		<td>收集流Integer属性的统计值。如平均值</td>
	</tr>
	<tr>
		<td colspan="3">示例：IntSummaryStatistics iss = list.stream().collect(Collectors.summarizingInt(Employee::getSalary));</td>
	</tr>
	<tr>
		<td>joining</td>
		<td>String</td>
		<td>连接流中每个字符串</td>
	</tr>
	<tr>
		<td colspan="3">示例：String str = list.stream().map(Employee::getName).collect(Collectors.joining());</td>
	</tr>
	<tr>
		<td>maxBy</td>
		<td>Optional&lt;T&gt;</td>
		<td>根据比较器选择最大值</td>
	</tr>
	<tr>
		<td colspan="3">示例：Optional&lt;Employee&gt; max = list.stream().collect(Collectors.maxBy(comparingInt(Employee::getSalary)));</td>
	</tr>
	<tr>
		<td>minBy</td>
		<td>Optional&lt;T&gt;</td>
		<td>根据比较器选择最小值</td>
	</tr>
	<tr>
		<td colspan="3">示例：Optional&lt;Employee&gt; min = list.stream().collect(Collectors.minBy(comparingInt(Employee::getSalary)));</td>
	</tr>
	<tr>
		<td>reducing</td>
		<td>归约产生的类型</td>
		<td>从一个作为累加器的初始化值开始，利用BinaryOperator与流中元素逐个结合，从而归约成单个值</td>
	</tr>
	<tr>
		<td colspan="3">示例：int total = list.stream().collect(Collectors.reducing(0, Employee::getSalary, Integer::sum))</td>
	</tr>
	<tr>
		<td>collectingAndThen</td>
		<td>转换函数返回的类型</td>
		<td>包裹另一个收集器，对其结果转换函数</td>
	</tr>
	<tr>
		<td colspan="3">示例：int low = list.stream().collect(Collectors.collectingAndThen(Collectors.toList(), List::size))</td>
	</tr>
	<tr>
		<td>groupingBy</td>
		<td>Map&lt;K, List&lt;T&gt;&gt;</td>
		<td>根据某属性值对流分组，属性为K，结果为V</td>
	</tr>
	<tr>
		<td colspan="3">示例：Map&lt;Employee&gt; map = list.stream().collect(Collectors.groupingBy(Employee::Status))</td>
	</tr>
	<tr>
		<td>partitioningBy</td>
		<td>Map&lt;Boolean, List&lt;T&gt;&gt;</td>
		<td>根据true或false进行分区</td>
	</tr>
	<tr>
		<td colspan="3">示例：Map&lt;Boolean, List&lt;Employee&gt;&gt; map = list.stream().collect(Collectors.partitioningBy(e -> e.getSalary() > 8000)</td>
	</tr>
</table>

```java
/**
 * Collect: 收集
 */
@Test
public void test10() {
    List<String> list = employees.stream().map(Employee::getName)
            .collect(Collectors.toList());
    list.forEach(System.out::println);

    System.out.println("-------------");
    Set<String> hs = employees.stream().map(Employee::getName)
            .collect(Collectors.toCollection(HashSet::new));
    hs.forEach(System.out::println);
}

/**
 * Collect：收集 用法展示
 */
@Test
public void test11() {
    // 总数
    Long count = employees.stream().collect(Collectors.counting());
    System.out.println(count);

    System.out.println("------------");

    // 平均值
    Double avg = employees.stream().collect(Collectors.averagingDouble(Employee::getSalary));
    System.out.println(avg);

    System.out.println("---------------");

    // 总和
    Double sum = employees.stream().collect(Collectors.summingDouble(Employee::getSalary));
    System.out.println(sum);

    System.out.println("---------------");

    // 最大值
    Optional<Employee> max = employees.stream().collect(Collectors.maxBy((e1,e2) -> Double.compare(e1.getSalary(),e2.getSalary())));
    System.out.println(max.get());

    System.out.println("---------------");

    Optional<Double> min = employees.stream().map(Employee::getSalary).collect(Collectors.minBy(Double::compare));
    System.out.println(min.get());
}

/**
 * 分组
 */
@Test
public void test12() {
    Map<Employee.Status, List<Employee>> map = (Map<Employee.Status, List<Employee>>) employees.stream().collect(Collectors.groupingBy(Employee::getStatus));
    System.out.println(map);
}

// 多级分组
@Test
public void test13() {
    Map<Employee.Status, Map<String, List<Employee>>> map = employees.stream().collect(Collectors.groupingBy(Employee::getStatus,Collectors.groupingBy(e -> {
        if (e.getAge() <= 35) {
            return "青年";
        } else if (e.getAge() <= 50){
            return "中年";
        } else {
            return "老年";
        }
    })));

    System.out.println(map);
}

// 分区
@Test
public void test14() {
    Map<Boolean, List<Employee>> map = employees.stream().collect(Collectors.partitioningBy(e -> e.getSalary() > 8000));
    System.out.println(map);
}

@Test
public void test15() {
    DoubleSummaryStatistics dss = employees.stream().collect(Collectors.summarizingDouble(Employee::getSalary));
    System.out.println(dss);
}

// 连接
@Test
public void test16() {
    String str = employees.stream().map(Employee::getName).collect(Collectors.joining(","));
    System.out.println(str);

    String str2 = employees.stream().map(Employee::getName).collect(Collectors.joining(",","===","==="));
    System.out.println(str2);
}
```

