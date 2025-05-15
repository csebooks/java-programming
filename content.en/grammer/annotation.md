---
title: 'Annotation'
weight: 9
---

Here’s a concise and engaging walkthrough of **annotations** in Java, continuing your grammar section in the same tone and structure.

---

Annotations are a way to **attach metadata** to your Java code. They don’t change how your program runs but can **influence tools, frameworks, or the compiler**.

You’ve already seen some built-in ones like `@Override` or `@FunctionalInterface`.

---

### ✳️ Basic Usage

```java
@Override
public String toString() {
    return "Hello";
}
```

This tells the compiler: "Make sure this method is actually overriding a method from a superclass."

---

### 📦 Where Annotations Can Be Used

Annotations can be placed on:

* Classes
* Methods
* Fields
* Parameters
* Local variables
* Modules and packages (in advanced use)

---

### ⏰ Types of Annotations (by Retention)

Annotations can be categorized based on **how long they live** — controlled using `@Retention`.

#### 1. **Compile-Time Annotations**

These are available only during compilation. Tools and compilers use them and discard them later.

```java
@Retention(RetentionPolicy.CLASS)
@interface ExampleCompileOnly {}
```

Most standard annotations like `@Override` fall into this category.

---

#### 2. **Runtime Annotations**

These are available even after compilation — and can be accessed using **reflection**. Frameworks like Spring, JUnit, or Hibernate heavily use runtime annotations.

```java
@Retention(RetentionPolicy.RUNTIME)
@interface Author {
    String name();
}
```

Usage:

```java
@Author(name = "Vijay")
class Book {}
```

You can then read it via reflection:

```java
Author a = Book.class.getAnnotation(Author.class);
System.out.println(a.name());  // Vijay
```

---

#### 3. **Source-Only Annotations**

Used only during source analysis — ignored by the compiler and runtime. Useful in tools like Lombok.

```java
@Retention(RetentionPolicy.SOURCE)
@interface GeneratedCode {}
```

---

### 🧪 Built-in Java Annotations

* `@Override` – Ensures you're overriding a method.
* `@Deprecated` – Marks code as outdated.
* `@SuppressWarnings` – Tells the compiler to ignore specific warnings.

---

### 🛠 Custom Annotations

You can create your own annotations like this:

```java
@interface Info {
    String value();
}
```

You can even define default values, arrays, etc.

---

### 🧠 Annotation Processors

Advanced topic: At compile-time, you can build **tools or processors** that read annotations and generate code, configs, or validations. Tools like **Lombok**, **MapStruct**, and **Dagger** use this heavily.

---

Annotations let you **describe behavior without writing logic**, and modern Java frameworks rely on them extensively for configuration, injection, validation, and more.

Want to follow this with **exception handling**, or move to the next major chapter?
