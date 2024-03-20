# StringBuilder

## 一、概述

StringBuilder类可以用于高效地拼接字符串，它是一个可变对象（可预分配缓冲区)，不像String类。StringBuilder对象在新增字符时不会创建新的临时对象。

举一个最简单的例子：

```java
public class Main {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < 10; i++) {
            sb.append(i).append(" ");
        }

        System.out.println(sb.toString());
    }
}
// 0 1 2 3 4 5 6 7 8 9 
```

## 二、链式调用

`StringBuilder`还可以进行链式操作：

```java
public class Main {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();

        sb.append("Hello").append(" ").append("World").append("!");

        System.out.println(sb.toString());  // 输出: Hello World!
    }
}
```

这是因为，`append()`方法在最后会返回`this`，这样就可以不断地调用自身的其他方法：

```java
// 源码：
public StringBuilder append(StringBuffer sb) {
    super.append(sb);
    return this;
}
```

## 三、自动优化

在Java 8及更高版本中，Java编译器会自动优化字符串连接操作。当编译器遇到字符串连接操作（例如，`String s = "Hello " + "World!"`）时，它会自动将其转换为`StringBuilder`操作或者`StringConcatFactory`的操作，这取决于具体的情况。

然而，如果字符串连接操作在一个循环中，编译器可能无法进行这种优化。在这种情况下，使用`StringBuilder`通常会更有效率。例如：

```java
public class Main {
    public static void main(String[] args) {
        String s = "";
        for (int i = 0; i < 1000; i++) {
            s += i + " ";  // 这里的字符串连接操作在每次循环时都会创建一个新的字符串对象
        }
        System.out.println(s);
    }
}
```

在上面的代码中，每次循环都会创建一个新的字符串对象，这可能会导致性能问题。如果我们使用`StringBuilder`，则可以避免这个问题：

```java
public class Main {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 1000; i++) {
            sb.append(i).append(" ");  // 这里的字符串连接操作不会创建新的字符串对象
        }
        System.out.println(sb.toString());
    }
}
```

在这个版本的代码中，`StringBuilder`对象在整个循环中只被创建一次，然后在每次循环时被复用。这通常会比创建新的字符串对象更有效率。

## 四、StringBuffer

`StringBuffer`和`StringBuilder`在功能上是相似的，它们都是用于处理可变字符串的。然而，`StringBuffer`是线程安全的，而`StringBuilder`不是。因此如果你在多线程环境中处理字符串，你应该使用`StringBuffer`。

然而，线程安全是有代价的。`StringBuffer`的操作需要进行同步，这会导致它在单线程环境中的性能比`StringBuilder`差。因此，如果你在单线程环境中处理字符串，你应该使用`StringBuilder`，因为它更快。

## 五、常用方法

