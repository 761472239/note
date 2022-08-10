# 2、Lock接口

## 目录

*   [2.1 Synchronized](#21-synchronized)

    *   [2.1.1 Synchronized作用范围](#211-synchronized作用范围)

    *   [2.1.2 Synchronized实现卖票的例子](#212-synchronized实现卖票的例子)

*   [2.2 什么是Lock](#22-什么是lock)

    *   [2.2.1 Lock接口](#221-lock接口)

    *   [2.2.2 Lock实现可重入锁](#222-lock实现可重入锁)

    *   [2.2.3 创建线程的多种方式](#223-创建线程的多种方式)

    *   [2.2.4 使用Lock实现卖票的例子](#224-使用lock实现卖票的例子)

*   [2.3 ReentrantLock](#23-reentrantlock)

*   [2.4 ReadWriteLock](#24-readwritelock)

*   [2.5 小结](#25-小结)

## 2.1 Synchronized

synchronized是Java的关键字，是一种同步锁

### 2.1.1 Synchronized作用范围

1.  代码块，被修饰的代码成为同步语句块，作用范围是“{ }”中的代码，**作用的对象是调用这个代码块的对象**。

2.  方法，被修饰的方法成为**同步方法**，其作用范围是整个方法，**作用的对象是调用这个方法的对象**。

3.  静态方法，其作用的范围是整个方法，**作用的对象是这个类的所有对象**。

4.  类，其作用的范围是**synchronized**后面括号括起来的部分，**作用的对象是这个类的所有对象**。

### 2.1.2 Synchronized实现卖票的例子

> 三个售票员，卖出30张票

多线程编程步骤：

1.  创建资源类，创建属性和操作方法

2.  创建多线程调用资源类的方法

【代码实现】

```java
class Ticket {
    int number = 100;

    public synchronized void sale() {
        if (number > 0) {
            System.out.println(Thread.currentThread().getName() + " 卖出：" + number-- + " 剩下：" + number);
        }
    }
}

public class SaleTicket {
    public static void main(String[] args) {
        Ticket ticket = new Ticket();
        new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 40; i++) {
                    ticket.sale();
                }
            }
        }).start();
        new Thread(() -> {
            for (int i = 0; i < 40; i++) {
                ticket.sale();
            }
        }).start();
        new Thread(() -> {
            for (int i = 0; i < 40; i++) {
                ticket.sale();
            }
        }).start();

    }
}

```

如果一个代码块被 synchronized 修饰了，当一个线程获取了对应的锁，并执行该代码块时，其他线程便只能一直等待，等待获取锁的线程释放锁，而这里获取锁的线程释放锁只会有两种情况：

1）获取锁的线程执行完了该代码块，然后线程释放对锁的占有；

2）线程执行发生异常，此时 JVM 会让线程自动释放锁；

那么如果这个获取锁的线程由于要等待 IO 或者其他原因（比如调用 sleep方法）被阻塞了，但是又没有释放锁，其他线程便只能干巴巴地等待，试想一下，这多么影响程序执行效率。

因此就需要有一种机制可以不让等待的线程一直无期限地等待下去（比如只等待一定的时间或者能够响应中断），通过 Lock 就可以手动释放锁。

## 2.2 什么是Lock

Lock 锁实现提供了比使用同步方法和语句可以获得的更广泛的锁操作。它们允许更灵活的结构，可能具有非常不同的属性，并且可能支持多个关联的条件对象。Lock 提供了比 synchronized 更多的功能。
**【Lock 与的 Synchronized 区别】**

*   Lock不是Java语言内置的，synchronized是Java语言的关键字，因此是内置的。Lock是一个类，通过这个类可以实现同步访问。

*   使用synchronized不需要用户手动释放锁，当 synchronized 方法或者 synchronized 代码块执行完之后，系统会自动让线程释放对锁的占用；而 Lock 则必须要用户去手动释放锁，如果没有主动释放锁，就有可能导致出现死锁现象

### 2.2.1 Lock接口

```java
public interface Lock {
    void lock();
    void lockInterruptibly() throws InterruptedException;
    boolean tryLock();
    boolean tryLock(long time, TimeUnit unit) throws InterruptedException;
    void unlock();
    Condition newCondition();
}
```

lock()方法是平常使用得最多的一个方法，就是用来获取锁。如果锁已被其他线程获取，则进行等待。
采用 Lock，必须主动去释放锁，并且在发生异常时，不会自动释放锁。因此一般来说，使用 Lock 必须在 `try{}catch{}`块中进行，并且将释放锁的操作放在finally 块中进行，以保证锁一定被被释放，防止死锁的发生。通常使用 Lock来进行同步的话，是以下面这种形式去使用的：

```java
//创建线程
new Thread(()->{
    try {
        //上锁
        lock.lock();
        System.out.println(Thread.currentThread().getName()+" 外层");

        try {
            //上锁
            lock.lock();
            System.out.println(Thread.currentThread().getName()+" 内层");
        }finally {
            //释放锁
            lock.unlock();
        }
    }finally {
        //释放锁
        lock.unlock();
    }
},"t1").start();
```

### 2.2.2 Lock实现可重入锁

**【可重入锁】**：可以重新进入的锁

```java

public class ReentrantLock
      extends
      Object
      implements 
      Lock, 
      Serializable
```

一个可重入的互斥锁`Lock`

### 2.2.3 创建线程的多种方式

关键字 synchronized 与 wait()/notify()这两个方法一起使用可以实现等待/通知模式， Lock 锁的 newContition()方法返回 Condition 对象，Condition 类也可以实现等待/通知模式。
用 notify()通知时，JVM 会随机唤醒某个等待的线程， 使用 Condition 类可以进行选择性通知， Condition 比较常用的两个方法：
1\. await()会使当前线程等待,同时会释放锁,当其他线程调用 signal()时,线程会重
&#x20;    新获得锁并继续执行。
2\. signal()用于唤醒一个等待的线程。

**注意：**
在调用 Condition 的 `await()/signal()`方法前，也需要线程持有相关的 Lock 锁，调用 `await()`后线程会释放这个锁，在 `singal()`调用后会从当前Condition 对象的等待队列中，唤醒 一个线程，唤醒的线程尝试获得锁， 一旦获得锁成功就继续执行。

### 2.2.4 使用Lock实现卖票的例子

## 2.3 ReentrantLock

ReentrantLock 是唯一实现了 Lock 接口的类，并且 ReentrantLock 提供了更多的方法。

```java
public void insert(Thread thread) {
    Lock lock = new ReentrantLock(); //注意这个地方
    lock.lock();
    try {
        System.out.println(thread.getName() + "得到了锁");
        for (int i = 0; i < 5; i++) {
            arrayList.add(i);
        }
    } catch(Exception e) {
        // TODO: handle exception
    } finally {
        System.out.println(thread.getName() + "释放了锁");
        lock.unlock();
    }
}
```

## 2.4 ReadWriteLock

## 2.5 小结

Lock和Synchronized的区别

1.  Lock是一个接口，Synchronized是Java语言内置关键字。

2.  Synchronized发生异常的时候会自动释放线程占用的锁，因此不会发生死锁的情况。
    Lock发生异常时候，需要在try、catch、finally块中手动释放锁。

3.  Lock可以让等待锁的线程响应中断，Synchronized不可以让等待锁的线程中断，而是会一直等待下去，极大地影响效率。

4.  通过Lock可以知道有没有成功获取锁，Synchronized不行。

5.  Lock可以提高多个线程进行读操作的效率。

在性能上来说，如果竞争资源不激烈，两者的性能是差不多的，而当竞争资源非常激烈时（即有大量线程同时竞争），此时 Lock 的性能要远远优于synchronized。
