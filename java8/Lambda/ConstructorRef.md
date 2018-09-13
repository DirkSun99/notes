## 构造器引用

格式：`ClassName::new`

与函数式接口相结合，自动与函数式接口中的方法兼容。可以把构造器引用赋值给定义的方法，与构造器参数列表要与接口中抽象方法的参数列表一致！

* 示例

```java
// 构造器引用：引用无参构造方法
@Test
public void test1() {
    Supplier<Employee> sup = () -> new Employee();
    Supplier<Employee> sup2 = Employee::new;
    Employee emp = sup2.get();
    System.out.println(emp);
}

// 构造器引用：调用一个参数构造方法
@Test
public void test2() {
    Function<String, Employee> fun = x -> new Employee(x);
    Function<String, Employee> fun2 = Employee::new;
    Employee emp = fun2.apply("Nash");
    System.out.println(emp);
}
```

注意：需要调用的构造器的参数列表要与函数式接口中抽象方法的参数列表保持一致！