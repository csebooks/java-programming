---
title: 'Learning the Java Language'
weight: 2
---

## Object-Oriented Programming Concepts

If you've never used an object-oriented programming language before, you'll need to learn a few basic concepts before you can begin writing any code. This lesson will introduce you to objects, classes, inheritance, interfaces, and packages. Each discussion focuses on how these concepts relate to the real world, while simultaneously providing an introduction to the syntax of the Java programming language.

### What Is an Object?

An object is a software bundle of related state and behavior. Software objects are often used to model the real-world objects that you find in everyday life. This lesson explains how state and behavior are represented within an object, introduces the concept of data encapsulation, and explains the benefits of designing your software in this manner.

Objects are key to understanding object-oriented technology. Look around right now and you'll find many examples of real-world objects: your dog, your desk, your television set, your bicycle.

Real-world objects share two characteristics: They all have state and behavior. Dogs have state (name, color, breed, hungry) and behavior (barking, fetching, wagging tail). Bicycles also have state (current gear, current pedal cadence, current speed) and behavior (changing gear, changing pedal cadence, applying brakes). Identifying the state and behavior for real-world objects is a great way to begin thinking in terms of object-oriented programming.

Take a minute right now to observe the real-world objects that are in your immediate area. For each object that you see, ask yourself two questions: "What possible states can this object be in?" and "What possible behavior can this object perform?". Make sure to write down your observations. As you do, you'll notice that real-world objects vary in complexity; your desktop lamp may have only two possible states (on and off) and two possible behaviors (turn on, turn off), but your desktop radio might have additional states (on, off, current volume, current station) and behavior (turn on, turn off, increase volume, decrease volume, seek, scan, and tune). You may also notice that some objects, in turn, will also contain other objects. These real-world observations all translate into the world of object-oriented programming.

<!-- image -->

Software objects are conceptually similar to real-world objects: they too consist of state and related behavior. An object stores its state in fields (variables in some programming languages) and exposes its behavior through methods (functions in some programming languages). Methods operate on an object's internal state and serve as the primary mechanism for object-to-object communication. Hiding internal state and requiring all interaction to be performed through an object's methods is known as data encapsulation — a fundamental principle of object-oriented programming.

<!-- image -->

By attributing state (current speed, current pedal cadence, and current gear) and providing methods for changing that state, the object remains in control of how the outside world is allowed to use it. For example, if the bicycle only has 6 gears, a method to change gears could reject any value that is less than 1 or greater than 6.

Bundling code into individual software objects provides a number of benefits, including:

1. Modularity: The source code for an object can be written and maintained independently of the source code for other objects. Once created, an object can be easily passed around inside the system.

2. Information-hiding: By interacting only with an object's methods, the details of its internal implementation remain hidden from the outside world.

3. Code re-use: If an object already exists (perhaps written by another software developer), you can use that object in your program. This allows specialists to implement/test/debug complex, task-specific objects, which you can then trust to run in your own code.

4. Pluggability and debugging ease: If a particular object turns out to be problematic, you can simply remove it from your application and plug in a different object as its replacement. This is analogous to fixing mechanical problems in the real world. If a bolt breaks, you replace it, not the entire machine.

### What Is a Class?

A class is a blueprint or prototype from which objects are created. This section defines a class that models the state and behavior of a real-world object. It intentionally focuses on the basics, showing how even a simple class can cleanly model state and behavior.

### What Is Inheritance?

Inheritance provides a powerful and natural mechanism for organizing and structuring your software. This section explains how classes inherit state and behavior from their superclasses, and explains how to derive one class from another using the simple syntax provided by the Java programming language.

### What Is an Interface?

An interface is a contract between a class and the outside world. When a class implements an interface, it promises to provide the behavior published by that interface. This section defines a simple interface and explains the necessary changes for any class that implements it.

### What Is a Package?

A package is a namespace for organizing classes and interfaces in a logical manner. Placing your code into packages makes large software projects easier to manage. This section explains why this is useful, and introduces you to the Application Programming Interface (API) provided by the Java platform.

