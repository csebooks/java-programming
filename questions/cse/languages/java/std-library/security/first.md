---
choices:
  - "Purge sensitive information from exceptions."
  - "Validate file formats before processing untrusted files."
  - "Make public static fields final."
answer:
  - "Release resources in all cases."
  - "Resource limit checks should not suffer from numeric overflow."explanation: "To prevent denial of service (DoS) attacks, it’s important to manage resources carefully and ensure calculations (like limits and counters) don’t suffer from numeric overflows, which can be exploited. Releasing resources ensures the system doesn't run out of them under attack conditions."
---

Which two are guidelines for preventing denial of service attacks?
