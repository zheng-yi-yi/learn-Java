# Pattern与Matcher

## 概述

在Java中，`Pattern`和`Matcher`是用于处理正则表达式的两个类。在前面，我们是使用`String.matches()`方法来检查一个字符串是否匹配一个正则表达式。但是，如果我们需要对同一个正则表达式进行多次匹配，使用`String.matches()`方法就会变得效率较低，因为每次调用这个方法都会创建一个新的`Pattern`对象。

为了提高效率，我们可以先创建一个`Pattern`对象，然后使用这个对象来创建一个`Matcher`对象，然后反复使用这个`Matcher`对象进行匹配。这样，我们就可以实现编译一次正则表达式，多次匹配。

## 使用流程

`Pattern`类是一个编译后的正则表达式，对字符串进行匹配操作的引擎。我们可以使用它的`compile`方法来编译一个正则表达式，然后得到一个`Pattern`对象。

```java
Pattern pattern = Pattern.compile("[a-z]+");
```

`Matcher`类是对输入字符串进行解释和匹配操作的引擎。我们可以使用`Pattern`对象的`matcher`方法来得到一个`Matcher`对象。

```java
Matcher matcher = pattern.matcher("hello");
```

然后，我们可以使用`Matcher`对象的各种方法，如`matches`，`find`，`group`等来进行匹配操作和获取匹配结果。例如：

```java
if (matcher.matches()) {
    System.out.println("The string matches the pattern.");
}
```

> 使用 `Matcher` 时，必须首先调用 `matches()` 判断是否匹配成功，匹配成功后，才能调用 `group()` 提取子串。


## 分组匹配

前面讲过，使用括号可以包含子规则，比如正则表达式`learn\s(Java|php|go)`可以匹配`"learn java"`，`"learn php"`或`"learn go"`。

但实际上，括号 `(...)` 也常用于创建一个分组，这个分组可以包含一个或多个字符。分组的主要作用是允许我们对正则表达式的一部分应用量词，或者从匹配的字符串中提取子串。

分组后，我们就可以使用`Pattern`和`Matcher`类来进行正则匹配和分组提取。举个例子：

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Main {
    public static void main(String[] args) {
        Pattern p = Pattern.compile("(\\d{4})-(\\d{2})-(\\d{2})"); // 创建Pattern对象
        Matcher m = p.matcher("2024-03-21"); // 创建Matcher对象
        if (m.matches()) {
            String year = m.group(1);   // 提取年份
            String month = m.group(2);  // 提取月份
            String day = m.group(3);    // 提取日期
            System.out.println(year);   // 输出年份
            System.out.println(month);  // 输出月份
            System.out.println(day);    // 输出日期
        } else {
            System.out.println("匹配失败!");
        }
    }
}
```

在这里，我们使用了正则表达式`(\d{4})-(\d{2})-(\d{2})`来匹配年、月和日。

其中；

- `(\d{4})`是第一个分组，匹配4位的数字（即年份）
- `(\d{2})`是第二个分组，匹配2位的数字（即月份）
- 最后一个`(\d{2})`是第三个分组，匹配2位的数字（即日期）。

如果匹配成功，我们可以使用`Matcher.group(index)`方法来提取分组。这个方法的参数是分组的索引，1表示第一个分组（年份），2表示第二个分组（月份），3表示第三个分组（日期）。

> 注意，`Matcher.group(index)`方法的参数用`1`表示第一个子串，`2`表示第二个子串。如果我们传入`0`，就会得到整个正则匹配到的字符串。