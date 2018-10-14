## 引用计数算法

* 在对象中添加一个引用计数器，当有地方引用这个对象的时候，引用计数器的值就+1，当引用失效的时候，计数器的值就-1。

* 打印垃圾回收信息

	* `-verbose:gc`
	* `-XX:+PrintGCDetails`

* 在Jvm中，并不使用引用计数算法作为回收算法，原因是引用计数法对于对象循环引用时无法进行回收。

```java
public class GCTest {
    /**
     * 属性：对象引用
     */
    private Object instance;

    /**
     * 构造方法，模拟占用20M内存空间
     */
    public GCTest() {
        byte[] m = new byte[20 * 1024 * 1024];
    }

    /**
     * 声明两个对象，两个对象循环引用
     * 当两个对象的引用分别断开连接后，查看JVM是否会回收此对象
     * 如果使用引用计数法，则不会回收
     * 实际结果是会回收，说明JVM使用的不是引用计数法
     */
    @Test
    public void test() {

        // 创建对象1
        GCTest gc1 = new GCTest();

        // 创建对象2
        GCTest gc2 = new GCTest();

        // 对象1引用对象2，形成内部引用
        gc1.instance = gc2;
        // 对象2引用对象1，形成内部引用
        gc2.instance = gc1;

        // 断开对象1的引用
        gc1 = null;
        // 断开对象2的引用
        gc2 = null;

        // 垃圾回收
        System.gc();
    }
}
```