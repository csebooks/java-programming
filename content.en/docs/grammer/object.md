---
title: 'Object'
weight: 14
categories:
    - object
---

Almost every class in Java is a direct or indirect subclass of `java.lang.Object`. Understanding this root class—and the methods it provides—is key to mastering how Java objects behave and interact.

---

Everything you create in Java—unless you explicitly extend another class—automatically gets these methods. Let’s look at the most important ones and why they matter.

---

**`toString()`**
Returns a string that “represents” the object. By default, it prints the class name and hash code, but you almost always override it for readability.

```java
@Override
public String toString() {
    return "Person[name=" + name + ", age=" + age + "]";
}
```

Without override: `Person@5e2de80c`
With override: `Person[name=Arun, age=30]`

---

**`equals(Object other)`**
Tests whether this object is “equal” to another. The default is reference equality (`this == other`), so you override it to compare data instead.

```java
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (!(o instanceof Person)) return false;
    Person p = (Person) o;
    return age == p.age && Objects.equals(name, p.name);
}
```

When you override `equals()`, you almost always override `hashCode()` too.

---

**`hashCode()`**
Returns an integer hash code for the object, used in hash-based collections. Objects that are equal **must** have the same hash code.

```java
@Override
public int hashCode() {
    return Objects.hash(name, age);
}
```

Good `hashCode()` implementations distribute values uniformly to avoid collisions.

---

**`getClass()`**
Returns a `Class<?>` object representing the runtime class of the instance. Useful for reflection or debugging.

```java
System.out.println(obj.getClass().getName());
```

---

**`clone()`**
Creates and returns a copy of the object. The class must implement `Cloneable`, and override `clone()` with `protected Object clone() throws CloneNotSupportedException`.

```java
@Override
public Person clone() throws CloneNotSupportedException {
    return (Person) super.clone();
}
```

Because cloning is tricky, many prefer copy constructors or factory methods instead.

---

**`finalize()`** *(deprecated)*
Called by the garbage collector before reclaiming an object’s memory. It’s unpredictable, often dangerous, and deprecated in modern Java. Instead, use try-with-resources or explicit cleanup methods.

---

**Threading Helpers: `wait()`, `notify()`, `notifyAll()`**
Since every object has a monitor, these methods let threads coordinate:

```java
synchronized (shared) {
    while (!condition) {
        shared.wait();
    }
    // ...
    shared.notifyAll();
}
```

Use higher-level constructs (`Lock`, `Condition`, `Semaphore`) from `java.util.concurrent` for complex scenarios.

---

### Why It Matters

* **Consistent APIs**: Any object can be printed, compared, cloned, or used in synchronized blocks.
* **Collections**: `equals()` and `hashCode()` are the backbone of `Set`, `Map`, and other data structures.
* **Debugging**: A good `toString()` makes logs and error messages far more informative.
* **Concurrency**: The built-in monitor model is simple but limited—knowing about `wait`/`notify` helps you appreciate modern concurrency utilities.

Understanding `java.lang.Object` is like learning the alphabet of Java’s object model. Everything else builds on these fundamental methods.