### Questions and Exercises: Object-Oriented Programming Concepts

Use the questions and exercises presented in this section to test your understanding of objects, classes, inheritance, interfaces, and packages.

## Language Basics

### Variables

You've already learned that objects store their state in fields. However, the Java programming language also uses the term "variable" as well. This section discusses this relationship, plus variable naming rules and conventions, basic data types (primitive types, character strings, and arrays), default values, and literals.

#### Primitive Data Types



#### Arrays



#### Summary of Variables



#### Questions and Exercises



### Operators

This section describes the operators of the Java programming language. It presents the most commonly-used operators first, and the less commonly-used operators last. Each discussion includes code samples that you can compile and run.

#### Assignment, Arithmetic, and Unary Operators



#### Equality, Relational, and Conditional Operators



#### Bitwise and Bit Shift Operators



#### Summary of Operators



#### Questions and Exercises



### Expressions, Statements, and Blocks

Operators may be used in building expressions, which compute values; expressions are the core components of statements; statements may be grouped into blocks. This section discusses expressions, statements, and blocks using example code that you've already seen.

#### Questions and Exercises


### Control Flow Statements

This section describes the control flow statements supported by the Java programming language. It covers the decisions-making, looping, and branching statements that enable your programs to conditionally execute particular blocks of code.

#### The if-then and if-then-else Statements


#### The switch Statement


#### The while and do-while Statements


#### The for Statement


#### Branching Statements


#### Summary of Control Flow Statements


#### Questions and Exercises

## Classes and Objects

With the knowledge you now have of the basics of the Java programming language, you can learn to write your own classes. In this lesson, you will find information about defining your own classes, including declaring member variables, methods, and constructors.

You will learn to use your classes to create objects, and how to use the objects you create.

This lesson also covers nesting classes within other classes, and enumerations


### Classes

This section shows you the anatomy of a class, and how to declare fields, methods, and constructors.

#### Declaring Classes



#### Declaring Member Variables



#### Defining Methods



#### Providing Constructors for Your Classes



#### Passing Information to a Method or a Constructor



### Objects

This section covers creating and using objects. You will learn how to instantiate an object, and, once instantiated, how to use the dot operator to access the object's instance variables and methods.

#### Creating Objects



#### Using Objects



### More on Classes

This section covers more aspects of classes that depend on using object references and the dot operator that you learned about in the preceding section: returning values from methods, the this keyword, class vs. instance members, and access control.

#### Returning a Value from a Method



#### Using the this Keyword



#### Controlling Access to Members of a Class



#### Understanding Class Members



#### Initializing Fields



#### Summary of Creating and Using Classes and Objects



#### Questions and Exercises



#### Questions and Exercises



### Nested Classes

Static nested classes, inner classes, anonymous inner classes, local classes, and lambda expressions are covered. There is also a discussion on when to use which approach.

#### Inner Class Example



#### Local Classes



#### Anonymous Classes



#### Lambda Expressions



#### Method References



#### When to Use Nested Classes, Local Classes, Anonymous Classes, and Lambda Expressions



#### Questions and Exercises



### Enum Types

This section covers enumerations, specialized classes that allow you to define and use sets of constants.

#### Questions and Exercises




## Annotations

Annotations, a form of metadata, provide data about a program that is not part of the program itself. Annotations have no direct effect on the operation of the code they annotate.

Annotations have a number of uses, among them:

- **Information for the compiler** — Annotations can be used by the compiler to detect errors or suppress warnings.

- **Compile-time and deployment-time processing** — Software tools can process annotation information to generate code, XML files, and so forth.

- **Runtime processing** — Some annotations are available to be examined at runtime.

This lesson explains where annotations can be used, how to apply annotations, what predefined annotation types are available in the Java Platform, Standard Edition (Java SE API), how type annotations can be used in conjunction with pluggable type systems to write code with stronger type checking, and how to implement repeating annotations.




### Annotations Basics



