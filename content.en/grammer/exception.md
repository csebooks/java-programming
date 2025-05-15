---
title: 'Exceptions'
weight: 10
---

Exception handling helps your program **deal with unexpected problems** (errors) without crashing abruptly.

It separates normal code from error-handling code, making programs more robust and easier to maintain.

---

### How Exceptions Work

When something goes wrong, Java creates an **Exception object** and throws it.

You can **catch** these exceptions to handle them gracefully.

---

### Try-Catch Block

```java
try {
    int result = 10 / 0;  // This will cause ArithmeticException
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero!");
}
```

`try` contains code that might throw exceptions.

`catch` handles specific exceptions.

---

### Finally Block

Runs **always**, whether or not an exception occurs.

Good for cleanup tasks like closing files or connections.

```java
try {
    // code
} catch (Exception e) {
    // handle exception
} finally {
    System.out.println("This runs no matter what.");
}
```

---

### Throwing Exceptions

You can throw exceptions yourself using `throw`:

```java
if (age < 18) {
    throw new IllegalArgumentException("Age must be 18 or older");
}
```

---

### Checked vs Unchecked Exceptions

* **Checked exceptions** must be handled or declared (`IOException`, `SQLException`).
* **Unchecked exceptions** (runtime exceptions) like `NullPointerException` don’t require mandatory handling.

---

### Declaring Exceptions

Use `throws` in method signature to declare checked exceptions:

```java
void readFile() throws IOException {
    // code that might throw IOException
}
```

---

Exception handling lets your programs be more **stable, user-friendly, and maintainable**.

