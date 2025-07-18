---
choices:
  - "Inner obj = new Inner();"
  - "Outer.Inner obj = new Outer.Inner();"
  - "Inner i = Outer.new Inner();"
answer:
  - "Outer o = new Outer(); Outer.Inner i = o.new Inner();"
explanation: "Non-static inner classes are tied to an instance of the outer class. To create them, you first need an object of the outer class, then instantiate the inner using `o.new Inner()`."
---

How do you create an object of a non-static inner class?
