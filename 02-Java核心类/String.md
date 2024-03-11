<div align="center">

<h1>String</h1>

</div>

## 概述

在Java中，`String`是一个引用类型，它本身也是一个`class`。

```java
String s1 = "Hello World!";
```

实际上`String` 类的内部是通过一个 `char[]` 数组来存储字符串的。但从Java 9开始，为了优化内存使用，`String` 类的内部实现改为使用一个 `byte[]` 数组和一个编码标志字段来存储字符串。

> 这是因为许多字符串（例如HTTP头信息和文本文件）主要包含ISO-8859-1/Latin-1字符，这些字符可以用一个字节来表示，而不是 `char` 类型的两个字节。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence,
               Constable, ConstantDesc {

    @Stable
    private final byte[] value;

    // ...
}
```

## 不可变性

在Java中，`String` 对象一旦被创建，其值就不能被改变。任何看似改变 `String` 值的操作，如 `substring`, `concat`, `replace` 等，都会返回一个新的 `String` 对象，原始的 `String` 对象的值不会改变。

```java
String s = "Hello";
System.out.println(s);  // 输出 "Hello"
s = s.toUpperCase();
System.out.println(s);  // 输出 "HELLO"
```

这种不可变性是通过内部类的 `private final byte[]` 字段以及没有任何修改 `byte[]` 的方法实现的。

## 字符串池

为了提高性能和减少内存开销，Java语言设计者引入了字符串池的概念。当你创建一个 `String` 对象时，JVM首先会在字符串池中查找是否存在相同的字符串，如果存在，就返回字符串池中的对象，否则，它将在字符串池中创建一个新的 `String` 对象。

## 字符串比较

在Java中，比较两个字符串可以使用 `equals()` 方法或 `compareTo()` 方法。

`equals()` 方法比较两个字符串的内容是否相同。例如：

```java
String s1 = "Hello";
String s2 = "Hello";
System.out.println(s1.equals(s2));  // 输出 true
```

`compareTo()` 方法比较两个字符串的字典顺序。如果两个字符串相等，它返回0；如果调用字符串小于参数字符串，它返回一个负数；如果调用字符串大于参数字符串，它返回一个正数。例如：

```java
String s1 = "Hello";
String s2 = "World";
System.out.println(s1.compareTo(s2));  // 输出一个负数
```

**注意，不要使用 `==` 来比较字符串的内容，因为 `==` 比较的是两个引用是否指向同一个对象**。

## 搜索子串

1. `indexOf(String str)`: 返回指定子字符串在此字符串中第一次出现处的索引。
2. `indexOf(String str, int fromIndex)`: 返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始。
3. `lastIndexOf(String str)`: 返回指定子字符串在此字符串中最右边出现处的索引。
4. `lastIndexOf(String str, int fromIndex)`: 返回指定子字符串在此字符串中最后一次出现处的索引，从指定的索引开始反向搜索。
5. `contains(CharSequence s)`: 当且仅当此字符串包含指定的 char 值序列时，返回 true。

```java
public class StringSearch {
    public static void main(String[] args) {
        String str = "Hello World! Hello Java!";

        // 1. indexOf(String str)
        int indexOfHello = str.indexOf("Hello");
        System.out.println("第一次出现 \"Hello\" 的索引: " + indexOfHello);

        // 2. indexOf(String str, int fromIndex)
        int indexOfHello2 = str.indexOf("Hello", indexOfHello + 1);
        System.out.println("从指定索引后开始的第一次出现 \"Hello\" 的索引: " + indexOfHello2);

        // 3. lastIndexOf(String str)
        int lastIndexOfHello = str.lastIndexOf("Hello");
        System.out.println("最后一次出现 \"Hello\" 的索引: " + lastIndexOfHello);

        // 4. lastIndexOf(String str, int fromIndex)
        int lastIndexOfHello2 = str.lastIndexOf("Hello", lastIndexOfHello - 1);
        System.out.println("从指定索引前开始的最后一次出现 \"Hello\" 的索引: " + lastIndexOfHello2);

        // 5. contains(CharSequence s)
        boolean containsJava = str.contains("Java");
        System.out.println("字符串是否包含 \"Java\": " + containsJava);
    }
}

