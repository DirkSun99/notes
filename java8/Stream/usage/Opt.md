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