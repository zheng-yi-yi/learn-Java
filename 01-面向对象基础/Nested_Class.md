<div align="center">

<h1>内部类</h1>

</div>


## 概述

在Java中，内部类主要分为四种类型：

1. **成员内部类**：这是最常见的内部类，它可以访问外部类的所有成员和方法，即使它们被声明为私有。成员内部类是外部类的一个成员，所以它可以在外部类的任何地方访问。

2. **静态内部类**：静态内部类就像静态成员变量一样，它不需要外部类的对象就可以创建。静态内部类不能访问外部类的非静态成员和方法。

3. **局部内部类**：如果一个类定义在一个方法内，那么它就是一个局部内部类。就像局部变量一样，局部内部类不能被公有访问。

4. **匿名内部类**：匿名内部类是一种没有构造器的类。它通常用来简化代码，如在创建线程或者按钮动作监听时。

## 举例

以下是Java中四种内部类的示例代码：

1. **成员内部类**：

```java
public class OuterClass {
    private String outerField = "Outer field";

    class InnerClass {
        void printOuterField() {
            System.out.println(outerField);
        }
    }
}
// 使用方式
OuterClass outer = new OuterClass();
OuterClass.InnerClass inner = outer.new InnerClass();
inner.printOuterField();
```

2. **静态内部类**：

```java
public class OuterClass {
    private static String staticOuterField = "Static outer field";

    static class StaticInnerClass {
        void printOuterField() {
            System.out.println(staticOuterField);
        }
    }
}
// 使用方式
OuterClass.StaticInnerClass staticInner = new OuterClass.StaticInnerClass();
staticInner.printOuterField();
```

3. **局部内部类**：

```java
public class OuterClass {
    void outerMethod() {
        class LocalInnerClass {
            void printSomething() {
                System.out.println("Hello from Local Inner Class");
            }
        }
        // 使用方式
        LocalInnerClass localInner = new LocalInnerClass();
        localInner.printSomething();
    }
}
// 使用方式
OuterClass outer = new OuterClass();
outer.outerMethod();
```

4. **匿名内部类**：

```java
public class OuterClass {
    void outerMethod() {
        new Thread() {
            public void run() {
                System.out.println("Hello from Anonymous Inner Class");
            }
        }.start();
    }
}
// 使用方式
OuterClass outer = new OuterClass();
outer.outerMethod();
```

