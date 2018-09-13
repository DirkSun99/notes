## 方法引用

当要传递给 Lambda 体的操作，已经有实现的方法了，可以使用“方法引用”。（可以理解为方法引用是 Lambda 表达式的另外一种表现形式）

**实现抽象方法的参数列表，必须与方法引用方法的参数列表保持一致！**

方法引用：使用操作符 `::` 将方法名和对象或类的名字分隔开来。

* 三种语法格式
	* 对象::实例方法名
	```java
	//对象::实例方法名
    @Test
    public void test1() {
        Consumer<String> con = (x) -> System.out.println(x);
        PrintStream ps = System.out;
        Consumer<String> con1 = ps::println;
        Consumer<String> con2 = System.out::println;
    }

    @Test
    public void test2() {
        Employee emp = new Employee();
        Supplier<String> sup = () -> emp.getName();
        String str = sup.get();
        System.out.println(str);

        Supplier<Integer> sup1 = emp::getAge;
        Integer num = sup1.get();
        System.out.println(num);
    }
	```
	* 类::静态方法名
	```java
	//类::静态方法名
    @Test
    public void test3() {
        Comparator<Integer> com = (x,y) -> Integer.compare(x,y);
        Comparator<Integer> com1 = Integer::compare;
    }
	```
	* 类::实例方法名
	```java
	//类::实例方法名
    @Test
    public void test4() {
        BiPredicate<String, String> bp = (x, y) -> x.equals(y);
        BiPredicate<String, String> bp2 = String::equals;
    }
	```

* 注意
	* Lambda 体中调用方法的参数列表与返回值类型，要与函数式接口中抽象方法的函数列表和返回值类型保持一致！
	* 若 Lambda 参数列表中第一个参数是**实例方法的调用者**，而第二个参数是**实例方法的参数**时，可以使用`ClassName::method`