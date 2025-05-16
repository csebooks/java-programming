---
title: 'Annotation'
weight: 9
---

Annotations in Java provide a powerful way to add metadata to your code. They are used to convey additional information to the compiler or runtime, without changing the code logic. Annotations can be applied to classes, methods, variables, parameters, and packages.

## Why Use Annotations?

Annotations serve several purposes:

* **Compile-time instructions**: They can provide information to the compiler, like marking deprecated methods or suppressing warnings.
* **Runtime processing**: Some annotations are available during runtime and can be processed using reflection.
* **Deployment-time processing**: Tools can use annotations to generate configuration files, code, or perform code analysis.

---

# The Format of an Annotation

In its simplest form, an annotation looks like this:

```java
@Entity
```

The `@` symbol indicates to the compiler that what follows is an annotation. For example:

```java
@Override
public void myMethod() {
    // method implementation
}
```

Annotations can also include elements with values:

```java
@Author(
   name = "Benjamin Franklin",
   date = "3/27/2003"
)
public class MyClass { }
```

If the element name is `value`, it can be omitted:

```java
@SuppressWarnings("unchecked")
public void myMethod() {
    // method implementation
}
```

Multiple annotations can be used together:

```java
@Author(name = "Jane Doe")
@EBook
public class MyClass { }
```

---

# Where Annotations Can Be Used

Annotations can be applied to:

* **Declarations**: Classes, fields, methods, parameters, and local variables.
* **Types**: Since Java SE 8, annotations can also be applied to types, like:

```java
new @Readonly MyObject();
(@NonNull String) str;
```

---

# Declaring an Annotation Type

You can define your own annotations. Here is an example:

```java
public @interface ClassPreamble {
   String author();
   String date();
   int currentRevision() default 1;
   String lastModified() default "N/A";
   String lastModifiedBy() default "N/A";
   String[] reviewers();
}
```

This can be used as follows:

```java
@ClassPreamble(
   author = "John Doe",
   date = "3/17/2002",
   currentRevision = 6,
   lastModified = "4/12/2004",
   lastModifiedBy = "Jane Doe",
   reviewers = {"Alice", "Bob", "Cindy"}
)
public class Generation3List extends Generation2List {
    // class code here
}
```

---

# Predefined Annotation Types

Java provides several predefined annotation types:

* **@Deprecated**: Marks a method, class, or field as deprecated, signaling it should no longer be used.
* **@Override**: Indicates that a method is intended to override a method in a superclass.
* **@SuppressWarnings**: Tells the compiler to suppress specific warnings.
* **@SafeVarargs**: Indicates that the annotated method or constructor does not perform unsafe operations on its varargs parameters.
* **@FunctionalInterface**: Marks an interface as a functional interface, which can have only one abstract method.

---

# Type Annotations and Pluggable Type Systems

From Java SE 8, annotations can be applied to any use of a type, not just declarations. This allows stronger type checking and enables custom plug-in validation. For example:

```java
@NonNull String str;
myString = (@NonNull String) str;
```

Java SE 8 allows the creation of **pluggable type systems**, which let you write modules for custom type checking. These annotations help avoid issues like `NullPointerException` by enforcing constraints at compile-time.

---

# Repeating Annotations

As of Java SE 8, annotations can be repeated on the same declaration. For example:

```java
@Schedule(dayOfMonth="last")
@Schedule(dayOfWeek="Fri", hour="23")
public void doPeriodicCleanup() {
    // implementation
}
```

To enable repeating annotations, use the `@Repeatable` annotation:

```java
import java.lang.annotation.Repeatable;

@Repeatable(Schedules.class)
public @interface Schedule {
  String dayOfMonth() default "first";
  String dayOfWeek() default "Mon";
  int hour() default 12;
}

public @interface Schedules {
    Schedule[] value();
}
```

The container annotation `@Schedules` holds the repeated annotations. This allows retrieval through the Reflection API during runtime.

---

# Conclusion

Annotations in Java enhance metadata representation, streamline compile-time checks, and offer runtime processing capabilities. They are integral in modern Java development, aiding frameworks, reducing boilerplate, and enhancing readability and maintainability.

Feel confident using annotations in your projects, and explore their deeper integrations with tools like Spring, Hibernate, and custom pluggable types!
