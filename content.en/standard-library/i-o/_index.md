---
title: 'File I/O'
weight: 4
categories:
    - i-o
---

> Java provides a robust and flexible set of APIs for file input and output operations. These APIs abstract the complexity of reading from and writing to files, and they support both **character-based** and **binary-based** data handling.

### Realtime usage of java file I/O
 
* Data Storage & Retrieval
* Configuration Management
* Backup & recovery
* Logging
* File upload & Download
* Text processing

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

With these three layers—**character streams**, **byte streams**, and **NIO with `Path` and `Files`**—Java enables clean, powerful, and cross-platform file I/O operations.

