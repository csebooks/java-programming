---
title: 'Collection Framework'
weight: 5
references:
    links:
        - https://dev.java/learn/api/collections-framework/
---

> Java’s Collection Framework is a powerful suite of interfaces and classes that help you store, access, and manage groups of data efficiently. Instead of building your own data structures, Java provides well-tested implementations for Lists, Sets, Maps, Queues, and more — allowing you to focus on logic, not plumbing.

It supports a variety of needs — ordering, uniqueness, sorting, concurrency, and parallelism — all with unified access patterns.

---

### Interface Hierarchy

```goat
                    Iterable<T>
                        |
        +---------------+----------------+
        |                                |
    Collection<T>                      Map<K, V>
        |
  +-----+------+----------+----------------+
  |            |          |                |
List<T>     Set<T>    Queue<T>     SequencedCollection<T>
              |                          |
         SortedSet<T>                (New in Java 21+)
              |
         NavigableSet<T>

Map<K, V>
    |
SortedMap<K, V>
    |
NavigableMap<K, V>
    |
SequencedMap<K, V>  (Java 21+)
```

This foundation lets you choose the right structure based on your needs — whether it’s performance, thread-safety, or ordering.

---

### Notable Implementations and Their Purpose

#### Lists

* **ArrayList** – Fast random access, backed by an array. Good for general-purpose use.
* **LinkedList** – Efficient inserts/removals at both ends; useful for queues and stacks.
* **Vector** – Synchronized version of ArrayList (mostly legacy).
* **CopyOnWriteArrayList** – Thread-safe and immutable during iteration; ideal for concurrent reads.

#### Sets

* **HashSet** – Unordered, no duplicates. Backed by a `HashMap`.
* **LinkedHashSet** – Preserves insertion order.
* **TreeSet** – Sorted set using natural order or custom comparator.
* **EnumSet** – High-performance set for enum types.

#### Queues

* **PriorityQueue** – Elements ordered by priority.
* **ArrayDeque** – Efficient double-ended queue.
* **LinkedBlockingQueue**, **ConcurrentLinkedQueue** – Concurrency-safe queue implementations.

#### Maps

* **HashMap** – Unordered key-value pairs.
* **LinkedHashMap** – Preserves insertion order.
* **TreeMap** – Sorted map by keys.
* **EnumMap** – Efficient map for enum keys.
* **ConcurrentHashMap** – Thread-safe alternative to HashMap.
* **WeakHashMap**, **IdentityHashMap** – Specialized behavior with reference-based keys.