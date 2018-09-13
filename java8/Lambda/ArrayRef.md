## 数组引用

格式：`Type[]::new`

* 示例
```java
// 数组引用
@Test
public void test7() {
    Function<Integer,String[]> fun = x -> new String[x];
    String[] strs = fun.apply(10);
    System.out.println(strs.length);

    Function<Integer, String[]> fun2 = String[]::new;
    String[] strs2 = fun2.apply(20);
    System.out.println(strs2.length);
}
```