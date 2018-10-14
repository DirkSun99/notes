### 终止操作
终端操作会从流的流水线生成结果。其结果可以是任何不是流的值。例如：List、Integer，甚至是void。

* **查找与匹配**

| 方法 | 描述 | 短路 | 
| :---: | :---: | :---: |
| allMatch(Predicate p) | 检查是否匹配所有元素 | 是 |
| anyMatch(Predicate p) | 检查是否至少匹配一个元素 | 是 | 
| noneMatch(Predicate p) | 检查是否没有匹配所有元素 | 是 |
| findFirst() | 返回第一个元素 | 是 | 
| findAny() | 返回当前流中的任意元素 | 是 |
| count | 返回流中元素总数 | 否 | 
| max(Comparator c) | 返回流中最大值 | 否 | 
| min(Comparator c) | 返回流中最小值 | 否 | 
| forEach(Consumer c) | 内部迭代（使用 Collection 接口需要用户去做迭代，称为外部迭代）| 否 |
| forEachOrdered(Consumer c) | 使用 parallel().sorted() 后使用 | 否 |

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

| 方法 | 描述 | 短路 | 
| :---: | :---: | :---: | 
| reduce(T iden, BinaryOperator b) | 可以将流中元素反复结合起来，得到一个值。返回T| 否 | 
| reduce(BinaryOperator b) | 可以将流中元素反复结合起来，得到一个值。返回Optional<T> | 否 |

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

| 方法 | 描述 | 短路 | 
| :---: | :---: | :---: |
| collect(Collector c) | 将流转换为其他形式。接收一个 Collector 接口的实现，用于给 Stream 中元素做汇总的方法 | 否 |

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

	// 最小值
    Optional<Double> min = employees.stream().map(Employee::getSalary).collect(Collectors.minBy(Double::compare));
    System.out.println(min.get());
}

/**
 * 分组
 */
@Test
public void test12() {
	// 按员工状态分组，得到每个组中员工信息
    Map<Employee.Status, List<Employee>> map = employees.stream().collect(Collectors.groupingBy(Employee::getStatus));
    System.out.println(map);

	// 按员工状态分组，得到每个组员工个数
	Map<Employee.Status, Long> map1 = employees.stream().collect(Collectors.groupingBy(Employee::getStatus,Collectors.counting()));
    System.out.println(map1);
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

// 统计信息汇总: 总数(count)、总和(sum)、最小值(min)、最大值(max)、平均值(average)
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