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

## Files.createDirectory\(\)

把Path对象实例作为Files.createDirectory\(\)的参数，创建了一个新的目录。下面是一个例子：

```
Path path = Paths.get("data/subdir");

try {
    Path newDir = Files.createDirectory(path);
} catch(FileAlreadyExistsException e){
    // the directory already exists.
} catch (IOException e) {
    //something else went wrong
    e.printStackTrace();
}
```

如果目录在就存在了，那么将会抛出`java.nio.file.FileAlreadyExistsException`异常。

## Files.copy\(\)

把文件从一个地方，拷贝到另一个地方，下面是一个例子：

```
Path sourcePath      = Paths.get("data/logging.properties");
Path destinationPath = Paths.get("data/logging-copy.properties");

try {
    Files.copy(sourcePath, destinationPath);
} catch(FileAlreadyExistsException e) {
    //destination file already exists
} catch (IOException e) {
    //something else went wrong
    e.printStackTrace();
}
```

先创建source file的path实例，然后再创建dest file的path实例，最后执行Files.copy方法。如果dest文件已经存在，那么将会抛出`java.nio.file.FileAlreadyExistsException`异常。

### 覆盖已经存在的文件

在复制文件的时候，如果目标文件已经存在了，那么也可以覆盖目标文件，具体看下面例子：

```
Path sourcePath      = Paths.get("data/logging.properties");
Path destinationPath = Paths.get("data/logging-copy.properties");

try {
    Files.copy(sourcePath, destinationPath,
            StandardCopyOption.REPLACE_EXISTING);
} catch(FileAlreadyExistsException e) {
    //destination file already exists
} catch (IOException e) {
    //something else went wrong
    e.printStackTrace();
}
```

其实主要是使用了StandardCopyOption.REPLACE\_EXISTING这个参数。关于StandardCopyOption这个枚举类，还有其它枚举值，具体可以查看API文档。

## Files.move\(\)

Java NIO支持把文件从某个目录移到另外一个目录中去，这里和标准IO中java.io.File类的renameTO\(\)方法比较像。下面是Files.move\(\)方法的一个例子：

```
Path sourcePath      = Paths.get("data/logging-copy.properties");
Path destinationPath = Paths.get("data/subdir/logging-moved.properties");

try {
    Files.move(sourcePath, destinationPath,
            StandardCopyOption.REPLACE_EXISTING);
} catch (IOException e) {
    //moving file failed.
    e.printStackTrace();
}
```

同样的，如果目标文件已经存在，由于配置了参数StandardCopyOption.REPLACE\_EXISTING，所以会把原来存在的文件给替换掉。

## Files.delete\(\)

执行Files.delete\(\)方法可以删除指定的文件或者目录，下面是一个例子：

```
Path path = Paths.get("data/subdir/logging-moved.properties");

try {
    Files.delete(path);
} catch (IOException e) {
    //deleting file failed
    e.printStackTrace();
}
```

## Files.walkFileTree\(\)

Files.walkFileTree\(\)方法用来遍历某个目录下的目录树。walkFileTree\(\)方法把path和FileVisitor作为参数，Path指向需要遍历的目录，在遍历的过程中，将会调用FileVisitor。

