---
choices:
  - "12345"
  - "1245"
  - "1345"
  - "1234"
answer:
  - "1245"

explanation: "`continue` skips printing when i is 3. So it prints 1, 2, 4, 5."
---

## What will be the output of this code?

```java
for (int i = 1; i <= 5; i++) {
    if (i == 3) continue;
    System.out.print(i);
}
