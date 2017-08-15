Java NIO支持scatter和gather，scatter和gather这两个概念是使用在同时把channel中的数据读到多个buffer中，以及同时把多个buffer中的数据写入到一个channel中的场景。

当需要处理的数据在多个buffer中时，就要用scattering\(分散\)和gathering\(聚合\)的方式来操作buffer。

## Scattering Reads

一个“Scattering read”把数据从一个channel中读到多个buffer中去。下面是关于Scatter的一个插图：



