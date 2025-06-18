---
title: 'File I/O'
weight: 4
---

> Java provides a robust and flexible set of APIs for file input and output operations. These APIs abstract the complexity of reading from and writing to files, and they support both **character-based** and **binary-based** data handling.

---

### Realtime usage of java file I/O
 
* Data Storage & Retrieval
* Configuration Management
* Backup & recovery
* Logging
* File upload & Download
* Text processing


### Character-Based I/O (Reader / Writer)

Character streams are designed to handle **text data** (Unicode characters). These classes automatically handle character encoding and decoding.

- **Reader** – Abstract class for reading character streams.
- **Writer** – Abstract class for writing character streams.

#### Common Implementations:
- `FileReader` / `FileWriter` – Read/write characters from/to files.
- `BufferedReader` / `BufferedWriter` – Buffer characters for efficient reading/writing.
- `PrintWriter` – Convenient methods to write formatted text (like `println`).

##### Sample Usage:

```java
try (BufferedReader reader = new BufferedReader(new FileReader("data.txt"))) {
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
}
````

---

### Binary-Based I/O (InputStream / OutputStream)

Byte streams are used for **raw binary data**, such as images, audio files, and other non-text files.

* **InputStream** – Abstract class for reading byte streams.
* **OutputStream** – Abstract class for writing byte streams.

#### Common Implementations:

* `FileInputStream` / `FileOutputStream` – Read/write bytes from/to files.
* `BufferedInputStream` / `BufferedOutputStream` – Buffer for performance.
* `DataInputStream` / `DataOutputStream` – Read/write primitive types in binary.

##### Sample Usage:

```java
try (FileInputStream in = new FileInputStream("image.jpg");
     FileOutputStream out = new FileOutputStream("copy.jpg")) {
    byte[] buffer = new byte[1024];
    int bytesRead;
    while ((bytesRead = in.read(buffer)) != -1) {
        out.write(buffer, 0, bytesRead);
    }
}
```

---

### Path and Files Utility (java.nio.file)

Java NIO introduced the `Path` interface and `Files` utility class to modernize and simplify file handling.

#### Path

* Represents a location on the filesystem.
* Use `Paths.get("file.txt")` or `Path.of("file.txt")` (Java 11+).
* Works well with `Files` API.

```java
Path path = Paths.get("example.txt");
```

#### Files Utility

The `Files` class provides convenient static methods for common file operations.

##### Common Operations:

* `Files.readAllLines(Path)` – Read all lines from a text file.
* `Files.write(Path, List<String>)` – Write lines to a file.
* `Files.copy(Path, Path)` – Copy files.
* `Files.delete(Path)` – Delete a file.
* `Files.exists(Path)` – Check file existence.
* `Files.lines(Path)` – Stream file lines.

##### Sample Usage:

```java
Path path = Paths.get("example.txt");
List<String> lines = Files.readAllLines(path);
lines.forEach(System.out::println);
```

---

With these three layers—**character streams**, **byte streams**, and **NIO with `Path` and `Files`**—Java enables clean, powerful, and cross-platform file I/O operations.

```
