# Java NIO Buffer

Java NIO中Buffer用于和Channel之间进行数据交流。数据将会从channel中被读到buffer中去，同样数据也会从buffer中被写入到channel中去。

Buffer本质上是一块内存，你可以向这块内存写入数据，然后再从这块内存中把数据读出来。该内存块被包装在NIO Buffer对象中，buffer类提供了一系列的方法，便于开发人员操作这块buffer内存。

## Buffer的基础操作

使用NIO Buffer来读或写数据
