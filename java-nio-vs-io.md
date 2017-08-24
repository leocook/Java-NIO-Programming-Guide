# Java NIO vs. IO

当我们同时学习Java NIO和标准IO的时候，我们可能会有个问题：

> 何时使用NIO？何时使用IO？

本文将会简单的来说明一下Java中NIO和标准IO的区别，以及在使用它们进行代码设计时需要注意哪些事项。

## 主要的区别

| IO | NIO |
| :--- | :--- |
| 面向流\(stream\)编程 | 面向缓存\(buffer\)编程 |
| 阻塞IO | 非阻塞IO |
|  | 支持选择器\(Selectors\) |



