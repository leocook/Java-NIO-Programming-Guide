# Java NIO ServerSocketChannel

Java NIO中的ServerSocketChannel会监听TCP端口、处理请求，就想标准Java网络编程中的ServerSocket。ServerSocketChannel类在java.nio.channels包中，下面是一个例子：

```
//打开channel
ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();

//绑定监听端口
serverSocketChannel.socket().bind(new InetSocketAddress(9999));

//轮询，接收请求
while(true){
    SocketChannel socketChannel =
            serverSocketChannel.accept();

    //do something with socketChannel...
}
```

## ServerSocketChannel的open方法

可通过执行ServerSocketChannel.open\(\)方法来打开一个channel，如下面的例子：

```
ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
```

## ServerSocketChannel的close方法

可通过执行ServerSocketChannel.close\(\)方法来关闭channel，如下面的例子：

```
serverSocketChannel.close();
```



