---
title: 'Set'
weight: 2
categories:
    - set
---



# Set Interface in Java

The `Set` interface is a collection that **does not allow duplicate elements**. It models a mathematical set and is mainly used when **uniqueness** is required.

---

## Characteristics

- No duplicate elements
- Does not maintain index-based order
- Allows at most one `null` element (varies by implementation)
- Offers fast membership checking (`contains()`)

---

## Interface Hierarchy

```

Collection<E>
|
Set<E>
|
+-- HashSet<E>
+-- LinkedHashSet<E>
+-- TreeSet<E>
+-- EnumSet<E>

````

---

## Common Implementations

### HashSet

- Backed by a `HashMap`
- No guaranteed order
- Constant-time performance for basic operations (O(1))

```java
Set<String> colors = new HashSet<>();
colors.add("Red");
colors.add("Green");
colors.add("Red"); // Duplicate ignored
System.out.println(colors); // [Red, Green]
````

### LinkedHashSet

* Maintains insertion order
* Slightly slower than `HashSet` due to ordering overhead

```java
Set<String> names = new LinkedHashSet<>();
names.add("Alice");
names.add("Bob");
System.out.println(names); // [Alice, Bob]
```

### TreeSet

* Sorted in natural order or by custom comparator
* No null elements allowed
* Backed by a `TreeMap`

```java
Set<Integer> numbers = new TreeSet<>();
numbers.add(10);
numbers.add(5);
numbers.add(20);
System.out.println(numbers); // [5, 10, 20]
```

### EnumSet

* Specialized for `enum` types
* Very fast and memory-efficient
* All elements must be of a single enum type

```java
enum Role { ADMIN, USER, GUEST }
Set<Role> roles = EnumSet.of(Role.ADMIN, Role.USER);
```

---

## Core Methods

| Method               | Description                            |
| -------------------- | -------------------------------------- |
| `add(E e)`           | Adds an element if not already present |
| `remove(Object o)`   | Removes the specified element          |
| `contains(Object o)` | Checks if element exists               |
| `size()`             | Returns the number of elements         |
| `clear()`            | Removes all elements                   |
| `isEmpty()`          | Checks if the set is empty             |
| `iterator()`         | Iterates through the set               |

---

## Convenient Set Methods

### Set.of(...)

Creates an immutable set.

```java
Set<String> tags = Set.of("A", "B", "C");
```

* Throws `UnsupportedOperationException` if modified
* Throws `IllegalArgumentException` if duplicates are present

### Collections.emptySet()

Returns a shared immutable empty set.

```java
Set<String> empty = Collections.emptySet();
```

### Collections.singleton(...)

Creates an immutable set with one element.

```java
Set<String> onlyOne = Collections.singleton("ROOT");
```

### Set.copyOf(...)

Creates an immutable copy of another set.

```java
Set<String> copy = Set.copyOf(tags);
```

---

## Performance Comparison

| Implementation | Ordering        | Null Allowed | Thread-safe | Lookup Time |
| -------------- | --------------- | ------------ | ----------- | ----------- |
| HashSet        | No order        | Yes          | No          | O(1)        |
| LinkedHashSet  | Insertion order | Yes          | No          | O(1)        |
| TreeSet        | Sorted          | No           | No          | O(log n)    |
| EnumSet        | Enum-defined    | No           | No          | O(1)        |

---

## When to Use

* Use `HashSet` for fast and unordered uniqueness
* Use `LinkedHashSet` when order of insertion matters
* Use `TreeSet` when sorted data is needed
* Use `EnumSet` for enum-based sets with performance in mind

---

## Example

```java
Set<String> uniqueNames = new HashSet<>();
uniqueNames.add("Alice");
uniqueNames.add("Bob");
uniqueNames.add("Alice"); // ignored

System.out.println(uniqueNames.contains("Bob")); // true
System.out.println(uniqueNames.size());          // 2
```

---

## Summary

The `Set` interface is essential when you need to enforce uniqueness in your data. Each implementation provides a different trade-off in terms of performance, ordering, and memory usage. Choosing the right `Set` can lead to cleaner and more efficient code.

By understanding and applying the right `Set` type, you eliminate redundancy and write logic that naturally enforces data integrity.

```