### Declaring an Annotation Type



### Predefined Annotation Types



### Type Annotations and Pluggable Type Systems



### Repeating Annotations



### Questions and Exercises


## Interfaces and Inheritance




### Interfaces

You saw an example of implementing an interface in the previous lesson. You can read more about interfaces here—what they are for, why you might want to write one, and how to write one.

#### Defining an Interface



#### Implementing an Interface



#### Using an Interface as a Type



#### Evolving Interfaces



#### Default Methods



#### Summary of Interfaces



#### Questions and Exercises



### Inheritance

This section describes the way in which you can derive one class from another. That is, how a subclass can inherit fields and methods from a superclass. You will learn that all classes are derived from the `Object` class, and how to modify the methods that a subclass inherits from superclasses. This section also covers interface-like abstract classes.

#### Multiple Inheritance of State, Implementation, and Type



#### Overriding and Hiding Methods



#### Polymorphism



#### Hiding Fields



#### Using the Keyword super



#### Object as a Superclass



#### Writing Final Classes and Methods



#### Abstract Methods and Classes



#### Summary of Inheritance



#### Questions and Exercises





## Numbers and Strings

### Numbers

This section begins with a discussion of the Number class (in the `java.lang` package) and its subclasses. In particular, this section talks about the situations where you would use instantiations of these classes rather than the primitive data types. Additionally, this section talks about other classes you might need to work with numbers, such as formatting or using mathematical functions to complement the operators built into the language. Finally, there is a discussion on autoboxing and unboxing, a compiler feature that simplifies your code.

#### The Numbers Classes



#### Formatting Numeric Print Output



#### Beyond Basic Arithmetic



#### Summary of Numbers



#### Questions and Exercises



#### Characters



### Strings

Strings, which are widely used in Java programming, are a sequence of characters. In the Java programming language, strings are objects. This section describes using the `String` class to create and manipulate strings. It also compares the `String` and `StringBuilder` classes.

#### Converting Between Numbers and Strings



#### Manipulating Characters in a String



#### Comparing Strings and Portions of Strings



#### The StringBuilder Class



#### Summary of Characters and Strings



#### Autoboxing and Unboxing



#### Questions and Exercises


## Generics (Updated)

In any nontrivial software project, bugs are simply a fact of life. Careful planning, programming, and testing can help reduce their pervasiveness, but somehow, somewhere, they'll always find a way to creep into your code. This becomes especially apparent as new features are introduced and your code base grows in size and complexity.

Fortunately, some bugs are easier to detect than others. Compile-time bugs, for example, can be detected early on; you can use the compiler's error messages to figure out what the problem is and fix it, right then and there. Runtime bugs, however, can be much more problematic; they don't always surface immediately, and when they do, it may be at a point in the program that is far removed from the actual cause of the problem.

Generics add stability to your code by making more of your bugs detectable at compile time. After completing this lesson, you may want to follow up with the Generics tutorial by Gilad Bracha.


### Why Use Generics?



### Generic Types



#### Raw Types



### Generic Methods



### Bounded Type Parameters



#### Generic Methods and Bounded Type Parameters



### Generics, Inheritance, and Subtypes



### Type Inference



### Wildcards



#### Upper Bounded Wildcards



#### Unbounded Wildcards



#### Lower Bounded Wildcards



#### Wildcards and Subtyping



#### Wildcard Capture and Helper Methods



#### Guidelines for Wildcard Use



### Type Erasure



#### Erasure of Generic Types



#### Erasure of Generic Methods



#### Effects of Type Erasure and Bridge Methods



#### Non-Reifiable Types



### Restrictions on Generics



#### Questions and Exercises

## Packages

This lesson explains how to bundle classes and interfaces into packages, how to use classes that are in packages, and how to arrange your file system so that the compiler can find your source files.




### Creating and Using Packages



#### Creating a Package



#### Naming a Package



#### Using Package Members



#### Managing Source and Class Files



#### Summary of Creating and Using Packages



### Questions and Exercises
