---
title: 'Module'
weight: 12
---


  **Java Modules (JPMS)**

Modules let you define **clear boundaries** in large applications by specifying which packages are available to others and which dependencies you need. You declare them in a `module-info.java` file at the root of your module.

```java
// module-info.java
module com.example.app {
    requires com.example.utils;       // needs another module
    requires java.sql;                // needs a standard module

    exports com.example.app.api;      // makes this package public
    opens com.example.app.internal;   // allows deep reflection
}
```

---

### `requires`

Lists modules your code depends on. The compiler and runtime ensure these are present.

```java
requires java.base;      // implicitly added, contains core Java
requires java.logging;   // for java.util.logging APIs
```

---

### `exports`

Makes a package available at **compile time** and **runtime** to other modules.

```java
exports com.example.service;  
```

Clients can then `requires com.example.app;` and import types from `com.example.service`.

---

### `opens`

Grants **runtime** reflective access (for frameworks like Jackson or Spring) but does **not** allow compile-time access.

```java
opens com.example.model; 
```

Your JSON mapper can use reflection to read/write private fields in `com.example.model`.

---

### `uses` & `provides`

Enable a **service-loader** pattern for loose coupling.

```java
// In module A (consumer)
uses com.example.spi.PaymentProcessor;

// In module B (provider)
provides com.example.spi.PaymentProcessor
    with com.example.impl.CreditCardProcessor;
```

At runtime, `ServiceLoader.load(PaymentProcessor.class)` finds all providers automatically.

---

### Other Options

* `requires static` — compile-time only dependency (e.g., annotations)
* `exports ... to` — export a package to specific modules only
* `opens ... to` — open a package reflectively to specific modules

```java
exports com.example.secret to com.example.trusted;
opens com.example.reflection to org.junit.jupiter.api;
```

---

With modules, you gain **strong encapsulation**, **better performance**, and **safer class loading**, making large-scale Java applications more maintainable and secure.
