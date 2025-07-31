---
date: '2025-07-17T00:12:02+08:00'
draft: true
title: 'Singleton Pattern'
math: true
cascade:
  type: docs
---

## Singleton Pattern

Singleton Pattern 是一种创建型设计模式，确保一个类只有一个实例，并提供一个全局访问点。

### Singleton Pattern vs. Static Variable

* 大量的 Static Variable 会污染命名空间
* Static Variable 本身可以保证在单个类中的唯一性，但问题在于每个类都可以定义自己的静态变量，无法跨类保证全局唯一性
* Singleton Pattern 允许延迟实例化（lazy instantiation），即在需要时才创建实例
* 静态变量在类加载时就被创建，无法控制实例的创建时机。

### Some Methods to Implement Singleton Pattern

#### Wrong Method

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {
        // 私有构造函数，防止外部实例化
    }
}
```

这是错误的代码，外部对象永远，无法获取实例

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {
        // 私有构造函数
    }

    // 提供全局访问点
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

现在，这个单例类可以正常工作了，但是此代码有线程安全问题(Thread Safety Issue)

#### Eager Initialization Singleton

饿汉式单例类是实现起来最简单的单例类。由于在定义静态变量的时候实例化单例类，因此在类加载的时候就已经创建了单例对象。

```java
public class Singleton {
    private static final Singleton instance = new Singleton();
    private Singleton() {
        // 私有构造函数，防止外部实例化
    }
    public static Singleton getInstance() {
        return instance;
    }
}
```

#### Lazy Singleton - synchronized

最直接的方式是给 getInstance() 方法加锁，确保同一时间只有一个线程能进入该方法。

```java
public static synchronized Singleton getInstance() {
    if (instance == null) {
        instance = new Singleton();
    }
    return instance;
}
```

* 优点：简单直接，解决了线程安全问题。
* 缺点：性能开销大。每次调用 getInstance() 都会进行同步，但实际上只有在第一次创建实例时才需要同步。一旦实例创建完毕，后续的同步都是不必要的性能损耗。

{{< callout type="info" >}}
如果无需频繁调用 getInstance() 方法，或者对性能要求不高，这种方式是完全可以接受的，简单易懂！
{{< /callout >}}

#### Lazy Singleton - 双重检查锁定（Double-Checked Locking）

```java
public class Singleton {
    // 必须用 volatile 关键字修饰
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) { // 第一次检查
            synchronized (Singleton.class) {
                if (instance == null) { // 第二次检查
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

{{% details title="volatile常见八股" closed="true" %}}

* `volatile` 关键字可以保证变量的可见性，即这个变量的值是共享且不稳定的，每次都需要到内存中读取，并且读写都会开启相应的屏障保证可见性的实现
* `volatile` 关键字保证了数据的可见性，但是不能保证数据的原子性：即只能保障“当一个线程对 `volatile` 变量进行写操作时，其他线程可以立刻看到最新的值”，而不能保证“不能保证多线程同时写环境下复合操作的原子性，例如x++”
* `volatile`可以禁止对代码进行重排序

{{% /details %}}

{{% details title="synchronized常见八股" closed="true" %}}
**synchronized的用法**

1. 同步一个代码块

```java
public class SynchronizedExample {
    private final Object lock = new Object(); // 推荐：创建一个专门的锁对象

    // 同步一个代码块，使用 this 作为锁
    public void syncBlockThis() {
        System.out.println(Thread.currentThread().getName() + " 尝试进入 syncBlockThis 同步块...");
        synchronized (this) {
            System.out.println(Thread.currentThread().getName() + " 已进入 syncBlockThis 同步块。");
            try {
                // 模拟耗时操作
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + " 即将离开 syncBlockThis 同步块。");
        }
    }

    // 同步一个代码块，使用专门的 lock 对象作为锁
    public void syncBlockObject() {
        System.out.println(Thread.currentThread().getName() + " 尝试进入 syncBlockObject 同步块...");
        synchronized (lock) {
            System.out.println(Thread.currentThread().getName() + " 已进入 syncBlockObject 同步块。");
            try {
                // 模拟耗时操作
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + " 即将离开 syncBlockObject 同步块。");
        }
    }
}
```

2. 同步一个方法

```java
public class SynchronizedExample {
    // 同步一个实例方法
    public synchronized void syncMethod() {
        System.out.println(Thread.currentThread().getName() + " 已进入 syncMethod。");
        try {
            // 模拟耗时操作
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread().getName() + " 即将离开 syncMethod。");
    }
    
    // 以下方法等价于 syncMethod()
    public void equivalentSyncMethod() {
        synchronized(this) {
            System.out.println(Thread.currentThread().getName() + " 已进入 equivalentSyncMethod。");
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + " 即将离开 equivalentSyncMethod。");
        }
    }
}
```

3. 类级别的同步

```java
public class SynchronizedExample {
    // 1. 同步一个静态方法
    public static synchronized void staticMethod() {
        System.out.println(Thread.currentThread().getName() + " 已进入 staticMethod。");
        try {
            // 模拟耗时操作
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread().getName() + " 即将离开 staticMethod。");
    }

    // 2. 上述方法等价于同步一个以 .class 为锁的代码块
    public static void equivalentStaticMethod() {
        System.out.println(Thread.currentThread().getName() + " 尝试进入 equivalentStaticMethod 同步块...");
        synchronized (SynchronizedExample.class) {
            System.out.println(Thread.currentThread().getName() + " 已进入 equivalentStaticMethod 同步块。");
            try {
                // 模拟耗时操作
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + " 即将离开 equivalentStaticMethod 同步块。");
        }
    }
}
```

---
**线程安全的实现方法有哪些?**

1. 互斥同步
synchronized 和 ReentrantLock
2. 非阻塞同步
CAS 和 AtomicInteger（本质也是CAS）
3. 无同步方案
如果一个方法本来就不涉及共享数据，那它自然就无须任何同步措施去保证正确性。例如：栈封闭、ThreadLocal、可重入代码

---
**synchronized是公平锁吗？**  
不是，是抢占式，新来的线程有可能立即获得monitor锁，这样有利于提高整体的性能

{{% /details %}}

{{< callout type="warning" >}}
  **可以对单例进行子类化吗？**
  继承单例模式的一个问题是其构造函数是私有的。你无法扩展一个具有私有构造函数的类。因此，你首先需要做的是将构造函数改为公有或受保护的。但是，这样它就不再是真正的单例了，因为其他类可以实例化它。
{{< /callout >}}

### 单例模式存在的问题

1. **线程安全问题**：在多线程环境下，单例的创建可能会出现线程安全问题，导致多个实例的产生。
2. **反序列化问题**：单例类在反序列化时可能会被重新实例化，破坏单例性。
3. **反射问题**：通过反射可以绕过私有构造函数，创建多个实例。
