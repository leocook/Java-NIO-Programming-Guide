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

```
CharBuffer buf = CharBuffer.allocate(1024);
```

## 把数据写入Buffer

有两个方法可以把数据写入buffer：

* 把数据从Channel中直接写入到buffer中；

```
int bytesRead = inChannel.read(buf); //read into buffer.
```

* 开发者自己编码把数据写入到Buffer中，例如调用buffer的put方法。

```
buf.put(127);
```

put方法有多个版本，意味着允许你使用多种方式把数据写入到buffer中。例如：把数据写入到buffer指定位置\(positions\)，或者一次写入一个字节数组。具体可见JavaDoc。

## flip\(\)方法

flip\(\)方法会把Buffer从writing模式切换到reading模式。调用了flip\(\)方法后，position将会被设置为0，limit将会被设置为position之前的值。

换言之，执行了flip\(\)方法后，position将标记读的位置，limit标记读取写入的数据时，最多可以读到的位置。

## 从Buffer中读数据

有两个方法可以从buffer中读取数据：

* 把数据直接读到channel中

```
//read from buffer into channel.
int bytesWritten = inChannel.write(buf);
```

* 开发人员自己编码，可以调用get\(\)方法从buffer中读数据

```
byte aByte = buf.get();
```

当然，get也有多个版本可以调用。具体可以查看JavaDoc。

## rewind\(\)方法

调用Buffer.rewind\(\)方法可以把position的值设置回0，然后重新从buffer中开始读取数据。

## clear\(\)和compact\(\)

当从Buffer中读完数据后，需要再次把Buffer标识为writing状态，以便于可以向buffer写入新的数据。执行clear\(\)和compact\(\)方法都可以达到这个目的。

* clear\(\)方法

如果执行了clear\(\)方法，position将会设置为0，limit将会被设置为capacity值。换言之，buffer将会被清空。但是buffer中的数据不会被清除掉。只是标识position从0到capacity的位置都可以写入数据。

如果之前有数据没有被读过，那么调用了clear\(\)方法后，这些数据都会被丢失掉，因为没有标识哪些数据是被读过的，哪些数据是没有被读过的。

* compact\(\)方法

如果在buffer中存在一些还没有被读过的数据，开发者希望稍后再来读，在读之前会向buffer中写一些数据，那么就需要调用compact\(\)方法，而不是clear\(\)方法。

执行compact\(\)方法时，会把所有未读过的数据拷贝到buffer的开始。然后把position设置为未读数据的下一个位置。

limit将会被设置为capacity值，这里和clear\(\)方法一样。

现在可以向Buffer中写数据了，但不会覆盖之前没有读取的数据。

## mark\(\)和reset\(\)

开发可以通过执行方法Buffer.mark\(\)来标记Buffer中的某个指定的position位置。稍后可以执行Buffer.reset\(\)方法来设置position返回到之前标记的position位置。下面是一个实例：

```
buffer.mark();

//call buffer.get(), and do something else.

buffer.reset();  //set position back to mark.
```

## equals\(\)和compareTo\(\)方法

当比较两个buffer是否相等时，可以使用equals\(\)或者compareTo\(\)方法。

### equals\(\)方法

equals方法主要是比较两个buffer是否相等。当buffer满足下面条件时，equals\(\)方法将会返回true:

* buffer类型相同，例如：byte、char以及int等等；
* 在buffer中存放的数据量相同；

* buffer中存放的数据是相同的。

简言之：类型相同、存放的数据相同。

### compareTo\(\)

compareTo方法主要是基于字典顺序来比较两个buffer的大小。假设有A、B两个buffer，当满足下面任意一个条件时，表示A buffer是小于B buffer的：

* position从0开始到limit，当出现position位置上，A的元素小于B的元素时，A buffer小于B buffer；

* A和B两个Buffer里的数据相同，但是A的长度比B的长度短。

## 总结

Buffer在Java NIO扮演者重要的角色，甚至可以理解Java NIO API就是面向Buffer编程的。

* Buffer存在reading和writing两个状态，调用flip\(\)方法可以实现writing ---&gt; reading状态的转换；调用clear\(\)或者compact\(\)方法可以实现 reading ---&gt; writing 状态的转换



