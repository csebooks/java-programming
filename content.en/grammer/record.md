---
title: 'Record'
weight: 11
categories:
    - record
---

Records are a **compact way to create data-carrying classes** — like a class with only fields and a constructor, no extra code.

They are useful when you just want to store and pass data without writing boilerplate code.

---

### Traditional Way (Before Records)

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

---

### With Records (Java 14+)

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

---

Records keep your code **clean, short, and safe** — especially in modern Java where immutability and readability are valued.