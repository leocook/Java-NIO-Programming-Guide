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

注意register\(\)方法的第二个参数，该参数表示Selector监听Channel的事件，只有触发指定的Channel事件时，Selector才会去处理该Channel的数据。可以监听四种不同的事件：

* 1.Connect
* 2.Accept
* 3.Read
* 4.Write

当Channel触发了某个事件时，也就意味着它已经准备好处理该事件了。当channel触发了connect事件后，意味着已经连接成功；当触发了accept事件后，意味着一个server socket已经成功的接收了客户端的请求；当channel触发了read事件后，意味着channel已经准备好被读取数据了；当channel触发了write事件后，意味着channel已经准备好被写入数据了。

这四种事件，对应了SelectionKey类中的四个常量：

* 1.SelectionKey.OP\_CONNECT
* 2.SelectionKey.OP\_ACCEPT
* 3.SelectionKey.OP\_READ
* 4.SelectionKey.OP\_WRITE



