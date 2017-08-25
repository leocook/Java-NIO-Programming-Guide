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
Paths.get()
```



