## Lambda示例展示

* **示例1：自定义TreeSet排序规则**

```java
/**
 * 使用匿名内部类，指定TreeSet排列顺序
 */
@Test
public void test1() {
    // 排序规则，从大到小
    Comparator<Integer> com = new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            return Integer.compare(o2, o1); // 有效代码
        }
    };
    
    // 声明TreeSet
    Set<Integer> ts = new TreeSet<>(com);
    ts.add(1);
    ts.add(5);
    ts.add(3);
    ts.add(12);
    
    // 遍历TreeSet打印
    ts.forEach(System.out::println);
}

/**
 * 使用Lambda表达式，指定TreeSet排序规则
 */
@Test
public void test2() {
    Comparator<Integer> com = (x, y) -> Integer.compare(y, x);
    // 声明TreeSet
    Set<Integer> ts = new TreeSet<>(com);
    ts.add(1);
    ts.add(5);
    ts.add(3);
    ts.add(12);

    // 遍历TreeSet打印
    ts.forEach(System.out::println);
}
```

* <font color='red'><b>示例2：过滤员工信息</b></font> : 根据此示例引出Lambda表达式及StreamAPI

```java
/**
 * 员工类
 */
 @Data
 @ToString
 @NoArgsConstructor
 @AllArgsConstructor
public class Employee {
    /**
     * 姓名
     */
    private String name;

    /**
     * 年龄
     */
    private int age;

    /**
     * 工资
     */
    private double salary;
}

// 模拟数据，正式开发时会从数据库中读取
List<Employee> employees = Arrays.asList(
    new Employee("Dirk",31,5555.55),
    new Employee("Kobe",25,9999.99),
    new Employee("Duncan",18,3333.33),
    new Employee("Nash",38,6666.66),
    new Employee("James",21,8888.88),
    new Employee("Kid",48,7777.77),
    new Employee("Terry",34,2222.22)
);

/**
 * 需求1：获取当前公司员工年龄大于35的员工信息
 * @param list
 * @return
 */
public List<Employee> filterEmployees(List<Employee> list) {
    List<Employee> emps = new ArrayList<>();
    for (Employee emp: list) {
        if (emp.getAge() >= 35) {
            emps.add(emp);
        }
    }
    return emps;
}

/**
* 需求1测试
*/
@Test
public void test3() {
    List<Employee> list = filterEmployees(employees);
    for (Employee employee : list) {
        System.out.println(employee);
    }
}

/**
 * 需求2：获取当前公司中员工工资大于5000的员工信息
 */
public List<Employee> filterEmployee2(List<Employee> list) {
    List<Employee> emps = new ArrayList<>();
    for (Employee emp: list) {
        if (emp.getSalary() >= 5000) {
            emps.add(emp);
        }
    }
    return emps;
}

/**
* 需求3、需求4.....
**/
```

上面代码存在问题：过滤条件千变万化，不可能针对每一个需求编写一段过滤代码；代码几乎相同，只是过滤条件有差别，考虑抽取共通部分。

**优化方式一：使用策略设计模式**

```java
/**
 * 1. 声明策略接口
 * @param <T>
 */
public interface MyPredicate<T> {
    public boolean test(T t);
}


// 2. 根据需求编写过滤策略，每个需求对应一个策略

/**
 * 按员工年龄过滤
 */
public class FilterEmployeeByAge implements MyPredicate<Employee> {

    @Override
    public boolean test(Employee employee) {
        return employee.getAge() >= 35;
    }
}

/**
 * 按员工工资过滤
 */
public class FilterEmployeeBySalary implements MyPredicate<Employee> {

    @Override
    public boolean test(Employee employee) {
        return employee.getSalary() >= 5000;
    }
}

/**
 * 3. 通用过滤方法
 * @param list
 * @param mp
 * @return
 */
public List<Employee> filterEmployee(List<Employee> list, MyPredicate<Employee> mp) {
    List<Employee> emps = new ArrayList<>();

    for (Employee employee : list) {
        if (mp.test(employee)) {
            emps.add(employee);
        }
    }
    return emps;
}

/**
* 4. 编写测试用例
*/
@Test
public void test4() {
    List<Employee> list = filterEmployee(employees, new FilterEmployeeByAge());
    for (Employee employee : list) {
        System.out.println(employee);
    }

    System.out.println("--------华丽丽的分隔符--------");

    List<Employee> list2 = filterEmployee(employees, new FilterEmployeeBySalary());
    for (Employee employee : list2) {
        System.out.println(employee);
    }
}
```

优化方式一存在问题：需要为每一个需求编写一个策略实现类；策略实现类中的条件不够灵活，比如在实现类中写死了过滤条件（employee.getAge() >= 35）

** 优化方式二：使用匿名内部类 **

```java
/**
 * 优化方式二：在优化方式一的基础上使用匿名内部类
 */
@Test
public void test5() {
    List<Employee> list = filterEmployee(employees, new MyPredicate<Employee>() {
        @Override
        public boolean test(Employee employee) {
            return employee.getSalary() <= 5000; // 核心代码
        }
    });

    for (Employee employee : list) {
        System.out.println(employee);
    }
}
```

优化方式二存在问题：代码不够简洁，有用代码只有一句

** 优化方式三：使用lambda表达式 **

```java
/**
 * 优化方式三：Lambda 表达式
 */
@Test
public void test6() {
    List<Employee> list = filterEmployee(employees, (e) -> e.getSalary() <= 5000);
    list.forEach(System.out::println);
}
```

优化方式三存在问题：需要自定义MyPredicate接口

** 优化方式四：Stream API **

```java
/**
 * 优化方式四：Stream API
 */
@Test
public void test7() {
    employees.stream()
            .filter(e -> e.getSalary() >= 5000)
            .limit(2)
            .forEach(System.out::println);
}
```

扩展：lambda表达式map示例

```java
/**
 * 扩展：获取所有员工的姓名
 */
@Test
public void test8() {
    employees.stream().map(e -> e.getName()).forEach(System.out::println);
}
```