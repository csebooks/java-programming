---
title: 'Installation'
weight: 4
categories:
   - installation
references:
    videos:
        - youtube:FXwgzaUcxOE
        - youtube:X_gRJs1aTdY
    links:
        - https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/How-to-install-Java-21
        - https://www.java.com/en/download/help/linux_x64_install.html
---

Java is not bundled with your OS by default. To write and run Java programs, you must install the **JDK** (Java Development Kit).

This section shows how to install the JDK manually using `.zip` (for Windows) or `.tar.gz` (for Linux), and how to verify the installation.

---

### Windows Setup (Using .zip)

1.**Download the JDK**

   - Visit [https://jdk.java.net](https://jdk.java.net) or your preferred vendor.
   - Download the `.zip` archive suitable for Windows.


2.**Extract the JDK**
   
   - Extract the archive to a location such as:

   ```
   C:\Java\jdk-<version>
   ```

3.**Set Environment Variables**

   - Open PowerShell and set the following:

   ```powershell
   setx JAVA_HOME "C:\Java\jdk-<version>"
   setx PATH "%PATH%;%JAVA_HOME%\bin"
   ```

4.**Verify Installation**

   - Open a new Command Prompt window and run:

   ```cmd
   java -version
   javac -version
   ```

   - You should see the installed version displayed.

---

### Linux Setup (Using .tar.gz)

1.**Download the JDK**

   - Visit [https://jdk.java.net](https://jdk.java.net)
   - Download the `.tar.gz` archive for Linux.

2.**Extract the JDK**

   ```bash
   tar -xvzf jdk-21_linux-x64_bin.tar.gz
   sudo mv jdk-21 /opt/java/
   ```

3.**Set Environment Variables**

   * Edit `~/.bashrc` (or `~/.zshrc` if using Zsh):

   ```bash
   export JAVA_HOME=/opt/java/jdk-21
   export PATH=$JAVA_HOME/bin:$PATH
   ```

   * Apply the changes:

   ```bash
   source ~/.bashrc
   ```

4.**Verify Installation**

   ```bash
   java -version
   javac -version
   ```


Now your JDK is installed and configured. You’re ready to write, compile, and run Java programs.


### Your First Java Program

Let’s create and run a simple Java program to verify the setup and understand the basic development workflow.

#### Step 1: Create the Java File

Open any text editor and write the following code:

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, Java!");
    }
}
```

Save the file as:

```
HelloWorld.java
```

Note: The file name must match the class name (`HelloWorld`).

---

### Step 2: Run the Program

You can directly run the `.java` file using the Java interpreter:

```bash
java HelloWorld.java
```

This should print:

```
Hello, Java!
```

---

### Closer Look at Compilation

Let’s look at what happens when you compile the Java source file.

```bash
javac HelloWorld.java
ls
```

This produces a file called `HelloWorld.class`, which contains bytecode.

You can inspect this bytecode using:

```bash
javap HelloWorld
```

---

### Creating a Custom Runtime (JRE)

The full JDK is around 300 MB. But your program might only need the core Java module `java.base`.

You can use `jlink` to create a lightweight runtime:

```bash
jlink --add-modules java.base --output my_jre
```

Check the size of `my_jre` — it should be significantly smaller.

You can now run your program using the custom JRE:

```bash
./my_jre/bin/java HelloWorld
```

This demonstrates how Java’s modularity allows you to create minimal runtimes tailored to your application.

```

Let me know if you'd like this broken into two Markdown files — one for **installation**, another for **your first program** — or we can continue with the next topic.
```
