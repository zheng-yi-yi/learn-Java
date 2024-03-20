# StringBuilder

## 一、前言

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

