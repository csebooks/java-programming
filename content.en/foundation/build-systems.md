---
title: 'Build Systems'
weight: 5
---

After writing your first `HelloWorld.java`, you’ve seen how Java works at a basic level. But real-world Java projects are more than a single file. They involve organizing code, reusing libraries, and automating builds.

In this chapter, you’ll learn how Java programs are structured—from simple packages to large, modular applications—and how to bundle and build them using modern tools like Maven and Gradle.

By the end of this chapter, you’ll be able to:

* Organize Java code using **packages**
* Understand Java’s **module system**
* Create and run **JAR files**
* Use **Maven** and **Gradle** to automate builds, manage dependencies, and streamline your workflow

---

### Structuring Java Programs

As your program grows, you’ll have multiple `.java` files. To avoid naming conflicts and keep things clean, Java uses **packages**.

Here’s an example of moving from a flat HelloWorld to a structured layout:

```
src/
└── com/
    └── example/
        └── app/
            ├── Main.java
            └── utils/
                └── GreetHelper.java
```

Inside `src/com/example/app/utils/GreetHelper.java`:

```java
package com.example.app.utils;

public class GreetHelper {
    public static void sayHello() {
        System.out.println("Hello from GreetHelper!");
    }
}
```

And in `src/com/example/app/Main.java`:

```java
package com.example.app;

import com.example.app.utils.GreetHelper;

public class Main {
    public static void main(String[] args) {
        GreetHelper.sayHello();
    }
}
```

---

### Java Modules (JPMS)

Modules are a step above packages. Introduced in Java 9, they help structure large applications with clear boundaries and strong encapsulation.

At the root of a module, you define a `src/com/example/app/module-info.java`:

```java
module com.example.app {
    exports com.example.app.utils;
}
```

This makes only `com.example.app.utils` available to other modules—everything else is hidden by default.


To Run this,

```bash
java src/com/example/app/Main.java
```

---

### Packaging Your Application into a JAR

To distribute or run your app easily, you can package it as a **JAR** (Java ARchive).

To compile al java classes, you'd use:

```bash
javac -d out src/com/example/app/Main.java src/com/example/app/utils/GreetHelper.java
```

From your compiled classes:

```bash
jar --create --file app.jar -C out .
```

To run the JAR:

```bash
java -cp app.jar com.example.app.Main
```

You can also add a manifest to make it executable:

```bash
jar --create --file app.jar --main-class com.example.app.Main -C out .
java -jar app.jar
```

---


This is how real-world Java development happenss.

