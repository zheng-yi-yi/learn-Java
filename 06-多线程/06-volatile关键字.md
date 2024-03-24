# volatile关键字

在Java中，`volatile`关键字是一种用于修饰变量的关键字，它可以确保线程之间共享变量的可见性。

## Java内存模型

在Java虚拟机中，变量的值保存在主内存中。当线程访问变量时，它会先获取一个副本，并保存在自己的工作内存中。如果线程修改了变量的值，虚拟机会在某个时刻把修改后的值回写到主内存，但是，这个时间是不确定的。

这会导致一个问题：如果一个线程更新了某个变量，另一个线程读取的值可能还是更新前的。这就造成了多线程之间共享的变量不一致。

## volatile关键字的作用

`volatile`关键字的目的是告诉虚拟机：

- 每次访问变量时，总是获取主内存的最新值；
- 每次修改变量后，立刻回写到主内存。

这样，当一个线程修改了某个共享变量的值，其他线程能够立刻看到修改后的值。

```java
class SharedVariable {
    volatile boolean flag = true;
}

SharedVariable sharedVariable = new SharedVariable();

// 线程1
new Thread(() -> {
    while (sharedVariable.flag) {
        // 执行任务
    }
}).start();

// 线程2
new Thread(() -> {
    sharedVariable.flag = false;
}).start();
```

在这个例子中，我们创建了一个共享变量`flag`，并在两个线程中访问它。线程1会一直执行任务，直到`flag`被设置为`false`。线程2则会在某个时刻把`flag`设置为`false`。由于`flag`被声明为`volatile`，所以线程1可以立刻看到线程2对`flag`的修改。

## 注意事项

虽然`volatile`关键字可以确保共享变量的可见性，但是它并不能提供原子性。也就是说，`volatile`关键字不能防止并发问题。如果需要在多线程环境中安全地修改变量，我们应该使用`synchronized`关键字或者`java.util.concurrent`包中的工具。我们会在后面的章节中讲述。