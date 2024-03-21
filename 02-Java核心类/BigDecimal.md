# BigDecimal

## 一、概述

在Java中，`BigDecimal`类提供了一种方式来操作高精度的浮点数。这个类在`java.math`包中，可以表示任意大小且精度完全准确的浮点数，并提供了一系列的方法来进行高精度的算术运算，比如加法、减法、乘法、除法、取模、幂运算等。

## 二、基础用法

1. **创建BigDecimal**：可以通过传递一个字符串或者一个双精度浮点数给`BigDecimal`的构造函数来创建一个`BigDecimal`实例。

```java
BigDecimal bd1 = new BigDecimal("0.1");
BigDecimal bd2 = new BigDecimal(0.1);
```

注意：由于浮点数的精度问题，推荐使用字符串的方式来创建`BigDecimal`。

2. **加法**：使用`add`方法进行加法运算。

```java
BigDecimal bd1 = new BigDecimal("0.1");
BigDecimal bd2 = new BigDecimal("0.2");
BigDecimal sum = bd1.add(bd2);
```

3. **减法**：使用`subtract`方法进行减法运算。

```java
BigDecimal bd1 = new BigDecimal("0.2");
BigDecimal bd2 = new BigDecimal("0.1");
BigDecimal difference = bd1.subtract(bd2);
```

4. **乘法**：使用`multiply`方法进行乘法运算。

```java
BigDecimal bd1 = new BigDecimal("0.1");
BigDecimal bd2 = new BigDecimal("0.2");
BigDecimal product = bd1.multiply(bd2);
```

5. **除法**：使用`divide`方法进行除法运算。注意，除法可能会出现无法除尽的情况，这时可以传入一个舍入模式来处理。

```java
BigDecimal bd1 = new BigDecimal("0.2");
BigDecimal bd2 = new BigDecimal("0.1");
BigDecimal quotient = bd1.divide(bd2, BigDecimal.ROUND_HALF_UP); // 使用四舍五入的舍入模式
```

## 三、其他方法

1. **比较**：`BigDecimal`类提供了`compareTo`方法来比较两个`BigDecimal`的大小。

```java
BigDecimal bd1 = new BigDecimal("0.1");
BigDecimal bd2 = new BigDecimal("0.2");
int result = bd1.compareTo(bd2); // 返回-1表示bd1小于bd2，0表示相等，1表示bd1大于bd2
```

2. **绝对值**：`abs`方法返回`BigDecimal`的绝对值。

```java
BigDecimal bd = new BigDecimal("-0.1");
BigDecimal absBd = bd.abs(); // 返回0.1
```

3. **最大值和最小值**：`max`和`min`方法可以用来找出两个`BigDecimal`中的最大值和最小值。

```java
BigDecimal bd1 = new BigDecimal("0.1");
BigDecimal bd2 = new BigDecimal("0.2");
BigDecimal maxBd = bd1.max(bd2); // 返回0.2
BigDecimal minBd = bd1.min(bd2); // 返回0.1
```

4. **取反**：`negate`方法返回`BigDecimal`的取反值。

```java
BigDecimal bd = new BigDecimal("0.1");
BigDecimal negBd = bd.negate(); // 返回-0.1
```

5. **设置精度**：`setScale`方法可以设置`BigDecimal`的小数点后的位数和舍入模式。

```java
BigDecimal bd = new BigDecimal("0.12345");
BigDecimal newBd = bd.setScale(2, BigDecimal.ROUND_HALF_UP); // 返回0.12
```

6. **查看位数**：`scale()`方法返回`BigDecimal`小数点后的位数。

```java
BigDecimal bd = new BigDecimal("123.456");
int scale = bd.scale(); // 返回3，因为小数点后有3位
```

7. **去掉末尾0**：`stripTrailingZeros`方法返回一个`BigDecimal`，它等价于这个`BigDecimal`，但是去掉了末尾的0。

```java
BigDecimal bd = new BigDecimal("123.4500");
BigDecimal newBd = bd.stripTrailingZeros(); // 返回123.45
```
