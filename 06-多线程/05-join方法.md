# 线程的join方法

在Java中，`Thread`类的`join()`方法允许一个线程等待另一个线程完成。

## join方法的基本用法

当在一个线程实例上调用`join()`方法时，当前执行线程**将被阻塞**，直到该线程实例完成执行。

```java
Thread thread = new Thread(() -> {
    // 执行一些操作
});
thread.start();

try {
    thread.join();
} catch (InterruptedException e) {
    // 当前线程在等待时被中断
}
// 当执行到这里时，可以确保thread已经执行完毕
```

在这个例子中，我们首先启动了一个新的线程，然后在当前线程中调用`thread.join()`。这会使当前线程等待，直到`thread`线程执行完毕。

## join方法与中断

值得注意的是，如果当前线程在等待`join()`方法返回时被中断，那么`join()`方法会立即返回，并抛出`InterruptedException`。这是因为`join()`方法是一个可以响应中断的阻塞方法（我们在上一讲《中断线程》中讲过）。

```java
Thread thread = new Thread(() -> {
    // 执行一些操作
});
thread.start();

try {
    thread.join();
} catch (InterruptedException e) {
    // 当前线程在等待时被中断，处理中断逻辑
}
```

如果当前线程在等待`thread`线程完成时被中断，那么`join()`方法会抛出`InterruptedException`，我们可以在捕获这个异常后处理中断逻辑。

## join方法的超时用法

`join()`方法还有一个重载版本，接受一个毫秒数参数。这个版本的`join()`方法会使当前线程等待指定的毫秒数，或者等待被`join()`的线程完成，以先到者为准。

```java
Thread thread = new Thread(() -> {
    // 执行一些操作
});
thread.start();

try {
    thread.join(1000); // 等待1秒
} catch (InterruptedException e) {
    // 当前线程在等待时被中断
}
// 当执行到这里时，无法确保thread已经执行完毕，因为可能已经超时
```

这里我们在`join()`方法中传入了1000毫秒作为超时时间。也就是说，当前线程会等待`thread`线程完成或者等待1秒，以先到者为准。因此我们无法确定thread已经执行完毕（有可能是超时）。