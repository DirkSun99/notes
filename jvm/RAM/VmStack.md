## Java 虚拟机栈

* 虚拟机栈描述的是 Java 方法执行的动态内存模型

* 栈帧
	
	* 每个方法执行，都会创建一个栈帧，伴随着方法从创建到执行完成。用于存储局部变量表，操作数栈，动态链接，方法出口等。

* 局部变量表
	
	* 存放编译期可知的各种基本数据类型，引用类型，returnAddress类型。

	* 局部变量表的内存空间在编译期完成分配，当进入一个方法时，这个方法需要在帧分配多少内存是固定的，在方法运行期间是不会改变局部变量表的大小。

* 大小
	
	如果超出了虚拟机栈的大小，会出现如下异常

	* StackOverflowError

	* OutOfMemory

	```java
	/**
     * 模拟 StackOverflowError
     * 方法一直进栈
     */
    @Test
    public void test() {
        System.out.println("方法执行...");
        test();
    }
	```