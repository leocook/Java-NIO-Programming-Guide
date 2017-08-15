Java NIO支持scatter和gather，scatter和gather这两个概念是使用在同时把channel中的数据读到多个buffer中，以及同时把多个buffer中的数据写入到一个channel中的场景。

当需要处理的数据在多个buffer中时，就要用scattering\(分散\)和gathering\(聚合\)的方式来操作buffer。

## Scattering Reads

“Scattering read”是把数据从一个channel中读到多个buffer中去。下面是关于Scatter的一个插图：

![](/assets/impo1rt.png)

下面是一个scattering read的例子：

```
ByteBuffer header = ByteBuffer.allocate(128);
ByteBuffer body   = ByteBuffer.allocate(1024);

ByteBuffer[] bufferArray = { header, body };

channel.read(bufferArray);
```

> 注意：这种模式下，会先读数据到其中一个buffer中，直到该buffer被写满，再向下一个buffer中写数据。

## Gathering Writes

"gathering write"是把多个buffer的数据写入到一个channel中去。对应的示意图：

![](/assets/im11111port.png)

下面是一个gathering write的例子：

```
ByteBuffer header = ByteBuffer.allocate(128);
ByteBuffer body   = ByteBuffer.allocate(1024);

//write data into buffers

ByteBuffer[] bufferArray = { header, body };

channel.write(bufferArray);
```

write\(\)方法将会按照buffer在数组中的位置以此把数据写入到channel中去，只有buffer中position到limit位置之间的数据才会被写入到channel中去。

这种

