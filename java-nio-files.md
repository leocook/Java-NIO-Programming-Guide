# Java NIO Files

Java NIO里的`java.nio.file.Files`类提供了一系列操作文件系统的方法。如果在本文没有你想了解的方法，可以去Java的API文档中查看。

java.nio.file.Files类一般和java.nio.file.Path类结合使用的，所以在使用Files类之前，需要先理解Path类。

## File.exists\(\)

Files.exists\(\)方法是用来检查指定的path在操作系统中是否真正的存在。在创建一个path的时候，指定的路径可以是不存在的，例如创建一个目录时，

