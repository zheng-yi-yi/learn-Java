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

1. `append()`: 添加数据到当前`StringBuilder`对象的末尾。这个方法有多个重载版本，可以接受不同类型的参数，如`String`，`int`，`char`等。

    ```java
    StringBuilder sb = new StringBuilder();
    sb.append("Hello");
    sb.append(123);
    sb.append('A');
    ```

2. `insert()`: 在指定位置插入数据。这个方法也有多个重载版本，可以接受不同类型的参数。

    ```java
    StringBuilder sb = new StringBuilder("Hello");
    sb.insert(5, " World");  // 插入字符串
    ```

3. `delete()`: 删除从`start`位置开始到`end`位置的字符。

    ```java
    StringBuilder sb = new StringBuilder("Hello World");
    sb.delete(5, 6);  // 删除空格
    ```

4. `reverse()`: 将当前`StringBuilder`对象中的字符反转。

    ```java
    StringBuilder sb = new StringBuilder("Hello");
    sb.reverse();  // olleH
    ```

5. `toString()`: 返回当前`StringBuilder`对象的字符串表示。

    ```java
    StringBuilder sb = new StringBuilder("Hello");
    String s = sb.toString();
    ```

6. `length()`: 返回当前`StringBuilder`对象的长度。

    ```java
    StringBuilder sb = new StringBuilder("Hello");
    int len = sb.length();  // 5
    ```

7. `setLength()`: 设置当前`StringBuilder`对象的长度。如果新的长度小于当前长度，那么多余的字符将被丢弃。如果新的长度大于当前长度，那么会添加空字符。

    ```java
    StringBuilder sb = new StringBuilder("Hello");
    sb.setLength(3);  // Hel
    sb.setLength(5);  // Hel\0\0
    ```
