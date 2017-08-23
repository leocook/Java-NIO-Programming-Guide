# Java NIO: Non-blocking Server

DatagramChannel是Java NIO中发送和接收UDP包的channel。因为UDP是无网络连接的协议，所以不能像操作其它channel那样使用read和write方法。一般使用send和receive方法来处理。

## DatagramChannel的open方法

下面代码是怎么打开一个DatagramChannel：

```
DatagramChannel channel = DatagramChannel.open();
channel.socket().bind(new InetSocketAddress(9999));
```

这样就可以创建一个DatagramChannel，用来接收本机的9999端口上的UDP数据包。

