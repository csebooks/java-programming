---
title: "String"
weight: 3
---

Lets check a testcase

```
"Sample String"

```

Lets create the test case at `src/main/java/org/example/parser/JsonTest.java`

```java

  @Test
    void testString() throws IOException {
        Assertions.assertEquals("Sampletext",parse(Reader.of("\"Sampletext\"")));
    }
```
String will have more combinations. Lets keep the samples in a text `resources/json-strings.txt`


```java
 
 "Sample \"String" //String with Escape character
 
 "A\nB" //String with feed character
 "A\tB"
 "A\bB"
 "A\rB"
 "A\fB"

 "A\u9097" //String with unicode
```

In test we are using `JsonParser.parseString(line).getAsString()` in gson dependency for testing a case and adding dependency in `pom.xml`

```java
<dependency>
			<groupId>com.google.code.gson</groupId>
			<artifactId>gson</artifactId>
			<version>{$gson version}</version>
			<scope>test</scope>
</dependency>
```

Lets create Json Type for String at `src/main/java/com/example/json/type/JsonString.java`

```java

public class JsonString implements Json<String> {
    private final StringBuilder builder;
        }


    @Override
    public String getValue() {
        return builder.toString();
    }

```

We need to add support for this in Json at `src/main/java/com/example/json/Json.java`

```java

   static Json<?> read(Reader reader) throws IOException {
       char firstChar = (char)  reader.read();
       if (firstChar == 'n') {
           return new JsonNull(reader);
       } else if(firstChar == 't'){
           return new JsonBoolean(reader, true);
       } else if (firstChar == 'f') {
           return new JsonBoolean(reader, false);
       } else if (firstChar == '"') {
           return new JsonString(reader);
       }
       return null;
   }
```

Time to implement....

```java
public JsonString(Reader reader) throws IOException {
        builder = buildString(reader);
    }

    private static StringBuilder buildString(final Reader reader) throws IOException {
        final StringBuilder sb = new StringBuilder();
        char a;
        while ((a = (char) reader.read()) != '\\' && a != '"') {
            sb.append(a);
        }
        if (a == '\\') {
            a = ((char) reader.read());
            if (a == '"' || a == '\\' || a == '\'' || a == '/') {
                sb.append(a);
            } else if (a == 'n') {
                sb.append('\n');
            } else if (a == 'b') {
                sb.append('\b');
            } else if (a == 'r') {
                sb.append('\r');
            } else if (a == 't') {
                sb.append('\t');
            } else if (a == 'f') {
                sb.append('\f');
            } else if (a == 'u') {
                char[] value = new char[4];
                for (int i = 0; i < 4; i++) {
                    int next = reader.read();
                    value[i] = (char) next;
                }
                String hexstr = new String(value);
                int value1 = Integer.parseInt(hexstr, 16);
                sb.append((char) value1);
            }
            sb.append(buildString(reader));
        }
        return sb;
    }

```
