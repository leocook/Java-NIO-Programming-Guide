# Java NIO Channel

Java NIO的Channel和stream\(流\)比较相似，但是也有些不同之处：

* 你可以同时从Channel中读数据和向Channle中写数据，对于Channle的操作可以双向的，但是对stream的操作通常是单向的，只能使用输入流中读取数据，使用输出流写入数据；
* Channel支持异步读写；而stream不支持异步；
* * 

