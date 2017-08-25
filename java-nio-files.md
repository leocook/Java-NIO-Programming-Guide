# Java NIO Files

Java NIO里的`java.nio.file.Files`类提供了一系列操作文件系统的方法。如果在本文没有你想了解的方法，可以去Java的API文档中查看。

java.nio.file.Files类一般和java.nio.file.Path类结合使用的，所以在使用Files类之前，需要先理解Path类。

## File.exists\(\)

Files.exists\(\)方法是用来检查指定的path在操作系统中是否真正的存在。在创建一个path的时候，指定的路径可以是不存在的，例如创建一个目录时。

所以在使用path对象前，可以先使用File.exists\(\)来check一下该path是否存在。下面是一个例子：

```
Path path = Paths.get("data/logging.properties");

boolean pathExists =
        Files.exists(path,
            new LinkOption[]{ LinkOption.NOFOLLOW_LINKS});
```

这里需要注意一下方法Files.exists的第二个参数，该参数是可选的。LinkOption.NOFOLLOW\_LINKS在这里的意义是指忽略符号链接的文件（超链接）。

