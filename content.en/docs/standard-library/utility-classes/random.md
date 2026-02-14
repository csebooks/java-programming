---
title: 'Random'
weight: 4
---

The `Random` class in `java.util` provides methods to generate **pseudo-random numbers** of different types (int, long, float, double, boolean). Java also offers `ThreadLocalRandom` and `SecureRandom` for different use cases.

---

### Scenarios

- Simulating games, lottery, or test data.
- Randomizing list items or shuffling arrays.
- Generating random IDs or temporary passwords.
- Using reproducible sequences of numbers with seeded randomness.

---

### Sample Usage

```java
import java.util.Random;

public class RandomExample {
    public static void main(String[] args) {
        Random rand = new Random(); // or new Random(seed)

        int randomInt = rand.nextInt(100);  // 0 to 99
        double randomDouble = rand.nextDouble();  // 0.0 to 1.0
        boolean randomBool = rand.nextBoolean();

        System.out.println(randomInt);
        System.out.println(randomDouble);
        System.out.println(randomBool);
    }
}
```

#### Better for concurrent use (Java 7+):

```java
import java.util.concurrent.ThreadLocalRandom;

int randomInRange = ThreadLocalRandom.current().nextInt(10, 50); // 10 to 49
```

#### For cryptographically secure values:

```java
import java.security.SecureRandom;

SecureRandom secure = new SecureRandom();
byte[] seed = new byte[16];
secure.nextBytes(seed); // fills the byte array with secure random values
```

Use `Random` for general purposes, `ThreadLocalRandom` for concurrent performance, and `SecureRandom` when security matters.
