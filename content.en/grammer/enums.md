---
title: 'Enums'
weight: 5
---

Enums give you a **type-safe way** to represent a fixed set of constants—like days of the week, states in a workflow, or categories in your app—while letting you attach behavior or data to each constant.

```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY;
}
```

Usage:

```java
Day today = Day.WEDNESDAY;

if (today == Day.SATURDAY || today == Day.SUNDAY) {
    System.out.println("Weekend!");
} else {
    System.out.println("Workday");
}
```


### Attaching Attribute and Behavior

Enums are full-fledged classes—you can add fields, constructors, and methods.

```java
enum Planet {
    MERCURY(3.30e+23, 2.4397e6),
    EARTH  (5.97e+24, 6.3710e6),
    MARS   (6.42e+23, 3.3895e6);

    private final double mass;   // in kilograms
    private final double radius; // in meters

    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
    }

    double surfaceGravity() {
        final double G = 6.67430e-11;
        return G * mass / (radius * radius);
    }
}
```

Please note that you enum is a final class internally. so you can not override

```java
double earthGravity = Planet.EARTH.surfaceGravity();
System.out.println("Earth gravity: " + earthGravity);
```

---

### Built-in Methods

* `values()` – returns an array of all enum constants
* `valueOf(String name)` – returns the enum constant with the given name
* `ordinal()` – returns the position (0-based) of the constant

```java
for (Day d : Day.values()) {
    System.out.println(d + " is at position " + d.ordinal());
}

Day d = Day.valueOf("FRIDAY"); // FRIDAY
```


### Using Enums in `switch`

```java
switch (today) {
    case MONDAY, FRIDAY -> System.out.println("Ugh, start or end of workweek");
    case SATURDAY, SUNDAY -> System.out.println("Enjoy the weekend!");
    default -> System.out.println("Middle of the week");
}
```

Enums give you **type safety**, **readability**, and the power to bundle data and behavior with your constants—making your code more expressive and less error-prone.
