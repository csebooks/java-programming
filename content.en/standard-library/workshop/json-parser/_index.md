---
title: 'Json Parser'
weight: 1
---

> Parser are the programs that will read and understand (mostly) text content. For e/g Java or C Program files, JSON files etc. In this workshop we are going to create our own JSON parser

Parser consiste two phases

1. Reading Phase - We read the input sequencially and tokenize
2. Understand Phase Oerate on these tokens to understand

Lets define Json at `src/main/java/org/example/parser/Json.java`

```java
package org.example.parser;

import java.io.IOException;
import java.io.Reader;

/**
 * Parser for Json.
 * @param <T>
 */
public interface Json<T> {

    /**
     * Parse Json to get valyes.
     * @param reader
     * @return value.
     * @throws IOException
     */
    static Object parse(final Reader reader) throws IOException {
        try(reader) {
            return read(reader) // Phase 1
                    .getValue(); // Phase 2
        }
    }

    /**
     * Reads Json from Reader.
     * @param reader
     * @return json
     */
    private static Json<?> read(final Reader reader) throws IOException {
        char firrstChar = (char) reader.read();
        return null;
    }

    /**
     * Gets Value from Json.
     * @return value
     */
    T getValue();
}
```

Lets define Json Test at `src/main/java/org/example/parser/JsonTest.java`

```java
package org.example.parser;

class JsonTest {
    
}
```




