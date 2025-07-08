---
title: 'File Tree'
weight: 3
---

> A **File Tree** represents the hierarchical structure of files and directories starting from a root path. In Java, file trees are typically traversed using the `java.nio.file` package—especially with `Files.walkFileTree()` and the `FileVisitor` interface.

It allows recursive access to directories and files, enabling actions like search, copy, delete, or analyze directory structures programmatically.

---

### 🔧 **Common Use Cases:**

* Recursively listing files and directories
* Searching for files by pattern or type
* Copying or deleting entire folder structures
* Gathering statistics (e.g., file size, count)
* Implementing backup or cleanup utilities

---

### 🧪 **Sample Usage: Walk and Print File Tree**

```java
import java.io.IOException;
import java.nio.file.*;
import java.nio.file.attribute.BasicFileAttributes;

public class FileTreeExample {
    public static void main(String[] args) throws IOException {
        Path startPath = Paths.get("your/start/directory");

        Files.walkFileTree(startPath, new SimpleFileVisitor<Path>() {
            @Override
            public FileVisitResult visitFile(Path file, BasicFileAttributes attrs)
                    throws IOException {
                System.out.println("Visited file: " + file);
                return FileVisitResult.CONTINUE;
            }

            @Override
            public FileVisitResult preVisitDirectory(Path dir, BasicFileAttributes attrs)
                    throws IOException {
                System.out.println("Entering directory: " + dir);
                return FileVisitResult.CONTINUE;
            }
        });
    }
}
```

---

### 📘 **Key Classes and Interfaces:**

| Component              | Purpose                                      |
| ---------------------- | -------------------------------------------- |
| `Files.walkFileTree()` | Starts the recursive traversal               |
| `FileVisitor<T>`       | Interface for visiting file tree nodes       |
| `SimpleFileVisitor<T>` | A convenient base class with default methods |
| `Path` / `Paths`       | Represents filesystem paths                  |
| `BasicFileAttributes`  | Provides file metadata                       |
