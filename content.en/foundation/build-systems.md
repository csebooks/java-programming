---
title: 'Build Systems'
weight: 5
---

After writing your first `HelloWorld.java`, you’ve seen how Java works at a basic level. But real-world Java projects are more than a single file. They involve organizing code, reusing libraries, and automating builds.

In this chapter, you’ll learn how Java programs are structured—from simple packages to large, modular applications—and how to bundle and build them using modern tools like Maven and Gradle.

By the end of this chapter, you’ll be able to:

* Organize Java code using **packages**
* Understand Java’s **module system**
* Create and run **JAR files**
* Use **Maven** and **Gradle** to automate builds, manage dependencies, and streamline your workflow

---

### Structuring Java Programs

As your program grows, you’ll have multiple `.java` files. To avoid naming conflicts and keep things clean, Java uses **packages**.

Here’s an example of moving from a flat HelloWorld to a structured layout:

```
src/
└── com/
    └── example/
        └── app/
            ├── Main.java
            └── utils/
                └── GreetHelper.java
pom.xml
```

Inside `GreetHelper.java`:

```java
package com.example.app.utils;

public class GreetHelper {
    public static void sayHello() {
        System.out.println("Hello from GreetHelper!");
    }
}
```

And in `Main.java`:

```java
package com.example.app;

import com.example.app.utils.GreetHelper;

public class Main {
    public static void main(String[] args) {
        GreetHelper.sayHello();
    }
}
```

To compile this, you'd use:

```bash
javac -d out src/com/example/app/Main.java src/com/example/app/utils/GreetHelper.java
```

To Run this,
```bash
java -cp out com.example.app.Main
```

---

### Java Modules (JPMS)

Modules are a step above packages. Introduced in Java 9, they help structure large applications with clear boundaries and strong encapsulation.

At the root of a module, you define a `module-info.java`:

```java
module com.example.app {
    exports com.example.app.utils;
}
```

This makes only `com.example.app.utils` available to other modules—everything else is hidden by default.

---

### Packaging Your Application into a JAR

To distribute or run your app easily, you can package it as a **JAR** (Java ARchive).

From your compiled classes:

```bash
jar --create --file app.jar -C out .
```

To run the JAR:

```bash
java -cp app.jar com.example.app.Main
```

You can also add a manifest to make it executable:

```bash
jar --create --file app.jar --main-class com.example.app.Main -C out .
java -jar app.jar
```

---

### Maven Build Tool for Real Projects

Maintaining complex classpaths and manually compiling files is painful. That’s where **build tools** come in.

Absolutely! Here's a simple and complete guide to help your readers **set up Maven** on their system (both Windows and Linux), followed by verifying the installation.

---

## Maven Setup Instructions

To build and manage Java projects efficiently, Maven is a must-have. Here's how to set it up on your system.

---

### Windows Setup

#### 1. **Download Maven**

Go to the official Apache Maven site:
[https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi)

* Download the **binary zip archive** (e.g., `apache-maven-<version>-bin.zip`)
* Extract it to a folder, e.g., `C:\Programs\apache-maven-<version>`

#### 2. **Set Environment Variables**

   * Open Windows Powershell.
   * Set the installation path as follows:

   ```powershell
   setx MAVEN_HOME "C:\Programs\apache-maven-<version>"
   ```

   ```powershell
   setx PATH "%PATH%;%MAVEN_HOME%\bin"
   ```

#### 3. **Verify Installation**

Open Command Prompt and run:

```bash
mvn -v
```

You should see Maven version info.

---

### Linux Setup

#### 1. **Download Maven**

```bash
wget https://downloads.apache.org/maven/maven-3/<version>/binaries/apache-maven-<version>-bin.tar.gz
```

#### 2. **Extract the Archive**

```bash
tar -xvzf apache-maven-<version>-bin.tar.gz
sudo mv apache-maven-<version> /opt/maven
```

#### 3. **Set Environment Variables**

Edit your shell profile (`~/.bashrc`, `~/.zshrc`, etc.) and add:

```bash
export MAVEN_HOME=/opt/maven
export PATH=$MAVEN_HOME/bin:$PATH
```

Apply the changes:

```bash
source ~/.bashrc  # or source ~/.zshrc
```

#### 4. **Verify Installation**

```bash
mvn -v
```

You should see output with the Maven version, Java version, and JAVA\_HOME path.

---

Now Lets add below in `pom.xml`:

### You're Ready

```xml
<!-- pom.xml -->
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example</groupId>
  <artifactId>app</artifactId>
  <version>1.0</version>
</project>
```

Run:

```bash
mvn package
```

JAR gets built inside `target/`.

This is how real-world Java development happenss.

