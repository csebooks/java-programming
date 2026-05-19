---
title: 'Optional'
weight: 1
---

`Optional<T>` is a container object introduced in Java 8 that may or may not hold a non-null value. It’s a better alternative to using `null` to represent missing values and helps write cleaner, more expressive code.

---

### Scenarios

- When a method might not return a value but you want to avoid returning `null`.
- To chain operations safely without explicit null checks.
- To make APIs more readable by expressing “optional” values clearly.

---

### Sample Usage

```java
// Creating Optional instances
Optional<String> name = Optional.of("Java");
Optional<String> empty = Optional.empty();
Optional<String> maybeNull = Optional.ofNullable(getUserInput());

// Safe access
name.ifPresent(System.out::println); // prints "Java"

String result = maybeNull.orElse("Default Value");
System.out.println(result);

// Chaining
Optional<String> upper = maybeNull.map(String::toUpperCase);
System.out.println(upper.orElse("NO VALUE"));

// Throw if missing
String mustHave = maybeNull.orElseThrow(() -> new IllegalArgumentException("Missing value"));
```