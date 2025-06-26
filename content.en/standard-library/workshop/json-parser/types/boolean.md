---
title: "Boolean"
weight: 2
---

Lets create the test case at `src/main/java/org/example/parser/JsonTest.java`

```java
    @Test
    void testBoolean() throws IOException {
        Assertions.assertEquals(true, Json.parse(Reader.of("true")), "True parse failed");
        Assertions.assertEquals(false, Json.parse(Reader.of("false")), "False parse failed");
    }
```

Lers create Json Type for null at `src/main/java/com/techatpark/jsonparser/type/JsonBoolean.java`

```java
public class JsonBoolean implements Json<Boolean> {

    /**
     * Value inside Json.
     */
    private final boolean value ;

    /**
     * Reads Boolean value.
     * @param reader
     * @param value true or false
     * @throws IOException
     */
    public JsonBoolean(final Reader reader, boolean value) throws IOException {
    }

    @Override
    public Boolean getValue() {
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
    } else if(firstChar == 't'){
        return new JsonBoolean(reader,true);
    } else if (firstChar == 'f') {
        return new JsonBoolean(reader,false);
    }
    return null;
}
```

Time to implement....

```java
   private final boolean value ;
    public JsonBoolean(final Reader reader, boolean value) throws IOException {
        this.value = value;
        if(value) {
            char[] values = new char[3];
            reader.read(values);
            if(values[0] != 'r' || values[1] !='u'|| values[2] !='e'){
                throw new IllegalArgumentException("not true value");
            }
        } else {
            char[] values = new char[4];
            reader.read(values);
            if(values[0] != 'a' || values[1] !='l'|| values[2] !='s' || values[3] != 'e'){
                throw new IllegalArgumentException("not false value");
            }
        }
        @Override
        public Boolean getValue() {
        return this.value;
    }
    }

```
