---
title: 'Operators'
weight: 3
---

Great! Let’s move to the next module in the **Grammar** chapter.

We'll now cover **Operators** — the tools used in Java to perform actions like calculations, comparisons, and logic.

As per your structure, I’ll keep the explanation simple, start with usage and examples, and not repeat the title/definition header. Here's the content:

---

Operators allow you to perform operations on values and variables. Java has a wide range of operators grouped by purpose.

---

### ➕ Arithmetic Operators

Used for basic math operations.

```java
int a = 10, b = 3;

System.out.println(a + b);  // Addition → 13
System.out.println(a - b);  // Subtraction → 7
System.out.println(a * b);  // Multiplication → 30
System.out.println(a / b);  // Division → 3
System.out.println(a % b);  // Modulus (remainder) → 1
```

---

### 🔍 Comparison Operators

Used to compare two values. These always return a boolean (`true` or `false`).

```java
int x = 5, y = 8;

System.out.println(x > y);   // false
System.out.println(x < y);   // true
System.out.println(x == y);  // false
System.out.println(x != y);  // true
System.out.println(x >= y);  // false
System.out.println(x <= y);  // true
```

---

### 🔗 Logical Operators

Used to combine multiple boolean expressions.

```java
boolean a = true;
boolean b = false;

System.out.println(a && b);  // AND → false
System.out.println(a || b);  // OR  → true
System.out.println(!a);      // NOT → false
```

---

### 🟰 Assignment Operators

Used to assign or update values in variables.

```java
int n = 10;

n += 5;  // Same as n = n + 5 → 15
n -= 3;  // Now n = 12
n *= 2;  // Now n = 24
n /= 4;  // Now n = 6
```

---

### ⚙️ Other Useful Operators

#### Increment/Decrement

```java
int i = 0;

i++;  // i becomes 1
i--;  // i goes back to 0
```

#### Ternary Operator

A shortcut for simple if-else.

```java
int age = 20;
String result = (age >= 18) ? "Adult" : "Minor";
System.out.println(result); // Adult
```

---

Java also has **bitwise**, **instanceof**, and other advanced operators, which we’ll touch upon later as needed.

Let me know if you'd like to move on to **Keywords**, **OOP Basics**, or start the next major chapter after Grammar.
