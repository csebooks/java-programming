---
choices:
  - "Mr. Smith"
  - "Ms. Jones"
  - "Mr. Jones"
  - "Smith Ms."
answer:
  - "Ms. Jones"

explanation: "`names[0][1]` = \"Ms. \", `names[1][1]` = \"Jones\" → Result: Ms. Jones"
---

## What is the output of this code?

```java
String[][] names = {
  {"Mr. ", "Ms. "},
  {"Smith", "Jones"}
};
System.out.println(names[0][1] + names[1][1]);
