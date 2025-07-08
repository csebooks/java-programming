---
title: 'Realtime I/O'
weight: 5
---


While you may have a strong foundation in Java I/O, real-world applications often demand high-level APIs that simplify code and improve readability. In this chapter, we explore modern Java APIs that help you **do more with less code**, especially improvements introduced since **Java 8**.

## 📘 What You’ll Learn

- Reading and writing text files
- Reading text, images, JSON from the web
- Visiting files in a directory
- Reading ZIP files as file systems
- Creating temporary files and directories

This chapter focuses on practical, modern I/O features using `java.nio.file.Files`, `java.net.http.HttpClient`, and related classes.


## ✅ Modern Enhancements

- **Java 18+**: UTF-8 is the default charset (JEP 400)
- `Files` API: Expanded significantly since Java 7 with new methods in Java 8, 11, and 12
- `InputStream` now includes `readAllBytes`, `transferTo`, etc.
- `java.io.File` and `BufferedReader` are considered outdated



## 📄 Reading Text Files

```java
Path path = Path.of("/usr/share/dict/words");
String content = Files.readString(path); // Reads the whole file
````

Read as lines:

```java
List<String> lines = Files.readAllLines(path);
```

Process lazily with streams:

```java
try (Stream<String> lines = Files.lines(path)) {
    lines.forEach(System.out::println);
}
```

### 🔍 Tokenizing with Scanner

```java
Stream<String> tokens = new Scanner(path).useDelimiter("\\PL+").tokens();
```

## ✍️ Writing Text Files

```java
Files.writeString(path, content);
Files.write(path, List.of("line1", "line2"));
```

Using `PrintWriter` (for formatted output):

```java
try (PrintWriter writer = new PrintWriter(path.toFile())) {
    writer.printf(locale, "Hello, %s!", name);
}
```

Using `BufferedWriter`:

```java
try (BufferedWriter writer = Files.newBufferedWriter(path)) {
    writer.write("Hello");
    writer.newLine();
}
```

## 🌐 Reading from the Web

```java
InputStream in = new URI("https://example.com").toURL().openStream();
String result = new String(in.readAllBytes());
```



## 🖼️ Reading Images or JSON

Using Jackson (JSON):

```java
Map<String, Object> result = JSON.std.mapFrom(new URI("https://api").toURL());
```

Reading image:

```java
BufferedImage img = ImageIO.read(new URI(imageUrl).toURL());
```

## 📂 Traversing Directories

List files:

```java
try (Stream<Path> entries = Files.list(path)) {
    entries.forEach(System.out::println);
}
```

Walk recursively:

```java
try (Stream<Path> entries = Files.walk(path)) {
    entries.filter(p -> p.toString().endsWith(".html"))
           .forEach(System.out::println);
}
```

Use `Files.find()` for attribute-based filtering.

## 📦 Working with ZIP Files

Mount ZIP file as a virtual file system:

```java
try (FileSystem zipFs = FileSystems.newFileSystem(pathToZip)) {
    Files.walk(zipFs.getPath("/"))
         .filter(Files::isRegularFile)
         .forEach(System.out::println);
}
```



## 🧪 Creating Temporary Files & Directories

```java
Path tempFile = Files.createTempFile("prefix", ".tmp");
Path tempDir  = Files.createTempDirectory("prefix");
```

These are deleted automatically on reboot or can be managed manually.



## 🔚 Conclusion

Modern Java I/O is simpler, more powerful, and safer:

* Use `Files.readString`, `Files.writeString`, and `Files.walk` instead of old APIs.
* Prefer high-level classes and built-in charset handling (UTF-8).
* Avoid legacy `File` and `BufferedReader` where possible.
* Explore the full power of the `Files` and `Path` APIs.