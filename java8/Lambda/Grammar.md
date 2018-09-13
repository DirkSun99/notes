## 基础语法

Java8中引入了一个新的操作符 `->` 该操作符称为箭头操作符或 Lambda 操作符

箭头操作符将 Lambda 表达式拆分成两部分：

左侧：Lambda 表达式的参数列表

右侧：Lambda 表达式中所需执行的功能， 即 Lambda 体

* 语法格式一：无参数，无返回值: `() -> System.out.println("Hello Lambda!");`
	* 示例

	```java
	@Test
    public void test1() {
        Runnable r = new Runnable() {
            @Override
            public void run() {
                System.out.println("Hello World!");
            }
        };

        r.run();

        System.out.println("----------");

        Runnable r1 = () -> System.out.println("Hello Lambda");

        r1.run();
    }
	```

* 语法格式二：有一个参数，无返回值：`(x) -> System.out.println(x);`
	* 示例
	```java
	@Test
    public void test2() {
        Consumer<String> con = (x) -> System.out.println(x);
        con.accept("测试");
    }
	```

* 语法格式三：若只有一个参数，小括号可以省略不写：`x -> System.out.println(x);`

* 语法格式四：有两个以上的参数，有返回值，并且 Lambda 体中有多条语句
	* 示例
	```java
	@Test
    public void test3() {
        Comparator<Integer> com = (x, y) -> {
            System.out.println("函数式接口");
            return Integer.compare(x,y);
        };
    }
	```

* 语法格式五：若 Lambda 体中只有一条语句，`return` 和 `{}` 都可以省略不写
	* 示例
	```java
	@Test
    public void test4() {
        Comparator<Integer> com = (x,y) -> Integer.compare(x,y);
    }
	```

* 语法格式六：Lambda 表达式的参数列表的数据类型可以省略不写，因为JVM编译器通过上下文推断出数据类型，即“类型推断”。如果一个参数写了数据类型，所有的参数都需要写数据类型
	* 示例
	```java
	@Test
    public void test5() {
        Comparator<Integer> com = (Integer x, Integer y) -> Integer.compare(x,y);
    }
	```


Lambda 表达式需要“函数式接口”支持
函数式接口：接口中只有一个抽象方法的接口，称为函数式接口。可以使用注解`@FunctionalInterface`修饰

示例：对一个数进行运算
```java
// 1. 定义一个函数式接口
@FunctionalInterface
public interface MyFun {

    public Integer getValue(Integer num);
}

// 2. 编写实现
public Integer operation(Integer num, MyFun mf) {
    return mf.getValue(num);
}

// 3. 编写测试用例
@Test
public void test6() {
    Integer num = operation(100, x -> x*x);
    System.out.println(num);

    System.out.println(operation(200, y->y+200));
}
```