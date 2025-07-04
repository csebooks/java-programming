---
title: 'Text'
weight: 2
categories:
    - text
--- 

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


#### String

Strings lie at the heart of almost every Java program. You’ll learn how they’re represented under the hood, how to build and manipulate them efficiently, and the rich set of tools Java provides for searching, splitting, and templating text.

---

### Internal Representation

* Java 8 and earlier: a `char[]` array storing UTF-16 code units.
* Java 9+: a compact `byte[]` plus a `coder` flag (LATIN1 vs. UTF-16), saving memory for ASCII-heavy text.
* Immutable: once created, the contents never change; concatenation always produces a new object.

---

### StringBuilder

* **Why:** Fast, mutable sequence of characters—ideal for repeated concatenation.
* **How:**

  ```java
  StringBuilder sb = new StringBuilder();
  sb.append("Hello");
  sb.append(", ").append("World!");
  String result = sb.toString();
  ```
* **When:** In loops or any situation where `+` would create many temporary `String` objects.

---

### Regular Expressions (RegEx)

* **API:** `java.util.regex.Pattern` & `Matcher`.
* **Pattern.compile** creates a regex; `matcher()` applies it to input.

  ```java
  Pattern p = Pattern.compile("\\d{3}-\\d{2}-\\d{4}");
  Matcher m = p.matcher("123-45-6789");
  if (m.matches()) { /* valid SSN format */ }
  ```
* **Use cases:** Validation (emails, phone numbers), search/replace (`Matcher.replaceAll`), complex parsing.

### String Templating (`String.format`)

* **Why:** Inject variables into a template without manual concatenation.
* **Syntax:** Format specifiers like `%s`, `%d`, `%f`.

  ```java
  String template = "User %s has %d new messages.";
  String msg = String.format(template, "Alice", 5);
  // "User Alice has 5 new messages."
  ```
* **Advanced:** Use `java.text.MessageFormat` for locale-aware templating and named parameters.

With this toolkit—knowing how strings live in memory, when to use mutable builders, how to leverage regex, and how to split or template text—you’ll handle almost any textual requirement efficiently and cleanly.

### Escape Sequences

In Java, special character sequences like `\n`, `\t`, etc., are called **escape sequences**.

> **Escape sequences** are used in Java (and many programming languages) to represent special characters that cannot be typed directly or would be interpreted differently in a string or character literal.

Each escape sequence starts with a **backslash (`\`)**, which tells the compiler to interpret the following character(s) in a special way.


| Escape Sequence | Name              | Meaning                                                                                       |
| --------------- | ----------------- | --------------------------------------------------------------------------------------------- |
| `\n`            | Newline           | Moves the cursor to the next line                                                             |
| `\t`            | Tab               | Inserts a horizontal tab                                                                      |
| `\b`            | Backspace         | Moves the cursor one position back                                                            |
| `\r`            | Carriage Return   | Moves the cursor to the beginning of the line                                                 |
| `\'`            | Single Quote      | Inserts a single quote character                                                              |
| `\"`            | Double Quote      | Inserts a double quote character                                                              |
| `\\`            | Backslash         | Inserts a backslash (`\`)                                                                     |
| `\f`            | Form Feed         | Advances the printer to the next page (rarely used today)                                     |
| `\uXXXX`        | Unicode Character | Represents a Unicode character using its hexadecimal code (e.g., `\u0B85` for Tamil letter அ) |


```java
public class EscapeDemo {
    public static void main(String[] args) {
        System.out.println("Line1\nLine2");      // Prints in two lines
        System.out.println("Name:\tSathish");    // Adds tab space
        System.out.println("She said: \"Hello\"");// Prints double quotes
        System.out.println("Path: C:\\Users\\");  // Prints backslash
    }
}
```


