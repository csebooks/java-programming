---
title: 'Float'
weight: 2
categories:
    - float-point
---
Here's the complete **`float.md`** markdown file content, covering `float`, `double`, and `BigDecimal` in Java:

````markdown


> Java provides **floating-point types** to represent numbers with decimals. These are critical when working with measurements, scientific calculations, currency, and real-world quantities that aren't whole numbers.

---

###  Overview of Floating-Point Types

| Type          | Size     | Precision              | Range (Approx)                             | Default Value   | Wrapper Class  |
|---------------|----------|------------------------|---------------------------------------------|-----------------|----------------|
| `float`       | 4 bytes  | ~7 decimal digits      | ±3.4E−38 to ±3.4E+38                        | `0.0f`          | `Float`        |
| `double`      | 8 bytes  | ~15 decimal digits     | ±1.7E−308 to ±1.7E+308                      | `0.0d`          | `Double`       |
| `BigDecimal`  | Varies   | Arbitrary precision    | Limited only by memory                      | `BigDecimal.ZERO` | `BigDecimal` |

---

##  `float`: Lightweight Decimal Type

```java
float temperature = 36.6f; // must use 'f' suffix
````

*  Memory-efficient (4 bytes)
*  Limited precision; use cautiously in financial calculations

---

##  `double`: Standard for Decimals

```java
double pi = 3.141592653589793;
```

*  Higher precision than `float` (15 decimal digits)
*  Default type for decimal literals
*  Still susceptible to rounding errors

---

##  `BigDecimal`: Precise Decimal Type

```java
import java.math.BigDecimal;

BigDecimal price = new BigDecimal("19.99");
BigDecimal total = price.multiply(new BigDecimal("2"));
```

*  Arbitrary precision — perfect for finance, billing, tax
*  Slower, more verbose; avoid primitive operators (`+`, `*`)

---

###  Floating-Point Precision Warning

Floating-point types (`float`, `double`) may introduce rounding errors:

```java
System.out.println(0.1 + 0.2);  // Output: 0.30000000000000004
```

Use `BigDecimal` for accurate monetary operations.

---

###  Comparing Floating-Point Values

Avoid direct equality:

```java
double a = 0.1 + 0.2;
if (Math.abs(a - 0.3) < 0.000001) {
    System.out.println("Equal enough!");
}
```

---

###  Summary Table

| Type         | Use Case                 | Pros                     | Cons                        |
| ------------ | ------------------------ | ------------------------ | --------------------------- |
| `float`      | Graphics, games          | Memory efficient         | Low precision               |
| `double`     | Scientific, engineering  | Good precision & speed   | Not accurate for currency   |
| `BigDecimal` | Finance, tax, e-commerce | Accurate, arbitrary size | Verbose, slower performance |

---

### Best Practices

* Use `double` for general-purpose decimal values.
* Use `BigDecimal` for financial or high-precision needs.
* Avoid `float` unless working in performance-sensitive environments like game development or embedded systems.

---

###  Example: Choosing the Right Floating Type

```java
double distanceKm = 42.195;
BigDecimal accountBalance = new BigDecimal("1050.75");
float speed = 24.5f;
```




