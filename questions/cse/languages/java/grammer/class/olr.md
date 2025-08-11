---
choices:
  - "byte…"
  - "Byte, Byte"
  - "compilation error"
answer:
  - "long, long"
explanation: "The method `doCalc(b, b)` is invoked with two primitive `byte` arguments. Java chooses the most specific applicable method. `doCalc(Byte, Byte)` requires boxing, and `doCalc(byte...)` is varargs, but `doCalc(long, long)` is the best match via primitive widening. Therefore, it prints \"long, long\"."
---

What is the output of the following code?

```java
public class Client {
  static void doCalc(byte... a) {
    System.out.print("byte…");
  }

  static void doCalc(long a, long b) {
    System.out.print("long, long");
  }

  static void doCalc(Byte s1, Byte s2) {
    System.out.print("Byte, Byte");
  }

  public static void main(String[] args) {
    byte b = 5;
    doCalc(b, b);
  }
}
