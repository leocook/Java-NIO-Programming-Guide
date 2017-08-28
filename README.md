# Java NIO编程指南

标准的Java IO API使用起来比较方便，由于它是同步阻塞的IO，所以在使用时可能会效率地下，我们可以称标准的Java IO API为Java BIO API。自Java1.4开始，Java提供了NIO\(New IO\) API，Java NIO API可以用来替换原来的Java BIO API,例如：`Java IO`和`Java Networking`。Java NIO提供了一套与标准的Java IO完全不同的API。

## Java对BIO/NIO/AIO的支持

### BIO/NIO/AIO概念说明

* BIO

同步并阻塞，BIO中应用程序直接调用操作系统的内核，然后阻塞，直到内核返回数据。Java中标准IO设计实现的就是BIO。在应用系统中，服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，当然可以通过线程池机制改善。JDK1.4之前，主要是基于这种机制来实现IO操作。

* NIO

同步非阻塞，NIO中应用程序在调用内核后，立马返回，然后会使用类似自旋锁的方式不断的请求内核，直到内核返回数据，可见NIO的单词请求开销是大于BIO的。Java中的NIO是从1.4开始加入的。服务器实现模式为一个请求一个线程，即客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到连接有I/O请求时才启动一个线程进行处理。

* AIO\(NIO2\)

异步非阻塞，AIO中应用程序在调用内核后，立马返回，然后应用程序可以去处理其它事情，等内核处理完数据后，会把数据拷贝到用户空间中去供应用程序使用，并向应用程序发出返回数据的信号，应用程序得到信号后，将会使用数据。Java中的AIO是从1.7才开始加入的，由于AIO也就是NIO2，所以AIO的实现都在NIO包中。服务器实现模式为一个有效请求一个线程，客户端的I/O请求都是由OS先完成了再通知服务器应用去启动线程进行处理。

### BIO/NIO/AIO的场景选择

从单个请求对的计算机性能消耗上来排序，我们得到的是：BIO性能消耗 &lt; AIO性能消耗 &lt; NIO性能消耗。那么我们为什么还要使用NIO以及AIO呢？原因是为了“提高服务器的资源利用率”。AIO目前操作系统级别的支持还不是很成熟，所以重点讨论下NIO。

* BIO

在连接数比较小的场景下，推荐使用BIO。比较适合一些重资源的连接，对IO吞吐要求极高的应用。

* NIO

在连接数比较多，且连接都比较短的场景下。适合一些轻资源的连接，例如聊天服务器等等。

* AIO

在连接数比较多，且连接比较长的场景下。对IO吞吐量较高的（比BIO低，比NIO高）要求的场景，例如相册服务器等等。

## Java NIO: Channels and Buffers

在Java BIO API中，开发人员会直接使用`字节流`和`字符流`来进行IO开发，所以有人会说`Java BIO API是面向流的IO`。在Java NIO API中，开发人员将会使用channel和buffer来进行IO开发，数据将会从channel中被读到buffer中去，也会从buffer中被写入到channel中去。

## Java NIO: Non-blocking IO

Java NIO允许开发者使用“Non-blocking IO”\(非阻塞IO\)。举个例子，当一个线程在把数据从channel读到buffer的过程中，该线程还可以去处理其它事情。直到完成数据读到buffer中后，线程才会去处理它。在把数据写入到channle的过程中，同理。

## Java NIO: Selectors

Java NIO中提供了一个概念被称之为“selector”\(选择器\)。一个selector对象可以同时监控多个channel中的事件，这里的事件包括：l连接的打开，接收到数据等等。因此，一个线程可以监控多个channel中的数据。

[http://blog.csdn.net/skiof007/article/details/52873421](http://blog.csdn.net/skiof007/article/details/52873421)

