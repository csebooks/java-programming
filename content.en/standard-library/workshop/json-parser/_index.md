---
title: 'Json Parser'
weight: 1
---

> Parser are the programs that will read and understand (mostly) text content. For e/g Java or C Profram files, JSON files etc. In this workshop we are going to create our own JSON parser

Parser consiste two phases

1. Reading Phase - We read the input sequencially and tokenize
2. Understand Phase Oerate on these tokens to understand

```java
public interface Json<T> {

    static Object parse(final Reader reader) throws IOException {
        try(reader) {
            return read(reader).getValue();
        }
    }

    T getValue() ;

}
```

