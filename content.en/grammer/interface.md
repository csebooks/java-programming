---
title: 'Interface'
weight: 7
---

Here’s a clean, practical walkthrough of **interfaces** in Java — focused on real usage, with key concepts like default methods, functional interfaces, and marker interfaces. It fits smoothly with the tone of your previous grammar sections.

---

Interfaces define a **contract** — a set of methods that a class promises to implement. They help build flexible and loosely coupled systems.

---

### ✍️ Declaring an Interface

```java
interface Animal {
    void makeSound();
}
```

A class implements it like this:

```java
class Dog implements Animal {
    public void makeSound() {
        System.out.println("Woof");
    }
}
```

---

### ✅ Default Methods

Java allows interfaces to have **default implementations**.

```java
interface Logger {
    default void log(String msg) {
        System.out.println("LOG: " + msg);
    }
}
```

A class can use it as-is or override it:

```java
class MyLogger implements Logger {
    // log() method inherited
}
```

Default methods allow interfaces to **evolve** over time without breaking old code.

---

### 🎯 Functional Interfaces

A functional interface has **exactly one abstract method** and is used in lambda expressions or method references.

```java
@FunctionalInterface
interface Calculator {
    int compute(int a, int b);
}
```

Usage with lambda:

```java
Calculator add = (a, b) -> a + b;
System.out.println(add.compute(2, 3));  // 5
```

You can omit `@FunctionalInterface`, but it's helpful to signal intent and catch mistakes early.

---

### 🏷 Marker Interfaces

Marker interfaces **have no methods at all**. They are used to "mark" a class with a special property, often recognized by frameworks or runtime checks.

Example: `java.io.Serializable`

```java
class Book implements java.io.Serializable {
    String title;
}
```

Here, the presence of `Serializable` tells Java that `Book` objects can be serialized — no methods needed.

You can also define your own:

```java
interface Important {}

class Report implements Important {
    // Just marked, no method to implement
}
```

---

Interfaces are a key part of Java's design. With features like **default methods** and **lambdas**, modern Java makes interfaces more powerful and flexible than ever.


