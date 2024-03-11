<div align="center">

<h1>JVM类加载与双亲委派模型</h1>

</div>


## 类加载器的基本功能

1. **加载**：这是类加载过程的首要任务。加载的过程是由类加载器负责读取Java二进制代码（`.class`文件），并将其内容转化为Java虚拟机可以识别的数据结构（在堆内存中生成一个代表这个类的`java.lang.Class`对象）。

2. **链接**：链接过程主要包括验证、准备和解析三个阶段。验证是为了确保被加载的类的正确性和安全性；准备阶段是为类的静态变量分配内存并设置默认值；解析阶段是将类的符号引用转换为直接引用。

3. **初始化**：初始化阶段是执行类的初始化方法，包括类变量的初始化和执行类的静态初始化块。

4. **卸载**：当一个类不再被引用，或者其对应的Java ClassLoader对象被回收，或者在Java虚拟机关闭的时候，Java虚拟机将会卸载Class对象。

> 备注：每一个类加载器，都拥有一个独立的类名称空间，保证了不同类加载器可以加载同名的类，而这些类又互不相干，是完全独立的。


```
Java类加载过程
├── 加载（Loading）:查找并加载字节码文件（.class文件）,通过类的全限定名查找类文件，将类文件加载到内存中
│   ├── 查找并加载字节码文件（.class文件）通过一个类的全限定名来获取定义此类的二进制字节流。
│   ├── 将字节流代表的静态存储结构转化为方法区的运行时数据结构。
│   └── 在内存中生成一个代表这个类的java.lang.Class对象，作为方法区这个类的各种数据的访问入口。
│
├── 链接（Linking）
│   ├── 验证（Verification）检查类文件的格式、语义等是否符合Java规范
│   │   ├── 文件格式验证：验证字节流是否符合Class文件格式的规范。
│   │   ├── 元数据验证：对字节码进行语义分析，保证其符合Java语言规范。
│   │   ├── 字节码验证：检查字节码流是否包含不正确的或不合法的代码。
│   │   └── 符号引用验证：确保解析后的符号引用可以正确链接到目标。
│   │
│   ├── 准备（Preparation）:为类的静态变量分配内存并初始化默认值
│   │   ├── 为类变量分配内存并设置默认初始值（0、null等）。
│   │   └── 这里的“准备”阶段不会真正初始化类变量的值。
│   │
│   └── 解析（Resolution）:将类、接口、字段和方法的符号引用解析为直接引用
│       ├── 将常量池中的符号引用替换为直接引用。
│       └── 解析过程主要针对类或接口、字段、类方法、接口方法、方法类型等。
│
│── 初始化（Initialization）为类的静态变量赋予正确的初始值、执行静态初始化器和执行静态变量赋值语句
│    ├── 类初始化时机：包括类被首次主动使用（如创建实例、调用静态方法、访问静态字段等）时
│    └── 类初始化的执行顺序：类初始化时，先初始化父类，然后再初始化子类。
│
│── 使用（Using）通过类的引用来使用该类
│   ├── 创建类的实例
│   ├── 调用类的方法
│   └── 访问类的字段
└── 卸载
```

## 四种类型的类加载器

1. **引导类加载器（`Bootstrap ClassLoader`）**：这是最顶层的类加载器，主要负责加载Java的核心类库，如`java.lang.*`，`java.util.*`等。这个类加载器是用C++编写的，是JVM的一部分，它不是一个Java类。

2. **扩展类加载器（`Extension ClassLoader`）**：这个类加载器负责加载JRE的扩展目录（`jre/lib/ext`）中的类库。它是由`sun.misc.Launcher$ExtClassLoader`实现的。

3. **应用类加载器（`Application ClassLoader`）**：这个类加载器负责加载用户类路径（`ClassPath`）上所指定的类库，这也是程序中默认的类加载器，一般来说，Java应用的类都是由它来完成加载的。它是由`sun.misc.Launcher$AppClassLoader`实现的。

4. **自定义类加载器（`User ClassLoader`）**：Java也允许我们自定义类加载器，我们可以继承`java.lang.ClassLoader`类，然后覆盖`findClass(String name)`方法，实现自己的类加载逻辑。

## 双亲委派模型

双亲委派模型是Java类加载器在加载类时所遵循的一个重要原则。具体来说，这个模型规定：

1. **委派**：如果一个类加载器收到了类加载请求，它首先不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成。这就是所谓的“委派”的概念。

2. **顶层尝试加载**：每一个层次的类加载器都是如此，因此所有的加载请求最终都应该传送到顶层的引导类加载器中。引导类加载器会尝试加载这个类。

3. **子加载器尝试加载**：只有当父类加载器反馈自己无法完成这个加载请求（它的搜索范围中没有找到所需的类）时，子加载器才会尝试自己去加载。

比如一个定义在`java.lang`包下的类，因为它被存放在`rt.jar`之中，所以在被加载过程中，会被一直委托到`Bootstrap ClassLoader`，最终由`Bootstrap ClassLoader`所加载。

而一个用户自定义的`com.zyy.ClassZyy`类，它也会被一直委托到`Bootstrap ClassLoader`，但是因为`Bootstrap ClassLoader`不负责加载该类，那么会在由`Extention ClassLoader`尝试加载，而`Extention ClassLoader`也不负责这个类的加载，最终就被`Application ClassLoader`加载。

