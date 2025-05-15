---
title: 'Scanner'
weight: 9
---


`Scanner` (from `java.util`) is a simple text parser that reads input and breaks it into tokens using regular expressions. It can read from a variety of input sources such as keyboard, files, or strings.

---

### Scenarios

- Reading user input from the console.
- Parsing structured text or files.
- Quickly tokenizing input without writing custom parsers.

---

### Sample Usage – Console Input

```java
import java.util.Scanner;

public class ScannerExample {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Enter your name: ");
        String name = scanner.nextLine();

        System.out.print("Enter your age: ");
        int age = scanner.nextInt();

        System.out.println("Hello, " + name + ". You are " + age + " years old.");

        scanner.close();
    }
}
````

---

### Sample Usage – From String

```java
String data = "100 200 300";
Scanner scanner = new Scanner(data);

while (scanner.hasNextInt()) {
    System.out.println("Read number: " + scanner.nextInt());
}
scanner.close();
```

---

### Sample Usage – From File

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

Scanner fileScanner = new Scanner(new File("data.txt"));

while (fileScanner.hasNextLine()) {
    System.out.println(fileScanner.nextLine());
}
fileScanner.close();
```

---

### Notes

* Always close the `Scanner` when done (especially with file input).
* For input mismatch exceptions, check types using `hasNextInt()`, `hasNextDouble()`, etc.
* `Scanner` uses whitespace as the default delimiter, but you can customize it using `useDelimiter()`.

