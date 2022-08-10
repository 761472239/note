# Java高级操作之try-with-catch自动关闭资源

要求：JDK1.7

新语法：

```java
try(
  //定义且获取资源
){
  //正常的使用资源
}catch(Exception e){
  e.printStackTrace();
}


```

try后面多了一个括号，这个括号里面写资源的定义和创建，如果没有其他处理可以不写finally块，会自动释放资源。

新的写法：

```java
Path path= Paths.get(file.getAbsolutePath());
try (
   BufferedWriter writer= Files.newBufferedWriter(path,StandardCharsets.UTF_8);
){
   writer.write(context);
} catch (IOException e) {
  e.printStackTrace();
}

```

以前的写法：

```java
  定义资源=null;
try{
  //创建资源
}catch(Exception e){
  //捕获异常
  e.printStackTrace();
}finally{
  //有资源使用需要确保关闭
}

```

新语法的原理：

资源继承了`AutoCloseable`接口，编译器会调用实现该接口的close的方法

注意：

1.  jdk1.7后面的才有

2.  括号里面 创建的是final的对象

3.  要么？括号里面的语句是正常的Java 代码语句，所以多行需要

4.  可以自定义的资源，实现了AutoCloseable接口就行；
