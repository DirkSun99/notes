## 静态方法

Java8 中，接口中允许添加静态方法，使用关键字**static**修饰

```java
/**
* 接口中的静态方法
**/
public interface MyFun {
    static void show() {
        System.out.println("接口中的静态方法");
    }
}

/**
* 测试用例
**/
@Test
public void test1() {
    MyFun.show();
}
```