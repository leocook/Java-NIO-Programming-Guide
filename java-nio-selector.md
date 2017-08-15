# Java NIO Selector\(选择器\)

Selector是Java NIO的一个重要组件，用来检查一个或者多个channel，检查channel是否已经准备好读和写。这样就能让一个线程同时管理多个channel了，例如多个网络连接。

## 为什么使用Selector

使用Selector的主要优势是可以使用单线程来处理多个channel，甚至所有channel。多线程间切换时会多余的消耗操作系统的资源，例如内存和cpu。所以使用较少的线程，系统的整体性能会表现得更好。

下面就是一个线程操作三个channel的示意图：

![](/assets/1.png)

## 创建Selector

直接调用静态方法open\(\)就可以创建一个Selector：

```
Selector selector = Selector.open();
```

## 向Selector注册Channel

为了能让Channel和Selector一起使用，需要向Selector注册Channel，可以通过调用类似`SelectableChannel.register()`的方法来实现，例如下面：

```
channel.configureBlocking(false);
SelectionKey key = channel.register(selector, SelectionKey.OP_READ);
```

当使用Selector时，Channel必须要是非阻塞模式。因为FileChannel不能切换到非阻塞模式，所以FileChannel不能和Selector一起使用。

注意register\(\)方法的第二个参数，

