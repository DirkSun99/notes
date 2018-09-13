## 默认方法

Java8 中允许接口中包含具有具体实现的方法，该方法称为"默认方法"，默认方法使用 **default** 关键字修饰。

```java
default String getName() {
    return "Hello World!";
}
```

默认方法的**“类优化”**原则

若一个接口中定义了一个默认方法，而另一个父类或接口中又定义了一个同名的方法时：
* **选择父类中的方法。**如果一个父类提供了具体的实现，那么接口中具有相同名称和参数的默认方法会被忽略。

```java
/**
* 类方法，提供了getName方法
**/
public class MyClass {

    public String getName() {
        return "ooo";
    }
}

/**
* 接口默认方法，也提供了getName方法
**/
public interface MyFun {
    default String getName() {
        return "Hello World!";
    }
}

/**
* 子类：继承了MyClass，同时又实现了MyFun
**/
public class SubClass extends MyClass implements MyFun {
}

/**
* 测试用例：调用subClass的getName方法，默认使用类中的getName
**/
@Test
public void test1() {
    SubClass sub = new SubClass();
    System.out.println(sub.getName()); // 打印 ooo
}
```

* **接口冲突。**如果一个父接口提供了一个默认方法，而另一个接口也提供了一个具有相同名称和参数列表的方法（不管方法是不是默认方法），那么必须覆盖该方法来解决冲突。

```java
/**
* 接口默认方法1，提供了getName方法
**/
public interface MyFun1 {
    default String getName() {
        return "Hello World!";
    }
}

/**
* 接口默认方法1，提供了getName方法
**/
public interface MyFun2 {
    default String getName() {
        return "kkk";
    }
}

/**
* 子类实现：必须要覆写getName方法
**/
public class SubClass implements MyFun1, MyFun2 {
    @Override
    public String getName() {
        return MyFun2.super.getName();
    }
}
```