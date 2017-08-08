# Java NIO编程指南

标准的Java IO API使用起来比较方便，由于它是同步阻塞的IO，所以在使用时可能会效率地下，我们可以称标准的Java IO API为Java BIO API。自Java1.4开始，Java提供了NIO\(New IO\) API，Java NIO API可以用来替换原来的Java BIO API,例如：`Java IO`和`Java Networking`。Java NIO提供了一套与标准的Java IO完全不同的API。

## Java NIO: Channels and Buffers

在Java BIO API中，开发人员会直接使用`字节流`和`字符流`来进行IO开发，所以有人会说`Java BIO API是面向流的IO`。在Java NIO API中，开发人员将会使用channel和buffer来进行IO开发，数据将会从channel中被读到buffer中去，也会从buffer中被写入到channel中去。

## Java NIO: Non-blocking IO

Java NIO允许开发者使用“Non-blocking IO”\(非阻塞IO\)。举个例子，当一个线程在把数据从channel读到buffer的过程中，该线程还可以去处理其它事情。直到完成数据读到buffer中后，线程才会去处理它。在把数据写入到channle的过程中，同理。

## Java NIO: Selectors

Java NIO中提供了一个概念被称之为“selector”\(选择器\)。一个selector对象可以同时监控多个channel中的事件，这里的事件包括：l连接的打开，ji接收到数据等等。因此，一个线程可以监控多个channel中的数据。

