---
title: 'Binary streams'
weight: 2
categories:
    - binary-streams
---

> Byte streams are used for **raw binary data**, such as images, audio files, and other non-text files.

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