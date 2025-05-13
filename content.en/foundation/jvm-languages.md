---
title: 'JVM Languages'
weight: 3
---


The Java Virtual Machine (JVM) was initially designed to support only the Java language. However, over time, many other languages have been adapted or designed to run on the JVM. These languages can be broadly categorized into high-profile languages, JVM implementations of existing languages, and new languages with JVM implementations.

## High-Profile JVM Languages

**Java**: Java is a general-purpose, object-oriented programming language known for its cross-platform portability. It compiles code into bytecode, which is then executed by the JVM. Java is strongly statically typed, platform-independent, garbage-collected, and supports multithreading.  
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
````

**Kotlin**: Developed by JetBrains, Kotlin is a statically typed language that combines object-oriented and functional programming paradigms. It is fully supported by Google for Android development and is known for its concise syntax and interoperability with Java.

```kotlin
fun main(args: Array<String>) {
    println("Hello, World!")
}
```

**Scala**: Scala stands for "scalable language" and combines object-oriented and functional programming. It is known for its strong static typing, algebraic data types, pattern matching, and enhanced immutability support.

```scala
object HelloWorld {
    def main(args: Array[String]): Unit = println("Hello, world!")
}
```

**Groovy**: Groovy is an object-oriented, optionally typed, dynamic language with support for static typing and static compilation. It integrates easily with Java and adds powerful features like scripting capabilities and runtime meta-programming.

```groovy
println("Hello, world!")
```

**Clojure**: Clojure is a functional programming language that runs on the JVM and Microsoft's Common Language Runtime. It is a modern dialect of Lisp and supports dynamic typing, concurrency, and runtime polymorphism.

```clojure
(println "Hello, World!")
```

## JVM Implementations of Existing Languages

Several existing languages have been implemented to run on the JVM. Some notable examples include:

* **Jython**: The Java platform implementation of Python, allowing Python code to run on the JVM.
* **JRuby**: An implementation of the Ruby programming language for the JVM.
* **Rhino and Nashorn**: JavaScript engines for the JVM.
* **Renjin**: An implementation of the R language for the JVM.

## New Languages with JVM Implementations

Several new languages have been designed specifically to run on the JVM. Some examples include:

* **Ballerina**: A language for cloud applications with structural typing and support for network client objects, services, and resource functions.
* **Ceylon**: A language from Red Hat designed to be a Java competitor.
* **Fantom**: A language built to be portable across the JVM, .NET Common Language Runtime (CLR), and JavaScript.
* **Xtend**: An object-oriented, functional, and imperative programming language built by the Eclipse foundation.

---

In conclusion, the JVM has evolved to support a wide variety of languages, each bringing unique features and paradigms to the table. This versatility makes the JVM a compelling platform for modern-day programming.

