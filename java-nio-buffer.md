# Java NIO Buffer

Java NIO中Buffer用于和Channel之间进行数据交流。数据将会从channel中被读到buffer中去，同样数据也会从buffer中被写入到channel中去。

Buffer本质上是一块内存，你可以向这块内存写入数据，然后再从这块内存中把数据读出来。该内存块被包装在NIO Buffer对象中，buffer类提供了一系列的方法，便于开发人员操作这块buffer内存。

## Buffer的基础操作

使用NIO Buffer来读或写数据通常需要下面这4个小步骤：

1. 向buffer中写数据
2. 调用buffer.flip\(\)方法
3. 从buffer中读数据
4. 调用buffer.clear\(\)或者buffer.compact方法

当你向buffer中写入数据时，buffer将会记录下你写入了多少数据。当你需要读数据时，你将要调用flip\(\)方法把buffer从writing模式切换到reading模式。在reading模式下，你可以把写入buffer中的数据全部都读取出来。

一旦读完所有的数据，你就需要清空所有的buffer，使其能够再次被写入数据。有两个方法可以清空buffer：调用clear\(\)方法或者compact\(\)方法。clear\(\)方法将会清空buffer中的所有数据；compact\(\)方法只会清空你已经读取过的数据，未被读取的数据将会移动到buffer的开始位置，新写入的数据将会被写到这部分未被读取的数据的后边。

下面是一个关于buffer使用的简单案例：

```
RandomAccessFile aFile = new RandomAccessFile("data/nio-data.txt", "rw");
FileChannel inChannel = aFile.getChannel();

//create buffer with capacity of 48 bytes
ByteBuffer buf = ByteBuffer.allocate(48);

int bytesRead = inChannel.read(buf); //read into buffer.
while (bytesRead != -1) {

  buf.flip();  //make buffer ready for read

  while(buf.hasRemaining()){
      System.out.print((char) buf.get()); // read 1 byte at a time
  }

  buf.clear(); //make buffer ready for writing
  bytesRead = inChannel.read(buf);
}
aFile.close();
```

## Buffer的capacity（内存大小），指针（Position）以及限制（Limit）

Buffer的本质就是一块内存，你可以向这块内存写入一些数据，然后再从这块内存中把这些数据读出来。该内存块被包装在NIO Buffer对象中，buffer类提供了一系列的方法，便于开发人员操作这块buffer内存。

为了能够理解Buffer的工作原理，需要了解它的三个属性：

* capacity
* position
* limit

当buffer的模式（reading/writing）不同时，position和limit的意义会有所不同。

* 


