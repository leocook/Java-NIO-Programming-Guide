# Java NIO: Non-blocking Server

DatagramChannel是Java NIO中发送和接收UDP包的channel。因为UDP是无网络连接的协议，所以不能像操作其它channel那样使用read和write方法。一般使用send和receive方法来处理。

## DatagramChannel的open方法

下面代码是怎么打开一个DatagramChannel：

```
DatagramChannel channel = DatagramChannel.open();
channel.socket().bind(new InetSocketAddress(9999));
```

这样就可以创建一个DatagramChannel，用来接收本机的9999端口上的UDP数据包。

## 使用receive方法接收数据

可以调用DatagramChannel对象的receive方法来接收数据：

```
ByteBuffer buf = ByteBuffer.allocate(48);
buf.clear();

channel.receive(buf);
```

receive方法会把接收到的UDP数据包里的数据拷贝到buffer中去。如果接收到的数据包中数据大小大于buffer的容量，多余出来的数据将会被丢掉。

## 使用send方法发送数据

可以调用DatagramChannel对象的send方法来发送数据：

```
String newData = "New String to write to file..."
                    + System.currentTimeMillis();

ByteBuffer buf = ByteBuffer.allocate(48);
buf.clear();
buf.put(newData.getBytes());
buf.flip();

//向www.baidu.com的80端口发送UDP数据包
int bytesSent = channel.send(buf, new InetSocketAddress("www.baidu.com", 80));
```

这个例子实现了向远程端口发送UDP数据，如果该远程主机端口上没有被监听，那么数据发送后将不会产生任何实际效果。当然，数据发送后，你将不会得到数据是否被接收相关的任何消息。

## 连接一个指定的端口

UDP协议是无连接的网络传输协议，如果和远程端口建立一个连接的话，其实并不能建立一个真正的连接，而是锁定DatagramChannel，使其只能向指定的端口读写数据。下面是一个例子：

* “连接”远程端口

```
channel.connect(new InetSocketAddress("www.baidu.com", 80));
```

查看源码可以发现，connect方法里会对指定的地址和端口做check的。当"建立连接后"我们就可以调用read和write方法了，但是执行write方法时无法保证数据一定能发送成功。下面是一些例子：

```
int bytesRead = channel.read(buf);
```

```
int bytesWritten = channel.write(buf);
```



