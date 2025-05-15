---
title: 'Arrays'
weight: 4
--- 

Arrays in Java are **fixed-size, ordered collections** of elements of the same type. They’re a low-level, efficient way to group related values and work with them by index.

---

```java
// Declaration and allocation
int[] numbers = new int[5];    // holds five ints, all initialized to 0

// Literal initialization
String[] fruits = { "Apple", "Banana", "Cherry" };
```

Access elements by index (0-based):

```java
fruits[1] = "Blueberry";
System.out.println(fruits[2]);   // Cherry
```

---

### Multi-Dimensional Arrays

Java supports arrays of arrays:

```java
int[][] matrix = new int[3][4];  
// 3 rows, 4 columns, all zeros

// Literal
int[][] grid = {
    {1, 2, 3},
    {4, 5, 6}
};
System.out.println(grid[1][2]);  // 6
```

---

### Common Patterns

* **Iterating**:

  ```java
  for (int i = 0; i < numbers.length; i++) {
      System.out.println(numbers[i]);
  }

  // Enhanced for-loop
  for (String fruit : fruits) {
      System.out.println(fruit);
  }
  ```

* **Copying**:

  ```java
  int[] copy = Arrays.copyOf(numbers, numbers.length);
  ```

* **Filling**:

  ```java
  Arrays.fill(numbers, 42);  // every slot becomes 42
  ```

* **Sorting & Searching**:

  ```java
  Arrays.sort(fruits);                     // alphabetical
  int idx = Arrays.binarySearch(numbers, 5);
  ```

---

### Array Reflection & Utilities

* **`array.length`** gives size (no parentheses).
* Arrays are objects—`numbers.getClass().isArray()` returns `true`.
* The **`java.util.Arrays`** class provides static methods for:

  * Converting to string: `Arrays.toString(numbers)`
  * Deep string for nested arrays: `Arrays.deepToString(matrix)`
  * Comparing: `Arrays.equals(a, b)` or `Arrays.deepEquals(a2, b2)`

---

Arrays are simple, fast, and ideal when you know your collection size up front. For dynamic or richer operations, Java’s **Collections** framework (like `List` or `Set`) adds flexibility—but arrays remain the building block under the hood.
