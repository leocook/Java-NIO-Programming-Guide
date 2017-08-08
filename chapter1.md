# Java NIO概述

Java NIO包括下面三个核心组件：

* Channel
* Buffers
* Selectors

除了上面这三个，Java NIO提供了多个类和组件，但Channel、Buffer和Selector这三个属于核心组件，其它的例如Pipe和FileLock都是用来连接这三大核心组件的工具类。

## Channels and Buffers

通常来说，NIO中的数据都是从Channel开始的。Channel和Stream比较像，数据可以从Channel中被读到Buffer中去，同样数据也可以从Buffer中被写入到Channel中去。例如下图：

![](/assets/import.png)

Channel和Buffer都是有多个的，下面列表是Java NIO中实现的几个主要的Channel：

* FileChannel：File System IO
* DatagramChannel：UDP
* SocketChannel：TCP
* ServerSocketChannel：TCP

这里可以看到，这些channel包含了UDP、TCP以及文件IO。

下面列表是Java NIO中实现的一些比较重要的Buffer：

* ByteBuffer
* CharBuffer
* DoubleBuffer
* FloatBuffer
* IntBuffer
* LongBuffer
* ShortBuffer

从字面上就很好理解，这些Buffer是用来传输基础类型数据的，例如：byte、short、int、long、float、double以及character等。

Java NIO中还有一个`MappedByteBuffer`，用来作为内存和文件的映射。

## Selectors

Selector允许一个线程同时处理多个Channel，当你的应用程序中有大量的连接，但是每个连接传输的数据量都很小时，这种场景比较适合使用selector，例如聊天服务器。

下图是一个Selector处理三个Channel时的架构图：

![](/assets/import1.png)

你需要把Channel注册到你所使用的Selector上，然后你需要调用select\(\)方法，该方法将会阻塞，直到注册在该Selector上的某个Channel中存在可被处理的事件，当

&lt;br /&gt;

&lt;br /&gt;

&lt;br /&gt;

&lt;br /&gt;

&lt;br /&gt;

&lt;br /&gt;

