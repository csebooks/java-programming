---
title: 'Base64'
weight: 11
---

`Base64` (in `java.util`) provides encoding and decoding of binary data to and from Base64 representation, which is a way to encode binary data using only ASCII characters. This is useful for transmitting binary data over media designed to handle text.

---

### Scenarios

- Encoding binary data like images or files for transmission in emails or JSON.
- Decoding Base64 encoded strings back to original binary data.
- Storing binary data in text-based formats like XML or JSON.

---

### Sample Usage

```java
import java.util.Base64;

public class Base64Example {
    public static void main(String[] args) {
        String originalString = "Hello, Java!";
        
        // Encode
        String encodedString = Base64.getEncoder().encodeToString(originalString.getBytes());
        System.out.println("Encoded: " + encodedString);

        // Decode
        byte[] decodedBytes = Base64.getDecoder().decode(encodedString);
        String decodedString = new String(decodedBytes);
        System.out.println("Decoded: " + decodedString);
    }
}
```

### Output

```
Encoded: SGVsbG8sIEphdmEh
Decoded: Hello, Java!
```

---

### Notes

* Java 8 introduced `Base64` class in `java.util` package.
* It supports basic, URL-safe, and MIME variants of Base64 encoding.
* Useful in security, data serialization, and communication protocols.
