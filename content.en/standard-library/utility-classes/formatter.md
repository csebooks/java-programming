---
title: 'Formatter'
weight: 8
---


The `Formatter` class (in `java.util`) is used to **format values into strings** using a format string and arguments, similar to C-style `printf`. It supports locale-sensitive formatting and is often used behind the scenes in methods like `String.format()` and `System.out.printf()`.

---

### Scenarios

- Creating human-readable, aligned reports or logs.
- Formatting numbers, dates, and strings based on templates.
- Producing output specific to locale or custom layout.

---

### Sample Usage

```java
public class FormatterExample {
    public static void main(String[] args) {
        String name = "Java";
        int version = 17;
        double score = 98.765;

        // Using String.format
        String formatted = String.format("Language: %s, Version: %d, Score: %.2f", name, version, score);
        System.out.println(formatted);

        // Using Formatter directly
        Formatter formatter = new Formatter();
        formatter.format("Hex of %d is %x", 255, 255);
        System.out.println(formatter.toString());

        formatter.close();
    }
}
```

### Output

```
Language: Java, Version: 17, Score: 98.77
Hex of 255 is ff
```

### Format Specifiers (Common)

* `%s` – String
* `%d` – Decimal Integer
* `%f` – Floating point (e.g., `%.2f` limits to 2 decimals)
* `%x` – Hexadecimal
* `%t` – Date/Time

### With Locale

```java
import java.util.Locale;

System.out.println(String.format(Locale.FRANCE, "Montant : %.2f €", 1234.56));
```

Use `Formatter` when you want clean, precise, and customizable output generation.

