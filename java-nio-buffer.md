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

当buffer的模式（reading/writing）不同时，position和limit的意义会有所不同。但无论buffer在什么模式下，capacity的意义都是一样的。

### Capacity

Buffer的本质是一块确定大小的内存块，“capacity”指的就是这个内存块的大小。你可以向buffer中写入capacity字节的数据，当buffer写满后，如果还要向里面写数据，则需要清空（读取玩里边的数据，或者清空）buffer里的数据。

### Position

在writing模式下，position将会从0开始，每向内存中写入一个字节的数据，buffer的指针将会向后移动一个位置，也就是`position`将会加1。`position`的最大值是`capacity-1`.

在reading模式下，开发者也可以从一个指定的position位置开始读取数据。当执行了flip\(\)方法后，buffer从writing模式切换到reading模式后，position将会被重新设置为0。当读取了第position字节的数据后，position将会移动指向下一个位置，并继续准备去读取下一个字节的数据。

### Limit

在writing模式下，limit表示最多可以向buffer中写入多少数据。writing模式下，它的值等于capacity。

在reading模式下，limit表示你可以读取的数据量。所以，当buffer从writing模式切换到reading模式时，reading模式开始时的limit值就等于writing模式结束时的position。

Capacity、position以及Limit都可以理解是buffer的字节位置，在buffer从writing模式切换到reading模式时，reading模式开始时的limit值就等于writing模式结束时的position值，reading模式开始时position值默认是0，当然也可以由开发人员自己去指定这个position。

## Buffer类型

Java NIO有下面这些类型的Buffer：

* ByteBuffer
* MappedByteBuffer
* CharBuffer
* DoubleBuffer
* FloatBuffer
* IntBuffer
* LongBuffer
* ShortBuffer

可以看出来，每种缓存都对应一种数据类型，开发者可以使用缓存来存储不同类型的数据。

MappedByteBuffer稍微有点特别，它可以定义内存和文件的映射，当你操作这个内存的buffer时，其实操作的就是映射的那个文件。

## 创建Buffer

如果想得到一个Buffer对象，就得首先来创建一个buffer对象。每个buffer类都有一个`allocate()`方法用来创建buffer对象。下面是一个创建了48个字节的ByteBuffer实例：

```
ByteBuffer buf = ByteBuffer.allocate(48);
```

下面是一个创建了1024个字节的CharBuffer实例：

