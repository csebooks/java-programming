---
title: 'Boolean'
weight: 2
---

Lers create Json Type for null at `src/main/java/com/techatpark/jsonparser/type/JsonNull.java`

```java
package com.techatpark.jsonparser.type;

public class JsonBoolean implements Json<Boolean> {
    @Override
    public Object getValue() {
        return null;
    }
}
```
