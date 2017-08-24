# Java NIO vs. IO

当我们同时学习Java NIO和标准IO的时候，我们可能会有个问题：

> 何时使用NIO？何时使用IO？

本文将会简单的来说明一下Java中NIO和标准IO的区别，以及在使用它们进行代码设计时需要注意哪些事项。

## 主要的区别

下面表格是NIO和IO的对比汇总，后边将对其中的每个点进行详细讨论：

| IO | NIO |
| :--- | :--- |
| 面向流\(stream\)编程 | 面向缓存\(buffer\)编程 |
| 阻塞IO | 非阻塞IO |
| 无 | 支持选择器\(Selectors\) |

## 面向流 VS 面向缓存

首先，Java NIO和标准IO之间最大的一个区别就是标准IO是面向流的，而NIO是面向缓存的。

标准IO是面向流的，意味着你可以从流中读一个或多个字节的数据。数据读取出来之后用来做什么事情，取决于程序实现的逻辑。在从流中读取数据时，不能后退去读取已读过的数据，也不能随机往前跳跃去读取数据。如果想实现流的随机读取，那么就得先把流中的所有数据都先缓存下来。

NIO是面向缓存（buffer）的，与标准IO略有不同。NIO中数据会被读取到缓存中，晚些才会处理。如果需要，可以在buffer中向前、向后移动下标，读取自己想要的数据。大大的提升了NIO操作的伸缩性。然而，为了能够完整的处理buffer中的数据，需要检查一下该buffer中的数据是否是完整的。需要注意，当在往buffer中写入更多的数据时，不要覆盖掉buffer中还未读取出来的数据。

## 阻塞 VS 非阻塞

Java标准IO中很多流都是阻塞的。当对流进行读写操作时，方法将会进入阻塞，只有完成读写的时候方法才会返回。阻塞期间该线程不能做任何其它操作。

Java NIO在非阻塞模式下，允许在线程从一个channel中读数据时，即使读取不到数据，也会立马返回。而不是一直阻塞直到有新的数据到来。期间线程可以去做其它任何想做的事情。

非阻塞写也是如此，线程将会请求把数据写入到channel中去，但是不会等待数据完全被写入后才返回。方法会立马返回，期间该线程可以去做任何想做的事情。

当一个线程一个线程非阻塞调用后，可以立马处理其它channel的数据。因此，单线程可以同时管理多个channel的读和写。

## Selector（选择器）

Java NIO的Selector允许单个线程同时监控多个channel的读写等事件。可以向Selector上注册多个channel，然后用一个线程去执行select方法，监听Channel的事件。在管理多个channel时，使用一个单线程执行Selector，一切都变得简单了。

## NIO和标准IO对应用设计的影响

选择NIO还是IO，在你的应用设计主要有如下几点影响：

1. NIO/IO接口API的调用
2. 处理数据的过程
3. 处理数据多需要的线程数

### API调用

NIO和标准IO的API调用很显然是不一样的。标准IO并不是直接从InputStream中读取数据，而是借助buffer类，先读到buffer中，然后再做数据上的处理。

### 处理数据的过程

选用NIO或者标准IO会影响到处理数据的过程。

在标准IO中，可以使用InputStream或者Reader来读数据。例如处理下面的纯文本数据：

```
Name: Anna
Age: 25
Email: anna@mailserver.com
Phone: 1234567890
```

这个文本数据流被处理如下：

```
InputStream input = ... ; // 获取到流

BufferedReader reader = new BufferedReader(new InputStreamReader(input));

String nameLine   = reader.readLine();
String ageLine    = reader.readLine();
String emailLine  = reader.readLine();
String phoneLine  = reader.readLine();
```

注意，数据处理的状态可由程序执行的状态得知。换句话说，当执行了第一个reader.readLine\(\)时候，你就知道第一行数据已经完全读完了。redLine\(\)方法会一直阻塞，直到一整行的数据都被读完，而且还能判断出这行数据包含`Name`。当执行到第二个readLine\(\)的时候，你会知道已经读到第二行，你会知道这行包含个Age，等等。

当执行的线程执行过了一行代码也就意味着读完了一些数据，该线程不能退回去重新在读那些读过的数据。该模式可以由下面的这幅插图来表示：

![](/assets/12.png)

使用NIO来实现的话，将会有所不同。

```
ByteBuffer buffer = ByteBuffer.allocate(48);
int bytesRead = inChannel.read(buffer);
```

注意第二行代码，把数据从channel读到ByteBuffer中。当read方法返回时，无法知道是不是所有的数据都读到buffer中去了。只知道buffer中有多少数据。这使得处理起来变得复杂很多。

假设一下，在第一次执行了read\(buffer\)方法后，读到buffer中的数据只是一行数据的一半，例如“Name: Ah”，很显然无法处理这个数据，只能等剩下的所有数据都写入到buffer中去之后，才能处理buffer中的数据。

所以，怎么才能确定buffer中的数据足够用来处理呢？很显然，无法确定。唯一的办法就是查看Buffer中的数据，在数据还没有完整写入到buffer中之前你需要多次检查buffer中的数据。这种方式不仅效率低，而且程序设计起来巨复杂：

```
ByteBuffer buffer = ByteBuffer.allocate(48);

int bytesRead = inChannel.read(buffer);

while(! bufferFull(bytesRead) ) {
    bytesRead = inChannel.read(buffer);
}
```

bufferFull方法将会记录读进buffer中去的数据，当buffer完整时返回true，否则返回false。换句话说，当数据可以被处理时返回true，否则返回false。

bufferFull\(\)方法将会扫描buffer，但需要保证在整个扫描过程中buffer的过程中，buffer的状态不能发生变化。否则的话接下来再把数据读到buffer中去时无法保证数据能写到bufferd的正确位置上。

下面是上述过程中的插图：

![](/assets/13.png)

## 总结

NIO允许你通过单个线程来管理多个Channel，但是解析数据将比使用阻塞IO更加复杂。

如果需要同时管理上千个连接，但是每个连接只发送少量的数据，例如聊天系统，在使用NIO来实现server端将会变得很有优势。同样的道理，如果你需要和其它计算机保持着大量的连接，例如P2P网络，使用单个线程来管理你的对外接口将会很有优势。下面是一个线程处理着大量连接的示意图：

![](/assets/14.png)

如果你有少量数据传输很频繁的线程，在同一时间将会发送大量数据，那么使用标准的IO库来实现将会是首选方案。下面是使用标准IO时的示意图：

![](/assets/15.png)



