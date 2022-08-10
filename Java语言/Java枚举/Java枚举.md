# Java枚举

# 基本 enum 特性

`I18NAudit[] values = I18NAudit.values();`

*   `values()`返回一个实例数组；

*   `ordinal()` 方法返回一个 int 值，这是每个 enum 实例在声明时的次序，从 0 开始。可以使用 == 来比较 enum 实例，编译器会自动为你提供 equals() 和 hashCode() 方法。

*   Enum 类实现了 Comparable 接口，所以它具有 compareTo() 方法。同时，它还实现了Serializable 接口。

*   name()方法返回 enum 实例声明时的名字，这与使用 toString() 方法效果相同。

*   valueOf() 是在 Enum 中定义的 static 方法，它根据给定的名字返回相应的 enum 实例，如果不存在给定名字的实例，将会抛出异常。

*   使用 static import能够将 enum 实例的标识符带入当前的命名空间，所以无需再用 enum 类型来修饰 enum 实例。

# 添加方法

1.  提供一个构造器

    ```java
    SOUTH("Good by inference, but missing");

    private String description;
    // Constructor must be package or private access:
    private OzWitch(String description) {
      this.description = description;
    }
    public String getDescription() { return description; }
    ```

2.  自定义方法
    在 enum 实例序列的最后添加一个分号。在后面可以添加属性和方法，反之报错。

# switch 语句中的 enum
