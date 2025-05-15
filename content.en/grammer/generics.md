---
title: 'Generics'
weight: 8
---

Here’s a clear, practical explanation of **Generics** in Java, fitting your style — simple, focused on why and how to use them.

---

Generics allow you to write **classes and methods that work with different types safely**, without losing type information or needing casts.

They add **type parameters** so you can reuse the same code for many data types while keeping type safety.

---

### Why Use Generics?

Without generics, you’d use raw types like `Object` and cast back and forth:

```java
List list = new ArrayList();
list.add("Hello");
String s = (String) list.get(0);  // Cast needed, unsafe
```

With generics, the compiler checks types for you:

```java
List<String> list = new ArrayList<>();
list.add("Hello");
String s = list.get(0);  // No cast needed, safer
```

---

### Using Generics in Classes

```java
class Box<T> {
    private T content;

    void set(T content) {
        this.content = content;
    }

    T get() {
        return content;
    }
}
```

Usage:

```java
Box<Integer> intBox = new Box<>();
intBox.set(123);
int value = intBox.get();
```

---

### Generics with Methods

You can create **generic methods** independent of the class type:

```java
public static <T> void printArray(T[] arr) {
    for (T item : arr) {
        System.out.println(item);
    }
}
```

---

### Bounded Type Parameters

Sometimes, you want to restrict types to a certain family (like numbers or comparable objects):

```java
class Calculator<T extends Number> {
    double square(T value) {
        return value.doubleValue() * value.doubleValue();
    }
}
```

---

### Wildcards (`?`)

Use wildcards when you want flexibility:

* `List<?>` — a list of unknown type
* `List<? extends Number>` — list of Number or its subclasses (read-only)
* `List<? super Integer>` — list of Integer or its superclasses (write allowed)

---

Generics improve code **reusability, readability, and safety** — and are essential in modern Java libraries.

Want me to continue with **Exception Handling** next?
