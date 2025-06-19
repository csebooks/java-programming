---
title: 'Exceptions'
weight: 10
---

> Exception handling helps your program **deal with unexpected problems** (errors) without crashing abruptly. It separates normal code from error-handling code, making programs more robust and easier to maintain.

### When exception happens

1. Null object reference
2. Out of bound
3. Type Mismatch
4. Connectivity
5. Unavailable file
6. Disk space issue
7. Invalid user input
8. Arithmetic errors
9. Database errors
10. Device failures
11. Code errors

### Errors vs Exceptions
1. Error -> Cannot recover
2. Exception -> Recover by handling it 

### Types of Exceptions

1. **Checked exceptions** must be either handled or declared (`IOException`, `SQLException`).
2. **Unchecked exceptions** (runtime exceptions) like `NullPointerException`, `ArithmeticException` don’t require mandatory handling.

### How Exceptions Work

When something goes wrong, Java creates an **Exception object** and throws it. You can **catch** these exceptions to handle them gracefully.

```java
try {
    int result = 10 / 0;  // Causes ArithmeticException
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero!");
}
```

* `try` contains code that might throw exceptions.
* `catch` handles specific exceptions and prevents crashes.

### Finally Block

The `finally` block **always runs**, whether or not an exception occurs. It's typically used for **cleanup operations** like closing files, database connections, or releasing locks.

```java
FileInputStream input = null;
try {
    input = new FileInputStream("data.txt");
    // Read data
} catch (IOException e) {
    System.out.println("Error reading file");
} finally {
    if (input != null) {
        try {
            input.close();  // Cleanup
        } catch (IOException e) {
            System.out.println("Error closing file");
        }
    }
}
```

However, this approach is **verbose and error-prone**. That’s where `try-with-resources` comes in.

### Try-With-Resources (Automatic Cleanup)

`try-with-resources` lets you declare and use resources (like files, streams, connections) **that are automatically closed** at the end of the block. The resource must implement the `AutoCloseable` interface.

```java
try (FileInputStream input = new FileInputStream("data.txt")) {
    // Read data
} catch (IOException e) {
    System.out.println("Error reading file");
}
// No need for finally — input is closed automatically!
```

This is the **recommended way** to handle resource cleanup in modern Java:

* **Shorter** and **cleaner**
* Avoids forgetting to close resources
* Prevents resource leaks

Use it for:

* File and stream handling
* JDBC connections
* Socket programming
* Any custom class that implements `AutoCloseable`

### Exception Chaining (Throwing and Declaring Exceptions Together)

Sometimes you catch a **low-level exception** but want to **throw a higher-level exception** with additional context. This is called **exception chaining**, and it uses both `throw` and `throws`.

```java
void readUserData() throws UserDataReadException {
    try {
        // Simulate reading a file
        readFile(); 
    } catch (IOException e) {
        // Wrap original exception in a custom one
        throw new UserDataReadException("Failed to read user data", e);
    }
}

void readFile() throws IOException {
    throw new IOException("File not found");
}
```

In this example:

* `readFile()` throws a checked exception.
* `readUserData()` catches it and rethrows a new exception with context, preserving the original cause using `e`.

This pattern makes it easier to **debug and understand the root cause** of errors, especially in layered applications.


### Best Practices

* **Catch the most specific exception first.** Avoid catching generic `Exception` unless necessary.
* **Never swallow exceptions silently.** At least log them.
* **Use exception chaining** to retain context.
* **Clean up resources** using `finally` or try-with-resources.
* **Don’t overuse checked exceptions.** Too many can clutter your API.
* **Use custom exceptions** for meaningful domain-specific errors.

