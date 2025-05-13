---
title: 'Installation'
weight: 3
---

Java is not bundled with your OS by default—you need to install the **JDK** to start writing and running Java programs.

Here’s how you can set it up manually using `.zip` (Windows) or `.tar.gz` (Linux) files.

---

### 🪟 **Windows Setup (Using .zip)**

1. **Download the JDK**

   * Go to [https://jdk.java.net](https://jdk.java.net) or your preferred JDK vendor.
   * Download the **.zip** version for Windows.

2. **Extract the JDK**

   * Extract it to a location like:
     `C:\Java\jdk-21`

3. **Set Environment Variables**

   * Open **System Properties** → **Environment Variables**

   * Under **System Variables**, add:

     ```
     JAVA_HOME = C:\Java\jdk-21
     ```

   * Add `JAVA_HOME\bin` to the `Path`:

     ```
     Path = ...;C:\Java\jdk-21\bin
     ```

4. **Verify Installation**

   Open Command Prompt and run:

   ```cmd
   java -version
   javac -version
   ```

   You should see the installed version printed.

---

### 🐧 **Linux Setup (Using .tar.gz)**

1. **Download the JDK**

   * Visit [https://jdk.java.net](https://jdk.java.net)
   * Download the **.tar.gz** version for Linux.

2. **Extract the JDK**

   ```bash
   tar -xvzf jdk-21_linux-x64_bin.tar.gz
   sudo mv jdk-21 /opt/java/
   ```

3. **Set Environment Variables**

   Edit `~/.bashrc` (or `~/.zshrc` if you're using zsh):

   ```bash
   export JAVA_HOME=/opt/java/jdk-21
   export PATH=$JAVA_HOME/bin:$PATH
   ```

   Then apply the changes:

   ```bash
   source ~/.bashrc
   ```

4. **Verify Installation**

   ```bash
   java -version
   javac -version
   ```

---

Now your JDK setup is ready—on both Windows and Linux!
You're all set to compile and run Java programs like a pro. ☕

Perfect! Let’s keep the momentum going with a simple and beginner-friendly section on writing and running your **first Java program**.

---

### ✍️ Your First Java Program

Now that the JDK is set up, let’s write, compile, and run a simple Java program. This will help confirm everything works and show how the development flow fits together.

---

### 📄 1. Create a Java File

Open a text editor (like Notepad, VS Code, or nano) and write the following:

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

✅ **Note:** The filename *must* match the class name (`HelloWorld`).

---

### 🛠️ 2. Compile the Program

Open your terminal or command prompt, navigate to the folder where `HelloWorld.java` is saved, and run:

```bash
javac HelloWorld.java
```

This creates a file named `HelloWorld.class` — this is the **bytecode** version of your program.

---

### 🚀 3. Run the Program

Now run the compiled bytecode using the JVM:

```bash
java HelloWorld
```

You should see:

```
Hello, Java!
```

🎉 **That’s it!** You've just written, compiled, and run your first Java program!

---

### 🔁 Recap: What Happened?

```plaintext
HelloWorld.java  --[javac]-->  HelloWorld.class  --[java]-->  "Hello, Java!"
```

This simple flow—**write → compile → run**—is at the heart of every Java application.


