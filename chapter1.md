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

Java NIO中还有一个`MappedByteBuffer`，用来

&lt;br /&gt;

&lt;br /&gt;

&lt;br /&gt;

&lt;br /&gt;

&lt;br /&gt;

&lt;br /&gt;



