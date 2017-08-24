# Java NIO Pipe

Java NIO Pipe是两个线程之间数据单向传输的连接管道。一个Pipe中可以有一个source channel和一个sink channel。一个线程把数据写到sink channel中，另一个线程从source channel中把它读出来。

下面是一个插图：

![](/assets/11.png)

## 创建一个Pipe

可以通过调用Pipe类的静态方法open来创建一个Pipe：

```
Pipe pipe = Pipe.open();
```

## 把数据写入Pipe



