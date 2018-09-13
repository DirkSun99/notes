## 内置四大核心接口

* **Consumer<T>：消费型接口**：`void accept(T t);`
	* 示例
	```java
	// 需求：购买物品，花费RMB
    public void buy(double money, Consumer<Double> con) {
        con.accept(money);
    }

    @Test
    public void test1() {
        buy(3000000, m-> System.out.println("购买房子消费"+m+"元"));
    }
	```

* **Supplier<T>：供给型接口**：`T get();`
	* 示例
	```java
	// 需求：产生指定个数整数，并放入集合中
    public List<Integer> getNumberList(int num, Supplier<Integer> sup) {
        List<Integer> list = new ArrayList<>();

        for (int i=0; i<num; i++) {
            Integer n = sup.get();
            list.add(n);
        }

        return list;
    }

    @Test
    public void test2() {
        List<Integer> numList = getNumberList(10,() -> (int)(Math.random() * 1000));
        for (Integer num: numList) {
            System.out.println(num);
        }
    }
	```

* **Function<T, R>：函数型接口**：`R apply(T t);`
	* 示例
	```java
	// 需求：处理字符串
    public String strHandler(String str, Function<String, String> fun) {
       return fun.apply(str);
    }

    @Test
    public void test3() {
        String newStr = strHandler("abcde",(str) -> str.toUpperCase());
        System.out.println(newStr);
    }
	```

* **Predicate<T>：断言型接口**：`boolean test(T t);`
	* 示例
	```java
	//需求：将满足条件的字符串，放入集合中
    public List<String> filterStr(List<String> list, Predicate<String> pre) {
        List<String> strList = new ArrayList<>();
        for(String str: list) {
            if (pre.test(str)) {
                strList.add(str);
            }
        }

        return strList;
    }

    @Test
    public void test4() {
        List<String> list = Arrays.asList("Dirk","Nash","Kid","Duncan","Kobe");
        List<String> strList = filterStr(list,(str)->str.length()>4);
        for(String str: strList) {
            System.out.println(str);
        }
    }
	```
