---
title: 'Control Statements'
weight: 2
--- 

> Control statements are the part of Java that lets your program **make decisions** and **repeat tasks**. They help control **how and when** certain pieces of code run.

Java offers three main categories of control statements:

* **Decision-making** (e.g., `if`, `switch`)
* **Loops** (e.g., `for`, `while`, `do-while`)
* **Branching** (e.g., `break`, `continue`, `return`)

---

### Decision-Making Statements

#### 1. `if` and `else`

**Use:** To execute code conditionally.

```java
int score = 85;

if (score >= 90) {
    System.out.println("Grade: A");
} else if (score >= 75) {
    System.out.println("Grade: B");
} else {
    System.out.println("Grade: C or below");
}
```

#### 2. `switch`

**Use:** A cleaner alternative to multiple `if-else` checks on the same variable.

```java
String day = "MONDAY";

switch (day) {
    case "MONDAY":
        System.out.println("Start of the week");
        break;
    case "FRIDAY":
        System.out.println("Almost weekend");
        break;
    default:
        System.out.println("Midweek day");
}
```

From Java 14+, you can use **enhanced switch expressions**:

```java
String result = switch (day) {
    case "MONDAY" -> "Start of the week";
    case "FRIDAY" -> "Almost weekend";
    default -> "Midweek day";
};
```

---

### Looping Statements

#### 1. `for` loop

**Use:** To run a block of code a fixed number of times.

```java
for (int i = 0; i < 5; i++) {
    System.out.println("Count: " + i);
}
```

#### 2. `while` loop

**Use:** Runs as long as a condition is true.

```java
int i = 0;
while (i < 3) {
    System.out.println("i = " + i);
    i++;
}
```

#### 3. `do-while` loop

**Use:** Similar to `while`, but runs **at least once**.

```java
int i = 0;
do {
    System.out.println("i = " + i);
    i++;
} while (i < 3);
```

---

### Branching Statements

#### 1. `break`

**Use:** Exits a loop or switch early.

```java
for (int i = 1; i <= 5; i++) {
    if (i == 3) break;
    System.out.println(i);
}
```

#### 2. `continue`

**Use:** Skips the current iteration of a loop.

```java
for (int i = 1; i <= 5; i++) {
    if (i == 3) continue;
    System.out.println(i);
}
```

#### 3. `return`

**Use:** Exits from a method.

```java
public int sum(int a, int b) {
    return a + b;
}
```

---

### Summary

Control statements are the **logic muscles** of your program. They let your code think, decide, and repeat—all the essentials of real-world programming. With these tools, your programs can interact with input, respond to conditions, and run smartly.

