# Java NIO Pipe

Java NIO Pipe是两个线程之间数据单向传输的连接管道。一个Pipe中可以有一个source channel和一个sink channel。一个线程把数据写到sink channel中，另一个线程从source channel中把它读出来。

下面是一个插图：

![](/assets/11.png)

## 创建一个Pipe

可以通过调用Pipe类的静态方法open来创建一个Pipe：

```
Pipe pipe = Pipe.open();
```

## 把数据写入Pipe

可以使用SinkChannel来向Pipe中写入数据，下面是一个例子：

```
获取到SinkChannel对象
Pipe.SinkChannel sinkChannel = pipe.sink();
```

可以调用SinkChannel的write方法来向SinkChannel中写入数据：

```
String newData = "New String to write to file..." + System.currentTimeMillis();

ByteBuffer buf = ByteBuffer.allocate(48);
buf.clear();
buf.put(newData.getBytes());

buf.flip();

while(buf.hasRemaining()) {
    sinkChannel.write(buf);
}
```

## 从Pipe中读取数据

可以使用SourceChannel来读取Pipe中的数据：

```
Pipe.SourceChannel sourceChannel = pipe.source();
```

然后调用SourceChannel的read方法来读取数据：

```
ByteBuffer buf = ByteBuffer.allocate(48);

int bytesRead = inChannel.read(buf);
```

返回的整数bytesRead表示读出来并写入buffer中的数据量。

