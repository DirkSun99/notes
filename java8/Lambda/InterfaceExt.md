## 核心接口

| 函数式接口 | 参数类型 | 返回类型 | 用途 |
| :-----:  | :------: | :------: | :-----: |
| Consumer<T> <br /> 消费型接口 | T | void | 对类型为T的对象应用操作，<br/>包含方法：void accept(T t) |
| Supplier<T> <br /> 供给型接口 | 无 | T | 返回类型为T的对象，<br />包含方法：T get() |
| Function<T,R> <br /> 函数式接口 | T | R | 对类型为T的对象应用操作，并返回结果。<br />结果是R类型的对象。<br />包含方法：R apply(T t) |
| Predicate<T> <br />断言型接口 | T | boolean | 确定类型为T的对象是否满足某约束，并返回boolean值。<br />包含方法：boolean test(T t) |

## 扩展接口

| 函数式接口 | 参数类型 | 返回类型 | 用途 |
| :------:  | :-----: | :-----: | :---: |
| BiFunction<T,U,R> | T,U | R | 对类型为T, U参数应用操作，返回R类型的结果<br />包含方法为：R apply(T t, U u) |
| UnaryOperator<T><br />(Function子接口) | T| T | 对类型为T的对象进行一元运算，并返回T类型的结果。<br />包含方法为：T apply(T t) |
| BinaryOperator<T><br />(BiFunction子接口) | T,T | T | 对类型为T的对象进行二元运算，并返回T类型的结果。<br />包含方法为：T apply(T t1, T t2) |
| BiConsumer<T,U> | T,U | void | 对类型为T,U参数应用操作。<br />包含方法为：void accept(T t, U u) |
| ToIntFunction<T> <br /> ToLongFunction<T> <br /> ToDoubleFunction<T> | T | int <br />long<br /> double | 分别计算int、long、double值的函数 |
| IntFunction<R> <br /> LongFunction<R> <br /> DoubleFunction<R> | int <br /> long <br /> double | R | 参数分别为int、long、double类型的函数 |