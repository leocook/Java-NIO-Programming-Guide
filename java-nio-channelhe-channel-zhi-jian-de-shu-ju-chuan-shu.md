在Java NIO中，两个channel中如果有一个channel是FileChannel，那么就可以把数据从一个channel传输到另外一个channel中去。FileChannel类中有transferTo\(\)和transferFrom\(\)两个方法可以做这个事情。

## transferFrom\(\)方法

FileChannel.transferFrom\(\)这个方法把数据从一个源source读取到FileChannel中去。下面是一个示例：

```
RandomAccessFile fromFile = new RandomAccessFile("fromFile.txt", "rw");
FileChannel      fromChannel = fromFile.getChannel();

RandomAccessFile toFile = new RandomAccessFile("toFile.txt", "rw");
FileChannel      toChannel = toFile.getChannel();

long position = 0;
long count    = fromChannel.size();

toChannel.transferFrom(fromChannel, position, count);
```

参数position和count是用来表示从目标文件的position位置开始写入，并且最多可写入count这么多数据。

