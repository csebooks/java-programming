---
title: 'BitSet'
weight: 3
---

`BitSet` is a class in `java.util` that represents a vector of bits that grows as needed. It is a memory-efficient alternative to using arrays of booleans or integers to track flags or binary states.

---

### Scenarios

- Efficiently represent sets of flags or toggles.
- Perform fast bitwise operations (AND, OR, XOR, etc.).
- Solve problems involving combinations, masks, or presence/absence indicators.
- Common in low-level data structures, bloom filters, or scheduling applications.

---

### Sample Usage

```java
import java.util.BitSet;

public class BitSetExample {
    public static void main(String[] args) {
        BitSet flags = new BitSet();

        // Set some bits
        flags.set(0);  // true at index 0
        flags.set(3);  // true at index 3

        // Check a bit
        System.out.println(flags.get(0)); // true
        System.out.println(flags.get(1)); // false

        // Flip a bit
        flags.flip(1);
        System.out.println(flags.get(1)); // true

        // Clear a bit
        flags.clear(3);

        // Print all set bits
        System.out.println(flags); // {0, 1}
    }
}
```

BitSet is useful when dealing with large datasets where space and performance are critical.