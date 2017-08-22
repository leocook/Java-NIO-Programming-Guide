# Java NIO SocketChannel

Java NIO的`SocketChannel`是一个可以连接socket的channel，它可以用来替换标准的Java Socket接口，创建`SocketChannel`有两个方法。

* 创建一个SocketChannel对象，然后连接一个指定的网络地址；
* 借助ServerSocketChannel来创建SocketChannel对象。

## SocketChannel的open\(\)方法

下面是一个例子：

```
SocketChannel socketChannel = SocketChannel.open();
socketChannel.connect(new InetSocketAddress("http://www.baidu.com", 80));
```

## SocketChannel的close\(\)方法

在调用了SocketChannel对象的close方法后，将会关闭SocketChannel，例如：

```
socketChannel.close();
```

## 从SocketChannel中读取数据

可以调用SocketChannel对象的read方法来读取channel中的数据：

```
ByteBuffer buf = ByteBuffer.allocate(48);

int bytesRead = socketChannel.read(buf);
```



