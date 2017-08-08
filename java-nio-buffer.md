# Java NIO Buffer

Java NIO中Buffer用于和Channel之间进行数据交流。数据将会从channel中被读到buffer中去，同样数据也会从buffer中被写入到channel中去。

Buffer本质上是一块内存，你可以向这块内存写入数据，然后再从这块内存中把数据读出来。该内存块被包装在NIO Buffer对象中，buffer类提供了一系列的方法，便于开发人员操作这块buffer内存。

## Buffer的基础操作

使用NIO Buffer来读或写数据通常需要下面这4个小步骤：

1. 向buffer中写数据
2. 调用buffer.flip\(\)方法
3. 从buffer中读数据
4. 调用buffer.clear\(\)或者buffer.compact方法

当你向buffer中写入数据时，buffer将会记录下你写入了多少数据。当你需要读数据时，你将要调用flip\(\)方法把buffer从writing模式切换到reading模式，

