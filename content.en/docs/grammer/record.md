---
title: 'Record'
weight: 11
categories:
    - record
---

> Records are a **compact way to create data-carrying classes** — like a class with only fields and a constructor, no extra code.Records keep your code **clean, short, and safe** — especially in modern Java where immutability and readability are valued.

They are useful when you just want to store and pass data without writing boilerplate code.

### Traditional Way

```java
class Point {
    int x;
    int y;

    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    int x() { return x; }
    int y() { return y; }

    public String toString() {
        return "Point[x=" + x + ", y=" + y + "]";
    }

    // equals() and hashCode() usually added too
}
```

This is a lot of repetitive code — even for simple data holders.

### With Records

```java
record Point(int x, int y) {}
```

That’s it!

It auto-generates:

* Constructor
* `x()` and `y()` accessors
* `toString()`
* `equals()` and `hashCode()`

Usage:

```java
Point p = new Point(3, 4);
System.out.println(p.x());  // 3
System.out.println(p);      // Point[x=3, y=4]
```

---

### Immutable by Design

Records are **immutable** — you can’t change values after creation:

```java
Point p = new Point(1, 2);
// p.x = 5;  //  Not allowed
```

This makes them ideal for value objects, like coordinates, configuration data, responses, etc.

---

### Can Have Methods Too

```java
record Point(int x, int y) {
    int distanceFromOrigin() {
        return (int) Math.sqrt(x * x + y * y);
    }
}
```

---

### What You *Can't* Do with Records

* Can’t extend another class
* All fields are final
* Mainly meant for plain data — not behavior-heavy classes

### 🧩 Compact Constructors

> A **compact constructor** in a Java `record` is a special kind of constructor that allows you to add validation or custom logic **without having to reassign fields manually**. It avoids the boilerplate code of repeating parameter declarations and assignments.

### Why Use Compact Constructors?

When you want to:

* **Validate** field values
* **Normalize** inputs
* **Throw exceptions** if fields are invalid

And you want to **avoid** this repetition:

```java
public record User(String name, int age) {
    public User(String name, int age) {
        if (age < 0) throw new IllegalArgumentException("Age must be positive");
        this.name = name; // redundant
        this.age = age;   // redundant
    }
}
```

### Compact Constructor Syntax

```java
public record User(String name, int age) {
    public User {
        if (age < 0) throw new IllegalArgumentException("Age must be positive");
    }
}
```

* You **don’t declare parameters**
* You **don’t assign fields**
* Java assigns fields automatically from the record header

### Rules

* Parameter names in the compact constructor **must match** the record components
* Fields are **implicitly assigned** (`this.name = name`, etc.)
* You can’t assign fields yourself inside a compact constructor

```java
public record Email(String address) {
    public Email {
        if (!address.contains("@")) {
            throw new IllegalArgumentException("Invalid email address");
        }
    }
}
```


### 💡 When *Not* to Use Compact Constructors

* When you need to compute or modify field values (e.g., assign `this.name = name.toUpperCase()`)
* In that case, use a **canonical constructor** (with parameters and assignments)


