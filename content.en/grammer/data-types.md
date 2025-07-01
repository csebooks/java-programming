---
title: 'Data Types'
weight: 4
categories:
    - data-types
--- 

> Everything in a Java program works with data—numbers, characters, text, true/false flags, dates, and more. To work with these effectively, Java provides a rich set of **data types**, broadly grouped into **primitive types** and **object-based types**.

---

### Primitive Types

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
In Java, the number of **bytes** a `char`, `String`, or any text content (like a chat message) takes depends on **character encoding**. Here's a breakdown:

---

### **Java `char` Size**

* A `char` in Java is always **2 bytes** (16 bits).
* Java uses **UTF-16** internally for `char` and `String`.

---

### **Estimating Chat Message Size in Bytes**

Let’s say your chat message is stored as a `String`:

```java
String chat = "DIY Spring JDBC Client ...";
```

#### Option A: **Memory Size in Java (UTF-16)**

* Each character takes **2 bytes**.
* So:

  ```java
  int bytes = chat.length() * 2;
  ```

#### Option B: **Storage or Network Size (Encoded like UTF-8 or UTF-16)**

* When sending or saving (e.g., over network or file), `String` is encoded using a charset (e.g., UTF-8 or UTF-16).
* Use:

  ```java
  byte[] utf8Bytes = chat.getBytes(StandardCharsets.UTF_8);
  int size = utf8Bytes.length;
  ```

---

### Example

```java
String chat = "DIY Spring JDBC Client";
System.out.println("UTF-8 Bytes: " + chat.getBytes(StandardCharsets.UTF_8).length);
System.out.println("UTF-16 Bytes: " + chat.getBytes(StandardCharsets.UTF_16).length);
```

#### Output:

```
UTF-8 Bytes: 24
UTF-16 Bytes: 50
```

Note: UTF-16 adds a **2-byte BOM (Byte Order Mark)** in many cases, hence 50 instead of 48.



| Context         | Encoding | Size per char | Notes                                |
| --------------- | -------- | ------------- | ------------------------------------ |
| In Java memory  | UTF-16   | 2 bytes       | Fixed per char                       |
| UTF-8 encoding  | UTF-8    | 1–4 bytes     | ASCII chars take 1 byte, others more |
| UTF-16 encoding | UTF-16   | 2–4 bytes     | Includes BOM if written to file      |

Other primitive types include:

* `byte` – very small integers (-128 to 127)
* `short` – small integers
* `long` – very large integers
* `float` – less precise decimals

You’ll usually start with `int`, `double`, and `boolean` for most cases.

---

### Object-Based Types

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

### Auto Conversion and Wrappers

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

### Summary

Java gives you the right type for every kind of data—whether it's raw performance with primitives or richer behavior with object-based types. Mastering data types early on will make your code more reliable, readable, and efficient.

In Java, the number of **bytes** a `char`, `String`, or any text content (like a chat message) takes depends on **character encoding**. Here's a breakdown:

---

### **Java `char` Size**

* A `char` in Java is always **2 bytes** (16 bits).
* Java uses **UTF-16** internally for `char` and `String`.

---

### **Estimating Chat Message Size in Bytes**

Let’s say your chat message is stored as a `String`:

```java
String chat = "DIY Spring JDBC Client ...";
```

#### Option A: **Memory Size in Java (UTF-16)**

* Each character takes **2 bytes**.
* So:

  ```java
  int bytes = chat.length() * 2;
  ```

#### Option B: **Storage or Network Size (Encoded like UTF-8 or UTF-16)**

* When sending or saving (e.g., over network or file), `String` is encoded using a charset (e.g., UTF-8 or UTF-16).
* Use:

  ```java
  byte[] utf8Bytes = chat.getBytes(StandardCharsets.UTF_8);
  int size = utf8Bytes.length;
  ```

---

### Example

```java
String chat = "DIY Spring JDBC Client";
System.out.println("UTF-8 Bytes: " + chat.getBytes(StandardCharsets.UTF_8).length);
System.out.println("UTF-16 Bytes: " + chat.getBytes(StandardCharsets.UTF_16).length);
```

#### Output:

```
UTF-8 Bytes: 24
UTF-16 Bytes: 50
```

Note: UTF-16 adds a **2-byte BOM (Byte Order Mark)** in many cases, hence 50 instead of 48.



| Context         | Encoding | Size per char | Notes                                |
| --------------- | -------- | ------------- | ------------------------------------ |
| In Java memory  | UTF-16   | 2 bytes       | Fixed per char                       |
| UTF-8 encoding  | UTF-8    | 1–4 bytes     | ASCII chars take 1 byte, others more |
| UTF-16 encoding | UTF-16   | 2–4 bytes     | Includes BOM if written to file      |

