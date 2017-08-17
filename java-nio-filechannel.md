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









在

