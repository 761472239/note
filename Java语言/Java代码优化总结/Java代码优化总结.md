# Java代码优化总结

## 目录

### for循环中复用同一个对象的引用

例如:

```java
List<SortPerson> list = new ArrayList();
long start = System.currentTimeMillis();
for (int i = 0; i < 1000000; i++) {
  list.add(new SortPerson("aa" + i, i));
}
long end = System.currentTimeMillis();
System.out.println(end - start); // 5-6毫秒

SortPerson sortPerson;
long start1 = System.currentTimeMillis();
for (int i = 0; i < 1000000; i++) {
  sortPerson = new SortPerson("aa" + i, i);
  list.add(sortPerson);
}
long end1 = System.currentTimeMillis();
System.out.println(end1-start1); // 0毫秒

```

当 i=1000 时，6/0
当 i=1 0000时，11/3
当 i=10 0000时，41/15
当 i=100 0000时，103/121
当数量高于1000000时，这种写法就没有什么效果了，需要进一步研究
