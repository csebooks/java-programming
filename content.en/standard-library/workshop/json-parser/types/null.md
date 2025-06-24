---
title: 'Null'
weight: 1
---

Lers create Json Type for null at `src/main/java/com/techatpark/jsonparser/type/JsonNull.java`

```java
package com.techatpark.jsonparser.type;

public class JsonNull implements Json<Object> {
    @Override
    public Object getValue() {
        return null;
    }
}
```
