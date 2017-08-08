# Java NIO概述

Java NIO包括下面三个核心组件：

* Channel
* Buffers
* Selectors

除了上面这三个，Java NIO提供了多个类和组件，但Channel、Buffer和Selector这三个属于核心组件，其它的例如Pipe和FileLock都是用来连接这三大核心组件的工具类。

## Channels and Buffers

通常来说，NIO中的数据都是从Channel开始的。Channel和Stream比较像，数据可以从Channel中被读到Buffer中去，同样数据也可以从Buffer中被写入到Channel中去。

