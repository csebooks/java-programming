---
choices:
  - "It throws an exception"
  - "It can lead to skipped logic or cleanup code"
  - "It terminates the loop"
  - "It can only be used in while loops"
answer:
  - "It can lead to skipped logic or cleanup code"explanation: "`continue` causes the loop to skip the rest of the current iteration, which may bypass essential logic such as updates, cleanups, or logging, making code harder to follow and maintain."
---

What is the major limitation of using `continue` in a loop with complex logic?
