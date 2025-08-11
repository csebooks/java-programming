---
title: 'Null'
weight: 1
---

Lets create the test case at `src/main/java/org/example/parser/JsonTest.java`

```java
    @Test
    void testNull() throws IOException {
        Assertions.assertNull(Json.parse(new StringReader("null")), "Null parse failed");
    }
```

Lers create Json Type for null at `src/main/java/com/example/jsonparser/type/JsonNull.java`

```java
package org.example.parser.type;

import org.example.parser.Json;

public class JsonNull implements Json<Object> {

    public JsonNull(final Reader reader) {

    }

    @Override
    public Object getValue() {
        return null;
    }
}
```

We need to add support for this in Json at `src/main/java/org/example/parser/Json.java`


```java
private static Json<?> read(final Reader reader) throws IOException {
    char firrstChar = (char) reader.read();
    if(firrstChar == 'n') {
        return new JsonNull(reader);
    }
    return null;
}
```

Time to implement....

```java
public JsonNull(final Reader reader) throws IOException {
    char[] cbff = new char[3];
    reader.read(cbff);
    if(cbff[0] != 'u' || cbff[1] != 'l' || cbff[2] != 'l') {
        throw new IllegalArgumentException("Not a valid null value");
    }
}
```

