## Optional 类

Optional<T> 类 (java.util.Optional) 是一个容器类，代表一个值存在或不存在。原来用 null 表示一个值不存在，现在 Optional 可以更好的表达这个概念，并且可以避免空指针异常。

| 方法 | 描述 |
| :--: | :--: |
| Optional.of(T t) | 创建一个 Optional 实例 |
| Optional.empty() | 创建一个空的 Optional 实例 |
| Optional.ofNummable(T t) | 若 t 不为 null，创建 Optional实例，否则创建空实例 |
| isPresent() | 判断是否包含值 |
| orElse(T t) | 如果调用对象包含值，返回该值，否则返回t |
| orElseGet(Supplier s) | 如果调用对象包含值，返回该值，否则返回 s 获取的值 | 
| map(Function f) | 如果有值对其处理，并返回处理后的 Optional, 否则返回 Optional.empty() |
| flatMap(Function f) | 与 map 类似，要求返回值必须是 Optional |

```java
/**
 * of
 */
@Test
public void test1() {
    Optional<Employee> op = Optional.of(new Employee());
    Employee emp = op.get();
    System.out.println(emp);
}

/**
 * empty
 */
@Test
public void test2() {
    Optional<Employee> op = Optional.empty();
    System.out.println(op.get());
}

/**
 * ofNullable
 */
@Test
public void test3() {
    Optional<Employee> op = Optional.ofNullable(null);
    if (op.isPresent()) {
        System.out.println(op.get());
    }
}

/**
 * orElse
 */
@Test
public void test4() {
    Optional<Employee> op = Optional.ofNullable(null);
    Employee emp = op.orElse(new Employee("aa",18,1000,Employee.Status.VOCATION));
    System.out.println(emp);
}

/**
 * orElseGet
 */
@Test
public void test5() {
    Optional<Employee> op = Optional.ofNullable(null);
    Employee emp = op.orElseGet(Employee::new);
    System.out.println(emp);
}

/**
 * map
 */
@Test
public void test6() {
    Optional<Employee> op = Optional.ofNullable(new Employee("aa",18,1000,Employee.Status.VOCATION));
    Optional<String> str = op.map(e -> e.getName());
    System.out.println(str.get());
}

/**
 * flatMap
 */
@Test
public void test7() {
    Optional<Employee> op = Optional.ofNullable(new Employee("aa",18,1000,Employee.Status.VOCATION));
    Optional<String> str = op.flatMap(e -> Optional.of(e.getName()));
    System.out.println(str.get());
}
```