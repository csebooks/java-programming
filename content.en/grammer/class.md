---
title: 'Class'
weight: 6
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
```

You can now create objects from this class:

```java
Person p = new Person();
p.name = "Arun";
p.sayHello();  // Hello, I'm Arun
```

---

### Inheritance with `extends`

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

### `abstract` Classes

Use `abstract` when you want a base class that **can’t be instantiated** directly, but provides a template:

```java
abstract class Shape {
    abstract double area(); 
}

class Circle extends Shape {
    double radius;
    Circle(double r) { this.radius = r; }
    double area() { return Math.PI * radius * radius; }
}
```

---

### Inner Classes

Define a class **inside another**, useful when it’s tightly tied to its outer:

```java
class Outer {
    int outerValue = 10;

    class Inner {
        void show() {
            System.out.println("Outer value is " + outerValue);
        }
    }
}

// Usage:
Outer outer = new Outer();
Outer.Inner inner = outer.new Inner();
inner.show();  // Outer value is 10
```

---

### `static` Inner Classes

When your inner class doesn’t need the outer instance, mark it `static`:

```java
class Box {
    static class Dimensions {
        int width = 10, height = 20;
    }
}

// Usage:
Box.Dimensions d = new Box.Dimensions();
System.out.println(d.width);  //output: 10
```

---

### Sealed Classes (Java 17+)

Sealed classes let you **restrict which other classes** can extend or implement them, improving control over your type hierarchy:

```java
public abstract sealed class Vehicle
    permits Car, Truck {
    abstract void drive();
}

public final class Car extends Vehicle {
    void drive() { System.out.println("Car driving"); }
}

public non-sealed class Truck extends Vehicle {
    void drive() { System.out.println("Truck driving"); }
}
```

* `sealed` marks the superclass.
* `permits` lists allowed subclasses.
* Subclasses must be `final`, `sealed`, or `non-sealed`.

This gives you strong encapsulation over how your APIs evolve.

---

With simple classes, inheritance, abstract templates, inner classes, and now sealed classes, you have a full toolbox for designing robust, well-controlled Java applications.
