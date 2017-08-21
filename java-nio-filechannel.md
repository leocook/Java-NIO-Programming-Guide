# Java NIO FileChannel

Java NIO中的FileChannel是一个用来连接文件的channel。使用FileChannel可以从文件中读数据，也可以把数据写入到文件中去。Java NIO中的FileChannel类的相关API可以替换Java标准IO流的API。

> 注意：FileChannel不能设置非阻塞模式，只能一直在阻塞模式下运行。

## 打开一个FileChannel

在使用FileChannel之前，你必须要先打开它。FileChannel不能直接创建，而是要通过输入输出流（例如：InputStream, OutputStream、或者RandomAccessFile）来获取。下面是一个通过RandomAccessFile来获取到RandomAccessFile的例子：

```
RandomAccessFile aFile     = new RandomAccessFile("data/nio-data.txt", "rw");
FileChannel      inChannel = aFile.getChannel();
```

## 从FileChannel中读取数据

可通过调用FileChannel的read方法来读取数据，下面是一个例子：

```
ByteBuffer buf = ByteBuffer.allocate(48);

int bytesRead = inChannel.read(buf);
```

* 第一步：分配Buffer，数据是先从FileChannel中读到Buffer中去的；
* 第二步：调用FileChannel的read方法，把数据读到buffer中去。read方法返回的int整数表示读了多少数据到buffer中去，当该整数为-1时，表示数据已经被读完了。

## 向FileChannel中写入数据

可以调用FileChannel的write\(buffer\)方法来把buffer中的数据写入到FileChannel中去，下面是一个例子：

```
String newData = "New String to write to file..." + System.currentTimeMillis();

ByteBuffer buf = ByteBuffer.allocate(48);
buf.clear();
buf.put(newData.getBytes());

buf.flip();

while(buf.hasRemaining()) {
    channel.write(buf);
}
```

## FileChannel的close方法

当使用完FileChannel后，必须关闭它：

```
channel.close();
```

## FileChannel的Position方法

当向FileChannel读写数据时，其实操作的是Channel的一个指定的position。可以调用position\(\)方法来获取到FileChannel中当前的position位置。

当然，也可以调用FileChannel的position\(long pos\)方法来设置position，下面是一段例子：

```
long pos = channel.position();

channel.position(pos +123);
```

当设置position的值为文件的尾部位置时。若从channel中读数据，则返回-1；若向channel中写入数据，则可以正常写入。

## FileChannel的大小

FileChannel的大小也就是它所连接的文件大小，可以通过调用FileChannel的size\(\)方法来获得。下面是个例子：

```
long fileSize = channel.size();
```

## 截取FileChannel

可以调用方法FileChannel.truncate\(size\)来截取FileChannel。如些：

```
FileChannel newChannel = channel.truncate(1024);
```

这段代码表示截取了channel的前1024个字节数据，并用次来创建一个新的FileChannel。当截取的长度大于原来Channel的长度后，则新的channel长度和原来channel的长度一致。

