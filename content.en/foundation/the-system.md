---
title: 'The System'
weight: 2
references:
    links:
        - https://medium.com/@PavelTrifonov/why-jvm-is-great-4b3d8b224eae
---

To build and run Java programs, two key pieces come into play: the **JDK** and the **JVM**.

### 🛠️ What is JDK?

> The **Java Development Kit (JDK)** is the toolbox for Java developers.It gives you everything you need to **write**, **compile**, and **build** Java applications.

Inside the JDK, you'll find:

```goat
+---------------------------------+
|        Java Development Kit     |
+---------------------------------+
|                                 |
|  javac  --> Java Compiler       |
|  java   --> Launcher (JVM)      |
|  javadoc --> Doc Generator      |
|  jar     --> Packaging Tool     |
|  tools   --> Debugging, etc.    |
+---------------------------------+
```



Think of the JDK as your workshop—it doesn't just let you write code, it helps you turn that code into something that actually works.

---

### 🧠 What is JVM?

> The **Java Virtual Machine (JVM)** is where the magic happens.Many people think JVM is just about portability—"Write once, run anywhere." That’s true, but that’s only part of the story. The JVM is a **managed runtime**. That means it does much more:

* It **runs your Java bytecode**
* It **optimizes performance** through Just-In-Time (JIT) compilation
* It **manages memory** through Garbage Collection
* It **keeps your application secure and stable**

```goat
+-----------------+
|  Your Code      |   <-- You write a .java file
|  (Hello.java)   |
+--------+--------+
         |
         |  Compile (javac)
         v
+-------------------+
|  Bytecode         |   <-- javac outputs Hello.class
|  (Hello.class)    |
+--------+----------+
         |
         |  Run (java)
         v
+---------------------+
|  Java Virtual Machine|   <-- JVM executes bytecode
|  (JVM)              |
+--------+------------+
         |
         |  Execution with:
         |  - Memory Management
         |  - JIT Compilation
         |  - Security Checks
         v
+-----------------------+
|  Your Program Runs    |
+-----------------------+
```

In simple terms, the JVM is like the brain behind the scenes. It doesn’t care which machine you’re on—Windows, Linux, Mac—it runs your program in a controlled, optimized environment.

---

### 🌀 Compile Once, Run Smart

Unlike many other languages, Java uses both **compilation and interpretation**:

1. You **write code** in `.java` files
2. The JDK **compiles** it into **bytecode** (`.class` files)
3. The JVM then **interprets or compiles** that bytecode at runtime

This gives Java the **best of both worlds**—the speed of compiled languages and the flexibility of interpreted ones.

Next time someone says Java is old-school, tell them it's running on one of the smartest virtual machines in software history.


```
