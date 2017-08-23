# Java NIO: Non-blocking Server

DatagramChannel是Java NIO中发送和接收UDP包的channel。因为UDP是无网络连接的协议，所以不能像操作其它channel那样使用read和write方法。一般使用send和receive方法来处理。

## DatagramChannel的open方法