## 为什么需要双亲委派，不委派有什么问题？

1. **避免类的重复加载**：如果没有双亲委派模型，每个类加载器都会尝试自己去加载类，这可能会导致同一个类被多次加载，造成资源的浪费。

2. **保护Java核心API**：双亲委派模型确保了所有的类加载请求都会传递到引导类加载器中，这就保证了Java核心API类（如java.lang.Object类）不会被覆盖和篡改。如果没有双亲委派模型，用户自定义的类加载器可能会加载一个与Java核心API类同名的类，这可能会导致严重的问题。

3. **保证类加载的正确性**：双亲委派模型确保了类加载的有序性和正确性。父类加载器加载的类对于子类加载器是可见的，这保证了子类加载器可以正确地加载依赖于父类加载器加载的类的类。

> 总结：如果没有双亲委派模型，可能会出现类的重复加载、Java核心API被篡改以及类加载的错误等问题。

## "父加载器"和"子加载器"之间的关系是继承的吗？

"父加载器"和"子加载器"之间的关系并不是继承关系，而是一种委托关系。也就是说，类加载器（`ClassLoader`）之间的父子关系通常是通过组合（`Composition`）来实现的，而不是继承（Inheritance）。

当我们说一个类加载器是另一个类加载器的父加载器时，实际上是指：当子加载器需要加载一个类时，它首先会请求其父加载器来加载。如果父加载器无法加载这个类，子加载器才会尝试自己去加载。


## 双亲委派是怎么实现的？


实现双亲委派的代码都集中在`java.lang.ClassLoader`的`loadClass()`方法之中：

```java
public Class<?> loadClass(String name) throws ClassNotFoundException {
    return loadClass(name, false);
}

protected Class<?> loadClass(String name, boolean resolve)
    throws ClassNotFoundException
{
    synchronized (getClassLoadingLock(name)) {
        // First, check if the class has already been loaded
        Class<?> c = findLoadedClass(name);
        if (c == null) {
            long t0 = System.nanoTime();
            try {
                if (parent != null) {
                    c = parent.loadClass(name, false);
                } else {
                    c = findBootstrapClassOrNull(name);
                }
            } catch (ClassNotFoundException e) {
                // ClassNotFoundException thrown if class not found
                // from the non-null parent class loader
            }

            if (c == null) {
                // If still not found, then invoke findClass in order
                // to find the class.
                long t1 = System.nanoTime();
                c = findClass(name);

                // this is the defining class loader; record the stats
                PerfCounter.getParentDelegationTime().addTime(t1 - t0);
                PerfCounter.getFindClassTime().addElapsedTimeFrom(t1);
                PerfCounter.getFindClasses().increment();
            }
        }
        if (resolve) {
            resolveClass(c);
        }
        return c;
    }
}
```

`java.lang.ClassLoader` 的 `loadClass` 方法是双亲委派模型的核心。

1. 首先检查类是否已经被加载
2. 如果没有被加载，那么它会：
   1. 检查是否存在父类加载器：
      1. 如果存在，就让父类加载器尝试加载这个类
      2. 如果不存在父类加载器，就让引导类加载器尝试加载这个类。
   2. 如果父类加载器和引导类加载器都无法加载这个类，那么这个类加载器就会尝试自己去加载这个类。


> 在加载类的过程中，使用了 `synchronized` 关键字对类加载过程进行了同步，这是为了防止在多线程环境下出现并发问题。


## 如何主动破坏双亲委派机制？


知道了双亲委派模型的实现，那么想要破坏双亲委派机制就很简单了。由于它的双亲委派过程都是在`loadClass`方法中实现的，那么想要破坏这种机制，那么就自定义一个类加载器，重写其中的`loadClass`方法，使其不进行双亲委派即可。

举个例子：

```java
public class CustomClassLoader extends ClassLoader {
    @Override
    public Class<?> loadClass(String name) throws ClassNotFoundException {
        // 首先, 检查该类型是否已经被加载
        Class<?> c = findLoadedClass(name);
        if (c == null) {
            // 如果类还没有被加载, 则由自己加载
            c = findClass(name);
        }
        return c;
    }
}
```

在这个例子中，我们创建了一个自定义的类加载器 `CustomClassLoader`，并重写了 `loadClass` 方法。在这个方法中，我们首先检查类是否已经被加载，如果没有被加载，那么我们直接调用 `findClass` 方法来加载这个类，而不是像双亲委派模型那样，先尝试让父类加载器来加载这个类。


## loadClass()、findClass()、defineClass() 的区别

- `loadClass()`: 这是主要的类加载方法，它实现了双亲委派模型。如果你想破坏双亲委派模型，你可以重写这个方法。

- `findClass()`: 这个方法是用来找到类的字节码文件并加载它。如果你想自定义类加载器，但是仍然想遵守双亲委派模型，你可以重写这个方法。在 `loadClass()` 方法中，如果父类加载器无法加载类，那么就会调用 `findClass()` 方法。

- `defineClass()`: 这个方法是用来将字节码转换为 `Class` 对象。这个方法通常在 `findClass()` 方法中被调用，当你找到了类的字节码文件后，你可以调用 `defineClass()` 方法来创建 `Class` 对象。

