# Java NIO Selector\(选择器\)

Selector是Java NIO的一个重要组件，用来检查一个或者多个channel，检查channel是否已经准备好读和写。这样就能让一个线程同时管理多个channel了，例如多个网络连接。

## 为什么使用Selector

使用Selector的主要优势是可以使用单线程来处理多个channel，甚至所有channel。多线程间切换时会多余的消耗操作系统的资源，例如内存和cpu。

