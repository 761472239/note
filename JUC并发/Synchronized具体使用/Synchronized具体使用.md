# Synchronized具体使用

参考：[https://blog.csdn.net/import\_sadaharu/article/details/78846675](https://blog.csdn.net/import_sadaharu/article/details/78846675 "https://blog.csdn.net/import_sadaharu/article/details/78846675")

## 作用对象

1.  代码块

2.  方法

3.  静态方法

4.  类

## 修饰类

```java
class ClassName {
   public void method() {
      synchronized(ClassName.class) {
         
      }
   }
}
```

作用对象是这个类的所有对象

## 修饰方法

```java
public synchronized void method()
{
   // todo
}
```

在方法前面加synchronized关键字，作用对象是所有调用这个方法的对象。
【注意】

*   在定义接口方法的时候不能使用synchronized关键字。

*   构造方法不能使用synchronized关键字，但可以使用synchronized代码块来进行同步。

*   synchronized关键字不能被继承。如果子类覆盖了父类的 被 synchronized 关键字修饰的方法，那么子类的该方法只要没有 synchronized 关键字，那么就默认没有同步，也就是说，不能继承父类的 synchronized。

## 修饰静态方法

```java
public synchronized static void method() {
   // todo
}
```

静态方法是属于类的， synchronized修饰的静态方法锁定的是这个类的所有对象

## 修饰代码块

1.  当两个并发线程访问同一个对象object中的这个synchronized(this)同步代码块时，一个时间内只能有一个线程得到执行。另一个线程必须等待当前线程执行完这个代码块以后才能执行该代码块。

2.  当一个线程访问object的一个synchronized(this)同步代码块时，另一个线程仍然可以访问该object中的非synchronized(this)同步代码块。

3.  尤其关键的是，当一个线程访问object的一个synchronized(this)同步代码块时，其他线程对object中所有其它synchronized(this)同步代码块的访问将被阻塞。

4.  第三个例子同样适用其它同步代码块。也就是说，当一个线程访问object的一个synchronized(this)同步代码块时，它就获得了这个object的对象锁。结果，其它线程对该object对象所有同步代码部分的访问都被暂时阻塞。

5.  以上规则对其它对象锁同样适用

​

## ps.

静态锁会和类锁互斥，和对象锁不会影响

```java
public ClassA {
    public synchronized static void staticMethod() {
    // todo
}

public synchronized void method() {
    // todo
}

public void method2() {
synchronized(ClassA.class) {
            // todo
        }
    }
}

ClassA obj = new ClassA();
```
