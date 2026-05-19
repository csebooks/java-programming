---
title: 'Integer'
weight: 1
categories:
    - integer
---



> Java supports multiple **integer types** to represent whole numbers. These types differ in **memory size**, **value range**, and **performance**, allowing developers to choose the right one based on the data requirements.

---

###  Overview of Integer Types

| Type         | Size     | Range                                                 | Default Value     | Object Wrapper     |
|--------------|----------|--------------------------------------------------------|-------------------|---------------------|
| `byte`       | 1 byte   | -128 to 127                                            | `0`               | `Byte`              |
| `short`      | 2 bytes  | -32,768 to 32,767                                      | `0`               | `Short`             |
| `int`        | 4 bytes  | -2,147,483,648 to 2,147,483,647                        | `0`               | `Integer`           |
| `long`       | 8 bytes  | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807| `0L`              | `Long`              |
| `BigInteger` | Varies   | Virtually unlimited (depends on system memory)         | `BigInteger.ZERO` | `BigInteger` (class)|

---

##  `int`: Standard Integer

The most commonly used integer type for general whole number operations.

```java
int age = 25;
int marks = -100;
````

*  Good performance and memory efficiency
*  Be careful of overflow for very large values

---

##  `long`: Large-Scale Integer

Used for storing very large whole numbers, such as timestamps or IDs.

```java
long worldPopulation = 8000000000L;  // Note the 'L' suffix
```

*  Stores massive values beyond `int` range
*  Always use `L` suffix to avoid confusion

---

##  `short`: Compact Integer

A small-range integer, typically used where memory is tight (e.g., embedded systems, large arrays).

```java
short temperature = -200;
short port = 8080;
```

*  Saves memory (2 bytes per value)
*  Not ideal for arithmetic or general-purpose values

---

##  `BigInteger`: Arbitrary Precision Integer

Used when integer values exceed the range of `long`. Found in `java.math`.

```java
import java.math.BigInteger;

BigInteger big = new BigInteger("999999999999999999999");
BigInteger result = big.multiply(BigInteger.valueOf(2));
```

*  No upper limit—perfect for scientific or financial applications
*  Slower, and requires method calls (not `+`, `-`, etc.)

---

###  Summary Table

| Type         | Use Case                              | Pros                   | Cons                    |
| ------------ | ------------------------------------- | ---------------------- | ----------------------- |
| `int`        | Default for numeric values            | Fast, memory-efficient | Limited to \~2 billion  |
| `long`       | Timestamps, large counters            | Huge range             | Slightly more memory    |
| `short`      | Compact data (e.g., network packets)  | Smaller size           | Rarely used in practice |
| `BigInteger` | Cryptography, finance, math libraries | Virtually unlimited    | Slower, verbose syntax  |

---

###  Best Practices

* Prefer `int` unless your use case demands larger or smaller types.
* Use `long` for timestamps, durations, or unique IDs.
* Use `BigInteger` for extreme numerical precision.
* Avoid `short` unless optimizing for memory or dealing with legacy APIs.
* Always check for overflow conditions in calculations.

---

### Example: Choosing the Right Integer Type

```java
int studentCount = 3000;
long timestamp = System.currentTimeMillis();
BigInteger balance = new BigInteger("123456789012345678901234567890");
```

Use the smallest appropriate type for clarity and efficiency.


```
