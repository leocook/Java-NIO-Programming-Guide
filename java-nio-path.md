Java6和Java7实现了一些NIO2的特性，Path接口就是其中的一个。Path接口是在Java7才被加入到Java NIO中的。Path接口在java.nio.file这个包下，所以全路径为`java.nio.file.Path`。

Java的一个Path对象实例相当于系统中的一个路径。

* Path对象可以指向一个文件也可以指向一个目录；
* 这个Path可以是相对路径，也可以是绝对路径

在很多方面，`java.nio.file.Path`接口和java.io.File类有些相似，但是也有一些微小的区别。在很多场景，都可以使用File类来替换Path接口使用的。

## 创建一个Path实例

为了能够使用java.nio.file.Path对象实例，你必须创建一个Path对象。可以使用java.nio.file.Paths类的静态的方法get来创建一个Path对象的实例。下面是一个例子：

```
import java.nio.file.Path;
import java.nio.file.Paths;

public class PathExample {

    public static void main(String[] args) {

        Path path = Paths.get("c:\\data\\myfile.txt");

    }
}
```

注意一下，这里import导入的两个类，是在java.nio.file包下的。

### 创建一个绝对路径的path

在调用Paths.get\(\)方法来创建path时，可以传入一个绝对路径，来创建一个绝对路径的path:

```
Path path = Paths.get("c:\\data\\myfile.txt"); //windows下
Path path = Paths.get("/home/jakobjenkov/myfile.txt"); //linux下
```

当在windows下使用`/home/jakobjenkov/myfile.txt`来创建path时，那么实际上指向的文件路径是`C:/home/jakobjenkov/myfile.txt`。

### 创建一个相对路径

相对路径是什么应该不用过多的解释了，下面直接看段实例吧：

```
//对应的是"d:\data\projects"目录
Path projects = Paths.get("d:\\data", "projects");

//对应的是"d:\data\projects\a-project\myfile.txt"目录
Path file = Paths.get("d:\\data", "projects\\a-project\\myfile.txt");
```

在说到相对路径的时候，需要看下这两个特殊符号"."和".."。"."是指当前目录，".."是指当前目录的父目录。下面看一些实例：

```
//获取到当前项目目录的path
Path currentDir = Paths.get(".");

//获取到项目目录父目录的path
Path parentDir = Paths.get("..");

//对应 d:\\data\\projects\a-project
Path currentDir = Paths.get("d:\\data\\projects\.\a-project");


//对应 d:\data\projects\another-project
Path parentDir2 = Paths.get("d:\\data\\projects\\a-project\\..\\another-project");

//对应 d:\\data\\projects\a-project
Path path1 = Paths.get("d:\\data\\projects", ".\\a-project");

//对应 d:\\data\\projects\another-project
Path path2 = Paths.get("d:\\data\\projects\\a-project",
                       "..\\another-project");
```

## Path.normalize\(\)方法

normalize\(\)方法是用来规范path的，例如：

```
String originalPath =
        "d:\\data\\projects\\a-project\\..\\another-project";

Path path1 = Paths.get(originalPath);
System.out.println("path1 = " + path1);

Path path2 = path1.normalize();
System.out.println("path2 = " + path2);
```

上面这段代码执行的结果是：

```
path1 = d:\data\projects\a-project\..\another-project
path2 = d:\data\projects\another-project
```

所以这个方法应该很好理解啦。

