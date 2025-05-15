---
title: 'Data Types'
weight: 4
--- 

> Everything in a Java program works with data—numbers, characters, text, true/false flags, dates, and more. To work with these effectively, Java provides a rich set of **data types**, broadly grouped into **primitive types** and **object-based types**.

---

### 🔹 Primitive Types

Primitive types are the most basic building blocks of data in Java. They’re **efficient**, **fixed in size**, and not objects.

#### `int`

Used for whole numbers.
Example:

```java
int age = 25;
```

#### `double`

Used for decimal numbers.
Example:

```java
double price = 19.99;
```

#### `boolean`

Used for true/false values.
Example:

```java
boolean isActive = true;
```

#### `char`

Used to represent a single character.
Example:

```java
char grade = 'A';
```

Other primitive types include:

* `byte` – very small integers (-128 to 127)
* `short` – small integers
* `long` – very large integers
* `float` – less precise decimals

You’ll usually start with `int`, `double`, and `boolean` for most cases.

---

### 🔹 Object-Based Types

Java is an object-oriented language, so it also supports types that are full-fledged **objects**. These types provide **additional behavior** and **methods**.

#### `String`

Used for text data.
Example:

```java
String name = "Alice";
```

Strings come with many useful methods like `length()`, `toUpperCase()`, etc. We’ll explore Strings in detail in the upcoming chapters.

#### `Date`, `LocalDate`, `LocalDateTime`

Used to work with dates and times.
Example:

```java
LocalDate today = LocalDate.now();
```

Handling dates and times properly is important and deserves its own space—we’ll see this in a later chapter.

---

### 🔹 Auto Conversion and Wrappers

Java automatically converts between primitive types and their object counterparts when needed. This is known as **autoboxing** and **unboxing**.

Example:

```java
Integer count = 10; // int to Integer (autoboxing)
int x = count;      // Integer to int (unboxing)
```

Each primitive has a corresponding wrapper class:

* `int` → `Integer`
* `double` → `Double`
* `boolean` → `Boolean`, etc.

---

### 🧠 Summary

Java gives you the right type for every kind of data—whether it's raw performance with primitives or richer behavior with object-based types. Mastering data types early on will make your code more reliable, readable, and efficient.

Let me know if you'd like to proceed with **Operators**, **Keywords**, or jump to **Object-Oriented Programming** next.
