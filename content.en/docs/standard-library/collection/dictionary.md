---
title: 'Dictionary'
weight: 2
categories:
    - dictionary
---


# Map Interface in Java

The `Map` interface represents a collection of **key-value pairs**. Unlike `List` or `Set`, a `Map` is not a true collection but an essential part of the Java Collections Framework.

Each key is unique, and each key maps to at most one value.

---

## Characteristics

- Stores entries as `key -> value` pairs
- Keys are unique; values can be duplicated
- Allows `null` keys and/or values (depends on implementation)
- Fast lookup by key

---

## Interface Hierarchy

```

Map\<K, V>
|
+-- HashMap\<K, V>
+-- LinkedHashMap\<K, V>
+-- TreeMap\<K, V>
+-- EnumMap\<K extends Enum<K>, V>
+-- WeakHashMap\<K, V>
+-- IdentityHashMap\<K, V>
+-- ConcurrentHashMap\<K, V>

````

---

## Common Implementations

### HashMap

- Unordered map
- Allows one `null` key and multiple `null` values
- Fast average performance (O(1) for get/put)

```java
Map<String, String> capitals = new HashMap<>();
capitals.put("India", "New Delhi");
capitals.put("USA", "Washington");
System.out.println(capitals.get("India")); // New Delhi
````

### LinkedHashMap

* Maintains insertion order
* Useful when predictable iteration order is needed

```java
Map<Integer, String> map = new LinkedHashMap<>();
map.put(1, "One");
map.put(2, "Two");
System.out.println(map); // {1=One, 2=Two}
```

### TreeMap

* Sorted by natural order or custom comparator
* Does not allow `null` keys

```java
Map<Integer, String> sorted = new TreeMap<>();
sorted.put(20, "B");
sorted.put(10, "A");
System.out.println(sorted); // {10=A, 20=B}
```

### EnumMap

* Special map for enum keys
* Very efficient, both in time and memory

```java
enum Status { SUCCESS, ERROR }
Map<Status, String> messages = new EnumMap<>(Status.class);
messages.put(Status.SUCCESS, "Operation completed");
```

### ConcurrentHashMap

* Thread-safe version of HashMap
* Allows concurrent reads and updates

```java
Map<String, Integer> scores = new ConcurrentHashMap<>();
scores.put("Alice", 100);
scores.put("Bob", 90);
```

---

## Core Methods

| Method                  | Description                              |
| ----------------------- | ---------------------------------------- |
| `put(K, V)`             | Inserts a key-value pair                 |
| `get(Object key)`       | Returns value mapped to key              |
| `remove(Object key)`    | Removes the entry with the specified key |
| `containsKey(Object)`   | Checks if a key exists                   |
| `containsValue(Object)` | Checks if a value exists                 |
| `keySet()`              | Returns a `Set` of all keys              |
| `values()`              | Returns a `Collection` of all values     |
| `entrySet()`            | Returns a `Set` of key-value pairs       |
| `size()`                | Returns number of key-value pairs        |
| `clear()`               | Removes all entries                      |

---

## Convenient Map Methods

### Map.of(...)

Creates a small, immutable map.

```java
Map<String, String> countryCodes = Map.of(
    "IN", "India",
    "US", "United States"
);
```

* Immutable
* Throws exception if keys or values are null
* Up to 10 pairs allowed in `Map.of(...)`, beyond that use `Map.ofEntries(...)`

### Collections.emptyMap()

Returns an unmodifiable, empty map.

```java
Map<String, String> empty = Collections.emptyMap();
```

### Collections.singletonMap(...)

Returns a map with a single entry.

```java
Map<String, String> single = Collections.singletonMap("ID", "12345");
```

### Map.copyOf(...)

Creates an immutable copy of another map.

```java
Map<String, String> copied = Map.copyOf(countryCodes);
```

---

## Performance Comparison

| Implementation    | Ordering        | Null Keys | Thread-Safe | Lookup Time |
| ----------------- | --------------- | --------- | ----------- | ----------- |
| HashMap           | No order        | Yes       | No          | O(1)        |
| LinkedHashMap     | Insertion order | Yes       | No          | O(1)        |
| TreeMap           | Sorted by key   | No        | No          | O(log n)    |
| EnumMap           | Enum ordinal    | No        | No          | O(1)        |
| ConcurrentHashMap | No order        | No        | Yes         | O(1)        |

---

## When to Use

* Use `HashMap` for general-purpose key-value storage
* Use `LinkedHashMap` when iteration order matters
* Use `TreeMap` when sorted keys are required
* Use `EnumMap` with enum-based keys for performance
* Use `ConcurrentHashMap` for thread-safe applications

---

## Example

```java
Map<Integer, String> days = new HashMap<>();
days.put(1, "Sunday");
days.put(2, "Monday");
days.put(3, "Tuesday");

System.out.println(days.get(2));       // Monday
System.out.println(days.containsKey(3)); // true
System.out.println(days.size());       // 3
```

---

## Summary

The `Map` interface provides a powerful way to model key-value data in Java. With various implementations offering different trade-offs in ordering, performance, and thread-safety, `Map` is a core part of efficient and clean Java application design.

Understanding how to use and choose the right Map implementation is essential for writing scalable and readable code.

```
