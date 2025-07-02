---
title: 'Boolean'
weight: 1
categories:
    - numbers
--- 

Here is the complete **`boolean.md`** file for the `datatype -> boolean` path in your structure:

````markdown
---
title: 'Boolean Type in Java'
weight: 3
categories:
  - data-types
  - boolean
---

> In Java, the `boolean` data type represents only two values: `true` or `false`. It's the foundation for all **decision-making**, **conditions**, and **logical operations** in Java.

---

### What Is `boolean`?

- The `boolean` type has only **two possible values**: `true` or `false`.
- It is **1 bit** conceptually, but usually occupies **1 byte** in memory.
- It is used to control program flow, such as with `if`, `while`, and `for` loops.

```java
boolean isOnline = true;
boolean isEmpty = false;
````

---

###  Common Boolean Use Cases

#### 1. **Conditional Statements**

```java
boolean isAdult = age >= 18;

if (isAdult) {
    System.out.println("Access granted.");
} else {
    System.out.println("Access denied.");
}
```

#### 2. **Loop Control**

```java
boolean keepRunning = true;

while (keepRunning) {
    // loop body
    keepRunning = false; // stop after one iteration
}
```

#### 3. **Logical Expressions**

```java
boolean isWeekend = true;
boolean isHoliday = false;

boolean canRelax = isWeekend || isHoliday;
```

---

### Boolean Operators in Java

| Operator | Description    | Example         | Result              |        |   |         |        |
| -------- | -------------- | --------------- | ------------------- | ------ | - | ------- | ------ |
| `&&`     | Logical AND    | `true && false` | `false`             |        |   |         |        |
| \`       |                | \`              | Logical OR          | \`true |   | false\` | `true` |
| `!`      | Logical NOT    | `!true`         | `false`             |        |   |         |        |
| `==`     | Equality check | `x == true`     | `true` if x is true |        |   |         |        |
| `!=`     | Not equal      | `x != false`    | `true` if x is true |        |   |         |        |

---

### 💡 Best Practices

* Use **meaningful names** for boolean variables: `isActive`, `hasPermission`, `canFly`.
* Avoid comparisons like `if (isReady == true)`; prefer `if (isReady)`.
* Use boolean return types in methods for checks and validations.

```java
public boolean isValidEmail(String email) {
    return email != null && email.contains("@");
}
```

---

### 🧪 Boolean Wrapper Class: `Boolean`

Java provides a wrapper class for the primitive `boolean`:

```java
Boolean flag = Boolean.TRUE;
boolean value = flag.booleanValue();
```

Useful for collections and objects that require non-primitive types.

---

### 📌 Summary

| Feature           | Details                  |
| ----------------- | ------------------------ |
| Type              | `boolean`                |
| Values            | `true`, `false`          |
| Size              | 1 byte (internally)      |
| Default value     | `false`                  |
| Wrapper class     | `Boolean`                |
| Typical use cases | Conditions, flags, logic |

---

### ✅ Example

```java
public class BooleanExample {
    public static void main(String[] args) {
        boolean isJavaFun = true;
        boolean isRainy = false;

        if (isJavaFun && !isRainy) {
            System.out.println("Let's code with joy!");
        }
    }
}
```

Booleans are small but **powerful** tools that control the **logic** of your entire program.

```


Used for true/false values.
Example:

```java
boolean isActive = true;
```