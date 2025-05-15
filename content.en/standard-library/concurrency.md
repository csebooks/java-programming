---
title: 'Concurrency'
weight: 6
---

> Concurrency in Java allows multiple threads to execute independently, making better use of system resources and enabling faster, responsive applications. Java provides built-in support for multithreading, synchronization, and concurrent data structures.

---

### Modules Involved

- `java.lang` – `Thread`, `Runnable`, `Object.wait()`, `Object.notify()`
- `java.util.concurrent` – Executors, Futures, Locks, Queues
- `java.util.concurrent.atomic` – Atomic variables
- `java.util.concurrent.locks` – Explicit Lock implementations

---

### High-Level Concurrency Interfaces (ASCII Hierarchy)

```

```
           +------------------+
           |     Runnable     |
           +------------------+
                    |
                    v
           +------------------+       +---------------------+
           |     Thread       |<------|   Callable<T>       |
           +------------------+       +---------------------+
                    |
                    v
           +------------------+       +----------------------+
           | ExecutorService  |<------|  Future<T>           |
           +------------------+       +----------------------+
                    |
                    v
           +------------------+
           | ScheduledExecutor|
           +------------------+
```

````

---

### Core Concepts

#### 1. Threads and Runnable

You can create a thread by either extending `Thread` or implementing `Runnable`.

```java
Runnable task = () -> System.out.println("Running in thread!");
Thread thread = new Thread(task);
thread.start();
````

#### 2. Executors

The `ExecutorService` provides a thread pool for managing and reusing threads.

```java
ExecutorService executor = Executors.newFixedThreadPool(2);
executor.submit(() -> System.out.println("Task in thread pool"));
executor.shutdown();
```

#### 3. Callable and Future

Use `Callable<T>` for tasks that return results, and `Future<T>` to get those results asynchronously.

```java
Callable<Integer> task = () -> 1 + 1;
Future<Integer> future = executor.submit(task);
System.out.println("Result: " + future.get());
```

#### 4. Locks

The `Lock` interface offers more granular control compared to `synchronized`.

```java
Lock lock = new ReentrantLock();
lock.lock();
try {
    // critical section
} finally {
    lock.unlock();
}
```

#### 5. Concurrent Collections

* `ConcurrentHashMap` – thread-safe version of HashMap
* `CopyOnWriteArrayList` – safe for iteration while modifying
* `BlockingQueue` – supports producer-consumer scenarios

---

### Sample Producer-Consumer with BlockingQueue

```java
BlockingQueue<String> queue = new ArrayBlockingQueue<>(10);

Runnable producer = () -> {
    try {
        queue.put("Item");
        System.out.println("Produced");
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
    }
};

Runnable consumer = () -> {
    try {
        String item = queue.take();
        System.out.println("Consumed: " + item);
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
    }
};

new Thread(producer).start();
new Thread(consumer).start();
```

---

Java’s concurrency utilities allow us to build highly scalable and responsive applications with minimal low-level thread management. We'll explore parallelism, synchronization, and newer constructs like virtual threads in upcoming chapters.

```

Let me know if you'd like to go next with **Networking**, **Security**, or **Virtual Threads** (Project Loom).
```
