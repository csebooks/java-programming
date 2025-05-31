---
title: 'Arrays'
weight: 5
---

> An array is a container object that holds a **fixed number of values** of a **single type**. The length of an array is established when the array is created. After creation, its length is fixed.

```java
int[] numbers = new int[3];  // Creates space for 3 integers
numbers[0] = 10;
numbers[1] = 20;
numbers[2] = 30;

System.out.println(numbers[1]);  // Output: 20
```

---

## Declaring and Initializing Arrays

There are two main ways:

### 1. Declare and assign later:

```java
int[] marks;
marks = new int[5];
```

### 2. Declare and initialize in one step:

```java
int[] marks = {90, 80, 70, 60, 50};
```

---

## Looping Through Arrays

Use a loop to print all elements:

```java
for (int i = 0; i < marks.length; i++) {
    System.out.println(marks[i]);
}
```

Or use an enhanced for-loop:

```java
for (int mark : marks) {
    System.out.println(mark);
}
```

---

## Common Array Types

You can create arrays of any data type:

```java
String[] names;
double[] prices;
boolean[] flags;
```

---

## 🧩 Multidimensional Arrays

Arrays inside arrays (like a table):

```java
String[][] names = {
    {"Mr. ", "Ms. "},
    {"Smith", "Jones"}
};

System.out.println(names[0][0] + names[1][0]);  // Output: Mr. Smith
```

---

## ✂️ Copying Arrays

### Manual way:

```java
int[] source = {1, 2, 3};
int[] target = new int[3];

for (int i = 0; i < source.length; i++) {
    target[i] = source[i];
}
```

### Using `System.arraycopy()`:

```java
System.arraycopy(source, 0, target, 0, source.length);
```

### Using `Arrays.copyOf()`:

```java
int[] target = java.util.Arrays.copyOf(source, source.length);
```

---

## 🛠️ Helpful Methods in `java.util.Arrays`

```java
import java.util.Arrays;

Arrays.sort(marks);               // Sort the array
int pos = Arrays.binarySearch(marks, 70);  // Search
boolean same = Arrays.equals(marks, target); // Compare
Arrays.fill(marks, 100);          // Fill with same value
System.out.println(Arrays.toString(marks)); // Print nicely
```

---

## Summary

* Arrays hold multiple values of the same type.
* Indexing starts from 0.
* Use loops to process arrays.
* Use utility methods from `Arrays` class to copy, sort, or print arrays.


