# 前言

关于BIO、NIO、AIO这块已经解除了很久，一直想能有机会总结一下。Java NIO编程这块，已经有封装很好的Netty了，但是市面上现在很少有对于java原生的NIO API介绍，也没有这方面的书籍，所以我就写了这个系列的文章。

本文主要是基于 `http://tutorials.jenkov.com/java-nio/index.html` 这个系列博文的翻译。其中有些片段直译成中文后阅读起来会很蹩脚，所以加入了一些自己主观的理解，所以会和原著稍有偏差，但偏差不大。

关于计算的IO模型，以及学习思路，要感谢大牛同事的指导，以及一些博主的精彩博文。下面是我学习期间参考的几篇博文：

```
http://tutorials.jenkov.com/java-nio/index.html
http://blog.csdn.net/historyasamirror/article/details/5778378
http://blog.csdn.net/skiof007/article/details/52873421
```

今天情人节，刚好完成这个教程的编写，感谢一下老婆对我的照顾，老婆辛苦了！

关于我个人，小码农一枚，主要从事大数据方向的工作，对算法和数学都有浓厚兴趣。如果你也是小码农，并且对技术充满热情，不妨加个微信吧，成长的路途中又能多一位相互鼓励的小伙伴！

> 个人微信：**leocook**，记得备注：“**Java NIO**”。

![](/assets/weixin.png)

