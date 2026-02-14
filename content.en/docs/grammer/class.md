Here’s the updated markdown with a **definition of inheritance**, an explanation of **why Java does not support multiple inheritance with classes**, and integration into your structure:

---

````markdown
---
title: 'Class'
weight: 6
categories:
    - class
---

> Classes are the foundation of all Java programs—your blueprint for creating objects with data (fields) and behavior (methods).

---

```java
class Person {
    String name;
    int age;

    void sayHello() {
        System.out.println("Hello, I'm " + name);
    }
}
````

You can now create objects from this class:

```java
Person p = new Person();
p.name = "Arun";
p.sayHello();  // Hello, I'm Arun
```

---

### Inheritance with `extends`

**Inheritance** is a mechanism in Java that allows one class to acquire the properties (fields) and behaviors (methods) of another class. It promotes **code reuse** and represents an **is-a relationship**.

One class can **extend** another to reuse and specialize behavior:

```java
class Employee extends Person {
    String company;

    void work() {
        System.out.println(name + " works at " + company);
    }
}
```

Now `Employee` has everything from `Person` plus its own additions.

---

### Why Java Does Not Support Multiple Inheritance (With Classes)

Java **does not support multiple inheritance with classes** (i.e., a class cannot extend more than one class) to avoid the **Diamond Problem**.

**Diamond Problem Example:**

```java
class A {
    void show() {
        System.out.println("A");
    }
}

class B extends A {
    void show() {
        System.out.println("B");
    }
}

class C extends A {
    void show() {
        System.out.println("C");
    }
}

// Problem if Java allowed this:
class D extends B, C { // Not allowed in Java
    ...
}
```

In the above scenario, if `D` calls `show()`, it's **ambiguous** whether it should use `B`'s or `C`'s version.

### Solution in Java: Interfaces

Instead of allowing multiple class inheritance, Java supports **multiple inheritance through interfaces**, which only contain method signatures and no implementation (until Java 8's default methods, which are also conflict-resolved explicitly by the programmer).

---

### `abstract` Classes

Use `abstract` when you want a base class that **can’t be instantiated** directly, but provides a template:

```java
abstract class Shape {
    abstract double area(); 
}
```

An abstract class can have both abstract methods (without body) and regular methods.

---

