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
//分配buffer
ByteBuffer buf = ByteBuffer.allocate(48);

//把数据读到buffer中
//bytesRead表示读取到的数据字节数，-1表示没有读到数据
int bytesRead = socketChannel.read(buf);
```

## 把数据写入到SocketChannel中去

可以调用SocketChannel的write方法，把buffer作为参数，然后把数据写入，例如：

```
String newData = "New String to write to file..." + System.currentTimeMillis();

ByteBuffer buf = ByteBuffer.allocate(48);
buf.clear();
buf.put(newData.getBytes());

buf.flip();

while(/*判断buffer中是否还有数据*/buf.hasRemaining()) {
    channel.write(buf);//把buffer中数据写入channel
}
```

## 非阻塞模式

当我们可以设置SocketChannel为非阻塞模式时，connect\(\)，read\(\)以及write\(\)这些方法将会以异步的方式来执行。

### connect\(\)

SocketChannel在非阻塞模式下时执行了connect\(\)方法，可能会在连接还没有创建完成时connect\(\)方法就已经return了。那么在使用connect前就需要验证connect是否已经创建完成，可以通过调用finishConnect\(\)方法来完成验证，例如：

```
socketChannel.configureBlocking(false);
socketChannel.connect(new InetSocketAddress("http://www.baidu.com", 80));

while(! socketChannel.finishConnect() ){
    //wait, or do something else...    
}
```



