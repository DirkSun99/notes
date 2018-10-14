## 内存分配策略

* 优先分配到Eden
* 大对象直接分配到老年代
* 长期存活的对象分配到老年代
* 空间分配担保
	
	`-XX:+HandlePromotionFailure`：开启

	`-XX:-HandlePromotionFailure`：关闭
* 动态对象年龄判断
* 逃逸分析与栈上分配
	* 逃逸分析就是分析对象的作用域