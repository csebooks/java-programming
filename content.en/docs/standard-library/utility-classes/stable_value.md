---
title: 'StableValue'
weight: 2
---

`StableValue<T>` is not part of the Java Standard Library but represents a **design pattern** or **custom utility** used to capture and work with values that **do not change** once initialized—often as a wrapper to enforce immutability and predictability in code.

---

### Scenarios

- When you want to **explicitly express** that a value, once set, should not change.
- Useful for **configuration values**, **shared constants**, or **lazy-initialized** but stable results.
- Can be implemented as a final wrapper class or a record in Java.

---

### Sample Usage

```java
public final class StableValue<T> {
    private final T value;

    public StableValue(T value) {
        this.value = value;
    }

    public T get() {
        return value;
    }
}

// Usage
StableValue<String> apiKey = new StableValue<>("ABC123");
System.out.println(apiKey.get());  // "ABC123"
```

Or using `record` (Java 16+):

```java
public record StableValue<T>(T value) { }

// Usage
StableValue<Integer> maxConnections = new StableValue<>(10);
System.out.println(maxConnections.value()); // 10
```

This utility encourages safer design by **limiting change** and **clarifying intent** in your code.

