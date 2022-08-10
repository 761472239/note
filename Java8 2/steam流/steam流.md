# steam流

## 目录

*   [包装数据类型](#包装数据类型)

*   [处理Map对象](#处理map对象)

*   [处理List对象](#处理list对象)

## 包装数据类型

```java
@Test
public void main22() {
    List<Integer> list = new ArrayList<Integer>(){{
        add(7); add(5); add(1); add(2);
        add(8); add(4); add(3); add(6);
        add(3); add(6); add(3); add(6);
    }};
    // 过滤
    List<Integer> filterList = list.stream()
            .filter(a -> a < 5).collect(Collectors.toList());
    System.out.println(filterList); // [1, 2, 4, 3, 3, 3]
    // 排序（正序）
    List<Integer> sortList1 = list.stream()
            .sorted().collect(Collectors.toList());
    System.out.println(sortList1); // [1, 2, 3, 3, 3, 4, 5, 6, 6, 6, 7, 8]

    List<Integer> sortList2 = list.stream()
            .sorted(Comparator.comparing(a -> a, Comparator.naturalOrder()))
            .collect(Collectors.toList());
    System.out.println(sortList2); // [1, 2, 3, 3, 3, 4, 5, 6, 6, 6, 7, 8]

    // 排序（倒序）
    List<Integer> sortList3 = list.stream()
            .sorted(Comparator.comparing(a -> a, Comparator.reverseOrder()))
            .collect(Collectors.toList());
    System.out.println(sortList3); // [8, 7, 6, 6, 6, 5, 4, 3, 3, 3, 2, 1]

    // 最大数
    Integer max = list.stream().max(Comparator.comparing(a -> a, Comparator.naturalOrder())).get();
    System.out.println(max);

    // 最小数
    Integer min = list.stream().min(Comparator.comparing(a -> a, Comparator.naturalOrder())).get();
    System.out.println(min);

    // 去重
    List<Integer> distinctList = list.stream().distinct().collect(Collectors.toList());
    System.out.println(distinctList); // [7, 5, 1, 2, 8, 4, 3, 6]

    // 对每个元素进行操作
    List<Integer> mapList = list.stream().map(a -> a * a).collect(Collectors.toList());
    System.out.println(mapList); // [49, 25, 1, 4, 64, 16, 9, 36, 9, 36, 9, 36]
}
```

## 处理Map对象

*   数据初始化

```java
HashMap<String, Integer> map = new HashMap<String, Integer>() {{
    put("A", 1);put("B", 2);put("C", 3);
    put("D", 4);put("E", 1);put("F", 1);
    put("", 1);put("", 1);
}};
```

*   key集合转list

```java
List<String> keyList1 = map.keySet().stream()
        .filter(a -> StringUtils.isNotBlank(a)).collect(Collectors.toList());
System.out.println(keyList1);
List<String> keyList2 = map.entrySet().stream()
        .map(entry -> entry.getKey()).filter(a -> StringUtils.isNotBlank(a)).collect(Collectors.toList());

```

> 输出：\[A, B, C, D, E, F]

*   value集合转list

```java
List<Integer> valueList = map.entrySet().stream()
        .map(entry -> entry.getValue()).collect(Collectors.toList());
System.out.println(valueList);

// 处理value，同理可处理key
Map<String, Integer> newMap = map.entrySet().stream()
        .collect(Collectors.toMap(Map.Entry::getKey, a -> a.getValue() * a.getValue()));
System.out.println(newMap);
```

> \[1, 1, 2, 3, 4, 1, 1]
> {=1, A=1, B=4, C=9, D=16, E=1, F=1}

*   key和value换位，key冲突时，新value替换旧value

```java
Map<Integer, String> reMap1 = map.entrySet().stream()
        .collect(Collectors.toMap(Map.Entry::getValue,
        Map.Entry::getKey, (String val1, String val2) -> val2));
```

> {1=F, 2=B, 3=C, 4=D}

*   key和value换位，key冲突时，加入value列表中

```java
Map<Integer, List<String>> reMap2 = map.entrySet().stream()
              .collect(Collectors.toMap(Map.Entry::getValue,
              a -> new ArrayList<String>() {{
                  add(a.getKey());
              }},
              (List<String> v1, List<String> v2) -> {
                  v1.addAll(v2);
                  return v1;
              }
      ));
```

> {1=\[, A, E, F], 2=\[B], 3=\[C], 4=\[D]}

## 处理List对象

*   初始化数据

```java
public class User {
    public User(Integer id, String name, Integer age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public User() {
    }

    private Integer id;
    private String name;
    private Integer age;


    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        final StringBuilder sb = new StringBuilder();
        sb.append("{")
                .append("\"id\":").append(id)
                .append(", \"name\":").append(name)
                .append(", \"age\":").append(age)
                .append('}');
        return sb.toString();
    }
}
```

添加到List容器中

```java
List<User> list = new ArrayList<User>() {{
    add(new User(1, "张三", 18));
    add(new User(2, "李四", 22));
    add(new User(3, "王五", 24));
    add(new User(4, "赵六", 28));
    add(new User(5, "孙七", 28));
}};
```

*   list转map，如遇到key冲突，可参考第二段map解决办法

```java
Map<Integer, User> map1 = list.stream()
        .collect(Collectors.toMap(User::getId, Function.identity()));
System.out.println(map1);

Map<Integer, User> map2 = list.stream().collect(Collectors.toMap(User::getId, a -> a)); 
```

> {1={"id":1, "name":张三, "age":18}, 2={"id":2, "name":李四, "age":22}, 3={"id":3, "name":王五, "age":24}, 4={"id":4, "name":赵六, "age":28}, 5={"id":5, "name":孙七, "age":28}}

*   查找某一个字段值

```java
// 查找
User user = list.stream().filter(a -> "张三".equals(a.getName())).findFirst().get();
System.out.println(user);
```

*   过滤计数

```java
// 过滤计数
long count = list.stream().filter(a -> a.getAge() >= 22).count();
System.out.println(count);
```

> 4

*   排序

```java
  // 排序（通过年龄排正序）
  List<User> orderList = list.stream()
          .sorted(Comparator.comparing(User::getAge, Comparator.naturalOrder()))
          .collect(Collectors.toList());
  // 排序（通过年龄排倒序）
  List<User> reorderList = list.stream()
          .sorted(Comparator.comparing(User::getAge, Comparator.reverseOrder()))
          .collect(Collectors.toList());
```

*   对象去重（通过ID去重）

```java
List<User> unionUser = list.stream().collect(
        Collectors.collectingAndThen(Collectors.toCollection(
                () -> new TreeSet<>(Comparator.comparing(o -> o.getId()))
        ), ArrayList::new));
```

*   属性转list集合

```java
 List<String> nameList = list.stream().map(User::getName).collect(Collectors.toList());
```

*   截取，skip：跳过前n个，limit：取n个

```java
List<User> limitList = list.stream().skip(1).limit(2).collect(Collectors.toList());
```

*   判断

```java
// 判断是否存在（半月是否存在）
boolean isHave1 = list.stream().anyMatch(a -> "张三".equals(a.getName()));
// 判断所有是否满足（是否都大于16岁）
boolean fullAge = list.stream().allMatch(a -> a.getAge() > 16);
// 判断是否不存在（里面没有ID为10的用户）
boolean isHave2 = list.stream().noneMatch(a -> "10".equals(a.getId()));
```
