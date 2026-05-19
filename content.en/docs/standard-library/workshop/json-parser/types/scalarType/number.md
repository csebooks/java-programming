---
title: 'True'
weight: 4
---

Lets check a testcase
```
123
```

Lets create the test case at `src/main/java/org/example/parser/JsonTest.java`

```java

  @Test
    void testNumber() throws IOException {
        Assertions.assertEquals(123,parse(Reader.of("123")));
    }
```

Lets keep the samples in a text `resources/json-number.txt`

```java
0
123
-9876544569876543
-123
-4567898765487654
3.14
```

Lets create Json Type for number at `src/main/java/com/example/json/type/JsonNumber.java`

```java

public class JsonNumber implements Json<Number> {
    private final StringBuilder builder;
        }
    public Number buildNumber(StringBuilder numberBuilder) {
    }


    @Override
    public Number getValue() {
          return buildNumber(builder);
    }

```
We need to add support for this in Json at `src/main/java/com/example/json/Json.java`

```java
    char firstChar = (char) reader.read();
        if (firstChar == 'n') {
            return new JsonNull(reader);
        } else if (firstChar == 't') {
            return new JsonBoolean(reader, true);
        } else if (firstChar == 'f') {
            return new JsonBoolean(reader, false);
        }
         else if (firstChar == '"') {
            return new JsonString(reader);
         }
         else if (Character.isDigit(firstChar) || firstChar == '-') {
            return new JsonNumber(firstChar, reader);
        }

```

Time to implement....

```java
private final StringBuilder builder;

    public JsonNumber(char firstChar, Reader reader) throws IOException {
        builder = new StringBuilder().append(firstChar).append(buildString(reader));

    }

    private StringBuilder buildString(Reader reader) throws IOException {
        final StringBuilder sb = new StringBuilder();

        int a;
        while ((a = reader.read()) != -1) {
            sb.append((char) a);

        }
        return sb;
    }


    private static final int DEFAULT_MAX_NUM_LEN = 100;

    public Number buildNumber(StringBuilder numberBuilder) {
        String numberStr = numberBuilder.toString();
        try {
            // Try to parse as different types based on the range
            if (numberStr.contains(".")) {
               return new BigDecimal(numberStr).stripTrailingZeros();
            } else {
                // Check if the number is an integer
                if (numberBuilder.length() > DEFAULT_MAX_NUM_LEN) {
                    throw new IllegalArgumentException(
                            "Number value length exceeds the maximum allowed ("
                                    + DEFAULT_MAX_NUM_LEN + ")");
                }
                long longValue = Long.parseLong(numberStr);
                if (longValue >= Byte.MIN_VALUE
                        && longValue <= Byte.MAX_VALUE) {
                    return (byte) longValue;
                } else if (longValue >= Short.MIN_VALUE
                        && longValue <= Short.MAX_VALUE) {
                    return (short) longValue;
                } else if (longValue >= Integer.MIN_VALUE
                        && longValue <= Integer.MAX_VALUE) {
                    return (int) longValue;
                } else {
                    return longValue;
                }
            }
        } catch (NumberFormatException e) {
            return parseBigNumber(numberStr);
        }
    }

        private Number parseBigNumber(final String numberStr) {

        try {
            return new BigInteger(numberStr); // Try BigInteger first
        } catch (NumberFormatException e) {
            // Only create BigDecimal if BigInteger fails
            BigDecimal bd = new BigDecimal(numberStr);
            try {
                // Convert to BigInteger if there's no fraction
                return bd.toBigIntegerExact();
            } catch (ArithmeticException ex) {
                return bd; // If it's a decimal, return BigDecimal
            }
        }
    }
```