```

运行结果：

```
第一次出现 "Hello" 的索引: 0
从指定索引后开始的第一次出现 "Hello" 的索引: 13
最后一次出现 "Hello" 的索引: 13
从指定索引前开始的最后一次出现 "Hello" 的索引: 0
字符串是否包含 "Java": true
```



## 提取子串

1. `substring(int beginIndex)`: 返回一个新的字符串，它是此字符串的一个子字符串。子串开始于指定的 `beginIndex` 并扩展到此字符串的末尾。
2. `substring(int beginIndex, int endIndex)`: 返回一个新字符串，它是此字符串的一个子字符串。子串从指定的 `beginIndex` 开始，直到索引 `endIndex - 1` 的字符。因此，子串的长度为 `endIndex-beginIndex`。

```java
public class SubstringExample {
    public static void main(String[] args) {
        String str = "Hello World";

        // 1. substring(int beginIndex)
        String substring1 = str.substring(6);
        System.out.println("从索引 6 开始到字符串末尾的子字符串: " + substring1);

        // 2. substring(int beginIndex, int endIndex)
        String substring2 = str.substring(0, 5);
        System.out.println("从索引 0 开始到索引 4 的子字符串: " + substring2);
    }
}
```

运行结果：

```bash
从索引 6 开始到字符串末尾的子字符串: World
从索引 0 开始到索引 4 的子字符串: Hello
```



## 替换子串

1. `replace(char oldChar, char newChar)`: 这个方法用指定的新字符替换字符串中所有的旧字符。

```java
public class ReplaceExample {
    public static void main(String[] args) {
        String str = "Hello World";
        
        // 用 'W' 替换 'l'
        String replacedString = str.replace('l', 'W');
        System.out.println("替换后的字符串: " + replacedString);
    }
}
```

2. `replace(CharSequence target, CharSequence replacement)`: 这个方法用指定的新字符序列替换字符串中所有匹配的旧字符序列。

```java
public class ReplaceExample {
    public static void main(String[] args) {
        String str = "Hello World";
        
        // 用 "Java" 替换 "World"
        String replacedString = str.replace("World", "Java");
        System.out.println("替换后的字符串: " + replacedString);
    }
}
```

3. `replaceAll(String regex, String replacement)`: 这个方法使用给定的替换字符串替换所有匹配给定正则表达式的子字符串。

```java
public class ReplaceExample {
    public static void main(String[] args) {
        String str = "Hello World";
        
        // 用 "Java" 替换所有字母
        String replacedString = str.replaceAll("[a-zA-Z]", "Java");
        System.out.println("替换后的字符串: " + replacedString);
    }
}
```

4. `replaceFirst(String regex, String replacement)`: 这个方法使用给定的替换字符串替换第一个匹配给定正则表达式的子字符串。

```java
public class ReplaceExample {
    public static void main(String[] args) {
        String str = "Hello World";
        
        // 用 "Java" 替换第一个单词
        String replacedString = str.replaceFirst("\\b\\w+\\b", "Java");
        System.out.println("替换后的字符串: " + replacedString);
    }
}
```



## 分割字符串

1. `split(String regex)`: 这个方法根据给定的正则表达式来拆分字符串。它会将字符串拆分成多个子字符串，并将这些子字符串存储在一个数组中。

```java
public class SplitExample {
    public static void main(String[] args) {
        String str = "apple,banana,orange";
        
        // 使用逗号 "," 来拆分字符串
        String[] parts = str.split(",");
        
        // 打印拆分后的子字符串
        for (String part : parts) {
            System.out.println(part);
        }
    }
}
```

2. `split(String regex, int limit)`: 这个方法根据给定的正则表达式来拆分字符串，但是可以限制拆分的次数：
   - 如果 `limit` 大于0，那么模式将被应用最多 `limit-1` 次，数组的长度将不会大于 `limit`，并且数组的最后一个条目将包含所有输入超出最后一个匹配的剩余部分。
   - 如果 `limit` 小于等于0，那么模式将被应用尽可能多的次数。

```java
public class SplitExample {
    public static void main(String[] args) {
        String str = "apple,banana,orange,grape";
        
        // 使用逗号 "," 来拆分字符串，限制最多拆分 2 次
        String[] parts = str.split(",", 2);
        
        // 打印拆分后的子字符串
        for (String part : parts) {
            System.out.println(part);
        }
    }
}
```



## 拼接字符串

1. `concat(String str)`: 将指定字符串连接到此字符串的结尾。这等价于使用 `+` 运算符。
2. `join(CharSequence delimiter, CharSequence... elements)`: 使用指定的分隔符将元素连接成一个字符串。
3. `join(CharSequence delimiter, Iterable<? extends CharSequence> elements)`: 使用指定的分隔符将元素连接成一个字符串。

```java
// 使用 concat 方法连接字符串
String str1 = "Hello";
String str2 = "World";
String result1 = str1.concat(str2); // "HelloWorld"

