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

