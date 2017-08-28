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

在解释遍历目录的过程之前，先查看一下FileVisitor接口的源码：

```
public interface FileVisitor {

    public FileVisitResult preVisitDirectory(
        Path dir, BasicFileAttributes attrs) throws IOException;

    public FileVisitResult visitFile(
        Path file, BasicFileAttributes attrs) throws IOException;

    public FileVisitResult visitFileFailed(
        Path file, IOException exc) throws IOException;

    public FileVisitResult postVisitDirectory(
        Path dir, IOException exc) throws IOException {

}
```

开发人员需要自己去实现FileVisitor接口，在遍历目录的不同阶段，会调用FileVisitor接口不同的方法，如果你不想去自己实现这些方法，有个默认的SimpleFileVisitor类可以提供使用。下面是一个例子：

```
Files.walkFileTree(path, new FileVisitor<Path>() {
  @Override
  public FileVisitResult preVisitDirectory(Path dir, BasicFileAttributes attrs) throws IOException {
    System.out.println("pre visit dir:" + dir);
    return FileVisitResult.CONTINUE;
  }

  @Override
  public FileVisitResult visitFile(Path file, BasicFileAttributes attrs) throws IOException {
    System.out.println("visit file: " + file);
    return FileVisitResult.CONTINUE;
  }

  @Override
  public FileVisitResult visitFileFailed(Path file, IOException exc) throws IOException {
    System.out.println("visit file failed: " + file);
    return FileVisitResult.CONTINUE;
  }

  @Override
  public FileVisitResult postVisitDirectory(Path dir, IOException exc) throws IOException {
    System.out.println("post visit directory: " + dir);
    return FileVisitResult.CONTINUE;
  }
});
```

在遍历目录的不同阶段会调用FileVisitor接口的不同方法：

在访问每个目录前都会调用preVisitDirectory方法；在访问每个目录后都会调用postVisitDirectory方法。

在访问每个文件期间都会调用visitFile方法，访问目录将不会调用；在访问文件失败后都会调用visitFileFailed方法，例如权限错误等等等。

上面四个方法中，每个方法都返回一个FileVisitResult枚举值，FileVisitResult枚举包含下面四个可选项，返回的这个枚举值，决定了文件遍历过程中将会执行什么操作：

* CONTINUE

文件遍历将会正常进行。

* TERMINATE

结束文件遍历

* SKIP\_SIBLINGS

继续遍历，但是跳过同一级别目录下的其它文件和目录

* SKIP\_SUBTREE

继续遍历，但是该目录下的其它文件和目录

### 查找文件

下面是一个继承了SimpleFileVisitor类，并查找README.txt文件的代码：

```
Path rootPath = Paths.get("data");
String fileToFind = File.separator + "README.txt";

try {
  Files.walkFileTree(rootPath, new SimpleFileVisitor<Path>() {

    @Override
    public FileVisitResult visitFile(Path file, BasicFileAttributes attrs) throws IOException {
      String fileString = file.toAbsolutePath().toString();
      //System.out.println("pathString = " + fileString);

      if(fileString.endsWith(fileToFind)){
        System.out.println("file found at path: " + file.toAbsolutePath());
        return FileVisitResult.TERMINATE;
      }
      return FileVisitResult.CONTINUE;
    }
  });
} catch(IOException e){
    e.printStackTrace();
}
```

### 使用递归删除目录中的内容

File.walkFileTree\(\)方法可以用来递归删除一个目录，并且也删除该目录下的文件、以及子目录。Files.delete\(\)方法只会删除一个空目录。

