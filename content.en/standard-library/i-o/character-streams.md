---
title: 'Character streams'
weight: 1
categories:
    - character-streams
---

> Character streams are designed to handle **text data** (Unicode characters). These classes automatically handle character encoding and decoding.

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