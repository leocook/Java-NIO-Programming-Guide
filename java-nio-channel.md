# Java NIO Channel

Java NIO的Channel和stream\(流\)比较相似，但是也有些不同之处：

* 你可以同时从Channel中读数据和向Channle中写数据，对于Channle的操作可以双向的，但是对stream的操作通常是单向的，只能使用输入流中读取数据，使用输出流写入数据；
* Channel支持异步读写；而stream不支持异步；
* Channel总是把数据读到Buffer中，或者把buffer的数据写到Channel中去。

上面的这三条说明，可以使用下面的贴图来表示：

![](/assets/import2.png)

## Channel的几种实现

下面是Java NIO中最重要的几个Channel的实现类：

* FileChannel：从文件中读数据，或者把数据写入到文件中去；

* DatagramChannel：通过UDP协议来读取数据，或者把数据以UDP形式广播出去；

* SocketChannel：通过TCP来读写数据；

* ServerSocketChannel：以web server的形式来监听TCP连接，每有一个新的连接都会创建一个SocketChannel。

## Channel的简单实例

下面这个例子实现从FileChannel重读取数据到Buffer中：

```
RandomAccessFile aFile = new RandomAccessFile("data/nio-data.txt", "rw");
    FileChannel inChannel = aFile.getChannel();

    ByteBuffer buf = ByteBuffer.allocate(48);

    int bytesRead = inChannel.read(buf);
    while (bytesRead != -1) {

      System.out.println("Read " + bytesRead);
      buf.flip();

      while(buf.hasRemaining()){
          System.out.print((char) buf.get());
      }

      buf.clear();
      bytesRead = inChannel.read(buf);
    }
    aFile.close();
```

关于buffer的flip\(\)方法，下一篇会重点介绍到。

