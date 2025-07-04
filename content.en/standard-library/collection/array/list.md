---
title: 'List'
weight: 1
categories:
    - list
---

# List Interface in Java

The `List` interface is a part of the Java Collections Framework and represents an ordered collection that allows duplicate elements. Elements are stored in sequence, and each has a zero-based index.

---

## Characteristics

- Maintains insertion order
- Allows duplicate elements
- Supports positional access using indexes
- Accepts null elements (depending on implementation)

---

## Interface Hierarchy

```

Collection<E>
|
List<E>
|
+-- ArrayList<E>
+-- LinkedList<E>
+-- Vector<E>
+-- CopyOnWriteArrayList<E>

````

---

## Common Implementations

### ArrayList

- Backed by a dynamic array
- Allows fast random access (O(1))
- Insertions and removals in the middle are slower (O(n))

```java
List<String> names = new ArrayList<>();
names.add("Alice");
names.add("Bob");
System.out.println(names.get(0)); // Alice
````

### LinkedList

* Doubly-linked list
* Efficient for insertions/deletions at the beginning or end
* Slower for indexed access (O(n))

```java
List<String> items = new LinkedList<>();
items.add("First");
items.add("Second");
items.add(0, "Inserted");
System.out.println(items); // [Inserted, First, Second]
```

### Vector

* Legacy implementation
* Synchronized version of ArrayList
* Generally replaced by `Collections.synchronizedList()`

### CopyOnWriteArrayList

* Thread-safe variant of ArrayList
* Ideal for read-heavy scenarios where the list is rarely modified
* Expensive write operations (due to array copy)

---

## Core Methods

| Method                | Description                |
| --------------------- | -------------------------- |
| `add(E e)`            | Adds an element            |
| `add(int, E)`         | Inserts element at index   |
| `get(int)`            | Retrieves element at index |
| `set(int, E)`         | Replaces element at index  |
| `remove(int)`         | Removes element at index   |
| `contains(Object)`    | Checks presence of element |
| `indexOf(Object)`     | First occurrence index     |
| `lastIndexOf(Object)` | Last occurrence index      |
| `size()`              | Number of elements         |
| `clear()`             | Removes all elements       |

---

## Convenient List Methods

### List.of(...)

Creates an immutable list with predefined elements.

```java
List<String> fruits = List.of("Apple", "Banana", "Mango");
```

* Immutable
* Fixed size
* Nulls not allowed

### Collections.emptyList()

Returns an empty immutable list.

```java
List<String> empty = Collections.emptyList();
```

### Collections.singletonList(...)

Creates a list with a single immutable element.

```java
List<String> oneItem = Collections.singletonList("OnlyOne");
```

### Arrays.asList(...)

Creates a fixed-size list backed by an array.

```java
List<Integer> numbers = Arrays.asList(10, 20, 30);
```

* Modifications to the list affect the underlying array

### List.copyOf(...)

Creates an immutable copy of an existing collection.

```java
List<String> copy = List.copyOf(fruits);
```

---

## Performance Summary

| Operation            | ArrayList | LinkedList | CopyOnWriteArrayList |
| -------------------- | --------- | ---------- | -------------------- |
| Access (get/set)     | O(1)      | O(n)       | O(1)                 |
| Insert/Delete at end | O(1)      | O(1)       | O(n)                 |
| Insert/Delete middle | O(n)      | O(n)       | O(n)                 |
| Thread-safe          | No        | No         | Yes                  |

---

## When to Use

* Use `ArrayList` for most general-purpose needs
* Use `LinkedList` for frequent insertions/removals at head or tail
* Use `CopyOnWriteArrayList` for read-heavy concurrent use
* Avoid `Vector` in modern Java

---

## Example

```java
List<String> names = new ArrayList<>();
names.add("John");
names.add("Jane");
names.add(1, "Doe");

System.out.println(names);         // [John, Doe, Jane]
System.out.println(names.size());  // 3
System.out.println(names.get(2));  // Jane
```

---

## Summary

The `List` interface is one of the most versatile and commonly used collection types in Java. Choosing the right implementation depends on whether you prioritize access speed, memory efficiency, or thread safety. Java also provides several convenient factory methods to create immutable and thread-safe lists easily.

By understanding `List` thoroughly, you can write cleaner, faster, and more maintainable Java code.

```

---

Let me know if you want:
- A matching design for `Set`, `Map`, or `Queue`
- Dark-mode CSS suggestions if you're rendering this on a static site
- PDF/print-optimized version of this document
```