// 使用 join 方法连接字符串数组
String[] elements = {"Java", "Python", "C++"};
String result2 = String.join(", ", elements); // "Java, Python, C++"

// 使用 join 方法连接 Iterable
List<String> list = Arrays.asList("Java", "Python", "C++");
String result3 = String.join(", ", list); // "Java, Python, C++"
```



## 格式化字符串

1. `format(String format, Object... args)`: 返回一个格式化字符串，使用指定的格式字符串和参数。
2. `format(Locale l, String format, Object... args)`: 使用指定的语言环境、格式字符串和参数返回一个格式化字符串。

```java
// 使用 format 方法格式化字符串
String result1 = String.format("Hello, %s!", "World"); // "Hello, World!"

// 使用 format 方法格式化字符串，并指定语言环境
Locale locale = new Locale("zh", "CN"); // 中国的语言环境
String result2 = String.format(locale, "你好，%s！", "世界"); // "你好，世界！"
```

> 备注，可以使用以下占位符：
>
> - `%s`：用于字符串。
> - `%d`：用于十进制整数。
> - `%f`：用于浮点数。
> - `%b`：用于布尔值。
> - `%c`：用于字符。
> - `%e`：用于科学记数法表示的浮点数。
> - `%t`：用于时间和日期。
> - `%x`：用于十六进制整数。
> - `%n`：用于平台特定的行分隔符。
>
> 每个占位符都可以与一组可选的标志和宽度一起使用，以控制格式化的输出。例如，`%10s` 将字符串格式化为至少10个字符宽，`%-10s` 将字符串格式化为至少10个字符宽，并左对齐文本。



## 类型转换

### 基本数据类型转为String类型

`String.valueOf(dataType data)`是一个静态方法，可以接受所有基本数据类型，并将其转换为字符串。

### String类型转为基本数据类型

每个包装类都有一个 `parseXXX(String s)` 方法可以将字符串转换为相应的基本数据类型：

1. `Integer.parseInt(String s)`
2. `Long.parseLong(String s)`
3. `Double.parseDouble(String s)`
4. `Float.parseFloat(String s)`
5. `Boolean.parseBoolean(String s)`
6. `Byte.parseByte(String s)`
7. `Short.parseShort(String s)`

### String类型转换为char数组

`toCharArray()`方法可以将字符串转换为一个新的字符数组。



## 其他方法

`trim()`: 返回字符串的副本，忽略前导空白和尾部空白。

`isEmpty()`: 当且仅当 `length()` 为 0 时返回 true。

`isBlank()`: 如果字符串为空或仅包含空白字符，则返回 true。

