在Java NIO中，两个channel中如果有一个channel是FileChannel，那么就可以把数据从一个channel传输到另外一个channel中去。FileChannel类中有transferTo\(\)和transferFrom\(\)两个方法可以做这个事情。

