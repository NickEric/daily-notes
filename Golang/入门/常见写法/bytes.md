# Bytes



### 问题1

**`bytes.Buffer`类型的值记录的已读计数，在其中起到了怎样的作用？**

`bytes.Buffer`中的已读计数的大致功能如下所示:

* 1）读取内容时，相应方法会依据已读计数找到未读部分，并在读取后更新计数。
* 2）写入内容时，如需扩容，相应方法会根据已读计数实现扩容策略。
* 3）截断内容时，相应方法截掉的是已读计数代表索引之后的未读部分。
* 4）读回退时，相应方法需要用已读计数记录回退点。
* 5) 重置内容时，相应方法会把已读计数置为`0`。
* 6) 导出内容时，相应方法只会导出已读计数代表的索引之后的未读部分。
* 7) 获取长度时，相应方法会依据已读计数和内容容器的长度，计算未读部分的长度并返回。



### 问题2

**`bytes.Buffer`的扩容策略是怎样的？**

`Buffer`值既可以被手动扩容，也可以进行自动扩容。并且，这两种扩容方式的策略是基本一致的。所以，除非我们完全确定后续内容所需的字节数，否则让`Buffer`值自动去扩容就好了。



更具体地说，如果内容容器的容量与其长度的差，大于或等于另需的字节数，那么扩容代码就会通过切片操作对原有的内容容器的长度进行扩充，就像下面这样：

```
b.buf = b.buf[:length+need]
```

反之，如果内容容器的剩余容量不够了，那么扩容代码可能就会用新的内容容器去替代原有的内容容器，从而实现扩容。不过，这里还一步优化。

如果当前内容容器的容量的一半仍然大于或等于其现有长度再加上另需的字节数的和，即：

```
cap(b.buf)/2 >= len(b.buf)+need
```

那么，扩容代码就会复用现有的内容容器，并把容器中的未读内容拷贝到它的头部位置。这也意味着其中的已读内容，将会全部被未读内容和之后的新内容覆盖掉。

这样的复用预计可以至少节省掉一次后续的扩容所带来的内存分配，以及若干字节的拷贝。



### 问题3

**`bytes.Buffer`中的哪些方法可能会造成内容的泄露？**

> 这里所说的内容泄露是指，使用`Buffer`值的一方通过某种非标准的（或者说不正式的）方式得到了本不该得到的内容。

比如说，我通过调用`Buffer`值的某个用于读取内容的方法，得到了一部分未读内容。我应该，也只应该通过这个方法的结果值，拿到在那一时刻`Buffer`值中的未读内容。

但是，在这个`Buffer`值又有了一些新内容之后，我却可以通过当时得到的结果值，直接获得新的内容，而不需要再次调用相应的方法。

这就是典型的非标准读取方式。这种读取方式是不应该存在的，即使存在，我们也不应该使用。因为它是在无意中（或者说一不小心）暴露出来的，其行为很可能是不稳定的。

在`bytes.Buffer`中，`Bytes`方法和`Next`方法都可能会造成内容的泄露。原因在于，它们都把基于内容容器的切片直接返回给了方法的调用方。

我们都知道，通过切片，我们可以直接访问和操纵它的底层数组。不论这个切片是基于某个数组得来的，还是通过对另一个切片做切片操作获得的，都是如此。

在这里，`Bytes`方法和`Next`方法返回的字节切片，都是通过对内容容器做切片操作得到的。也就是说，它们与内容容器共用了同一个底层数组，起码在一段时期之内是这样的。