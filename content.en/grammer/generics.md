---
title: 'Generics'
weight: 8
categories:
    - generics
---

Generics enable types (classes and interfaces) to be parameters when defining classes, interfaces, and methods. This allows for code reusability and type safety by allowing the same code to operate on different data types.

## 1. Introducing Generics

### Why Use Generics?

* **Type Safety**: Detects errors at compile time rather than at runtime.
* **Code Reusability**: Write a single class or method that can operate on different types.
* **Elimination of Type Casting**: Reduces the need for explicit type casting.

### Generic Types

A generic type is a class or interface that is parameterized over types.

```java
public class Box<T> {
    private T t;
    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
```

### Raw Types

Using a generic class or interface without specifying a type parameter is called a raw type. This can lead to runtime errors and is discouraged.

```java
Box rawBox = new Box(); // Raw type
rawBox.set(123); // Allowed, but type safety is compromised
```

### Generic Methods

Generic methods are methods that introduce their own type parameters. This allows for more flexible and reusable code.

```java
public <T> void printArray(T[] array) {
    for (T element : array) {
        System.out.println(element);
    }
}
```

### Bounded Type Parameters

You can restrict the types that can be used as type arguments by using bounded type parameters.

```java
public <T extends Number> void process(T number) {
    // Method implementation
}
```

### Generics, Inheritance, and Subtypes

Generics in Java are invariant. This means that `List<Number>` is not a superclass or subclass of `List<Integer>`, even though `Integer` is a subclass of `Number`.

## 2. Type Inference

Type inference is the Java compiler's ability to look at each method invocation and corresponding declaration to determine the type argument(s) that make the invocation applicable.

### Type Inference and Generic Methods

The compiler can infer the type parameters of generic methods based on the arguments passed to the method.

```java
List<String> list = Arrays.asList("a", "b", "c");
```

### Type Inference and Instantiation of Generic Classes

Since Java 7, the diamond operator (`<>`) can be used to simplify the instantiation of generic classes.

```java
Map<String, List<String>> myMap = new HashMap<>();
```

### Target Types and Lambda Expressions

In Java 8 and later, the target type (the type expected in a particular context) can influence type inference, especially with lambda expressions.

```java
List<String> list = Arrays.asList("a", "b", "c");
list.forEach(s -> System.out.println(s));
```

## 3. Wildcards

In generic code, the question mark (`?`), called the wildcard, represents an unknown type. Wildcards can be used in three ways:

### Upper Bounded Wildcards

Restricts the unknown type to be a specific type or a subtype of that type.

```java
List<? extends Number> list = new ArrayList<Integer>();
```

### Unbounded Wildcards

Represents an unknown type, useful when you are dealing with generic types, but you do not care about the actual type parameter.

```java
List<?> list = new ArrayList<String>();
```

### Lower Bounded Wildcards

Restricts the unknown type to be a specific type or a supertype of that type.

```java
List<? super Integer> list = new ArrayList<Number>();
```

### Wildcards and Subtyping

Wildcards allow for more flexible subtyping. For instance, `List<? extends Number>` can accept `List<Integer>`, `List<Double>`, etc.

### Wildcard Capture and Helper Methods

Helper methods can be used to capture wildcards to perform certain operations.

```java
public static void printList(List<?> list) {
    for (Object obj : list) {
        System.out.println(obj);
    }
}
```

### Guidelines for Wildcard Use

* Use an unbounded wildcard (`<?>`) if you only need to read from the structure.
* Use an upper bounded wildcard (`<? extends T>`) when you need to read and the type is a subtype of T.
* Use a lower bounded wildcard (`<? super T>`) when you need to write and the type is a supertype of T.

## 4. Type Erasure

Type erasure is a process by which Java removes all generic type information during compilation. This ensures that no new classes are created for parameterized types, which maintains backward compatibility with older Java versions.

### Erasure of Generic Types

The compiler replaces generic types with their bound types or `Object` if no bound is specified.

### Erasure of Generic Methods

Similarly, type parameters in methods are replaced by their bound or `Object`.

### Bridge Methods

Bridge methods are synthetic methods created by the compiler to preserve polymorphism with generics.

### Non-Reifiable Types

Generic types are not available at runtime because of type erasure. This makes it impossible to use certain operations like:

```java
List<String> list = new ArrayList<>();
if (list instanceof ArrayList<String>) { // Error
}
```

## 5. Restrictions on Generics

### Cannot Instantiate Generic Types with Primitive Types

```java
List<int> list = new ArrayList<>(); // Invalid
```

### Cannot Create Instances of Type Parameters

```java
public class Test<T> {
    T obj = new T(); // Invalid
}
```

### Cannot Declare Static Fields Whose Types are Type Parameters

```java
public class Test<T> {
    private static T obj; // Invalid
}

```

### Cannot Use Casts or instanceof with Parameterized Types

```java
if (list instanceof List<String>) { // Invalid
}
```

### Cannot Create Arrays of Parameterized Types

```java
List<String>[] array = new ArrayList<String>[10]; // Invalid
```

### Cannot Create, Catch, or Throw Objects of Parameterized Types

```java
// Invalid
try {
    throw new MyGenericException<T>();
} catch (MyGenericException<T> e) {
    // Invalid
}
```

### Cannot Overload a Method Where the Formal Parameter Types Erase to the Same Raw Type

```java
public void doSomething(List<String> list) {}
public void doSomething(List<Integer> list) {} // Invalid
```

### DisAdvantages of Generics

* **Complexity**: Might be difficult to understand for beginners.
* **Performance Overhead**: Type erasure converts generic types to objects at runtime which causes some overhead.
* **Primitives are not supported**: No support for primitive data types like, int, flot, char etc.
* **Limits reflextion**: Type erasure limits the usage of reflection with generics as type information is not available at runtime.


### Java classes which uses Generic

* **java.util.List**:  Generic elements (E)
* **java.lang.Comparable**: Generic Type (T)
* **java.util.Dictionary**: Generic Key Value Pair(K,V)
* 

### Glassory 

* **T**: Type
* **E**: Element
* **K**: Key
* **V**: Value
* **N**: Number
* **?**: Wildcard

