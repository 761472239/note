# ArrayList分配空间

如果已经知道或者估计出数组可能存储的元素数量，就可以**在填充数组前**调用`ensureCapacity()`方法。

![](image/image_4OZeoB3AEC.png)

**注意：虽然可以指定容量，但是也只是指定了初始化的容量；如果list的容量满了，还是会继续扩容的**
