# Java NIO FileChannel

Java NIO中的FileChannel是一个用来连接文件的channel。使用FileChannel可以从文件中读数据，也可以把数据写入到文件中去。Java NIO中的FileChannel类的相关API可以替换Java标准IO流的API。

> 注意：FileChannel不能设置非阻塞模式，只能一直在阻塞模式下运行。

## 打开一个FileChannel

在使用FileChannel之前，你必须要先打开它。FileChannel不能直接创建，而是要通过输入输出流（例如：InputStream, OutputStream、或者RandomAccessFile）来创建，例如

